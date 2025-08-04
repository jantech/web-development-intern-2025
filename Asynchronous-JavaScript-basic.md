# **Asynchronous JavaScript**

* **Callbacks**
* **Promises**
* **`.then()`, `.catch()`**
* **Async/Await**
* **Fetch API**
* **Error handling in async code**

---

## 🔁 1. **Callbacks**

### ➤ What is it?

A **callback** is a function passed as an argument to another function, executed later.

### 🔹 Example:

```javascript
function fetchData(callback) {
  setTimeout(() => {
    const data = "User data";
    callback(data);
  }, 1000);
}

function handleData(result) {
  console.log("Received:", result);
}

fetchData(handleData);
```

### 🔹 Step-by-Step:

1. `fetchData` takes a callback.
2. `setTimeout` simulates a delay (1s).
3. After 1s, `callback(data)` is called.
4. `handleData` logs the result.

### ⚠️ Problem:

Callbacks can lead to **callback hell** — nested, unreadable code.

---

## 🔁 2. **Promises**

### ➤ What is it?

A **Promise** represents a value that may be available now, later, or never.

### 🔹 Example:

```javascript
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const success = true;
      if (success) {
        resolve("User data");
      } else {
        reject("Failed to fetch data");
      }
    }, 1000);
  });
}
```

### 🔹 Step-by-Step:

1. `fetchData` returns a Promise.
2. After 1s:

   * If `success`, it calls `resolve()`.
   * If failure, it calls `reject()`.

---

## 🔁 3. **`.then()`, `.catch()`**

### ➤ What is it?

Used to handle **fulfilled** or **rejected** Promises.

### 🔹 Example:

```javascript
fetchData()
  .then(data => {
    console.log("Received:", data);
  })
  .catch(error => {
    console.error("Error:", error);
  });
```

### 🔹 Step-by-Step:

1. `.then()` handles the resolved value.
2. `.catch()` handles any rejection.

---

## 🔁 4. **Async/Await**

### ➤ What is it?

Syntactic sugar over Promises. Makes async code look synchronous.

### 🔹 Example:

```javascript
async function getData() {
  try {
    const data = await fetchData();
    console.log("Received:", data);
  } catch (error) {
    console.error("Error:", error);
  }
}

getData();
```

### 🔹 Step-by-Step:

1. `async` allows `await` inside.
2. `await fetchData()` waits for the Promise.
3. `try/catch` handles errors cleanly.

---

## 🔁 5. **Fetch API**

### ➤ What is it?

Used to make HTTP requests in the browser (returns a Promise).

### 🔹 Example:

```javascript
async function fetchUser() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users/1');
    
    if (!response.ok) {
      throw new Error("Network response was not ok");
    }

    const user = await response.json();
    console.log("User:", user);
  } catch (error) {
    console.error("Fetch error:", error.message);
  }
}

fetchUser();
```

### 🔹 Step-by-Step:

1. `fetch()` sends a GET request.
2. `await response` waits for the response.
3. Check if `response.ok` is `true`.
4. `await response.json()` parses the JSON.
5. Errors are caught in `catch`.

---

## 🔁 6. **Error Handling in Async Code**

### 🔹 Common Pattern:

```javascript
async function safeFetch(url) {
  try {
    const res = await fetch(url);

    if (!res.ok) {
      throw new Error(`HTTP error! Status: ${res.status}`);
    }

    const data = await res.json();
    console.log("Data:", data);
  } catch (err) {
    console.error("Handled error:", err.message);
  }
}

safeFetch("https://jsonplaceholder.typicode.com/posts/1");
```

### 🔹 Key Takeaways:

* Always use `try/catch` with `async/await`.
* Use `.catch()` with Promises.
* Check `response.ok` to catch HTTP-level errors.
* Log or handle errors to avoid silent failures.

---

# Let's walk through how **callback hell** can be refactored into **Promises**, then into **async/await**.

---

## 🔁 1. **Callback Hell Example**

### 😣 Problem: Nested callbacks become messy.

```javascript
function getUser(callback) {
  setTimeout(() => {
    console.log("Fetched user");
    callback({ id: 1, name: "Alice" });
  }, 1000);
}

function getPosts(userId, callback) {
  setTimeout(() => {
    console.log("Fetched posts for user:", userId);
    callback(["Post1", "Post2"]);
  }, 1000);
}

function getComments(post, callback) {
  setTimeout(() => {
    console.log("Fetched comments for:", post);
    callback(["Comment1", "Comment2"]);
  }, 1000);
}

// Callback hell
getUser(user => {
  getPosts(user.id, posts => {
    getComments(posts[0], comments => {
      console.log("Final data:", comments);
    });
  });
});
```

### 🔹 Output:

```
Fetched user
Fetched posts for user: 1
Fetched comments for: Post1
Final data: [ 'Comment1', 'Comment2' ]
```

---

## 🔁 2. **Refactor to Promises**

```javascript
function getUser() {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log("Fetched user");
      resolve({ id: 1, name: "Alice" });
    }, 1000);
  });
}

function getPosts(userId) {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log("Fetched posts for user:", userId);
      resolve(["Post1", "Post2"]);
    }, 1000);
  });
}

function getComments(post) {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log("Fetched comments for:", post);
      resolve(["Comment1", "Comment2"]);
    }, 1000);
  });
}

// Promise chain
getUser()
  .then(user => getPosts(user.id))
  .then(posts => getComments(posts[0]))
  .then(comments => console.log("Final data:", comments))
  .catch(error => console.error("Error:", error));
```

### ✅ Benefits:

* Flat structure
* Easier to read
* Centralized error handling

---

## 🔁 3. **Refactor to Async/Await**

```javascript
async function fetchAllData() {
  try {
    const user = await getUser();
    const posts = await getPosts(user.id);
    const comments = await getComments(posts[0]);

    console.log("Final data:", comments);
  } catch (error) {
    console.error("Error:", error);
  }
}

fetchAllData();
```

### ✅ Benefits:

* Looks synchronous
* Easier to follow logic
* Errors handled with `try/catch`

---

## 🧠 Summary

| Pattern         | Pros                         | Cons                         |
| --------------- | ---------------------------- | ---------------------------- |
| **Callback**    | Simple for single async task | Hard to read when nested     |
| **Promise**     | Cleaner, chainable           | Can get long chains          |
| **Async/Await** | Most readable, linear flow   | Needs `try/catch` for errors |

---


