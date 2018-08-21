# Separate Components from Containers.

## Problem

We are thinking about moving from Redux. The first step will be start from refactoring our components and separate it from store and actions. There are some components which take only few properties from the store, which we can pass as a props from component above.

## Solution

### Components with few properties from store

For all those components which take only few properties from the store, we can pass it as a props from component above.
Component will be connect only with translations and will have all CSS styles there too.

```js
// components/MyComponent/index.jsx

import React from 'react';
import { translate } from 'react-i18next';
import PropTypes from 'prop-types';

 MyComponent.propTypes = {
  ...<YOUR PROPS GO HERE>
};

MyComponent.defaultProps = {
  ...<YOUR DEFAULT PROPS GO HERE>
};

 export function MyComponent({PROPS}) {
  return (
      ....<YOUR CODE GO HERE/>
  );
}
 export default translate(['translation_json'])(MyComponent);
```

The component best explame is: `/development/festicket-app/app/components/partners/ItemTable`

There is also PR which can help while refactoring `https://github.com/festicket/festicket-app/pull/1202`

### Components with store connection needed

For components which need to be connected to the store we will create a folder inside /containers folder.
It will be looks like that `/containers/myComponent/index.jsx`
Of course `myComponent` will be have /tests folder and it own /selectors.js file

COMPONENT:

```js
// components/MyComponent/index.jsx

import React from 'react';
import { translate } from 'react-i18next';
import PropTypes from 'prop-types';

 MyComponent.propTypes = {
  ...<YOUR PROPS GO HERE>
};

MyComponent.defaultProps = {
  ...<YOUR DEFAULT PROPS GO HERE>
};

 export function MyComponent({PROPS}) {
  return (
      ....<YOUR CODE GO HERE/>
  );
}
 export default translate(['translation_json'])(MyComponent);
```

CONTAINER:

```js
// containers/MyComponent/index.jsx

import React from 'react';
import PropTypes from 'prop-types';
import { connect } from 'react-redux';

// Actions
import {<ACTION GO HERE>} from '@actions';

// Components
import MyComponent from '@components/MyComponent';

// Selectors
import { <SELECTORS GO HERE> } from '@selectors';

// Types
import {<TYPES GO HERE>} from '@constants/types';

MyComponentContainer.propTypes = {
  ...<YOUR PROPS AND FUNCTIONS (ACTIONS) GO HERE>
};

MyComponentContainer.defaultProps = {
  ...<YOUR DEFAULT PROPS GO HERE>
};

function MyComponentContainer(props) {
  return (
    <MyComponent
     ...<PASSING ALL PROPS FOR THIS COMPONENT, ALL STORE CONNECTIONS AND ACTIONS/>
    />
  );
}

export function mapStateToProps(state) {
  return {
   .... <STORE>
    },
  };
}

function mapDispatchToProps(dispatch) {
  return {
    ... <ACTIONS>
  };
}

export default connect(mapStateToProps, mapDispatchToProps)(
  MyComponentContainer,
);
```

SELECTORS:

```js
    // containers/MyComponent/selectors.jsx

import { createSelector } from '@utils/reselect-memoize';

export const myComponentSelector = createSelector(
    ... <YOUR SELECTOR CODE GOES HERE>
);
```

The best example of that container is: `/development/festicket-app/app/containers/partners/TicketForm/DealSettingsSectionContainer`

The component best explame is: `/development/festicket-app/app/components/partners/DealSettingsSection`

There is also PR which can help while refactoring `https://github.com/festicket/festicket-app/pull/1171/files`

### Separation of concerns

This concept is also called `Separation of concerns` you can find more here `https://www.wikiwand.com/en/Separation_of_concerns`.

The basic idea of this concept is principle for separating a computer program into distinct sections, such that each section addresses a separate concern.
