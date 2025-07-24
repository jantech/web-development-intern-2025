# Practical HTML exercises


### 🔹 1. Basic HTML Page

**Goal:** Create a simple HTML document with a title and a paragraph.

```html
<!DOCTYPE html>
<html>
<head>
    <title>My First Web Page</title>
</head>
<body>
    <h1>Welcome to My Website</h1>
    <p>This is a simple paragraph on my web page.</p>
</body>
</html>
```

---

### 🔹 2. Add an Image and a Link

**Goal:** Add an image and a link to your webpage.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Image and Link</title>
</head>
<body>
    <h1>Check out this cool photo!</h1>
    <img src="https://via.placeholder.com/300" alt="Placeholder Image">
    <p>Visit <a href="https://example.com" target="_blank">Example.com</a> for more info.</p>
</body>
</html>
```

---

### 🔹 3. Create a Form

**Goal:** Make a basic form with input fields and a submit button.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Simple Form</title>
</head>
<body>
    <h2>Sign Up</h2>
    <form action="/submit" method="post">
        <label for="name">Name:</label><br>
        <input type="text" id="name" name="name" placeholder="Enter your name"><br><br>

        <label for="email">Email:</label><br>
        <input type="email" id="email" name="email" placeholder="Enter your email"><br><br>

        <button type="submit">Submit</button>
    </form>
</body>
</html>
```

---

### 🔹 4. Create a Table

**Goal:** Display tabular data using HTML `<table>`.

```html
<!DOCTYPE html>
<html>
<head>
    <title>HTML Table</title>
</head>
<body>
    <h2>Student Scores</h2>
    <table border="1">
        <tr>
            <th>Name</th>
            <th>Math</th>
            <th>Science</th>
        </tr>
        <tr>
            <td>Alice</td>
            <td>90</td>
            <td>85</td>
        </tr>
        <tr>
            <td>Bob</td>
            <td>78</td>
            <td>88</td>
        </tr>
    </table>
</body>
</html>
```

---

### 🔹 5. Use Multimedia (Audio/Video)

**Goal:** Embed an audio and video element.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Multimedia</title>
</head>
<body>
    <h2>Video Example</h2>
    <video width="320" controls>
        <source src="sample.mp4" type="video/mp4">
        Your browser does not support the video tag.
    </video>

    <h2>Audio Example</h2>
    <audio controls>
        <source src="sample.mp3" type="audio/mpeg">
        Your browser does not support the audio element.
    </audio>
</body>
</html>
```

> 💡 Replace `"sample.mp4"` and `"sample.mp3"` with actual file paths on your system or use a URL.

---
Absolutely! Here's a collection of **diverse HTML practice examples** covering more areas of HTML, including layout, semantic tags, forms, multimedia, and interactivity.

---

## 🔸 Example 6: Semantic HTML Structure

**Goal:** Use semantic tags like `<header>`, `<main>`, `<article>`, `<footer>`.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Semantic Layout</title>
</head>
<body>
    <header>
        <h1>My Blog</h1>
        <nav>
            <a href="#">Home</a> |
            <a href="#">Articles</a> |
            <a href="#">Contact</a>
        </nav>
    </header>

    <main>
        <article>
            <h2>HTML Tips</h2>
            <p>Learn how to build websites using semantic HTML elements.</p>
        </article>
        <article>
            <h2>CSS Basics</h2>
            <p>Start styling your web pages with CSS.</p>
        </article>
    </main>

    <footer>
        <p>© 2025 My Blog</p>
    </footer>
</body>
</html>
```

---

## 🔸 Example 7: HTML Lists

**Goal:** Practice ordered, unordered, and nested lists.

```html
<!DOCTYPE html>
<html>
<head>
    <title>HTML Lists</title>
</head>
<body>
    <h2>Shopping List</h2>
    <ul>
        <li>Fruits
            <ul>
                <li>Apples</li>
                <li>Bananas</li>
            </ul>
        </li>
        <li>Vegetables</li>
        <li>Bread</li>
    </ul>

    <h2>Steps to Bake a Cake</h2>
    <ol>
        <li>Preheat the oven</li>
        <li>Mix ingredients</li>
        <li>Bake for 30 minutes</li>
    </ol>
</body>
</html>
```

---

## 🔸 Example 8: Contact Form with Fieldset

**Goal:** Use `<fieldset>`, `<legend>`, and multiple input types.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Contact Us</title>
</head>
<body>
    <h2>Contact Form</h2>
    <form>
        <fieldset>
            <legend>Personal Information</legend>
            Name: <input type="text" name="name"><br><br>
            Email: <input type="email" name="email"><br><br>
        </fieldset>

        <fieldset>
            <legend>Your Message</legend>
            Subject: <input type="text" name="subject"><br><br>
            Message:<br>
            <textarea rows="5" cols="30"></textarea><br>
        </fieldset>

        <button type="submit">Send</button>
    </form>
</body>
</html>
```

---

## 🔸 Example 9: HTML with Inline CSS Styling

**Goal:** Practice inline CSS styles directly in HTML tags.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Styled Page</title>
</head>
<body style="font-family: Arial; background-color: #f0f0f0; padding: 20px;">
    <h1 style="color: darkblue;">Welcome!</h1>
    <p style="color: gray;">This paragraph is styled using inline CSS.</p>
</body>
</html>
```

---

## 🔸 Example 10: HTML Audio and Video with Controls

**Goal:** Embed multimedia content with multiple sources.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Media Demo</title>
</head>
<body>
    <h2>Watch This Video</h2>
    <video controls width="400">
        <source src="movie.mp4" type="video/mp4">
        <source src="movie.ogg" type="video/ogg">
        Your browser does not support the video tag.
    </video>

    <h2>Listen to This Audio</h2>
    <audio controls>
        <source src="music.mp3" type="audio/mpeg">
        Your browser does not support the audio element.
    </audio>
</body>
</html>
```

---

## 🔸 Example 11: Simple Web Layout Using `<div>` and Classes

**Goal:** Use `<div>` and classes to build a sectioned layout.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Div Layout</title>
    <style>
        .container { display: flex; }
        .box { width: 200px; height: 100px; margin: 10px; padding: 10px; color: white; }
        .box1 { background-color: tomato; }
        .box2 { background-color: teal; }
        .box3 { background-color: purple; }
    </style>
</head>
<body>
    <h1>Div Layout Example</h1>
    <div class="container">
        <div class="box box1">Box 1</div>
        <div class="box box2">Box 2</div>
        <div class="box box3">Box 3</div>
    </div>
</body>
</html>
```

---

## 🔸 Example 12: Registration Form with Input Validation Attributes

**Goal:** Practice form validation using HTML attributes like `required`, `pattern`, etc.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Register</title>
</head>
<body>
    <h2>Registration Form</h2>
    <form>
        Username: <input type="text" name="username" required><br><br>
        Password: <input type="password" name="password" required minlength="6"><br><br>
        Email: <input type="email" name="email" required><br><br>
        Phone: <input type="tel" pattern="[0-9]{10}" placeholder="10-digit number"><br><br>
        <input type="submit" value="Register">
    </form>
</body>
</html>
```

---

