## Focus
Focus is a front-end framework developed by Klee Group, designed to make management software easier to implement thanks to React. 

* [Call actions in bulk on a page](#call-actions-in-bulk-on-a-page)

### Call actions in bulk on a page
React makes `this.refs` available to the parent component. Hence it is possible to do the following :

#### Example code

```JSX
<Parent>
  <Child1 ref="child1" />
  <Child2 ref="child2 />
</Parent>
```

Let's say we want to call the save action for Child1 and Child2 : simply iterate over these in the parent component and call the corresponding action :

```javascript
function onSave() {
  Object.values(this.refs).forEach((ref) => {
    if (ref.action && ref.action.save) {
        ref.action.save.call(ref, ref._getEntity());
    }
  })
}
```

### Conditional navigation with Backbone
There are several usecases to controlling the navigation, but a common one is to prevent users from leaving the page if data has been entered so that they cannot lose it accidentally.

One solution would be to control each `<a>` and `<button>` click events however this is cumbersome and might not affect all navigation elements. In the end, a practical solution is to override the `navigate` function from `Backbone.history`.

```javascript
import { history } from 'backbone';

const navigate = history.__proto__.navigate;

history.navigate = (fragment, params) => {
  if(history.preventNavigation === true) {
     history.trigger('prevented');
  } else {
     navigate.call(history, fragment, params);
  }
}

history.preventNavigation = (bool) => {
  history.preventNavigation = bool;
}

export default history;
```

It can then be used in any component that imports `history`, e.g. in `componentDidMount()` :
```javascript

import history from '../navigation/overriden-history';

componentDidMount() {
  /* Check if user has entered some data */
  history.preventNavigation(this.state.hasUserEnteredData);
  history.on('prevented', this.onNavigationPrevented.bind(this));
},

onNavigationPrevented() {
  this.popin('Do you really want to leave the page ? All entered data will be lost.');
},
```




