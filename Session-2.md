# Session 2: React Fundamentals — JSX, Components, Props, Events & Lists

**Duration:** 4 hours (2h theory + 2h practice)
**Level:** Beginner
**Prerequisites:** Session 1 (TypeScript basics), HTML, CSS, JavaScript ES6+

---

> **How to use this session**
> - **💡 In simple words**: a plain-language explanation under each topic. Read it first, then the code.
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.

## Session Timeline

- **Part 1: React Foundations (30 min)** — what React is, library vs framework, SPA/CSR, ecosystem
- **Part 2: Setup & Project Structure (30 min)** — Node.js, Vite, project anatomy
- **Part 3: JSX Essentials (40 min)** — JSX rules, expressions, className/htmlFor, fragments
- **Part 4: Components (30 min)** — functional components, naming, composition
- **Part 5: Props (40 min)** — prop types, children, destructuring, interfaces, defaults
- **Part 6: Events (30 min)** — synthetic events, onClick/onChange/onSubmit, event object
- **Part 7: Conditional Rendering & Lists (40 min)** — if/ternary/&&, states, map(), keys
- **Practical Project: Product Listing App (remaining time)**

---

## Part 1: React Foundations

### What is React

> 💡 **In simple words:** React is a tool for building web pages out of small reusable pieces called **components**: a button, a card, a menu. You describe what the screen should look like for the current data, and React updates the page for you whenever the data changes.

React is a **JavaScript library** (not a framework) for building user interfaces, created by Meta. It uses a **component-based architecture** and a **virtual DOM** to efficiently update only what changed.

```tsx
function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

**Why React:** declarative UI, reusable components, virtual DOM performance, huge ecosystem, industry standard.

### Library vs Framework

> 💡 **In simple words:** A library is a toolbox: you pick a tool when you need it. A framework is a house plan: it decides the structure, and you fill in the rooms. React only handles the UI, so you choose other tools for pages, data and so on.

- **Library (React):** you call it when needed; handles only the view layer — you add routing/state/HTTP yourself
- **Framework (Next.js, Angular):** calls your code (inversion of control); batteries included

### SPA & CSR

> 💡 **In simple words:** On a normal website, every click loads a new page from the server. In an **SPA** the browser loads the app once, then JavaScript swaps the content: no white flash, and it feels like a phone app. **CSR** means the browser (the client) builds the HTML with JavaScript.

- **SPA (Single Page Application):** loads one HTML page; JavaScript swaps content without full reloads. Fast transitions, app-like feel; downsides: initial load size, SEO needs work.
- **CSR (Client-Side Rendering):** browser downloads minimal HTML + JS bundle; JS renders the UI and fetches data via APIs.

```tsx
// Typical CSR data fetching
function UserList() {
  const [users, setUsers] = useState([]);
  useEffect(() => {
    fetch('/api/users').then(r => r.json()).then(setUsers);
  }, []);
  return <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}
```

### React vs Vanilla JS vs Next.js

> 💡 **In simple words:** With vanilla JS you give step-by-step orders: create a button, find the text, change it (**imperative**). With React you describe the result, "the button shows the count", and React works out the steps (**declarative**). Next.js is a framework built on top of React that adds pages and server rendering.

```tsx
// Vanilla JS — imperative DOM manipulation
const button = document.createElement('button');
button.addEventListener('click', () => { count++; display.textContent = count; });

// React — declarative: describe the UI, React updates the DOM
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>Clicked {count} times</button>;
}
```

| Feature | React | Next.js |
|---|---|---|
| Type | Library | Framework on React |
| Routing | React Router | File-based, built-in |
| Rendering | CSR default | SSR / SSG / ISR |
| SEO | Manual | Built-in |
| Use when | Interactive SPAs | Production apps needing SEO |

### Ecosystem (pick what you need)

> 💡 **In simple words:** React doesn't do everything, so the community built extra libraries around it. You don't need them all. This course introduces each one when you need it.

- Routing: React Router · State: Redux, Zustand, Context · Forms: React Hook Form · HTTP: Axios/Fetch · Styling: Tailwind, CSS Modules · Testing: Vitest/Jest + RTL · Build: Vite

---

## Part 2: Setup & Project Structure

### Node.js + Vite

> 💡 **In simple words:** React runs in the browser, but the **tools** that build it run on your computer with Node.js. Vite creates the project, runs a local server while you work, and refreshes the page instantly every time you save.

React tooling runs on Node.js (npm comes with it). Install the **LTS** version from nodejs.org, then verify:

```bash
node --version   # v20.x
npm --version
```

Create a project with Vite (fast dev server, instant HMR, TS support):

```bash
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install
npm run dev      # dev server → http://localhost:5173
npm run build    # production build
npm run preview  # preview the build
```

### Project structure

> 💡 **In simple words:** You will spend almost all your time in `src/`. Most other files are configuration. `public/` is for files copied as they are; `src/assets/` is for files you `import` in your code.

```
my-app/
├── public/            # static assets served as-is (favicon, robots.txt)
├── src/
│   ├── assets/        # images/fonts that get processed/hashed
│   ├── components/    # reusable components
│   ├── hooks/         # custom hooks
│   ├── types/         # TS type definitions
│   ├── App.tsx        # root component
│   ├── main.tsx       # entry point — mounts React to the DOM
│   └── index.css
├── index.html
├── package.json       # metadata + scripts + dependencies
├── tsconfig.json
└── vite.config.ts
```

**public vs src/assets:** use `public/` for files needing absolute URLs (`/logo.png`, robots.txt); use `src/assets/` for images imported in code (hashed + optimized).

### Key files

> 💡 **In simple words:** `index.html` contains an empty `<div id="root">`. `main.tsx` finds that div and tells React to draw `<App />` inside it. `App.tsx` is your first component, the top of the component tree.

```tsx
// main.tsx — entry point
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './index.css';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

`React.StrictMode` adds extra dev-only checks. `App.tsx` is the root component — keep it focused on layout/providers, delegate logic to children.

```jsonc
// package.json — key sections
{
  "scripts": { "dev": "vite", "build": "tsc && vite build", "preview": "vite preview" },
  "dependencies": { "react": "^18.2.0", "react-dom": "^18.2.0" },
  "devDependencies": { "typescript": "^5.2.2", "vite": "^5.0.8" }
}
```

---

### ✍️ Your Turn: Part 2

**1.** Create a project called `my-react-app`. Fill the blanks:

```bash
npm create vite@latest my-react-app -- --template ____
cd my-react-app
npm ____
npm run ____          # then open http://localhost:5173
```

**2.** Complete `main.tsx`:

```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './____';

ReactDOM.createRoot(document.getElementById('____')!).render(
  <React.StrictMode>
    <____ />
  </React.StrictMode>
);
```

**3.** Delete everything in `App.tsx` and write a component that shows your name in an `<h1>` and your city in a `<p>`. Save and watch the browser update by itself.

**4.** Where do you put each file: (a) `favicon.ico` (b) a logo image you `import` inside a component (c) a reusable `Button.tsx`?

<details>
<summary>✅ Solution</summary>

```bash
npm create vite@latest my-react-app -- --template react-ts
cd my-react-app
npm install
npm run dev
```

```tsx
import App from './App';
ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

```tsx
// App.tsx
function App() {
  return (
    <div>
      <h1>Sara Ahmed</h1>
      <p>I live in Amman</p>
    </div>
  );
}
export default App;
```

4. (a) `public/` (b) `src/assets/` (c) `src/components/`

</details>

---

## Part 3: JSX Essentials

> 💡 **In simple words:** JSX lets you write HTML-like code inside JavaScript. It **looks** like HTML, but it is JavaScript underneath, so a few rules are different. For example, `className` replaces `class` because `class` is a reserved word in JavaScript. Curly braces `{}` are a window back into JavaScript: anything inside them is calculated.

JSX is HTML-like syntax inside JavaScript, transpiled to `React.createElement` calls. **Rules:**

1. Return a **single parent** element (or a Fragment)
2. `className` not `class`, `htmlFor` not `for` — camelCase attributes
3. **Self-close** void elements: `<img />`, `<input />`, `<br />`
4. `{}` embeds **expressions** (not statements)
5. Inline styles are objects: `style={{ fontSize: '16px' }}`
6. Comments: `{/* ... */}`

```tsx
function ProductCard({ name, price }: { name: string; price: number }) {
  const formatted = `$${price.toFixed(2)}`;
  return (
    <div className="card">
      <h2>{name}</h2>                      {/* variable */}
      <p>Price: {formatted}</p>            {/* computed */}
      <p>Tax: ${(price * 0.1).toFixed(2)}</p>
      <button onClick={() => alert('hi')}>Buy</button>
    </div>
  );
}
```

### Fragments

> 💡 **In simple words:** A component must return **one** parent element. If you don't want an extra `<div>` on the page, wrap the elements in `<>...</>`, an invisible wrapper.

Group elements without adding DOM nodes. Short syntax `<>`; use `<React.Fragment key={...}>` when you need keys (e.g., in lists of rows).

```tsx
function UserCard({ name, email }: { name: string; email: string }) {
  return (
    <>
      <h2>{name}</h2>
      <p>{email}</p>
    </>
  );
}
```

---

### ✍️ Your Turn: Part 3

```tsx
// 1. This code was copied from HTML. It has 6 JSX mistakes.
//    Find them all and fix them.
function Profile() {
  const name = "Lina";
  return (
    <div class="profile">
      <img src="avatar.png" alt="avatar">
      <label for="email">Email</label>
      <input id="email" type="email">
      <h2>name</h2>
    </div>
    <p>Welcome!</p>
  );
}

// 2. Fill the blanks using { } expressions
function Receipt() {
  const item = "Coffee";
  const price = 3.5;
  const quantity = 2;
  return (
    <div>
      <p>Item: ____</p>
      {/* price × quantity with 2 decimals → 7.00 */}
      <p>Total: $____</p>
      {/* make this text green with an inline style */}
      <p style={____}>Thank you!</p>
    </div>
  );
}
```

<details>
<summary>✅ Solution</summary>

```tsx
function Profile() {
  const name = "Lina";
  return (
    <>                                            {/* 6. one parent only → Fragment */}
      <div className="profile">                   {/* 1. class → className */}
        <img src="avatar.png" alt="avatar" />     {/* 2. self-close img */}
        <label htmlFor="email">Email</label>      {/* 3. for → htmlFor */}
        <input id="email" type="email" />         {/* 4. self-close input */}
        <h2>{name}</h2>                           {/* 5. use { } for variables */}
      </div>
      <p>Welcome!</p>
    </>
  );
}

function Receipt() {
  const item = "Coffee";
  const price = 3.5;
  const quantity = 2;
  return (
    <div>
      <p>Item: {item}</p>
      <p>Total: ${(price * quantity).toFixed(2)}</p>
      <p style={{ color: 'green' }}>Thank you!</p>
    </div>
  );
}
```

</details>

---

## Part 4: Functional Components

> 💡 **In simple words:** A component is a JavaScript function that returns JSX. You use it like your own HTML tag: `<UserProfile />`. Build pages the way you build with LEGO: small pieces combined into bigger ones.

A component is a function returning JSX. **PascalCase required** — lowercase is treated as an HTML tag.

```tsx
// ✅ correct          // ❌ treated as <userprofile> HTML element
function UserProfile() {}    function userProfile() {}
```

**Naming:** descriptive (`UserAuthenticationForm` not `Form`), PascalCase, avoid abbreviations.

### Composition

> 💡 **In simple words:** Composition means putting components inside other components. A `Card` doesn't need to know what's inside it: it just draws the frame, and whatever you put between `<Card>` and `</Card>` appears inside.

Build complex UIs from small pieces; prefer composition over inheritance.

```tsx
function Card({ children }: { children: React.ReactNode }) {
  return <div className="card">{children}</div>;
}

function App() {
  return (
    <Card>
      <h2>Card Title</h2>
      <p>Content</p>
    </Card>
  );
}
```

### Reusable components

> 💡 **In simple words:** Instead of writing five different buttons, write **one** `Button` and change it with props (color, size, disabled). Fix a bug once, and every button in the app is fixed.

Flexible props + sensible defaults = reuse. TypeScript interfaces enforce the contract.

```tsx
interface ButtonProps {
  children: React.ReactNode;
  onClick?: () => void;
  variant?: 'primary' | 'secondary' | 'danger';
  disabled?: boolean;
}

function Button({ children, onClick, variant = 'primary', disabled = false }: ButtonProps) {
  return (
    <button className={`btn btn-${variant}`} onClick={onClick} disabled={disabled}>
      {children}
    </button>
  );
}
```

---

### ✍️ Your Turn: Part 4

```tsx
// 1. TODO: create a Header component that returns:
//    <header><h1>My Shop</h1></header>

// 2. TODO: create a Footer component that returns:
//    <footer>© 2026 My Shop</footer>

// 3. Fill the blanks to use them
function App() {
  return (
    <>
      <____ />
      <main>Welcome to my shop!</main>
      <____ />
    </>
  );
}

// 4. This component does NOT appear on the page. Why? Fix it.
function menu() {
  return <nav>Home | About | Contact</nav>;
}
// used like this: <menu />
```

<details>
<summary>✅ Solution</summary>

```tsx
function Header() {
  return (
    <header>
      <h1>My Shop</h1>
    </header>
  );
}

function Footer() {
  return <footer>© 2026 My Shop</footer>;
}

function App() {
  return (
    <>
      <Header />
      <main>Welcome to my shop!</main>
      <Footer />
    </>
  );
}

// 4. Lowercase names are treated as HTML tags (<menu> is a real HTML tag).
//    Components must start with a capital letter:
function Menu() {
  return <nav>Home | About | Contact</nav>;
}
// <Menu />
```

</details>

---

## Part 5: Props

> 💡 **In simple words:** Props are the **inputs** of a component, like the arguments of a function or the attributes of an HTML tag. The parent decides the values and the child only reads them. Data always flows **down**, from parent to child.

Props pass data **parent → child** (one-way data flow). They are **read-only** in the child — changes come from the parent.

```tsx
function Greeting({ name }: { name: string }) {
  return <h1>Hello, {name}!</h1>;
}
<Greeting name="John" />
```

### Prop types

> 💡 **In simple words:** Text can go in quotes. Everything else (numbers, booleans, objects, arrays, functions) goes inside `{}`. When you pass a function, pass its **name** (`onSave={handleSave}`). Don't call it (`onSave={handleSave()}`).

```tsx
<Comp
  text="literal"               // string literal
  count={5}                    // number — needs {}
  isActive={true}              // boolean
  user={{ name: 'A' }}         // object — needs {}
  items={itemsArray}           // array
  onSave={handleSave}          // function — pass reference, don't call
  {...propsObject}             // spread
/>
```

**Function props** enable child → parent communication via callbacks:

```tsx
interface UserFormProps {
  onSubmit: (data: { name: string; email: string }) => void;
  onCancel: () => void;
}

function UserForm({ onSubmit, onCancel }: UserFormProps) {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  return (
    <form onSubmit={(e) => { e.preventDefault(); onSubmit({ name, email }); }}>
      <input value={name} onChange={(e) => setName(e.target.value)} placeholder="Name" />
      <input value={email} onChange={(e) => setEmail(e.target.value)} placeholder="Email" />
      <button type="submit">Submit</button>
      <button type="button" onClick={onCancel}>Cancel</button>
    </form>
  );
}
```

### children prop

> 💡 **In simple words:** `children` is a special prop: it holds whatever you put **between** the opening and closing tags of your component. It lets you build wrappers like Card, Modal or Layout.

Content between tags arrives as `children: React.ReactNode` — use it for wrapper components (Card, Modal, Layout).

### Destructuring + defaults

> 💡 **In simple words:** Instead of writing `props.message`, `props.type` and so on, we unpack the props right in the parameters: `{ message, type }`. Writing `type = 'info'` sets a **default value**, used when the parent doesn't send one.

```tsx
interface AlertProps {
  message: string;
  type?: 'success' | 'error' | 'warning' | 'info';
  dismissible?: boolean;
}

function Alert({ message, type = 'info', dismissible = true }: AlertProps) {
  return <div className={`alert alert-${type}`}>{message}</div>;
}
```

**Rules:** always define a `Props` interface, mark optionals with `?`, give defaults in destructuring, name booleans `is*/has*`.

---

### ✍️ Your Turn: Part 5

```tsx
// 1. Complete the props interface and the component
interface StudentCardProps {
  name: ____;
  age: ____;
  isGraduated?: ____;
}

function StudentCard({ name, age, isGraduated = ____ }: StudentCardProps) {
  return (
    <div className="card">
      <h3>{____}</h3>
      <p>Age: {____}</p>
      {/* TODO: show "Graduated ✅" when isGraduated is true, otherwise "Student 📚" */}
    </div>
  );
}

// 2. TODO: show 3 StudentCard components with different data.
//    At least one of them must be graduated.
function App() {
  return <div>{/* TODO */}</div>;
}

// 3. children: complete Box so this works:
//    <Box title="Notice"><p>Class starts at 9:00</p></Box>
interface BoxProps {
  title: string;
  children: ____;
}

function Box({ title, children }: BoxProps) {
  return (
    <section className="box">
      <h4>{title}</h4>
      {/* TODO: show the children here */}
    </section>
  );
}
```

<details>
<summary>✅ Solution</summary>

```tsx
interface StudentCardProps {
  name: string;
  age: number;
  isGraduated?: boolean;
}

function StudentCard({ name, age, isGraduated = false }: StudentCardProps) {
  return (
    <div className="card">
      <h3>{name}</h3>
      <p>Age: {age}</p>
      <p>{isGraduated ? 'Graduated ✅' : 'Student 📚'}</p>
    </div>
  );
}

function App() {
  return (
    <div>
      <StudentCard name="Ali" age={22} isGraduated={true} />
      <StudentCard name="Mona" age={19} />
      <StudentCard name="Omar" age={20} />
      <Box title="Notice">
        <p>Class starts at 9:00</p>
      </Box>
    </div>
  );
}

interface BoxProps {
  title: string;
  children: React.ReactNode;
}

function Box({ title, children }: BoxProps) {
  return (
    <section className="box">
      <h4>{title}</h4>
      {children}
    </section>
  );
}
```

</details>

---

## Part 6: Events

> 💡 **In simple words:** An event is something the user does: click, type, submit. You give React a function, and React calls it when the event happens. React wraps the browser's event in a "synthetic event" so it works the same in every browser.

React uses **synthetic events** — wrappers around native events for cross-browser consistency, delegated at the root.

### The big three

> 💡 **In simple words:** Use `onClick` for buttons, `onChange` for inputs (it runs on every key press), and `onSubmit` for forms. In a submit handler, always call `e.preventDefault()`, or the browser reloads the page.

```tsx
// onClick
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}

// onChange — controlled input
function SearchBox() {
  const [term, setTerm] = useState('');
  return <input value={term} onChange={(e) => setTerm(e.target.value)} />;
}

// onSubmit — always preventDefault
function LoginForm() {
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    // validate + submit
  };
  return <form onSubmit={handleSubmit}>{/* fields */}</form>;
}
```

### Event object

> 💡 **In simple words:** React passes an event object `e` to your handler. It describes what happened:
> - `e.target` is the element; for inputs, `e.target.value` is the text.
> - `preventDefault()` stops the browser's default action.
> - `stopPropagation()` stops the event from reaching the parent elements.

```tsx
const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
  e.preventDefault();       // stop default browser behavior
  e.stopPropagation();      // stop bubbling to parent handlers
  console.log(e.target);    // element that triggered
  console.log(e.currentTarget); // element with the handler
};
```

Common types: `React.MouseEvent`, `React.ChangeEvent<HTMLInputElement>`, `React.FormEvent`, `React.KeyboardEvent`, `React.FocusEvent`.

### Passing arguments to handlers

> 💡 **In simple words:** `onClick` needs a **function**. If you need to send a value (like an id), wrap the call in an arrow function: `onClick={() => deleteTask(id)}`. Without the arrow, the function would run immediately while the page is drawn.

```tsx
// Arrow wrapper (most common)
{tasks.map(task => (
  <li key={task.id} onClick={() => handleTaskClick(task.id)}>{task.title}</li>
))}

// With event + custom arg — event comes first in the wrapper
<button onClick={(e) => deleteTask(task.id, e)}>Delete</button>
```

---

### ✍️ Your Turn: Part 6

```tsx
import { useState } from 'react';
// useState keeps a value that can change. Session 3 explains it fully.

// 1. Like button: every click adds 1
function LikeButton() {
  const [likes, setLikes] = useState(0);
  return <button ____={() => setLikes(____)}>❤️ {likes}</button>;
}

// 2. Live greeting: show what the user types
function NameInput() {
  const [name, setName] = useState('');
  return (
    <div>
      <input value={name} onChange={(e) => setName(____)} placeholder="Your name" />
      <p>Hello, {name || 'stranger'}!</p>
    </div>
  );
}

// 3. Form: the page must NOT reload
function FeedbackForm() {
  const handleSubmit = (e: React.FormEvent) => {
    // TODO: stop the page reload
    alert('Thank you for your feedback!');
  };
  return (
    <form ____={handleSubmit}>
      <textarea placeholder="Your feedback" />
      <button type="submit">Send</button>
    </form>
  );
}

// 4. Passing arguments: clicking a button shows "You picked <color>"
function ColorPicker() {
  const colors = ['red', 'green', 'blue'];
  const pick = (color: string) => alert(`You picked ${color}`);
  return (
    <div>
      {colors.map(c => (
        <button key={c} onClick={____}>{c}</button>
      ))}
    </div>
  );
}

// 5. Bug: the alert appears when the page LOADS, not when you click. Why? Fix it.
// <button onClick={pick('red')}>Red</button>
```

<details>
<summary>✅ Solution</summary>

```tsx
function LikeButton() {
  const [likes, setLikes] = useState(0);
  return <button onClick={() => setLikes(likes + 1)}>❤️ {likes}</button>;
}

function NameInput() {
  const [name, setName] = useState('');
  return (
    <div>
      <input value={name} onChange={(e) => setName(e.target.value)} placeholder="Your name" />
      <p>Hello, {name || 'stranger'}!</p>
    </div>
  );
}

function FeedbackForm() {
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    alert('Thank you for your feedback!');
  };
  return (
    <form onSubmit={handleSubmit}>
      <textarea placeholder="Your feedback" />
      <button type="submit">Send</button>
    </form>
  );
}

function ColorPicker() {
  const colors = ['red', 'green', 'blue'];
  const pick = (color: string) => alert(`You picked ${color}`);
  return (
    <div>
      {colors.map(c => (
        <button key={c} onClick={() => pick(c)}>{c}</button>
      ))}
    </div>
  );
}

// 5. pick('red') CALLS the function right away during render.
//    Pass a function instead, so React calls it on click:
// <button onClick={() => pick('red')}>Red</button>
```

</details>

---

## Part 7: Conditional Rendering & Lists

### Three techniques

> 💡 **In simple words:** Conditional rendering means showing different UI depending on a condition, exactly like `if` in normal code.
> - `if` + `return`: completely different screens.
> - `? :`: choose between two things.
> - `&&`: show something, or nothing.

```tsx
// 1. if / early returns — best for distinct states (loading, error, not-found)
function StatusMessage({ status }: { status: 'loading' | 'success' | 'error' }) {
  if (status === 'loading') return <div>Loading...</div>;
  if (status === 'success') return <div>Success!</div>;
  return <div>Error occurred</div>;
}

// 2. Ternary — either/or inside JSX
{isLoggedIn ? <LogoutButton /> : <LoginButton />}

// 3. Logical && — render or nothing
{user.role === 'admin' && <AdminPanel />}
```

**Falsy-value trap:** `{count && <div>{count}</div>}` renders `0` when count is 0 — use `{count > 0 && ...}`.

### The three states every data UI needs

> 💡 **In simple words:** Data from the internet doesn't arrive instantly, and the request can fail. So every screen that loads data must be ready for: **still loading**, **it failed**, **it arrived but is empty**, and **it arrived with data**. Users should never see a blank page.

```tsx
function ProductList() {
  const [products, setProducts] = useState([]);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    fetch('/api/products')
      .then(r => { if (!r.ok) throw new Error('Failed'); return r.json(); })
      .then(setProducts)
      .catch(e => setError(e.message))
      .finally(() => setIsLoading(false));
  }, []);

  if (isLoading) return <LoadingSpinner message="Loading products..." />;
  if (error) return <ErrorState message={error} onRetry={fetchAgain} />;
  if (products.length === 0) return <EmptyState title="No products" />;
  return <div>{products.map(p => <ProductCard key={p.id} product={p} />)}</div>;
}
```

- **Loading:** spinner or skeleton screens — set loading=false in `finally`
- **Empty:** friendly message + call to action ("Start Shopping")
- **Error:** user-friendly message + retry button; never show raw technical errors

### Rendering lists with map()

> 💡 **In simple words:** To show a list, turn **each item** of the array into a piece of JSX with `.map()`. `map` returns a new array of JSX elements, and React draws them all.

```tsx
{items.map(item => (
  <Component key={item.id} data={item} />
))}
```

- `map()` returns a new array → immutable pattern; `forEach` returns nothing — don't use it for JSX
- Filter/sort **before** mapping: `products.filter(...).sort(...).map(...)`

```tsx
const filtered = products.filter(p => p.category === selected);
const sorted = [...filtered].sort((a, b) => a.price - b.price);   // copy before sort!
return sorted.map(p => <ProductCard key={p.id} product={p} />);
```

### Keys

> 💡 **In simple words:** A key is a unique name tag for each item in a list. When the list changes, React uses the keys to know which item was added, removed or moved. Use the item's **id**, never something that can change, like the item's position.

Keys let React identify items across renders — they must be **unique among siblings, stable, and from the data** (`item.id`).

**Why index keys break things:** when a list reorders or an item is inserted at the top, indices shift — React then pairs stale component state (edit mode, focus, input value) with the wrong item.

```tsx
// ❌ breaks on reorder/insert
{users.map((user, i) => <UserRow key={i} user={user} />)}

// ✅ stable identity
{users.map(user => <UserRow key={user.id} user={user} />)}
```

Index keys are acceptable **only** for static lists that never reorder/change and hold no component state.

---

### ✍️ Your Turn: Part 7

```tsx
interface Fruit {
  id: number;
  name: string;
  price: number;
}

const fruits: Fruit[] = [
  { id: 1, name: 'Apple', price: 3 },
  { id: 2, name: 'Banana', price: 1 },
  { id: 3, name: 'Mango', price: 5 },
  { id: 4, name: 'Orange', price: 2 },
];

// 1. Show every fruit
function FruitList() {
  return (
    <ul>
      {fruits.____(fruit => (
        <li key={____}>{fruit.name}: ${fruit.price}</li>
      ))}
    </ul>
  );
}

// 2. TODO: in FruitList, show "🔥 Cheap!" next to fruits with price < 2 (use &&)

// 3. TODO: create ExpensiveFruits that shows ONLY fruits with price > 2
//    Hint: .filter(...) first, then .map(...)

// 4. TODO: create FruitBox({ items }: { items: Fruit[] })
//    if items is empty → <p>No fruits today</p>
//    otherwise        → the list of names
//    Test it: <FruitBox items={[]} /> and <FruitBox items={fruits} />

// 5. if / early return
function OrderStatus({ status }: { status: 'pending' | 'shipped' | 'delivered' }) {
  // TODO: pending   → <p>⏳ Preparing your order</p>
  //       shipped   → <p>🚚 On the way</p>
  //       delivered → <p>✅ Delivered</p>
}
```

<details>
<summary>✅ Solution</summary>

```tsx
function FruitList() {
  return (
    <ul>
      {fruits.map(fruit => (
        <li key={fruit.id}>
          {fruit.name}: ${fruit.price}
          {fruit.price < 2 && <span> 🔥 Cheap!</span>}
        </li>
      ))}
    </ul>
  );
}

function ExpensiveFruits() {
  return (
    <ul>
      {fruits
        .filter(fruit => fruit.price > 2)
        .map(fruit => (
          <li key={fruit.id}>{fruit.name}</li>
        ))}
    </ul>
  );
}

function FruitBox({ items }: { items: Fruit[] }) {
  if (items.length === 0) {
    return <p>No fruits today</p>;
  }
  return (
    <ul>
      {items.map(item => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}

function OrderStatus({ status }: { status: 'pending' | 'shipped' | 'delivered' }) {
  if (status === 'pending') return <p>⏳ Preparing your order</p>;
  if (status === 'shipped') return <p>🚚 On the way</p>;
  return <p>✅ Delivered</p>;
}
```

</details>

---

## Practical Project: Product Listing App

Build a catalog demonstrating props, events, conditional rendering, and lists.

**Setup:**
```bash
npm create vite@latest product-listing -- --template react-ts
cd product-listing && npm install && npm run dev
```

**Structure:**
```
src/
├── types/product.ts      # Product interface
├── data/products.ts      # mock product array
├── components/
│   ├── ProductCard.tsx   # props + events + conditional badges
│   └── ProductList.tsx   # filter/sort/search + map + states
└── App.tsx
```

```ts
// src/types/product.ts
export interface Product {
  id: number;
  name: string;
  price: number;
  originalPrice?: number;
  description: string;
  category: string;
  image: string;
  rating: number;
  reviewCount: number;
  inStock: boolean;
  stockCount: number;
  discount?: number;
  featured?: boolean;
}
```

```tsx
// src/components/ProductCard.tsx — props, events, conditional rendering
import { Product } from '../types/product';

interface ProductCardProps {
  product: Product;
  onAddToCart: (id: number) => void;
  onToggleWishlist: (id: number) => void;
  isWishlisted: boolean;
}

function ProductCard({ product, onAddToCart, onToggleWishlist, isWishlisted }: ProductCardProps) {
  return (
    <div className={`product-card ${product.featured ? 'featured' : ''} ${!product.inStock ? 'out-of-stock' : ''}`}>
      <div className="product-image-container">
        <img src={product.image} alt={product.name} loading="lazy" />
        <button
          className={`wishlist-button ${isWishlisted ? 'active' : ''}`}
          onClick={(e) => { e.stopPropagation(); onToggleWishlist(product.id); }}
        >
          {isWishlisted ? '♥' : '♡'}
        </button>
        {product.discount && <span className="discount-badge">-{Math.round(product.discount * 100)}%</span>}
      </div>
      <div className="product-info">
        <span className="product-category">{product.category}</span>
        <h3 className="product-name">{product.name}</h3>
        <div className="product-rating">★ {product.rating} ({product.reviewCount})</div>
        <div className="product-price">
          {product.originalPrice && <span className="original-price">${product.originalPrice}</span>}
          <span className="current-price">${product.price}</span>
        </div>
        <p className={`stock-status ${product.inStock ? 'in-stock' : 'out-of-stock'}`}>
          {product.inStock ? `In Stock (${product.stockCount})` : 'Out of Stock'}
        </p>
        <button
          className={`add-to-cart-button ${!product.inStock ? 'disabled' : ''}`}
          disabled={!product.inStock}
          onClick={(e) => { e.stopPropagation(); onAddToCart(product.id); }}
        >
          {product.inStock ? 'Add to Cart' : 'Out of Stock'}
        </button>
      </div>
    </div>
  );
}
export default ProductCard;
```

```tsx
// src/components/ProductList.tsx — list rendering, filter/sort, states
import { useState } from 'react';
import { Product } from '../types/product';
import ProductCard from './ProductCard';

function ProductList({ products }: { products: Product[] }) {
  const [category, setCategory] = useState('all');
  const [sortBy, setSortBy] = useState('featured');
  const [search, setSearch] = useState('');
  const [wishlist, setWishlist] = useState<Set<number>>(new Set());
  const [cart, setCart] = useState<number[]>([]);

  const categories = ['all', ...new Set(products.map(p => p.category))];

  const filtered = products.filter(p =>
    (category === 'all' || p.category === category) &&
    p.name.toLowerCase().includes(search.toLowerCase())
  );

  const sorted = [...filtered].sort((a, b) => {
    if (sortBy === 'price-low') return a.price - b.price;
    if (sortBy === 'price-high') return b.price - a.price;
    if (sortBy === 'rating') return b.rating - a.rating;
    return (b.featured ? 1 : 0) - (a.featured ? 1 : 0);
  });

  const toggleWishlist = (id: number) =>
    setWishlist(prev => {
      const next = new Set(prev);
      next.has(id) ? next.delete(id) : next.add(id);
      return next;
    });

  return (
    <div className="product-list">
      <div className="product-controls">
        <input value={search} onChange={e => setSearch(e.target.value)} placeholder="Search products..." />
        <select value={category} onChange={e => setCategory(e.target.value)}>
          {categories.map(c => <option key={c} value={c}>{c === 'all' ? 'All Categories' : c}</option>)}
        </select>
        <select value={sortBy} onChange={e => setSortBy(e.target.value)}>
          <option value="featured">Featured</option>
          <option value="price-low">Price: Low to High</option>
          <option value="price-high">Price: High to Low</option>
          <option value="rating">Rating</option>
        </select>
      </div>

      <div className="product-stats">
        <span>{sorted.length} products</span>
        <span>{cart.length} in cart</span>
        <span>{wishlist.size} in wishlist</span>
      </div>

      {sorted.length === 0 ? (
        <div className="empty-state">
          <h3>No products found</h3>
          <button onClick={() => { setSearch(''); setCategory('all'); }}>Clear Filters</button>
        </div>
      ) : (
        <div className="product-grid">
          {sorted.map(p => (
            <ProductCard
              key={p.id}
              product={p}
              onAddToCart={(id) => setCart(prev => [...prev, id])}
              onToggleWishlist={toggleWishlist}
              isWishlisted={wishlist.has(p.id)}
            />
          ))}
        </div>
      )}
    </div>
  );
}
export default ProductList;
```

Style with CSS grid (`grid-template-columns: repeat(auto-fill, minmax(280px, 1fr))`), card hover effects, and badge styles — see original session for the full stylesheet.

---

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. Is React a library or a framework? What is the difference?
2. What is an SPA (Single Page Application)?
3. What does `npm run dev` do in a Vite project?
4. What is the job of `main.tsx`?
5. Write 3 differences between JSX and HTML.
6. Why must a component name start with a capital letter?
7. Can a child component change its own props? Who can change them?
8. What is the `children` prop? Name one component that would use it.
9. Why do we call `e.preventDefault()` inside `onSubmit`?
10. Why does every list item need a `key`? Why is the array index a bad key?

<details>
<summary>✅ Solution</summary>

1. A library. You call a library when you need it (React only builds the UI). A framework calls your code and includes everything (routing, data, and so on).
2. An app that loads one HTML page and then changes the content with JavaScript, without reloading the page.
3. It starts a development server (usually at `http://localhost:5173`) that updates the browser every time you save.
4. It is the entry point. It finds `<div id="root">` and renders `<App />` inside it.
5. `className` instead of `class`. `htmlFor` instead of `for`. Every tag must be closed (`<img />`). Use `{}` for JavaScript. Only one parent element.
6. Lowercase names are treated as normal HTML tags, so React won't render your component.
7. No, props are read-only. The parent passes new props.
8. Whatever you put between the opening and closing tags. Examples: `Card`, `Modal`, `Layout`.
9. A form submit reloads the page by default. `preventDefault()` stops that.
10. The key helps React know which item is which between renders. If you add, remove or reorder items, the index changes, so React can mix up the items.

</details>

### B. Predict the Output

Write what the browser shows for each snippet.

```tsx
// 1
const count = 0;
return <div>{count && <p>You have messages</p>}</div>;

// 2
const user = { name: 'Sam', isAdmin: false };
return <p>{user.isAdmin ? 'Admin' : 'Member'}: {user.name}</p>;

// 3
const items = ['A', 'B', 'C'];
return <ul>{items.map(i => <li key={i}>{i.toLowerCase()}</li>)}</ul>;

// 4
function Hello({ name = 'Guest' }: { name?: string }) {
  return <h1>Hi {name}</h1>;
}
// <Hello />  and  <Hello name="Nour" />
```

<details>
<summary>✅ Solution</summary>

1. It shows **`0`** (the falsy-value trap). Fix: `{count > 0 && <p>You have messages</p>}`
2. `Member: Sam`
3. A list with `a`, `b`, `c`
4. `Hi Guest` and `Hi Nour`

</details>

### C. Coding Tasks

**Task 1: Greeting (easy).** A `Greeting` component with props `name: string` and an optional `emoji` (default `"👋"`). `<Greeting name="Sara" />` shows `👋 Hello, Sara!`

**Task 2: Counter (easy).** Buttons `−`, `+` and `Reset`. The count can never go below 0: disable the `−` button when the count is 0.

**Task 3: Show / hide (easy).** A button that says "Show details" or "Hide details" and toggles a paragraph.

**Task 4: Todo list (medium).**

```tsx
interface Todo { id: number; title: string; done: boolean; }

const todos: Todo[] = [
  { id: 1, title: 'Install Node.js', done: true },
  { id: 2, title: 'Create Vite project', done: true },
  { id: 3, title: 'Build my first component', done: false },
];

// TODO: show every todo with map + key
// TODO: done todos get className "done" and a ✔ before the title
// TODO: above the list show "2 of 3 done"
```

**Task 5: Product card with a callback (medium).** `ProductCard` takes `name`, `price`, `inStock` and `onAddToCart`. When out of stock, show "Out of stock" and disable the button. The parent `Shop` counts how many items were added and shows `🛒 3`.

**Challenge: Contact search.** A list of contacts `{ id, name, phone }` and a search input. Filter by name (ignore upper/lower case), show `"X contact(s)"`, and show `No contacts found` when nothing matches.

<details>
<summary>✅ Solution: Tasks 1–5 and Challenge</summary>

```tsx
import { useState } from 'react';

// Task 1
interface GreetingProps { name: string; emoji?: string; }
function Greeting({ name, emoji = '👋' }: GreetingProps) {
  return <h2>{emoji} Hello, {name}!</h2>;
}

// Task 2
function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count - 1)} disabled={count === 0}>−</button>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}

// Task 3
function Details() {
  const [isOpen, setIsOpen] = useState(false);
  return (
    <div>
      <button onClick={() => setIsOpen(!isOpen)}>
        {isOpen ? 'Hide details' : 'Show details'}
      </button>
      {isOpen && <p>React is a library for building user interfaces.</p>}
    </div>
  );
}

// Task 4  (CSS: .done { text-decoration: line-through; color: gray; })
function TodoList() {
  const doneCount = todos.filter(t => t.done).length;
  return (
    <div>
      <p>{doneCount} of {todos.length} done</p>
      <ul>
        {todos.map(todo => (
          <li key={todo.id} className={todo.done ? 'done' : ''}>
            {todo.done && '✔ '}{todo.title}
          </li>
        ))}
      </ul>
    </div>
  );
}

// Task 5
interface ProductCardProps {
  name: string;
  price: number;
  inStock: boolean;
  onAddToCart: () => void;
}
function ProductCard({ name, price, inStock, onAddToCart }: ProductCardProps) {
  return (
    <div className="card">
      <h3>{name}</h3>
      <p>${price}</p>
      {!inStock && <p style={{ color: 'gray' }}>Out of stock</p>}
      <button disabled={!inStock} onClick={onAddToCart}>Add to cart</button>
    </div>
  );
}
function Shop() {
  const [cartCount, setCartCount] = useState(0);
  const add = () => setCartCount(cartCount + 1);
  return (
    <div>
      <p>🛒 {cartCount}</p>
      <ProductCard name="Laptop" price={999} inStock={true} onAddToCart={add} />
      <ProductCard name="Phone" price={499} inStock={false} onAddToCart={add} />
    </div>
  );
}

// Challenge
interface Contact { id: number; name: string; phone: string; }
const contacts: Contact[] = [
  { id: 1, name: 'Ahmad', phone: '0791111111' },
  { id: 2, name: 'Sara', phone: '0792222222' },
  { id: 3, name: 'Samer', phone: '0793333333' },
];
function ContactSearch() {
  const [search, setSearch] = useState('');
  const results = contacts.filter(c =>
    c.name.toLowerCase().includes(search.toLowerCase())
  );
  return (
    <div>
      <input value={search} onChange={e => setSearch(e.target.value)} placeholder="Search..." />
      <p>{results.length} contact(s)</p>
      {results.length === 0 ? (
        <p>No contacts found</p>
      ) : (
        <ul>
          {results.map(c => (
            <li key={c.id}>{c.name}: {c.phone}</li>
          ))}
        </ul>
      )}
    </div>
  );
}
```

</details>

---

## Review

### Key Takeaways

- React = library; JSX = JS with HTML-like syntax (`className`, `{}` expressions, self-closing tags, one parent or Fragment)
- Components are PascalCase functions; compose them with `children`
- Props flow one-way (parent→child); type them with interfaces; defaults in destructuring
- Events are synthetic; `preventDefault` on submit; pass args with arrow wrappers
- Conditional rendering: `if` early-returns for states, ternary for either/or, `&&` for optional (beware `0`)
- Lists: `map()` + stable unique keys — never index keys on dynamic lists
- Every data UI needs loading / error / empty states

### 15 Review Questions

1. What is React and why is it popular? 2. Library vs framework? 3. What is an SPA / CSR? 4. Why Node.js for React dev? 5. `src` vs `public` folders? 6. Role of `main.tsx`? 7. JSX vs HTML differences? 8. Why `className` not `class`? 9. What are props / one-way data flow? 10. Why TypeScript interfaces for props? 11. `children` prop use case? 12. Synthetic events vs native? 13. When `if` vs ternary vs `&&`? 14. Why `map()` not `forEach`? 15. Why are index keys problematic?

### Interview Questions

1. **Virtual DOM?** Lightweight JS copy of the DOM; React diffs old vs new and updates only changed nodes.
2. **State vs props?** Props = immutable inputs from parent; state = internal mutable data via `useState`.
3. **Controlled vs uncontrolled forms?** Controlled bind input `value` to state via `onChange` (preferred); uncontrolled read via refs.
4. **Best practices for conditional rendering?** Right operator per case, early returns, handle all states, extract complex conditions.
5. **Optimize a 10k-item list?** Virtualization (react-window), stable keys, `React.memo`, pagination, debounced search.

### Exercises

1. **Registration form** — name/email/password/confirm with live validation + disabled submit until valid.
2. **Todo list** — add/edit/delete/complete, filter + search, localStorage persistence, empty state.
3. **Image gallery** — grid + lightbox with keyboard navigation and loading/error/empty states.
4. **Component library** — reusable Input/Select/Checkbox/Radio with typed props.

### Homework

- Read React docs: "Lists and Keys", "Handling Events", "Thinking in React"
- Extend the product app: cart page, quantity controls, `React.memo` on ProductCard, lazy images
- Research form approaches (controlled vs uncontrolled vs React Hook Form) — 500-word comparison

**Resources:** [react.dev](https://react.dev) · [React TS Cheatsheet](https://react-typescript-cheatsheet.netlify.app/) · [Vite](https://vitejs.dev)
