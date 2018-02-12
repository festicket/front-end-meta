# Analytics


## Problem
Breaking our monolithic application into several different applications has many benefits. It also introduces many levels of complexity.
It becomes very difficult to manage complex dependencies like analytics configuration.
This is exacerbated in the library context where, configuration, for example, must come from the application context.

## Solution
We need a cross library/application solution that we can inject configuration into at the application level.
This solution must not leak into the component structure such that library dependencies become unusable without application configuration.

### Initialisation
In order to configure analytics we must build a library API that can be injected into:

```js
// module: utils/analytics

import { init } from '@festicket/analytics';

export const { track, identify }  = init({
  ACCOUNT_KEY: process.env.ACCOUNT_KEY,
  ACCOUNT_SECRET: process.env.ACCOUNT_SECRET,
});
```

This means that, once configured, an application can identify or track with ease:

```js
// any file within the application
import { track } from '@utils/analytics';

// Do some stuff
track({ type: 'MY_CUSTOM_EVENT', ...data });
```

### Automatic event capture
This library should bind an event handle to the `document.body` so that all anchors can be tracked automatically.
The developer must only provide `data-analytics-*` properties and `data-analytics=true` to enable the automatic tracking.

```html
<a
  href="http://some-link.com"
  data-analytics=true
  data-analytics-value=test
>
  Some Text
</a>
```

Which should produce an analytics event payload like this:

```json
{ "value": "test" }
```

This means that in a library context a developer can cater for event tracking using only data attributes.
The application can then configure analytics in it's own context where configuration is shared easily.
