Using JavaScript's built-in functions is often more concise and optimized. Here's a more concise way to achieve the same result:
```JavaScript
function findLargestElement(numbers) {
   return Math.max(...numbers);
}
```
`Math.max(...numbers)` uses **the spread operator (...)** to pass all elements of the numbers array as individual arguments to Math.max.

---
`const Str1 = str1.toLowerCase().split("").sort().join("");` -> to sort strings, feel free to remove the lower-case conversion function, kept to show that it has to be applied earlier than the rest else it gives error.

---
##### **Week-2 Assignment easy-3 -** 
You can implement the `calculateTotalSpentByCategory` function by iterating over the list of transactions, grouping them by category, and summing up the `price` for each category. Here’s how you can do it:
```javaScript
function calculateTotalSpentByCategory(transactions) {
    const categoryTotals = {}; //initialise it as object
    // Iterate over each transaction
    transactions.forEach(transaction => {
        const { category, price } = transaction; //obejct destructuring, see point 3
        
        // If the category exists, add the price to the total
        if (categoryTotals[category]) {
            categoryTotals[category] += price;
        } else {
            // If the category doesn't exist, initialize it with the current price
            categoryTotals[category] = price;
        }
    });
    // Convert the categoryTotals object to an array of objects
    return Object.keys(categoryTotals).map(category => ({
        category,
        totalSpent: categoryTotals[category]
    }));
}
```
###### Explanation:
1. **`categoryTotals` Object**: This object stores the total price for each category as you iterate over the transactions.
2. **`forEach` Loop**: Iterates over each transaction, checking if the category already exists in the `categoryTotals` object. If it does, it adds the current transaction's `price` to the existing total; if not, it initializes the total for that category with the current `price`.
3. The line `const { category, price } = transaction;` is using **object destructuring** in JavaScript. This syntax allows you to extract specific properties from an object and assign them to variables.
4. **`Object.keys(...).map(...)`**: Converts the `categoryTotals` object into an array of objects, where each object has a `category` and the corresponding `totalSpent`.
This function will return an array of objects, each representing a category with the total amount spent in that category. V IMP, GOOD METHOD

#### Replace stuff in strings using RegEx - 
Common Syntax:`/pattern/flags`A regular expression in JavaScript is defined between two slashes, and optional flags can follow the second slash.
Example: Removing Whitespace
`expression = expression.replace(/\s+/g, '');`
`\s`: Matches any whitespace character (spaces, tabs, line breaks).
+: Quantifier that matches one or more of the preceding element (`\s` in this case).
g: Global flag, which means the pattern will be applied to all matches in the string, not just the first one.
replace(...):
This method replaces all occurrences of the matched pattern in the string with the specified replacement (in this case, an empty string '', which removes the whitespace).

#### Validating Expressions using RegEx - 
```JavaScript
if (!/^[0-9+\-*/().]+$/.test(expression)) {
    throw new Error("Invalid characters in expression.");
}
```
![[Pasted image 20240813033416.png]]

Log custom errors to console - 
`console.error("error message");`


[[Async-Await]]
[[Promises -]]
