# 🛠️ Full Warranty Form App with Node.js, Express & MySQL (Step-by-Step)

This guide walks you through building a warranty form application using:

* **Frontend**: HTML + Bootstrap + JavaScript (fetch API)
* **Backend**: Node.js + Express
* **Database**: MySQL
* **Extras**: .env config, routes, controller, DB connection module

---

## 📁 Project Structure

```
warranty-form-app/
├── backend/
│   ├── config/
│   │   └── db.js
│   ├── controllers/
│   │   └── warrantyController.js
│   ├── routes/
│   │   └── warrantyRoutes.js
│   ├── .env
│   ├── server.js
│   └── package.json
├── frontend/
│   ├── index.html
│   └── assets/
│       └── css/
│           └── bootstrap.min.css
```

---

## 🔧 Step 1: Database Setup (MySQL)

### SQL Script

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

## 🛠️ Step 2: Backend Setup

### 1. Navigate and initialize:

```bash
mkdir -p warranty-form-app/backend && cd warranty-form-app/backend
npm init -y
npm install express cors mysql2 dotenv
```

### 2. Create `.env`

File: `backend/.env`

```env
PORT=3000
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=
DB_NAME=warranty_db
CLIENT_URL=http://127.0.0.1:5500
```

### 3. MySQL DB connection

File: `backend/config/db.js`

```js
const mysql = require("mysql2");
const dotenv = require("dotenv");
dotenv.config();

const db = mysql.createConnection({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME,
});

db.connect((err) => {
  if (err) {
    console.error("❌ DB connection failed:", err.message);
  } else {
    console.log("✅ Connected to MySQL");
  }
});

module.exports = db;
```

### 4. Controller logic

File: `backend/controllers/warrantyController.js`

```js
const db = require("../config/db");

const submitWarrantyForm = (req, res) => {
  const {
    customer_name,
    customer_phone_number,
    customer_email,
    product_purchased,
    order_number,
    purchased_from,
  } = req.body;

  if (!customer_name || !customer_phone_number || !customer_email ||
      !product_purchased || !order_number || !purchased_from) {
    return res.status(400).send("❗ All fields are required");
  }

  const sql = `
    INSERT INTO warranty_requests
    (name, phone, email, product, order_number, purchased_from)
    VALUES (?, ?, ?, ?, ?, ?)
  `;

  db.query(sql, [
    customer_name,
    customer_phone_number,
    customer_email,
    product_purchased,
    order_number,
    purchased_from,
  ], (err) => {
    if (err) {
      console.error("❌ Insert error:", err);
      return res.status(500).send("❗ Database error");
    }
    res.send("✅ Record inserted successfully");
  });
};

module.exports = { submitWarrantyForm };
```

### 5. Route setup

File: `backend/routes/warrantyRoutes.js`

```js
const express = require("express");
const router = express.Router();
const { submitWarrantyForm } = require("../controllers/warrantyController");

router.post("/submit", submitWarrantyForm);

module.exports = router;
```

### 6. Main server file

File: `backend/server.js`

```js
const express = require("express");
const cors = require("cors");
const dotenv = require("dotenv");
const warrantyRoutes = require("./routes/warrantyRoutes");

dotenv.config();

const app = express();
const PORT = process.env.PORT || 3000;

app.use(cors({ origin: process.env.CLIENT_URL }));
app.use(express.urlencoded({ extended: true }));
app.use(express.json());

app.use("/", warrantyRoutes);

app.listen(PORT, () => {
  console.log(`🚀 Server running at http://localhost:${PORT}`);
});
```

---

## 🌐 Step 3: Frontend Setup

File: `frontend/index.html`

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
        <label for="customer_phone_number" class="form-label">Phone Number *</label>
        <input type="text" id="customer_phone_number" class="form-control" required />
      </div>

      <div class="mb-3">
        <label for="customer_email" class="form-label">Email *</label>
        <input type="email" id="customer_email" class="form-control" required />
      </div>

      <div class="mb-3">
        <label for="product_purchased" class="form-label">Product Purchased *</label>
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
      document.getElementById("warrantyForm").addEventListener("submit", function (e) {
        e.preventDefault();

        const data = {
          customer_name: document.getElementById("customer_name").value.trim(),
          customer_phone_number: document.getElementById("customer_phone_number").value.trim(),
          customer_email: document.getElementById("customer_email").value.trim(),
          product_purchased: document.getElementById("product_purchased").value,
          order_number: document.getElementById("order_number").value.trim(),
          purchased_from: document.getElementById("purchased_from").value,
        };

        for (let key in data) {
          if (!data[key]) {
            alert("Please fill all fields.");
            return;
          }
        }

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

## ▶️ Step 4: Run the Project

### ✅ Backend

```bash
cd backend
node server.js
```

### ✅ Frontend

Open `frontend/index.html` in the browser (or use Live Server).

---

## 🧪 Test Flow

1. Open the form.
2. Fill in all required fields.
3. Submit.
4. Data goes to Node.js backend.
5. Backend inserts data to MySQL.
6. You see an alert with success message.

---

