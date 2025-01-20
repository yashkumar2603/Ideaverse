---
Rank: "2"
---

| Resources                                                                                            | Rating | Study??                                                                                                                                |     |     |
| ---------------------------------------------------------------------------------------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- | --- | --- |
| [Full Stack JavaScript The Odin Project](https://www.theodinproject.com/paths/full-stack-javascript) | 10     | Yes, atleast the start, very concise, it is only what i need to learn<br>fully text based<br>Reference as a Index of what all to learn |     |     |
|                                                                                                      |        |                                                                                                                                        |     |     |
The point is that i need to learn the parts of JavaScript that are relevant to react development, so that i can start learning full fledged react.

### Variables - 
```JavaScript
let x = "hi";
let y = 1524;  //same definition for all data types
let z = 1234567890987654321234567890n; //add an n at the end to get it in the form of a big integer type.

let hehe;
console.log(hehe); //will return undefined, as the variable hasnt been defined with anything in it.

z = null; //used to clear out the value in the variable.

//define object
let course = {
	name:'javaScript for beginners',
	duration: 3
};
console.log(course); //outputs object
//accessing each value inside of the object -> 
Course.name = 'hehe';
//or
Course['name'] = 'hehe';

const c = Symbol(); //known as symbol
console.log(c); //returns Symbol();

//on doing typeof null -> we get 'object' and not null
```

#### Dynamically typing Datatypes - 
```JavaScript
let first = 'yash';
first = 100;
first = true;
```
This is dynamic typing, the variable changes its datatype with what is being filled in it.

### Arrays - 
```JavaScript
let arr = ['blue', 'green'];
console.log(arr);
//will output all in brackets
console.log(arr[0]);
//we can have different datatypes in the same array.
//datatype of array is object
```

for loop - 
```JavaScript
for(let i=0; i<arr.length(); i++){
}

//for in loop
for(const key in arr){
	console.log(arr[key]);
}

//for of loop
for(let element of arr){
	console.log(element);
}
```
[[JavaScript Array Methods -]]
### Functions - 
```JavaScript
function sayHi(name) {
	console.log('Hi'+name);
}
sayHi('inrgo');

//or
function sayHi() {
	console.log('Hi');
}
sayHi();

//if the function doesnt return anything but u still try to print the value of the function, it returns undefined

//Functions are considered as object in JS, we can do 
const n=sayHi;
console.log(sayHi.length); //returns the no. of parameters
```

#### Checking Equality - 
There are two things here, strict equality and loose equality - 
```JavaScript
let a=2;
let b='2';
console.log(a==b); //loose equality, returns true, JS converts and checks for us
console.log(a===b); //strict equality, returns false as datatypes are different.
```


#### **The patterns we’ll be covering are:**
- [x] Plain Old JavaScript Objects and Object Constructors
- [x] Factory Functions and the Module Pattern
- [x] Classes
- [x] ES6 Modules

### Objects Basics -
```JavaScript
function increaseCounterObject(objectCounter) {
  objectCounter.counter += 1;
}

function increaseCounterPrimitive(primitiveCounter) {
  primitiveCounter += 1;
}

const object = { counter: 0 };
let primitive = 0;

increaseCounterObject(object);
increaseCounterPrimitive(primitive);
```
the object counter would increase by 1, and the primitive counter wouldn’t change, you’re correct. Remember that the parameter `objectCounter` contains a _reference_ to the same object as the `object` variable we gave it, while `primitiveCounter` contains only a copy of the primitive value only.

Remember, functions and also objects in JavaScript
#### Constructor - 
When you have a specific type of object that you need to duplicate like our player or inventory items, a better way to create them is using an object constructor, which is a function that looks like this:

```javascript
function Player(name, marker) {
  this.name = name;
  this.marker = marker;
}
```

and which you use by calling the function with the keyword `new`.

```javascript
const player = new Player('steve', 'X');
console.log(player.name); // 'steve'
```

Just like with objects created using the Object Literal method, you can add functions to the object:

```javascript
function Player(name, marker) {
  this.name = name;
  this.marker = marker;
  this.sayName = function() {
    console.log(this.name)
  };
}

const player1 = new Player('steve', 'X');
const player2 = new Player('also steve', 'O');
player1.sayName(); // logs 'steve'
player2.sayName(); // logs 'also steve'
```


### Closures - 
```javascript
function makeAdding (firstNumber) {
  // "first" is scoped within the makeAdding function
  const first = firstNumber;
  return function resulting (secondNumber) {
    // "second" is scoped within the resulting function
    const second = secondNumber;
    return first + second;
  }
}
// but we've not seen an example of a "function"
// being returned, thus far - how do we use it?

const add5 = makeAdding(5);
console.log(add5(2)) // logs 7
```

A lot going on, so let’s break it down:
1. The `makeAdding` function takes an argument, `firstNumber`, declares a constant `first` with the value of `firstNumber`, and returns another **function**.
2. When an argument is passed to the returned function, which we have assigned to **add5**, it returns the result of adding up the number passed earlier to the number passed now (`first` to `second`).

Now, while it may sound good at first glance, you may already be raising your eyebrows at the second statement. As we’ve learned, the `first` variable is scoped within the `makeAdding` function. When we declare and use `add5`, however, we’re **outside** the `makeAdding` function. How does the `first` variable still exist, ready to be added when we pass an argument to the `add5` function? This is where we encounter the concept of closures.
Functions in JavaScript form closures. A closure refers to the combination of a function and the **surrounding state** in which the function was declared. This surrounding state, also called its **lexical environment**, consists of any local variables that were in scope at the time the closure was made. Here, `add5` is a reference to the `resulting` function, created when the `makeAdding` function is executed, thus it has access to the lexical environment of the `resulting` function, which contains the `first` variable, making it available for use.
This is a **crucial** behavior of functions - allowing us to associate data with functions and manipulate that data anywhere outside of the enclosing function.

### Factory Functions - 
One of the key arguments against constructors, in fact, the biggest argument is how they _look_ like regular JavaScript functions, even though they do not _behave_ like regular functions. If you try to use a constructor function without the `new` keyword, not only does your program fail to work, but it also produces error messages that are hard to track down and understand.

Yet another issue stems from the way the `instanceof` works. It checks the presence of a constructor’s prototype in an object’s _entire_ prototype chain - which does nothing to confirm if an object was made with that constructor since the constructor’s prototype can even be reassigned after the creation of an object.
While still seen in code, these problems led to the use of a pattern that was similar to constructors but addressed a ton of these problems: Factory Functions.

Instead of using the `new` keyword to create an object, factory functions set up and return the new object when you call the function. They do not use the prototype, which incurs a performance penalty - but as a general rule, this penalty isn’t significant unless you’re creating thousands of objects. Let’s take a basic example to compare them to constructor functions.
```javascript
const User = function (name) {
  this.name = name;
  this.discordName = "@" + name;
}
// hey, this is a constructor - 
// then this can be refactored into a factory!

function createUser (name) {
  const discordName = "@" + name;
  return { name, discordName };
}
// and that's very similar, except since it's just a function,
// we don't need a new keyword
```

#### Wrapping functions - 
Oftentimes, you do not need a factory to produce multiple objects - instead, you are using it to wrap sections of code together, hiding the variables and functions that you do not need elsewhere as private. This is easily achievable by wrapping your factory function in parentheses and immediately calling (invoking) it. This immediate function call is commonly referred to as an Immediately Invoked Function Expression (duh) or IIFE in short. This pattern of wrapping a factory function inside an IIFE is called the module pattern.
```javascript
const calculator = (function () {
  const add = (a, b) => a + b;
  const sub = (a, b) => a - b;
  const mul = (a, b) => a * b;
  const div = (a, b) => a / b;
  return { add, sub, mul, div };
})();

calculator.add(3,5); // 8
calculator.sub(6,2); // 4
calculator.mul(14,5534); // 77476
```
In this example, we have a factory function creating some basic operations that we need only once. We can wrap it in parentheses and immediately call it by adding `()` - returning the result object that we store in `calculator`. In this way we can write code, wrapping away things that we do not need as private variables and functions inside our factory function and while they are tucked inside of our module, we can use the returned variables and functions outside the factory, as necessary.

### Next Topics - 
[[Form Validations in JavaScript]]
[[Async Functions -]]
[[APIs in JavaScript -]]
