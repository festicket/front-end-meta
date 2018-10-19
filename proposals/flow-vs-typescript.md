# Flow vs TypeScript

Flow and TypeScript are both fantastic ways to limit technical debt in your JavaScript projects, allowing you to, among other things, evolve your codebase with confidence.
They are similar tools that focus on static types checking.

# Flow

## Pros:

1. As you're using React, we might prefer Flow because it easily integrates with Babel and other infrastructure.

2. Flow is known to be a simpler language than TypeScript due to it’s out of the box utility. A developer would have to learn TypeScript syntax e.g. Type Annotation and how to modify the code.

3. Eslint plugins for React and React Native provide valuable static analysis and seem to be superior to their tslint counterparts.

4. As we are using PropTypes, It will be easier to switch to Flow.

## Cons:

1. New project, what might be cause a lot of changes, (errors) when we will be upgarding to the newest verion.

2. Not all utility types from Flow are documented. This might be fixed soon though.

# TypeScript

## Pros:

1. Larger community: Because it is an older project its community is larger.

2. Faster then flow.

3. Upfront compile errors: This can probably be configured in both TypeScript and Flow, but having upfront compilation errors instead of a non mandatory type check seems to save time when testing your code by preventing you from running code that is bogus anyways.

4. More solid type checking and less changes when upgrade to newest version.

## Cons:

1. Lesser React support because its great for Angular project. But nowdays its getting better and better.

2. Less flexible then Flow.

#Key Differences Between TypeScript vs flow

1. Flow is any day a better choice to go when you have to work with type checking static kind functionalities without even writing the non-standard Javascript code i.e. the code which asks for compilation back into Javascript. To use this feature, you can write type annotations in comments rather than using them in the executable code itself.

2. Typescript provides to you some additional language services such as code completion features, navigation and refactoring features whereas flow aims to build a deeper level of understanding of your code and is responsible for doing an interprocedural analysis.

3. Difference with flow and TS syntax here - https://github.com/niieani/typescript-vs-flowtype

4. TypeScript wants to provide great tooling and language services for autocompletion, code navigation, and refactoring. Flow, on the other hand, develops a deeper understanding of your code and even does interprocedural analyses.

5. TypeScript implements both a type checker and a transpiler that emits plain JavaScript. Flow only does type checking and relies on Babel or flow-remove-types or some other tool to remove type annotations.

# How easy would it be to migrate from flow to typescript?

Migrate from Flow to Typescript is easy. It reqiured few configuration change:

1. Babel
2. Rename a JavaScript file extensions from .js to .ts
3. Remove @flow comment
4. Run tsc to typecheck all .ts files
5. Fix all type errors

Flow and TypeScript syntex is very similar, so it will take no long to change it.

Great medium blog explaning change diffrence : https://medium.com/entria/incremental-migration-to-typescript-on-a-flowtype-codebase-515f6490d92d

# Conclusion

I made two project one using Flow and one using TypeScript and I found out that for me Flow is more simple and more flexible.
Documentation for flow https://flow.org/en/docs is very good and clear.
There are some cases when flow gives an error and is hard to find out why, but overall I would say Flow will be better choice for festicket-app because:

1. We are using Flow in `react-ui-components` and `react-app-components` what means that some people in our team already know Flow very well and we dont need to chnage those project to TypeScript.

2. Flow works well and do the job which we want for type checking.

3. We are using PropTypes, what will make easy switch to Flow.
