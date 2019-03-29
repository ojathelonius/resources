## Apache

* [Subdomain with VirtualHost](#subdomain-with-virtualhost)

### Subdomain with VirtualHost

#### Serving a web application
```
<VirtualHost *:80>
    ServerName sub.domain.fr
    ProxyRequests Off
    ProxyPreserveHost On
    <Proxy *>
        Order deny,allow
        Allow from all
    </Proxy>
    ProxyPass / http://localhost:3000/
    ProxyPassReverse /  http://localhost:3000/
    RewriteEngine on
    RewriteCond %{SERVER_NAME} = sub.domain.fr
    RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```

#### Serving files
```
<VirtualHost *:80>
    ServerName sub.domain.fr
    ProxyRequests Off
    ProxyPreserveHost On
    <Proxy *>
        Order deny,allow
        Allow from all
    </Proxy>
    DocumentRoot /var/www/website/
    RewriteEngine on
    RewriteCond %{SERVER_NAME} = sub.domain.fr
    RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```


