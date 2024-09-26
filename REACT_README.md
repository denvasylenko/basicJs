# React JS CORE

## Containers and Components (Smart vs Dumb Components)

**Explanation:**

**Explanation:**

`UserContainer` is the **container** (smart component) that handles the data-fetching logic and manages state. `UserList` is the **dumb component** that only renders the list of users. This separation makes the components more reusable and maintainable.

**Good Example: Separation of Logic and UI**

```js
// Good Example: Separation of concerns in functional components

// Dumb Component: Renders the user list
function UserList({ users }) {
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

// Smart Component: Handles fetching and passing data
function UserContainer() {
  const [users, setUsers] = React.useState([]);

  React.useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/users')
      .then(response => response.json())
      .then(data => setUsers(data));
  }, []);

  return <UserList users={users} />;
}

export default UserContainer;
```

**Bad Example: Mixing Logic and Presentation**

**Explanation:**

In the bad example, the data-fetching logic and the rendering logic are mixed together in one component (`UserComponent`). This makes the component harder to maintain, test, and reuse. Separation of concerns is crucial for scalability.

```js
function UserComponent() {
  const [users, setUsers] = React.useState([]);

  React.useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/users')
      .then(response => response.json())
      .then(data => setUsers(data));
  }, []);

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

export default UserComponent;
```

## Context API

**Good Example: Using Context to Manage Theme**

**Explanation:**

the **Context API** is used to provide and consume the `theme` value throughout the component tree without manually passing props. The useContext hook inside `ThemedButton` accesses the current `theme` and allows the button to toggle it.


```js
import React, { createContext, useContext, useState } from 'react';

const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

function ThemedButton() {
  const { theme, setTheme } = useContext(ThemeContext);
  
  return (
    <button
      style={{
        background: theme === 'light' ? '#fff' : '#333',
        color: theme === 'light' ? '#000' : '#fff',
      }}
      onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}
    >
      Toggle Theme
    </button>
  );
}

function App() {
  return (
    <ThemeProvider>
      <ThemedButton />
    </ThemeProvider>
  );
}

export default App;
```

**Bad Example: Manually Passing Theme Through Props**

**Explanation:**

The theme is passed manually through props, making it less scalable and harder to manage in larger applications. This approach leads to prop drilling, where props are passed through multiple layers, even if intermediate components don't need them.

```js
function ThemedButton({ theme, setTheme }) {
  return (
    <button
      style={{
        background: theme === 'light' ? '#fff' : '#333',
        color: theme === 'light' ? '#000' : '#fff',
      }}
      onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}
    >
      Toggle Theme
    </button>
  );
}

function App() {
  const [theme, setTheme] = React.useState('light');

  return <ThemedButton theme={theme} setTheme={setTheme} />;
}

export default App;

```

## Higher-Order Components (HOC)

**Good Example: HOC for Logging Props**

**Explanation:**

Here, **withLogger** is a higher-order component (HOC) that wraps **MyComponent**. The HOC adds additional behavior to log the props whenever they change without modifying **MyComponent** itself. This allows reusing the logging behavior across different components, following the DRY (Don't Repeat Yourself) principle.

```js
import React, { useEffect } from 'react';

function withLogger(WrappedComponent) {
  return function LoggerComponent(props) {
    useEffect(() => {
      console.log('Props:', props);
    }, [props]);

    return <WrappedComponent {...props} />;
  };
}

function MyComponent({ value }) {
  return <div>Component with props: {value}</div>;
}

export default withLogger(MyComponent);
```

**Bad Example: Duplicating Code Across Components**

**Explanation:**

In the bad example, the **useEffect** logging logic is repeated inside both **MyComponent** and **AnotherComponent**. This results in redundant code, making it harder to maintain and update the logging logic if needed.

```js
import React, { useEffect } from 'react';

function MyComponent({ value }) {
  useEffect(() => {
    console.log('Props:', { value });
  }, [value]);

  return <div>Component with props: {value}</div>;
}

function AnotherComponent({ value }) {
  useEffect(() => {
    console.log('Props:', { value });
  }, [value]);

  return <div>Another component with props: {value}</div>;
}

export default MyComponent;
```

## Custom Hooks for Reusability

Custom hooks allow you to extract and reuse stateful logic across multiple components. They are simply functions that use React hooks.

**Example: Custom Hook for Fetching Data**

```js
// Good Example: Custom Hook
import React, { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetch(url)
      .then((response) => response.json())
      .then((data) => {
        setData(data);
        setLoading(false);
      })
      .catch((error) => {
        setError(error);
        setLoading(false);
      });
  }, [url]);

  return { data, loading, error };
}

// Component using the custom hook
function UserList() {
  const { data, loading, error } = useFetch('https://jsonplaceholder.typicode.com/users');

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error loading data</p>;

  return (
    <ul>
      {data.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

export default UserList;
```

In this example:

- The `useFetch` hook encapsulates the logic for fetching data, managing loading state, and handling errors.
- `UserList` uses `useFetch` to fetch and display users, keeping the component clean and focused on rendering.

**Bad Example:**

```js
// Bad Example: Repeating logic inside components
function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/users')
      .then((response) => response.json())
      .then((data) => {
        setUsers(data);
        setLoading(false);
      });
  }, []);

  if (loading) return <p>Loading...</p>;

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```
Here, the fetch logic is tightly coupled with the component itself, making it harder to reuse or test.

## Memoization for Performance Optimization

React provides `React.memo` to prevent unnecessary re-renders of functional components if their props haven't changed. This is crucial for performance in complex apps.

**Example: Optimizing with React.memo**

```js
// Good Example: Using React.memo
import React, { memo } from 'react';

const ExpensiveComponent = memo(({ value }) => {
  console.log("Expensive component rendered!");
  return <div>Value: {value}</div>;
});

function ParentComponent() {
  const [count, setCount] = useState(0);
  const [value, setValue] = useState("Initial");

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
      <ExpensiveComponent value={value} />
    </div>
  );
}

export default ParentComponent;
```

- `ExpensiveComponent` will only re-render if the value prop changes, reducing unnecessary renders.
- `React.memo` wraps the component, enabling shallow comparison of props.

**Bad Example:**

```js
// Bad Example: No memoization
function ExpensiveComponent({ value }) {
  console.log("Expensive component rendered!");
  return <div>Value: {value}</div>;
}
```

Without `React.memo`, `ExpensiveComponent` will re-render every time `ParentComponent` re-renders, even if value hasn't changed.

## `useCallback` and `useMemo` for Performance

React’s useCallback and useMemo hooks help optimize performance by caching functions and computed values, respectively.

**Example: Using `useCallback` and `useMemo`**

```js
// Good Example: useCallback and useMemo
import React, { useState, useCallback, useMemo } from 'react';

function ExpensiveCalculation({ number }) {
  const calculatedValue = useMemo(() => {
    console.log("Calculating...");
    return number * 2;
  }, [number]);

  return <div>Calculated Value: {calculatedValue}</div>;
}

function ParentComponent() {
  const [count, setCount] = useState(0);
  const [number, setNumber] = useState(5);

  const increment = useCallback(() => {
    setCount(count + 1);
  }, [count]);

  return (
    <div>
      <button onClick={increment}>Increment Count</button>
      <ExpensiveCalculation number={number} />
    </div>
  );
}

export default ParentComponent;
```

- `useMemo` caches the result of expensive calculations. In this case, the multiplication is only recalculated when number changes.
- `useCallback` prevents unnecessary re-creation of the increment function unless count changes, reducing unnecessary renders of child components that rely on this callback.

**Bad Example:**

```js
// Bad Example: No use of useMemo or useCallback
function ExpensiveCalculation({ number }) {
  const calculatedValue = number * 2; // recalculates every render
  return <div>Calculated Value: {calculatedValue}</div>;
}
```

Without `useMemo`, the calculation is done on every render, regardless of whether the input changed. Similarly, without `useCallback`, functions are re-created on every render.

## `useReducer` for Complex State Management

When you have more complex state logic or want to manage multiple state variables, useReducer provides a powerful alternative to `useState`.

**Example: `useReducer` for Managing Complex State**

```js
// Good Example: useReducer
import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
    </div>
  );
}

export default Counter;
```

- `useReducer` is ideal for managing more complex state logic, such as toggling between multiple states or handling different types of actions.
- It provides a clean, centralized way to manage state transitions, avoiding clutter with multiple `useState` calls.

**Bad Example:**

```js
// Bad Example: Managing complex state with multiple useState hooks
function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
}
```

Using multiple `useState` hooks for more complex state logic can become cumbersome and difficult to maintain as the component grows.


## React Hook Rules

**Rule 1: Only Call Hooks at the Top Level**

- **Explanation**: Hooks should be called only at the top level of your component, not inside loops, conditions, or nested functions.
- **Why**: Hooks rely on the order in which they are called. If you conditionally or dynamically call hooks, React can’t properly track the hook's state between renders, which can cause inconsistent or incorrect behavior.

**Good Example:**
```js
function MyComponent() {
  const [count, setCount] = React.useState(0); // Called at the top level

  return <div>{count}</div>;
}
```

**Bad Example (Hook inside condition):**
```js
function MyComponent() {
  if (someCondition) {
    const [count, setCount] = React.useState(0); // This is incorrect!
  }

  return <div>{count}</div>;
}
```

- **Why this is bad**: The useState hook is conditionally called, which breaks the rules. The hook order may change between renders, leading to incorrect state tracking.

**Rule 2: Only Call Hooks from React Functions**

- **Explanation**: Hooks should be called only from within React functional components or custom hooks, not regular JavaScript functions or class methods.
- **Why**: Hooks are designed to work in React's rendering context, which doesn't apply to ordinary functions.

**Good Example:**

```js
function MyComponent() {
  const [count, setCount] = React.useState(0); // Called inside a functional component

  return <div>{count}</div>;
}
```

**Bad Example (Hook outside component):**

```js
function someRegularFunction() {
  const [count, setCount] = React.useState(0); // This is incorrect!
}
```

- **Why this is bad**: Hooks are meant to be used within React's rendering flow, not in arbitrary functions. Using them outside components will cause errors.

### Common Pitfalls and How to Avoid Them

1. **Forgetting to Add Dependencies in `useEffect`
The `useEffect` hook can be tricky because it depends on an array of dependencies to determine when the effect should run.**

**Example of a Mistake:**

```js
function MyComponent({ count }) {
  React.useEffect(() => {
    console.log("Count changed:", count);
  }); // Dependency array is missing!
  
  return <div>{count}</div>;
}
```

**Pitfall:**

If you omit the dependency array, the effect will run after every render, potentially causing unnecessary performance issues or even infinite loops.

**Solution: Specify Dependencies**

```js
function MyComponent({ count }) {
  React.useEffect(() => {
    console.log("Count changed:", count);
  }, [count]); // Specify 'count' as a dependency
  
  return <div>{count}</div>;
}
```

- **Why it's correct**: The effect will now run only when the count prop changes, which is the intended behavior.

2. **Incorrectly Using `useEffect` for Data Fetching**

Many developers initially try to fetch data inside a `useEffect` hook but forget to clean up or handle race conditions.

**Bad Example (Uncleaned effect):**

```js
function MyComponent() {
  React.useEffect(() => {
    fetch("https://api.example.com/data")
      .then(response => response.json())
      .then(data => console.log(data));

    // No cleanup, potential memory leaks
  }, []);
}
```

**Pitfall**: 

If the component unmounts before the fetch completes, the data may arrive too late, or memory leaks could occur if listeners (like event listeners) are attached.

**Solution: Clean Up in `useEffect`**

```js
function MyComponent() {
  React.useEffect(() => {
    let isMounted = true;

    fetch("https://api.example.com/data")
      .then(response => response.json())
      .then(data => {
        if (isMounted) {
          console.log(data);
        }
      });

    return () => {
      isMounted = false; // Cleanup
    };
  }, []);
}
```

Another good example clean something

```js
import React, { useEffect, useState } from 'react';

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds((prev) => prev + 1);
    }, 1000);

    // Cleanup function to clear the interval when the component unmounts
    return () => clearInterval(interval);
  }, []);

  return <div>{seconds} seconds have passed</div>;
}

export default Timer;
```

Notify user

```js
import React, { useEffect, useState } from 'react';

function Chat({ isChatOpen }) {
  useEffect(() => {
    if (isChatOpen) {
      console.log("Chat is opened");

      return () => {
        console.log("Chat is closed");
      };
    }
  }, [isChatOpen]); // Cleanup runs only when `isChatOpen` changes to false
}

export default Chat;
```

**Why it's correct**: The cleanup function prevents the component from updating its state or performing side effects after it has unmounted.


3. **Overuse of `useEffect`**

Sometimes developers use `useEffect` to synchronize state, when in fact they could have done it directly with a state update.

**Bad Example:**

```js
function MyComponent({ value }) {
  const [state, setState] = React.useState(value);

  React.useEffect(() => {
    setState(value); // Updating state in effect
  }, [value]);

  return <div>{state}</div>;
}
```

- **Pitfall**: Using `useEffect` here is unnecessary because state can be derived directly from value.

Solution: Use `useState` Directly

```js
function MyComponent({ value }) {
  const [state, setState] = React.useState(value);

  React.useMemo(() => {
    setState(value); // Memoize it to avoid re-render
  }, [value]);

  return <div>{state}</div>;
}
```
## React Lifecycle Summary Using Hooks

| **Lifecycle Method**       | **Functional Component Equivalent with Hooks**                        |
|----------------------------|----------------------------------------------------------------------|
| `componentDidMount`         | `useEffect(() => { ... }, [])` (runs once on mount)                   |
| `componentDidUpdate`        | `useEffect(() => { ... }, [dependency])` (runs after `dependency` updates)         |
| `componentWillUnmount`      | `useEffect(() => { return () => { ... } }, [])` (cleanup function on unmount) |


## Handle error and not break UI 

### 1. Error Boundaries

**Error Boundaries** are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of crashing the entire component tree.

Error boundaries **only catch errors** in the following cases:

- Rendering
- Lifecycle methods
- Constructors of the whole tree below them

They **don’t catch** errors inside event handlers or asynchronous code like promises or setTimeout.

Example of an Error Boundary:

**Example of an Error Boundary:**

```js
import React from 'react';

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render shows the fallback UI.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // You can also log the error to an error reporting service
    console.error("Error:", error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // Fallback UI when an error occurs
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```

**Explanation:**

The ErrorBoundary component will catch any errors in the component tree below it and display a fallback UI ("Something went wrong") instead of crashing the app.

**Usage:**

```js
function BuggyComponent() {
  throw new Error("I crashed!");
}

function App() {
  return (
    <div>
      <ErrorBoundary>
        <BuggyComponent />
      </ErrorBoundary>
    </div>
  );
}

export default App;
```

In this example, the `ErrorBoundary` catches the error thrown in `BuggyComponent` and displays the fallback UI, preventing the entire app from crashing.

### 2. Error Handling in Event Handlers

Errors inside event handlers don’t propagate to the error boundary, so you need to handle them explicitly.

**Example of Event Error Handling:**

```js
function MyComponent() {
  const handleClick = () => {
    try {
      // Some code that might throw an error
      throw new Error("An error occurred!");
    } catch (error) {
      console.error("Caught an error in the event handler:", error);
      // Handle the error gracefully
    }
  };

  return <button onClick={handleClick}>Click me</button>;
}
```

**Explanation:**

Since React event handlers don’t throw errors up the component tree, you should use `try-catch` blocks in event handlers to handle errors and ensure the UI remains functional.


## Reconciliation and Re-Rendering


**Reconciliation** is the algorithm React uses to decide how to update the UI in response to changes in state or props. When a component’s state or props change, React needs to determine how to efficiently update the DOM to reflect those changes.

**How Reconciliation Works:**

- React creates a virtual DOM (a lightweight representation of the actual DOM).
- When there are changes, React generates a new virtual DOM and compares it with the previous version. This process is called diffing.
- React then updates the actual DOM only in places where the virtual DOM differs. This minimizes performance impact by reducing unnecessary updates.

**Example:**

```js
function MyComponent() {
  const [count, setCount] = React.useState(0);

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

In this example, when the `count` state changes, React generates a new virtual DOM and only updates the part of the real DOM where the `<h1>` element's value changes, rather than re-rendering the entire component tree.

### Key Props

In React, **keys** are special props used to uniquely identify elements in a list. They help React optimize updates during reconciliation by detecting which items have changed, been added, or removed.

**Why Are Keys Important?**

When rendering a list of items, React needs to keep track of each item between renders. If items are re-ordered, added, or removed, keys help React maintain the correct associations between items and DOM nodes. Without keys, React may update the DOM inefficiently, causing incorrect behavior, such as re-rendering the wrong components or losing component state.


```js
function ItemList({ items }) {
  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```

**Explanation:**

Each `<li>` element is given a key prop, which in this case is `item.id`. This ensures that when the list of items changes, React can efficiently determine which items have changed, and update only the affected elements.

**Why You Shouldn’t Use Index as a Key:**

Using the array index as a key can lead to bugs, especially when the order of items changes, or items are added or removed. React may incorrectly associate DOM elements with the wrong data, causing unexpected behavior (e.g., losing focus or resetting input fields).

**Bad Example (Using Index as Key):**

```js
function ItemList({ items }) {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{item.name}</li>
      ))}
    </ul>
  );
}
```

**Problem:**

If the order of the items changes, React might not update the DOM correctly, leading to performance issues or incorrect rendering.

### Zombie Children

**Zombie children** refer to situations where a child component or some asynchronous logic continues to execute after the parent component has unmounted. This can lead to memory leaks, state updates on unmounted components, or unexpected behavior in the UI.

**Common Cause of Zombie Children**

The most common cause of zombie children is when you initiate an asynchronous operation (e.g., a network request or a timer) in a component, and the component unmounts before the operation completes. If the component still tries to update state or the DOM after it has been unmounted, React will throw a warning like:

*Warning: Can't perform a React state update on an unmounted component.*

**Example of Zombie Child Problem (Bad Example):**

```js
import React, { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    // Fetch user data from API
    fetch(`/api/users/${userId}`)
      .then((response) => response.json())
      .then((data) => setUser(data));

    // No cleanup when component unmounts
  }, [userId]);

  return <div>{user ? user.name : "Loading..."}</div>;
}
```

**Problem:** 

If UserProfile unmounts before the API call completes, the component will try to update state with setUser after it has been unmounted, leading to a "zombie" state update. This can cause memory leaks and trigger React's "Can't perform a React state update on an unmounted component" warning.

**Solution: Add Cleanup to Avoid Zombie Children (Good Example):**

```js
import React, { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    let isMounted = true;

    // Fetch user data from API
    fetch(`/api/users/${userId}`)
      .then((response) => response.json())
      .then((data) => {
        if (isMounted) {
          setUser(data);
        }
      });

    // Cleanup function to avoid setting state if the component unmounts
    return () => {
      isMounted = false;
    };
  }, [userId]);

  return <div>{user ? user.name : "Loading..."}</div>;
}
```

**Solution:** 

We track whether the component is still mounted using the isMounted variable. If the component is unmounted before the API call finishes, we avoid updating the state. This prevents the zombie child issue.

### Stale Closures

In React, stale closures occur when a function within a component holds onto an outdated version of a variable due to closure. This can lead to unexpected behavior, especially when dealing with asynchronous operations or event handlers.

**Example of a Stale Closure Problem (Bad Example):**

```js
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCount(count + 1); // `count` is stale here
    }, 1000);

    return () => clearInterval(interval);
  }, []); // Empty dependency array, so `count` is stuck at initial value

  return <div>Count: {count}</div>;
}
```

**Problem:** 

The count inside setInterval is captured when the effect first runs. Since the effect has no dependencies (due to the empty dependency array), the value of count remains stuck at 0, leading to a stale closure.

**Solution: Use Functional State Update to Avoid Stale Closure (Good Example):**

```js
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCount((prevCount) => prevCount + 1); // Correctly uses the latest state
    }, 1000);

    return () => clearInterval(interval);
  }, []); // No need to include `count` in dependency array

  return <div>Count: {count}</div>;
}
```

**Solution:**

Use the functional form of setState, which ensures that the latest state (prevCount) is used. This avoids the stale closure issue.

### Memory Leaks from Not Cleaning Up Effects

If you create side effects like timers, subscriptions, or event listeners inside useEffect, you should always return a cleanup function. Failing to do so can cause memory leaks, especially if the component unmounts but the effect keeps running.

**Example of a Memory Leak (Bad Example):**

```js
function MyComponent() {
  useEffect(() => {
    const handleScroll = () => {
      console.log('User scrolled');
    };

    window.addEventListener('scroll', handleScroll);

    // No cleanup function, event listener stays after component unmounts
  }, []);

  return <div>Scroll to see effect</div>;
}
```

**Problem:** 

The event listener is not removed when the component unmounts, potentially causing memory leaks and unwanted behavior.

**Solution: Cleanup with `useEffect` (Good Example):**

```js
function MyComponent() {
  useEffect(() => {
    const handleScroll = () => {
      console.log('User scrolled');
    };

    window.addEventListener('scroll', handleScroll);

    // Cleanup function to remove the event listener when component unmounts
    return () => {
      window.removeEventListener('scroll', handleScroll);
    };
  }, []);

  return <div>Scroll to see effect</div>;
}
```
