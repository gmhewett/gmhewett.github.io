---
layout: single
title: "Athens: Day 1"
date: "2021-10-26"
categories: "travel"
tags: "greece athens"
author_profile: true
excerpt: "Arrival in Athens and a visit to the Ancient Agora."
---

1. Spin up a $5 DigitalOcean Ubuntu Droplet.
1. Follow https://matomo.org/docs/requirements/
   1. run `sudo apt-get install php php-curl php-gd php-cli mysql-server php-mysql php-xml php-mbstring`
   1. run `sudo apt-get update`
1. follow the sql steps https://matomo.org/faq/how-to-install/faq_23484/
   1. run `mysql`.
   1. run `CREATE DATABASE matomo_analytics;`.
   1. run `CREATE USER 'matomo'@'localhost' IDENTIFIED WITH mysql_native_password BY 'my-strong-password-here';`
   1. run `GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, INDEX, DROP, ALTER, CREATE TEMPORARY TABLES, LOCK TABLES ON matomo_analytics.* TO 'matomo'@'localhost';`
   1. run `GRANT FILE ON *.* TO 'matomo'@'localhost';`
   1. run `quit`
1. Install Matomo
   1. run `apt install unzip`
   1. run `/var/www/html`
   1. run `wget https://builds.matomo.org/matomo.zip && unzip matomo.zip`
   1. remove the index.html file
   1. run `chown -R www-data:www-data /var/www/html/matomo`
1. Go through install steps.
1. Follow these steps: https://mmistakes.github.io/minimal-mistakes/docs/javascript/#customizing
