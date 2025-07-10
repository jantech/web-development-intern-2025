Perfect! Below is a complete **end-to-end project setup** for your warranty form using:

- ✅ Frontend: HTML + JavaScript
- ✅ Backend: Node.js + Express
- ✅ Database: MySQL
- ✅ CORS Support
- ✅ Server-side Validation
- ✅ Fetch-based form submission

---

## 📁 Project Folder Structure

```
warranty-form-project/
│
├── backend/              # Node.js Express server
│   ├── server.js
│   └── package.json
│
├── frontend/             # HTML + JavaScript form
│   ├── index.html
│   └── assets/
│       └── css/
│           └── bootstrap.min.css
```

---

## 🔧 1. Backend: `server.js` (Node.js)

### 📂 `backend/server.js`

```js
const express = require("express");
const cors = require("cors");
const mysql = require("mysql2");

const app = express();
const PORT = 3000;

// Enable CORS for frontend origin
app.use(
  cors({
    origin: "http://127.0.0.1:5500",
  })
);

// Middleware to parse form data
app.use(express.urlencoded({ extended: true }));
app.use(express.json());

// Connect to MySQL
const db = mysql.createConnection({
  host: "localhost",
  user: "root",
  password: "", // Use your own MySQL password
  database: "warranty_db",
});

db.connect((err) => {
  if (err) {
    console.error("❌ DB connection failed:", err);
    return;
  }
  console.log("✅ Connected to MySQL");
});

// Handle form submission
app.post("/submit", (req, res) => {
  const {
    customer_name,
    customer_phone_number,
    customer_email,
    product_purchased,
    order_number,
    purchased_from,
  } = req.body;

  if (
    !customer_name ||
    !customer_phone_number ||
    !customer_email ||
    !product_purchased ||
    !order_number ||
    !purchased_from
  ) {
    return res.status(400).send("❗ All fields are required");
  }

  const sql = `
    INSERT INTO warranty_requests 
    (name, phone, email, product, order_number, purchased_from)
    VALUES (?, ?, ?, ?, ?, ?)
  `;

  db.query(
    sql,
    [
      customer_name,
      customer_phone_number,
      customer_email,
      product_purchased,
      order_number,
      purchased_from,
    ],
    (err, result) => {
      if (err) {
        console.error("❌ Insert error:", err);
        return res.status(500).send("❗ Database error");
      }
      res.send("✅ Record inserted successfully");
    }
  );
});

// Start server
app.listen(PORT, () => {
  console.log(`🚀 Server running at http://localhost:${PORT}`);
});
```

---

### 🗃️ MySQL Table Schema

Create your database and table:

```sql
CREATE DATABASE IF NOT EXISTS warranty_db;

USE warranty_db;

CREATE TABLE IF NOT EXISTS warranty_requests (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  phone VARCHAR(20),
  email VARCHAR(100),
  product VARCHAR(100),
  order_number VARCHAR(100),
  purchased_from VARCHAR(50),
  submitted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## 📦 Backend Setup Instructions

1. Inside `backend/` folder:

```bash
npm init -y
npm install express cors mysql2
node server.js
```

---

## 🌐 2. Frontend: `index.html`

### 📂 `frontend/index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Warranty Form</title>
    <link rel="stylesheet" href="assets/css/bootstrap.min.css" />
  </head>
  <body class="p-4">
    <h1 class="mb-4">Warranty Information</h1>

    <form id="warrantyForm">
      <div class="mb-3">
        <label for="customer_name" class="form-label">Name *</label>
        <input type="text" id="customer_name" class="form-control" required />
      </div>

      <div class="mb-3">
        <label for="customer_phone_number" class="form-label"
          >Phone Number *</label
        >
        <input
          type="text"
          id="customer_phone_number"
          class="form-control"
          required
        />
      </div>

      <div class="mb-3">
        <label for="customer_email" class="form-label">Email *</label>
        <input type="email" id="customer_email" class="form-control" required />
      </div>

      <div class="mb-3">
        <label for="product_purchased" class="form-label"
          >Product Purchased *</label
        >
        <select id="product_purchased" class="form-select" required>
          <option value="">Select a product</option>
          <option value="product1">Product 1</option>
          <option value="product2">Product 2</option>
          <option value="product3">Product 3</option>
        </select>
      </div>

      <div class="mb-3">
        <label for="order_number" class="form-label">Order Number *</label>
        <input type="text" id="order_number" class="form-control" required />
      </div>

      <div class="mb-3">
        <label for="purchased_from" class="form-label">Purchased From *</label>
        <select id="purchased_from" class="form-select" required>
          <option value="">Choose an option</option>
          <option value="amazon">Amazon</option>
          <option value="flipkart">Flipkart</option>
          <option value="offline">Offline</option>
          <option value="other">Other</option>
        </select>
      </div>

      <button type="submit" class="btn btn-primary">Submit</button>
      <button type="reset" class="btn btn-secondary">Reset</button>
    </form>

    <script>
      document
        .getElementById("warrantyForm")
        .addEventListener("submit", function (e) {
          e.preventDefault();

          // Collect form data
          const data = {
            customer_name: document
              .getElementById("customer_name")
              .value.trim(),
            customer_phone_number: document
              .getElementById("customer_phone_number")
              .value.trim(),
            customer_email: document
              .getElementById("customer_email")
              .value.trim(),
            product_purchased:
              document.getElementById("product_purchased").value,
            order_number: document.getElementById("order_number").value.trim(),
            purchased_from: document.getElementById("purchased_from").value,
          };

          // Client-side validation
          for (let key in data) {
            if (!data[key]) {
              alert("Please fill all fields.");
              return;
            }
          }

          // Submit via fetch
          fetch("http://localhost:3000/submit", {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify(data),
          })
            .then((res) => res.text())
            .then((msg) => alert(msg))
            .catch((err) => {
              console.error(err);
              alert("Submission failed.");
            });
        });
    </script>
  </body>
</html>
```

---

## ✅ How to Run the Entire Setup

### 1. **Run Backend**

```bash
cd backend
node server.js
```

### 2. **Run Frontend**

Open `frontend/index.html` with **Live Server** in VS Code or manually in your browser.

Ensure your form POSTs to:
**[http://localhost:3000/submit](http://localhost:3000/submit)**

---

## 🧪 Test Submission

1. Fill the form.
2. Submit.
3. ✅ Data goes to Node.js backend.
4. ✅ Node inserts into MySQL.
5. ✅ You get a success alert.
