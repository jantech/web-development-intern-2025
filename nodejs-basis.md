**Basics of Node.js**:

---

### 🌐 What is Node.js?

**Node.js** is a runtime environment that allows you to run **JavaScript on the server** (outside the browser). It's built on **Chrome’s V8 JavaScript engine** and is especially good for building scalable, fast, and real-time applications (like chat apps, APIs, etc.).

---

### 📦 Key Features of Node.js

* **Asynchronous & Non-blocking I/O**
* **Single-threaded but highly scalable**
* **Event-driven**
* **NPM (Node Package Manager)** to install libraries
* **Great for API development, real-time apps, etc.**

---

### 📁 How to Get Started

1. **Install Node.js**
   Go to [https://nodejs.org](https://nodejs.org) and download the LTS version.

2. **Check installation** in your terminal:

   ```bash
   node -v
   npm -v
   ```

---

### 📜 Writing a Simple Node.js App

#### 1. Hello World:

```js
// hello.js
console.log("Hello from Node.js!");
```

Run it with:

```bash
node hello.js
```

#### 2. Simple HTTP Server:

```js
// server.js
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello, this is a simple Node.js server!');
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000/');
});
```

Run it:

```bash
node server.js
```

Then visit: `http://localhost:3000`

---

### 📁 Node.js Core Modules

* `http` – for web server
* `fs` – file system operations
* `path` – path utilities
* `os` – operating system info
* `events` – event handling

---

### 📦 NPM (Node Package Manager)

Install packages like:

```bash
npm init -y          # create a package.json
npm install express  # install express.js
```

Use them in your code:

```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello from Express!');
});

app.listen(3000, () => console.log('Server is running...'));
```

---

### 🛠 Common Uses of Node.js

* REST APIs
* Real-time apps (chat, games)
* File manipulation tools
* Microservices
* Backend for web & mobile apps

---


## ✅ 1. **Express: Small REST API**

### 📂 Project: Simple "To-Do List" API

**Install Express:**

```bash
npm init -y
npm install express
```

**Code (`app.js`):**

```js
const express = require('express');
const app = express();

app.use(express.json());

let todos = [
  { id: 1, task: 'Buy groceries' },
  { id: 2, task: 'Clean the house' }
];

app.get('/todos', (req, res) => {
  res.json(todos);
});

app.post('/todos', (req, res) => {
  const newTodo = { id: Date.now(), task: req.body.task };
  todos.push(newTodo);
  res.status(201).json(newTodo);
});

app.listen(3000, () => {
  console.log('API running on http://localhost:3000');
});
```

Try it with [Postman](https://www.postman.com/) or curl:

* `GET http://localhost:3000/todos`
* `POST http://localhost:3000/todos` with JSON body: `{ "task": "Walk the dog" }`

---

## ✅ 2. **File Handling: Reading/Writing Files**

### 📂 Project: Log Messages to a File

**Code (`file-log.js`):**

```js
const fs = require('fs');

const logMessage = (msg) => {
  const log = `[${new Date().toISOString()}] ${msg}\n`;
  fs.appendFile('log.txt', log, (err) => {
    if (err) throw err;
    console.log('Log saved!');
  });
};

logMessage('User signed in');
```

**Explanation:**

* Appends log messages to `log.txt`
* Uses `fs.appendFile` for non-blocking I/O

Run it:

```bash
node file-log.js
```

---

## ✅ 3. **Database Integration**

### 📂 Project: Store Users in a Database
* Backend: Node.js + Express + MySQL
* Frontend: HTML or React (your choice)
* Functionality: Add and list users

**Install dependencies:**
```bash
npm init -y
npm install express mysql2 cors
```


### 🧠 Create MySQL Database and Table

```sql
-- In your MySQL client:
CREATE DATABASE userdb;

USE userdb;

CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100)
);
```

### 🧠 Backend Code (`server.js`):

```js
const express = require('express');
const mysql = require('mysql2');
const cors = require('cors');

const app = express();
app.use(cors());
app.use(express.json());

// MySQL connection
const db = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: '',     // <-- set your password
  database: 'userdb'
});

db.connect(err => {
  if (err) throw err;
  console.log('MySQL connected');
});

// API routes
app.get('/users', (req, res) => {
  db.query('SELECT * FROM users', (err, results) => {
    if (err) throw err;
    res.json(results);
  });
});

app.post('/users', (req, res) => {
  const { name, email } = req.body;
  db.query('INSERT INTO users (name, email) VALUES (?, ?)', [name, email], (err, result) => {
    if (err) throw err;
    res.status(201).json({ id: result.insertId, name, email });
  });
});

app.listen(3000, () => console.log('Server running at http://localhost:3000'));
```

---

## 🎨 Step 2: Frontend (Choose One)

---

### 🔹 Option A: **Simple HTML Frontend**

#### 📄 `index.html`

```html
<!DOCTYPE html>
<html>
<head>
  <title>User Manager</title>
</head>
<body>
  <h1>Add User</h1>
  <input id="name" placeholder="Name" />
  <input id="email" placeholder="Email" />
  <button onclick="addUser()">Add</button>

  <h2>Users:</h2>
  <ul id="userList"></ul>

  <script>
    async function addUser() {
      const name = document.getElementById('name').value;
      const email = document.getElementById('email').value;

      await fetch('http://localhost:3000/users', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ name, email })
      });

      loadUsers();
    }

    async function loadUsers() {
      const res = await fetch('http://localhost:3000/users');
      const users = await res.json();

      document.getElementById('userList').innerHTML = users
        .map(u => `<li>${u.name} (${u.email})</li>`)
        .join('');
    }

    loadUsers();
  </script>
</body>
</html>
```

📦 Run the HTML file in the browser, and make sure your backend is running.

---


### 🔹 Option B: **React Frontend**

#### 🛠 Create a React app (in separate folder):

```bash
npx create-react-app user-manager
cd user-manager
```

#### 🧠 React Code (`App.js`):

```jsx
import React, { useState, useEffect } from 'react';

function App() {
  const [users, setUsers] = useState([]);
  const [form, setForm] = useState({ name: '', email: '' });

  // Load users when component mounts
  useEffect(() => {
    loadUsers();
  }, []);

  // Fetch users from backend
  const loadUsers = async () => {
    const res = await fetch('http://localhost:3000/users');
    const data = await res.json();
    setUsers(data);
  };

  // Add a new user
  const handleSubmit = async () => {
    await fetch('http://localhost:3000/users', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(form)
    });

    setForm({ name: '', email: '' });
    loadUsers(); // reload users
  };

  return (
    <div style={{ padding: 20 }}>
      <h1>User Manager</h1>

      <input
        type="text"
        placeholder="Name"
        value={form.name}
        onChange={e => setForm({ ...form, name: e.target.value })}
      />
      <input
        type="email"
        placeholder="Email"
        value={form.email}
        onChange={e => setForm({ ...form, email: e.target.value })}
      />
      <button onClick={handleSubmit}>Add User</button>

      <h2>Users:</h2>
      <ul>
        {users.map(u => (
          <li key={u.id}>{u.name} ({u.email})</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

---


## 🚀 Final Setup

### 🔁 Run Everything:

1. Start MySQL server
2. Run Node.js backend:

   ```bash
   node server.js
   ```
3. For HTML: Open `index.html` in browser
   For React:

   ```bash
   npm start
   ```

---


## ✅ Result:

A full-stack web app where:

* Backend (Node.js + Express) handles API and talks to MySQL
* Frontend (HTML or React) allows you to add/view users

