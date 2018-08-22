# Vertical rhythm

## Current implementations

We have two main ways to control vertical rhythm: our `spacing` utility and the `<Section />` component.

The `spacing` utility takes one of five string values (`xl`, `lg`, `md`, `sm`, `none`) or a default and returns css with `margin-bottom`.

For example, calling `spacing('md')` returns the following CSS: `margin-bottom: 20px;`

The `<Section />` component is simply a `div` with two variants: `section` and `semi-section`.

The former gives the `div` a `margin-bottom` of `100px` and the latter of `50px` (these are for desktop, and values vary on other devices).

## Problems

### `margin-top` on the first section

If a layout consists of sections with 100px of space above and below, using tools that only tackle `margin-bottom` we are unable to provide the spacing at the top of the layout.

### `spacing` is a confusing name

Our utility currently only adds `margin-bottom`. Spacing could imply `margin` **_or_** `padding` in **_any_** direction.

### absolute values for `margin`

While we generally limit the use of absolute values for `margin-bottom` in `festicket-app` by using the `spacing` utility, the many exceptions and **all** values for `margin-top` remain absolute and therefore difficult to maintain.

## Proposal

We maintain the `spacing` utility and `<Section />` component, but with modifications.

The `spacing` module will be renamed and will allow an optional second argument to specify the direction of the margin.

```js
function rhythm(size, direction = 'bottom') {
  try {
    if (!['top', 'bottom'].includes(direction)) {
      throw `direction must be either 'top' or 'bottom'`;
    }

    switch(size) {
      case 'xl':
        return css`
          margin-${direction}: 40px;
        `;
      case 'lg':
        return css`
          margin-${direction}: 30px;
        `;
      ...
      default:
        return css`
          margin-${direction}: 5px;
        `;
    }
  }

  catch(e) {
    console.warn(e);
  }
}
```

As for the `<Section />` component, it will take a boolean prop `isFirst` that will determine whether or not the margin should be added to top and bottom.

```js
type Props = {|
  variant: 'section' | 'semiSection',
  isFirst: boolean,
  children: React.Node,
|};

Section.defaultProps = {
  variant: 'section',
  isFirst: false,
};

export default function Section(props: Props) {
  const { variant, isFirst } = props;

  return styled.section`
    ${switchProp('variant', {
      section: `
        margin-bottom: 100px;
        ${ifProp({ isFirst: true }, `margin-top: 100px`)};
      `,
      'semi-section': `
        margin-bottom: 50px;
        ${ifProp({ isFirst: true }, `margin-top: 50px`)};
      `,
    }
  `;
}
```

Finally, whenever we receive a design that uses spacing that contradicts any of the agreed systems, the first port of call should be to confirm with the design team that there is no way of making the design work using our utilities.
