---
title: "Web Share API Test"
permalink: /share-test/
layout: single
author_profile: false
noindex: true
---

<p>
  Test how this device's native share sheet handles each combination of
  <code>title</code>, <code>text</code>, <code>url</code>, and one JPEG file.
  Each action must be started by a tap. Results remain only in this browser.
</p>

<p id="share-support" role="status">Checking Web Share API support…</p>

<div id="share-tests" class="share-test-grid" aria-label="Web Share API permutations"></div>

<section class="share-test-status" aria-labelledby="share-status-heading">
  <h2 id="share-status-heading">Latest result</h2>
  <output id="share-status" aria-live="polite">Waiting for a test.</output>
</section>

<style>
  .share-test-grid {
    display: grid;
    gap: 0.75rem;
    grid-template-columns: repeat(auto-fit, minmax(13rem, 1fr));
    margin: 1.5rem 0;
  }

  .share-test-button {
    min-height: 4.5rem;
    padding: 0.75rem;
    text-align: left;
    color: #fff;
  }

  .share-test-status {
    padding: 1rem;
    border: 1px solid #ccc;
    border-radius: 0.25rem;
  }

  #share-status {
    display: block;
    min-height: 3rem;
    white-space: pre-wrap;
    overflow-wrap: anywhere;
  }
</style>

<script>
  (() => {
    const title = "Web Share API permutation test";
    const text = "Testing the iOS native share sheet.";
    const imageUrl = "{{ '/assets/images/headshot.jpg' | relative_url }}";
    const propertyNames = ["title", "text", "url", "files"];
    const support = document.getElementById("share-support");
    const tests = document.getElementById("share-tests");
    const status = document.getElementById("share-status");
    let imageFile;
    let imageLoadError;

    const setStatus = (message) => {
      status.textContent = message;
    };

    const propertyLabel = (properties) =>
      properties.length ? properties.join(" + ") : "no properties";

    const payloadSummary = (payload) => {
      const entries = [];
      if (payload.title) entries.push(`title: ${JSON.stringify(payload.title)}`);
      if (payload.text) entries.push(`text: ${JSON.stringify(payload.text)}`);
      if (payload.url) entries.push(`url: ${payload.url}`);
      if (payload.files) {
        const file = payload.files[0];
        entries.push(`files: ${file.name} (${file.type}, ${file.size} bytes)`);
      }
      return entries.length ? entries.join("\n") : "(empty share data object)";
    };

    const createPayload = (properties) => {
      const payload = {};
      if (properties.includes("title")) payload.title = title;
      if (properties.includes("text")) payload.text = text;
      if (properties.includes("url")) payload.url = window.location.href;
      if (properties.includes("files")) payload.files = [imageFile];
      return payload;
    };

    const runTest = async (properties) => {
      if (properties.includes("files") && !imageFile) {
        setStatus(`Cannot run ${propertyLabel(properties)}. Image file is unavailable: ${imageLoadError || "still loading"}.`);
        return;
      }

      const payload = createPayload(properties);
      if (typeof navigator.share !== "function") {
        setStatus(`Web Share API is unavailable.\n\nPayload:\n${payloadSummary(payload)}`);
        return;
      }
      if (typeof navigator.canShare !== "function") {
        setStatus(`navigator.canShare() is unavailable, so this test was not invoked.\n\nPayload:\n${payloadSummary(payload)}`);
        return;
      }

      let canShare;
      try {
        canShare = navigator.canShare(payload);
      } catch (error) {
        setStatus(`navigator.canShare() threw ${error.name}: ${error.message}\n\nPayload:\n${payloadSummary(payload)}`);
        return;
      }
      if (!canShare) {
        setStatus(`navigator.canShare() returned false; navigator.share() was not invoked.\n\nPayload:\n${payloadSummary(payload)}`);
        return;
      }

      setStatus(`Opening the native share sheet…\n\nPayload:\n${payloadSummary(payload)}`);
      try {
        await navigator.share(payload);
        setStatus(`Share promise resolved.\n\nPayload:\n${payloadSummary(payload)}`);
      } catch (error) {
        setStatus(`Share promise rejected with ${error.name}: ${error.message}\n\nPayload:\n${payloadSummary(payload)}`);
      }
    };

    for (let mask = 0; mask < 1 << propertyNames.length; mask += 1) {
      const properties = propertyNames.filter((__, index) => mask & (1 << index));
      const button = document.createElement("button");
      button.type = "button";
      button.className = "btn btn--primary share-test-button";
      button.textContent = `Share: ${propertyLabel(properties)}`;
      button.addEventListener("click", () => runTest(properties));
      tests.appendChild(button);
    }

    if (typeof navigator.share !== "function") {
      support.textContent = "Web Share API is not available in this browser.";
    } else if (typeof navigator.canShare !== "function") {
      support.textContent = "navigator.share() is available, but navigator.canShare() is not; tests will not be invoked.";
    } else {
      support.textContent = "Web Share API and navigator.canShare() are available.";
    }

    fetch(imageUrl)
      .then((response) => {
        if (!response.ok) throw new Error(`HTTP ${response.status}`);
        return response.blob();
      })
      .then((blob) => {
        imageFile = new File([blob], "share-test-headshot.jpg", {
          type: blob.type || "image/jpeg",
        });
        support.textContent += " JPEG test file is ready.";
      })
      .catch((error) => {
        imageLoadError = `${error.name}: ${error.message}`;
        support.textContent += " JPEG test file could not be loaded.";
      });
  })();
</script>
