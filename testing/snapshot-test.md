# Snapshot test

## Introduction
Snapshot tests are used to test visual regression of presentational components and for reducers in order to avoid deep comparison boilerplate code

[Example of snapshots usage](https://github.com/festicket/react-app-components/blob/master/src/components/payments/PaymentPlan/tests/index.test.jsx)

## Dos
- Make sure that the snapshots are small and easy to read. 
- Use shallow snapshot comparison. Use mount as exception and only when it doesn't produce a large amount of content.
- In cases where snapshot output is not readable use enzyme-to-json utility. 

## Don'ts
- Don't use snapshots for testing component prop changes. Do assertions on specific DOM nodes directly.
- Don't use mount for snapshots if it produces hard to read and to big file

## Process
1) Create a presentational component  
2) Create a snapshot with default props
3) Check if snapshot is meaningful else use enzyme-to-json util
4) If there is a need for different props test - assert on DOM nodes

## Notes
- Snapshots should be reviewed with the same seriousness as reviewing code, and that's why it is important to keep them concise
- Snapshot testing is not only limited to React components. You can use them for unit testing. Ex: reducer output deep comparison
