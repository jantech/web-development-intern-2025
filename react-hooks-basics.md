# 🪝 React Hooks Guide

## 🔹 1. `useState` – For Local Component State

### ✅ Use Case: Counter or Form Input

```tsx
import { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState<number>(0);

  return (
    <div>
      <p>You clicked {count} times.</p>
      <button onClick={() => setCount(count + 1)}>Click Me</button>
    </div>
  );
};
```

### 💡 Explanation:

* `useState<number>(0)` creates a state variable `count`, starting at 0.
* `setCount` updates the value.
* The component re-renders every time the state changes.

---

## 🔹 2. `useEffect` – For Side Effects

### ✅ Use Case: Fetch data when component loads

```tsx
import { useEffect, useState } from 'react';

type User = {
  id: number;
  name: string;
};

const UserProfile = () => {
  const [user, setUser] = useState<User | null>(null);

  useEffect(() => {
    // Simulated fetch
    setTimeout(() => {
      setUser({ id: 1, name: 'Alice' });
    }, 1000);
  }, []); // Empty array = run once after mount

  return <div>{user ? `Hello, ${user.name}` : 'Loading user...'}</div>;
};
```

### 💡 Explanation:

* `useEffect` runs after the component is rendered.
* Great for API calls, subscriptions, etc.
* The empty dependency array `[]` means it runs only once (on mount).

---

## 🔹 3. `useRef` – For DOM Access or Mutable Values

### ✅ Use Case: Focus an input field

```tsx
import { useRef } from 'react';

const FocusInput = () => {
  const inputRef = useRef<HTMLInputElement>(null);

  const focusInput = () => {
    inputRef.current?.focus();
  };

  return (
    <>
      <input ref={inputRef} placeholder="Type here..." />
      <button onClick={focusInput}>Focus</button>
    </>
  );
};
```

### 💡 Explanation:

* `useRef` gives access to DOM nodes (`input` in this case).
* It doesn’t cause re-renders when changed.
* Commonly used for animations, input focus, or saving values.

---

## 🔹 4. `useContext` – For Global State (like theme or auth)

### ✅ Use Case: Access global theme

```tsx
// ThemeContext.tsx
import { createContext, useContext } from 'react';

export const ThemeContext = createContext<'light' | 'dark'>('light');
export const useTheme = () => useContext(ThemeContext);
```

```tsx
// App.tsx
import { ThemeContext } from './ThemeContext';
import Page from './Page';

const App = () => (
  <ThemeContext.Provider value="dark">
    <Page />
  </ThemeContext.Provider>
);
```

```tsx
// Page.tsx
import { useTheme } from './ThemeContext';

const Page = () => {
  const theme = useTheme();
  return <div>Current Theme: {theme}</div>;
};
```

### 💡 Explanation:

* `useContext` lets you read values from a provider.
* Used for global things like themes, language, authentication state.

---

## 🔹 5. `useReducer` – For Complex State Logic

### ✅ Use Case: Counter with multiple actions

```tsx
import { useReducer } from 'react';

type State = { count: number };
type Action = { type: 'increment' } | { type: 'decrement' };

const reducer = (state: State, action: Action): State => {
  switch (action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'decrement': return { count: state.count - 1 };
    default: return state;
  }
};

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </>
  );
};
```

### 💡 Explanation:

* Use `useReducer` when state logic is too complex for `useState`.
* Works like Redux: a reducer receives actions and updates the state.

---

## 🔹 6. `useMemo` – For Expensive Calculations

### ✅ Use Case: Optimize slow calculation

```tsx
import { useMemo, useState } from 'react';

const slowFunction = (num: number) => {
  console.log('Calculating...');
  return num * 2;
};

const Calculator = () => {
  const [number, setNumber] = useState<number>(1);

  const result = useMemo(() => slowFunction(number), [number]);

  return (
    <div>
      <input
        type="number"
        value={number}
        onChange={(e) => setNumber(+e.target.value)}
      />
      <p>Result: {result}</p>
    </div>
  );
};
```

### 💡 Explanation:

* `useMemo` caches the result of a function unless dependencies change.
* Great for expensive computations to avoid re-running every render.

---

## 🔹 7. `useCallback` – For Stable Function References

### ✅ Use Case: Avoid re-rendering child components

```tsx
import { useCallback, useState } from 'react';

const Button = ({ onClick }: { onClick: () => void }) => {
  console.log('Button rendered');
  return <button onClick={onClick}>Click</button>;
};

const App = () => {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount((c) => c + 1);
  }, []);

  return (
    <>
      <p>Count: {count}</p>
      <Button onClick={handleClick} />
    </>
  );
};
```

### 💡 Explanation:

* `useCallback` returns a memoized version of the function.
* Prevents child components from re-rendering unless dependencies change.

---

## 🧠 Summary Table

| Hook          | Purpose                              | Real-World Example                  |
| ------------- | ------------------------------------ | ----------------------------------- |
| `useState`    | Store local state                    | Count clicks, form input            |
| `useEffect`   | Handle side effects                  | Fetch user data on mount            |
| `useRef`      | Reference DOM or store mutable value | Focus input, persist previous state |
| `useContext`  | Share global data across components  | Theme, auth info                    |
| `useReducer`  | Handle complex state logic           | Counter with actions                |
| `useMemo`     | Optimize calculations                | Slow math based on input            |
| `useCallback` | Memoize function for performance     | Prevent re-render of child props    |

---
