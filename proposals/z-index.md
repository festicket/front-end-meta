# z-index Convention

## Problem

We have no convention for choosing z-index. This means that developers can set an arbitrary z-index on a component that may have the side effect that it displays above or below another component of which it has no knowledge.

## Solution

### Base layers

First we define layers in an object. These base layers are 10 apart so there is room to define hierarchies within layers when necessary.

```js
const layers = {
  '-1': -1,
  content: 10,
  header: 20, // this includes, for example, the popover
  modal: 30
};
```

### Utility function

We create a function that takes a layer key and returns the corresponding z-index value for that base layer.

```js
export function getZIndex(layer) {
  try {
    const value = layers[layer];

    // throw an error if the key isn't defined in the layers object
    if (!value) {
      throw `You must use a key that's defined in the layers object`;
    }

    // if the key is defined, return the corresponding z-index base value
    return value;
  } catch (error) {
    console.warn(error);
  }
}
```

This function can then be interpolated in styled-components.

```js
export const Header = styled.div`
  z-index: ${getZIndex('header')};
`;
```
