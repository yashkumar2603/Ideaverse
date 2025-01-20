Before Advanced, everything is with respect to making a To-Do app, which is the common application of this
# Fetching elements
There are 5 popular methods available for fetching DOM elements -
- querySelector
- querySelectorAll
- getElementById
- getElementByClassName
- getElementsByClassName
#### 1. Fetching the title
![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F32405ea3-b9b2-4dca-85fe-104329f6ad6e%2FScreenshot_2024-08-17_at_5.39.48_PM.png?table=block&id=a4652ede-6101-4dcb-b5f4-945007843aa2&cache=v2)

```javascript
const title = document.querySelector('h1');
console.log(title.innerHTML)
```

#### 2. Fetching the first TODO (Assignment)
![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F96a53c36-b961-4adf-ae9f-38fd9ac4870e%2FScreenshot_2024-08-17_at_5.42.34_PM.png?table=block&id=3d216c96-4fc2-487c-a1af-a2eec7cca071&cache=v2)

```javascript
const firstTodo = document.querySelector('h4');
console.log(firstTodo.innerHTML)
```

#### 3. Fetching the `second` TODO (Assignment)

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Fc6711776-eb43-4f83-aaa6-721da76c2292%2FScreenshot_2024-08-17_at_5.44.15_PM.png?table=block&id=e3469432-9cd6-4b65-b2d8-ee9401e6787e&cache=v2)

```javascript
const secondTodo = document.querySelectorAll('h4')[1];
console.log(secondTodo.innerHTML)
```

# Deleting elements
- removeChild - Removes a specific `node` of a `parent`
- onclick - function that triggers whenever you `click` on a button

#### Assignment - Add a `delete` button right next to the `todo` that deletes that todo

```javascript
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>replit</title>
  <link href="style.css" rel="stylesheet" type="text/css" />
</head>

<body>
  <h1>Todo list</h1>
  <div>
    <div id="todo-1">
      <h4>1. Take class</h4>
      <button onclick="deleteTodo(1)">delete</button>
    </div>
    <div id="todo-2">
      <h4>2. Go out to eat</h4>
      <button onclick="deleteTodo(2)">delete</button>
    </div>
  </div>
  <div>
    <input type="text"></input>
    <button>Add Todo</button>
  </div>
</body>

<script>
  function deleteTodo(index) {
    const element = document.getElementById("todo-" + index);
    element.parentNode.removeChild(element);
  }
</script>

</html>
```

Another experiment we did in class -

```javascript
<html>
    <body id="body">
        <h2>Todo 1</h2>
        <h2>Todo 2</h2>
        <h2>Todo 3</h2>
        <button onclick="deleteRandomTodo()">Delete todo!</button>
    </body>
    <script>
        function deleteRandomTodo() {
            const element = document.querySelector("h2");
            const parentElement = element.parentNode;
            parentElement.removeChild(element);
        }

    </script>
</html>
```

# Adding elements
What we’re learning -
- createElement
- appendChild

#### Assignment - Write a function to add a TODO `text` to the list of todos
Steps -
1. Get the current text inside the input element
2. Create a new `div` element
3. Add the `text` from step 1 to the `div` element
4. Append the `div` to the todos list
```javascript
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>replit</title>
  <link href="style.css" rel="stylesheet" type="text/css" />
</head>

<body>
  <h1>Todo list</h1>
  <div id="todos">
    <div id="todo-1">
      <h4>1. Take class</h4>
      <button onclick="deleteTodo(1)">delete</button>
    </div>
    <div id="todo-2">
      <h4>2. Go out to eat</h4>
      <button onclick="deleteTodo(2)">delete</button>
    </div>
  </div>
  <div>
    <input id="inp" type="text"></input>
    <button onclick="addTodo()">Add Todo</button>
  </div>
</body>

<script>
  function addTodo() {
    const inputEl = document.getElementById("inp");
    const textNode = document.createElement("div");
    textNode.innerHTML = inputEl.value;
    const parentEl = document.getElementById("todos");
    parentEl.appendChild(textNode);

  }
</script>

</html>
```
![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F7f30661b-5052-4e15-9f94-1239b54ff2bf%2FScreenshot_2024-08-17_at_6.55.43_PM.png?table=block&id=f3a19c89-0824-49a3-801e-fb38bccab8b1&cache=v2)

# More complex elements
Until now, we created a simple `div` element
```javascript
const textNode = document.createElement("div");
textNode.innerHTML = inputEl.value;
```
The problem is it doesn’t have a corresponding `delete` button.
![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F0478828d-e281-45e4-b82f-58353be8553e%2FScreenshot_2024-08-17_at_7.05.47_PM.png?table=block&id=5962d472-3977-44d8-8eb0-951bd4ee705a&cache=v2)
Can you try to fix it?
#### Solution #1

```javascript
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>replit</title>
  <link href="style.css" rel="stylesheet" type="text/css" />
</head>

<body>
  <h1>Todo list</h1>
  <div id="todos">
    <div id="todo-1">
      <h4>1. Take class</h4>
      <button onclick="deleteTodo(1)">delete</button>
    </div>
    <div id="todo-2">
      <h4>2. Go out to eat</h4>
      <button onclick="deleteTodo(2)">delete</button>
    </div>
  </div>
  <div>
    <input id="inp" type="text"></input>
    <button onclick="addTodo()">Add Todo</button>
  </div>
</body>

<script>
  let currentIndex = 3;
  function addTodo() {
    const inputEl = document.getElementById("inp");
    const textNode = document.createElement("div");
    textNode.innerHTML = "<div id='todo-" + currentIndex + "'><h4>" + inputEl.value + '</h4><button onclick="deleteTodo(' + currentIndex + ') ">Delete</button>';
    const parentEl = document.getElementById("todos");
    parentEl.appendChild(textNode);

    currentIndex = currentIndex + 1;
  }

  function deleteTodo(index) {
    const element = document.getElementById("todo-" + index);
    element.parentNode.removeChild(element);
  }
</script>

</html>
```

#### Solution #2

```javascript
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>Todo List</title>
  <link href="style.css" rel="stylesheet" type="text/css" />
</head>

<body>
  <h1>Todo list</h1>
  <div id="todos">
    <div id="todo-1">
      <h4>1. Take class</h4>
      <button onclick="deleteTodo(1)">Delete</button>
    </div>
    <div id="todo-2">
      <h4>2. Go out to eat</h4>
      <button onclick="deleteTodo(2)">Delete</button>
    </div>
  </div>
  <div>
    <input id="inp" type="text">
    <button onclick="addTodo()">Add Todo</button>
  </div>

  <script>
    let currentIndex = 3;

    function addTodo() {
      const inputEl = document.getElementById("inp");
      const todoText = inputEl.value.trim();

      if (todoText === '') {
        alert('Please enter a todo item.');
        return;
      }

      const parentEl = document.getElementById("todos");

      // Create new todo div
      const newTodo = document.createElement('div');
      newTodo.setAttribute("id", 'todo-' + currentIndex);

      // Create new heading element
      const newHeading = document.createElement('h4');
      newHeading.textContent = currentIndex + '. ' + todoText;

      // Create new button element
      const newButton = document.createElement('button');
      newButton.textContent = 'Delete';
      newButton.setAttribute("onclick", "deleteTodo(" + currentIndex + ")");

      // Append elements to the new todo div
      newTodo.appendChild(newHeading);
      newTodo.appendChild(newButton);

      // Append new todo to the parent element
      parentEl.appendChild(newTodo);

      // Increment the index for the next todo item
      currentIndex++;

      // Clear the input field
      inputEl.value = '';
    }

    function deleteTodo(index) {
      const element = document.getElementById("todo-" + index);
      if (element) {
        element.parentNode.removeChild(element);
      }
    }
  </script>
</body>

</html>
```

# Advanced DOM -
a slightly complex DOM element gets appended to the DOM.
```javascript
<div>
	<h1>hi there<h1>
</div>
```
#### Approach #1
```javascript
<body>
	<button onclick="createComplexDomElement()">Add</button>
</body>
<script>
	function createComplexDomElement() {
		const div = document.createElement("div");
		div.innerHTML = "<h1> hi there </h1>";
		document.querySelector("body").appendChild(div);
	}
</script>
```
Let’s look at a slightly better approach of doing the same thing.
#### Creating a DOM element which has another DOM element inside
```javascript
<body>
	<button onclick="createComplexDomElement()">Add</button>
</body>
<script>
	function createComplexDomElement() {
		const div = document.createElement("div");
		const h1 = document.createElement("h1");
		h1.innerHTML = "random text";
		div.appendChild(h1);
		document.querySelector("body").appendChild(div);
	}
</script>
```

### Slightly advanced To-Do  app JS-
```JavaScript
function addTodo() {
      const inputEl = document.getElementById("inp");
      const todoText = inputEl.value.trim();

      if (todoText === '') {
        alert('Please enter a todo item.');
        return;
      }

      const parentEl = document.getElementById("todos");

      // Create new todo div
      const newTodo = document.createElement('div');
      newTodo.setAttribute("id", 'todo-' + currentIndex);

      // Create new heading element
      const newHeading = document.createElement('h4');
      newHeading.textContent = currentIndex + '. ' + todoText;

      // Create new button element
      const newButton = document.createElement('button');
      newButton.textContent = 'Delete';
      newButton.setAttribute("onclick", "deleteTodo(" + currentIndex + ")");

      // Append elements to the new todo div
      newTodo.appendChild(newHeading);
      newTodo.appendChild(newButton);

      // Append new todo to the parent element
      parentEl.appendChild(newTodo);

      // Increment the index for the next todo item
      currentIndex++;

      // Clear the input field
      inputEl.value = '';
    }

    function deleteTodo(index) {
      const element = document.getElementById("todo-" + index);
      if (element) {
        element.parentNode.removeChild(element);
      }
    }
```

### State derived frontends -
To make frontends easier to code, the concept of `state` came into the picture. You will see this more when we reach react.
There are three `jargon` we need to understand
1. State - The `variable` parts of an app.
2. Components - How to `render` `state` on screen.
3. Rendering - Taking the `state` and rendering it on the `DOM` based on the `components`
![[Pasted image 20240822031351.png]]

**[Best ToDo-Web App using only DOM Manipulation](https://github.com/Master-utsav/Render-Todo)**
