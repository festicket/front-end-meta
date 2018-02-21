# z-index Convention


## Problem
We have no convention for choosing z-index. This means that developers can set an arbitrary z-index on a component that may have the side effect that it displays above or below another component of which it has no knowledge.


## Solution

### Base layers
First we establish base layers for z-index values. A component's z-index should be relative to its base layer, such that the header, for example, can have its own z-index hierarchy, while remaining below all modal elements.

```js
const layers = {
  "-1": -1,
  content: 1, // use this layer new z-index requirements
  header: 21, // the popover, for example, sits above this base layer
  modal: 31, // the close button sits above this base layer
};
```

### Utility function
We then have a utility function to set our z-index values. By default, the function returns the z-index of the baseLayer. However, if within this baseLayer a hierarchy is required, you can specify the amountToAdd.

```js
export function getZIndex(baseLayer, amountToAdd = 0) {
  return css`
    z-index: `${layers[baseLayer] + amountToAdd}`;
  `;
}
```
