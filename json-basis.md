**JSON** (JavaScript Object Notation) is a lightweight, text-based format for storing and exchanging data. It's easy for humans to read and write, and easy for machines to parse and generate. JSON is widely used for APIs, configurations, and data interchange between systems (like between a web browser and a server).

---

### 🔧 **Basic Syntax Rules of JSON**

1. **Data is in name/value pairs**
   Example: `"name": "John"`
2. **Data is separated by commas**
3. **Curly braces `{}` hold objects**
4. **Square brackets `[]` hold arrays**
5. **Strings are in double quotes**
6. **Values can be**:

   - String (`"text"`)
   - Number (`123`)
   - Boolean (`true` or `false`)
   - Array (`[1, 2, 3]`)
   - Object (`{"key": "value"}`)
   - `null`

---

### 📄 **Example of a JSON Object**

```json
{
  "name": "Alice",
  "age": 30,
  "isStudent": false,
  "skills": ["Python", "JavaScript", "SQL"],
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "zip": "10001"
  }
}
```

---

### 🧠 **Explanation**

- `"name": "Alice"` – a string value
- `"age": 30` – a number
- `"isStudent": false` – a boolean
- `"skills": [...]` – an array of strings
- `"address": {...}` – a nested object (an object inside another object)

---

### 📦 **Common Uses of JSON**

- **APIs**: Send/receive data in web apps
- **Configuration files**: `.json` config for apps and libraries
- **Data storage**: Simple databases or cache formats
