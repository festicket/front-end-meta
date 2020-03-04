# Types architecture

The hypothetical problem is thus: we have a global `FestivalModel` that represents a normalised festival with all fields populated. We also have a `Location` component that is a tiny section on the `FestivalCard`. 

How should we organise our types?

## Option 1 

Types are redeclared in the component. This will result in duplication, but conceptually this approach is more encapsulated. When it comes to 'dumb' components, `Location` doesn't need to be aware of the whole FestivalModel. It just displays two strings.

```jsx
// app/types/festivals/index.ts
export interface FestivalModel {
  location: {
    x: string;
    y: string;
  }
}

// app-components/components/festivals/FestivalCard/components/Location/index.tsx
interface LocationProps {
  x: string;
  y: string;
}

export function Location({ x, y }: LocationProps) {
  return <div>{x}, {y}</div>
}
```

## Option 2
`Location` accesses the type from the `FestivalModel`. This prevents the need for duplication and ensures consistent typing, but it also makes our components less 'dumb' as they know about the `FestivalModel`.

```jsx
// app/types/festivals/index.ts
export interface FestivalModel {
  location: {
    x: string;
    y: string;
  }
}

// app-components/components/festivals/FestivalCard/components/Location/index.tsx
export function Location({ x, y }: FestivalModel['location']) {
  return <div>{x}, {y}</div>
}
```

### Option 3

Our model type is composed of other types. This means we can readily import the `FestivalLocation` type in `Location`. Again, we avoid duplication and ensure consistency but the 'dumb' component becomes less dumb. Also, our generic types file becomes very bloated.

```jsx
// app/types/festivals/index.ts
export interface FestivalLocation {
  x: string;
  y: string;
}

export interface FestivalModel {
  location: Location
}

// app-components/components/festivals/FestivalCard/components/Location/index.tsx
export function Location({ x, y }: FestivalLocation) {
  return <div>{x}, {y}</div>
}
```