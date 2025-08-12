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

```ts
// formSchema.ts
import { z } from "zod";

export const formSchema = z.object({
  fullName: z.string().min(1, "Full name is required"),
  email: z.string().email("Invalid email address"),
  country: z.string().min(1, "Please select your country"),
  gender: z.enum(["male", "female", "other"], { required_error: "Select your gender" }),
  hobbies: z.array(z.string()).min(1, "Select at least one hobby"),
  resume: z
    .instanceof(FileList)
    .refine((files) => files.length > 0, "Please upload a file")
    .refine((files) => files[0]?.size <= 2 * 1024 * 1024, "File must be under 2MB")
    .refine(
      (files) => ["application/pdf"].includes(files[0]?.type || ""),
      "Only PDF files are allowed"
    ),
});

export type FormData = z.infer<typeof formSchema>;
```

---

## 3️⃣ The component with all input types

```tsx
import { useForm, Controller } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { formSchema, FormData } from "./formSchema";

export default function AllInputsForm() {
  const {
    register,
    handleSubmit,
    control,
    watch,
    formState: { errors, isSubmitting },
  } = useForm<FormData>({
    resolver: zodResolver(formSchema),
  });

  const onSubmit = (data: FormData) => {
    console.log("Form submitted:", data);
    alert("Check console for form data");
  };

  // Watching hobbies to show dynamic behavior
  const selectedHobbies = watch("hobbies", []);

  return (
    <form onSubmit={handleSubmit(onSubmit)} noValidate>
      {/* TEXT INPUT */}
      <div>
        <label htmlFor="fullName">Full Name</label>
        <input
          id="fullName"
          type="text"
          {...register("fullName")}
          aria-invalid={!!errors.fullName}
        />
        {errors.fullName && <p role="alert">{errors.fullName.message}</p>}
      </div>

      {/* EMAIL INPUT */}
      <div>
        <label htmlFor="email">Email</label>
        <input
          id="email"
          type="email"
          {...register("email")}
          aria-invalid={!!errors.email}
        />
        {errors.email && <p role="alert">{errors.email.message}</p>}
      </div>

      {/* SELECT */}
      <div>
        <label htmlFor="country">Country</label>
        <select id="country" {...register("country")}>
          <option value="">-- Select --</option>
          <option value="us">United States</option>
          <option value="uk">United Kingdom</option>
          <option value="in">India</option>
        </select>
        {errors.country && <p role="alert">{errors.country.message}</p>}
      </div>

      {/* RADIO BUTTONS */}
      <div>
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
        {errors.gender && <p role="alert">{errors.gender.message}</p>}
      </div>

      {/* CHECKBOX GROUP */}
      <div>
        <p>Hobbies (pick at least one)</p>
        <label>
          <input
            type="checkbox"
            value="reading"
            {...register("hobbies")}
          /> Reading
        </label>
        <label>
          <input
            type="checkbox"
            value="sports"
            {...register("hobbies")}
          /> Sports
        </label>
        <label>
          <input
            type="checkbox"
            value="music"
            {...register("hobbies")}
          /> Music
        </label>
        {errors.hobbies && <p role="alert">{errors.hobbies.message}</p>}
        <p>Selected: {selectedHobbies.join(", ") || "None"}</p>
      </div>

      {/* FILE UPLOAD */}
      <div>
        <label htmlFor="resume">Upload Resume (PDF, max 2MB)</label>
        <input
          id="resume"
          type="file"
          accept="application/pdf"
          {...register("resume")}
        />
        {errors.resume && <p role="alert">{errors.resume.message}</p>}
      </div>

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? "Submitting…" : "Submit"}
      </button>
    </form>
  );
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
