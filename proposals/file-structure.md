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
- QA and AB testing
  - Easy to leak logic to components level that are used in different pages
  - **Hard for QA to identify the impact to business logic**
- Dependencies
  - High coupling between who uses given reducer, action, saga file
## Feature drive architecture
- A *Feature* is a self-contained user facing reusable complex building block
- Provides out of the box 
  - Disposability, modifiability - can easily remove modify a feature and not break unrelated logic
  - code splitting - there is a logical separation to split a code in webpack on.
  - co-location - all feature related code is in one place, it can be removed, other things will not crash
  - Easy for QA to identify the business logic impact

```
ui-components
packages
	app-components
app
	shared
		# Reusable functionality between features we don't want to put to app-components or ui-components
		# In case for some reason we don't store it in ui-componenst and app-components
		actions
		    # redux.entities related actions, sagas and reducers
		sagas
		reducers
	features
		# A reusable complex user facing component with actions / reducers / actions included
		# Feature cannot nest or depend on other feature
		Basket
			actions
			    # Actions, reducers and sagas that related to redux non entity branch	
			reducers
			sagas
			components
			    BasketTable
			    ... 
			containers
			index.js
    			# index.jsx is a contract of feature. 
                # Can have multiple exports. (variations for iframe/hosted etc) 
		Header
		    ...
		FestivalHeader
			...
		ProductListing
			...
		Footer
			...
		Guide
			...
			index.js 
			    # export both the guide for guide page and the guide for shop page
			 
		Login
			...
	pages (aka Scenes)
		# A page cannot contain other pages
		# A page can have a styles and components that are particual page layout oriented - cannot be reused in any other page
		discovery
			Home
				styles.js
				index.js
			Magazine
				Category.jsx
				Article.jsx
				index.js
		ecommerce
			OrderBuilder
				Accomodation.jsx
				Product.jsx
				ProductListing.jsx
				index.js

```

## Migration plan

1. Start by transforming `app/containers to features`

## Links
- [Feature Driven Architecture - Oleg Isonen](https://www.youtube.com/watch?v=BWAeYuWFHhs)
- [Github / feature driven architecture](https://github.com/kof/feature-driven-architecture)
- [Organizing Large React Applications](http://engineering.kapost.com/2016/01/organizing-large-react-applications/)
- [How to better organize your React applications?](https://medium.com/@alexmngn/how-to-better-organize-your-react-applications-2fd3ea1920f1)
- [Code Structure](https://redux.js.org/faq/code-structure)
