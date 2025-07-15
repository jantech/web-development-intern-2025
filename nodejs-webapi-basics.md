Building a **Node.js + Express + MySQL REST API**, including:

* ✅ Project structure
* ✅ Full CRUD (Create, Read, Update, Delete)
* ✅ MySQL connection using `mysql2`
* ✅ Environment variables with `.env`
* ✅ Error handling
* ✅ Modular routing

---

## 🔧 Prerequisites

Make sure you have:

* Node.js installed → [https://nodejs.org/](https://nodejs.org/)
* MySQL installed and running
* MySQL database created (`mydb`) with a `users` table

---

## 📁 Final Project Structure

```
my-api/
├── .env
├── db.js
├── index.js
├── routes/
│   └── users.js
├── package.json
```

---

## 🪜 Step-by-Step Instructions

---

### ✅ 1. Create the Project

```bash
mkdir my-api
cd my-api
npm init -y
```

---

### ✅ 2. Install Dependencies

```bash
npm install express mysql2 dotenv
```

---

### ✅ 3. Setup MySQL Database

In your MySQL CLI or GUI:

```sql
CREATE DATABASE mydb;

USE mydb;

CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100)
);
```

---

### ✅ 4. Create `.env` for Configuration

In the root of your project, create `.env`:

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=yourpassword
DB_NAME=mydb
PORT=3000
```

Replace `yourpassword` and other values with your MySQL credentials.

---

### ✅ 5. Setup Database Connection (`db.js`)

```js
// db.js
require('dotenv').config();
const mysql = require('mysql2');

const pool = mysql.createPool({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME
});

module.exports = pool.promise(); // Use promise-based queries
```

---

### ✅ 6. Create User Routes (`routes/users.js`)

```js
// routes/users.js
const express = require('express');
const router = express.Router();
const db = require('../db');

// GET all users
router.get('/', async (req, res) => {
  try {
    const [rows] = await db.query('SELECT * FROM users');
    res.json(rows);
  } catch (err) {
    res.status(500).json({ message: 'Database error' });
  }
});

// GET user by ID
router.get('/:id', async (req, res) => {
  try {
    const [rows] = await db.query('SELECT * FROM users WHERE id = ?', [req.params.id]);
    if (!rows.length) return res.status(404).json({ message: 'User not found' });
    res.json(rows[0]);
  } catch (err) {
    res.status(500).json({ message: 'Database error' });
  }
});

// POST new user
router.post('/', async (req, res) => {
  const { name, email } = req.body;
  if (!name || !email) return res.status(400).json({ message: 'Name and email are required' });

  try {
    const [result] = await db.query('INSERT INTO users (name, email) VALUES (?, ?)', [name, email]);
    res.status(201).json({ id: result.insertId, name, email });
  } catch (err) {
    res.status(500).json({ message: 'Insert failed' });
  }
});

// PUT update user
router.put('/:id', async (req, res) => {
  const { name, email } = req.body;
  try {
    const [result] = await db.query(
      'UPDATE users SET name = ?, email = ? WHERE id = ?',
      [name, email, req.params.id]
    );
    if (result.affectedRows === 0) return res.status(404).json({ message: 'User not found' });
    res.json({ message: 'User updated' });
  } catch (err) {
    res.status(500).json({ message: 'Update failed' });
  }
});

// DELETE user
router.delete('/:id', async (req, res) => {
  try {
    const [result] = await db.query('DELETE FROM users WHERE id = ?', [req.params.id]);
    if (result.affectedRows === 0) return res.status(404).json({ message: 'User not found' });
    res.json({ message: 'User deleted' });
  } catch (err) {
    res.status(500).json({ message: 'Delete failed' });
  }
});

module.exports = router;
```

---

### ✅ 7. Main Entry File (`index.js`)

```js
// index.js
require('dotenv').config();
const express = require('express');
const app = express();
const userRoutes = require('./routes/users');

const PORT = process.env.PORT || 3000;

app.use(express.json()); // Parse JSON bodies
app.use('/api/users', userRoutes);

// Optional: Global error handler
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ message: 'Internal server error' });
});

app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}`);
});
```

---

## ✅ Test Your API

### ▶ Start Server

```bash
node index.js
```

### ▶ API Endpoints

| Method | URL              | Description    |
| ------ | ---------------- | -------------- |
| GET    | `/api/users`     | Get all users  |
| GET    | `/api/users/:id` | Get user by ID |
| POST   | `/api/users`     | Add new user   |
| PUT    | `/api/users/:id` | Update user    |
| DELETE | `/api/users/:id` | Delete user    |

### ▶ Sample POST Body

```json
{
  "name": "Alice",
  "email": "alice@example.com"
}
```

Use [Postman](https://www.postman.com/) or [Insomnia](https://insomnia.rest/) to test API endpoints.

---

## 📦 Summary

You now have a **modular and scalable REST API** in Node.js that:

* Connects to MySQL via `mysql2`
* Supports full CRUD operations
* Uses `.env` to manage sensitive config
* Has clean folder and file separation

---
