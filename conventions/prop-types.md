# Prop types and default props


## Problem

We do not have a consistent implementation of prop types and default props across our components.


## Solution

```jsx
// @flow
import * as React from 'react';

/* 
  any other functions or variables would be here
*/

// Flow props are grouped with defaultProps and come
// immediately before the component declaration
type Props = {
  children: React.Node,
  to?: string,
  fullWidth?: boolean,
  size?: 'regular' | 'small' | 'inline',
  variant?: 'regular' | 'bordered' | 'transparent',
  element?: 'a' | 'button',
  fontSize?: 'regular' | 'small' | 'tiny',
  isDisabled?: boolean,
};

Button.defaultProps = {
  to: '#',
  fullWidth: false,
  size: 'regular',
  variant: 'regular',
  element: 'a',
  fontSize: 'regular',
  isDisabled: false,
};

// we don't need to explicitly mention all props 
// here as they're visible directly above
export default function Button(props: Props) {
  return (
    <Button 
      to={to}
      fullWidth={fullWidth}
      size={size}
      variant={variant}
      element={element}
      fontSize={fontSize}
      isDisabled={isDisabled}
    >
      {children}
    </Button>
  )
}

```