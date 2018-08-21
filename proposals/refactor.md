# Separate Components from Containers.

## Problem

We are thinking about moving from Redux. The first step will be start from refactoring our components and separate it from store and actions. There are some components which take only few properties from the store, which we can pass as a props from component above.

## Solution

### Components with few properties from store

For all those components which take only few properties from the store, we can pass it as a props from component above.
Component will be connect only with tarnslations and will have all CSS styles there too.

The component best explame is: `/development/festicket-app/app/components/partners/ItemTable`

There is also PR which can help while refactoring `https://github.com/festicket/festicket-app/pull/1202`

### Components with store connection needed

For components which need to be connected to the store we will create a folder inside /containers folder.
It will be looks like that `/containers/myComponent/index.jsx`
Of course `myComponent` will be have /tests folder and /selectors.js file

The best example of that container is: `/development/festicket-app/app/containers/partners/TicketForm/DealSettingsSectionContainer`

The component best explame is: `/development/festicket-app/app/components/partners/DealSettingsSection`

There is also PR which can help while refactoring `https://github.com/festicket/festicket-app/pull/1171/files`

### Separation of concerns

This concept is also called `Separation of concerns` you can find more here `https://www.wikiwand.com/en/Separation_of_concerns`.

The basic idea of this concept is principle for separating a computer program into distinct sections, such that each section addresses a separate concern.
