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

## 📘 API Endpoints

### GET `/todos`

**Description:** Retrieve all TODO items.

---

### POST `/todos`

**Description:** Create a new TODO item.

**Request Body:**

```json
{
  "title": "New Task"
}
```

---

### PUT `/todos/:id`

**Description:** Fully replace a TODO item.

**Request Body:**

```json
{
  "title": "Rewritten Task",
  "completed": true
}
```

**Validation:**

* `title` is required, must be a non-empty string
* `completed` is required, must be a boolean

---

### PATCH `/todos/:id`

**Description:** Partially update fields of a TODO item.

---

### DELETE `/todos/:id`

**Description:** Delete a TODO item by ID.

---

**Source Code (`app.js`):**

```js
const express = require('express');
const app = express();

app.use(express.json());

let todos = [
    { id: 1, title: 'Learn Node.js', completed: false },
    { id: 2, title: 'Build a REST API', completed: false },
    { id: 3, title: 'Deploy to App', completed: false }
];

// GET all todos
app.get('/todos', (req, res) => {
    res.json(todos);
});

// POST a new todo
app.post('/todos', (req, res) => {
    const { title } = req.body;

    if (!title || typeof title !== 'string' || title.trim() === '') {
        return res.status(400).json({ message: 'Title is required and must be a non-empty string.' });
    }

    const newTodo = {
        id: todos.length + 1,
        title: title.trim(),
        completed: false
    };

    todos.push(newTodo);
    res.status(201).json(newTodo);
});

// PUT (replace entire todo)
app.put('/todos/:id', (req, res) => {
    const id = parseInt(req.params.id);
    const { title, completed } = req.body;

    const index = todos.findIndex(t => t.id === id);
    if (index === -1) {
        return res.status(404).json({ message: 'Todo not found' });
    }

    if (!title || typeof title !== 'string' || title.trim() === '') {
        return res.status(400).json({ message: 'Title is required and must be a non-empty string.' });
    }

    if (typeof completed !== 'boolean') {
        return res.status(400).json({ message: 'Completed is required and must be a boolean.' });
    }

    const updatedTodo = { id, title: title.trim(), completed };
    todos[index] = updatedTodo;

    res.json(updatedTodo);
});

// PATCH (partial update)
app.patch('/todos/:id', (req, res) => {
    const id = parseInt(req.params.id);
    const todo = todos.find(t => t.id === id);

    if (!todo) {
        return res.status(404).json({ message: 'Todo not found' });
    }

    const { title, completed } = req.body;

    if (title !== undefined) {
        if (typeof title !== 'string' || title.trim() === '') {
            return res.status(400).json({ message: 'Title must be a non-empty string.' });
        }
        todo.title = title.trim();
    }

    if (completed !== undefined) {
        if (typeof completed !== 'boolean') {
            return res.status(400).json({ message: 'Completed must be a boolean.' });
        }
        todo.completed = completed;
    }

    res.json(todo);
});

// DELETE a todo by ID
app.delete('/todos/:id', (req, res) => {
    const id = parseInt(req.params.id);
    const index = todos.findIndex(t => t.id === id);

    if (index === -1) {
        return res.status(404).json({ message: 'Todo not found' });
    }

    const deletedTodo = todos.splice(index, 1)[0];
    res.json(deletedTodo);
});

// Start the server
app.listen(3000, () => {
    console.log('API running on http://localhost:3000');
});
```
---
## 🚀 Run the Server

```bash
node app.js
```

Server runs on: [http://localhost:3000](http://localhost:3000)

---
Try it with [Postman](https://www.postman.com/) or curl:

## 🧪 curl Commands for TODO API

| Method | Endpoint            | Description               | Example curl Command |
|--------|---------------------|---------------------------|-----------------------|
| GET    | `/todos`            | Get all TODOs             | `curl http://localhost:3000/todos` |
| POST   | `/todos`            | Create new TODO           | `curl -X POST http://localhost:3000/todos -H "Content-Type: application/json" -d '{"title":"Walk the dog"}'` |
| PUT    | `/todos/:id`        | Replace TODO (full)       | `curl -X PUT http://localhost:3000/todos/1 -H "Content-Type: application/json" -d '{"title":"Go shopping", "completed":true}'` |
| PATCH  | `/todos/:id`        | Update TODO (partial)     | `curl -X PATCH http://localhost:3000/todos/1 -H "Content-Type: application/json" -d '{"completed":true}'` |
| DELETE | `/todos/:id`        | Delete a TODO             | `curl -X DELETE http://localhost:3000/todos/1` |


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

