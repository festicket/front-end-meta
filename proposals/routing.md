# Routing

## Problem

Currently every URL in our application is generated via a small function. These functions have inter-module dependencies.
Managing many files has become a burden and enforcing patterns has become difficult.
We have seen many occurrences of url generators not implementing locale, for example.

## Solution:

We want to manage URLs in a consistent way, with only one configuration that can be used both for routing paths and for generating URLs.
We should use a centralised router object to keep track of all the URL definitions.
This router utility should generate all the different route configurations within required by our stack (React Router, Express routing etc.) and URLs.

## API

### URL definitions

Url definitions should be comprised of key/value pairs:

```js
// app/utils/route-config.js

import routing from '@festicket/route-manager';

export const { getUrl, getPattern, getAllPatterns } = routing({
  home: '/',
  search: '/search',
  complex: '/complex/:param1/:param2',
});
```

### getUrl

 We can generate URLs by referring to them using the format `key`:

 ```js
import { getUrl } from 'app/utils/route-config'

// Generate a simple url with no route params

const homeUrl = getUrl('home');
// returns => '/home'

// Generate a complex url with many route params

const complexUrl = getUrl('complex', { param1: 'test-1', param2: 'test-2' });
// returns => /complex/test-1/test-2

// Generate a url with a query

const searchUrl = getUrl('search', {}, {q: 'something'});
// returns => /search?q=something

 ```


### getPattern

We can generate url Patterns by referring to them by `key`:


```js
import { getPattern } from 'app/utils/route-config'

// Generate the home pattern

const homePattern = getPattern('home');
// returns => /home

const complexPattern = getPattern('complex');
// returns /complex/:paramq1/param2

```

### getAllPatterns

We can get all patterns (usefull for debugging) by calling `getAllPatterns`:

```js
import { getAllPatterns } from 'app/utils/route-config'

const patterns = getAllPatterns();

// reutrns => { home: '/', search: '/search', complex: '/complex/:param1/:param2' }

```
