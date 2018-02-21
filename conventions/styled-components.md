# Styled Components Architecture

## Separating concerns

As we are moving towards component generators with a clear split between index, styles and story files we should adhere to that clear split between concerns and not mix & match as we like.

In certain situations, e.g the Text primitive component, everything is in the one file and it is a bit of a nightmare to read.

```javascript
function Primitive({ element = "p", children, ...props }: any) {
  return React.createElement(element, stripUnwantedProps(props), children);
}

const H1 = styled(Primitive)`
  ${textMixin};
  font-size: 36px;
  line-height: 39px;

  ${breakpoint("from-sm")`
    font-size: 42px;
    line-height: 45px;
  `};
  ${breakpoint("from-md")`
    font-size: 48px;
    line-height: 52px;
  `};
`;
```

I propose that we adopt a more functional approach while moving out all styles into the styles file. For the situation above where we don't want to import the Primitive component into the styles file, we need a higher order function that takes the styles and component, then composes them together.

So refactoring the above would look something like this:

```javascript
// in a utilities file somewhere
import styled from "styled-components";

export const withStyles = (...args) => Component => styled(Component)(...args);

// in styles.jsx
import { withStyles } from "../somewhere/utils";

const h1Styles = withStyles`
  ${textMixin};
  font-size: 36px;
  line-height: 39px;

  ${breakpoint("from-sm")`
    font-size: 42px;
    line-height: 45px;
  `};
  ${breakpoint("from-md")`
    font-size: 48px;
    line-height: 52px;
  `};
`;

// in index.js
import { h1Styles } from "./styles";
import { compose } from "recompose";

function Primitive({ element = "p", children, ...props }: any) {
  return React.createElement(element, stripUnwantedProps(props), children);
}

const H1 = compose(h1Styles)(Primitive);
```

In my opinion, separating our concerns into a functional, modular approach like this makes working with styled-components much better.
