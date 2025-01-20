The `async` and `await` syntax in JavaScript provides a way to write asynchronous code that looks and behaves like synchronous code, making it easier to read and maintain.
It builds on top of Promises and allows you to avoid chaining `.then()` and `.catch()` methods while still working with asynchronous operations.
`async/await` is essentially syntactic sugar on top of Promises.
### Things to keep in mind
1. You can only call `await` inside a function if that function is `async`
2. You cant have a `top level await`

```JavaScript
function setTimeoutPromisified(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function solve() {
	await setTimeoutPromisified(1000);
	console.log("hi");
	await setTimeoutPromisified(3000);
	console.log("hello")()
	await setTimeoutPromisified(5000);
	console.log("hi there");
}
solve();
```
Although it seems like the thread is stuck there while awaiting, but it is not, this looks like synchronous code, but it is not

using Async when no promise is returned from the await part - 
```js
const createAccountHandler = async () => {
    const newMnemonic = generateMnemonic();
    setMnemonic(newMnemonic);
};
```


