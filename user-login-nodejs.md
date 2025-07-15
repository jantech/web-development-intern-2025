Let's build a **full-stack form submission web application** using **React + Bootstrap (frontend), Node.js + Express (backend), and MySQL (via XAMPP)** — with **raw MySQL queries using `mysql2`**, **no Sequelize**, and a **clear file/folder structure**.

---

# 🧑‍💻 Full-Stack Form Submission Web App — React, Node.js, MySQL, Bootstrap

## 📋 Technologies Used

- Frontend: React + Bootstrap (locally installed)
- Backend: Node.js + Express
- Database: MySQL (via XAMPP)
- No ORMs: raw MySQL queries using `mysql2`

---

## 📁 Project Structure

```
form-app/
├── backend/
│   ├── index.js
│   ├── db.js
│   └── package.json
├── frontend/
│   ├── public/
│   └── src/
│       ├── App.js
│       ├── Form.js
│       └── index.js
│   └── package.json
└── README.md
```

---

## 🔧 Step 1: Install XAMPP and Set Up MySQL

### 1.1 Install XAMPP

- Download XAMPP from [https://www.apachefriends.org](https://www.apachefriends.org)
- Install it on your system using default options.

### 1.2 Start MySQL and Apache

- Open the XAMPP Control Panel.
- Click **Start** next to **Apache** and **MySQL**.

### 1.3 Create MySQL Database via phpMyAdmin

1. Go to [http://localhost/phpmyadmin](http://localhost/phpmyadmin)
2. Click on **"New"**.
3. Name your database: `form_data`
4. Create a table named `submissions` with the following structure:

```sql
CREATE TABLE submissions (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100),
  message TEXT
);
```

---

## 📦 Step 2: Create Project Folders

```bash
mkdir form-app
cd form-app
npx create-react-app frontend
mkdir backend
```

---

## 🚀 Step 3: Backend Setup (Node.js + Express + mysql2)

### 3.1 Initialize Node.js Backend

```bash
cd backend
npm init -y
npm install express cors mysql2
```

### 3.2 Create `db.js` to Connect to MySQL

```js
// backend/db.js
const mysql = require("mysql2");

const connection = mysql.createConnection({
  host: "localhost",
  user: "root",
  password: "", // default password in XAMPP is empty
  database: "form_data",
});

connection.connect((err) => {
  if (err) throw err;
  console.log("Connected to MySQL database");
});

module.exports = connection;
```

### 3.3 Create `index.js` Express Server

```js
// backend/index.js
const express = require("express");
const cors = require("cors");
const db = require("./db");
const app = express();
const port = 5000;

app.use(cors());
app.use(express.json());

// POST endpoint to receive form data
app.post("/submit", (req, res) => {
  const { name, email, message } = req.body;

  // Server-side validation
  if (!name || !email || !message) {
    return res.status(400).json({ error: "All fields are required" });
  }

  const sql = "INSERT INTO submissions (name, email, message) VALUES (?, ?, ?)";
  db.query(sql, [name, email, message], (err, result) => {
    if (err) {
      console.error("Error inserting data:", err);
      return res.status(500).json({ error: "Database error" });
    }
    res.json({ success: true, message: "Form submitted successfully" });
  });
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

### 3.4 Run the Backend Server

```bash
node index.js
```

---

## 🎨 Step 4: Frontend Setup (React + Bootstrap)

### 4.1 Navigate to Frontend and Install Bootstrap Locally

```bash
cd ../frontend
npm install bootstrap
```

### 4.2 Import Bootstrap in `src/index.js`

```js
// frontend/src/index.js
import React from "react";
import ReactDOM from "react-dom/client";
import "./index.css";
import "bootstrap/dist/css/bootstrap.min.css";
import App from "./App";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);
```

### 4.3 Create a `Form.js` Component

```js
// frontend/src/Form.js
import React, { useState } from "react";

function Form() {
  const [form, setForm] = useState({ name: "", email: "", message: "" });
  const [status, setStatus] = useState("");

  const handleChange = (e) => {
    setForm({ ...form, [e.target.name]: e.target.value });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    // Client-side validation
    if (!form.name || !form.email || !form.message) {
      setStatus("Please fill in all fields.");
      return;
    }

    try {
      const res = await fetch("http://localhost:5000/submit", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(form),
      });

      const data = await res.json();

      if (data.success) {
        setStatus("Form submitted successfully!");
        setForm({ name: "", email: "", message: "" });
      } else {
        setStatus("Submission failed: " + data.error);
      }
    } catch (err) {
      console.error(err);
      setStatus("Server error");
    }
  };

  return (
    <div className="container mt-5">
      <h2>Contact Form</h2>
      <form onSubmit={handleSubmit}>
        <div className="mb-3">
          <label>Name</label>
          <input
            type="text"
            name="name"
            value={form.name}
            onChange={handleChange}
            className="form-control"
          />
        </div>
        <div className="mb-3">
          <label>Email</label>
          <input
            type="email"
            name="email"
            value={form.email}
            onChange={handleChange}
            className="form-control"
          />
        </div>
        <div className="mb-3">
          <label>Message</label>
          <textarea
            name="message"
            value={form.message}
            onChange={handleChange}
            className="form-control"
          />
        </div>
        <button type="submit" className="btn btn-primary">
          Submit
        </button>
        {status && <p className="mt-3">{status}</p>}
      </form>
    </div>
  );
}

export default Form;
```

### 4.4 Use `Form.js` in `App.js`

```js
// frontend/src/App.js
import React from "react";
import Form from "./Form";

function App() {
  return (
    <div className="App">
      <Form />
    </div>
  );
}

export default App;
```

### 4.5 Run the React App

```bash
npm start
```

---

## 📤 Step 5: Full Stack Connection

- **React** makes a **POST** request to `http://localhost:5000/submit`
- **Express** receives the request, validates data, and saves to **MySQL**
- **MySQL** stores the data in the `submissions` table

---

## ✅ Step 6: Testing End-to-End

1. Start XAMPP's Apache and MySQL

2. Run backend server:

   ```bash
   cd backend
   node index.js
   ```

3. Run frontend app:

   ```bash
   cd frontend
   npm start
   ```

4. Open browser: `http://localhost:3000`

5. Fill the form and submit.

6. Go to `http://localhost/phpmyadmin`, select the `form_data` database, and click the `submissions` table.

7. You should see the submitted data!

---

## 🧪 Optional: Use Axios Instead of Fetch (React)

```bash
npm install axios
```

Update `handleSubmit` in `Form.js`:

```js
import axios from "axios";

// Inside handleSubmit
await axios.post("http://localhost:5000/submit", form);
```

---

## 📄 SQL Summary

```sql
-- Create database
CREATE DATABASE form_data;

-- Use the database
USE form_data;

-- Create table
CREATE TABLE submissions (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100),
  message TEXT
);
```

---

## 📂 Folder Recap

```
form-app/
├── backend/
│   ├── index.js        # Express server
│   ├── db.js           # MySQL connection
│   └── package.json
├── frontend/
│   └── src/
│       ├── Form.js     # Form component
│       ├── App.js
│       └── index.js
└── README.md
```

---

## 🎉 You Did It!

You just built a **complete full-stack web application** that:

- Uses React for the frontend
- Uses Bootstrap for styling
- Sends form data to a Node.js + Express backend
- Saves data into a MySQL database (via XAMPP)
