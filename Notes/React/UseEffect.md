### **How `useEffect` Manages Side Effects:**

The `useEffect` hook lets you perform side effects in functional components in a safe, predictable way:
```jsx
useEffect(() => {
  // Code here is the "effect" — this is where side effects happen
  fetchData();

  // Optionally, return a cleanup function that runs when the component unmounts.
  return () => {
    // Cleanup code, e.g., unsubscribing from an event or clearing timers.
  };
}, [/* dependencies */]);
```
- **The first argument** to `useEffect` is the effect function, where you put the code that performs the side effect.
- **The second argument** is the dependencies array, which controls when the effect runs. This array tells React to re-run the effect only when certain values (props or state) change. If you pass an empty array `[]`, the effect will only run **once** after the initial render.
- **Optional Cleanup**: If your side effect needs cleanup (e.g., unsubscribing from a WebSocket, clearing intervals), `useEffect` allows you to return a function that React will call when the component unmounts or before re-running the effect.

If we ever want to use -> access/change a state variable inside a useEffect() hook, we need to make sure that state is added in the dependencies array. Otherwise no re-render will occur on changing the state from the useEffect.
But it is always better to have these separated in 2 useEffects, for example the rendering on first load is to be handled by one useEffect and the running something else whenever the state changes can be done in another useEffect having a dependency array.
The first one must not be run again and again that is why we dont add it to first useEffect.
cleanup function that is returned is for calling when the component using the useEffect unmounts and we want to reset the timers for example. niche thing ngl.
## To recap
`useEffect` is a Hook that lets you perform side effects in functional components. It can be used for data fetching, subscriptions, or manually changing the DOM.

