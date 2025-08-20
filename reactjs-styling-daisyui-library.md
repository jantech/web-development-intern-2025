# Styling React App with DaisyUI

* **Vite** (fast dev environment)
* **TypeScript** (typed JavaScript)
* **Tailwind CSS** (utility-first styling)
* **DaisyUI** (Tailwind component library)

---

## 🛠️ Step-by-Step Setup

### ✅ 1. Create the Project with Vite + React + TypeScript

Open your terminal and run:

```bash
npm create vite@latest my-app -- --template react-ts
```

Then go into the project folder:

```bash
cd my-app
```

Install dependencies:

```bash
npm install
```

---

### ✅ 2. Install Tailwind CSS and daisyUI

```bash
npm install tailwindcss@latest @tailwindcss/vite@latest daisyui@latest
```
Now add Tailwind CSS to Vite config
Now update your `vite.config.ts`:

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite';

// https://vite.dev/config/
export default defineConfig({
  plugins: [
    tailwindcss(),
    react(),
  ],
})

```

Create a `src/index.css` file and add:

```css
@import "tailwindcss";
@plugin "daisyui";
```

Now import this CSS file in `src/main.tsx`:

```tsx
import './index.css'
```

---

### ✅ 4. Create a Sample Component

Create a new file: `src/components/Welcome.tsx`

```tsx
import React from 'react';

const Welcome: React.FC = () => {
  return (
    <div className="flex flex-col items-center justify-center min-h-screen bg-base-200">
      <h1 className="text-4xl font-bold text-primary">Welcome to React + Vite + TS!</h1>
      <p className="text-lg text-base-content mt-4">Styled with Tailwind CSS + DaisyUI</p>
      <button className="btn btn-primary mt-6">Click Me</button>
    </div>
  );
};

export default Welcome;
```

---

### ✅ 5. Use the Component in `App.tsx`

Replace the content of `src/App.tsx` with:

```tsx
import React from 'react';
import Welcome from './components/Welcome';

function App() {
  return <Welcome />;
}

export default App;
```

---

### ✅ 6. Run the App

Start the development server:

```bash
npm run dev
```

You should see a nice centered heading and button styled with DaisyUI and Tailwind.

---

## 🧠 How It Works

| Technology       | Role                             |
| ---------------- | -------------------------------- |
| **Vite**         | Fast build tool, replaces CRA    |
| **TypeScript**   | Adds static typing to JavaScript |
| **Tailwind CSS** | Utility-first CSS framework      |
| **DaisyUI**      | Prebuilt Tailwind UI components  |
| **React**        | Frontend UI library              |

---

## 🧩 Example Explained

```tsx
<div className="flex flex-col items-center justify-center min-h-screen bg-base-200">
```

* `flex flex-col`: Stack elements vertically.
* `items-center justify-center`: Center horizontally and vertically.
* `min-h-screen`: Full viewport height.
* `bg-base-200`: A DaisyUI background color.

```tsx
<h1 className="text-4xl font-bold text-primary">...</h1>
```

* `text-4xl`: Big text.
* `font-bold`: Bold text.
* `text-primary`: Theme primary color from DaisyUI.

```tsx
<button className="btn btn-primary">Click Me</button>
```

* `btn`: DaisyUI button style.
* `btn-primary`: Uses primary color theme.

---
