# A roadmap to learn **React.js** from scratch using **Vite + TypeScript (TSX)**

### **A. Basics**

* What is React?
* What is Vite?
* What is TypeScript?
* JSX vs TSX
* Folder/project structure

### **B. Core React Concepts**

* Components (Function-based)
* Props
* State (`useState`)
* Event handling
* Conditional rendering
* Lists and keys
* `useEffect` (side effects)
* Component lifecycle (via hooks)

### **C. Intermediate**

* Forms and controlled components
* Lifting state up
* Component composition
* Custom hooks
* Context API (for global state)

### **D. Advanced**

* Routing with `react-router-dom`
* Fetching API data (`fetch` / `axios`)
* Error boundaries
* Code splitting / lazy loading
* Deployment

---

## ✅ 2. **Setup Your Project (Vite + React + TypeScript)**

### 🔧 Step-by-step:

```bash
# 1. Create the app
npm create vite@latest my-react-app -- --template react-ts

# 2. Move into the directory
cd my-react-app

# 3. Install dependencies
npm install

# 4. Start the development server
npm run dev
```

✅ You’ll see the default Vite + React + TS app running at `http://localhost:5173`

---

## ✅ 3. **Basic File Structure**

```
my-react-app/
├── src/
│   ├── App.tsx
│   ├── main.tsx
│   └── components/    ← create this folder
├── index.html
├── tsconfig.json
├── vite.config.ts
└── package.json
```

---

## ✅ 4. **Write Your First Component**

### 📁 `src/components/Hello.tsx`

```tsx
type HelloProps = {
  name: string;
};

export function Hello({ name }: HelloProps) {
  return <h1>Hello, {name}!</h1>;
}
```

### 📁 `src/App.tsx`

```tsx
import { useState } from 'react';
import { Hello } from './components/Hello';

function App() {
  const [name, setName] = useState("React Learner");

  return (
    <div>
      <Hello name={name} />
    </div>
  );
}

export default App;
```

🧠 **Explanation**:

* `useState` hook is used to manage state.
* `Hello` is a reusable component that takes `name` as a prop.

---

## ✅ 5. **Handling Events**

### 📁 `src/App.tsx` (Add a button)

```tsx
function App() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Clicked {count} times</button>
    </div>
  );
}
```

🧠 **Explanation**:

* `onClick` is an event handler.
* `setCount` updates the state when clicked.

---

## ✅ 6. **Conditional Rendering**

```tsx
{count > 5 ? <p>You're clicking a lot!</p> : <p>Keep clicking!</p>}
```

---

## ✅ 7. **List Rendering**

```tsx
const items = ['Apple', 'Banana', 'Cherry'];

<ul>
  {items.map((item, index) => (
    <li key={index}>{item}</li>
  ))}
</ul>
```

---

## ✅ 8. **Side Effects: useEffect**

```tsx
import { useEffect } from 'react';

useEffect(() => {
  console.log("Component mounted or count changed");
}, [count]);
```

---

## ✅ 9. **Forms + Controlled Inputs**

```tsx
const [text, setText] = useState("");

<input value={text} onChange={(e) => setText(e.target.value)} />
<p>You typed: {text}</p>
```

---

## ✅ 10. **React Router Setup**

```bash
npm install react-router-dom
```

### 📁 `src/main.tsx`

```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

### 📁 `src/App.tsx`

```tsx
import { Routes, Route, Link } from 'react-router-dom';
import Home from './pages/Home';
import About from './pages/About';

function App() {
  return (
    <>
      <nav>
        <Link to="/">Home</Link> | <Link to="/about">About</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </>
  );
}

export default App;
```

---

## ✅ 11. **Fetch API Data**

```tsx
useEffect(() => {
  fetch('https://jsonplaceholder.typicode.com/posts')
    .then(res => res.json())
    .then(data => setPosts(data));
}, []);
```

---

## ✅ 12. **Context API (Global State)**

Create `context/UserContext.tsx`:

```tsx
import { createContext, useState } from 'react';

export const UserContext = createContext(null);

export function UserProvider({ children }: { children: React.ReactNode }) {
  const [user, setUser] = useState("React User");

  return (
    <UserContext.Provider value={{ user, setUser }}>
      {children}
    </UserContext.Provider>
  );
}
```

Wrap `<App />` with `<UserProvider />`.

---

## ✅ 13. **Deployment (Vercel)**

### 📦 Build for Production:

```bash
npm run build
```

Upload `dist/` folder to:

* [vercel.com](https://vercel.com)

---


