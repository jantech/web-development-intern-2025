React.js is a **JavaScript library for building user interfaces**, especially for single-page applications where you need a dynamic and fast user experience. Here's a breakdown of the **basics** of React.js to get you started:

---

## ⚛️ What is React?

* Created by **Facebook**.
* Allows developers to **build reusable UI components**.
* Uses a **component-based** architecture.
* Works with a **virtual DOM** to efficiently update the UI.

---


## 🚀 Step 1: Create the Project

To Create a **React project using Vite with TypeScript (TSX)**

### ✅ Prerequisites

* Node.js installed (LTS version recommended)
* `npm` or `yarn` (comes with Node.js)

### 🔧 Run the following in your terminal:

```bash
npm create vite@latest my-react-app -- --template react-ts
cd my-react-app
npm install
```

> 📝 `react-ts` = React + TypeScript
> 💡 `my-react-app` is your project folder. You can rename it as needed.

---

## 📁 Step 2: Project Structure Overview

```
my-react-app/
│
├── public/            → Static assets like icons/images
├── src/               → Source code
│   ├── App.tsx        → Main App component
│   ├── main.tsx       → Entry point for React
│   └── ...
├── index.html         → Root HTML file
├── vite.config.ts     → Vite configuration
├── tsconfig.json      → TypeScript configuration
└── package.json       → Project dependencies
```

---

## ⚙️ Step 3: Run the App

```bash
npm run dev
```

> 🟢 You’ll see a message like:

```
  VITE v5.x  ready in 500ms
  ➜  Local:   http://localhost:5173/
```

Open that URL in your browser – it’s your live app!

---

## 🧱 Step 4: Understand the Starter Code

### 📄 `main.tsx`

```tsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)
```

### 📄 `App.tsx`

```tsx
import { useState } from 'react'
import './App.css'

function App() {
  const [count, setCount] = useState(0)

  return (
    <div>
      <h1>Hello React + TypeScript!</h1>
      <button onClick={() => setCount(count + 1)}>
        Count is: {count}
      </button>
    </div>
  )
}

export default App
```

### Explanation:

* `useState` is a **React hook** to manage state (like a variable).
* `setCount` updates the value when the button is clicked.
* JSX with TypeScript = `.tsx` file.

---

## ✨ Step 5: Create Your First Component

### 📄 `src/components/Greeting.tsx`

```tsx
type GreetingProps = {
  name: string
}

export default function Greeting({ name }: GreetingProps) {
  return <h2>Hello, {name} 👋</h2>
}
```

### ➕ Use it in `App.tsx`

```tsx
import Greeting from './components/Greeting'

function App() {
  const [count, setCount] = useState(0)

  return (
    <div>
      <Greeting name="Beginner" />
      <h1>Hello React + TypeScript!</h1>
      <button onClick={() => setCount(count + 1)}>
        Count is: {count}
      </button>
    </div>
  )
}
```

---

## 🧼 Step 6: Styling with CSS

Edit `App.css`:

```css
h1 {
  color: royalblue;
}

button {
  padding: 10px;
  font-size: 1rem;
}
```

---

## 🛠 Optional: Use VS Code Extensions

* **ESLint** – for linting
* **Prettier** – for auto formatting
* **React TypeScript Snippets** – for writing TSX quickly

---

## ✅ Summary

You've learned:

* How to set up a React project using Vite + TypeScript
* What each core file does
* How to make a component
* How to use hooks like `useState`

---
