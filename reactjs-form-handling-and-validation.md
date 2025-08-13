# **Form handling and validation in ReactJS**

### ✅ What You'll Learn

1. How to create a form in React
2. How to handle form state
3. How to validate inputs
4. How to show error messages
5. How to use a library like `react-hook-form` for easier form management

---

## 📄 1. Create a Simple Form (Without Libraries)

Let's build a basic form with name and email fields.

```jsx
import React, { useState } from 'react';

const SimpleForm = () => {
  const [form, setForm] = useState({ name: '', email: '' });
  const [errors, setErrors] = useState({});

  const handleChange = (e) => {
    setForm({ ...form, [e.target.name]: e.target.value });
  };

  const validate = () => {
    const newErrors = {};
    if (!form.name.trim()) newErrors.name = 'Name is required';
    if (!form.email.trim()) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(form.email)) {
      newErrors.email = 'Email is invalid';
    }
    return newErrors;
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    const validationErrors = validate();
    if (Object.keys(validationErrors).length > 0) {
      setErrors(validationErrors);
    } else {
      console.log('Form submitted:', form);
      setErrors({});
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>Name:</label>
        <input name="name" value={form.name} onChange={handleChange} />
        {errors.name && <p style={{ color: 'red' }}>{errors.name}</p>}
      </div>
      <div>
        <label>Email:</label>
        <input name="email" value={form.email} onChange={handleChange} />
        {errors.email && <p style={{ color: 'red' }}>{errors.email}</p>}
      </div>
      <button type="submit">Submit</button>
    </form>
  );
};

export default SimpleForm;
```

---

## ✅ Explanation

* `useState`: To manage form and error states.
* `handleChange`: Updates form fields on input.
* `validate`: Checks if fields are filled and email is valid.
* `handleSubmit`: Prevents page reload and handles validation.

---

## 🚀 2. Use `react-hook-form` for Easier Handling
Alright — let’s build a **full-featured React 19 + Vite + TypeScript** form that includes **all major input types**:

* **Text**
* **Email**
* **Select**
* **Radio buttons**
* **Checkboxes**
* **File upload**

We’ll use **React Hook Form** + **Zod** for validation (best practice for speed + type safety).
I’ll also annotate each part so you understand exactly why it’s there and how it works.

---

## 1️⃣ Install dependencies

```bash
npm install react-hook-form zod @hookform/resolvers
```

---

## 2️⃣ The Zod schema

We define the form’s structure and rules here.
This gives **runtime validation** + **compile-time TypeScript types**.

---

### `formSchema.ts`

```ts
import { z } from "zod";

// Zod schema for validation
export const formSchema = z.object({
  fullName: z.string().min(1, "Full name is required"),
  email: z.string().email("Invalid email address"),
  country: z.string().min(1, "Please select a country"),
  gender: z.enum(["male", "female", "other"], {
    required_error: "Please select your gender",
  }),
  hobbies: z.array(z.string()).min(1, "Pick at least one hobby"),
  resume: z
    .any()
    .refine(
      (file) => file instanceof FileList && file.length > 0,
      "Resume file is required"
    )
    .refine(
      (file) =>
        file instanceof FileList &&
        file.length > 0 &&
        file[0].type === "application/pdf",
      "Only PDF files are allowed"
    )
    .refine(
      (file) =>
        file instanceof FileList &&
        file.length > 0 &&
        file[0].size <= 2 * 1024 * 1024,
      "File size must be 2MB or less"
    ),
});

// ✅ Give this a different name to avoid clashing with the built-in DOM FormData
export type FormValues = z.infer<typeof formSchema>;
```

---

### **`AdvancedForm.tsx`**

Make sure you import the CSS file at the top:

```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { formSchema } from "./formSchema";
import type { FormValues } from "./formSchema";
import "./AdvancedForm.css"; // ✅ Import the CSS file

export default function AdvancedForm() {
  const {
    register,
    handleSubmit,
    watch,
    formState: { errors, isSubmitting },
  } = useForm<FormValues>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      hobbies: [],
    },
  });

  const onSubmit = (data: FormValues) => {
    console.log("Form submitted:", data);
    alert("Check console for form data");
  };

  const selectedHobbies = watch("hobbies", []);

  const renderError = (message: unknown) =>
    typeof message === "string" ? <p className="error">{message}</p> : null;

  return (
    <form className="advanced-form" onSubmit={handleSubmit(onSubmit)} noValidate>
      <div className="form-group">
        <label htmlFor="fullName">Full Name</label>
        <input
          id="fullName"
          type="text"
          {...register("fullName")}
          aria-invalid={errors.fullName ? "true" : "false"}
        />
        {renderError(errors.fullName?.message)}
      </div>

      <div className="form-group">
        <label htmlFor="email">Email</label>
        <input
          id="email"
          type="email"
          {...register("email")}
          aria-invalid={errors.email ? "true" : "false"}
        />
        {renderError(errors.email?.message)}
      </div>

      <div className="form-group">
        <label htmlFor="country">Country</label>
        <select id="country" {...register("country")}>
          <option value="">-- Select --</option>
          <option value="us">United States</option>
          <option value="uk">United Kingdom</option>
          <option value="in">India</option>
        </select>
        {renderError(errors.country?.message)}
      </div>

      <div className="form-group">
        <p>Gender</p>
        <label>
          <input type="radio" value="male" {...register("gender")} /> Male
        </label>
        <label>
          <input type="radio" value="female" {...register("gender")} /> Female
        </label>
        <label>
          <input type="radio" value="other" {...register("gender")} /> Other
        </label>
        {renderError(errors.gender?.message)}
      </div>

      <div className="form-group">
        <p>Hobbies (pick at least one)</p>
        <label>
          <input type="checkbox" value="reading" {...register("hobbies")} />{" "}
          Reading
        </label>
        <label>
          <input type="checkbox" value="sports" {...register("hobbies")} />{" "}
          Sports
        </label>
        <label>
          <input type="checkbox" value="music" {...register("hobbies")} /> Music
        </label>
        {renderError(errors.hobbies?.message)}
        <p className="selected">
          Selected: {selectedHobbies.join(", ") || "None"}
        </p>
      </div>

      <div className="form-group">
        <label htmlFor="resume">Upload Resume (PDF, max 2MB)</label>
        <input
          id="resume"
          type="file"
          accept="application/pdf"
          {...register("resume")}
        />
        {renderError(errors.resume?.message)}
      </div>

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? "Submitting…" : "Submit"}
      </button>
    </form>
  );
}
```

---

### **`AdvancedForm.css`**

```css
.advanced-form {
  max-width: 500px;
  margin: 2rem auto;
  padding: 1.5rem;
  background: #fafafa;
  border-radius: 8px;
  border: 1px solid #ddd;
  font-family: Arial, sans-serif;
}

.form-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 1.25rem;
  text-align: left;
}

label {
  margin-bottom: 0.35rem;
  font-weight: 600;
}

input[type="text"],
input[type="email"],
input[type="file"],
select {
  padding: 0.5rem;
  border-radius: 4px;
  border: 1px solid #bbb;
  font-size: 1rem;
}

input[type="radio"],
input[type="checkbox"] {
  margin-right: 0.4rem;
}

button {
  background-color: #4cafef;
  color: white;
  padding: 0.6rem 1.2rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 1rem;
}

button:disabled {
  background-color: #a0c4dd;
  cursor: not-allowed;
}

.error {
  color: #d9534f;
  font-size: 0.875rem;
  margin-top: 0.25rem;
}

.selected {
  font-size: 0.9rem;
  color: #555;
}
```

---

## 4️⃣ Explanation of each section

### **Text input**

* Simple `type="text"` with `register("fieldName")`.
* `aria-invalid` and `role="alert"` make it accessible for screen readers.

### **Email input**

* Same as text, but `type="email"` triggers native email keyboard on mobile.
* Zod’s `.email()` ensures correct format.

### **Select**

* Use a `<select>` with an empty default option to force selection.
* Zod’s `.min(1)` ensures it’s not empty.

### **Radio buttons**

* All share the same `register("gender")`.
* Zod’s `z.enum([...])` ensures exactly one of the allowed values is chosen.

### **Checkbox group**

* `value="..."` on each checkbox and same `register("hobbies")` field name means RHF will return an **array** of strings.
* Zod `.array(z.string()).min(1)` ensures at least one checkbox is selected.

### **File input**

* Special case: in RHF, file inputs return a `FileList`.
* Zod `.instanceof(FileList)` checks the type.
* `.refine(...)` checks:

  * At least 1 file uploaded.
  * File size limit.
  * File type restriction (PDF only).

---

## 5️⃣ Output & Submission

* On submit, RHF validates via Zod **before** running `onSubmit`.
* If valid, we log and alert; in production, you’d `fetch()` or `axios.post()` to your API.
* **Important:** Always validate again on the server.
