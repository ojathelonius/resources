## Apache

* [Subdomain with VirtualHost](#subdomain-with-virtualhost)

**Troubleshoot**
* [Nexus as a npm registry proxy returns 404](#nexus-as-a-npm-registry-proxy-returns-404)

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

### Nexus as a npm registry proxy returns 404

The following problem can arise when [Nexus](https://www.sonatype.com/nexus-repository-oss) is set up as a proxy for the npm registry :
```
npm install @types/estree@0.0.39
npm ERR! code E404
npm ERR! 404 Not Found: @types/estree@0.0.39
```

This can either be due to a [bug in Nexus < 3.5](https://issues.sonatype.org/browse/NEXUS-13915) or can also be an **Apache configuration issue** with repositories containing slashes in their name. The following Apache directive on the Nexus VM should be set :

```
AllowEncodedSlashes On
```




