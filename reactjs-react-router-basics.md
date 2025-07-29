Perfect! Let’s build a **Personal Portfolio Website** using **React Router + Vite + TypeScript (TSX)**.

---

## 🎯 What We’ll Cover

✅ Vite + TypeScript setup
✅ React Router setup
✅ Page components (`Home`, `About`, `Projects`, `Contact`)
✅ Navigation bar with `Link`
✅ Dynamic routes (optional)
✅ Basic layout for portfolio

---

## 📦 1. Create Project

```bash
npm create vite@latest my-portfolio -- --template react-ts
cd my-portfolio
npm install
npm install react-router-dom
npm run dev
```

---

## 📁 2. Folder Structure

```
src/
├── App.tsx
├── main.tsx
├── pages/
│   ├── Home.tsx
│   ├── About.tsx
│   ├── Projects.tsx
│   └── Contact.tsx
├── components/
│   └── Navbar.tsx
```

---

## 📄 3. Set Up React Router

### 📁 `src/main.tsx`

```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import { BrowserRouter } from 'react-router-dom';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

---

## 🔁 4. Set Up Routing

### 📁 `src/App.tsx`

```tsx
import { Routes, Route } from 'react-router-dom';
import Navbar from './components/Navbar';
import Home from './pages/Home';
import About from './pages/About';
import Projects from './pages/Projects';
import Contact from './pages/Contact';

function App() {
  return (
    <div>
      <Navbar />
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/projects" element={<Projects />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </div>
  );
}

export default App;
```

---

## 🧭 5. Create Navbar

### 📁 `src/components/Navbar.tsx`

```tsx
import { Link } from 'react-router-dom';

export default function Navbar() {
  return (
    <nav style={{ display: 'flex', gap: '1rem', padding: '1rem', background: '#eee' }}>
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
      <Link to="/projects">Projects</Link>
      <Link to="/contact">Contact</Link>
    </nav>
  );
}
```

---

## 📄 6. Create Page Components

### 📁 `src/pages/Home.tsx`

```tsx
export default function Home() {
  return <h1>Welcome to My Portfolio</h1>;
}
```

### 📁 `src/pages/About.tsx`

```tsx
export default function About() {
  return <h1>About Me</h1>;
}
```

### 📁 `src/pages/Projects.tsx`

```tsx
export default function Projects() {
  return <h1>My Projects</h1>;
}
```

### 📁 `src/pages/Contact.tsx`

```tsx
export default function Contact() {
  return <h1>Contact Me</h1>;
}
```

---

## 🧠 7. Understanding Routing

| Component  | Path        | Role                  |
| ---------- | ----------- | --------------------- |
| `Home`     | `/`         | Landing page          |
| `About`    | `/about`    | Bio, experience, etc. |
| `Projects` | `/projects` | Showcase your work    |
| `Contact`  | `/contact`  | Email, social links   |

📝 Each route maps to a different **page component** via `<Route>` and `<Link>` is used instead of `<a>` for SPA navigation.

---

## ✨ 8. Style Suggestion (Optional)

Add this to `index.css` or use Tailwind / styled-components:

```css
body {
  font-family: sans-serif;
  margin: 0;
  padding: 0;
}

nav a {
  text-decoration: none;
  color: #333;
}

nav a:hover {
  color: #0070f3;
}
```

---

## 🚀 9. Deploy (optional)

### Deploy with Vercel:

```bash
npm run build
```

Upload the `dist/` folder.

🛠 If using React Router, configure:

* **Vercel:** Use `rewrite` rule to `index.html`

  ```
  /*    /index.html   200
  ```

---

## ✅ Summary

| Feature              | Status |
| -------------------- | ------ |
| Vite + TS setup      | ✅      |
| React Router working | ✅      |
| Pages created        | ✅      |
| Navigation bar       | ✅      |
