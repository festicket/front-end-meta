# Storybook Conventions


## Component variants

If a component has several variants, display them together in one Story.

Say we have four icon components: `Chevron Down`, `Chevron Left`, `Chevron Right` and `Chevron Up`. Each of these components has three colour variants `white`, `grey`, and `primary`. The component is also passed a boolean, `hoverable`.

If we are to provide a Story for each variant, we will have a total of 24 (4 * 3 * 2) Stories, making the Storybook tree structure difficult to navigate. And this is just for four components, let alone entire component repositories.

It is clearer to put all 6 variants of `Chevron Down`, say, in one Story. This will speed up development and testing as there is no need to navigate between variants.