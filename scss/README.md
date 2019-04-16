## SCSS
This file holds a collection of CSS/SCSS resources.

* [Box-shadow collection](#box-shadow-collection)

**Troubleshoot**

* [Browser CSS overrides stylesheet](#browser-css-overrides-stylesheet)

### Box-shadow collection

#### Large subtle shadow
```css
/* Idle */
box-shadow: 0 10px 40px 0 rgba(62, 57, 107, 0.1), 0 2px 9px 0 rgba(62, 57, 107, 0.1);

/* Hover */
box-shadow: 0 10px 40px 0 rgba(62, 57, 107, 0.2), 0 2px 9px 0 rgba(62, 57, 107, 0.2);
```

#### Background inset shadow
The shadow below adds depth to a plain, unicolored page.
```css
box-shadow: inset 0px 0px 500px #0000006e;
```

### Browser CSS overrides stylesheet
The `user agent stylesheet` can sometimes override a custom stylesheet even when this one is properly loaded.
There seems to a be a bug with `div` embedded into `ul` tags, causing browser stylesheet to override any CSS that loosely targets the `div`. It appears to be linked to JSP with `<head></head>` tags containing the `link rel="stylesheet"` links that are embedded in another JSPs.

The fix is to specifically target the div using : 
```css
ul div.ignored-style
```


