## Angular

* [Serving with Apache](#serving-with-apache)
* [Share form controls across components](#share-form-controls-across-components)

### Serving with Apache

**Note :** Git Bash is [bugged](https://github.com/angular/angular-cli/issues/12501), do not use when building with `ng build --base-href`.

Say you want to serve an app with Apache on X.X.X.X/my-app/.

Build the dist folder with `ng build --base-href /my-app/`.

Place it in `/var/www/my-app/` (or whatever the `DocumentRoot` is, or use a symlink).

Make sure your Apache config allows overriding with `.htaccess` files through :

```
<Directory "/var/www/">
  AllowOverride all
</Directory>
```

Create an .htaccess file in `/var/www/my-app` with the following content :

```
<IfModule mod_rewrite.c>
  RewriteEngine on

  # Don't rewrite files or directories
  RewriteCond %{REQUEST_FILENAME} -f [OR]
  RewriteCond %{REQUEST_FILENAME} -d
  RewriteRule ^ - [L]

  # Rewrite everything else to index.html to allow html5 state links
  RewriteRule ^ index.html [L]
</IfModule>

```

### Share form controls across components

Add the following in every child component that has `input` controls that have to be included in the parent component `ngForm`.

```
viewProviders: [{ provide: ControlContainer, useExisting: NgForm }],
````
