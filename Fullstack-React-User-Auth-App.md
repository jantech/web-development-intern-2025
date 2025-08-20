# Fullstack React User Auth App

# 1) Database (MariaDB)

## SQL (run this first)

```sql
-- database.sql
CREATE DATABASE IF NOT EXISTS auth_demo CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE auth_demo;

CREATE TABLE IF NOT EXISTS users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(255) NOT NULL UNIQUE,
  password_hash VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# 2) Backend — Node.js + Express


### ✅ 1. Create the Project

```bash
mkdir backend
cd backend
npm init -y
```
### ✅ 2. Install Dependencies

```bash
npm install express mysql2 dotenv jsonwebtoken cors bcryptjs
```

### ✅ 3. Folder Structure
```
backend/
  .env
  package.json
  src/
    server.js
    db.js
    middleware/
      auth.js
    routes/
      auth.js
```

---

## `backend/.env` (example)

```env
PORT=4000
NODE_ENV=development

DB_HOST=localhost
DB_USER=root
DB_PASS=your_password
DB_NAME=auth_demo

JWT_SECRET=super_secret_change_me
CORS_ORIGIN=http://localhost:5173
```

## `backend/package.json`

```json
{
  "name": "auth-backend",
  "version": "1.0.0",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "nodemon src/server.js",
    "start": "node src/server.js"
  },
  "dependencies": {
    "bcryptjs": "^2.4.3",
    "cors": "^2.8.5",
    "dotenv": "^16.4.5",
    "express": "^4.19.2",
    "jsonwebtoken": "^9.0.2",
    "mysql2": "^3.11.3"
  },
  "devDependencies": {
    "nodemon": "^3.1.4"
  }
}
```

## `backend/src/db.js`

```js
import mysql from 'mysql2/promise';
import dotenv from 'dotenv';
dotenv.config();

export const pool = mysql.createPool({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASS,
  database: process.env.DB_NAME,
  waitForConnections: true,
  connectionLimit: 10,
  namedPlaceholders: true
});
```

## `backend/src/middleware/auth.js`

```js
import jwt from 'jsonwebtoken';
import { pool } from '../db.js';

const JWT_SECRET = process.env.JWT_SECRET;

// Fetch user by ID without password hash
async function getUserById(id) {
  const [rows] = await pool.execute(
    'SELECT id, name, email, created_at FROM users WHERE id = ?',
    [id]
  );
  return rows[0] || null;
}

// Strict auth: 401 if invalid/missing
export async function requireAuth(req, res, next) {
  try {
    const hdr = req.headers.authorization || '';
    const token = hdr.startsWith('Bearer ') ? hdr.slice(7) : null;
    if (!token) return res.status(401).json({ error: 'Missing Authorization header' });
    const payload = jwt.verify(token, JWT_SECRET);
    const user = await getUserById(payload.id);
    if (!user) return res.status(401).json({ error: 'Invalid token' });
    req.user = user;
    next();
  } catch (err) {
    return res.status(401).json({ error: 'Unauthorized' });
  }
}

// Optional auth: never 401, just attaches user if token valid
export async function optionalAuth(req, res, next) {
  try {
    const hdr = req.headers.authorization || '';
    const token = hdr.startsWith('Bearer ') ? hdr.slice(7) : null;
    if (!token) return next();
    const payload = jwt.verify(token, JWT_SECRET);
    const user = await getUserById(payload.id);
    if (user) req.user = user;
    return next();
  } catch (_e) {
    // ignore invalid token
    return next();
  }
}
```

## `backend/src/routes/auth.js`

```js
import { Router } from 'express';
import { pool } from '../db.js';
import bcrypt from 'bcryptjs';
import jwt from 'jsonwebtoken';
import { optionalAuth } from '../middleware/auth.js';

const router = Router();
const JWT_SECRET = process.env.JWT_SECRET;
const TOKEN_EXPIRES = '7d'; // as required
const SALT_ROUNDS = 12;

// Helpers
function signToken(id) {
  return jwt.sign({ id }, JWT_SECRET, { expiresIn: TOKEN_EXPIRES });
}
function validateEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}

// POST /api/auth/register
router.post('/register', async (req, res) => {
  try {
    let { name, email, password } = req.body || {};
    if (
      typeof name !== 'string' || name.trim().length < 2 ||
      typeof email !== 'string' || !validateEmail(email) ||
      typeof password !== 'string' || password.length < 6
    ) {
      return res.status(400).json({ error: 'Invalid input' });
    }

    name = name.trim();
    email = email.trim().toLowerCase();

    // Unique email check
    const [existing] = await pool.execute('SELECT id FROM users WHERE email = ?', [email]);
    if (existing.length > 0) {
      return res.status(409).json({ error: 'Email already in use' });
    }

    const password_hash = await bcrypt.hash(password, SALT_ROUNDS);
    const [result] = await pool.execute(
      'INSERT INTO users (name, email, password_hash) VALUES (?, ?, ?)',
      [name, email, password_hash]
    );

    const user = {
      id: result.insertId,
      name,
      email,
      created_at: new Date().toISOString().slice(0, 19).replace('T', ' ')
    };
    const token = signToken(user.id);
    return res.status(201).json({ user, token });
  } catch (err) {
    console.error(err);
    return res.status(500).json({ error: 'Server error' });
  }
});

// POST /api/auth/login
router.post('/login', async (req, res) => {
  try {
    const { email, password } = req.body || {};
    if (typeof email !== 'string' || typeof password !== 'string') {
      return res.status(400).json({ error: 'Invalid input' });
    }
    const normalized = email.trim().toLowerCase();
    const [rows] = await pool.execute('SELECT * FROM users WHERE email = ?', [normalized]);
    const row = rows[0];
    if (!row) return res.status(400).json({ error: 'Invalid credentials' });

    const ok = await bcrypt.compare(password, row.password_hash);
    if (!ok) return res.status(400).json({ error: 'Invalid credentials' });

    const user = { id: row.id, name: row.name, email: row.email, created_at: row.created_at };
    const token = signToken(user.id);
    return res.json({ user, token });
  } catch (err) {
    console.error(err);
    return res.status(500).json({ error: 'Server error' });
  }
});

// GET /api/auth/me  -> { user | null }
router.get('/me', optionalAuth, async (req, res) => {
  return res.json({ user: req.user || null });
});

export default router;
```

## `backend/src/server.js`

```js
import express from 'express';
import cors from 'cors';
import dotenv from 'dotenv';
dotenv.config();

import authRoutes from './routes/auth.js';
import { requireAuth } from './middleware/auth.js';

const app = express();
const PORT = process.env.PORT || 4000;

app.use(express.json());

// CORS: allow Authorization header from configured origin
app.use(
  cors({
    origin: process.env.CORS_ORIGIN,
    methods: ['GET', 'POST', 'OPTIONS'],
    allowedHeaders: ['Content-Type', 'Authorization']
  })
);

// Routes
app.use('/api/auth', authRoutes);

app.get('/api/health', (_req, res) => res.json({ ok: true }));

// Protected demo route
app.get('/api/protected', requireAuth, (req, res) => {
  res.json({ message: `Welcome, ${req.user.name}. This is protected data.` });
});

app.listen(PORT, () => {
  console.log(`API listening on http://localhost:${PORT}`);
});
```

**Why this backend meets your spec**

* Uses `mysql2/promise` pool and validates unique email on register.
* Passwords hashed with **bcryptjs** (12 rounds).
* JWT expires in **7 days** and is returned on both register & login.
* `/api/auth/me` returns `{ user|null }` (never 401), making session restore simple.
* CORS strictly allows your Vite origin and the `Authorization` header.

---

# 3) Frontend — React 18 + Vite + TS + Tailwind + DaisyUI

```
frontend/
  .env
  package.json
  tsconfig.json
  vite.config.ts
  tailwind.config.js
  postcss.config.js
  styles.css
  src/
    main.tsx
    App.tsx
    auth.tsx
    lib/
      fetch.ts
    components/
      Navbar.tsx
      ProtectedRoute.tsx
    pages/
      Login.tsx
      Register.tsx
      Dashboard.tsx
```

## `frontend/.env`

```env
VITE_API_BASE=/api
```

## `frontend/package.json`

```json
{
  "name": "auth-frontend",
  "private": true,
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview --port 5173"
  },
  "dependencies": {
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-router-dom": "^6.26.2"
  },
  "devDependencies": {
    "@types/react": "^18.3.5",
    "@types/react-dom": "^18.3.0",
    "@types/node": "^22.5.4",
    "autoprefixer": "^10.4.20",
    "daisyui": "^4.12.14",
    "postcss": "^8.4.45",
    "tailwindcss": "^3.4.10",
    "typescript": "^5.5.4",
    "vite": "^5.4.2"
  }
}
```

## `frontend/tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    "moduleResolution": "Bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src"]
}
```

## `frontend/vite.config.ts`

```ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

// Proxy /api -> backend on 4000
export default defineConfig({
  plugins: [react()],
  server: {
    port: 5173,
    proxy: {
      '/api': {
        target: 'http://localhost:4000',
        changeOrigin: true
      }
    }
  }
});
```

## `frontend/tailwind.config.js`

```js
/** @type {import('tailwindcss').Config} */
import daisyui from 'daisyui';

export default {
  content: ['./index.html', './src/**/*.{ts,tsx}'],
  theme: { extend: {} },
  plugins: [daisyui],
  daisyui: {
    themes: ['winter']
  }
};
```

## `frontend/postcss.config.js`

```js
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {}
  }
};
```

## `frontend/styles.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* Optional: layout tweaks */
html, body, #root {
  height: 100%;
}
```

---

## `frontend/src/lib/fetch.ts`

```ts
export type HttpMethod = 'GET' | 'POST';

export interface ApiError {
  error?: string;
  message?: string;
}

const API_BASE = import.meta.env.VITE_API_BASE || '/api';

/**
 * Fetch helper that:
 * - attaches Authorization if token in localStorage
 * - JSON encodes body when needed
 * - throws Error with server message on non-2xx
 */
export async function apiFetch<T>(
  path: string,
  options: { method?: HttpMethod; body?: unknown; headers?: Record<string, string> } = {}
): Promise<T> {
  const token = localStorage.getItem('token');
  const headers: Record<string, string> = {
    'Content-Type': 'application/json',
    ...(options.headers || {})
  };
  if (token) headers['Authorization'] = `Bearer ${token}`;

  const res = await fetch(`${API_BASE}${path}`, {
    method: options.method || 'GET',
    headers,
    body: options.body ? JSON.stringify(options.body) : undefined
  });

  const isJson = res.headers.get('content-type')?.includes('application/json');
  const data = isJson ? await res.json() : null;

  if (!res.ok) {
    const message =
      (data && ((data as ApiError).error || (data as ApiError).message)) ||
      `${res.status} ${res.statusText}`;
    throw new Error(message);
  }

  return (data ?? ({} as T)) as T;
}
```

---

## `frontend/src/auth.tsx` (Auth Context)

```tsx
import React, { createContext, useContext, useEffect, useMemo, useState } from 'react';
import { apiFetch } from './lib/fetch';

export type User = {
  id: number;
  name: string;
  email: string;
  created_at: string;
};

type AuthContextType = {
  user: User | null;
  token: string | null;
  loading: boolean;
  login: (email: string, password: string) => Promise<void>;
  register: (name: string, email: string, password: string) => Promise<void>;
  logout: () => void;
};

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export const AuthProvider: React.FC<React.PropsWithChildren> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);
  const [token, setToken] = useState<string | null>(null);
  const [loading, setLoading] = useState(true);

  // On mount: restore token & validate session
  useEffect(() => {
    const t = localStorage.getItem('token');
    setToken(t);
    (async () => {
      try {
        if (t) {
          const data = await apiFetch<{ user: User | null }>('/auth/me');
          setUser(data.user);
          if (!data.user) {
            localStorage.removeItem('token');
            setToken(null);
          }
        }
      } catch {
        localStorage.removeItem('token');
        setToken(null);
        setUser(null);
      } finally {
        setLoading(false);
      }
    })();
  }, []);

  const login = async (email: string, password: string) => {
    const data = await apiFetch<{ user: User; token: string }>('/auth/login', {
      method: 'POST',
      body: { email, password }
    });
    localStorage.setItem('token', data.token);
    setToken(data.token);
    setUser(data.user);
  };

  const register = async (name: string, email: string, password: string) => {
    const data = await apiFetch<{ user: User; token: string }>('/auth/register', {
      method: 'POST',
      body: { name, email, password }
    });
    localStorage.setItem('token', data.token);
    setToken(data.token);
    setUser(data.user);
  };

  const logout = () => {
    localStorage.removeItem('token'); // server logout is no-op by spec
    setToken(null);
    setUser(null);
  };

  const value = useMemo(
    () => ({ user, token, loading, login, register, logout }),
    [user, token, loading]
  );

  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
};

export function useAuth(): AuthContextType {
  const ctx = useContext(AuthContext);
  if (!ctx) throw new Error('useAuth must be used within AuthProvider');
  return ctx;
}
```

---

## `frontend/src/components/ProtectedRoute.tsx`

```tsx
import React from 'react';
import { Navigate } from 'react-router-dom';
import { useAuth } from '../auth';

type Props = { children: React.ReactNode };

const ProtectedRoute: React.FC<Props> = ({ children }) => {
  const { user, loading } = useAuth();

  if (loading) {
    return (
      <div className="min-h-screen flex items-center justify-center">
        <span className="loading loading-spinner loading-lg" />
      </div>
    );
  }

  if (!user) return <Navigate to="/" replace />;

  return <>{children}</>;
};

export default ProtectedRoute;
```

---

## `frontend/src/components/Navbar.tsx`

```tsx
import { Link, useNavigate } from 'react-router-dom';
import { useAuth } from '../auth';

export default function Navbar() {
  const { user, logout } = useAuth();
  const navigate = useNavigate();

  const handleLogout = () => {
    logout();
    navigate('/');
  };

  return (
    <div className="navbar bg-base-100 shadow-sm">
      <div className="flex-1">
        <Link to="/dashboard" className="btn btn-ghost text-xl">
          React Auth App
        </Link>
        <div className="form-control ml-2 hidden md:block">
          <input type="text" placeholder="Search" className="input input-bordered w-64" />
        </div>
      </div>

      <div className="flex-none gap-2">
        {user && (
          <div className="dropdown dropdown-end">
            <div tabIndex={0} role="button" className="btn btn-ghost btn-circle avatar">
              <div className="w-10 rounded-full">
                {/* Placeholder avatar */}
                <img alt="avatar" src={`https://api.dicebear.com/8.x/initials/svg?seed=${encodeURIComponent(user.name)}`} />
              </div>
            </div>
            <ul
              tabIndex={0}
              className="mt-3 z-[1] p-2 shadow menu menu-sm dropdown-content bg-base-100 rounded-box w-52"
            >
              <li className="menu-title px-4 py-2">Signed in as<br /><b>{user.name}</b></li>
              <li><a>Profile</a></li>
              <li><a>Settings</a></li>
              <li><button onClick={handleLogout}>Logout</button></li>
            </ul>
          </div>
        )}
      </div>
    </div>
  );
}
```

---

## `frontend/src/pages/Login.tsx`

```tsx
import { FormEvent, useState } from 'react';
import { Link, useNavigate } from 'react-router-dom';
import { useAuth } from '../auth';

export default function Login() {
  const { login } = useAuth();
  const navigate = useNavigate();
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [submitting, setSubmitting] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const onSubmit = async (e: FormEvent) => {
    e.preventDefault();
    setError(null);
    setSubmitting(true);
    try {
      await login(email, password);
      navigate('/dashboard');
    } catch (err: unknown) {
      setError((err as Error).message);
    } finally {
      setSubmitting(false);
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-base-200">
      <div className="card w-full max-w-md bg-base-100 shadow-xl">
        <div className="card-body">
          <h2 className="card-title">Login</h2>
          {error && <div className="alert alert-error"><span>{error}</span></div>}

          <form onSubmit={onSubmit} className="space-y-4">
            <input
              className="input input-bordered w-full"
              type="email"
              placeholder="Email"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              required
            />
            <input
              className="input input-bordered w-full"
              type="password"
              placeholder="Password"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
              required
            />
            <button className="btn btn-primary w-full" disabled={submitting}>
              {submitting ? <span className="loading loading-spinner" /> : 'Login'}
            </button>
          </form>

          <p className="text-sm mt-4">
            New here? <Link to="/register" className="link">Create an account</Link>
          </p>
        </div>
      </div>
    </div>
  );
}
```

---

## `frontend/src/pages/Register.tsx`

```tsx
import { FormEvent, useState } from 'react';
import { Link, useNavigate } from 'react-router-dom';
import { useAuth } from '../auth';

export default function Register() {
  const { register } = useAuth();
  const navigate = useNavigate();
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [submitting, setSubmitting] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const onSubmit = async (e: FormEvent) => {
    e.preventDefault();
    setError(null);
    setSubmitting(true);
    try {
      await register(name, email, password);
      navigate('/dashboard');
    } catch (err: unknown) {
      setError((err as Error).message);
    } finally {
      setSubmitting(false);
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-base-200">
      <div className="card w-full max-w-md bg-base-100 shadow-xl">
        <div className="card-body">
          <h2 className="card-title">Register</h2>
          {error && <div className="alert alert-error"><span>{error}</span></div>}

          <form onSubmit={onSubmit} className="space-y-4">
            <input
              className="input input-bordered w-full"
              type="text"
              placeholder="Full name"
              value={name}
              onChange={(e) => setName(e.target.value)}
              required
              minLength={2}
            />
            <input
              className="input input-bordered w-full"
              type="email"
              placeholder="Email"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              required
            />
            <input
              className="input input-bordered w-full"
              type="password"
              placeholder="Password (min 6 chars)"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
              required
              minLength={6}
            />
            <button className="btn btn-primary w-full" disabled={submitting}>
              {submitting ? <span className="loading loading-spinner" /> : 'Create account'}
            </button>
          </form>

          <p className="text-sm mt-4">
            Already have an account? <Link to="/" className="link">Login</Link>
          </p>
        </div>
      </div>
    </div>
  );
}
```

---

## `frontend/src/pages/Dashboard.tsx`

```tsx
import { useEffect, useState } from 'react';
import Navbar from '../components/Navbar';
import { apiFetch } from '../lib/fetch';

export default function Dashboard() {
  const [message, setMessage] = useState<string>('');
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    (async () => {
      try {
        const data = await apiFetch<{ message: string }>('/protected');
        setMessage(data.message);
      } catch (e) {
        setError((e as Error).message);
      } finally {
        setLoading(false);
      }
    })();
  }, []);

  return (
    <div className="min-h-screen bg-base-200">
      <Navbar />
      <div className="container mx-auto p-6">
        {loading && (
          <div className="flex justify-center py-16">
            <span className="loading loading-spinner loading-lg" />
          </div>
        )}
        {error && <div className="alert alert-error"><span>{error}</span></div>}
        {!loading && !error && (
          <div className="card bg-base-100 shadow-xl">
            <div className="card-body">
              <h2 className="card-title">Dashboard</h2>
              <p>{message}</p>
            </div>
          </div>
        )}
      </div>
    </div>
  );
}
```

---

## `frontend/src/App.tsx`

```tsx
import { Route, Routes } from 'react-router-dom';
import Login from './pages/Login';
import Register from './pages/Register';
import Dashboard from './pages/Dashboard';
import ProtectedRoute from './components/ProtectedRoute';

export default function App() {
  return (
    <Routes>
      <Route path="/" element={<Login />} />
      <Route path="/register" element={<Register />} />
      <Route
        path="/dashboard"
        element={
          <ProtectedRoute>
            <Dashboard />
          </ProtectedRoute>
        }
      />
    </Routes>
  );
}
```

## `frontend/src/main.tsx`

```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import { AuthProvider } from './auth';
import App from './App';
import '../styles.css';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <BrowserRouter>
      <AuthProvider>
        <App />
      </AuthProvider>
    </BrowserRouter>
  </React.StrictMode>
);
```

---

# 4) Run it

## Prereqs

* Node.js **18+**
* MariaDB running locally

## Steps

1. **Create DB & table**

   * Run the SQL from **database.sql**.

2. **Backend**

   * `cd backend`
   * Create `.env` (use the example above).
   * `npm install`
   * `npm run dev` (starts at **[http://localhost:4000](http://localhost:4000)**)

3. **Frontend**

   * Open a new terminal.
   * `cd frontend`
   * Create `.env` with `VITE_API_BASE=/api`
   * `npm install`
   * `npm run dev` (Vite at **[http://localhost:5173](http://localhost:5173)** with proxy to backend)

4. **Use it**

   * Visit `http://localhost:5173`
   * Register → auto-redirects to `/dashboard`
   * Refresh: session persists via `/api/auth/me`
   * Logout from the avatar menu

---

# 5) Why this matches your exact requirements

* **Tech stack**: React 18 + Vite + TS + Tailwind + DaisyUI (theme **winter**), React Router v6; Node 18+ + Express; MariaDB via `mysql2/promise`.
* **Auth**: Email/password, **bcrypt 12 rounds**, **JWT stored in localStorage** (no cookies).
* **Pages**: `/` (Login), `/register`, `/dashboard`.
* **Redirects**: After login/register → `/dashboard`.
* **Protected dashboard**: Guarded by `<ProtectedRoute>` checking `user` + `loading`.
* **Navbar**: DaisyUI with title, optional search, avatar menu (Profile/Settings/Logout).
* **Session persistence**: On mount read token from `localStorage` and call `/api/auth/me`.
* **API**:

  * `POST /api/auth/register` → **201** `{ user, token }`
  * `POST /api/auth/login` → **200** `{ user, token }`
  * `POST /api/auth/logout` → client clears localStorage (no server state)
  * `GET /api/auth/me` → `{ user|null }`
  * `GET /api/protected` → requires `Authorization`
  * **JWT expires in 7 days**
* **CORS**: Allows `Authorization` header from `CORS_ORIGIN`.
* **Vite proxy**: `/api` → `http://localhost:4000`.
* **TypeScript**: `strict: true`, typed `User` and context.
* **Security**: Never returns password hashes; validates inputs; notes on CORS + JWT.

---

# 6) Production notes (quick)

* Set real `CORS_ORIGIN` (your deployed frontend URL).
* Consider **shorter JWT** expirations and add **refresh tokens** if you want long-lived sessions.
* Harden error handling, logging, rate limiting, and input validation as you scale.
* Storing JWT in `localStorage` is per your spec; be mindful of XSS in production.

---
