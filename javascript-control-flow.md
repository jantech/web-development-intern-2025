## 🔁 **Control Flow in JavaScript**

### 1. **If..Else**

```javascript
let score = 85;

if (score >= 90) {
  console.log("Grade A");
} else if (score >= 80) {
  console.log("Grade B");
} else {
  console.log("Grade C");
}
// Output: "Grade B"
```

---

### 2. **Switch..Case**

```javascript
let day = 3;

switch (day) {
  case 1:
    console.log("Monday");
    break;
  case 2:
    console.log("Tuesday");
    break;
  case 3:
    console.log("Wednesday");
    break;
  default:
    console.log("Other day");
}
// Output: "Wednesday"
```

---

### 3. **For Loop**

```javascript
for (let i = 0; i < 5; i++) {
  console.log(i);
}
// Output: 0, 1, 2, 3, 4
```

---

### 4. **While Loop**

```javascript
let i = 0;
while (i < 3) {
  console.log(i);
  i++;
}
// Output: 0, 1, 2
```

---

### 5. **Do..While Loop**

Executes at least once.

```javascript
let i = 5;
do {
  console.log(i);
  i++;
} while (i < 3);
// Output: 5 (runs once before condition fails)
```

---

### 6. **Infinite Loops**

Be cautious—can crash your app.

```javascript
// while (true) {
//   console.log("Infinite");
// }
```

---

### 7. **For…in**

Loops through **object properties**.

```javascript
let person = { name: "Alice", age: 25 };

for (let key in person) {
  console.log(key + ": " + person[key]);
}
// Output: name: Alice, age: 25
```

---

### 8. **For…of**

Loops through **iterables** like arrays.

```javascript
let colors = ["red", "green", "blue"];

for (let color of colors) {
  console.log(color);
}
// Output: red, green, blue
```

---

### 9. **Break and Continue**

```javascript
for (let i = 1; i <= 5; i++) {
  if (i === 3) continue; // skips 3
  if (i === 5) break; // stops at 4
  console.log(i);
}
// Output: 1, 2, 4
```
