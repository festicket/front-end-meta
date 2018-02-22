# Default Props


## Problem

```js
type Props = {
  to?: string,
  fullWidth?: boolean,
  size?: 'regular' | 'small' | 'inline',
  variant?: 'regular' | 'bordered' | 'transparent',
  element?: 'a' | 'button',
  fontSize?: 'regular' | 'small' | 'tiny',
  isDisabled?: boolean,
  render?: () => mixed,
};

export default function Button({
  to = '#',
  size = 'regular',
  variant = 'regular',
  element = 'a',
  fullWidth = false,
  fontSize = 'regular',
  isDisabled = false,
  render = () => null,
  ...props
}: Props) {
  return (
    <Button
      to={to}
      size={size}
      variant={variant}
      element={element}
      fullWidth={fullWidth}
      fontSize={fontSize}
      isDisabled={isDisabled}
      render={render}
      {...props}
    >
      {children}
  )