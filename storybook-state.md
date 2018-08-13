# State Management inside storybook components

Within the react world it's a common pattern that state is stored outside of dumb components. These dumb components are typically highly reusable and inside of a components library. 

To help manage this in a more generic manner I'm proposing a withState HOC that will wrap a storybook element and return a store element.

## Inital findings
There are already a few components that will do this, [@sambego/storybook-state - npm](https://www.npmjs.com/package/@sambego/storybook-state), [@dump247/storybook-state - npm](https://www.npmjs.com/package/@dump247/storybook-state) I found the first to be overly complex to our use, and the second didn't work with percy. And so I created a very simple HOC inside the react-ui-components item to do it for us. 

## API
```
store: {
  setState(newState: {[key]: value})
  state: {[key]: value}
}
withState(defaultState: {[key]: value}, component: WrappedComponent<Props: {store: Store}> ): ReactComponent
``````

## Example
```
const defaultState = {
  value: "Test Button"
}
addStory(withState(defaultState, (store) => (
  <Button onClick={() => {store.setState({value: "Clicked"})}}>
    {store.value}
  </button>
)))
```