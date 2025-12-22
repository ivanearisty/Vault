---
tags:
---
# JS
## Variables
### var
1. **Scope**: Function-scoped.
2. **Hoisting**: Declaration is moved to the top, although the assignment stays where it is.
### let
1. **Scope**: Block-scoped, accessible only within the nearest enclosing `{ ... }`
2. **Temporal Dead Zone**: Can't access before declaration.
### const
1. **Constant**: Once a `const` is assigned, its reference cannot be changed.
2. **Scope**: Block-scoped.
3. **No Redeclaration, No Reassignment**: You must assign a value at the time of declaring a `const`, and you can’t change the reference later.

## Functions
### Traditional
Declared with the `function` keyword. Has its own `this` context depending on how it is invoked.
```js
function greet(name) {
  return `Hello, ${name}!`;
}
console.log(greet("Alice")); // "Hello, Alice!"
```
### Arrow Function
- Declared with =>.
- **Implicit Return**: If the function body is a single expression, you can omit the braces and the `return` keyword, and the expression’s value is returned automatically.
- **this Binding**: Arrow functions do not bind their own `this`; they inherit the `this` from the parent scope.

```js
// Arrow function with braces and explicit return
const greet = (name) => {
  return `Hello, ${name}!`;
};

// Arrow function with implicit return
const greetImplicit = (name) => `Hello, ${name}!`;

// Example demonstrating `this` behavior
function Person() {
  this.age = 0;

  setInterval(() => {
    // 'this' here is inherited from Person()
    this.age++;
    console.log(this.age);
  }, 1000);
}
```
## Syntax
### Spread Operator
Spread syntax can be used when all elements from an object or array need to be included in a new array or object, or should be applied one-by-one in a function call's arguments list. There are three distinct places that accept the spread syntax:
- [Function arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#spread_in_function_calls) list (`myFunction(a, ...iterableObj, b)`)
- [Array literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#spread_in_array_literals) (`[1, ...iterableObj, '4', 'five', 6]`)
- [Object literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#spread_in_object_literals) (`{ ...obj, key: 'value' }`)

Only iterable values, like Array and String, can be spread in array literals and argument lists. Many objects are not iterable, including all plain objects that lack a Symbol.iterator method.

```js
const obj = { key1: "value1" };
const array = [...obj]; // TypeError: obj is not iterable
```
#### Spread in function calls
```js
function myFunction(x, y, z) {}
const args = [0, 1, 2];
myFunction(...args);

function myFunction(x, y, z) {}
const args = [0, 1, 2];
myFunction(...args);
```

## Objects
### Modifying
React often deals with deeply nested data (like state or props). You should be comfortable navigating and updating nested arrays and objects without mutating them directly (since React state updates should be immutable whenever possible).
```js
// Example nested data
const user = {
  name: "Alice",
  address: {
    street: "123 Main St",
    city: "Wonderland"
  },
  hobbies: ["reading", "painting"]
};

// Access nested property
console.log(user.address.street); // "123 Main St"

// Updating nested array (immutably)
const newUser = {
  ...user,
  hobbies: [...user.hobbies, "hiking"]
};
```

## Promises vs Callbacks
| **Feature**        | **Callbacks**                                          | **Promises**                                                       |     |
| ------------------ | ------------------------------------------------------ | ------------------------------------------------------------------ | --- |
| **Chaining**       | Nested callbacks (callback hell)                       | Cleaner chaining with `.then()` and `.catch()`.                    |     |
| **Error Handling** | Errors need to be handled in each callback explicitly. | Errors bubble up and can be caught in a single `.catch()`.         |     |
| **Readability**    | Can get messy with deeply nested callbacks.            | Promises make the code more readable.                              |     |
| **Modern Usage**   | Older pattern, used for backward compatibility.        | Promises are the modern standard, especially with `async`/`await`. |     |
