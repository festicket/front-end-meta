# Visual regression test (percy)

## Introduction
Visual regression test protect us from unwanted changes in the style of our components by capturing the screenshots of web pages/UI and compare them with the original images.

Percy provide visual regression on every build of `react-app-components` and `react-ui-components`. So every visual change in this two library needs a manual approval.

Percy rely on the storybook to do visual regression.

Currently we do not have any visual regression test in `festicket-app`.

## Dos
- When creating a new component, or editing another one create a story in the storybook for every meaningful combination of props.
- If a component is interactive every state should be initializable with props: percy can't perform any interaction, so in case of components that show/hide part of the content we need to be able to create two stories, one with the content shown by default and another one with the content hidden by default.

## Don'ts
- Do not rely ONLY on knobs when creating a new story.
- Avoid to create story for combinations of props that will never be used together, or that do not affect the visual style of the component, we pay by snapshots so useless test will slow down the build and make unreasonably expensive.


## Future ideas

We could split the storybook in two, so one use a set of plugin and has story in human readable format.
The other without plugins and clear stories just for percy.
