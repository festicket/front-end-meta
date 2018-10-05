# Snapshot test

## Introduction
Snapshot tests are used to test visual regression of presentational components and for reducers in order to avoid deep comparison boilerplate code

## Dos
- The visual component output has to be relatively small.  
- Focus to shallow snapshot comparison.
- In cases where snapshot output is not readable use enzyme-to-json utility.
- Create snapshot for every presentational component.  

## Don'ts
- Don't use snapshots for testing component prop changes. Do assertions on specific DOM nodes directly.
- Don't use mounted snapshots, better divide components to smaller ones and do shallow snapshots on them directly.

## Process
1) Create a presentational component  
2) Create a snapshot with default props
3) Check if snapshot is meaningful else use enzyme-to-json util
4) If there is a need for different props test - assert on DOM nodes  
