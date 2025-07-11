## 🔹 What Is a Database?

A **database** is an organized collection of data, usually stored and accessed electronically. Think of it like a smart digital filing cabinet.

---

## 🔹 Key Terms to Know:

| Term         | Meaning                                                                   |
| ------------ | ------------------------------------------------------------------------- |
| **Database** | A collection of data organized in a structured way.                       |
| **Table**    | A collection of related data in rows and columns (like a spreadsheet).    |
| **Row**      | A single record in a table (e.g., one person’s info).                     |
| **Column**   | A data field (e.g., name, email, age).                                    |
| **SQL**      | Structured Query Language – the language used to interact with databases. |

---

## 🔹 Popular Database Systems:

- **MySQL** – widely used, open-source, great for web apps.
- **PostgreSQL** – powerful and also open-source, good for complex queries.
- **SQLite** – lightweight and easy for beginners.
- **MongoDB** – NoSQL database (document-based, not table-based).

---

## 🔹 Sample SQL Commands:

```sql
-- Create a table
CREATE TABLE Users (
  id INTEGER PRIMARY KEY,
  name TEXT,
  email TEXT
);

-- Insert data
INSERT INTO Users (name, email) VALUES ('Alice', 'alice@example.com');

-- View data
SELECT * FROM Users;

-- Update data
UPDATE Users SET name = 'Alicia' WHERE id = 1;

-- Delete data
DELETE FROM Users WHERE id = 1;
```

---

## 🔹 Free Tools to Practice:

- [SQLite Online](https://sqliteonline.com/)
- [SQLBolt](https://sqlbolt.com/)
- [W3Schools SQL Tutorial](https://www.w3schools.com/sql/)

---

## ✅ Step-by-Step: Practice SQL with SQLite Online

### 🔗 Open this site: [https://sqliteonline.com/](https://sqliteonline.com/)

Once open, follow these steps:

---

### 🧱 Step 1: Create a Table

Paste this into the left side (the SQL editor):

```sql
CREATE TABLE Users (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT NOT NULL
);
```

➡️ Then press **Run** (or Ctrl+Enter).

---

### ✍️ Step 2: Insert Some Data

Now paste this below your first command and run:

```sql
INSERT INTO Users (name, email) VALUES ('Alice', 'alice@example.com');
INSERT INTO Users (name, email) VALUES ('Bob', 'bob@example.com');
```

---

### 🔍 Step 3: View the Data

Now paste and run this to see what’s in your table:

```sql
SELECT * FROM Users;
```

You should see something like:

| id  | name  | email                                         |
| --- | ----- | --------------------------------------------- |
| 1   | Alice | [alice@example.com](mailto:alice@example.com) |
| 2   | Bob   | [bob@example.com](mailto:bob@example.com)     |

---

### ✏️ Step 4: Update a Record

```sql
UPDATE Users SET name = 'Robert' WHERE id = 2;
```

Then run `SELECT * FROM Users;` again to check.

---

### 🗑️ Step 5: Delete a Record

```sql
DELETE FROM Users WHERE id = 1;
```

Check again with `SELECT * FROM Users;`

---

## 🔗 What Is a One-to-Many Relationship?

In a **one-to-many** relationship:

- One record in **Table A** is related to **many records** in **Table B**.

**Example:**

- One **customer** can place **many orders**.
- One **author** can write **many books**.

---

## 🧱 Let’s Create Two Tables

We'll make:

- `Customers` (parent table)
- `Orders` (child table with a foreign key to Customers)

### SQL Code (You can run this at [sqliteonline.com](https://sqliteonline.com))

```sql
-- Create Customers table
CREATE TABLE Customers (
  customer_id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT NOT NULL
);

-- Create Orders table (has a foreign key to Customers)
CREATE TABLE Orders (
  order_id INTEGER PRIMARY KEY,
  order_date TEXT,
  amount REAL,
  customer_id INTEGER,
  FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);
```

---

## ✍️ Insert Sample Data

```sql
-- Insert Customers
INSERT INTO Customers (name, email) VALUES ('Alice', 'alice@example.com');
INSERT INTO Customers (name, email) VALUES ('Bob', 'bob@example.com');

-- Insert Orders
INSERT INTO Orders (order_date, amount, customer_id) VALUES ('2025-07-01', 50.00, 1);
INSERT INTO Orders (order_date, amount, customer_id) VALUES ('2025-07-02', 75.00, 1);
INSERT INTO Orders (order_date, amount, customer_id) VALUES ('2025-07-03', 100.00, 2);
```

---

## 🔍 SELECT with JOIN (Connect Both Tables)

### 1. Show orders **with customer names**

```sql
SELECT
  Orders.order_id,
  Orders.order_date,
  Orders.amount,
  Customers.name AS customer_name
FROM Orders
JOIN Customers ON Orders.customer_id = Customers.customer_id;
```

**Output:**

| order_id | order_date | amount | customer_name |
| -------- | ---------- | ------ | ------------- |
| 1        | 2025-07-01 | 50.00  | Alice         |
| 2        | 2025-07-02 | 75.00  | Alice         |
| 3        | 2025-07-03 | 100.00 | Bob           |

---

## 🔄 More JOIN Examples

### 2. Show all customers and their orders (including customers with no orders)

```sql
SELECT
  Customers.name,
  Orders.order_id,
  Orders.amount
FROM Customers
LEFT JOIN Orders ON Customers.customer_id = Orders.customer_id;
```

✅ This is a **LEFT JOIN** — it shows all customers even if they have no orders.

---

## 🔢 GROUPING: Total spent by each customer

```sql
SELECT
  Customers.name,
  SUM(Orders.amount) AS total_spent
FROM Customers
JOIN Orders ON Customers.customer_id = Orders.customer_id
GROUP BY Customers.name;
```

---

---

## 🎯 Your Turn (Practice Questions)

Try writing queries for:

1. Show orders **only** over \$70, with customer names.
2. Count how many orders each customer has.
3. Show the **latest order** for each customer.

---

Let’s write the SQL queries for the three tasks using the same `Customers` and `Orders` tables we created earlier.

---

## ✅ 1. **Show orders only over \$70, with customer names**

```sql
SELECT
  Orders.order_id,
  Orders.order_date,
  Orders.amount,
  Customers.name AS customer_name
FROM Orders
JOIN Customers ON Orders.customer_id = Customers.customer_id
WHERE Orders.amount > 70;
```

🎯 **What this does:** Shows only the orders where the `amount` is greater than `$70`, and includes the customer name.

---

## ✅ 2. **Count how many orders each customer has**

```sql
SELECT
  Customers.name,
  COUNT(Orders.order_id) AS order_count
FROM Customers
LEFT JOIN Orders ON Customers.customer_id = Orders.customer_id
GROUP BY Customers.name;
```

🎯 **What this does:**

- Joins `Customers` and `Orders`
- Groups results by each customer
- Counts how many orders they placed

📝 Using `LEFT JOIN` ensures customers with **zero orders** are still shown (count will be 0).

---

## ✅ 3. **Show the latest order for each customer**

```sql
SELECT
  Customers.name,
  MAX(Orders.order_date) AS latest_order_date
FROM Customers
JOIN Orders ON Customers.customer_id = Orders.customer_id
GROUP BY Customers.name;
```

🎯 **What this does:**

- Finds the **maximum date** (latest) from `Orders` for each customer
- Joins with `Customers` to show their name

📝 This will give just the **latest order date**, not full order details. If you want the full order (e.g. amount, order ID), that’s a bit more advanced — let me know if you’d like that version too.

---
