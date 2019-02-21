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
Object.values(this.refs).forEach((ref) => {
  if (ref.action && ref.action.save) {
      ref.action.save.call(ref, ref._getEntity());
  }
})
```

