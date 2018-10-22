# Error in the route managers


Currently our package [route-manager](https://github.com/festicket/route-manager) currently fail silently in a series of scenario.
I believe that should raise understandable errors if use improperly.

This will protect us form involuntary changes, since a misconfiguration will trigger error in our testing suite.

But also help us in daily use.

## getPattern

Should check if the pattern has been registered.

```javascript
router.getPattern('not-a-thing');

// Error: the pattern "not-a-thing" has not been registered
```

## getUrl

Should checks the parameters, raising errors if the format is not correct.

```javascript
router.getUrl('festivalGuide', { festivalPk: '123'});

// Error: wrong parameters for the pattern "festivalGuide"
//  - the parameter "festivalPk" is not present in the pattern
//  - the parameter "series" is required in the pattern
//  - the parameter "edition" is required in the pattern
```

And also check that the pattern has been registered.

```javascript
router.getUrl('not-a-thing');

// Error: the patter "not-a-thing" has not being registered
```

# getParams

Should raise an error if the params doesn't match any route.


```javascript
router.getParams('/mispelled/value1/value2'))


// Error: the path "/mispelled/value1/value2" does not match any registered route.
```
