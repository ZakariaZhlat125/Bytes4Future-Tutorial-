# Session 3: React State, Forms & Effects

**Duration:** 3 hours
**Level:** Beginner → Intermediate
**Prerequisites:** Sessions 1–2 (TypeScript essentials, JSX, props, events, lists)
**Condensed from:** React Sessions 3 (useState) and 4 (Forms + useEffect)

---

> **How to use this session**
> - **💡 In simple words**: a plain-language explanation under each topic. Read it first, then the code.
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.

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

> 💡 **In simple words:** State is the component's **memory**. A normal variable is forgotten every time the component redraws. State is remembered, and when you change it, React redraws the screen with the new value. Examples: the number in a counter, the text in a search box, whether a menu is open.

State is **component-internal data that React watches and re-renders on change**. Unlike ordinary variables (which reset every render and don't trigger updates), state persists between renders and drives the UI.

| Aspect | Regular Variable | State |
|--------|-----------------|-------|
| Persists across renders | No | Yes |
| Triggers re-render | No | Yes |
| Updated via | Assignment | Setter function |
| Use for | Temporary calculations | Anything the UI reflects |

**Props vs State:** props are inputs passed *from a parent* (read-only inside the component); state is data the component *owns and changes*.

### `useState` Syntax

> 💡 **In simple words:** `useState(0)` creates a piece of state that starts at 0. It gives you two things: the current value (`count`) and a function that changes it (`setCount`). Never change the value directly. Always call the setter, because that is how React knows it must redraw.

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

### ✍️ Your Turn: Part 1

```tsx
import { useState } from 'react';

// 1. Counter with three buttons
function Counter() {
  const [count, setCount] = useState(____);
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => ____}>+1</button>
      <button onClick={() => ____}>-1</button>
      {/* TODO: add a Reset button that sets count back to 0 */}
    </div>
  );
}

// 2. Give each state the right type
interface User { id: number; name: string; }
const [user, setUser] = useState<____>(null);     // a User, or nothing yet
const [tags, setTags] = useState<____>([]);       // a list of strings
const [mode, setMode] = useState<____>('light');  // only 'light' or 'dark'

// 3. The screen never changes when you click. Why? Rewrite it with useState.
function BrokenCounter() {
  let count = 0;
  return <button onClick={() => { count = count + 1; }}>Clicked {count}</button>;
}

// 4. TODO: create ThemeSwitch: a button that switches between 'light' and 'dark'
//    and shows the current mode, e.g. "Mode: dark"
```

<details>
<summary>✅ Solution</summary>

```tsx
function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
      <button onClick={() => setCount(count - 1)}>-1</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}

const [user, setUser] = useState<User | null>(null);
const [tags, setTags] = useState<string[]>([]);
const [mode, setMode] = useState<'light' | 'dark'>('light');

// 3. A normal variable resets to 0 on every render and does NOT trigger a re-render.
function FixedCounter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>Clicked {count}</button>;
}

function ThemeSwitch() {
  const [mode, setMode] = useState<'light' | 'dark'>('light');
  return (
    <button onClick={() => setMode(mode === 'light' ? 'dark' : 'light')}>
      Mode: {mode}
    </button>
  );
}
```

</details>

---

## Part 2: Updating State Correctly

### Never Mutate — React Uses Reference Equality

> 💡 **In simple words:** To see whether state changed, React asks "is this the **same object** as before?" It does not look inside. If you change a property of the same object, React sees the same object and does nothing. So always make a **new copy** that includes the change; the `...` spread operator does the copying.

```tsx
// ❌ Wrong — same reference, React won't re-render
user.name = 'Jane';
setUser(user);

// ✅ Correct — new object reference
setUser({ ...user, name: 'Jane' });
```

### Object Updates

> 💡 **In simple words:** `{ ...prev, name: newName }` means: copy every property of the old object, then overwrite `name`. For nested objects, copy every level you change.

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

> 💡 **In simple words:** Don't use `push`, `splice` or `sort` on state arrays, because they change the original array. Use the ways that return a **new** array: `[...arr, x]` to add, `filter` to remove, `map` to change one item.

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

> 💡 **In simple words:** React doesn't update state immediately: it waits and applies the updates together (**batching**). `setCount(prev => prev + 1)` means "take the newest value and add 1", so it stays correct even if you call it many times in a row.

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

### ✍️ Your Turn: Part 2

```tsx
interface Task { id: number; title: string; done: boolean; }

function TaskManager() {
  const [tasks, setTasks] = useState<Task[]>([]);
  const [profile, setProfile] = useState({ name: 'Ali', city: 'Amman' });
  const [count, setCount] = useState(0);

  // 1. Add a task to the END of the list
  const addTask = (title: string) => {
    setTasks(prev => [____, { id: Date.now(), title, done: false }]);
  };

  // 2. Delete a task by id
  const deleteTask = (id: number) => {
    setTasks(prev => prev.____(t => ____));
  };

  // 3. Toggle done / not done
  const toggleTask = (id: number) => {
    setTasks(prev => prev.map(t => (t.id === id ? ____ : t)));
  };

  // 4. Change ONLY the city to 'Irbid' and keep the name
  const moveToIrbid = () => {
    setProfile(prev => ({ ____, city: 'Irbid' }));
  };

  // 5. Bug: this does NOT update the screen. Why? Fix it.
  const renameFirst = () => {
    tasks[0].title = 'Renamed';
    setTasks(tasks);
  };

  // 6. Bug: this button should add 3, but it only adds 1. Fix it.
  const addThree = () => {
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
  };

  return null; // UI not needed for this exercise
}
```

<details>
<summary>✅ Solution</summary>

```tsx
const addTask = (title: string) => {
  setTasks(prev => [...prev, { id: Date.now(), title, done: false }]);
};

const deleteTask = (id: number) => {
  setTasks(prev => prev.filter(t => t.id !== id));
};

const toggleTask = (id: number) => {
  setTasks(prev => prev.map(t => (t.id === id ? { ...t, done: !t.done } : t)));
};

const moveToIrbid = () => {
  setProfile(prev => ({ ...prev, city: 'Irbid' }));
};

// 5. We changed the SAME array, so React sees the same reference and skips the render.
//    Create a new array with a new object:
const renameFirst = () => {
  setTasks(prev => prev.map((t, i) => (i === 0 ? { ...t, title: 'Renamed' } : t)));
};

// 6. All three lines use the same old `count`. Use the functional update:
const addThree = () => {
  setCount(prev => prev + 1);
  setCount(prev => prev + 1);
  setCount(prev => prev + 1);
};
```

</details>

---

## Part 3: State Architecture

### Derived State

> 💡 **In simple words:** If a value can be **calculated** from state you already have (a count, a total, a filtered list), don't store it in another state. Calculate it while rendering, and it can never be out of date.

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

> 💡 **In simple words:** Keeping the same information in two places is like keeping two copies of a phone number: when it changes, you will forget to update one of them. Keep each piece of information in **one** place only.

Storing the same data in two places creates synchronization bugs. Keep **one source of truth** and derive everything else: filtered lists, counts, totals, full names, formatted values.

### Lifting State Up

> 💡 **In simple words:** When two components need the same data, move the state **up** to their common parent. The parent passes the value down as a prop, and also passes down a function the children call to ask for changes.

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

### ✍️ Your Turn: Part 3

```tsx
// 1. This component stores the same information 3 times.
//    Remove the `total` and `count` state and CALCULATE them instead.
function CartSummary() {
  const [items, setItems] = useState([
    { name: 'Pen', price: 2 },
    { name: 'Book', price: 10 },
  ]);
  const [total, setTotal] = useState(12);
  const [count, setCount] = useState(2);

  const addNotebook = () => {
    setItems(prev => [...prev, { name: 'Notebook', price: 5 }]);
    setTotal(total + 5);
    setCount(count + 1);
  };

  return (
    <div>
      <p>{count} items, total ${total}</p>
      <button onClick={addNotebook}>Add notebook</button>
    </div>
  );
}

// 2. Lifting state up: SearchInput and ResultCount must use the SAME text.
//    Put the state in App and pass it down with props.
const names = ['Ali', 'Alaa', 'Sara', 'Samer'];

function SearchInput(/* TODO: props */) {
  return <input /* TODO: value + onChange */ placeholder="Search..." />;
}

function ResultCount(/* TODO: props */) {
  // TODO: count the names that include the text (ignore upper/lower case)
  return <p>____ results</p>;
}

function App() {
  // TODO: const [text, setText] = ...
  return (
    <>
      <SearchInput /* TODO */ />
      <ResultCount /* TODO */ />
    </>
  );
}
```

<details>
<summary>✅ Solution</summary>

```tsx
function CartSummary() {
  const [items, setItems] = useState([
    { name: 'Pen', price: 2 },
    { name: 'Book', price: 10 },
  ]);

  // derived: always correct, nothing to keep in sync
  const count = items.length;
  const total = items.reduce((sum, item) => sum + item.price, 0);

  const addNotebook = () => {
    setItems(prev => [...prev, { name: 'Notebook', price: 5 }]);
  };

  return (
    <div>
      <p>{count} items, total ${total}</p>
      <button onClick={addNotebook}>Add notebook</button>
    </div>
  );
}

function SearchInput({ text, onTextChange }: { text: string; onTextChange: (v: string) => void }) {
  return <input value={text} onChange={e => onTextChange(e.target.value)} placeholder="Search..." />;
}

function ResultCount({ text }: { text: string }) {
  const count = names.filter(n => n.toLowerCase().includes(text.toLowerCase())).length;
  return <p>{count} results</p>;
}

function App() {
  const [text, setText] = useState('');
  return (
    <>
      <SearchInput text={text} onTextChange={setText} />
      <ResultCount text={text} />
    </>
  );
}
```

</details>

---

## Part 4: Forms in React

### HTML vs React Forms

> 💡 **In simple words:** In React, the state is the boss of the input. The input shows what the state says (`value`), and every key press updates the state (`onChange`). This is called a **controlled input**. Because the data is always in state, it is easy to validate it or show it somewhere else.

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

> 💡 **In simple words:** Each input type gives a different kind of value. Text inputs give a string. Checkboxes give true/false, so read `checked`, not `value`. Number inputs **still give a string**, so convert it with `Number()`.

| Type | State | Key props |
|------|-------|-----------|
| text/email/password/tel/url | `string` | `value`, `onChange` |
| number/range | `number` — convert with `Number(e.target.value)` | `min`, `max`, `step` |
| checkbox | `boolean` | `checked` (not `value`!) |
| radio | `string` (shared `name`, `checked={val === opt}`) | `value`, `checked` |
| select | `string` | `value` on `<select>`, `<option>` children |
| textarea | `string` | `value`, `onChange`, `rows` |

### Validation & Errors

> 💡 **In simple words:** Validation means checking the user's input before you use it. We keep three things in state:
> - the values
> - the error messages
> - the fields the user has already visited (`touched`)
>
> `touched` stops us from showing errors before the user has had a chance to type.

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

### ✍️ Your Turn: Part 4

```tsx
function SignupForm() {
  const [form, setForm] = useState({ name: '', email: '', agree: false });
  const [error, setError] = useState('');

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value, type, checked } = e.target;
    // a checkbox gives `checked`, other inputs give `value`
    setForm(prev => ({ ...prev, [name]: type === 'checkbox' ? ____ : ____ }));
  };

  const handleSubmit = (e: React.FormEvent) => {
    ____; // stop the page reload
    // TODO: if name is empty               → setError('Name is required') and stop
    // TODO: if email does not include '@'  → setError('Email is invalid') and stop
    // TODO: if agree is false              → setError('You must accept the terms') and stop
    setError('');
    alert(`Welcome ${form.name}!`);
    // TODO: reset the form to empty values
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="name" value={form.name} onChange={handleChange} placeholder="Name" />
      <input name="email" value={____} onChange={____} placeholder="Email" />
      <label>
        <input name="agree" type="checkbox" ____={form.agree} onChange={handleChange} />
        I accept the terms
      </label>
      {error && <p style={{ color: 'red' }}>{error}</p>}
      <button type="submit">Sign up</button>
    </form>
  );
}
```

<details>
<summary>✅ Solution</summary>

```tsx
const initialForm = { name: '', email: '', agree: false };

function SignupForm() {
  const [form, setForm] = useState(initialForm);
  const [error, setError] = useState('');

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value, type, checked } = e.target;
    setForm(prev => ({ ...prev, [name]: type === 'checkbox' ? checked : value }));
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    if (!form.name.trim()) return setError('Name is required');
    if (!form.email.includes('@')) return setError('Email is invalid');
    if (!form.agree) return setError('You must accept the terms');
    setError('');
    alert(`Welcome ${form.name}!`);
    setForm(initialForm);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="name" value={form.name} onChange={handleChange} placeholder="Name" />
      <input name="email" value={form.email} onChange={handleChange} placeholder="Email" />
      <label>
        <input name="agree" type="checkbox" checked={form.agree} onChange={handleChange} />
        I accept the terms
      </label>
      {error && <p style={{ color: 'red' }}>{error}</p>}
      <button type="submit">Sign up</button>
    </form>
  );
}
```

</details>

---

## Part 5: `useEffect` — Side Effects

### Rendering vs Events vs Side Effects

> 💡 **In simple words:** Rendering should only **calculate** what to show. Anything that reaches **outside** the component (the network, timers, the browser tab title, localStorage) is a **side effect**. A side effect caused by a click goes in the event handler. A side effect that should happen because the component is on the screen goes in `useEffect`.

| | Rendering | Events | Side Effects |
|--|-----------|--------|--------------|
| Purpose | Display UI | Handle user interaction | External operations |
| Trigger | State/props change | User action | Lifecycle / dependency change |
| Handled by | Component body | `onX` handlers | `useEffect` |

A **side effect** is anything outside rendering: data fetching, subscriptions, timers, DOM manipulation, `localStorage`, event listeners, third-party libraries.

### Syntax

> 💡 **In simple words:** `useEffect(fn, deps)` means "after the screen is drawn, run `fn`". The dependency array says **when to run it again**: only when one of those values changes. The function you return is the **cleanup**: it undoes the effect, for example by stopping a timer or removing a listener.

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

> 💡 **In simple words:** These four patterns cover most real uses:
> - load data when the component appears
> - run a timer
> - listen to browser events
> - cancel a request that is no longer needed
>
> Every effect that **starts** something also **stops** it in its cleanup.

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

> 💡 **In simple words:** Beginners often use `useEffect` to calculate one state from another. That causes an extra render and creates bugs. If you can calculate a value from props or state, calculate it directly in the component body.

- Derived state → calculate during render (`const fullName = `${first} ${last}``)
- Data transformation → render-time calc or `useMemo` if expensive
- Syncing props to state → use `key` prop or make it controlled
- Simple calculations → just compute in the body

**Rule of thumb:** `useEffect` is for synchronizing with systems *outside* React. If you're only updating React state based on other React state, you don't need it.

---

### ✍️ Your Turn: Part 5

```tsx
import { useState, useEffect } from 'react';

// 1. Change the browser tab title every time count changes
function TitleCounter() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    document.title = `Clicked ${count} times`;
  }, [____]);
  return <button onClick={() => setCount(count + 1)}>Click</button>;
}

// 2. Clock: add 1 every second, and STOP the timer when the component is removed
function Clock() {
  const [seconds, setSeconds] = useState(0);
  useEffect(() => {
    const id = setInterval(() => setSeconds(____), 1000);
    return () => ____;
  }, []);
  return <p>{seconds}s</p>;
}

// 3. Load users ONE time when the component appears
interface User { id: number; name: string; }

function Users() {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // TODO: write an async function inside the effect that:
    //   - fetches https://jsonplaceholder.typicode.com/users
    //   - saves the result with setUsers
    //   - calls setLoading(false)
    // TODO: call the function
  }, ____);

  if (loading) return <p>Loading...</p>;
  return <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}

// 4. Remember the name after a page refresh
function NameSaver() {
  const [name, setName] = useState(() => localStorage.getItem('name') ?? '');
  useEffect(() => {
    // TODO: save name in localStorage with the key 'name'
  }, [____]);
  return <input value={name} onChange={e => setName(e.target.value)} />;
}

// 5. Bug: this component freezes the browser. Why? Fix it.
function Broken() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    setCount(count + 1);
  });
  return <p>{count}</p>;
}
```

<details>
<summary>✅ Solution</summary>

```tsx
// 1
useEffect(() => {
  document.title = `Clicked ${count} times`;
}, [count]);

// 2
useEffect(() => {
  const id = setInterval(() => setSeconds(prev => prev + 1), 1000);
  return () => clearInterval(id);
}, []);

// 3
useEffect(() => {
  const loadUsers = async () => {
    const res = await fetch('https://jsonplaceholder.typicode.com/users');
    const data: User[] = await res.json();
    setUsers(data);
    setLoading(false);
  };
  loadUsers();
}, []);

// 4
useEffect(() => {
  localStorage.setItem('name', name);
}, [name]);

// 5. No dependency array → the effect runs after EVERY render.
//    It changes state → new render → effect runs again → forever.
//    Fix: add a dependency array so it runs only once ([] = on mount).
//    (In development, StrictMode runs effects twice on purpose, so you may see 2.)
useEffect(() => {
  setCount(prev => prev + 1);
}, []);
```

</details>

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

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. What is the difference between a normal variable and state?
2. What does `useState` return?
3. Why must we never change state directly, like `user.name = 'x'`?
4. When should you use the functional update `setCount(prev => prev + 1)`?
5. What is derived state? Give an example.
6. What does "lifting state up" mean?
7. What is a controlled input?
8. For a checkbox, which prop do we use: `value` or `checked`?
9. What is the difference between no dependency array, `[]`, and `[userId]` in `useEffect`?
10. What is a cleanup function? Give one example where you need it.

<details>
<summary>✅ Solution</summary>

1. A normal variable resets on every render and doesn't update the screen. State is kept between renders, and changing it re-renders the component.
2. An array with two items: the current value and a function to change it, e.g. `[count, setCount]`.
3. React compares references. If you change the same object, React thinks nothing changed and doesn't re-render. Always create a new object or array.
4. When the new value depends on the old value, especially when you update several times in a row or inside timers.
5. A value you can calculate from other state, so you don't store it, e.g. `const total = items.reduce(...)`.
6. Moving state to the closest common parent so that two or more children can share it through props.
7. An input whose `value` comes from state and whose `onChange` updates that state.
8. `checked`.
9. No array: runs after every render. `[]`: runs once, after the first render. `[userId]`: runs first and then again every time `userId` changes.
10. A function returned from the effect that undoes what the effect did, e.g. `clearInterval`, `removeEventListener`, or aborting a fetch.

</details>

### B. Predict the Output / Find the Bug

```tsx
// 1. What is count after ONE click?
const [count, setCount] = useState(0);
const handleClick = () => {
  setCount(count + 1);
  setCount(count + 1);
};

// 2. What does the <p> show after clicking "Add" 3 times? Why?
const [list, setList] = useState<string[]>([]);
const add = () => { list.push('x'); setList(list); };
return <><button onClick={add}>Add</button><p>{list.length}</p></>;

// 3. Find the bug
useEffect(async () => {
  const res = await fetch('/api/data');
  setData(await res.json());
}, []);

// 4. Find the bug: the timer keeps running after the component is removed
useEffect(() => {
  setInterval(() => setSeconds(s => s + 1), 1000);
}, []);
```

<details>
<summary>✅ Solution</summary>

1. **1**, because both lines use the same old `count` (0). Fix: `setCount(prev => prev + 1)` twice gives 2.
2. **0**. `push` changes the same array, so React doesn't re-render. Fix: `setList(prev => [...prev, 'x'])`
3. The effect function can't be `async`. Write an async function inside and call it:
   ```tsx
   useEffect(() => {
     const load = async () => {
       const res = await fetch('/api/data');
       setData(await res.json());
     };
     load();
   }, []);
   ```
4. There is no cleanup. Fix:
   ```tsx
   useEffect(() => {
     const id = setInterval(() => setSeconds(s => s + 1), 1000);
     return () => clearInterval(id);
   }, []);
   ```

</details>

### C. Coding Tasks

**Task 1: Character counter (easy).** A `<textarea>` with a 100-character limit. Show `37 / 100` under it. The counter turns red after 90 characters.

**Task 2: Shopping list (easy).** An input and an "Add" button. Empty text can't be added. Each item has a ❌ delete button. Show `No items yet` when the list is empty.

**Task 3: Todo app with filters (medium).** Add, toggle (click the title), and delete todos. Add filter buttons **All / Active / Done**, and show `3 items left` (derived, not stored).

**Task 4: Login form with validation (medium).** Email and password fields. Show an error under each field: the email must include `@` and `.`, and the password needs at least 6 characters. When both are valid, show `Welcome, <email>!`

**Task 5: Stopwatch (medium).** Buttons **Start**, **Stop**, **Reset**. Use `useEffect` with `setInterval` and a cleanup.

**Challenge: Posts viewer.** Fetch `https://jsonplaceholder.typicode.com/posts?_limit=10`. Show `Loading...`, then the titles. If the request fails (try a wrong URL), show the error and a **Try again** button. Add a **Reload** button.

<details>
<summary>✅ Solution: Tasks 1–5 and Challenge</summary>

```tsx
import { useState, useEffect } from 'react';

// Task 1
function CharCounter() {
  const [text, setText] = useState('');
  const max = 100;
  return (
    <div>
      <textarea value={text} maxLength={max} onChange={e => setText(e.target.value)} />
      <p style={{ color: text.length > 90 ? 'red' : 'black' }}>
        {text.length} / {max}
      </p>
    </div>
  );
}

// Task 2
function ShoppingList() {
  const [items, setItems] = useState<string[]>([]);
  const [text, setText] = useState('');

  const addItem = () => {
    if (!text.trim()) return;
    setItems(prev => [...prev, text.trim()]);
    setText('');
  };
  const removeItem = (index: number) => {
    setItems(prev => prev.filter((_, i) => i !== index));
  };

  return (
    <div>
      <input value={text} onChange={e => setText(e.target.value)} />
      <button onClick={addItem}>Add</button>
      {items.length === 0 ? (
        <p>No items yet</p>
      ) : (
        <ul>
          {items.map((item, i) => (
            <li key={i}>
              {item} <button onClick={() => removeItem(i)}>❌</button>
            </li>
          ))}
        </ul>
      )}
    </div>
  );
}
// Note: index keys are OK here only because items have no own state.
// In real apps give each item an id (see Task 3).

// Task 3
interface Todo { id: number; title: string; done: boolean; }
type Filter = 'all' | 'active' | 'done';

function TodoApp() {
  const [todos, setTodos] = useState<Todo[]>([]);
  const [text, setText] = useState('');
  const [filter, setFilter] = useState<Filter>('all');

  const addTodo = () => {
    if (!text.trim()) return;
    setTodos(prev => [...prev, { id: Date.now(), title: text.trim(), done: false }]);
    setText('');
  };
  const toggleTodo = (id: number) =>
    setTodos(prev => prev.map(t => (t.id === id ? { ...t, done: !t.done } : t)));
  const deleteTodo = (id: number) =>
    setTodos(prev => prev.filter(t => t.id !== id));

  // derived values
  const visible = todos.filter(t => {
    if (filter === 'active') return !t.done;
    if (filter === 'done') return t.done;
    return true;
  });
  const leftCount = todos.filter(t => !t.done).length;

  return (
    <div>
      <input value={text} onChange={e => setText(e.target.value)} />
      <button onClick={addTodo}>Add</button>

      <div>
        <button onClick={() => setFilter('all')}>All</button>
        <button onClick={() => setFilter('active')}>Active</button>
        <button onClick={() => setFilter('done')}>Done</button>
      </div>

      <ul>
        {visible.map(t => (
          <li key={t.id}>
            <span
              onClick={() => toggleTodo(t.id)}
              style={{ textDecoration: t.done ? 'line-through' : 'none', cursor: 'pointer' }}
            >
              {t.title}
            </span>
            <button onClick={() => deleteTodo(t.id)}>❌</button>
          </li>
        ))}
      </ul>
      <p>{leftCount} items left</p>
    </div>
  );
}

// Task 4
interface LoginErrors { email?: string; password?: string; }

function LoginForm() {
  const [form, setForm] = useState({ email: '', password: '' });
  const [errors, setErrors] = useState<LoginErrors>({});
  const [success, setSuccess] = useState(false);

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setForm(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    const newErrors: LoginErrors = {};
    if (!form.email.includes('@') || !form.email.includes('.')) {
      newErrors.email = 'Enter a valid email';
    }
    if (form.password.length < 6) {
      newErrors.password = 'Password must be at least 6 characters';
    }
    setErrors(newErrors);
    setSuccess(Object.keys(newErrors).length === 0);
  };

  if (success) return <h2>Welcome, {form.email}!</h2>;

  return (
    <form onSubmit={handleSubmit}>
      <input name="email" value={form.email} onChange={handleChange} placeholder="Email" />
      {errors.email && <p style={{ color: 'red' }}>{errors.email}</p>}
      <input name="password" type="password" value={form.password} onChange={handleChange} placeholder="Password" />
      {errors.password && <p style={{ color: 'red' }}>{errors.password}</p>}
      <button type="submit">Login</button>
    </form>
  );
}

// Task 5
function Stopwatch() {
  const [seconds, setSeconds] = useState(0);
  const [isRunning, setIsRunning] = useState(false);

  useEffect(() => {
    if (!isRunning) return;
    const id = setInterval(() => setSeconds(prev => prev + 1), 1000);
    return () => clearInterval(id);
  }, [isRunning]);

  return (
    <div>
      <h2>{seconds}s</h2>
      <button onClick={() => setIsRunning(true)}>Start</button>
      <button onClick={() => setIsRunning(false)}>Stop</button>
      <button onClick={() => { setIsRunning(false); setSeconds(0); }}>Reset</button>
    </div>
  );
}

// Challenge
interface Post { id: number; title: string; }

function PostsViewer() {
  const [posts, setPosts] = useState<Post[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  const [reloadKey, setReloadKey] = useState(0);   // change it to fetch again

  useEffect(() => {
    const load = async () => {
      setLoading(true);
      setError(null);
      try {
        const res = await fetch('https://jsonplaceholder.typicode.com/posts?_limit=10');
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        setPosts(await res.json());
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Something went wrong');
      } finally {
        setLoading(false);
      }
    };
    load();
  }, [reloadKey]);

  const reload = () => setReloadKey(k => k + 1);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error} <button onClick={reload}>Try again</button></p>;

  return (
    <div>
      <button onClick={reload}>Reload</button>
      <ul>
        {posts.map(p => <li key={p.id}>{p.title}</li>)}
      </ul>
    </div>
  );
}
```

</details>

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
