## ✅ **Operators in JavaScript**

### 1. **Arithmetic Operators**

Used to perform arithmetic on numbers.

```javascript
let a = 10,
  b = 3;

console.log(a + b); // 13 (Addition)
console.log(a - b); // 7  (Subtraction)
console.log(a * b); // 30 (Multiplication)
console.log(a / b); // 3.33 (Division)
console.log(a % b); // 1  (Modulus - Remainder)
console.log(a ** b); // 1000 (Exponentiation)
```

---

### 2. **Assignment Operators**

Used to assign values to variables.

```javascript
let x = 5;

x += 3; // same as x = x + 3
console.log(x); // 8

x *= 2; // same as x = x * 2
console.log(x); // 16
```

---

### 3. **Comparison Operators**

Used to compare two values.

```javascript
let a = 5,
  b = 10;

console.log(a > b); // false
console.log(a < b); // true
console.log(a >= 5); // true
console.log(b <= 9); // false
```

---

### 4. **Equality Operators**

Compare values (and types if strict).

```javascript
console.log(5 == "5"); // true (loose equality - type coercion)
console.log(5 === "5"); // false (strict equality - no type coercion)
console.log(5 != "5"); // false
console.log(5 !== "5"); // true
```

---

### 5. **Ternary Operator**

Short-hand for if..else.

```javascript
let age = 20;
let canVote = age >= 18 ? "Yes" : "No";
console.log(canVote); // "Yes"
```

---

### 6. **Logical Operators**

Combine boolean expressions.

```javascript
let a = true,
  b = false;

console.log(a && b); // false (AND)
console.log(a || b); // true  (OR)
console.log(!a); // false (NOT)
```

---

### 7. **Bitwise Operators**

Operate on 32-bit binary numbers.

```javascript
console.log(5 & 1); // 1 (AND)
console.log(5 | 1); // 5 (OR)
console.log(5 ^ 1); // 4 (XOR)
console.log(~5); // -6 (NOT)
console.log(5 << 1); // 10 (Left shift)
console.log(5 >> 1); // 2 (Right shift)
```
