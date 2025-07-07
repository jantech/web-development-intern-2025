In JavaScript, an **array** is a special type of object used to store **multiple values in a single variable**. Each value (called an **element**) in an array has a **numeric index**, starting from `0`.

Arrays can hold any type of data: numbers, strings, objects, other arrays, etc.

---

### 🔹 Declaring an Array

```javascript
let fruits = ["apple", "banana", "cherry"];
```

* `fruits` is an array with 3 elements.
* The elements are strings.
* Indexes:

  * `fruits[0]` is `"apple"`
  * `fruits[1]` is `"banana"`
  * `fruits[2]` is `"cherry"`

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

* `length`: returns the number of elements.

```javascript
console.log(fruits.length); // 3
```

* `push()`: adds an element to the end.

```javascript
fruits.push("mango");
console.log(fruits); // ["apple", "blueberry", "cherry", "mango"]
```

* `pop()`: removes the last element.

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
fruits.forEach(function(fruit) {
  console.log(fruit);
});
```

---

### 🔹 Mixed Data Types in Arrays

```javascript
let mixed = [42, "hello", true, {name: "John"}, [1, 2, 3]];
```

This is valid in JavaScript—arrays can store different types.

---

### Summary

* Arrays hold multiple values in an ordered list.
* Elements are accessed using zero-based indexes.
* JavaScript arrays are dynamic—you can add, remove, and modify elements easily.

Let me know if you'd like to see examples of common operations like sorting or filtering arrays.
