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

  const handleSetName = () => {

    console.log(inputRef);
    const inputValue = inputRef.current?.value;

    // add the input validation
    if (inputValue === undefined || inputValue === null || inputValue.trim() === "") {
      console.error("Input value is undefined or null");
      return;
    }

    console.log("User set to: " + inputValue);
    // set the value
    setUser({ name: inputValue || null });

    // Clear the input field after setting the name
    inputRef.current!.value = '';

    // Focus back on the input field
    inputRef.current?.focus(); 
  };

  return (
    <>
      <input ref={inputRef} placeholder="Type here..." />
      <button onClick={handleSetName}>Set Name</button>
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

---

### 🧠 1. **What is `useContext`?**

* `useContext` is a React **hook** that allows you to **read values from a context**.
* It helps **avoid prop drilling** by making data available globally to any component that needs it.

> 💬 Think of it like a global state system, built into React, for static or shared values like **theme**, **language**, **auth state**, etc.

---

### 🏗️ 2. **What is a Context?**

A **Context** is a React object created using `createContext()` that lets you share data **across your component tree**, without passing props manually at every level.

```tsx
import { createContext } from 'react';

const ThemeContext = createContext('light'); // default value
```

---

### 🧩 3. **How `useContext` Works**

`useContext` reads the value from the nearest `<MyContext.Provider>` up the component tree.

```tsx
import { useContext } from 'react';

const value = useContext(ThemeContext);
```

If no Provider is found, it uses the **default value** from `createContext()`.

---

## 🚀 Example: Using `useContext` to Share Theme

### 🎯 Goal: Toggle Light/Dark Theme Using `useContext`

We’ll create a theme system with:

* `ThemeContext` – for global state
* `ThemeProvider` – to wrap your app
* `useTheme` – a custom hook
* A component (`Page`) to toggle the theme

---

## ✅ Step-by-Step Setup

### 📁 Final File Structure:

```
src/
├── context/
│   ├── ThemeContext.tsx     # Context + Provider
│   └── useTheme.ts          # Custom hook
├── App.tsx                  # App root
└── Page.tsx                 # UI using the theme
```

---

### 🔧 Step 1: Create the Theme Context

📄 **`src/context/ThemeContext.tsx`**

```tsx
import { createContext, useEffect, useState, ReactNode } from 'react';

// Define theme type
type Theme = 'light' | 'dark';

// Define context value shape
interface ThemeContextType {
  theme: Theme;
  toggleTheme: () => void;
}

// Create the context with a default value
export const ThemeContext = createContext<ThemeContextType>({
  theme: 'light',
  toggleTheme: () => {},
});

// Create the Provider component
export const ThemeProvider = ({ children }: { children: ReactNode }) => {
  const [theme, setTheme] = useState<Theme>('light');

  // Load from localStorage on first render
  useEffect(() => {
    const storedTheme = localStorage.getItem('theme') as Theme;
    if (storedTheme) setTheme(storedTheme);
  }, []);

  // Save to localStorage whenever theme changes
  useEffect(() => {
    localStorage.setItem('theme', theme);
  }, [theme]);

  // Toggle between light and dark
  const toggleTheme = () => {
    setTheme((prev) => (prev === 'light' ? 'dark' : 'light'));
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};
```

### 🧠 What this does:

* Creates a **ThemeContext** object
* Wraps state logic in a **ThemeProvider**
* Syncs theme with **localStorage**
* Shares `theme` and `toggleTheme()` via context

---

### 🔧 Step 2: Create the Custom Hook

📄 **`src/context/useTheme.ts`**

```tsx
import { useContext } from 'react';
import { ThemeContext } from './ThemeContext';

// Custom hook to use theme
export const useTheme = () => useContext(ThemeContext);
```

### 🧠 Why a separate file?

React Fast Refresh **requires hooks and components in separate files** to work properly in development.

---

### 🔧 Step 3: Wrap Your App with the Provider

📄 **`src/App.tsx`**

```tsx
import { ThemeProvider } from './context/ThemeContext';
import Page from './Page';

const App = () => (
  <ThemeProvider>
    <Page />
  </ThemeProvider>
);

export default App;
```

### 🧠 Why this is needed:

Without wrapping your app with `<ThemeProvider>`, any component that uses `useTheme()` will fall back to the default context value.

---

### 🔧 Step 4: Use the Hook in Your Component

📄 **`src/Page.tsx`**

```tsx
import { useTheme } from './context/useTheme';

const Page = () => {
  const { theme, toggleTheme } = useTheme();

  return (
    <div
      style={{
        backgroundColor: theme === 'dark' ? '#1e1e1e' : '#f5f5f5',
        color: theme === 'dark' ? '#ffffff' : '#000000',
        height: '100vh',
        display: 'flex',
        justifyContent: 'center',
        alignItems: 'center',
        flexDirection: 'column',
        transition: 'all 0.3s ease',
      }}
    >
      <h1>Current Theme: {theme}</h1>
      <button
        onClick={toggleTheme}
        style={{
          marginTop: '1rem',
          padding: '0.5rem 1rem',
          fontSize: '1.2rem',
          border: 'none',
          borderRadius: '8px',
          cursor: 'pointer',
        }}
      >
        {theme === 'light' ? '🌙 Switch to Dark' : '🌞 Switch to Light'}
      </button>
    </div>
  );
};

export default Page;
```

---

## ✅ Summary

| Step | What You Did                       | File               |
| ---- | ---------------------------------- | ------------------ |
| 1️⃣  | Created context + provider         | `ThemeContext.tsx` |
| 2️⃣  | Created a custom hook              | `useTheme.ts`      |
| 3️⃣  | Wrapped app in provider            | `App.tsx`          |
| 4️⃣  | Used theme + toggle in a component | `Page.tsx`         |

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
