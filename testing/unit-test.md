# Unit test

## Introduction
Unit test are used to test individual part of the application in isolation. They cover a huge variety of cases so this guideline are very generic.

## Dos
- Test the API of modules, if something is used outside his context should be robustly tested.
- Test as documentation: looking at a good set of unit test you should be able to understand how to use a function without reading its internal code.
- Test edge cases, what happen with undefined parameters, or with empty parameters?
- Test errors, if a function is expecting a specific type of parameter test that the function is correctly raising errors.
- Every test should be isolated, if your test share the same mocks or variables make sure to reset them with `beforeEach` and `afterEach`, if you are in doubt that some test may not be correctly isolated try to skip some of them and see if the other still pass.


## Don'ts
- Write huge tests: if a test require to initialise too many components to test just one that is not a good test. If that happen refactor the component and the test to be able to have a very small test.

  Changing the code to fit the test may sound counterintuitive, but if a piece of code is easy to test that means that is very modular and so very maintainable.

- Write big set of fixtures for a test, same reason as above, a function should only take a very limited set of parameters. If you need too much data to test something that is sign that it's time to break down your code.

## TDD
We are not following strict TDD approach.

1) Write a test for a small piece of the overall functionality.  
2) Make it pass.  
3) Refactor if required.  
4) Move onto the next test.  

## Refactoring
When refactoring a piece of code that is untested unit test are a great tool.

1) Find where the function that you are refactoring is used, then write a uni test to simulate every use case.  
2) This unit test should pass already (unless there is some bug)  
3) Start refactoring while the test run, you should not break your new tests with your refactoring.  
