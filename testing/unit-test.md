# Unit test

## Introduction
Unit test are used to test individual part of the application in isolation. They cover a huge variety of cases so this guideline is very generic.

## Dos
- Test the API of modules, if something is used outside should be robustly tested.
- Test as documentation: looking at a unit test you should be able to understand how to use a function without reading its internal code.
- Test edge cases, what happens with undefined parameters, or with empty parameters?
- Test errors, if a function is expecting a specific type of parameter test that the function is correctly raising errors.
- Every test should be isolated, if your test shares mocks or variables make sure to reset them with `beforeEach` and `afterEach`, if you are in doubt that some test may not be correctly isolated try to skip some of them and see if the other still pass.
- If there was a bug in production it should be covered by unit test or cypress

## Don'ts
- Write huge tests: if a test require to initialise too many components it is not a good test. If that happens refactor the component to smaller pieces.

  Changing the code to fit the test may sound counter intuitive, but if a piece of code is easy to test that means that it is very modular and maintainable.

- Write big set of fixtures for a test, same reason as above, a function should only take a very limited set of parameters. If you need too much data to test something than it is a sign that it's time to break down your code.

## TDD
We are not following strict TDD approach.

1) Write a test for a small piece of the overall functionality.  
2) Make it pass.  
3) Refactor if required.  
4) Move onto the next test.  

## Refactoring
When refactoring a piece of code that is untested unit tests are a great tool.

1) Find where the function that you are refactoring is used, then write a unit test to simulate every use case.  
2) This unit test should pass already (unless there are some bugs)  
3) Start refactoring while the test runs, you should not break your new tests with your refactoring.  
