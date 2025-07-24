# Full HTML5 Tags Documentation
---

## Text Content Tags

### `<p>`
- **Description**: Represents a paragraph of text.
- **Common/Global Attributes**:
  - `class`: CSS class for styling.
  - `id`: Unique identifier.
  - `style`: Inline CSS styling.
  - `title`: Text to display on hover.
- **Example**:
  ```html
  <p>This is a paragraph of text.</p>

### `<h1>` to `<h6>`

* **Description**: Represents headings, with `<h1>` being the highest level.
* **Common/Global Attributes**: `class`, `id`, `style`, `title`
* **Example**:

  ```html
  <h1>Main Heading</h1>
  <h2>Subheading</h2>
  ```

### `<span>`

* **Description**: Used for grouping inline elements to apply styles or attributes.
* **Common/Global Attributes**: `class`, `id`, `style`, `title`
* **Example**:

  ```html
  <span class="highlight">highlighted text</span>
  ```

## Document Structure Tags

### `<html>`

* **Description**: Root element of an HTML document.
* **Common/Global Attributes**:

  * `lang`: Specifies the language of the document.
* **Example**:

  ```html
  <html lang="en"></html>
  ```

### `<head>`

* **Description**: Contains metadata, links to stylesheets, and scripts.
* **Common/Global Attributes**: `profile`
* **Example**:

  ```html
  <head>
      <title>Page Title</title>
  </head>
  ```

### `<body>`

* **Description**: Contains the content of an HTML document.
* **Common/Global Attributes**: `class`, `id`, `style`, `title`
* **Example**:

  ```html
  <body>
      <p>Page content goes here.</p>
  </body>
  ```

## Multimedia Tags

### `<img>`

* **Description**: Embeds an image.
* **Common/Global Attributes**:

  * `src`, `alt`, `width`, `height`
* **Example**:

  ```html
  <img src="image.jpg" alt="Description of image" width="300" height="200">
  ```

### `<video>`

* **Description**: Embeds a video.
* **Common/Global Attributes**:

  * `src`, `controls`, `autoplay`, `loop`, `muted`
* **Example**:

  ```html
  <video src="video.mp4" controls width="400">
      Your browser does not support the video tag.
  </video>
  ```

### `<audio>`

* **Description**: Embeds audio content.
* **Common/Global Attributes**: `src`, `controls`
* **Example**:

  ```html
  <audio src="audio.mp3" controls>
      Your browser does not support the audio tag.
  </audio>
  ```

## Form Tags

### `<form>`

* **Description**: Represents an HTML form for user input.
* **Common/Global Attributes**: `action`, `method`, `enctype`, `autocomplete`, `novalidate`
* **Example**:

  ```html
  <form action="/submit" method="post">
      <input type="text" name="username">
  </form>
  ```

### `<input>`

* **Description**: Creates interactive controls for web-based forms.
* **Common/Global Attributes**: `type`, `name`, `value`, `placeholder`, `required`, `disabled`, `readonly`
* **Example**:

  ```html
  <input type="text" name="fullname" placeholder="Enter your full name">
  ```

### `<textarea>`

* **Description**: Represents a multi-line text input control.
* **Common/Global Attributes**: `name`, `rows`, `cols`, `placeholder`, `required`, `disabled`, `readonly`
* **Example**:

  ```html
  <textarea name="message" rows="5" cols="30" placeholder="Enter your message"></textarea>
  ```

### `<button>`

* **Description**: Represents a clickable button.
* **Common/Global Attributes**: `type`, `disabled`
* **Example**:

  ```html
  <button type="submit">Submit</button>
  ```

### `<select>`

* **Description**: Represents a drop-down list.
* **Common/Global Attributes**: `name`, `disabled`, `multiple`
* **Example**:

  ```html
  <select name="options">
      <option value="1">Option 1</option>
      <option value="2">Option 2</option>
  </select>
  ```

### `<option>`

* **Description**: Represents an option in a drop-down list.
* **Common/Global Attributes**: `value`, `selected`
* **Example**:

  ```html
  <option value="1">Option 1</option>
  ```

### `<label>`

* **Description**: Represents a caption for a form element.
* **Common/Global Attributes**: `for`
* **Example**:

  ```html
  <label for="username">Username:</label>
  <input type="text" id="username" name="username">
  ```

### `<fieldset>`

* **Description**: Groups related elements in a form.
* **Common/Global Attributes**: `disabled`
* **Example**:

  ```html
  <fieldset>
      <legend>Account Information</legend>
      <label for="email">Email:</label>
      <input type="email" id="email" name="email">
  </fieldset>
  ```

### `<legend>`

* **Description**: Provides a caption for the `<fieldset>`.
* **Example**:

  ```html
  <legend>Personal Details</legend>
  ```

## Semantic Tags

### `<header>`

* **Description**: Represents introductory content or navigation links.
* **Common/Global Attributes**: `class`, `id`, `style`, `title`
* **Example**:

  ```html
  <header>
      <h1>Website Header</h1>
  </header>
  ```

### `<footer>`

* **Description**: Represents the footer of a section or document.
* **Common/Global Attributes**: `class`, `id`, `style`, `title`
* **Example**:

  ```html
  <footer>
      <p>© 2025 My Website</p>
  </footer>
  ```

## Table Tags

### `<table>`

* **Description**: Represents tabular data.
* **Common/Global Attributes**: `border`
* **Example**:

  ```html
  <table border="1">
      <tr>
          <th>Header 1</th>
          <th>Header 2</th>
      </tr>
      <tr>
          <td>Data 1</td>
          <td>Data 2</td>
      </tr>
  </table>
  ```

### `<tr>`

* **Description**: Represents a row in a table.
* **Common/Global Attributes**: `class`, `id`, `style`, `title`
* **Example**:

  ```html
  <tr><td>Row Cell</td></tr>
  ```

## Link Tags

### `<a>`

* **Description**: Defines a hyperlink.
* **Common/Global Attributes**: `href`, `target`, `title`
* **Example**:

  ```html
  <a href="https://www.example.com" target="_blank">Visit Example.com</a>
  ```

## List Tags

### `<ul>`

* **Description**: Represents an unordered list.
* **Common/Global Attributes**: `class`, `id`, `style`, `title`
* **Example**:

  ```html
  <ul>
      <li>Item 1</li>
      <li>Item 2</li>
  </ul>
  ```

### `<ol>`

* **Description**: Represents an ordered list.
* **Common/Global Attributes**: `class`, `id`, `style`, `title`
* **Example**:

  ```html
  <ol>
      <li>First item</li>
      <li>Second item</li>
  </ol>
  ```

### `<li>`

* **Description**: Represents a list item.
* **Common/Global Attributes**: `value`
* **Example**:

  ```html
  <li>List item content</li>
  ```
