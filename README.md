# SSL-Apache2-VirtualHost-NodeApp
## Any Node app on any port converted to SSL
#### https://mydomain.com/api-design will get the node app on http://mydomain.com:8082/
#### http://mydomain.com/ will get the app on http://mydomain.com:3001/

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
#### Pre-requisites
Turn on all the modules
```
sudo a2enmod ssl

sudo a2enmod proxy
sudo a2enmod proxy_html
sudo a2enmod proxy_http
sudo a2enmod rewrite
```
