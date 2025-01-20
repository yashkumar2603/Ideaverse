### Map -
`map` is one function. It expects a `callback` as an argument, which is a fancy way to say “I want you to pass another function as an argument to my function”.
Let’s say we had a function `addOne`, which takes in `num` as an argument and outputs that `num` increased by 1. And let’s say we had an array of numbers, `[1, 2, 3, 4, 5]` and we’d like to increment all of these numbers by 1 using our `addOne` function. Instead of making a `for` loop and iterating over the above array, we could use our `map` array method instead, which **automatically** iterates over an array for us. We don’t need to do any extra work aside from simply passing the function we want to use in:

```javascript
function addOne(num) {
  return num + 1;
}
const arr = [1, 2, 3, 4, 5];
const mappedArr = arr.map(addOne);
console.log(mappedArr); // Outputs [2, 3, 4, 5, 6]
```

`map` returns a new array and does not change the original array.

```javascript
// The original array has not been changed!
console.log(arr); // Outputs [1, 2, 3, 4, 5]
```

This is a much more elegant approach, what do you think? For simplicity, we could also define an inline function right inside of `map` like so:

```javascript
const arr = [1, 2, 3, 4, 5];
const mappedArr = arr.map((num) => num + 1);
console.log(mappedArr); // Outputs [2, 3, 4, 5, 6]
```

### Filter Method - 
`filter` is somewhat similar to `map`. It still iterates through the array and applies the callback function on every item. However, instead of transforming the values in the array, it returns the original values of the array, but only IF the callback function returns `true`. Let’s say we had a function, `isOdd` that returns either `true` if a number is odd or `false` if it isn’t.

The `filter` method expects the `callback` to return either `true` or `false`. If it returns `true`, the value is included in the output. Otherwise, it isn’t. Consider the array from our previous example, `[1, 2, 3, 4, 5]`. If we wanted to remove all even numbers from this array, we could use `.filter()` like this:

```javascript
function isOdd(num) {
  return num % 2 !== 0;
}
const arr = [1, 2, 3, 4, 5];
const oddNums = arr.filter(isOdd);
console.log(oddNums); // Outputs [1, 3, 5];
console.log(arr); // Outputs [1, 2, 3, 4, 5], original array is not af
```

### Reduce Method - 
Finally, let’s say that we wanted to multiply all of the numbers in our `arr` together like this: `1 * 2 * 3 * 4 * 5`. First, we’d have to declare a variable `total` and initialize it to 1. Then, we’d iterate through the array with a `for` loop and multiply the `total` by the current number.

But we don’t actually need to do all of that, we have our `reduce` method that will do the job for us. Just like `.map()` and `.filter()` it expects a callback function. However, there are two key differences with this array method:

- The callback function takes two arguments instead of one. The first argument is the `accumulator`, which is the current value of the result _at that point in the loop_. The first time through, this value will either be set to the `initialValue` (described in the next bullet point), or the first element in the array if no `initialValue` is provided. The second argument for the callback is the `current` value, which is the item currently being iterated on.
- It also takes in an `initialValue` as a second argument (after the callback), which helps when we don’t want our initial value to be the first element in the array. For instance, if we wanted to sum all numbers in an array, we could call reduce without an `initialValue`, but if we wanted to sum all numbers in an array and add 10, we could use 10 as our `initialValue`.

```javascript
const arr = [1, 2, 3, 4, 5];
const productOfAllNums = arr.reduce((total, currentItem) => {
  return total * currentItem;
}, 1);
console.log(productOfAllNums); // Outputs 120;
console.log(arr); // Outputs [1, 2, 3, 4, 5]
```

In the above function, we:
- Pass in a callback function, which is `(total, currentItem) => total * currentItem`.
- Initialize total to `1` in the second argument.

So what `.reduce()` will do, is it will once again go through every element in `arr` and apply the `callback` function to it. It then changes `total`, without actually changing the array itself. After it’s done, it returns `total`.