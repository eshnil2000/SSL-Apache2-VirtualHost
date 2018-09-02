# SSL-Apache2-VirtualHost-NodeApp
## Any Node app on any port converted to SSL
#### https://mydomain.com/api-design will get the node app on http://mydomain.com:8082/
#### https://mydomain.com/ will get the app on http://mydomain.com:3001/

#### Contents of /etc/apache2/sites-available/000-default-le-ssl.conf

```
<IfModule mod_ssl.c>
<VirtualHost *:443>
        ServerAdmin webmaster@localhost
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        ServerAlias mydomain.com
        Options -Indexes
        SSLEngine on
        SSLProxyEngine on
        SSLProxyVerify none
        SSLProxyCheckPeerCN off
        SSLProxyCheckPeerName off
        SSLProxyCheckPeerExpire off
        ProxyVia Full
        ProxyPass /.well-known/ !
        ProxyPass /api-design http://mydomain.com:8082/
        ProxyPassReverse /api-design http://mydomain.com:8082/

        ProxyPass / http://mydomain.com:3001/
        ProxyPassReverse / http://mydomain.com:3001/

        ProxyPreserveHost On


ServerName mydomain.com
SSLCertificateFile /etc/letsencrypt/live/mydomain.com/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/mydomain.com/privkey.pem
Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>
</IfModule>
```
## Pre-requisites

#### Turn on all the modules
```
sudo a2enmod ssl

sudo a2enmod proxy
sudo a2enmod proxy_html
sudo a2enmod proxy_http
sudo a2enmod rewrite
```

#### All required ports open on server

#### Apache2 server running

#### Let's Encrypt SSL certificate installed

### NOTE:
Some resources (css, js, etc files may not load if referenced relative to folder paths for /api-design.
For example, a file that was at dist/template.css will not load, because the relative path would be:
https://mydomain.com/dist which does not exist! 

#### Example: this will not work over SSL
```
<link href="./dist/swagger-editor.css" rel="stylesheet">
```

TO fix this, change the path to:
#### Example: this will work OK over SSL
```
<link href="//mydomain.com/api-design/dist/swagger-editor.css" rel="stylesheet">
```



