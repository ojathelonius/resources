## Javascript

* [Transform any label into valid CSS classes](#transform-any-label-into-valid-css-classes)

### Transform any string into valid CSS classes

```javascript
let str = "Événements d'hier";
let cssClass = str.toLowerCase().replace(/'| |"/g, '-').normalize('NFD').replace(/[\u0300-\u036f]/g, ''); 
// cssClass becomes 'evenements-d-hier'
```

**Note :** this does not replace special chars such as `#` or `.`.

