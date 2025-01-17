- useCallback is a hook that returns a [memoized](Memoization) version of a function.
- i.e. it stores the result of function calls in memory, and when same inputs are encountered, it returns the stored output immediately instead of calling the function again.
- It only calls the function again if one of its dependencies (provided in the dependency array) changes.

### How `useCallback` memoizes a function
1. **Function recreation on every render:** Normally, in React, functions inside components are redefined every time the component re-renders. This can cause performance issues if the function is passed as a prop to child components, or is used inside `useEffect`, since re-creating the function causes re-renders.
2. **Memoization with `useCallback` hook:** When you wrap a function with `useCallback`, React memoizes that function and only recreates it when any of the dependencies in the dependency array changes. Otherwise it returns a **cached version** of the function, hence preventing unnecessary re-creation.
```js
import { useCallback } from 'react';

const memoizedFunction = useCallback(() => {
	// some function logic here
}, [dependency1, dependency2]);
```
3. **Why is it useful?** 
-  **With props:** When a function is passed down as props to child components, memoization helps by making the function stable thus preventing unnecessary re-renders of those children.
- **With hooks:** When a function is used in `useEffect` as a dependency, memoization prevents `useEffect` from triggering it on every re-render, as React considers the memoized version as stable.

**Function stability** means that *the same function reference* in memory is used across re-renders unless it's dependencies change. The `useCallback` function does exactly this.

### Example
**Without memoization**
```js
function Parent() {
	function handleClick() {
		console.log("Button clicked!");
	}
	return <Child onClick={handleClick} />;
}
```
Every time the parent component re-renders, `handleClick` function is re-created and a new reference of `handleClick` function is passed to the child component. This may cause child component to re-render unnecessarily even if the function's behaviour stay the same.

**With memoization**
```js
import { useCallback } from 'react';

function Parent() {
	const handleClick = useCallback(() => {
		console.log("Button clicked!");
	}, []);
	return <Child onClick={handleClick} />
}
```
Now `handleClick` is memoized, and React only re-creates it when its dependencies changes (in this case none). This makes the function **stable** as the same reference of the function is used across re-renders unless one of the dependencies changes. So unless there's some other reason for Parent component to re-render, the child won't re-render unnecessarily and will receive the cached version of memoized function `handleClick`.

### Summary
- Memoization avoids unnecessary re-computation.
- `useCallback` is used to memoize functions, making them stable across renders, unless their dependencies changes.
- Its especially useful for optimizing performance when passing callbacks to child components or using them in hooks like `useEffect`.

