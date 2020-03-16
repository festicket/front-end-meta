# Components Folder Structure

Folder structure for simple, generic components.

> Not to be a "one-for-all" solution, for complex business or edge cases, adapt to other strategy as needed

## Convention

**Overview**

```
- src
	- components
		+ index.ts
		- Select
			+ index.ts
			+ Select.ts
			+ types.ts
			- tests
			- utils
```

**Breakdown**

File: `src/components/index.ts`

```ts
export { default as Select } from "./Select";
```

File: `src/components/Select/index.ts`

```ts
import Select from "./Select";
export default Select;
```

File: `src/components/Select/Select.ts`

```ts
// code...
export default Select;
```

## Pros

**Import components in two ways**

Named export (concise syntax)

```ts
import { Select } from "src/components";
```

Default export (explicit access)

```ts
import Select from "src/components/Select";
```

## Cons

???

## Notes
