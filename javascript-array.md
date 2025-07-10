In JavaScript, an **array** is a special type of object used to store **multiple values in a single variable**. Each value (called an **element**) in an array has a **numeric index**, starting from `0`.

Arrays can hold any type of data: numbers, strings, objects, other arrays, etc.

---

### 🔹 Declaring an Array

```javascript
let fruits = ["apple", "banana", "cherry"];
```

- `fruits` is an array with 3 elements.
- The elements are strings.
- Indexes:

  - `fruits[0]` is `"apple"`
  - `fruits[1]` is `"banana"`
  - `fruits[2]` is `"cherry"`

---

### 🔹 Accessing Array Elements

```javascript
console.log(fruits[0]); // Output: "apple"
```

---

### 🔹 Modifying Elements

```javascript
fruits[1] = "blueberry";
console.log(fruits); // ["apple", "blueberry", "cherry"]
```

---

### 🔹 Array Properties and Methods

- `length`: returns the number of elements.

```javascript
console.log(fruits.length); // 3
```

- `push()`: adds an element to the end.

```javascript
fruits.push("mango");
console.log(fruits); // ["apple", "blueberry", "cherry", "mango"]
```

- `pop()`: removes the last element.

```javascript
fruits.pop();
console.log(fruits); // ["apple", "blueberry", "cherry"]
```

---

### 🔹 Looping Through an Array

```javascript
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}
```

Or using `forEach`:

```javascript
fruits.forEach(function (fruit) {
  console.log(fruit);
});
```

---

### 🔹 Mixed Data Types in Arrays

```javascript
let mixed = [42, "hello", true, { name: "John" }, [1, 2, 3]];
```

This is valid in JavaScript—arrays can store different types.

---

### 🔹 JavaScript Array Basics

An **array** is a special variable that can hold multiple values:

```javascript
let fruits = ["apple", "banana", "cherry"];
```

---

### ✅ 1) Adding Elements

**Methods:** `push()`, `unshift()`, direct assignment

```javascript
let colors = ["red", "green"];

// Add to end
colors.push("blue"); // ['red', 'green', 'blue']

// Add to beginning
colors.unshift("yellow"); // ['yellow', 'red', 'green', 'blue']

// Add at a specific index
colors[2] = "orange"; // ['yellow', 'red', 'orange', 'blue']
```

---

### ✅ 2) Finding One Element

**Method:** `find()`, `indexOf()`, `includes()`

```javascript
let numbers = [10, 20, 30, 40];

// Find first number greater than 25
let found = numbers.find((num) => num > 25); // 30

// Find index of a number
let idx = numbers.indexOf(20); // 1

// Check if value exists
let exists = numbers.includes(40); // true
```

---

### ✅ 3) Finding Multiple Elements

**Method:** `filter()`

```javascript
let scores = [45, 75, 60, 30, 90];

// Find scores above 50
let highScores = scores.filter((score) => score > 50); // [75, 60, 90]
```

---

### ✅ 4) Removing Elements

**Methods:** `pop()`, `shift()`, `splice()`, `filter()`

```javascript
let items = ["a", "b", "c", "d"];

// Remove from end
items.pop(); // ['a', 'b', 'c']

// Remove from start
items.shift(); // ['b', 'c']

// Remove by index
items.splice(1, 1); // ['b'] (removes 1 element at index 1)

// Remove by condition
let nums = [1, 2, 3, 4];
let filtered = nums.filter((n) => n !== 2); // [1, 3, 4]
```

---

### ✅ 5) Emptying an Array

**Methods:** set length to 0, reassign to `[]`, or use `splice()`

```javascript
let data = [1, 2, 3];

// Method 1
data.length = 0;

// Method 2
data = [];

// Method 3
data.splice(0, data.length);
```
