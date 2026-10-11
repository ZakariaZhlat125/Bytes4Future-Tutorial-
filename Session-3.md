# Session 3: React State, Forms & Effects

**Duration:** 3 hours
**Level:** Beginner → Intermediate
**Prerequisites:** Sessions 1–2 (TypeScript essentials, JSX, props, events, lists)
**Condensed from:** React Sessions 3 (useState) and 4 (Forms + useEffect)

---

## Session Overview

- **Part 1:** State fundamentals and the `useState` hook
- **Part 2:** Updating state correctly — objects, arrays, functional updates
- **Part 3:** State architecture — derived state, single source of truth, lifting state up
- **Part 4:** Forms in React — controlled components, validation, submission
- **Part 5:** `useEffect` — side effects, dependencies, cleanup
- **Project:** Login/Register forms + shopping cart state demo

---

## Part 1: State Fundamentals & `useState`

### What is State?

State is **component-internal data that React watches and re-renders on change**. Unlike ordinary variables (which reset every render and don't trigger updates), state persists between renders and drives the UI.

| Aspect | Regular Variable | State |
|--------|-----------------|-------|
| Persists across renders | No | Yes |
| Triggers re-render | No | Yes |
| Updated via | Assignment | Setter function |
| Use for | Temporary calculations | Anything the UI reflects |

**Props vs State:** props are inputs passed *from a parent* (read-only inside the component); state is data the component *owns and changes*.

### `useState` Syntax

```tsx
import { useState } from 'react';

const [value, setValue] = useState(initialValue);
```

With TypeScript, inference usually works; annotate explicitly for empty or union types:

```tsx
const [name, setName] = useState('');                    // string
const [count, setCount] = useState(0);                   // number
const [items, setItems] = useState<string[]>([]);        // typed array
const [user, setUser] = useState<User | null>(null);     // union type
const [theme, setTheme] = useState<'light' | 'dark'>('light'); // literal union
```

**Lazy initialization** — pass a function when the initial value is expensive:

```tsx
const [data, setData] = useState(() => expensiveComputation()); // runs once
```

**Multiple state variables:** group related values in one object; keep unrelated values separate.

```tsx
const [formData, setFormData] = useState({ name: '', email: '' });
const [isLoading, setIsLoading] = useState(false);
const [error, setError] = useState<string | null>(null);
```

---

## Part 2: Updating State Correctly

### Never Mutate — React Uses Reference Equality

```tsx
// ❌ Wrong — same reference, React won't re-render
user.name = 'Jane';
setUser(user);

// ✅ Correct — new object reference
setUser({ ...user, name: 'Jane' });
```

### Object Updates

```tsx
// Single property
setUser(prev => ({ ...prev, name: newName }));

// Nested property — copy each level
setUser(prev => ({
  ...prev,
  profile: { ...prev.profile, name: newName }
}));

// Multiple properties
setUser(prev => ({ ...prev, name, email }));
```

For deeply nested state, consider the `immer` library:

```tsx
setState(prev => produce(prev, draft => {
  draft.user.profile.city = 'New York';
}));
```

### Array Updates — Immutable Operations

```tsx
// Add
setItems(prev => [...prev, newItem]);           // end
setItems(prev => [newItem, ...prev]);           // beginning

// Remove
setItems(prev => prev.filter(item => item.id !== id));

// Update
setItems(prev => prev.map(item =>
  item.id === id ? { ...item, ...updates } : item
));

// Reorder
setItems(prev => {
  const arr = [...prev];
  const [moved] = arr.splice(fromIndex, 1);
  arr.splice(toIndex, 0, moved);
  return arr;
});

// Clear
setItems([]);
```

### Functional Updates & Previous State

React **batches** state updates. `setCount(count + 1)` uses the count captured at call time — stale if multiple updates queue up. A functional update always receives the latest state.

```tsx
// ❌ Increments by 1, not 3
setCount(count + 1);
setCount(count + 1);
setCount(count + 1);

// ✅ Increments by 3
setCount(prev => prev + 1);
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

**Golden rule: if new state depends on previous state, use a functional update.** This is essential inside `setInterval`, `setTimeout`, and event handlers — a callback closed over `count` holds a *stale closure* to the render-time value.

```tsx
useEffect(() => {
  const interval = setInterval(() => {
    setCount(prev => prev + 1); // ✅ always current
  }, 1000);
  return () => clearInterval(interval);
}, []);
```

---

## Part 3: State Architecture

### Derived State

Don't store what you can compute. Derived values calculate during render and stay in sync automatically.

```tsx
// ❌ Duplicated state — must sync manually
const [items, setItems] = useState([]);
const [itemCount, setItemCount] = useState(0);

// ✅ Single source of truth
const [items, setItems] = useState([]);
const itemCount = items.length;
const filtered = items.filter(i => i.active);
const total = items.reduce((sum, i) => sum + i.price, 0);
```

Use `useMemo` only when the derivation is expensive (covered in Session 5).

### State Duplication

Storing the same data in two places creates synchronization bugs. Keep **one source of truth** and derive everything else: filtered lists, counts, totals, full names, formatted values.

### Lifting State Up

When sibling components need the same data, move state to their nearest common parent. Data flows **down via props**, changes flow **up via callbacks**.

```tsx
function Parent() {
  const [celsius, setCelsius] = useState(0);
  return (
    <>
      <CelsiusInput value={celsius} onChange={setCelsius} />
      <FahrenheitDisplay celsius={celsius} />
    </>
  );
}

function CelsiusInput({ value, onChange }: { value: number; onChange: (v: number) => void }) {
  return <input type="number" value={value} onChange={e => onChange(Number(e.target.value))} />;
}

function FahrenheitDisplay({ celsius }: { celsius: number }) {
  return <div>{((celsius * 9) / 5 + 32).toFixed(1)}°F</div>;
}
```

Lift to the *lowest* common parent — over-lifting creates unnecessary prop drilling (Context API, Session 5, solves deep drilling).

### Common State Mistakes

1. Mutating state directly → always create new references
2. Skipping functional updates → stale values in rapid/async updates
3. State duplication → derive instead
4. `useState(expensiveFn())` → runs every render; use `useState(() => fn())`
5. `useState<any>(null)` → defeats TypeScript; write an interface
6. Missing cleanup in effects → memory leaks

---

## Part 4: Forms in React

### HTML vs React Forms

In HTML, the DOM owns form data and submit reloads the page. In React, **state owns the data** — inputs are *controlled*: `value` comes from state, `onChange` writes back to state.

```tsx
function MyForm() {
  const [formData, setFormData] = useState({ username: '', email: '' });

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value })); // name attr → field key
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault(); // stop page reload — always!
    console.log(formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="username" value={formData.username} onChange={handleChange} />
      <input name="email" type="email" value={formData.email} onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Uncontrolled components** (refs + `defaultValue`) are an alternative for file inputs and trivial forms; controlled is the default choice because it enables live validation and dynamic behavior.

### Input Type Reference

| Type | State | Key props |
|------|-------|-----------|
| text/email/password/tel/url | `string` | `value`, `onChange` |
| number/range | `number` — convert with `Number(e.target.value)` | `min`, `max`, `step` |
| checkbox | `boolean` | `checked` (not `value`!) |
| radio | `string` (shared `name`, `checked={val === opt}`) | `value`, `checked` |
| select | `string` | `value` on `<select>`, `<option>` children |
| textarea | `string` | `value`, `onChange`, `rows` |

### Validation & Errors

Track three pieces of state: `formData`, `errors`, `touched`. Validate **on blur** for real-time feedback and **on submit** as the gate.

```tsx
interface FormData { email: string; password: string; }

const validateField = (field: keyof FormData, value: string): string | undefined => {
  switch (field) {
    case 'email':
      if (!value.trim()) return 'Email is required';
      if (!/\S+@\S+\.\S+/.test(value)) return 'Invalid email';
      break;
    case 'password':
      if (value.length < 8) return 'At least 8 characters';
      break;
  }
};

// On blur: mark touched, show error
const handleBlur = (e: React.FocusEvent<HTMLInputElement>) => {
  const { name, value } = e.target;
  setTouched(prev => ({ ...prev, [name]: true }));
  setErrors(prev => ({ ...prev, [name]: validateField(name as keyof FormData, value) }));
};

// On change: re-validate only if already touched (don't nag while typing)
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  const { name, value } = e.target;
  setFormData(prev => ({ ...prev, [name]: value }));
  if (touched[name]) {
    setErrors(prev => ({ ...prev, [name]: validateField(name as keyof FormData, value) }));
  }
};

const handleSubmit = (e: React.FormEvent) => {
  e.preventDefault();
  // Validate everything; mark all touched
  const newErrors: Record<string, string | undefined> = {};
  (Object.keys(formData) as (keyof FormData)[]).forEach(f => {
    newErrors[f] = validateField(f, formData[f]);
  });
  setErrors(newErrors);
  if (Object.values(newErrors).every(e => !e)) {
    // proceed — async submit, loading state, success/error feedback
  }
};
```

Display errors inline, accessibly:

```tsx
{errors.email && <span role="alert" className="error-message">{errors.email}</span>}
```

**Reset** restores initial values — reset `formData`, `errors`, and `touched` together:

```tsx
const resetForm = () => {
  setFormData(initialFormData);
  setErrors({});
  setTouched({});
};
```

---

## Part 5: `useEffect` — Side Effects

### Rendering vs Events vs Side Effects

| | Rendering | Events | Side Effects |
|--|-----------|--------|--------------|
| Purpose | Display UI | Handle user interaction | External operations |
| Trigger | State/props change | User action | Lifecycle / dependency change |
| Handled by | Component body | `onX` handlers | `useEffect` |

A **side effect** is anything outside rendering: data fetching, subscriptions, timers, DOM manipulation, `localStorage`, event listeners, third-party libraries.

### Syntax

```tsx
useEffect(() => {
  // Effect code — runs AFTER render

  return () => {
    // Optional cleanup — runs before unmount and before each re-run
  };
}, [dep1, dep2]); // dependency array
```

- **No array** → runs after *every* render
- **`[]`** → runs once on mount (≈ `componentDidMount`)
- **`[deps]`** → runs on mount + whenever a dep changes (≈ `componentDidUpdate`)
- **Cleanup** → runs on unmount and before each re-run (≈ `componentWillUnmount`)

Effects run **after the DOM updates**, in declaration order. Never assume synchronous execution.

### Common Effects

**Data fetching:**

```tsx
useEffect(() => {
  const fetchUser = async () => {
    try {
      setLoading(true);
      const res = await fetch(`/api/users/${userId}`);
      setUser(await res.json());
    } catch (err) {
      setError('Failed to load user');
    } finally {
      setLoading(false);
    }
  };
  fetchUser();
}, [userId]); // re-fetch when userId changes
```

**Timer:**

```tsx
useEffect(() => {
  const interval = setInterval(() => setSeconds(prev => prev + 1), 1000);
  return () => clearInterval(interval); // cleanup prevents memory leak
}, []);
```

**Event listener:**

```tsx
useEffect(() => {
  const handleResize = () => setWidth(window.innerWidth);
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);
```

**Cancellable fetch** (prevents setting state after unmount/race conditions):

```tsx
useEffect(() => {
  const controller = new AbortController();
  fetch('/api/data', { signal: controller.signal }).then(setData);
  return () => controller.abort();
}, []);
```

### Common Mistakes

1. **Async effect function** — `useEffect(async () => ...)` is invalid (the return value must be a cleanup function, not a Promise). Define the async fn *inside* and call it.
2. **Missing dependencies** → stale closures. Include everything the effect reads, or restructure with functional updates.
3. **Infinite loops** — updating state that's in the dep array: `useEffect(() => setData([...data, x]), [data])` loops forever. Fix with a functional update and no `data` dep.
4. **No cleanup** → leaked timers/listeners/subscriptions.
5. **Unstable references** — objects/functions created in render are new every render; stabilize with `useMemo`/`useCallback` if used as deps.

### When NOT to Use `useEffect`

- Derived state → calculate during render (`const fullName = `${first} ${last}``)
- Data transformation → render-time calc or `useMemo` if expensive
- Syncing props to state → use `key` prop or make it controlled
- Simple calculations → just compute in the body

**Rule of thumb:** `useEffect` is for synchronizing with systems *outside* React. If you're only updating React state based on other React state, you don't need it.

---

## Practical Project: Auth Forms + Shopping Cart

The full course builds two projects; key patterns condensed:

**Login/Register app** (`src/`):
- `types/index.ts` — `LoginFormData`, `RegisterFormData`, `User`
- `utils/validation.ts` — `validateEmail`, `validatePassword`, `validateName`, `validateConfirmPassword`
- `components/LoginForm.tsx` — email/password/remember-me, blur+submit validation, `useEffect` loads remembered email from `localStorage` on mount
- `components/RegisterForm.tsx` — name/email/password/confirm, per-field errors, success state
- `App.tsx` — `user` state + `isLoginView` toggle; `useEffect` restores `currentUser` from `localStorage`; login persists it

**Shopping cart** demonstrates state architecture:

```tsx
// Single source of truth in App
const [cartItems, setCartItems] = useState<CartItem[]>([]);

// Derived — never stored
const cartCount = cartItems.reduce((n, i) => n + i.quantity, 0);
const cartTotal = cartItems.reduce((sum, i) => {
  const p = products.find(p => p.id === i.productId);
  return sum + (p ? p.price * i.quantity : 0);
}, 0);

const addToCart = (productId: number) => {
  setCartItems(prev => {
    const existing = prev.find(i => i.productId === productId);
    return existing
      ? prev.map(i => i.productId === productId ? { ...i, quantity: i.quantity + 1 } : i)
      : [...prev, { productId, quantity: 1 }];
  });
};
```

Props flow down (`onAddToCart`, `isInCart`) through `ProductList` → `ProductCard`, and cart operations through `Cart` → `CartItem` — an explicit prop-drilling demo that motivates Context in Session 5.

---

## Review

**Key takeaways:**
- State triggers re-renders; regular variables don't
- Functional updates (`prev => ...`) whenever new state depends on old
- Immutable updates: spread for objects, `map`/`filter` for arrays
- One source of truth; derive everything else
- Controlled forms: state owns values; always `preventDefault()` on submit
- Validate on blur + submit; track `errors` and `touched`
- `useEffect` for side effects only; always clean up timers/listeners/subscriptions
- Missing deps → stale closures; state-in-deps updates → infinite loops

**Practice questions:**
1. What happens when you mutate state directly instead of using the setter?
2. Why does `setCount(count + 1)` fail inside a `setInterval` with `[]` deps?
3. How does derived state prevent synchronization bugs?
4. When should state be lifted up, and to which component?
5. What's the difference between `value` and `checked` props on inputs?
6. Why can't the `useEffect` callback be `async`?
7. Give three cases where `useEffect` is the wrong tool.

**Mini-exercises:**
1. Build a todo app: add/toggle/delete, filter tabs (all/active/done), stats footer — all derived, functional updates only.
2. Build a registration form with real-time validation, password-strength meter, and reset.
3. Build a countdown timer to a target date with start/pause and cleanup.
4. Build a window-size tracker with a debounced resize listener.
5. Build a debounced search box that cancels in-flight fetches with `AbortController`.

**Resources:**
- [React docs: State — a component's memory](https://react.dev/learn/state-a-components-memory)
- [React docs: useEffect](https://react.dev/reference/react/useEffect)
- [React docs: You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect)
- [Immer — immutable updates](https://immerjs.github.io/immer/)

---

*Next session: API Integration & Routing — Axios, custom hooks, and React Router.*
