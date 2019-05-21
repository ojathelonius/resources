## SCSS
This file holds a collection of CSS/SCSS resources.

* [Box-shadow collection](#box-shadow-collection)
* [Diagonal gradient border](#diagonal-gradient-border)
* [Cheap dark mode](#cheap-dark-mode)

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

### Diagonal gradient border
Panic used a cool CSS trick for their [latest article](https://panic.com/next/), which involves `skew`-ing a block of text along with a gradient border. It works as shown below :

```scss
.diagonal-block-with-border {
  border-left-width: 0.4rem;
  border-left-style: solid;
  border-image: linear-gradient(to bottom, rgba(0,216,5,1), #2996bf, #ad51a3) 1 100%;
  padding-left: 3rem;
  transform: skew(-1deg);
}
```

### Browser CSS overrides stylesheet
The `user agent stylesheet` can sometimes override a custom stylesheet even when this one is properly loaded.
There seems to a be a bug with `div` embedded into `ul` tags, causing browser stylesheet to override any CSS that loosely targets the `div`. It appears to be linked to JSP with `<head></head>` tags containing the `link rel="stylesheet"` links that are embedded in another JSPs.

The fix is to specifically target the div using : 
```css
ul div.ignored-style
```

### Cheap dark mode

Apply the following to `body` :

```scss
filter: invert(95%) hue-rotate(190deg);
```


