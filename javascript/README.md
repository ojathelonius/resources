## Javascript

* [Transform any label into valid CSS classes](#transform-any-label-into-valid-css-classes)
* [Add non-anonymous event handler with callback](#add-non-anonymous-event-handler-with-callback)

### Transform any string into valid CSS classes

```javascript
let str = "Événements d'hier";
let cssClass = str.toLowerCase().replace(/'| |"/g, '-').normalize('NFD').replace(/[\u0300-\u036f]/g, ''); 
// cssClass becomes 'evenements-d-hier'
```

**Note :** this does not replace special chars such as `#` or `.`.

### Add non-anonymous event handler with callback
Let's say we have the following event listener :

```javascript
// say 'this' does not refer to window here
this.myCallback = function() {
  // Very handsome callback
}

window.addEventListener('message', () => {
  // All fine, the fat arrow allows me to keep the parent context
  this.myCallback();
});
```

Now I'd like to use a non-anonymous function so that I can remove the event listener as I please, with `removeEventListener(fn)`.

```javascript
this.myCallback = function() {
  // Very handsome callback
}

function onEvent(event) {
  this.myCallback();
}
window.addEventListener('message', () => onEvent.bind(this));
```

Simply make sure that the callback is part of the context that is bounded to the event handler.

