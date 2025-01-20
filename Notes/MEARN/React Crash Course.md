#### React Crash Course -
[React Full Course for free ⚛️ (2024) - YouTube](https://www.youtube.com/watch?v=CgkZ7MvWUAA)

JavaScript library, not framework
JSX -> JavaScript XML 
Uses Virtual DOM, react updates the virtual DOM(lightweight version of real DOM and easier to manipulate and update), then checks diff with the real DOM and then uses reconciliation to append the changes into the real DOM.
In the folder structure, the assets folder in the src/ directory is bundled finally with the app, but the images in the public directory are not and are available via a URL
#### Components - 
The basic layout is -
```jsx
import other from './other.jsx'

function App(){
	return (
		<> 
			<other/>
			{/*more XML to render*/}
		</>
	);
}

export default App
```
The <> </> is necessary because in the return we can only return a single component, it basically provides an empty component to return, without using unnecessary extra div. it is the same as <React.Fragment> provided by react for the same job but shorter.

using js variables or imports(images) within the return -> wrap it in a {stuff} curly braces.
#### Props - 
read-only properties that are shared between components.
A parent component can send data to a child  component.
`<Component key=value/>`
in the component - 
```jsx
function Student(props){
	return(
		<div>
			<p>Name: {props.name}</p>
			<p>Age: {props.age}</p>
		</div>
	)
}
Student.propTypes ={
	name: PropTypes.string,
	age: PropTypes.number,
}
Student.defaultProps = {
	name: "Guest",
	age: 0,
}
export default Student
```
To pass the prop when calling - `<Student name='Yash' age={19}/>`  notice that if it is not a string, it needs a {} 
To make sure that the passed value is of the correct datatype -> we use prop Types -> issues warning if wrong type is passed, doesn't stop the code but issues a warning
example - `age: propTypes.number` 

#### Conditional rendering -
allows us to control what gets rendered in the application based on certain conditions (show, hide or change components)
```jsx
function Student(props){
	return(
		if(props.isHuman){
			<div>
				<p>Name: {props.name}</p>
				<p>Age: {props.age}</p>
			</div>
		}
		else{
			<p>Please Become Human to continue</p>
		}
	)
}
export default Student
```
while calling, `<Student isHuman={true or false} Name="Human" Age="19" />` 

#### Render Lists -
```jsx
function List(){
	const fruits = [{id:1, name:"apple"},
					{id:2, name:"orange"},
					{id:3, name:"banana"},
					{id:4, name:"pineapple"}];
	
	return(fruits);  //prints it as one big string, all joined up together.

	const listItems = fruits.map(fruit => <li key={fruit.id}>{fruit.name}</li>); //we need to provide a unique id in case of more than one prop per object in a list of objects
	return(<ul>{listItems}</ul>);
}
```

#### onClick events -

```jsx
function Button(){
	const handle = () => {console.log("clicked!")};
	const handle2 = (name) => {console.log(`${name} stop clicking`)};
	return (
		<>
			<button onClick={handle}>Click here!</button>  //case 1
			<button onClick={handle2("Bro")}>Click here!</button>  //case 2
			<button onClick={() => handle2("Bro")}>Click here!</button>  //case 3
		</>
	);
}
export default Button
```

We have 3 cases here -
1. We call a parameter less function and it works just fine
2. We have a function in which a parameter is needed, now we pass it in the function right away, so when control reaches the function, it executes even without clicking the button (bad)
3. We pass it via a parameter-less, name-less arrow function -> which then calls the handle2("Bro") on being clicked, this prevents it from being executed right away when control reaches there.
So, mostly using the arrow function passing is better but depending on the situation, one might wanna use the direct passing as well.

**Using the click event to do stuff** to the DOM and other such stuff -
```jsx
function Button(){
	const handleClick = (e) => console.log(e);
	return (<button onClick={(e) => handleClick(e)}>Click here!</ button>);
}
```

the e used here is short for `event` and it basically provides the whole event that happened, which has multiple methods such as change DOM text of the button, get the timestamp of the click and many more, one can see all the methods it offers by `console.log(e)` and then see the logged object in the console. So, we can do multiple stuff like - 
```jsx
function Button(){
	const handleClick = (e) => e.target.textContent = 'Clicked';
	return (<button onClick={(e) => handleClick(e)}>Click here!</ button>);
}
```
This changes the button text to Clicked once u click the button.
There is also the `onDoubleClick` event handler, self explanatory
> We can use the `onClick` handler to handle clicks on anything ideally.

### React Hooks 
It is a special function that allows functional components to use React features without writing class components
Some are - (useState, useEffect, useContext, useReducer, useCallback, and more....)
if something starts with use...() it is most likely a Hook.
need to import these, like - `import React, {useState} from 'react';`

#### useState() Hook
Allows the creation of a stateful variable and a setter function to update its value in the virtual DOM. like `[name, setName]`
`const [name, setName] = useState();`
if we try to do just name = 'anything', it wont display in the webpage, although the actual data of the variable will be changed, but wont update the DOM.
To update the DOM while changing the value, use the setter -> `setName`.
We can pass in original default values for the states by using the `const [name, setName] = useState(0) or = useState("Guest")` and initialize it to something.

#### useEffect() Hook 
React hook that tells React to do some code when one of these things happens:
1. This component re-renders
2. This components mounts -> component is appended to the DOM
3. the state of a value changes 

`useEffect(function, [dependencies])`
1. `useEffect(() => {})`  runs after every re-render
2. `useEffect(() => {}, [])`  runs only on mount
3. `useEffect(() => {}, [value])` runs on mount + when value changes

Uses -
1. event Listeners
2. DOM Manipulation
3. Subscriptions (real-time updates)
4. Fetching Data from an API
5. Clean up when a component unmounts
use example -
```jsx
function component(){
	const [count, setCount] = useCount(0);
	useEffect(() => {   //use another function or just use an arrow function
		document.title = `Count: ${count}`;  //changes the tab name to Count: Current count
	}, [count]);  //happens while mounting + when count changes, we can add more variables by separating with comma and both changes will be watched
	function addCount(){
		setCount(c => c+1);
	}
}
```
we can directly do `document.title = `Count: ${count}`;`  to have the same effect, but useEffect shines when we want to control when this happens by passing the watch list. This also helps explicitly mention what we are trying to do. 
For example, here ->
```jsx
function component() {
	const [width, setWidth] = useState(window.innerWidth);
	const [height, setHeight] = useState(window.innerheight);
	window.addEventListener("resize", handleResize);
	console.log("Event Listener added");

	function handleResize(){
		setWidth(window.innerWidth);
		setHeight(window.innerHeight);
	}
}
```
Here, in this approach, we are basically adding one even Listener everytime we change the window size by even a bit (check console log and u will find many event listeners added).
Only after a few resizes, 1000s of event listeners are added.

To mitigate this, we will use the useEffect() method
```jsx
function component() {
	const [width, setWidth] = useState(window.innerWidth);
	const [height, setHeight] = useState(window.innerheight);

	useEffect(() => { //add event listener only when component mounts, then use the same listener throughout
		window.addEventListener("resize", handleResize);
		console.log("Event Listener added");

		return () => {
			window.removeEventListener("resize", handleResize);
		}
	}, []);
	
	function handleResize(){
		setWidth(window.innerWidth);
		setHeight(window.innerHeight);
	}
}
```
We can also use a return in the useEffect(), this can be used to add any clean up code during unmounting or just anything.
And, we can obviously use more than one useEffect() in one code.

#### useContext() Hook
React Hook that allows you to share values between multiple levels of components without passing props through each level
without prop drilling 
used when we have nested components and want to pass some props to the deeply nested components. 
In normal implementation, we will have to basically pass it to the outermost component, then it passes to its children and then it goes downwards from there.
This is the problem known as prop drilling, solved by useContext() hook.
Solution - 
build a provider component, usually in the outermost component of the nesting like so -
```jsx
import {creatContext} from 'react';
export const MyContext = creatContext();

//now in the return wrap the nested component in this special tag
<MyContext.Provider value={value}>
	<Child />
</MyContext.provider>
```

Then make the consumer components -
```jsx
import react, {creatContext} from 'react';
import { MyContext } from './Outermost_Component';
const value = useContext(MyContext);
```

#### useRef() Hook
"use Reference" does not cause re-renders when its value changes, unlike a useState() which causes re-renders when its value changes. So, when u want a component to "remember" some information, but don't want that information to trigger new renders.
Used generally to -
1. Accessing/Interacting with DOM elements
2. handling Focus, Animations, and Transitions
3. managing timers and intervals
useRef returns a ref object with a single current property initially set to the initial value you provided.

```jsx
const ref = useRef(0) 
function handleClick(){
	ref.current++;
}
//not rerendering the whole component but still increasing the ref.
```
How to handle inputs using the useRef so that the input element is not re-rendered everytime we type something.
```jsx
const inputRef = useRef(null);

function hanndleClick(){
	inputRef.current.focus();   //focuses the input element without re-rendering it.
	inputRef.current.style.backgroundColor = 'yellow';  //we can do anything without the component even re-rendering
}

return (
	<>
		<input ref={inputRef} />
	</>
);
```

---
#### onChange() event handler 
Primarily used with form elements -> example, `<input>, <textarea>, <select>, <radio>` -> triggers a function everytime the value of the input changes.
```jsx
function MyComponent(){
	const [name, setName] = useState("Guest");
	function handleChange(event){
		setName(event.target.value);
	}
	return(
		<>
			<input value={name} onChange={handleNameChange}/>
			<p>Name: {name}</p>
		</>
	);
}
```
renders as - Name: "The input provided in the input field"

#### updater function
A function passed as an argument to `setState()` usually example: `setYear(updater arrow function)` instead of `setYear(year+1)`.
Allows for safe updates based on the previous state used with multiple updates and asynchronous functions
Good practice to use these updater functions.
Now, if we want to do something like this,
```jsx
function Component() {
	const [count, setCount] = useState(0);
	function increment(){
		setCount(count+1);
		setCount(count+1);
		setCount(count+1);  //even on calling the same incrementation 3 times, the counter only goes up by 1.
	}

	return (
		<p>Count: {count}</p>
		<button onClick={increment}>Increase</button>
	);
}
```
The problem is that when we do this, react batches the 3 setter calls to 1 for performance reasons and then treats it as 1 call, to increment the count by 1 only finally.
So, this is more like setting count to be one 3 separate times from 0 rather than 3 update calls.
to mitigate ->
`setCount(prevCount => prevCount+1);` or `setCount(c => c+1);`, basically making an arrow function to do the stuff we wanted to do

**Updating objects in state -**
```jsx
function component(){
	const [car, setCar] = useState({year:2000,
									make:"Ford",
									model:"Mustang"});
	function handleYearChange(event){
		setCar(c => ({...c, year:event.target.value}));
		//equivalent to witing setCar({year:2000, make:"Ford", model:"Mustang", year:2025}), now the first parameter in year is ignored as we have a second one.
	}
	function handleMakeChange(event){
		setCar(c => ({...c, make:event.target.value}));
	}
}
```

**Update arrays in state -**
```jsx
function component(){
	const [foods, setFoods] = useState(["apple", "orange", "banana"]);
	function handleAddfood(){
		const newFood = document.getElementById("foodInput").value;
		document.getElementById("foodInput").value=""; //set it back to empty
		setFoods(f => [...f, newFood]); //spread and write after that to auto append
	}

	function handleRemoveFood(index){ 
		setFoods(foods.filter((_, i) => i!==index)); //filter according to the index
	}

	return(
		<>
			<ul>
				{foods.map((food, index) => 
				<li key={index} onClick={() => handleRemoveFood(index)}>
				</li>)}
			</ul>
			<input type="text" id="foodInput" placeholder="Enter food name" />
			<button onClick={handleAddfood}>Add Food</button>
		</>
	);
}
```

