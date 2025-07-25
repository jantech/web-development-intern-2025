React.js is a **JavaScript library for building user interfaces**, especially for single-page applications where you need a dynamic and fast user experience. Here's a breakdown of the **basics** of React.js to get you started:

---

## ⚛️ 1. What is React?

* Created by **Facebook**.
* Allows developers to **build reusable UI components**.
* Uses a **component-based** architecture.
* Works with a **virtual DOM** to efficiently update the UI.

---

## 📦 2. Setting Up React

You can start a React project using:

### ✅ Option 1: Create React App (easy setup)

```bash
npx create-react-app my-app
cd my-app
npm start
```

### ✅ Option 2: Vite (faster, modern)

```bash
npm create vite@latest my-app --template react
cd my-app
npm install
npm run dev
```

---

## 🧱 3. React Component Basics

### Function Component

```jsx
function Welcome() {
  return <h1>Hello, React!</h1>;
}
```

### JSX (JavaScript + XML)

* You write HTML-like syntax in JavaScript:

```jsx
const element = <h1>Hello, JSX!</h1>;
```

---

## 🔄 4. Props (Component Inputs)
props (short for properties) are a way to pass data from a parent component to a child component.

```jsx
function Greet(props) {
  return <h1>Hello, {props.name}!</h1>;
}

<Greet name="Alice" />
```

---

## 🔁 5. State (Internal Component Data)

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

---

## 🎯 6. Event Handling

```jsx
function ClickMe() {
  const handleClick = () => {
    alert("Button clicked!");
  };

  return <button onClick={handleClick}>Click</button>;
}
```

---

## 📂 7. Folder Structure (typical)

```
src/
├── App.js
├── index.js
├── components/
│   └── Header.js
```

---

## 🔄 8. Lifecycle (via Hooks)

* `useEffect()` is like `componentDidMount`, `componentDidUpdate`, `componentWillUnmount`:

```jsx
import { useEffect } from 'react';

useEffect(() => {
  console.log("Component mounted or updated");

  return () => {
    console.log("Cleanup on unmount");
  };
}, []);
```

---

## 🧩 9. Conditional Rendering

```jsx
{isLoggedIn ? <Dashboard /> : <Login />}
```

---

## 🗂️ 10. Lists and Keys

```jsx
const items = ['apple', 'banana', 'cherry'];

<ul>
  {items.map((item, index) => (
    <li key={index}>{item}</li>
  ))}
</ul>
```

---

