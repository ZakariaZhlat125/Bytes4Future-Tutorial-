# Session 2: React Fundamentals — JSX, Components, Props, Events & Lists

**Duration:** 4 hours (2h theory + 2h practice)
**Level:** Beginner
**Prerequisites:** Session 1 (TypeScript basics), HTML, CSS, JavaScript ES6+

---

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

- **Library (React):** you call it when needed; handles only the view layer — you add routing/state/HTTP yourself
- **Framework (Next.js, Angular):** calls your code (inversion of control); batteries included

### SPA & CSR

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

- Routing: React Router · State: Redux, Zustand, Context · Forms: React Hook Form · HTTP: Axios/Fetch · Styling: Tailwind, CSS Modules · Testing: Vitest/Jest + RTL · Build: Vite

---

## Part 2: Setup & Project Structure

### Node.js + Vite

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

## Part 3: JSX Essentials

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

## Part 4: Functional Components

A component is a function returning JSX. **PascalCase required** — lowercase is treated as an HTML tag.

```tsx
// ✅ correct          // ❌ treated as <userprofile> HTML element
function UserProfile() {}    function userProfile() {}
```

**Naming:** descriptive (`UserAuthenticationForm` not `Form`), PascalCase, avoid abbreviations.

### Composition

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

## Part 5: Props

Props pass data **parent → child** (one-way data flow). They are **read-only** in the child — changes come from the parent.

```tsx
function Greeting({ name }: { name: string }) {
  return <h1>Hello, {name}!</h1>;
}
<Greeting name="John" />
```

### Prop types

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

Content between tags arrives as `children: React.ReactNode` — use it for wrapper components (Card, Modal, Layout).

### Destructuring + defaults

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

## Part 6: Events

React uses **synthetic events** — wrappers around native events for cross-browser consistency, delegated at the root.

### The big three

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

```tsx
// Arrow wrapper (most common)
{tasks.map(task => (
  <li key={task.id} onClick={() => handleTaskClick(task.id)}>{task.title}</li>
))}

// With event + custom arg — event comes first in the wrapper
<button onClick={(e) => deleteTask(task.id, e)}>Delete</button>
```

---

## Part 7: Conditional Rendering & Lists

### Three techniques

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
