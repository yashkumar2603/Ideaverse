```JavaScript
function wait(n) {
  return new Promise(resolve => setTimeout(resolve, n*1000));
}
```
Most Simple implementation of the promise resolve, basically returns(execute the resolve function after the set timeout).

```JavaScript
function sleep(milliseconds) {
  return new Promise((resolve) => {
        const start = Date.now();
        while (Date.now() - start < milliseconds) {
            // Busy-wait loop
        }
        resolve();
  });
}
```
This code basically blocks the thread for the given time, so that the thread goes busy and no other process can be executed while it is blocked by this.'
See the use of resolve here,  after some code is executed, only then the resolve is returned.4
![[Pasted image 20240815022934.png]]
The resolve function can be anything, not necessarily just resolve 
![[Pasted image 20240815023818.png]]
We put the readFile() value in the proxy p, then that calls the readTheFile(resolving_function), then does some shit and the syntax is, function(err, data) -> handle errors and call the resolving_function then control reaches the p.then(callback), now we can do anything after the promise.

#### Promise.all()
```JavaScript
/*
 * Write 3 different functions that return promises that resolve after t1, t2, and t3 seconds respectively.
 * Write a function that uses the 3 functions to wait for all 3 promises to resolve using Promise.all,
 * Return a promise.all which return the time in milliseconds it takes to complete the entire operation.
 */
function wait1(t) {
  return new Promise(resolve => setTimeout(resolve, t*1000));
}
function wait2(t) {
   return new Promise(resolve => setTimeout(resolve, t*1000));
}
function wait3(t) {
   return new Promise(resolve => setTimeout(resolve, t*1000));
}
function calculateTime(t1, t2, t3) {
  return Promise.all([wait1(t1), wait2(t2), wait3(t3)]).then(() => {
    return (Math.max(t1, t2, t3))*1000;
  });
}

module.exports = calculateTime;
```
#### Promise chain
```JavaScript
/*
 * Write 3 different functions that return promises that resolve after t1, t2, and t3 seconds respectively.
 * Write a function that sequentially calls all 3 of these functions in order.
 * Return a promise chain which return the time in milliseconds it takes to complete the entire operation.
 * Compare it with the results from 3-promise-all.js
 */
function wait1(t) {
  return new Promise(resolve => setTimeout(resolve, t*1000));
}
function wait2(t) {
  return new Promise(resolve => setTimeout(resolve, t*1000));
}
function wait3(t) {
  return new Promise(resolve => setTimeout(resolve, t*1000));
}
function calculateTime(t1, t2, t3) {
  return wait1(t1).then(() => wait2(t2)).then(() => wait3(t3)).then(() => {
    return (t1+ t2+ t3)*1000;
  });
}

module.exports = calculateTime;
```

### Resolve/Reject 
![[Pasted image 20240815153342.png]]
**Or do this**
```JavaScript
const fs = require("fs");

function readFilePromisified(filePath) {
  return new Promise(function (resolve, reject) {
    fs.readFile(filePath, "utf-8", function (err, data) {
      if (err) {
        reject("Error while reading file");
      } else {
        resolve(data);
      }
    });
  });
}

function onDone(data) {
  console.log(data);
}

function onError(err) {
  console.log("Error: " + err);
}
readFilePromisified("a.txt").then(onDone).catch(onError);
```

 