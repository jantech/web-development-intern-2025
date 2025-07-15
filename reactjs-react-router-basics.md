**React Router** is an essential step in building multi-page applications in React.

React Router v6+ (latest version):

---

## 🧭 What is React Router?

React Router lets you:

* Navigate between pages (components)
* Use URLs to show different views
* Handle nested routes, redirects, and more

---

## ⚙️ 1. Installation

Run this in your React project:

```bash
npm install react-router-dom
```

---

## 🗺️ 2. Basic Structure

You'll wrap your app with a router and define routes.

### Example:

```jsx
// App.js
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Home from './Home';
import About from './About';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

---

## 🧱 3. Create Simple Pages (Components)

```jsx
// Home.js
export default function Home() {
  return <h1>Welcome to the Home Page</h1>;
}

// About.js
export default function About() {
  return <h1>About Us</h1>;
}
```

---

## 🔗 4. Navigation (Link vs `<a>`)

Use `<Link>` instead of `<a>` for internal navigation.

```jsx
import { Link } from 'react-router-dom';

function Navbar() {
  return (
    <nav>
      <Link to="/">Home</Link> | <Link to="/about">About</Link>
    </nav>
  );
}
```

---

## 🔁 5. Redirects and Not Found (Optional)

### Not Found (404):

```jsx
<Route path="*" element={<h2>Page Not Found</h2>} />
```

### Redirect:

```jsx
import { Navigate } from 'react-router-dom';

<Route path="/old" element={<Navigate to="/new" />} />
```

---

## 🧭 6. useNavigate Hook (Programmatic Navigation)

```jsx
import { useNavigate } from 'react-router-dom';

function Login() {
  const navigate = useNavigate();

  const handleLogin = () => {
    // do login logic
    navigate('/dashboard');
  };

  return <button onClick={handleLogin}>Login</button>;
}
```

---

## 🛠️ 7. useParams Hook (Dynamic Routing)

```jsx
<Route path="/user/:id" element={<User />} />
```

```jsx
// User.js
import { useParams } from 'react-router-dom';

export default function User() {
  const { id } = useParams();
  return <h2>User ID: {id}</h2>;
}
```

---

## ✅ Summary

| Feature        | Code Example                   |
| -------------- | ------------------------------ |
| Setup Router   | `<BrowserRouter>`              |
| Define Routes  | `<Routes> <Route /> </Routes>` |
| Navigate Links | `<Link to="/path">`            |
| Dynamic Route  | `/user/:id` and `useParams()`  |
| Redirect       | `<Navigate to="/new" />`       |
| Programmatic   | `useNavigate()`                |

---

Great! Let's build a **simple 3-page React app** using **React Router**. This app will include:

### ✅ Pages:

1. Home Page
2. About Page
3. Contact Page

---

## 📁 Folder Structure

```
src/
├── App.js
├── pages/
│   ├── Home.js
│   ├── About.js
│   └── Contact.js
├── components/
│   └── Navbar.js
```

---

## 1️⃣ Install React Router

Make sure it's installed:

```bash
npm install react-router-dom
```

---

## 2️⃣ Create Page Components

### `pages/Home.js`

```jsx
export default function Home() {
  return <h2>🏠 Welcome to the Home Page</h2>;
}
```

### `pages/About.js`

```jsx
export default function About() {
  return <h2>ℹ️ About Us</h2>;
}
```

### `pages/Contact.js`

```jsx
export default function Contact() {
  return <h2>📞 Contact Page</h2>;
}
```

---

## 3️⃣ Create Navigation Bar

### `components/Navbar.js`

```jsx
import { Link } from 'react-router-dom';

export default function Navbar() {
  return (
    <nav style={{ padding: '1rem', background: '#eee' }}>
      <Link to="/" style={{ marginRight: '10px' }}>Home</Link>
      <Link to="/about" style={{ marginRight: '10px' }}>About</Link>
      <Link to="/contact">Contact</Link>
    </nav>
  );
}
```

---

## 4️⃣ Main App Setup with Routing

### `App.js`

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import About from './pages/About';
import Contact from './pages/Contact';
import Navbar from './components/Navbar';

function App() {
  return (
    <BrowserRouter>
      <Navbar />
      <div style={{ padding: '1rem' }}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
          <Route path="/contact" element={<Contact />} />
          <Route path="*" element={<h2>404 - Page Not Found</h2>} />
        </Routes>
      </div>
    </BrowserRouter>
  );
}

export default App;
```

---

## ✅ How It Works:

| File               | Role                                       |
| ------------------ | ------------------------------------------ |
| `BrowserRouter`    | Wraps the app, enabling routing            |
| `Routes` + `Route` | Define what to show for each URL path      |
| `Link`             | Navigates between pages without refreshing |
| `Navbar.js`        | Provides navigation to all pages           |
| `*` Route          | Catches undefined routes (404 page)        |

---

## 🧪 To Run the App:

Make sure you have this in your `index.js` (default from Create React App):

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

Then run:

```bash
npm start
```

---

## 🏁 Result:

* Go to `/` → Home page
* Go to `/about` → About page
* Go to `/contact` → Contact page
* Try `/something` → See "404 - Page Not Found"

---

Perfect! Let's enhance the React Router app by adding **Context API** for **state management**. Context helps you **share state** (like user data, theme, language, etc.) across components **without prop drilling**.

---

## 🧠 What We'll Do:

We’ll build a context to manage a **user's login status**, and display their name across all pages.

---

## 📁 Updated Folder Structure

```
src/
├── App.js
├── index.js
├── context/
│   └── UserContext.js
├── pages/
│   ├── Home.js
│   ├── About.js
│   └── Contact.js
├── components/
│   └── Navbar.js
```

---

## 1️⃣ Create the Context

### `context/UserContext.js`

```jsx
import { createContext, useState } from 'react';

export const UserContext = createContext();

export function UserProvider({ children }) {
  const [user, setUser] = useState({ name: "Alice", loggedIn: true });

  return (
    <UserContext.Provider value={{ user, setUser }}>
      {children}
    </UserContext.Provider>
  );
}
```

✅ **Explanation:**

* `UserContext` is created with `createContext()`.
* `UserProvider` wraps your app and provides the state (`user` and `setUser`).

---

## 2️⃣ Wrap Your App with the Provider

### `index.js`

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import { UserProvider } from './context/UserContext';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <UserProvider>
    <App />
  </UserProvider>
);
```

---

## 3️⃣ Access Context in Any Component

### `components/Navbar.js`

```jsx
import { Link } from 'react-router-dom';
import { useContext } from 'react';
import { UserContext } from '../context/UserContext';

export default function Navbar() {
  const { user } = useContext(UserContext);

  return (
    <nav style={{ padding: '1rem', background: '#eee' }}>
      <span style={{ marginRight: '1rem' }}>👤 {user.name}</span>
      <Link to="/" style={{ marginRight: '10px' }}>Home</Link>
      <Link to="/about" style={{ marginRight: '10px' }}>About</Link>
      <Link to="/contact">Contact</Link>
    </nav>
  );
}
```

✅ Now the Navbar shows the current user's name from the context.

---

## 4️⃣ Update State from a Page

### `pages/Contact.js`

```jsx
import { useContext } from 'react';
import { UserContext } from '../context/UserContext';

export default function Contact() {
  const { user, setUser } = useContext(UserContext);

  const handleLogout = () => {
    setUser({ ...user, loggedIn: false, name: "Guest" });
  };

  return (
    <div>
      <h2>📞 Contact Page</h2>
      <p>Current User: {user.name}</p>
      <button onClick={handleLogout}>Log Out</button>
    </div>
  );
}
```

---

## ✅ Summary of What We Did:

| Step             | Purpose                                            |
| ---------------- | -------------------------------------------------- |
| `UserContext.js` | Created global state using `createContext`         |
| `UserProvider`   | Shared state via Context Provider                  |
| `useContext()`   | Accessed or modified user state from any component |
| Navbar           | Displayed current user                             |
| Contact page     | Modified global state (logged out user)            |

---

## 🌙 Use context for theme switching (light/dark)?

Awesome! Let's use **React Context API** to implement a **light/dark theme switcher** in your app. This is a real-world use case and very common in modern UIs.

---

## 🎯 What We’ll Build:

* A global theme context (`ThemeContext`)
* A toggle button in the `Navbar`
* Theme styles applied to the whole app

---

## 📁 Updated Folder Structure

```
src/
├── App.js
├── index.js
├── context/
│   ├── UserContext.js
│   └── ThemeContext.js  ← New
├── pages/
│   ├── Home.js
│   ├── About.js
│   └── Contact.js
├── components/
│   └── Navbar.js
```

---

## 1️⃣ Create Theme Context

### `context/ThemeContext.js`

```jsx
import { createContext, useState, useEffect } from 'react';

export const ThemeContext = createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  // Optional: Load from localStorage
  useEffect(() => {
    const savedTheme = localStorage.getItem('app-theme');
    if (savedTheme) setTheme(savedTheme);
  }, []);

  useEffect(() => {
    localStorage.setItem('app-theme', theme);
  }, [theme]);

  const toggleTheme = () => {
    setTheme(prev => (prev === 'light' ? 'dark' : 'light'));
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      <div className={`app ${theme}`}>{children}</div>
    </ThemeContext.Provider>
  );
}
```

✅ This provides the current theme and a function to toggle it.

---

## 2️⃣ Wrap App with ThemeProvider

### `index.js`

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import { UserProvider } from './context/UserContext';
import { ThemeProvider } from './context/ThemeContext';
import './index.css'; // Add theme styles here

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <UserProvider>
    <ThemeProvider>
      <App />
    </ThemeProvider>
  </UserProvider>
);
```

---

## 3️⃣ Add Toggle Button in Navbar

### `components/Navbar.js`

```jsx
import { Link } from 'react-router-dom';
import { useContext } from 'react';
import { UserContext } from '../context/UserContext';
import { ThemeContext } from '../context/ThemeContext';

export default function Navbar() {
  const { user } = useContext(UserContext);
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <nav style={{ padding: '1rem', background: theme === 'light' ? '#eee' : '#333', color: theme === 'light' ? '#000' : '#fff' }}>
      <span style={{ marginRight: '1rem' }}>👤 {user.name}</span>
      <Link to="/" style={{ marginRight: '10px' }}>Home</Link>
      <Link to="/about" style={{ marginRight: '10px' }}>About</Link>
      <Link to="/contact" style={{ marginRight: '10px' }}>Contact</Link>
      <button onClick={toggleTheme} style={{ float: 'right' }}>
        Switch to {theme === 'light' ? 'Dark' : 'Light'} Mode
      </button>
    </nav>
  );
}
```

---

## 4️⃣ Add Basic Theme Styles

### `index.css` (global styles)

```css
/* Default app styles */
body {
  margin: 0;
  font-family: sans-serif;
}

.app.light {
  background-color: #ffffff;
  color: #000000;
}

.app.dark {
  background-color: #121212;
  color: #ffffff;
}
```

---

## ✅ Result:

* The entire app background/text changes based on theme.
* Theme is saved in `localStorage`.
* Toggle is always available in the navbar.

---

## ✅ Summary

| Feature      | Code                                    |
| ------------ | --------------------------------------- |
| Theme State  | `useState('light')` in context          |
| Access Theme | `useContext(ThemeContext)`              |
| Theme Switch | `toggleTheme()` in Navbar               |
| Styling      | Based on `.light` / `.dark` CSS classes |

---
