### Hide Mixin Propposal

### TL;DR

Create a new styled components mixin in react-ui-compoents called `hide` which would be similar to `media`, except it would return a fully structured block of css with `display:none` and a media query around it based on the string argument.

### Motivation

We currently have a [`media` mixin](https://react-ui-components.netlify.com/?selectedKind=Utilities%20%2F%20Responsive%20Utilities&selectedStory=Media&full=0&addons=1&stories=1&panelRight=0&addonPanel=storybooks%2Fstorybook-addon-knobs) - 
this works as follows:
```js
import { media } from '@festicket/react-ui-components';

const Button = styled.button`
  .button {
    ${media('from-sm')} {
      background-color: green;
    };
  }
`
```

On medium and large screen widths (based on the [table here](https://react-ui-components.netlify.com/?selectedKind=Utilities%20%2F%20Responsive%20Utilities&selectedStory=Responsive%20Min-Width%20Breakpoints&full=0&addons=1&stories=1&panelRight=0&addonPanel=storybooks%2Fstorybook-addon-knobs) )
the background will be set to green.

One common use of this mixin is to set `display: none` for certain screen widths. This can already be achieved by writing the following:

```js
import { media } from '@festicket/react-ui-components';

const Button = styled.button`
  .button {
    ${media('from-sm')} {
      display:none;
    };
  }
`
```

But since it is such a common pattern, let's have a new mixin which does exactly this.

```js
import { media } from '@festicket/react-ui-components';

const Button = styled.button`
  .button {
    ${hide('from-sm')};
  }
`
```

This would involve creating a new mixin in React-UI-Components that takes as a single argument a string, and would add a display:none css rule that would only apply to the widths based on the input string.
