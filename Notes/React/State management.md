As your application grows, you might find that multiple components need access to the same state. Instead of duplicating state in each component, you can lift the state up to the Least Common Ancestor, allowing the common ancestor to manage it. This is called rolling up the state.
![[Pasted image 20241211165156.png]]
```JavaScript
import React, { useEffect, useState } from 'react';

function Parent() {
  const [count, setCount] = useState(0);

  return (
    <>
      <Incrase count={count} setCount={setCount} />
      <Decrease count={count} setCount={setCount} />
      <Value count={count} setCount={setCount} />
    </>
  );
}

function Decrease({ count, setCount }) {
  return <button onClick={() => setCount(count - 1)}>Decrease</button>;
}

function Incrase({ count, setCount }) {
  return <button onClick={() => setCount(count + 1)}>Increase</button>;
}

function Value({ count }) {
  return <p>Count: {count}</p>;
}

// App Component
const App = () => {
  return <div>
    <Parent />
  </div>
};

export default App;
```

#### Second example
![[Pasted image 20241211165233.png]]
```JavaScript
import { useState } from 'react'
import './App.css'

function App() {
  return <div>
    <LightBulb />
  </div>
}

function LightBulb() {
  const [bulbOn, setBulbOn] = useState(true)

  return <div>
    <BulbState bulbOn={bulbOn} />
    <ToggleBulbState bulbOn={bulbOn} setBulbOn={setBulbOn} />
  </div>
}

function BulbState({bulbOn}) {
  return <div>
    {bulbOn ? "Bulb on" : "Bulb off"}
  </div>
}

function ToggleBulbState({bulbOn, setBulbOn}) {

  function toggle() {
    // setBulbOn(currentState => !currentState)
    setBulbOn(!bulbOn)
    
  }

  return <div>
    <button onClick={toggle}>Toggle the bulb</button>
  </div>
}

export default App
```

# Prop drilling
**Prop drilling** occurs when you need to pass data from a higher-level component down to a lower-level component that is several layers deep in the component tree. This often leads to the following issues:

- **Complexity:** You may have to pass props through many intermediate components that don’t use the props themselves, just to get them to the component that needs them.
- **Maintenance:** It can make the code harder to maintain, as changes in the props structure require updates in multiple components.

```JavaScript
import React, { useState } from 'react';

// LightBulb Component
const LightBulb = ({ isOn }) => {
  return <div>The light is {isOn ? 'ON' : 'OFF'}</div>;
};

// LightSwitch Component
const LightSwitch = ({ toggleLight }) => {
  return <button onClick={toggleLight}>Toggle Light</button>;
};

// App Component
const App = () => {
  const [isLightOn, setIsLightOn] = useState(false);

  const toggleLight = () => {
    setIsLightOn((prev) => !prev);
  };

  return (
    <div>
      <LightBulb isOn={isLightOn} />
      <LightSwitch toggleLight={toggleLight} />
    </div>
  );
};

export default App;
```
![[Pasted image 20241211165959.png]]

**Actual Problems with it** 
![[Pasted image 20241211170007.png]]
Some intermediary component that is not actually using it will also be re-rendered on change in the state variable. 
Not being used in that component.

# Context API
The Context API is a powerful feature in React that enables you to manage state across your application more effectively, especially when dealing with deeply nested components.
The Context API provides a way to share values (state, functions, etc.) between components without having to pass props down manually at every level.
### Jargon
- **Context**: This is created using `React.createContext()`. It serves as a container for the data you want to share.
- **Provider**: This component wraps part of your application and provides the context value to all its descendants. Any component that is a child of this Provider can access the context.
- **Consumer**: This component subscribes to context changes. It allows you to access the context value (using `useContext` hook)

**Old Code -**
```JavaScript
import React, { useEffect, useState } from 'react';

function Parent() {
  const [count, setCount] = useState(0);

  return (
    <>
      <Incrase count={count} setCount={setCount} />
      <Decrease count={count} setCount={setCount} />
      <Value count={count} setCount={setCount} />
    </>
  );
}

function Decrease({ count, setCount }) {
  return <button onClick={() => setCount(count - 1)}>Decrease</button>;
}

function Incrase({ count, setCount }) {
  return <button onClick={() => setCount(count + 1)}>Increase</button>;
}

function Value({ count }) {
  return <p>Count: {count}</p>;
}

// App Component
const App = () => {
  return <div>
    <Parent />
  </div>
};

export default App;
```

![[Pasted image 20241211171603.png]]
![[Pasted image 20241211171614.png]]
## What context does, and what it doesn’t
The Context API primarily addresses the issue of prop drilling by allowing you to share state across your component tree without needing to pass props through every level.
It doesn’t optimise renders in your application, which becomes important if/when your application becomes bigger


# Introducing recoil
To minimise the number of re-renders, and ensure that only components that are `subscribed` to a value `render`, state management was introduced.
There are many libraries that let you do state management -
1. mobx
2. recoil
3. redux
Here is a simple example with `recoil`
- `npm install recoil`

```JavaScript
import React, { createContext, useContext, useState } from 'react';
import { RecoilRoot, atom, useRecoilValue, useSetRecoilState } from 'recoil';

const count = atom({
  key: 'countState', // unique ID (with respect to other atoms/selectors)
  default: 0, // default value (aka initial value)
});

function Parent() {
  return (
    <RecoilRoot>
      <Incrase />
      <Decrease />
      <Value />
    </RecoilRoot>
  );
}

function Decrease() {
  const setCount = useSetRecoilState(count);
  return <button onClick={() => setCount(count => count - 1)}>Decrease</button>;
}

function Incrase() {
  const setCount = useSetRecoilState(count);
  return <button onClick={() => setCount(count => count + 1)}>Increase</button>;
}

function Value() {
  const countValue = useRecoilValue(count);
  return <p>Count: {countValue}</p>;
}

// App Component
const App = () => {
  return <div>
    <Parent />
  </div>
};

export default App;
```
![[Pasted image 20241211172029.png]]
 **Main benefit of using recoil is that it is highly optimised in understanding what should re-render and what should not re-render.**
 Methods are mostly same, just syntactical names differ.