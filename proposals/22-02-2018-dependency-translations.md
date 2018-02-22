# Dependency Translations

## The Problem
By breaking out shared React components to `@festicket/react-ui-components` and `@festicket/react-app-components`, we can develop these components in one place and share them across the Festicket digital estate. However, some components - such as the header - have UI copy that require translation. We need a way to manage translating the strings in these dependency components in a way that does not conflict with the consuming app.

## The Solution
My implementation proposal for solving this is set out in two phases, in the interest of time and the rebranding deadline.

### Phase 1
The dependent components can have their own system for translating UI strings which is self-contained. For this proposal I would recommend that the `ui` and `app` component libraries use the [js-lingui](https://github.com/lingui/js-lingui) library.

```js
import React from 'react';
import { I18nProvider, Trans } from '@lingui/react';

import enCatalog from '../locale/en/messages.js';
import frCatalog from '../locale/fr/messages.js';

const catalogs = {
  en: enCatalog,
  fr: frCatalog
};

export default function Header({ language }) {
  return (<I18nProvider language={language} catalogs={catalogs}>
    <span>
      <Trans>This is a <strong>nested component</strong>...</Trans>
    </span>
  </I18nProvider>);
}
```

The standard Lingui workflow then applies, using `npm run lingui -- extract` and `npm run lingui -- compile` to pull out the translation strings and compile them for consumption respectively.

With this approach, the exported `Header` component accepts a `language` prop as its public API. This could also be passed down with context to avoid 'prop drilling', the end result is the same.

Now, whenever the component is dropped into a consuming app like so;

```js
import Header from '@festicket/react-ui-components';

export default function Page() {
  return <Header language="fr" />;
}
```

It will show the French translations on both server and client render. Happy days.

The DOWNSIDE of this approach is that all the language catalogs, en/fr/etc, must be loaded upfront, in order for the component to render consistently on the client and the server at the app level. Hopefully with the relatively small number of UI copy strings the bundle size for the end-user should remain manageable. Nonetheless, it is undesirable, which leads on to Phase 2...

### Phase 2
Post-rebranding, the focus of this effort would be to enable lazy-loading of the translation catalogs so that the end-user never has to download translations for a language they're not on.

This would mean the consuming app has to import all catalogs from the npm package and preload them on server-render, and lazy-load via Webpack the selected language's catalog on the client and pass it down to the library via either prop or context. Then, per the code block in the example above, `catalogs` would just be moved to the consuming app level. The npm package could export the catalogs separately from the React component in some way. I don't really know how this would look at this point.

Thank you for your time 🌈
