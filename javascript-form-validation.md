## ✅ 1. **Basic Form Validation in JavaScript**

Let's validate a form with:

- A required name field
- A valid email field
- A password of minimum 6 characters

---

### 🧱 HTML Form

```html
<form id="myForm">
  <label>Name:</label>
  <input type="text" id="name" required /><br />

  <label>Email:</label>
  <input type="email" id="email" required /><br />

  <label>Password:</label>
  <input type="password" id="password" required /><br />

  <button type="submit">Submit</button>
  <p id="message"></p>
</form>
```

---

### ⚙️ JavaScript: Form Validation

```javascript
document.getElementById("myForm").addEventListener("submit", function (e) {
  e.preventDefault(); // Prevent actual form submission

  // Get input values
  let name = document.getElementById("name").value.trim();
  let email = document.getElementById("email").value.trim();
  let password = document.getElementById("password").value.trim();
  let message = document.getElementById("message");

  // Validate fields
  if (name === "") {
    message.textContent = "Name is required.";
    message.style.color = "red";
    return;
  }

  if (!validateEmail(email)) {
    message.textContent = "Invalid email address.";
    message.style.color = "red";
    return;
  }

  if (password.length < 6) {
    message.textContent = "Password must be at least 6 characters.";
    message.style.color = "red";
    return;
  }

  // If all valid
  message.textContent = "Form submitted successfully!";
  message.style.color = "green";
});

// Helper function to validate email with RegEx
function validateEmail(email) {
  const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return re.test(email);
}
```

---

## 🧰 2. **Basic DOM Operations**

### ✅ Select Elements

```javascript
let heading = document.querySelector("h1");
let input = document.getElementById("name");
let allInputs = document.querySelectorAll("input");
```

### ✅ Modify Content

```javascript
heading.textContent = "Updated Title!";
document.getElementById("message").innerHTML = "<b>Hello</b>";
```

### ✅ Change Styles

```javascript
input.style.border = "2px solid blue";
document.body.style.backgroundColor = "#f0f0f0";
```

### ✅ Create and Add Elements

```javascript
let newPara = document.createElement("p");
newPara.textContent = "This is new!";
document.body.appendChild(newPara);
```

### ✅ Remove Elements

```javascript
newPara.remove();
```

---

## 🧪 Summary of What You Can Do

| Task               | Method/Property                      |
| ------------------ | ------------------------------------ |
| Get Element        | `getElementById`, `querySelector`    |
| Set Text/HTML      | `textContent`, `innerHTML`           |
| Add/Remove Classes | `classList.add/remove/toggle`        |
| Change Styles      | `element.style`                      |
| Create Element     | `document.createElement()`           |
| Append Element     | `appendChild()`                      |
| Event Handling     | `addEventListener("event", handler)` |

---

## 🎁 Bonus: Real-Time Validation (on input)

```javascript
document.getElementById("email").addEventListener("input", function () {
  if (!validateEmail(this.value)) {
    this.style.borderColor = "red";
  } else {
    this.style.borderColor = "green";
  }
});
```
