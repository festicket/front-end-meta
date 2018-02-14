# Locale

## Problem
Currently for every urls we have few configurations.

Manage urls in a consistent way, with only one configuration, that can be used both for routing and generate urls

## Solution:

A simple router object can be responsible to keep track of all the urls definition, so can be used to generate all the different routers, and also to generate urls.

### URL definitions

Every URLs need just a name and a pattern, for pattern we can use the React Router format, that is the same that express use.

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

var parnersUrls = [
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
        pattern: '/festival/:serie/:edition/',
    },
];
```

initialise the router with all the urls coming from different modules.
We can namespace the different groups using a prefix, and/or apply a groups to a specific root, archiving nesting urls.

```javascript
import router from '@festicket/urls';
import genericUrls from '@content/urls';
import parnersUrls from '@partner/urls';
import ecommerceUrls from '@ecommerce/urls';

router.register([
    {
        urlsMap: content,
    },
    {
        namePrefix: 'partner',
        urlRoot: 'partners',
        urlsMap: parnersUrls,
    },
    {
        namePrefix: 'ecommerce',
        urlsMap: parnersUrls,
    },
]);
```

### Patterns

A pattern can easily be retrieved using the name.

```javascript
router.getPattern('partner:festival');

// returns => '/partners/festival/:festivalPk/'
```

Or we can have the entire list of all the patterns in an array, so we can loop over it.

```javascript
// or you can ask for all the patterns
router.getAllPatterns();

// returns
[
    {
        name: 'homepage',
        pattern: '/',
    },
    {
        name: 'about',
        pattern: '/about/',
    },
    {
        name: 'partner:dashboard',
        pattern: '/partners/',
    },
    {
        name: 'partner:festival',
        pattern: '/partners/festival/:festivalPk/',
    },
    {
        name: 'ecommerce:festivals-list',
        pattern: '/festivals/',
    },
    {
        name: 'ecommerce:festival-guide',
        pattern: '/festival/:serie/:edition/',
    },
]
```

### URLs generation

We can generate URLs referring to them using the format `prefix:name`.

```javascript
router.getUrl('homepage');
// returns => '/'

router.getUrl('partner:dashboard');
// returns => '/partners/'

// parameters can be passed in an object
router.getUrl('partner:festival', { festivalPk: '123'});
// returns => '/partners/festival/123/'
```

The generation check against the parameters, raising errors if the format is not correct.

```javascript
router.getUrl('ecommerce:festival-guide', { festivalPk: '123'});

// Error: wrong parameters for the patter ""ecommerce:festival-guide""
//  - the parameter "festivalPk" is not present in the pattern
//  - the parameter "serie" is required in the pattern
//  - the parameter "edition" is required in the pattern

// as wrong pattern names
router.getUrl('not-a-thing');

// Error: the patter "not-a-thing" has not being registered
```
