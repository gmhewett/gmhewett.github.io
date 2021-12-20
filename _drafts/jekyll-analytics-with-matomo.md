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
1. for the tracking code:
   `var u = "https://analytics.gregoryhewett.com/";`

https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-18-04#step-5-%E2%80%94-setting-up-virtual-hosts-recommended

1. run `sudo vi /etc/apache2/sites-available/matomo.conf`
   copy

```
<VirtualHost *:80>
    ServerAdmin admin@localhost
    ServerName analytics.gregoryhewett.com
    ServerAlias analytics.gregoryhewett.com
    DocumentRoot /var/www/html/matomo
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

1. run `sudo a2ensite matomo.conf`
1. run `sudo a2dissite 000-default.conf`
1. run `sudo apache2ctl configtest`
1. run `systemctl reload apache2`

cert
https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-18-04

1. run `sudo add-apt-repository ppa:certbot/certbot`
1. run `sudo apt install python-certbot-apache`
1. run `sudo certbot --apache -d analytics.gregoryhewett.com`
1. enter email
1. go on
1. enforce redirect
