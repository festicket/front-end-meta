# File structure 
## Issues with current file structure

- Not consistent structure
  - Some components is in app-packages, some are still in app
  - Not clear rules for nesting levels
- Co-location / Navigating
  - Hard to understand dependencies, ex: what pages will be impacted by changing given given reducer, action or saga
- Disposability
  - Very hard to get rid / refactor parts of the system, as dependencies not clear
- Code splitting
  - Impossible to code split regarding not clear boundaries of files, needed for a given page
- AB testing
 -  Easy to leak logic to components level that are used in different pages
- Dependencies
  - High coupling between who uses given reducer, action, saga file
## Feature drive architecture
- A *Feature* is a self-contained user facing reusable complex building block
- Provides out of the box 
  - Disposability, modifiability - can easily remove modify a feature and not break unrelated logic
  - code splitting - there is a logical separation to split a code in webpack on.
  - co-location - all feature related code is in one place, it can be removed, other things will not crash
  

   .
    ├── ui-components              
    ├── packages                   
    ├──── app-components                     
    ├── app                    
    ├──── shared                   
    ├──── features
    ├──── Basket
    ├──── actions
    ├──── reducers
    ├──── sagas
    ├──── components
    ├──── containers
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    ├──── features
    └────── Basket
