# z-index Convention


## Problem
We have no convention for choosing z-index. This means that developers can set an arbitrary z-index on a component that may have the side effect that it displays above or below another component of which it has no knowledge.


## Solution

### Base layers
First we define the layers in an object.

```js
const layers = {
  "-1": "-1",
  "1": "1",
  "2": "2",
  "3": "3",
  "4": "4",
  "5": "5",
  header: "101", // this includes, for example, the popover
  modal: "102", // this includes the close button
};
```

### Utility function
We then have a function that you pass a layer key to. It returns CSS with the corresponding z-index value. If the key does not exist in the layers object.

```js
export function getZIndex(layer) {
  try {
    const value = layers[layer];

    // throw an error if the key isn't defined in the layers object
    if(!value) {
      throw(`You must use a key that's defined in the layers object`);
    }

    // if the key is defined, return css with the corresponding z-index value
    return css`
      z-index: ${value}
    `;
  }
  catch(error) {
    console.warn(error);
  }
};
```

This function can then be interpolated in styled-components.

```js
export const Header = styled.div`
  ${getZIndex('header')};
`;
```
