# **Basic CRUD app** setup using **Vite + TypeScript + React + DaisyUI (TailwindCSS)**

---

### ✅ Features

* Vite for fast development
* TypeScript for type safety
* ReactJS for UI
* TailwindCSS + DaisyUI for styling
* CRUD operations on a list of users (in-memory; no backend)

---

## 🧱 Project Setup

### 1. **Create Vite App**

```bash
npm create vite@latest my-crud-app -- --template react-ts
cd my-crud-app
```

### 2. **Install Dependencies**

```bash
npm install
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
npm install daisyui
```

### 3. **Configure Tailwind**

Edit `tailwind.config.ts`:

```ts
import type { Config } from 'tailwindcss'

const config: Config = {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [require("daisyui")],
}

export default config;
```

Update `src/index.css`:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

## ⚙️ Build the CRUD UI

### File Structure:

```
src/
  components/
    UserForm.tsx
    UserTable.tsx
  App.tsx
  types.ts
```

### 1. **`types.ts`**

```ts
export interface User {
  id: number;
  name: string;
  email: string;
}
```

---

### 2. **`UserForm.tsx`**

```tsx
import React, { useState, useEffect } from 'react';
import { User } from '../types';

interface Props {
  onSave: (user: Omit<User, 'id'>, id?: number) => void;
  editingUser?: User | null;
  onCancel?: () => void;
}

export default function UserForm({ onSave, editingUser, onCancel }: Props) {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');

  useEffect(() => {
    if (editingUser) {
      setName(editingUser.name);
      setEmail(editingUser.email);
    }
  }, [editingUser]);

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    onSave({ name, email }, editingUser?.id);
    setName('');
    setEmail('');
  };

  return (
    <form onSubmit={handleSubmit} className="space-y-4">
      <input
        type="text"
        placeholder="Name"
        className="input input-bordered w-full"
        value={name}
        onChange={(e) => setName(e.target.value)}
        required
      />
      <input
        type="email"
        placeholder="Email"
        className="input input-bordered w-full"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        required
      />
      <div className="flex gap-2">
        <button type="submit" className="btn btn-primary">
          {editingUser ? 'Update' : 'Add'}
        </button>
        {editingUser && (
          <button type="button" onClick={onCancel} className="btn btn-ghost">
            Cancel
          </button>
        )}
      </div>
    </form>
  );
}
```

---

### 3. **`UserTable.tsx`**

```tsx
import React from 'react';
import { User } from '../types';

interface Props {
  users: User[];
  onEdit: (user: User) => void;
  onDelete: (id: number) => void;
}

export default function UserTable({ users, onEdit, onDelete }: Props) {
  return (
    <div className="overflow-x-auto">
      <table className="table w-full">
        <thead>
          <tr>
            <th>Name</th>
            <th>Email</th>
            <th className="text-right">Actions</th>
          </tr>
        </thead>
        <tbody>
          {users.length === 0 ? (
            <tr>
              <td colSpan={3}>No users</td>
            </tr>
          ) : (
            users.map((user) => (
              <tr key={user.id}>
                <td>{user.name}</td>
                <td>{user.email}</td>
                <td className="text-right space-x-2">
                  <button
                    className="btn btn-sm btn-warning"
                    onClick={() => onEdit(user)}
                  >
                    Edit
                  </button>
                  <button
                    className="btn btn-sm btn-error"
                    onClick={() => onDelete(user.id)}
                  >
                    Delete
                  </button>
                </td>
              </tr>
            ))
          )}
        </tbody>
      </table>
    </div>
  );
}
```

---

### 4. **`App.tsx`**

```tsx
import React, { useState } from 'react';
import { User } from './types';
import UserForm from './components/UserForm';
import UserTable from './components/UserTable';

export default function App() {
  const [users, setUsers] = useState<User[]>([]);
  const [editingUser, setEditingUser] = useState<User | null>(null);

  const addOrUpdateUser = (user: Omit<User, 'id'>, id?: number) => {
    if (id != null) {
      setUsers((prev) =>
        prev.map((u) => (u.id === id ? { ...u, ...user } : u))
      );
      setEditingUser(null);
    } else {
      const newUser: User = {
        id: Date.now(),
        ...user,
      };
      setUsers((prev) => [...prev, newUser]);
    }
  };

  const deleteUser = (id: number) => {
    setUsers((prev) => prev.filter((u) => u.id !== id));
  };

  const editUser = (user: User) => {
    setEditingUser(user);
  };

  return (
    <div className="container mx-auto p-4 max-w-xl">
      <h1 className="text-2xl font-bold mb-4">User Management</h1>
      <UserForm
        onSave={addOrUpdateUser}
        editingUser={editingUser}
        onCancel={() => setEditingUser(null)}
      />
      <div className="mt-8">
        <UserTable users={users} onEdit={editUser} onDelete={deleteUser} />
      </div>
    </div>
  );
}
```

---

## 🚀 Run the App

```bash
npm run dev
```

You now have a fully functioning CRUD app using **Vite + React + TypeScript + DaisyUI**!
