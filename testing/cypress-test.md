# Cypress test

## Introduction

Cypress is an end-to-end testing library that mimics user interaction by running an instance of a browser.

# How we see end-to-end

We currently mock `fetch` manually and return static JSON responses that have been generated at a point in time.

Because we don't have a dynamic mock backend, we are unable to fully leverage the power of Cypress for testing full flows.

However, we are still able to write meaningful tests that prove the interaction between back and front end. We assert that given a certain API response, the front end should behave in a certain way.

## What we test with Cypress

- Every time a new scene is setup, it should not be merged into master without a cypress test
- This test should assert that all the key components in the page are displayed (if the test is important, assert with `.contains`)
- Each possible user interaction should be tested
  - for a button to increase the number of items in a basket, we should assert the number of items before and after interaction
  - for a click that fires a post request, we should assert that `fetch` is called with the right arguments
  - for an anchor that leads to another page, we should assert that browser visits the correct URL on click

## Setting up the API responses

- Create a mock API response JSON file in `/tests/fixtures/`
- Import it in `/tests/utils/config/index.js` and export is depending on whether it will be returned client- or server-side
- If it's client-side, define a case for it in `/tests/utils/client-mock-fetch.js`
- If it's server-side, you only need to add logic to `/tests/utils/mock-fetch.js` if you want a different response to the default (i.e. arriving at the shop page with an item already in your basket vs. an empty basket)

## Testing different API responses

### Server side

Add logic to `/tests/utils/mock-fetch.js` to handle a certain query parameter, then `cy.visit()` a URL with that parameter.

### Client side

You will need to persist the query parameter from the initial `cy.visit()`. For example, if you wanted to visit the shop for a festival that did not allow checkout for ticket only baskets, you could add a case for `/shop?ticket_only=true` to `/tests/utils/client-mock-fetch.js`.

You would then visit `/shop?ticket_only=true` in the spec and when clicking checkout the param would be appended to the API call.
