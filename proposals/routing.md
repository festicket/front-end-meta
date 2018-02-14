# Locale

## Problem

Currently for every URL we have a few different configurations.

## Solution:

We want to manage URLs in a consistent way, with only one configuration that can be used both for routing and for generating URLs.

A centralised router object keep track of all the URL definitions. This router generates all the different routers (React Router, Express routing etc.) and URLs.

### URL definitions

Every URL needs a name and a pattern. We can use the React Router format for the pattern as it's the same as the one used by Express.

```javascript
var genericUrls = [
    {
        name: 'homepage',
        pattern: '/',
    },
    {
        name: 'about',
        pattern: '/about/',
    },
];

var partnersUrls = [
    {
        name: 'dashboard',
        pattern: '/',
    },
    {
        name: 'festival',
        pattern: '/festival/:festivalPk/',
    },
];

var ecommerceUrls = [
    {
        name: 'festivals-list',
        pattern: '/festivals/',
    },
    {
        name: 'festival-guide',
        pattern: '/festival/:series/:edition/',
    },
];
```

Initialise the router with all the URLs imported from different modules.
We can namespace the different groups using a prefix, and/or apply a group to a specific root, archiving nested URLs.

```javascript
import router from '@festicket/urls';
import genericUrls from '@content/urls';
import partnersUrls from '@partners/urls';
import ecommerceUrls from '@ecommerce/urls';

router.register([
    {
        urlsMap: content,
    },
    {
        namePrefix: 'partners',
        urlRoot: 'partners',
        urlsMap: partnersUrls,
    },
    {
        namePrefix: 'ecommerce',
        urlsMap: partnersUrls,
    },
]);
```

### Patterns

A pattern can easily be retrieved using the name.

```javascript
router.getPattern('partners:festival');

// returns => '/partners/festival/:festivalPk/'
```

Or we can have the entire list of all the patterns in an array, so we can loop over it.

```javascript
// or you can ask for all the patterns
router.getAllPatterns();

// returns =>
// [
//     {
//         name: 'homepage',
//         pattern: '/',
//     },
//     {
//         name: 'about',
//         pattern: '/about/',
//     },
//     {
//         name: 'partner:dashboard',
//         pattern: '/partners/',
//     },
//     {
//         name: 'partner:festival',
//         pattern: '/partners/festival/:festivalPk/',
//     },
//     {
//         name: 'ecommerce:festivals-list',
//         pattern: '/festivals/',
//     },
//     {
//         name: 'ecommerce:festival-guide',
//         pattern: '/festival/:series/:edition/',
//     },
// ]
```

### URL generation

We can generate URLs referring to them using the format `prefix:name`.

```javascript
router.getUrl('homepage');
// returns => '/'

router.getUrl('partners:dashboard');
// returns => '/partners/'

// parameters can be passed in an object
router.getUrl('partners:festival', { festivalPk: '123'});
// returns => '/partners/festival/123/'
```

The generator checks the parameters, raising errors if the format is not correct.

```javascript
router.getUrl('ecommerce:festival-guide', { festivalPk: '123'});

// Error: wrong parameters for the pattern "ecommerce:festival-guide"
//  - the parameter "festivalPk" is not present in the pattern
//  - the parameter "series" is required in the pattern
//  - the parameter "edition" is required in the pattern

// for wrong pattern names
router.getUrl('not-a-thing');

// Error: the patter "not-a-thing" has not being registered
```
