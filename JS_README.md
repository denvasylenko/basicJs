# JS CORE

## Scope & Immutability

### **Immutability of Primitives**

**Explanation:**

Primitive values like numbers, strings, booleans, null, undefined, and symbols are immutable in JavaScript. This means that their value cannot be changed after creation.

**Example:**
```javascript
let str = "Hello";
str[0] = "h";  // Trying to change the first character
console.log(str);  // Outputs: "Hello" (the original string remains unchanged)
```

### **Mutable vs Immutable Objects**

**Explanation:**

While primitives are immutable, objects (including arrays and functions) are mutable. Changes to objects affect all references to that object.

**Example:**
```javascript
let obj = { name: "John" };
let objCopy = obj;  // objCopy references the same object
objCopy.name = "Doe";
console.log(obj.name);  // Outputs: "Doe" (Object is mutable)
```

### **Preventing Mutation (Creating Immutable Objects)**

#### **Object.freeze**

**Explanation:**

`Object.freeze()` prevents changes to an object's properties, but only at the first level. Nested objects can still be modified unless they are also frozen.

**Example:**
```javascript
// freeze only first level
let obj = Object.freeze({name: "John", obj: {name: "Jhon"}});
obj.name = "Doe";  // Fails silently (in strict mode throws an error)
obj.obj.name = "Doe";  // will allow modify
console.log(obj.name);  // Outputs: "John"
console.log(obj.obj.name);  // Outputs: "Doe"
```

### **Shallow vs Deep Copy**

**Explanation:**

When copying objects, shallow copies only duplicate the first level, whereas deep copies duplicate all levels. Shallow copying techniques like the spread operator or Object.assign() don’t clone nested objects.

**Example (Shallow Copy):**
```javascript
const person = { name: "Alice", details: { age: 25 } };
const shallowCopy = { ...person };
shallowCopy.details.age = 30;
console.log(person.details.age);  // Outputs: 30 (nested object reference is shared)
```
**Example (Deep Copy):**
```javascript
const deepCopy = JSON.parse(JSON.stringify(person));  // Deep copy without circular refs
deepCopy.details.age = 30;
console.log(person.details.age);  // Outputs: 25 (original object is unaffected)
```

## Object Destructuring

**Explanation:**

Object destructuring allows for extracting multiple properties from an object and assigning them to variables in a concise manner.

**Example:**

```javascript
const { name, age } = person;
console.log(name);  // Outputs: "Bob"
console.log(age);  // Outputs: 35

```

### Destructuring with Aliases
**Explanation:**

You can also rename variables when destructuring using the colon `(:)` syntax.

**Example:**

```javascript
const { name: personName, age: personAge } = person;
console.log(personName);  // Outputs: "Bob"
console.log(personAge);  // Outputs: 35
```


## Merging and Copying Objects

### Spread Operator

**Explanation:**

The spread operator `(...)` can be used to copy or merge objects. It performs a shallow copy, meaning nested objects remain referenced.

**Example:**
```javascript
const objA = { a: 1 };
const objB = { b: 2 };
const merged = { ...objA, ...objB };
console.log(merged);  // Outputs: { a: 1, b: 2 }
```

### Deep Merge

**Explanation:**

For deep copying/merging, libraries like Lodash’s cloneDeep or native cloning methods like `structuredClone()` (available in newer browsers) are required.

### Floating-point arithmetic 

**Explanation:**

In JavaScript, floating-point numbers are represented using a format called IEEE 754. This representation can introduce rounding errors for certain decimal values, especially when they cannot be represented exactly in binary.

0.1 and 0.2 are represented in binary with some precision errors due to their repeating nature in binary form.
0.1 + 0.2 in JavaScript actually evaluates to 0.30000000000000004.

**Example:**

```javascript
console.log(0.1 + 0.2 > 0.3);  // Outputs: true
```

The .toFixed(1) method is used to round a number to one decimal place and returns a string representation of that rounded value.

```javascript
console.log((0.1 + 0.2).toFixed(1) > 0.3);  // Outputs: true
```

## Coercion

### String Coercione

**Explanation:**

When performing operations between strings and numbers, JavaScript automatically converts (coerces) values to strings or numbers as needed.

**Example:**

```javascript
let result = 123 + "456";  
console.log(result);  // Outputs: "123456"
// Explanation: The number 123 is coerced to a string and concatenated with "456".
```

### Number Coercione

**Explanation:**

In numeric operations, strings that represent numbers are coerced into actual numbers. This leads to implicit type conversions.

**Example:**

```javascript
let result = "5" * 2;
console.log(result);  // Outputs: 10
// Explanation: The string "5" is coerced to the number 5, then multiplied by 2.
```

### Boolean Coercion

**Explanation:**

Certain values in JavaScript are considered “falsy”, meaning they coerce to false in boolean contexts. These include `false`, `0`, `""`, `null`, `undefined`, and `NaN`. All other values are considered “truthy”.

**Example:**

```javascript
if (0) {
  console.log("Won't be logged");
} else {
  console.log("Falsy value");  // This will be logged
}

if ("Hello") {
  console.log("Truthy value");  // This will be logged
}
```

### Double Equals (==) vs Triple Equals (===)
**Explanation:**

`==` allows type coercion when comparing values, while `===` requires both the value and the type to be the same.

**Example:**

```javascript
console.log(5 == "5");  // Outputs: true
// Explanation: The string "5" is coerced into the number 5 before comparison.

console.log(5 === "5");  // Outputs: false
// Explanation: No coercion happens here, so number 5 is not equal to string "5".
```

## Handling Coercion in Logical Operations
**Explanation:**

Logical operators `(&&, ||)` coerce values to booleans, but they return the actual value rather than `true` or `false`.

**Example:**

```javascript
console.log(0 || "default");  // Outputs: "default"
// Explanation: 0 is falsy, so "default" is returned.

console.log(1 && "next");  // Outputs: "next"
// Explanation: 1 is truthy, so "next" is returned.
```

## Coercion and 3 > 2 > 1
**Explanation:**

When null or undefined are used in arithmetic operations or comparisons, they are coerced as well.

**Example:**

```javascript
console.log(3 > 2 > 1);  // Outputs: false
// Explanation: 3 > 2 evaluates to true (which is 1 in JS), then 1 > 1 is false
```

## Coercion with null and undefined
**Explanation:**

Coercion with null and undefined: When null or undefined are used in arithmetic operations or comparisons, they are coerced as well.

**Example:**

```javascript
console.log(null + 1);  // Outputs: 1
// Explanation: `null` is coerced to 0 in numeric context.

console.log(undefined + 1);  // Outputs: NaN
// Explanation: `undefined` is coerced to `NaN` in numeric context, resulting in `NaN`.
```

## Object-to-Primitive Conversion
**Explanation:**

Objects can define how they convert to primitive values using the `valueOf()` method or `Symbol.toPrimitive` method. This is useful when an object is used in arithmetic or string contexts.

**Example:**

```javascript
let obj = {
  [Symbol.toPrimitive](hint) {
    if (hint === 'string') return 'Object as string';
    return 123;
  }
};
console.log(`${obj}`);  // Outputs: 'Object as string'
console.log(+obj);  // Outputs: 123

```

## Rest and Spread Operators

### Rest Operator `(...)`

**Explanation:**

The rest operator is used to collect multiple arguments or elements into an array. It is useful in function parameters and array destructuring.

**Example:**

```javascript
// In this example, ...numbers collects all the arguments into an array, which can then be processed using reduce().
function sum(...numbers) {
  return numbers.reduce((acc, num) => acc + num, 0);
}

console.log(sum(1, 2, 3, 4));  // Outputs: 10
```

Rest in Array Destructuring
You can use the rest operator to gather the "rest" of the elements in an array during destructuring.

**Example 2:** 

```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];
console.log(first);  // Outputs: 1
console.log(rest);  // Outputs: [2, 3, 4, 5]

```

Rest in Object Destructuring
You can use the rest operator to gather the "rest" of the elements in an object during destructuring.

**Example 3:** 

```javascript
const {first, ...rest} = {name: "Jhon", surname: "Enim", age: 45};
console.log(first);  // Outputs: Jhon
console.log(rest);  // Outputs: {surname: "Enim", age: 45}

```

### Spread Operator (...)

**Explanation:**

The spread operator expands arrays or objects. It is commonly used to copy or merge arrays and objects.

**Example:**

**Example 1: Spread in Arrays**

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log(combined);  // Outputs: [1, 2, 3, 4, 5, 6]
```

**Example 2: Spread in Objects**
```javascript
const obj1 = { name: "Alice", age: 25 };
const obj2 = { country: "USA" };
const mergedObj = { ...obj1, ...obj2 };
console.log(mergedObj);  // Outputs: { name: "Alice", age: 25, country: "USA" }
```


## Nullish Coalescing Operator (??)

**Explanation:**

The nullish coalescing operator (`??`) returns the right-hand operand when the left-hand operand is `null` or `undefined`, but not when it is a falsy value like `0` or `false`.

**Example:**

```javascript
let name = null;
let surname = null;
let defaultName = "Unknown";

console.log(name ?? defaultName);  // Outputs: "Unknown"
console.log(surname ?? defaultName);  // Outputs: "Unknown"
// Explanation: Since `name` is `null`, `defaultName` is returned.


// Unlike the logical OR (||), ?? only considers null and undefined as nullish values, not falsy values like 0 or false.

let value = 0;
let defaultValue = 10;

console.log(value || defaultValue);  // Outputs: 10 (because 0 is falsy)
console.log(value ?? defaultValue);  // Outputs: 0 (because 0 is not null or undefined)
```

## Scope:

### Global Scope

**Explanation:**

Variables declared outside of any function are in the global scope and can be accessed anywhere in the code. In browsers, these variables are attached to the global window object.

**Example:**


```javascript
var globalVar = "I'm global!";
console.log(window.globalVar);  // Outputs: I'm global
```


### Local Scope:*


**Explanation:**

Variables declared within a function are local to that function and cannot be accessed from outside.

**Example:**


```javascript
function localScope() {
  var localVar = "I'm local!";
  console.log(localVar);  // Outputs: "I'm local!"
}
localScope();
console.log(window.localVar);  // Outputs: undefined
```

### Block Scope:



**Explanation:**

Block-scoped variables (`let` and `const`) are only accessible within the block where they are defined. This is especially important in loops and conditionals.

**Example:**


```javascript
if (true) {
  let blockScopedVar = "I'm block scoped!";
}
console.log(blockScopedVar);  // Error: blockScopedVar is not defined

```

## Closures and `this` Context

### Closures


**Explanation:**

Closures are functions that retain access to variables from their outer scope, even after the outer function has finished executing.

**Example:**


```javascript
function createCounter() {
  let count = 0;
  return {
    increment() { count++; },
    getCount() { return count; }
  };
}
let counter = createCounter();
counter.increment();
console.log(counter.getCount());  // Outputs: 1
```


### `this` Context


**Explanation:**

The value of `this` depends on how a function is called. It refers to the object that is executing the current function.
**Example:**

```javascript
const obj = {
  name: "Alice",
  sayName() {
    console.log(this.name);
  }
};

obj.sayName();  // Outputs: Alice
const extracted = obj.sayName;
extracted();  // Outputs: undefined (in strict mode)
```

### Closures (Gotchas with Loops):


**Explanation:**

Closures remember the environment in which they were created, leading to unexpected behavior in loops.

**Example:**

```javascript
for (var i = 0; i < 3; i++) { // doesn't have block scope
  setTimeout(() => console.log(i), 100);
}
// Outputs: 3, 3, 3 (because `i` is shared)
```

```javascript
for (let i = 0; i < 3; i++) { // block scope
  setTimeout(() => console.log(i), 100);
}
// Outputs: 0, 1, 2 (let creates block-scoped variables)
```

## Hoisting

**Explanation:**

JavaScript hoists function and variable declarations to the top of their scope before execution, but assignments are not hoisted.

**Example:**

```javascript
console.log(a);  // Outputs: undefined
var a = 10;

function sayHello() {
  console.log("Hello");
}
```

### Function Hoisting


**Explanation:**

JavaScript hoists function and variable declarations to the top of their scope before execution, but assignments are not hoisted.

**Example:**

```javascript
sayHello();  // Outputs: "Hello"

function sayHello() {
  console.log("Hello");
}
```

## Functional Programming in JavaScript

### Array Methods: `filter`, `map`, `reduce`, `flat`, and `flatMap`

**Explanation:**

Functional programming focuses on writing pure functions that avoid side effects. JavaScript's array methods like filter(), map(), reduce(), and flatMap() align with this paradigm.

**`filter()` Example:**

```javascript
const nums = [1, 2, 3, 4, 5];
const evens = nums.filter(n => n % 2 === 0);
console.log(evens);  // Outputs: [2, 4]
```

**`map` Example:**  

```javascript
const doubled = nums.map(n => n * 2);
console.log(doubled);  // Outputs: [2, 4, 6, 8, 10]
```

**`reduce` Example:**  

```javascript
const sum = nums.reduce((acc, curr) => acc + curr, 0);
console.log(sum);  // Outputs: 15
```

**`flat` Example:**  

```javascript
const nested = [1, [2, 3], [4, [5]]];
console.log(nested.flat(2));  // Outputs: [1, 2, 3, 4, 5]
```

**`flatMap`  Example:**  
```javascript
const flatMapped = nums.flatMap(n => [n, n * 2]);
console.log(flatMapped);  // Outputs: [1, 2, 2, 4, 3, 6, 4, 8, 5, 10]
```

## Function Composition & Pipelining

### Function Composition

**Explanation:**

Function composition involves combining multiple functions so that the output of one function becomes the input to the next.

**Example:**

```javascript
const compose = (f, g) => x => f(g(x));

const add1 = x => x + 1;
const double = x => x * 2;

const add1ThenDouble = compose(double, add1);
console.log(add1ThenDouble(5));  // Outputs: 12
```

### Function Pipelining

**Explanation:**

Function pipelining is similar to composition but uses a left-to-right execution order.

**Example:**

```javascript
const add = x => x + 1;
const double = x => x * 2;
const compose = (...fs) => (x) => fs.reduce(f => f(a), x)
const composed = compose(add, double);  // Using lodash's flow for composition
console.log(composed(3));  // Outputs: 8 (3 + 1 = 4, then 4 * 2 = 8)
```


**Explanation:**

Currying is a functional programming technique where a function that takes multiple arguments is transformed into a sequence of functions, each taking a single argument. The curried version of a function allows partial application by pre-setting some arguments, similar to partial application but more naturally integrated.

Currying makes functions more reusable and flexible by enabling you to generate new functions with some arguments pre-defined. This technique is particularly useful for creating specialized functions from a generic one.

**Example:**

```javascript
const multiply = a => b => a * b;

const double = multiply(2);
const triple = multiply(3);

console.log(double(5));  // Outputs: 10
console.log(triple(5));  // Outputs: 15

```
### How It Works:
- The `multiply` function is written using currying, where it returns another function that takes a single argument.
- When `multiply(2)` is called, it returns a new function that expects one argument `(b)`. In this case, a is pre-set to `2`.
- Similarly, `multiply(3)` creates a function where `a` is pre-set to `3`.
- When `double(5)` is called, the inner function multiplies `2 * 5` to give `10`.
- When `triple(5)` is called, the inner function multiplies `3 * 5` to give `15`.

### Benefits of Currying:

- **Reusability**: You can create multiple specific functions from a single generic function (e.g., `double`, `triple` from `multiply`).
- **Modularity**: Currying encourages the breakdown of complex functions into smaller, more manageable functions.
- **Partial Application**: Currying naturally supports partial application, making it easier to set some arguments in advance.

**Example with Multiple Arguments:**

```javascript
const add = a => b => c => a + b + c;

const addFive = add(5);  // Pre-fills 'a' with 5
console.log(addFive(3)(2));  // Outputs: 10 (5 + 3 + 2)

```

### Partial Application

**Explanation:**

Partial application is a technique in functional programming where you create a new function by pre-filling some arguments of an existing function. It allows you to "lock in" certain arguments of a function, generating a new function that requires fewer arguments.

In JavaScript, you can achieve partial application using Function.prototype.bind(). The bind() method returns a new function with some of its arguments pre-set.

**Example:**


```javascript
function multiply(a, b, c) {
  return a * b * c;
}

const partiallyAppliedMultiply = multiply.bind(null, 2);
console.log(partiallyAppliedMultiply(3, 4));  // Outputs: 24

```
### How It Works:

- The multiply function takes three arguments: `a`, `b`, and `c`.
- Using `bind()`, we create a new function partiallyAppliedMultiply, where the first argument `(a)` is permanently set to `2`.
- When we call `partiallyAppliedMultiply(3, 4)`, it applies `2` as the first argument, `3` as the second argument `(b)`, and `4` as the third argument `(c)`.
- The result is `2 * 3 * 4 = 24`.


### Memoization of a Function


**Explanation:**

Memoization is an optimization technique that stores the results of expensive function calls and returns the cached result when the same inputs occur again. It is useful for functions that are called repeatedly with the same arguments, such as recursive algorithms.

In JavaScript, we can memoize functions by using a cache (often an object) to store previously computed results. When a memoized function is called, it first checks if the result for the given arguments exists in the cache. If it does, it returns the cached result. If not, it computes the result, stores it in the cache, and then returns it.

**Example:**


```javascript
function memoize(fn) {
  const cache = {};
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache[key]) {
      return cache[key];
    }
    const result = fn(...args);
    cache[key] = result;
    return result;
  };
}

const factorial = memoize(function(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
});

console.log(factorial(5));  // Outputs: 120
console.log(factorial(5));  // Outputs: 120 (cached result)
```

### How It Works:

- The `memoize()` function wraps a given function `(fn)` and adds caching behavior.
- The cache object stores previously computed results using the function arguments as the key.
- If the same function is called with the same arguments, it retrieves the result from the cache instead of recomputing it.
- In this case, the `factorial` function is memoized. The first call to `factorial(5)` computes and stores the result, while subsequent calls to `factorial(5)` return the cached value.

### Benefits:

- Memoization improves performance for expensive or recursive operations by avoiding redundant calculations.
- It is especially effective in dynamic programming problems, such as Fibonacci sequences or factorials, where overlapping subproblems occur.

### Higher-Order Functions

**Explanation:**

Higher-order functions are functions that take other functions as arguments or return functions.

**Example:**

```javascript
function higherOrder(fn) {
  return function(x) {
    return fn(x) * 2;
  };
}

function square(x) {
  return x * x;
}

const doubleSquare = higherOrder(square);
console.log(doubleSquare(3));  // Outputs: 18 (square(3) * 2 = 9 * 2 = 18)

```

### **Asynchronous Execution**

**Promises, async/await**

```javascript
async function fetchData() {
  const data = await new Promise(resolve => setTimeout(() => resolve("Data"), 1000));
  console.log(data);  // Outputs: Data
}

fetchData();
```

## Event Loop, setImmediate, Microtasks

### Promises & `async/await`

**Explanation:**

Promises and `async/await` simplify handling asynchronous operations by avoiding deeply nested callbacks.

**Example:**


```javascript
setImmediate(() => console.log('setImmediate'));
setTimeout(() => console.log('setTimeout'), 0);
Promise.resolve().then(() => console.log('Promise microtask'));
console.log('Sync log');

// Outputs: Sync log, Promise microtask, setTimeout, setImmediate
```

### try-catch-finally


**Explanation:**

The try block allows you to test code for errors, catch is used to handle the error, and `finally` executes after the try-catch, regardless of whether an error occurred or not.

**Example:**


```javascript
try {
  // Code that may throw an error
  let x = 5 / 0;
  console.log(x);
} catch (error) {
  // Handling the error
  console.log("An error occurred:", error.message);
} finally {
  // Code that always runs
  console.log("This runs no matter what");
}

// Outputs:
// Infinity
// This runs no matter what

```

### If an error occurs inside the try block

**Explanation:**

the `catch` block is executed. The `catch` block gets an error object that contains information about the error.

**Example:**


```javascript
try {
  // Code that will throw an error
  let result = JSON.parse('{"name": "John"');  // Missing closing }
} catch (error) {
  // Error is handled here
  console.log("Error caught:", error.message);
} finally {
  console.log("This will run regardless of error");
}

// Outputs:
// Error caught: Unexpected end of JSON input
// This will run regardless of error


```

### `finally` Block Always Executes

```javascript
try {
  // No error here
  console.log("Executing try block");
} catch (error) {
  console.log("This will not run since there's no error");
} finally {
  console.log("Cleaning up...");  // This always runs
}

// Outputs:
// Executing try block
// Cleaning up...

```

### Returning Values from `try`, `catch`, and `finally`

**Explanation:**

You can return values from try, catch, or finally, but finally will override the return values from try or catch if it also contains a return statement.

**Example:**


```javascript
function testReturn() {
  try {
    return "Returned from try";
  } catch (error) {
    return "Returned from catch";
  } finally {
    return "Returned from finally";  // This overrides the try/catch return
  }
}

console.log(testReturn());  // Outputs: "Returned from finally"

```


### Nested `try`, `catch`, and `finally`

**Explanation:**

You can nest try-catch-finally blocks inside each other for more granular error handling.

**Example:**


```javascript
try {
  console.log("Outer try block");

  try {
    throw new Error("Inner error");
  } catch (error) {
    console.log("Caught inner error:", error.message);
  } finally {
    console.log("Inner finally");
  }

} catch (error) {
  console.log("Caught outer error");
} finally {
  console.log("Outer finally");
}

// Outputs:
// Outer try block
// Caught inner error: Inner error
// Inner finally
// Outer finally

```


