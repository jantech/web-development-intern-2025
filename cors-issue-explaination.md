You're getting a **CORS error**, which is very common when using `fetch()` from a different origin (even `localhost` vs `127.0.0.1` counts as different origins).

---

## 🚫 Problem Recap:

When your HTML/JS runs at:

```
http://127.0.0.1:5500 (served by Live Server)
```

And tries to fetch data from:

```
http://localhost/intern-class/warrenty_server.php
```

The browser **blocks** the request due to **CORS policy**.

---

## ✅ Solution: Allow Cross-Origin Requests in PHP

You need to **tell the PHP server** to allow requests from other origins by adding this header:

### 🔧 **Fix: Add this to the very top of `warrenty_server.php`:**

```php
<?php
// Allow requests from any origin (or specify exact origin)
header("Access-Control-Allow-Origin: *");

// Optional: allow specific methods and headers
header("Access-Control-Allow-Methods: POST");
header("Access-Control-Allow-Headers: Content-Type");

...
// Your DB insert logic follows
```

---

## 🔐 Important Notes:

### ✅ Option 1: Allow Any Origin (for testing)

```php
header("Access-Control-Allow-Origin: *");
```

### ✅ Option 2: Allow Specific Origin (recommended for production)

```php
header("Access-Control-Allow-Origin: http://127.0.0.1:5500");
```

> This is safer, as it avoids allowing every website access to your backend.

---

## 📖 What is CORS?

**CORS (Cross-Origin Resource Sharing)** is a browser security feature that restricts web pages from making requests to a different domain than the one that served the web page.

By default, **browsers block AJAX requests** to different origins unless the server explicitly allows it via the `Access-Control-Allow-Origin` header.

---

## ✅ Final PHP Example (fixed):

```php
<?php
// Allow cross-origin for development
header("Access-Control-Allow-Origin: *");
header("Access-Control-Allow-Methods: POST");
header("Access-Control-Allow-Headers: Content-Type");

// DB config
$host = "localhost";
$user = "root";
$password = "";
$dbname = "warranty_db";

$conn = new mysqli($host, $user, $password, $dbname);
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

$name    = $_POST['customer_name'];
$phone   = $_POST['customer_phone_number'];
$email   = $_POST['customer_email'];
$product = $_POST['product_purchased'];
$order   = $_POST['order_number'];
$source  = $_POST['purchased_from'];

$stmt = $conn->prepare("INSERT INTO warranty_requests (name, phone, email, product, order_number, purchased_from) VALUES (?, ?, ?, ?, ?, ?)");
$stmt->bind_param("ssssss", $name, $phone, $email, $product, $order, $source);

if ($stmt->execute()) {
    echo "Record inserted successfully";
} else {
    echo "Error: " . $stmt->error;
}

$stmt->close();
$conn->close();
?>
```

---

## ✅ Bonus Tip for Live Server Users

If you serve HTML with VS Code's **Live Server ([http://127.0.0.1:5500](http://127.0.0.1:5500))** but your PHP is on **[http://localhost](http://localhost)**, the origins **do not match**, causing CORS.

To avoid this:

1. Either **run HTML & PHP from the same Apache server** (`http://localhost/project/index.html`)
2. Or set up **CORS headers** in your PHP files as shown above.
