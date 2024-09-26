### **Scope**

#### **JS CORE**

Immutability of Strings
Explanation:
Primitive types like strings are immutable. Once a string is created, it cannot be altered. When you try to change a string, a new string is created instead.
```javascript
let str = "Hello";
str[0] = "h";  // Trying to change the first character
console.log(str);  // Outputs: "Hello" (the original string remains unchanged)

```

Immutability Objects Explanation: 
Primitive values are immutable, whereas objects are mutable. This leads to different behaviors when modifying or assigning values.

```javascript
let obj = {name: "John"};
let objCopy = obj;  // objCopy references the same object
objCopy.name = "Doe";
console.log(obj.name);  // Outputs: "Doe", object is mutable and was modified

```
Prevent Mutation (Immutable Operations):
```javascript
// freeze only first level
let obj = Object.freeze({name: "John", obj: {name: "Jhon"}});
obj.name = "Doe";  // Fails silently (in strict mode throws an error)
obj.obj.name = "Doe";  // will allow modify
console.log(obj.name);  // Outputs: "John"
console.log(obj.obj.name);  // Outputs: "Doe"
```

Copying Objects

spread will copy only first level and deep object will remain reference
```javascript
const shallowCopy = { ...person };
shallowCopy.age = 40;
console.log(person.age);  // Outputs: 35 (original object remains unchanged)
console.log(shallowCopy.age);  // Outputs: 40 (copy is modified)

```
For a deep copy of objects containing nested objects, you need to use libraries like Lodash or structured cloning methods, as Object.assign() and the spread operator only perform shallow copies.

```javascript
// this approach will not copy function
const deepCopy = JSON.parse(JSON.stringify(person));  // Deep copies only non-circular objects
```

Object Destructuring

Destructuring allows you to extract values from an object and assign them to variables in a concise way.

```javascript
const { name, age } = person;
console.log(name);  // Outputs: "Bob"
console.log(age);  // Outputs: 35

```

```javascript
const { name: personName, age: personAge } = person;
console.log(personName);  // Outputs: "Bob"
console.log(personAge);  // Outputs: 35
```

Object Property Shorthand

```javascript
const firstName = "Charlie";
const age = 28;

const person = { firstName, age };  // Shorthand syntax
console.log(person);  // Outputs: { firstName: "Charlie", age: 28 }
```

Merging Objects

```javascript
// but again if you have deep structure then it's preferable use cloneDeep
const mergedWithSpread = { ...objA, ...objB };
console.log(mergedWithSpread);  // Outputs: { a: 1, b: 2 }

```

### **Coercion**
String Coercion: When a non-string value is used in a context where a string is expected, JavaScript will convert it to a string.

```javascript
let result = 123 + "456";  
console.log(result);  // Outputs: "123456"
// Explanation: The number 123 is coerced to a string and concatenated with "456".
```

Number Coercion: When a non-number value is used in a numeric context, JavaScript will attempt to convert it to a number.


```javascript
let result = "5" * 2;
console.log(result);  // Outputs: 10
// Explanation: The string "5" is coerced to the number 5, then multiplied by 2.
```

Boolean Coercion: JavaScript converts values to booleans in contexts such as conditions in if statements. The following values are falsy:

- false
- 0
- "" (empty string)
- null
- undefined
- NaN (Not-a-Number)
- Everything else is truthy.

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

Coercion in Comparisons (Double vs Triple Equals):

- == (double equals) allows coercion before comparing, while === (triple equals) does not.

```javascript
console.log(5 == "5");  // Outputs: true
// Explanation: The string "5" is coerced into the number 5 before comparison.

console.log(5 === "5");  // Outputs: false
// Explanation: No coercion happens here, so number 5 is not equal to string "5".
```

Coercion in Logical Operators (|| and &&): Logical operators (|| and &&) often perform coercion by converting values to booleans, but they return the actual value rather than a boolean.

```javascript
console.log(0 || "default");  // Outputs: "default"
// Explanation: 0 is falsy, so "default" is returned.

console.log(1 && "next");  // Outputs: "next"
// Explanation: 1 is truthy, so "next" is returned.
```
Coercion with null and undefined: When null or undefined are used in arithmetic operations or comparisons, they are coerced as well.


```javascript
console.log(null + 1);  // Outputs: 1
// Explanation: `null` is coerced to 0 in numeric context.

console.log(undefined + 1);  // Outputs: NaN
// Explanation: `undefined` is coerced to `NaN` in numeric context, resulting in `NaN`.
```


**Coercion and 3 > 2 > 1**
Explanation: When JavaScript compares 3 > 2 > 1, it performs comparisons step by step.

Code Example:

```javascript
console.log(3 > 2 > 1);  // Outputs: false
// Explanation: 3 > 2 evaluates to true (which is 1 in JS), then 1 > 1 is false
```

Convert Object to Primitive (valueOf/Symbol.toPrimitive)
Explanation: Objects can define how they convert to primitive values using valueOf or Symbol.toPrimitive.

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

Rest Operator

The rest operator (...) allows you to collect all remaining elements into an array or object. It is commonly used in function parameters and object/array destructuring.

**Example 1**: Rest in Function Parameters
The rest operator gathers all remaining arguments into an array.

```javascript
// In this example, ...numbers collects all the arguments into an array, which can then be processed using reduce().
function sum(...numbers) {
  return numbers.reduce((acc, num) => acc + num, 0);
}

console.log(sum(1, 2, 3, 4));  // Outputs: 10
```

**Example 2**: Rest in Array Destructuring
You can use the rest operator to gather the "rest" of the elements in an array during destructuring.

```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];
console.log(first);  // Outputs: 1
console.log(rest);  // Outputs: [2, 3, 4, 5]

```

Spread Operator

Explanation:
The spread operator (...) allows you to expand an array or object. It is useful for copying arrays/objects and merging them.

**Example 1: Spread in Arrays**

The spread operator can be used to spread elements of one array into another.

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log(combined);  // Outputs: [1, 2, 3, 4, 5, 6]
```

In this example, the contents of arr1 and arr2 are expanded and combined into a new array.

**Example 2: Spread in Objects**
```javascript
const obj1 = { name: "Alice", age: 25 };
const obj2 = { country: "USA" };
const mergedObj = { ...obj1, ...obj2 };
console.log(mergedObj);  // Outputs: { name: "Alice", age: 25, country: "USA" }
```

**Nullish Coalescing Operator (??)**
The nullish coalescing operator (??) returns the right-hand operand when the left-hand operand is null or undefined, but not when it is a falsy value like 0, false, or '' (empty string). This is useful when you want to provide a default value only if the value is null or undefined.

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

### **Scope:**  

**Local Scope:**  
Variables declared within a function are local to that function.

```javascript
function localScope() {
  var localVar = "I'm local!";
  console.log(localVar);  // Outputs: I'm local
}
localScope();
console.log(window.localVar);  // Outputs: undefined
```
**Global Scope:**  
Variables declared outside any function become properties of the global object (`window` in browsers).

```javascript
var globalVar = "I'm global!";
console.log(window.globalVar);  // Outputs: I'm global
```

**Block Scope:**  
Variables declared outside any function become properties of the global object (`window` in browsers).

```javascript
if () {
    // block scpoe
}
```

**Context (`this`):**  
The value of `this` depends on how a function is called.

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

**Closures (Gotchas with Loops):**  
Closures remember the environment in which they were created, leading to unexpected behavior in loops.

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

**Private Fields with Closures**
Explanation: Closures can emulate private fields in JavaScript.


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

#### **Hoisting**

Explanation: Variable and function declarations are moved (hoisted) to the top of their scope, but assignments are not.

```javascript
console.log(a);  // Outputs: undefined
var a = 10;

function sayHello() {
  console.log("Hello");
}
```

### **Functional Programming in JavaScript**

**filter, map, reduce, flat, flatMap**

**`filter`:**  
Creates a new array with all elements that pass the test.

```javascript
const nums = [1, 2, 3, 4, 5];
const evens = nums.filter(n => n % 2 === 0);
console.log(evens);  // Outputs: [2, 4]
```

**`map`:**  
Transforms each element in an array.

```javascript
const doubled = nums.map(n => n * 2);
console.log(doubled);  // Outputs: [2, 4, 6, 8, 10]
```

**`reduce`:**  
Reduces the array to a single value.

```javascript
const sum = nums.reduce((acc, curr) => acc + curr, 0);
console.log(sum);  // Outputs: 15
```

**`flat`:**  
Flattens nested arrays.

```javascript
const nested = [1, [2, 3], [4, [5]]];
console.log(nested.flat(2));  // Outputs: [1, 2, 3, 4, 5]
```

**`flatMap`:**  
Maps and flattens the array.

```javascript
const nested = [1, [2, 3], [4, [5]]];
console.log(nested.flat(2));  // Outputs: [1, 2, 3, 4, 5]
```

```javascript
const nested = [1, [2, 3], [4, [5]]];
console.log(nested.flat(2));  // Outputs: [1, 2, 3, 4, 5]
```

```javascript
const flatMapped = nums.flatMap(n => [n, n * 2]);
console.log(flatMapped);  // Outputs: [1, 2, 2, 4, 3, 6, 4, 8, 5, 10]
```
Function Composition (Using Lodash):
```javascript
const compose = (f, g) => x => f(g(x));

const add1 = x => x + 1;
const double = x => x * 2;

const add1ThenDouble = compose(double, add1);
console.log(add1ThenDouble(5));  // Outputs: 12
```
Function Pipelining
```javascript
const add = x => x + 1;
const double = x => x * 2;
const compose = (...fs) => (x) => fs.reduce(f => f(a), x)
const composed = compose(add, double);  // Using lodash's flow for composition
console.log(composed(3));  // Outputs: 8 (3 + 1 = 4, then 4 * 2 = 8)
```
Using Currying to Create Reusable Functions
```javascript
const multiply = a => b => a * b;

const double = multiply(2);
const triple = multiply(3);

console.log(double(5));  // Outputs: 10
console.log(triple(5));  // Outputs: 15

```
Partial Application
```javascript
function multiply(a, b, c) {
  return a * b * c;
}

const partiallyAppliedMultiply = multiply.bind(null, 2);
console.log(partiallyAppliedMultiply(3, 4));  // Outputs: 24

```
Memoization of a Function
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

Using a Higher-Order Function

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

**Event Loop, setImmediate, Microtasks**

```javascript
setImmediate(() => console.log('setImmediate'));
setTimeout(() => console.log('setTimeout'), 0);
Promise.resolve().then(() => console.log('Promise microtask'));
console.log('Sync log');

// Outputs: Sync log, Promise microtask, setTimeout, setImmediate
```

**try-catch-finally**

The try block allows you to test code for errors, catch is used to handle the error, and finally executes after the try-catch, regardless of whether an error occurred or not.
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
Explanation:
If an error occurs inside the try block, the catch block is executed. The catch block gets an error object that contains information about the error.
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

**finally Block Always Executes**

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
Returning Values from try, catch, and finally

You can return values from try, catch, or finally, but finally will override the return values from try or catch if it also contains a return statement.

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
You can nest try-catch-finally blocks inside each other for more granular error handling.

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


