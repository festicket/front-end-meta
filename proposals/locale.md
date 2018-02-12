# Locale

## Problem
Breaking our monolithic application into several different applications has many benefits. It also introduces many levels of complexity.
Locale should be managed in an application context however it can be changed in controls that live in a library context.

## Solution
- We need to know what the currently selected locale is.
- We need to know what to do once the locale is changed


### Determining the current locale
At festicket we rely on the url to determine the currently selected locale.
Whilst this is not perfect this **is** a business requirement.
On an application level we will need a server-side middleware that can query the festicket API
for the currently selected locale and re-write the url if required.

### Providing Context
We should use providers and pass the currently selected locale into the react context tree.
This value can then be queried by using a consumer. Whilst React is about to release a [new context API which](https://medium.com/dailyjs/reacts-%EF%B8%8F-new-context-api-70c9fe01596b)
follows this pattern, until we can update to react 16.x.x we must model this API to facilitate an easy upgrade path.

```js
// Node process
<Locale.Provider url="req.baseUrl">
  {/* some components*/}
</Locale.Provider>
```

Whilst the API below seems identical,
in the browser the provider, should listen for url changes and propagate this through the react context tree.

```js
// Browser process
<Locale.Provider url="window.location.pathname">
  {/* some components*/}
</Locale.Provider>
```

These API's can be built with the context API in react 15.x.x easily see:
[React Context Docs](https://reactjs.org/docs/context.html)

### Consuming Conext
We can model the new React Context API using the current 15.x.x release. Consuming the context should look like this:

```js
<Locale.Context>
  {
    ({ locale }) => (
      <SomeComponents />
    )
  }
</Locale.Context>
```

### Changing Locale

The simplest way to reload an entire application is to reload the whole page.
As the currently selected locale is stored in the API we can do this with confidence.
We can negate some of the performance implications of this approach by leveraging
Service Worker caching so page reloads can "feel" instant.
