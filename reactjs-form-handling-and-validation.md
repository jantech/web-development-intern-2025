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

You can install it:

```bash
npm install react-hook-form
```

Then update your form like this:

```jsx
import React from 'react';
import { useForm } from 'react-hook-form';

const HookForm = () => {
  const { register, handleSubmit, formState: { errors } } = useForm();

  const onSubmit = (data) => {
    console.log('Submitted data:', data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div>
        <label>Name:</label>
        <input {...register('name', { required: 'Name is required' })} />
        {errors.name && <p style={{ color: 'red' }}>{errors.name.message}</p>}
      </div>
      <div>
        <label>Email:</label>
        <input
          {...register('email', {
            required: 'Email is required',
            pattern: {
              value: /\S+@\S+\.\S+/,
              message: 'Email is invalid',
            },
          })}
        />
        {errors.email && <p style={{ color: 'red' }}>{errors.email.message}</p>}
      </div>
      <button type="submit">Submit</button>
    </form>
  );
};

export default HookForm;
```

---

# 3. **Handle all common input types** in React using **`react-hook-form`**, which simplifies state management and validation.

We'll cover:

✅ Text Input
✅ Select Dropdown
✅ Radio Buttons
✅ Checkbox
✅ File Upload

---

## 📦 Step 1: Install `react-hook-form`

```bash
npm install react-hook-form
```

---

## 🧠 Step 2: Full Example with All Input Types

```jsx
import React from 'react';
import { useForm } from 'react-hook-form';

const FullForm = () => {
  const {
    register,
    handleSubmit,
    watch,
    formState: { errors },
  } = useForm();

  const onSubmit = (data) => {
    // Handle file upload (for demo purpose only)
    const fileData = data.profilePicture?.[0];
    console.log('Form Data:', { ...data, profilePicture: fileData?.name });
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>

      {/* Text Input */}
      <div>
        <label>Name:</label>
        <input
          {...register('name', { required: 'Name is required' })}
          type="text"
        />
        {errors.name && <p style={{ color: 'red' }}>{errors.name.message}</p>}
      </div>

      {/* Email Input */}
      <div>
        <label>Email:</label>
        <input
          {...register('email', {
            required: 'Email is required',
            pattern: { value: /\S+@\S+\.\S+/, message: 'Invalid email' },
          })}
          type="email"
        />
        {errors.email && <p style={{ color: 'red' }}>{errors.email.message}</p>}
      </div>

      {/* Select Input */}
      <div>
        <label>Gender:</label>
        <select {...register('gender', { required: 'Gender is required' })}>
          <option value="">Select...</option>
          <option value="male">Male</option>
          <option value="female">Female</option>
          <option value="other">Other</option>
        </select>
        {errors.gender && <p style={{ color: 'red' }}>{errors.gender.message}</p>}
      </div>

      {/* Radio Buttons */}
      <div>
        <label>Subscription Type:</label><br />
        <label>
          <input
            type="radio"
            value="basic"
            {...register('subscription', { required: 'Select a subscription' })}
          />
          Basic
        </label>
        <label>
          <input
            type="radio"
            value="premium"
            {...register('subscription')}
          />
          Premium
        </label>
        {errors.subscription && <p style={{ color: 'red' }}>{errors.subscription.message}</p>}
      </div>

      {/* Checkbox */}
      <div>
        <label>
          <input
            type="checkbox"
            {...register('agree', { required: 'You must agree to terms' })}
          />
          I agree to terms and conditions
        </label>
        {errors.agree && <p style={{ color: 'red' }}>{errors.agree.message}</p>}
      </div>

      {/* File Upload */}
      <div>
        <label>Upload Profile Picture:</label>
        <input
          type="file"
          {...register('profilePicture', {
            required: 'Please upload a file',
          })}
        />
        {errors.profilePicture && <p style={{ color: 'red' }}>{errors.profilePicture.message}</p>}
      </div>

      {/* Submit Button */}
      <div>
        <button type="submit">Submit</button>
      </div>
    </form>
  );
};

export default FullForm;
```

---

## 📝 Notes

| Input Type       | Key Notes                                                               |
| ---------------- | ----------------------------------------------------------------------- |
| `text` / `email` | Straightforward                                                         |
| `select`         | Use native `<select>` element                                           |
| `radio`          | Use same name for all radios in the group                               |
| `checkbox`       | Returns boolean (`true`/`false`)                                        |
| `file`           | Returns a `FileList`, so use `data.file[0]` to access the uploaded file |

---

## ✅ Output Example (on submit)

```js
{
  name: "John",
  email: "john@example.com",
  gender: "male",
  subscription: "premium",
  agree: true,
  profilePicture: File // object with file metadata
}
```

