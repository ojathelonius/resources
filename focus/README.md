## Focus
Focus is a front-end framework developed by Klee Group, designed to make management software easier to implement thanks to React. 

* [Call actions in bulk on a page](#call-actions-in-bulk-on-a-page)
* [Conditional navigation with Backbone](#conditional-navigation-with-backbone)

### Call actions in bulk on a page
React makes `this.refs` available to the parent component. Hence it is possible to do the following :

#### Simple example

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

#### Complex example : call one action, and if they all succeed, call another
A typical use case would be to save all sub-forms, then submit another form. For this purpose, we need to know when all children forms are done saving.

The formMixin `afterChange()` function is called when the form store changes. However this function is at child component level, hence it needs to be conveniently set on the fly :

```javascript
saveAndDoSomething() {
  // Count the number of elements that are actually saved
  this.saveCount = 0;
  // Count the number of elements that have to be saved
  this.maxCount = Object.values(this.refs).filter(ref => ref.action && ref.action.save).length - 1;

  Object.values(this.refs).forEach((ref) => {
    if (ref.action && ref.action.save) {

      // Set afterChange() handler
      ref.afterChange = (response) => {
        // Check that the save request succeeded and increment the counter
        if (response.status.name === 'saved') {
          this.saveCount++;
        }
      }

      ref.action.save.call(ref, ref._getEntity()).then(() => {
        if (this.saveCount == this.maxCount) {
          // SUCCESS : all elements are saved, carry on with doSomething();
        } else {
          // FAILURE : at least one element has not saved, and it should handle its own behaviour
        }
      });
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
  if(history.canNavigate === false) {
     history.trigger('prevented');
     // Save navigation attempt for future use
     history.previousNavigation = [fragment, params];
  } else {
     navigate.call(history, fragment, params);
  }
}

history.preventNavigation = (bool) => {
  history.canNavigate = !bool;
}

history.finallyNavigate = () => {
  if(history.previousNavigation) {
     // Allow navigation
     history.preventNavigation(false);
     history.navigate(...history.previousNavigation);
  }
}

export default history;
```

It can then be used in any component that imports `history` :
```javascript

import history from '../navigation/overriden-history';

componentDidMount() {
  history.on('prevented', this.onNavigationPrevented.bind(this));
},

render() {
  // Check if user has entered some data, cannot be called in componentDidMount if using component state
  history.preventNavigation(this.state.hasUserEnteredData);
  
  return (
  ...
  );
}

onNavigationPrevented() {
  // Example of pop-in with Yes/No handlers
  this.popin('Do you really want to leave the page ? All entered data will be lost.', noHandler, yesHandler);
},

noHandler() {
  // Just stay on page
},

yesHandler() {
  // Navigate to previous route
  history.finallyNavigate();
}
```




