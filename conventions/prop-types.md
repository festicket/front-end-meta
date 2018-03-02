# Use React Default Props instead of ES6 Default Parameters

- All these examples exclude Flow types for simplicity. 
Production code should also use Flow types to show all the props.

- [Default Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)

- [React Default Props](https://reactjs.org/docs/typechecking-with-proptypes.html#default-prop-values)

## Problems with Default Parameters

### Every time you set a default parameter you have to destructure the props object.

Bad - every default prop has to use destructured assignment and therefore has to be manually passed as a prop to `Bar`
```jsx harmony
function Foo({size = 'large', ...props}: Props) {
  return (
    <Bar size={size} {...props} />
  )
}
```

Good - no props need to be destructured so we can simply pass `props` object directly to `Bar`.
```jsx harmony
Button.defaultProps = {
  size: 'large',
}

function Foo(props: Props) {
  return (
    <Bar {...props }/>
  )
}
```

### Makes it hard to work with Styled Components
Bad - have to create an additional wrapper function to set the default props for a Styled Component
```jsx harmony
function AnchorComponent(props) {
  return <a {...props}>
}

const StyledAnchorComponent = styled(AnchorComponent)`
  ${switchProp('variant', {
      standard: `color: 'red';`,
      navigation: `color: 'blue';`
  }
`

// This component only exists to pass the variant to the Styled Component since it got assigned to a variable when we set the default.
// The Styled Component needs `variant` to be on the prop object in order to use it.
export default function AnchorWrapper({
  variant='standard',
  ...props
  }) {
  return (<StyledAnchorComponent variant={variant} {...props} />);
  
}
```

Good - No wrapping component needed since Styled Components can access default prop directly from props object
```jsx harmony
 AnchorComponent.defaultProps = {
  variant: 'standard'
}

function AnchorComponent(props) {
  return <a {...props}>
}

export default styled(AnchorComponent)`
  ${switchProp('variant', {
      standard: `color: 'red';`,
      navigation: `color: 'blue';`
  }
`
```

### It doesn't scale well when you start setting many defaults
## Solution

Bad - all the destructured props now need to be manually passed on to the returned component
```jsx harmony
export default function Button({
  to = '#',
  size = 'regular',
  variant = 'regular',
  element = 'a',
  fontSize = 'regular',
  children,
  ...props
}) {
  return (
    <Button 
      to={to}
      fullWidth={fullWidth}
      size={size}
      variant={variant}
      element={element}
      fontSize={fontSize}
      isDisabled={isDisabled}
      // are all the props above? Better pass in `props` to be safe...
      {...props}
    >
      {children}
    </Button>
  )
}

```

Good - Scales well
```jsx harmony

Button.defaultProps = {
  to: '#',
  size: 'regular',
  variant: 'regular',
  element: 'a',
  fontSize: 'regular',
};

export default function Button({children, ...props}) {
  return (
    <Button {...props}>
      {children}
    </Button>
  )
}

```

## Default Props are also supported by Flow
- [Using Default Props with Flow](https://flow.org/en/docs/react/components/#using-default-props-a-classtoc-idtoc-using-default-props-hreftoc-using-default-propsa)
- [Using Default Props for Functional Components](https://flow.org/en/docs/react/components/#toc-using-default-props-for-functional-components)