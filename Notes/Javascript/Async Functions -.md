Since JavaScript is the language of the web, there are some functions that by necessity are going to take a decent amount of time to complete, such as fetching data from a server to display on your site. For this reason, JavaScript includes support for asynchronous functions, or to put it another way, functions that can happen in the background while the rest of your code executes.
## Callbacks -
A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action
```javascript
myDiv.addEventListener("click", function(){
  // do something!
})
```
Here, the function `addEventListener()` takes a callback (the “do something” function) and then calls it when `myDiv` gets clicked.
Unfortunately, though they are useful in situations like the above example, using callbacks can get out of hand, especially when you need to chain several of them together in a specific order.
## Promises - 
Essentially, a promise is an object that might produce a value at some point in the future. 
We need some way to solve this problem, and tell our code to wait until the data is done fetching to continue.
```JavaScript
const myData = getData() // if this is refactored to return a Promise...

myData.then(function(data){ // .then() tells it to wait until the promise is resolved
  const pieceOfData = data['whatever'] // and THEN run the function inside
})
```
A new Promise is created with the `new` keyword and the promise provides `resolve` and `reject` functions to the provided callback:
```JavaScript
var p = new Promise(function(resolve, reject) {
	// Do an async task async task and then..
	if(/* good condition */) {
		resolve('Success!');
	}
	else {
		reject('Failure!');
	}
});
p.then(function(result) { 
	/* do something with the result */
}).catch(function() {
	/* error :( */
}).finally(function() {
   /* executes regardless or success for failure */ 
});
```
It's up to the developer to manually call `resolve` or `reject` within the body of the callback based on the result of their given task.
Sometimes you don't need to complete an async tasks within the promise -- if it's possible that an async action will be taken, however, returning a promise will be best so that you can always count on a promise coming out of a given function. In that case you can simply call Promise.resolve() or Promise.reject() without using the new keyword. For example:
```JavaScript
var userCache = {};
function getUserDetail(username) {
  // In both cases, cached or not, a promise will be returned
  if (userCache[username]) {
  	// Return a promise without the "new" keyword
    return Promise.resolve(userCache[username]);
  }
  // Use the fetch API to get the information
  // fetch returns a promise
  return fetch('users/' + username + '.json')
    .then(function(result) {
      userCache[username] = result;
      return result;
    })
    .catch(function() {
      throw new Error('Could not find user: ' + username);
    });
}
```

## then
All promise instances get a `then` method which allows you to react to the promise.  The first `then` method callback receives the result given to it by the `resolve()` call:
```JavaScript
new Promise(function(resolve, reject) {
	// A mock async action using setTimeout
	setTimeout(function() { resolve(10); }, 3000);
})
.then(function(result) {
	console.log(result);
});

// From the console:
// 10
```
The `then` callback is triggered when the promise is resolved.  You can also chain `then` method callbacks:
```JavaScript
new Promise(function(resolve, reject) { 
	// A mock async action using setTimeout
	setTimeout(function() { resolve(10); }, 3000);
})
.then(function(num) { console.log('first then: ', num); return num * 2; })
.then(function(num) { console.log('second then: ', num); return num * 2; })
.then(function(num) { console.log('last then: ', num);});

// From the console:
// first then:  10
// second then:  20
// last then:  40
```
Each `then` receives the result of the previous `then`'s return value.
If a promise has already resolved but `then` is called again, the callback immediately fires. If the promise is rejected and you call `then` after rejection, the callback is never called.
## catch

The `catch` callback is executed when the promise is rejected:

```JavaScript
new Promise(function(resolve, reject) {
	// A mock async action using setTimeout
	setTimeout(function() { reject('Done!'); }, 3000);
})
.then(function(e) { console.log('done', e); })
.catch(function(e) { console.log('catch: ', e); });

// From the console:
// 'catch: Done!'``
```
What you provide to the `reject` method is up to you. A frequent pattern is sending an `Error` to the `catch`:

`reject(Error('Data could not be found'));`

## Try-Catch
If we know that some part of the code can throw an error, but we still want to continue execution and not make the program exit at the thrown error we should wrap the probable error part in a try-catch block. This catches any errors that may be thrown when the program executes
```JavaScript
try{
	//logic that might throw an error
} catch(e){
	//do something with the error dawg
}
```

## finally
The newly introduced `finally` callback is called regardless of success or failure:
```JavaScript
(new Promise((resolve, reject) => { reject("Nope"); }))
    .then(() => { console.log("success") })
    .catch(() => { console.log("fail") })
    .finally(res => { console.log("finally") });

// >> fail
// >> finally
```

**Using With APIs -** [[APIs in JavaScript -]]

## Async and Await - 
Asynchronous code can become difficult to follow when it has a lot of things going on. `async` and `await` are two keywords that can help make asynchronous read more like synchronous code. This can help code look cleaner while keeping the benefits of asynchronous code.
For example, the two code blocks below do the exact same thing. They both get information from a server, process it, and return a promise.
```javascript
function getPersonsInfo(name) {
  return server.getPeople().then(people => {
    return people.find(person => { return person.name === name });
  });
}
```
```javascript
async function getPersonsInfo(name) {
  const people = await server.getPeople();
  const person = people.find(person => { return person.name === name });
  return person;
}
```
The `async` keyword is what lets the JavaScript engine know that you are declaring an asynchronous function. This is required to use `await` inside any function. When a function is declared with `async`, it automatically returns a promise; returning in an `async` function is the same as resolving a promise. Likewise, throwing an error will reject the promise.
`await` does the following: it tells JavaScript to wait for an asynchronous action to finish before continuing the function. It’s like a ‘pause until done’ keyword. The `await` keyword is used to get a value from a function where you would normally use `.then()`. Instead of calling `.then()` after the asynchronous function, you would assign a variable to the result using `await`. Then you can use the result in your code as you would in your synchronous code.
### Error handling
Handling errors in `async` functions is very easy. Promises have the `.catch()` method for handling rejected promises, and since async functions just return a promise, you can call the function, and append a `.catch()` method to the end.
```javascript
asyncFunctionCall().catch(err => {
  console.error(err)
});
```
But there is another way: the mighty `try/catch` block! If you want to handle the error directly inside the `async` function, you can use `try/catch` just like you would inside synchronous code.
```javascript
async function getPersonsInfo(name) {
  try {
    const people = await server.getPeople();
    const person = people.find(person => { return person.name === name });
    return person;
  } catch (error) {
    // Handle the error any way you'd like
  }
}
```
Doing this can look messy, but it is a very easy way to handle errors without appending `.catch()` after your function calls. How you handle the errors is up to you, and which method you use should be determined by how your code was written.

> `await` does not work on the global scope, we will have to create an `async` function that wraps our API call, that is why letting know that the function is async is necessary for the await keyword to work properly.

