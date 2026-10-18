# Session 5: React Performance, Context & State Management

**Duration:** 3 hours
**Level:** Intermediate
**Prerequisites:** Sessions 1–4 (TypeScript, JSX, props, state, forms, effects, API integration, routing)
**Condensed from:** React Sessions 7 (Performance + Context API) and 8 (State Management + Zustand)

---

> **How to use this session**
> - **💡 In simple words**: a plain-language explanation under each topic. Read it first, then the code.
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.

## Session Overview

- **Part 1:** `useRef` — DOM access, mutable values, previous values
- **Part 2:** Performance hooks — `useMemo`, `useCallback`, `React.memo`
- **Part 3:** Re-renders & referential equality
- **Part 4:** Context API — Provider, `useContext`, the standard pattern
- **Part 5:** State management types — local, lifted, context, global, server
- **Part 6:** Zustand — stores, actions, selectors, persist
- **Part 7:** Custom hooks library — `useAuth`, `useTheme`, `useLocalStorage`, `useDebounce`
- **Project:** Shopping cart with Zustand + theme/auth contexts

---

## Part 1: `useRef`

> 💡 **In simple words:** A ref is a box (`{ current: ... }`) that React keeps for you between renders, like state. The difference: changing a ref does **not** redraw the screen. Use refs for things the user doesn't see: a DOM element, a timer id, an old value.

`useRef` returns a mutable `{ current: value }` object that **persists across renders without triggering re-renders**. React doesn't track ref changes — that's the point.

```tsx
const ref = useRef(initialValue);
ref.current;           // read
ref.current = newVal;  // write — no re-render
```

| Use state when | Use ref when |
|----------------|--------------|
| UI must update on change | Value is invisible to the UI |
| Value rendered in JSX | DOM element access |
| Effects depend on it | Timers, intervals, abort controllers |
| | Storing previous values |

### DOM access & focus

> 💡 **In simple words:** Sometimes you need the real HTML element, to put the cursor in an input, scroll to a section or play a video. Give the element `ref={myRef}`, and React puts the element in `myRef.current`.

```tsx
function LoginForm() {
  const emailRef = useRef<HTMLInputElement>(null);
  const passwordRef = useRef<HTMLInputElement>(null);

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    if (!emailRef.current?.value) emailRef.current?.focus();
    else if (!passwordRef.current?.value) passwordRef.current?.focus();
  };

  return (
    <form onSubmit={handleSubmit}>
      <input ref={emailRef} type="email" />
      <input ref={passwordRef} type="password" />
      <button type="submit">Login</button>
    </form>
  );
}
```

Other DOM uses: `scrollIntoView()`, measuring (`offsetWidth`), media control, reading uncontrolled input values.

### Mutable values & previous values

> 💡 **In simple words:** A timer id is needed later (to stop the timer) but is never shown on the screen. Keeping it in state would cause useless redraws; a ref keeps it quietly.

```tsx
// Timer IDs — need persistence, not re-renders
const timerRef = useRef<number | null>(null);
timerRef.current = window.setInterval(() => tick(), 1000);
useEffect(() => () => clearInterval(timerRef.current!), []);

// usePrevious — compare current vs previous prop/state
function usePrevious<T>(value: T): T | undefined {
  const ref = useRef<T>(undefined);
  useEffect(() => { ref.current = value; }, [value]);
  return ref.current;
}
```

---

### ✍️ Your Turn: Part 1

```tsx
import { useRef, useState } from 'react';

// 1. Put the cursor in the input when the button is clicked
function FocusInput() {
  const inputRef = useRef<____>(null);
  const handleClick = () => {
    inputRef.current?.____();
  };
  return (
    <div>
      <input ____={inputRef} placeholder="Type here" />
      <button onClick={handleClick}>Focus</button>
    </div>
  );
}

// 2. Count clicks WITHOUT re-rendering, then show the number in an alert
function SilentCounter() {
  const clicks = useRef(____);
  const handleClick = () => {
    clicks.current = ____;
    // TODO: alert(`Clicked ... times`)
  };
  return <button onClick={handleClick}>Click me</button>;
}

// 3. Stopwatch: keep the interval id in a ref so that Stop can clear it
function Stopwatch() {
  const [seconds, setSeconds] = useState(0);
  const intervalRef = useRef<number | null>(null);

  const start = () => {
    if (intervalRef.current !== null) return;   // already running
    intervalRef.current = window.setInterval(() => setSeconds(s => s + 1), 1000);
  };
  const stop = () => {
    // TODO: clear the interval, then set intervalRef.current back to null
  };

  return (
    <div>
      <h2>{seconds}s</h2>
      <button onClick={start}>Start</button>
      <button onClick={stop}>Stop</button>
    </div>
  );
}

// 4. Question: in SilentCounter, the screen never shows the number. Why?
//    When should you use state instead of a ref?
```

<details>
<summary>✅ Solution</summary>

```tsx
function FocusInput() {
  const inputRef = useRef<HTMLInputElement>(null);
  const handleClick = () => {
    inputRef.current?.focus();
  };
  return (
    <div>
      <input ref={inputRef} placeholder="Type here" />
      <button onClick={handleClick}>Focus</button>
    </div>
  );
}

function SilentCounter() {
  const clicks = useRef(0);
  const handleClick = () => {
    clicks.current = clicks.current + 1;
    alert(`Clicked ${clicks.current} times`);
  };
  return <button onClick={handleClick}>Click me</button>;
}

const stop = () => {
  if (intervalRef.current !== null) {
    clearInterval(intervalRef.current);
    intervalRef.current = null;
  }
};

// 4. Changing ref.current does NOT re-render the component.
//    Use state when the value must appear on the screen.
```

</details>

---

## Part 2: Performance Hooks

### The trio

> 💡 **In simple words:** Every time state changes, React runs your component function again and recalculates everything inside it. Usually that's fast. These three tools let React **remember** (memoize) work from the previous render and reuse it when nothing relevant changed:
> - `useMemo` remembers a **value**.
> - `useCallback` remembers a **function**.
> - `React.memo` skips redrawing a **component** whose props didn't change.

| | `useMemo` | `useCallback` | `React.memo` |
|--|-----------|---------------|--------------|
| Memoizes | A computed **value** | A **function** | A **component** |
| Returns | `value` | `fn` | `Component` |
| Recreates when | deps change | deps change | props change (shallow) |
| Use for | expensive calculations | stable fn props / effect deps | expensive children |

```tsx
// useMemo — cache an expensive computed value
const filtered = useMemo(
  () => products.filter(p => p.name.toLowerCase().includes(query.toLowerCase())),
  [products, query]
);

// useCallback — keep a function's identity stable across renders
const handleAdd = useCallback((id: number) => {
  setCart(prev => [...prev, id]);
}, []);

// React.memo — skip re-render when props are shallowly equal
const ProductCard = React.memo(({ product, onAdd }: Props) => (
  <div>{product.name} <button onClick={() => onAdd(product.id)}>Add</button></div>
));
```

They work **together**: `React.memo` only helps if the props are stable — that's what `useMemo`/`useCallback` provide.

```tsx
function ProductList({ products }: { products: Product[] }) {
  const [cart, setCart] = useState<number[]>([]);
  const addToCart = useCallback((id: number) => setCart(prev => [...prev, id]), []);

  return products.map(p => (
    <ProductCard key={p.id} product={p} onAdd={addToCart} /> // skips re-render
  ));
}
```

**Custom comparison** for `React.memo` (rarely needed):

```tsx
const Card = React.memo(Component, (prev, next) => prev.product.id === next.product.id);
```

**When NOT to memoize:** simple calculations (`a + b`), primitives, components that re-render rarely or are cheap. Memoization itself costs memory and comparison time — profile first with React DevTools Profiler.

---

## Part 3: Re-renders & Referential Equality

> 💡 **In simple words:** A component redraws (re-renders) when its state changes, when its parent redraws, or when a context it uses changes. Re-rendering is normal. It's only a problem when it is slow or happens far too often.

**Re-renders are triggered by:** state changes, prop changes, parent re-rendering, context value changes.
**Not triggered by:** ref changes, regular variable mutation.

### Referential equality

> 💡 **In simple words:** JavaScript compares objects by their **address**, not by their content. Two boxes with the same toys inside are still two different boxes. An object created during every render is a "different box" each time, so React thinks it changed.

React compares with `Object.is` / `===`. Primitives compare by value; objects/arrays/functions compare by **reference**:

```ts
1 === 1;       // true
{} === {};     // false — different objects
[] === [];     // false
```

That means **anything created inside render is a new reference every render**:

```tsx
function Parent() {
  const [count, setCount] = useState(0);
  const config = { count };                       // NEW object every render
  return <MemoizedChild config={config} />;       // always re-renders anyway
}

// Fix: stabilize the reference
const config = useMemo(() => ({ count }), [count]);
// or move the state down into the child — cheapest fix of all
```

### Common performance mistakes

> 💡 **In simple words:** A rule for beginners: write simple code first. Optimize only when something is actually slow, and measure it with React DevTools before and after.

- ❌ `useMemo` on cheap calculations (`a + b`) — overhead > benefit
- ❌ Inline functions/objects as props to memoized children — defeats `React.memo`
- ❌ Non-memoized context `value` object — re-renders all consumers every render
- ❌ `key={index}` on dynamic lists — use stable IDs
- ❌ State for derived values — compute during render (or `useMemo` if expensive)
- ❌ One giant context — split by concern so consumers only re-render for what they use
- ✅ Move state down — keep state as close to where it's used as possible

---

### ✍️ Your Turn: Parts 2 & 3

```tsx
import { useState, useMemo, useCallback, memo } from 'react';

const numbers = Array.from({ length: 5000 }, (_, i) => i + 1);

function NumberStats() {
  const [min, setMin] = useState(0);
  const [darkMode, setDarkMode] = useState(false);

  // 1. Recalculate ONLY when `min` changes (not when darkMode changes)
  const bigNumbers = ____(() => {
    console.log('filtering...');
    return numbers.filter(n => n > min);
  }, [____]);

  // 2. Keep the SAME function between renders
  const reset = ____(() => setMin(0), [____]);

  return (
    <div style={{ background: darkMode ? '#222' : '#fff', color: darkMode ? '#fff' : '#000' }}>
      <input type="number" value={min} onChange={e => setMin(Number(e.target.value))} />
      <button onClick={() => setDarkMode(d => !d)}>Toggle theme</button>
      <p>{bigNumbers.length} numbers are bigger than {min}</p>
      <ResetButton onReset={reset} />
    </div>
  );
}

// 3. Wrap this component so it does NOT re-render when only the theme changes
const ResetButton = ____(function ResetButton({ onReset }: { onReset: () => void }) {
  console.log('ResetButton rendered');
  return <button onClick={onReset}>Reset</button>;
});

// 4. Test: open the console and click "Toggle theme".
//    You should NOT see "filtering..." or "ResetButton rendered". Why?

// 5. Which ones need useMemo?
//    a) const total = a + b;
//    b) sorting 10,000 products
//    c) const fullName = first + ' ' + last;
```

<details>
<summary>✅ Solution</summary>

```tsx
const bigNumbers = useMemo(() => {
  console.log('filtering...');
  return numbers.filter(n => n > min);
}, [min]);

const reset = useCallback(() => setMin(0), []);   // setMin never changes

const ResetButton = memo(function ResetButton({ onReset }: { onReset: () => void }) {
  console.log('ResetButton rendered');
  return <button onClick={onReset}>Reset</button>;
});
```

4. `useMemo` returns the saved list because `min` didn't change. `reset` is the same function (thanks to `useCallback`), so `memo` sees the same props and skips rendering `ResetButton`.

5. Only **b**. A small calculation is cheaper than memoizing it.

</details>

---

## Part 4: Context API

> 💡 **In simple words:** Context works like a radio station. The **Provider** broadcasts a value, and any component inside it can tune in with `useContext`. There is no need to pass props through every level in between.

Context solves **prop drilling** — passing the same props through many intermediate components that don't use them.

```tsx
// ❌ Prop drilling — Main and Content don't use `user`, they just forward it
<App user={user}> → <Main user={user}> → <Content user={user}> → <UserProfile user={user} />
```

### The standard pattern

> 💡 **In simple words:** It's always the same five steps:
> 1. Create the context.
> 2. Make a Provider component that holds the state.
> 3. Wrap your app in it.
> 4. Write a small `useX()` hook.
> 5. Use the hook in any component.

```tsx
// 1. Create context + typed value
interface AuthContextType {
  user: User | null;
  login: (email: string, pw: string) => Promise<void>;
  logout: () => void;
}
const AuthContext = createContext<AuthContextType | null>(null);

// 2. Provider component holds the state
export function AuthProvider({ children }: { children: ReactNode }) {
  const [user, setUser] = useState<User | null>(null);

  const login = useCallback(async (email: string, password: string) => {
    const { token, user } = await authService.login(email, password);
    localStorage.setItem('authToken', token);
    setUser(user);
  }, []);

  const logout = useCallback(() => {
    localStorage.removeItem('authToken');
    setUser(null);
  }, []);

  // 3. Memoize the value so consumers don't re-render on unrelated parent renders
  const value = useMemo(() => ({ user, login, logout }), [user, login, logout]);

  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
}

// 4. Custom hook with safety check
export function useAuth() {
  const ctx = useContext(AuthContext);
  if (!ctx) throw new Error('useAuth must be used within AuthProvider');
  return ctx;
}

// 5. Consume anywhere — no props needed
function Header() {
  const { user, logout } = useAuth();
  return <header>{user?.name} <button onClick={logout}>Logout</button></header>;
}
```

**Context performance warning:** when the context `value` changes, **every consumer re-renders** — there's no selector. So:
- Memoize `value` with `useMemo`
- Split unrelated data into separate contexts (`AuthContext`, `ThemeContext`, `NotificationContext`)
- Don't put frequently-changing data in context

**Good for:** theme, locale, auth, feature flags — infrequently-changing global-ish data.
**Bad for:** high-frequency updates, complex app state → use Zustand.

---

### ✍️ Your Turn: Part 4

Build a language switcher (English / Arabic) with Context, without passing any props.

```tsx
import { createContext, useContext, useState, type ReactNode } from 'react';

type Language = 'en' | 'ar';

interface LanguageContextType {
  language: Language;
  toggleLanguage: () => void;
}

// 1. Create the context
const LanguageContext = ____<LanguageContextType | null>(null);

// 2. The provider holds the state
export function LanguageProvider({ children }: { children: ReactNode }) {
  const [language, setLanguage] = useState<Language>(____);
  const toggleLanguage = () => setLanguage(prev => (prev === 'en' ? ____ : ____));

  return (
    <LanguageContext.____ value={{ language, toggleLanguage }}>
      {children}
    </LanguageContext.____>
  );
}

// 3. Custom hook with a safety check
export function useLanguage() {
  const ctx = ____(LanguageContext);
  // TODO: if ctx is null → throw new Error('useLanguage must be used inside LanguageProvider')
  return ctx;
}

// 4. Use it
function Greeting() {
  const { language } = useLanguage();
  return <h1>{language === 'en' ? 'Hello' : 'مرحبا'}</h1>;
}

function LanguageButton() {
  // TODO: get toggleLanguage from the context and show a "Switch language" button
}

// 5. TODO: wrap the app so Greeting and LanguageButton can use the context
function App() {
  return (
    <>
      <Greeting />
      <LanguageButton />
    </>
  );
}
```

<details>
<summary>✅ Solution</summary>

```tsx
const LanguageContext = createContext<LanguageContextType | null>(null);

export function LanguageProvider({ children }: { children: ReactNode }) {
  const [language, setLanguage] = useState<Language>('en');
  const toggleLanguage = () => setLanguage(prev => (prev === 'en' ? 'ar' : 'en'));

  return (
    <LanguageContext.Provider value={{ language, toggleLanguage }}>
      {children}
    </LanguageContext.Provider>
  );
}

export function useLanguage() {
  const ctx = useContext(LanguageContext);
  if (!ctx) throw new Error('useLanguage must be used inside LanguageProvider');
  return ctx;
}

function LanguageButton() {
  const { toggleLanguage } = useLanguage();
  return <button onClick={toggleLanguage}>Switch language</button>;
}

function App() {
  return (
    <LanguageProvider>
      <Greeting />
      <LanguageButton />
    </LanguageProvider>
  );
}
```

</details>

---

## Part 5: Types of State — Picking the Right Tool

> 💡 **In simple words:** Not all state is the same. Ask yourself: who needs this data?
> - Only one component → `useState`.
> - A few neighbors → lift the state up.
> - Many components, and it rarely changes → Context.
> - Many components, and it changes often → a store like Zustand.
> - It comes from the server → React Query.

| Type | What it is | Examples | Tool |
|------|-----------|----------|------|
| **Local** | One component's data | inputs, toggles, modals | `useState` |
| **Lifted** | Shared by nearby components | siblings reading same value | `useState` in parent |
| **Context** | Simple global data | theme, locale, auth | Context API |
| **Global** | Complex shared state | cart, notifications, multi-component workflows | Zustand |
| **Server** | Data from an API | products, users, orders | React Query / `useFetch` |

The biggest conceptual win: **server state is not client state**. API data needs caching, refetching, dedup, staleness — that's what React Query (Session 6) is for. Don't hand-roll it into a store.

---

## Part 6: Zustand — Global State Management

> 💡 **In simple words:** A **store** is a global box of state that lives outside your components. Any component can read from it or call its actions, with no Provider and no prop drilling. `create` builds the store, and `set` changes it.

Zustand is a tiny (~1KB) global store: no Provider, no boilerplate, selective re-renders via selectors, first-class TypeScript.

```bash
npm install zustand
```

```tsx
import { create } from 'zustand';

interface CounterStore {
  count: number;
  increment: () => void;
  reset: () => void;
}

const useCounterStore = create<CounterStore>((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  reset: () => set({ count: 0 }),
}));

// In ANY component — no provider needed
function Counter() {
  const count = useCounterStore((state) => state.count);      // selector!
  const increment = useCounterStore((state) => state.increment);
  return <button onClick={increment}>{count} +</button>;
}
```

**vs Redux:** ~80% less code — no action types, reducers, dispatch, or Provider. `create` + `set` + `get` is the whole API.

### Selectors — the key to performance

> 💡 **In simple words:** A selector is a small function that picks the piece of the store a component needs: `state => state.count`. The component then redraws only when **that** piece changes, not whenever anything in the store changes.

```tsx
// ❌ subscribes to the WHOLE store — re-renders on any change
const store = useCartStore();

// ✅ subscribes to one slice — re-renders only when `items` changes
const items = useCartStore((state) => state.items);
```

Reusable selectors:

```tsx
const selectTotal = (s: CartStore) => s.items.reduce((sum, i) => sum + i.price * i.qty, 0);
const total = useCartStore(selectTotal);
```

### Actions — sync, async, and `get()`

> 💡 **In simple words:** Actions are the functions inside the store that change the data (`addItem`, `login`). All the "how to change it" logic stays in the store, so components just call `addItem(product)`. Inside an action, `get()` reads the current values of the store.

```tsx
const useStore = create<Store>((set, get) => ({
  user: null,
  loading: false,
  error: null,

  // set() with partial object — merges shallowly
  setUser: (user) => set({ user }),

  // functional update
  increment: () => set((s) => ({ count: s.count + 1 })),

  // async action with state transitions
  fetchUser: async (id) => {
    set({ loading: true, error: null });
    try {
      const user = await api.getUser(id);
      set({ user, loading: false });
    } catch (error) {
      set({ error: 'Failed to load user', loading: false });
    }
  },

  // get() reads current state — use inside actions only, never in render
  checkout: async () => {
    const { items } = useCartStore.getState(); // cross-store read
    await api.createOrder({ userId: get().user?.id, items });
    useCartStore.getState().clear();
  },
}));
```

### Immutable updates in stores

> 💡 **In simple words:** The same rule as `useState`: never change arrays or objects directly; always return new ones. Otherwise the components won't notice the change.

Same rules as `useState` — never mutate:

```tsx
addItem:    (item)  => set((s) => ({ items: [...s.items, item] })),
removeItem: (id)    => set((s) => ({ items: s.items.filter(i => i.id !== id) })),
toggleTodo: (id)    => set((s) => ({ todos: s.todos.map(t =>
                        t.id === id ? { ...t, done: !t.done } : t) })),
updateUser: (patch) => set((s) => ({ user: s.user ? { ...s.user, ...patch } : null })),
// nested objects need spread at each level — or use the `immer` middleware
```

### Derived state

> 💡 **In simple words:** Totals and counts are calculated from `items` whenever you need them, never stored separately, so they are always correct.

Compute from state, don't store it:

```tsx
const useCartStore = create<CartStore>((set, get) => ({
  items: [],
  addItem: (item) => set((s) => {
    const existing = s.items.find(i => i.id === item.id);
    if (existing) {
      return { items: s.items.map(i =>
        i.id === item.id ? { ...i, quantity: i.quantity + 1 } : i) };
    }
    return { items: [...s.items, { ...item, quantity: 1 }] };
  }),
  itemCount: () => get().items.reduce((n, i) => n + i.quantity, 0),
  total: () => get().items.reduce((sum, i) => sum + i.price * i.quantity, 0),
  isEmpty: () => get().items.length === 0,
}));
```

### Persist middleware — localStorage for free

> 💡 **In simple words:** `persist` saves the store to the browser's `localStorage` automatically and loads it back when the page opens. With one extra wrapper, the cart survives a page refresh.

```tsx
import { persist, createJSONStorage } from 'zustand/middleware';
import { devtools } from 'zustand/middleware';

const useCartStore = create<CartStore>()(
  devtools(
    persist(
      (set, get) => ({ /* store */ }),
      {
        name: 'cart-storage',                            // localStorage key
        storage: createJSONStorage(() => localStorage),  // or sessionStorage
        partialize: (state) => ({ items: state.items }), // persist only some fields
        version: 1,
      }
    ),
    { name: 'CartStore' } // Redux DevTools integration
  )
);
```

### Zustand best practices

- ✅ Always use selectors — never subscribe to the whole store
- ✅ Split stores by responsibility (`useUserStore`, `useCartStore`, `useThemeStore`)
- ✅ Type the store interface explicitly
- ✅ Keep heavy logic outside actions — call pure helper functions
- ✅ Handle `loading`/`error` in async actions
- ✅ `persist` important data (cart, prefs); skip ephemeral UI state
- ❌ Don't call `get()` inside a selector or render
- ❌ Don't create one mega-store for everything

---

### ✍️ Your Turn: Parts 5 & 6

Run `npm install zustand` first.

**1.** Which kind of state is it? Write **local**, **lifted**, **context**, **global** or **server**.

| Data | Kind |
|------|------|
| The text inside one search input | ____ |
| Light / dark theme for the whole app | ____ |
| A shopping cart used on 5 different pages | ____ |
| The list of products that comes from the API | ____ |

**2.** Complete the store and use it in two different components:

```tsx
import { create } from 'zustand';

interface CounterStore {
  count: number;
  increment: () => void;
  decrement: () => void;
  reset: () => void;
}

const useCounterStore = ____<CounterStore>((set) => ({
  count: ____,
  increment: () => set((state) => ({ count: ____ })),
  decrement: () => /* TODO */,
  reset: () => /* TODO */,
}));

// No props and no Provider are needed
function CounterDisplay() {
  const count = useCounterStore((state) => ____);
  return <h1>{count}</h1>;
}

function CounterButtons() {
  const increment = useCounterStore((state) => state.increment);
  // TODO: also select decrement and reset
  // TODO: return three buttons: -  +  Reset
}
```

**3.** TODO: create `useTodoStore` with `todos: { id: number; title: string; done: boolean }[]` and the actions `addTodo(title)`, `toggleTodo(id)` and `removeTodo(id)`. Don't mutate the array.

<details>
<summary>✅ Solution</summary>

1. local · context · global · server

```tsx
const useCounterStore = create<CounterStore>((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}));

function CounterDisplay() {
  const count = useCounterStore((state) => state.count);
  return <h1>{count}</h1>;
}

function CounterButtons() {
  const increment = useCounterStore((state) => state.increment);
  const decrement = useCounterStore((state) => state.decrement);
  const reset = useCounterStore((state) => state.reset);
  return (
    <div>
      <button onClick={decrement}>-</button>
      <button onClick={increment}>+</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}

// 3
interface Todo { id: number; title: string; done: boolean; }
interface TodoStore {
  todos: Todo[];
  addTodo: (title: string) => void;
  toggleTodo: (id: number) => void;
  removeTodo: (id: number) => void;
}

const useTodoStore = create<TodoStore>((set) => ({
  todos: [],
  addTodo: (title) =>
    set((s) => ({ todos: [...s.todos, { id: Date.now(), title, done: false }] })),
  toggleTodo: (id) =>
    set((s) => ({ todos: s.todos.map((t) => (t.id === id ? { ...t, done: !t.done } : t)) })),
  removeTodo: (id) =>
    set((s) => ({ todos: s.todos.filter((t) => t.id !== id) })),
}));
```

</details>

---

## Part 7: Custom Hooks Library

> 💡 **In simple words:** A custom hook is a function whose name starts with `use` and that calls other hooks. It lets you reuse **logic** (not UI) in many components: write it once, use it everywhere.

Custom hooks are functions prefixed `use` that compose built-in hooks into reusable logic.

### `useLocalStorage` — state that survives reloads

> 💡 **In simple words:** It works like `useState`, but it also saves the value in the browser's `localStorage`, so the value is still there after the tab is closed.

```tsx
function useLocalStorage<T>(key: string, initialValue: T) {
  const [value, setValue] = useState<T>(() => {
    try {
      const item = localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch { return initialValue; }
  });

  const set = useCallback((v: T | ((prev: T) => T)) => {
    const next = v instanceof Function ? v(value) : v;
    setValue(next);
    localStorage.setItem(key, JSON.stringify(next));
  }, [key, value]);

  return [value, set] as const;
}
```

### `useDebounce` — delay updates until input settles

> 💡 **In simple words:** Debounce means "wait until the user stops". If we searched on every key press, typing "laptop" would send 6 requests. With debounce, we wait 500 ms after the last key and send just one.

```tsx
function useDebounce<T>(value: T, delay = 500): T {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const t = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(t);
  }, [value, delay]);
  return debounced;
}

// Usage — search only fires 500ms after typing stops
function Search() {
  const [query, setQuery] = useState('');
  const debouncedQuery = useDebounce(query, 500);
  useEffect(() => {
    if (debouncedQuery) searchApi(debouncedQuery);
  }, [debouncedQuery]);
  return <input value={query} onChange={e => setQuery(e.target.value)} />;
}
```

### `useTheme` — context + persistence

> 💡 **In simple words:** This example combines everything from this session:
> - **Context** shares the theme everywhere.
> - **State** remembers it.
> - **An effect** saves it and applies it to the page.
> - **A custom hook** makes it easy to use.

```tsx
type Theme = 'light' | 'dark';
const ThemeContext = createContext<{ theme: Theme; toggle: () => void } | null>(null);

export function ThemeProvider({ children }: { children: ReactNode }) {
  const [theme, setTheme] = useState<Theme>(() =>
    (localStorage.getItem('theme') as Theme) ?? 'light'
  );
  useEffect(() => {
    localStorage.setItem('theme', theme);
    document.documentElement.className = theme; // for Tailwind darkMode: 'class'
  }, [theme]);
  const toggle = useCallback(() => setTheme(t => t === 'light' ? 'dark' : 'light'), []);
  return <ThemeContext.Provider value={{ theme, toggle }}>{children}</ThemeContext.Provider>;
}

export const useTheme = () => {
  const ctx = useContext(ThemeContext);
  if (!ctx) throw new Error('useTheme must be inside ThemeProvider');
  return ctx;
};
```

`useAuth` follows the identical pattern from Part 4.

---

### ✍️ Your Turn: Part 7

```tsx
import { useState, useEffect } from 'react';

// 1. useToggle → returns [value, toggle]
function useToggle(initial = false) {
  const [value, setValue] = useState(____);
  const toggle = () => setValue(____);
  return [value, toggle] as const;
}

function Faq() {
  const [isOpen, toggle] = useToggle();
  return (
    <div>
      <button onClick={toggle}>What is React?</button>
      {isOpen && <p>A library for building user interfaces.</p>}
    </div>
  );
}

// 2. TODO: useCounter(start = 0, step = 1)
//    returns { count, increment, decrement, reset }
//    Example: const { count, increment } = useCounter(10, 5);  → 10, 15, 20...

// 3. useWindowWidth → the current window width, updated on resize
function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);
  useEffect(() => {
    const handleResize = () => setWidth(____);
    window.addEventListener('resize', ____);
    return () => /* TODO: remove the listener */;
  }, []);
  return width;
}

function ScreenInfo() {
  const width = useWindowWidth();
  // TODO: show "📱 Mobile" if width < 768, otherwise "💻 Desktop"
}
```

<details>
<summary>✅ Solution</summary>

```tsx
function useToggle(initial = false) {
  const [value, setValue] = useState(initial);
  const toggle = () => setValue(v => !v);
  return [value, toggle] as const;
}

function useCounter(start = 0, step = 1) {
  const [count, setCount] = useState(start);
  const increment = () => setCount(c => c + step);
  const decrement = () => setCount(c => c - step);
  const reset = () => setCount(start);
  return { count, increment, decrement, reset };
}

function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);
  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);
  return width;
}

function ScreenInfo() {
  const width = useWindowWidth();
  return <p>{width < 768 ? '📱 Mobile' : '💻 Desktop'} ({width}px)</p>;
}
```

</details>

---

## Practice Project: Shopping Cart (Zustand)

A compact spec — implement with the structure from Session 4's project architecture:

**`stores/cartStore.ts`** — `items`, `addItem` (merge duplicates by incrementing quantity), `removeItem`, `increase/decreaseQuantity`, `clearCart`, `getTotal`, `itemCount`, `isEmpty` — wrapped in `persist`.

**`stores/authStore.ts`** — `user`, `isAuthenticated`, async `login`, `logout`, `updateUser` — persisted.

**Pages:** `Products` (grid of cards calling `addItem`), `ProductDetails` (`useParams` + `addItem`), `Cart` (quantity controls, totals, "Login to Checkout" vs "Proceed to Checkout" based on `isAuthenticated`), `Checkout` (shipping form, order summary, `clearCart` on success).

**Header** consumes both stores: cart badge from `itemCount`, user name + logout from `useAuthStore`.

```
Before: App → Layout → Products → ProductCard → AddToCartButton   (props every level)
After:  each component calls useCartStore(selector) directly        (no drilling)
```

---

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. What is the difference between `useState` and `useRef`?
2. Give two examples of things you store in a ref.
3. What does `useMemo` save? What does `useCallback` save? What does `React.memo` do?
4. Why is `{} === {}` false, and why does that matter in React?
5. What is prop drilling?
6. What are the 4 steps to create and use a Context?
7. Why is Context not good for data that changes very often?
8. Why do we pass a selector to a Zustand store: `useStore(s => s.count)`?
9. What is the difference between client state and server state?
10. What must a custom hook's name start with? Why?

<details>
<summary>✅ Solution</summary>

1. Changing state re-renders the component. Changing `ref.current` doesn't.
2. A DOM element (to focus or scroll it), a timer id, or the previous value of a prop.
3. `useMemo` saves a calculated **value**. `useCallback` saves a **function**. `React.memo` skips re-rendering a **component** when its props didn't change.
4. Objects are compared by reference, not by content. A new object or function on every render looks "changed" to `memo`, `useEffect` dependencies and Context.
5. Passing props through components that don't need them, only to reach a deep child.
6. `createContext` → a Provider component with the state → wrap the app in the Provider → read it with `useContext` (inside a custom hook).
7. Every component that uses the context re-renders when the value changes. There are no selectors.
8. So the component re-renders only when **that** piece of data changes, not on every store change.
9. Client state lives in the browser (theme, modal open, form). Server state comes from an API and can become old, so it needs fetching, caching and refetching.
10. `use`. React (and the linter) use that name to know it is a hook and to check the rules of hooks.

</details>

### B. Predict the Output / Find the Bug

```tsx
// 1. What does the button show after 3 clicks?
const ref = useRef(0);
return <button onClick={() => { ref.current++; }}>{ref.current}</button>;

// 2. true or false?
{} === {}
[1] === [1]
'a' === 'a'

// 3. What happens?
function Greeting() {
  const { language } = useLanguage();   // the hook from "Your Turn: Part 4"
  return <p>{language}</p>;
}
// App renders <Greeting /> WITHOUT <LanguageProvider> around it

// 4. Find the bug: the cart badge never updates
const useCartStore = create<CartStore>((set) => ({
  items: [],
  addItem: (item) => set((s) => {
    s.items.push(item);
    return { items: s.items };
  }),
}));
```

<details>
<summary>✅ Solution</summary>

1. **0**, because changing a ref doesn't re-render. Use `useState` to see the number.
2. `false`, `false`, `true`. Objects and arrays are compared by reference; strings by value.
3. It throws `useLanguage must be used inside LanguageProvider`. Wrap the app in the Provider.
4. `push` changes the same array, so Zustand sees the same reference. Create a new array:
   ```tsx
   addItem: (item) => set((s) => ({ items: [...s.items, item] })),
   ```

</details>

### C. Coding Tasks

**Task 1: Auto-focus (easy).** A search page where the cursor is already in the input when the page opens. Use `useRef` + `useEffect`.

**Task 2: Theme context (medium).** `ThemeProvider` + `useTheme()` with `theme` and `toggleTheme`. A button switches light/dark, and the page background changes. Save the theme in `localStorage` so it stays after a refresh.

**Task 3: Cart store (medium).** A Zustand store with `items: { id, name, price, quantity }[]`:
- `addItem(product)`: if the product is already in the cart, add 1 to its quantity; otherwise add it with quantity 1
- `removeItem(id)` and `clearCart()`
- `CartBadge` shows the total quantity (`🛒 3`), and `CartPage` shows the total price

**Task 4: `useOnlineStatus` hook (medium).** It returns `true` or `false` using `navigator.onLine` and the window events `online` and `offline` (with cleanup). Show `🟢 Online` or `🔴 Offline`. Test it in DevTools → Network → Offline.

**Task 5: Optimize this component (medium).** Clicking 👍 must NOT re-filter the list and must NOT re-render the rows (check with `console.log`).

```tsx
interface Product { id: number; name: string; }

function ProductPage({ products }: { products: Product[] }) {
  const [search, setSearch] = useState('');
  const [likes, setLikes] = useState(0);

  const filtered = products.filter(p => p.name.toLowerCase().includes(search.toLowerCase()));
  const handleSelect = (id: number) => console.log('selected', id);

  return (
    <div>
      <button onClick={() => setLikes(likes + 1)}>👍 {likes}</button>
      <input value={search} onChange={e => setSearch(e.target.value)} />
      {filtered.map(p => <ProductRow key={p.id} product={p} onSelect={handleSelect} />)}
    </div>
  );
}

function ProductRow({ product, onSelect }: { product: Product; onSelect: (id: number) => void }) {
  console.log('row rendered', product.id);
  return <div onClick={() => onSelect(product.id)}>{product.name}</div>;
}
```

**Challenge: Toast notifications.** A Zustand store with `toasts` and `add(message, type)` (`type` is `'success' | 'error' | 'info'`). Every toast removes itself after 3 seconds. A `Toasts` component shows them in the corner, and any component can call `add('Saved!', 'success')`.

<details>
<summary>✅ Solution: Tasks 1–5 and Challenge</summary>

```tsx
import { createContext, useContext, useEffect, useRef, useState, useMemo, useCallback, memo, type ReactNode } from 'react';
import { create } from 'zustand';

// Task 1
function SearchPage() {
  const inputRef = useRef<HTMLInputElement>(null);
  useEffect(() => {
    inputRef.current?.focus();
  }, []);
  return <input ref={inputRef} placeholder="Search..." />;
}

// Task 2
type Theme = 'light' | 'dark';
const ThemeContext = createContext<{ theme: Theme; toggleTheme: () => void } | null>(null);

function ThemeProvider({ children }: { children: ReactNode }) {
  const [theme, setTheme] = useState<Theme>(
    () => (localStorage.getItem('theme') as Theme) ?? 'light'
  );

  useEffect(() => {
    localStorage.setItem('theme', theme);
    document.body.style.background = theme === 'dark' ? '#222' : '#fff';
    document.body.style.color = theme === 'dark' ? '#fff' : '#000';
  }, [theme]);

  const toggleTheme = () => setTheme(t => (t === 'light' ? 'dark' : 'light'));

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

function useTheme() {
  const ctx = useContext(ThemeContext);
  if (!ctx) throw new Error('useTheme must be used inside ThemeProvider');
  return ctx;
}

function ThemeButton() {
  const { theme, toggleTheme } = useTheme();
  return <button onClick={toggleTheme}>{theme === 'light' ? '🌙 Dark' : '☀️ Light'}</button>;
}

// Task 3
interface CartItem { id: number; name: string; price: number; quantity: number; }
interface CartStore {
  items: CartItem[];
  addItem: (product: Omit<CartItem, 'quantity'>) => void;
  removeItem: (id: number) => void;
  clearCart: () => void;
}

const useCartStore = create<CartStore>((set) => ({
  items: [],
  addItem: (product) =>
    set((s) => {
      const existing = s.items.find((i) => i.id === product.id);
      if (existing) {
        return {
          items: s.items.map((i) =>
            i.id === product.id ? { ...i, quantity: i.quantity + 1 } : i
          ),
        };
      }
      return { items: [...s.items, { ...product, quantity: 1 }] };
    }),
  removeItem: (id) => set((s) => ({ items: s.items.filter((i) => i.id !== id) })),
  clearCart: () => set({ items: [] }),
}));

function CartBadge() {
  const count = useCartStore((s) => s.items.reduce((n, i) => n + i.quantity, 0));
  return <span>🛒 {count}</span>;
}

function CartPage() {
  const items = useCartStore((s) => s.items);
  const removeItem = useCartStore((s) => s.removeItem);
  const clearCart = useCartStore((s) => s.clearCart);
  const total = items.reduce((sum, i) => sum + i.price * i.quantity, 0);

  if (items.length === 0) return <p>Your cart is empty</p>;
  return (
    <div>
      {items.map((i) => (
        <p key={i.id}>
          {i.name} × {i.quantity} = ${i.price * i.quantity}
          <button onClick={() => removeItem(i.id)}>❌</button>
        </p>
      ))}
      <h3>Total: ${total}</h3>
      <button onClick={clearCart}>Clear cart</button>
    </div>
  );
}

// Task 4
function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(navigator.onLine);
  useEffect(() => {
    const goOnline = () => setIsOnline(true);
    const goOffline = () => setIsOnline(false);
    window.addEventListener('online', goOnline);
    window.addEventListener('offline', goOffline);
    return () => {
      window.removeEventListener('online', goOnline);
      window.removeEventListener('offline', goOffline);
    };
  }, []);
  return isOnline;
}

function StatusBar() {
  const isOnline = useOnlineStatus();
  return <p>{isOnline ? '🟢 Online' : '🔴 Offline'}</p>;
}

// Task 5
interface Product { id: number; name: string; }

function ProductPage({ products }: { products: Product[] }) {
  const [search, setSearch] = useState('');
  const [likes, setLikes] = useState(0);

  const filtered = useMemo(
    () => products.filter(p => p.name.toLowerCase().includes(search.toLowerCase())),
    [products, search]
  );
  const handleSelect = useCallback((id: number) => console.log('selected', id), []);

  return (
    <div>
      <button onClick={() => setLikes(likes + 1)}>👍 {likes}</button>
      <input value={search} onChange={e => setSearch(e.target.value)} />
      {filtered.map(p => <ProductRow key={p.id} product={p} onSelect={handleSelect} />)}
    </div>
  );
}

const ProductRow = memo(function ProductRow(
  { product, onSelect }: { product: Product; onSelect: (id: number) => void }
) {
  console.log('row rendered', product.id);
  return <div onClick={() => onSelect(product.id)}>{product.name}</div>;
});

// Challenge
interface Toast { id: number; message: string; type: 'success' | 'error' | 'info'; }
interface ToastStore {
  toasts: Toast[];
  add: (message: string, type?: Toast['type']) => void;
  remove: (id: number) => void;
}

const useToastStore = create<ToastStore>((set, get) => ({
  toasts: [],
  add: (message, type = 'info') => {
    const id = Date.now();
    set((s) => ({ toasts: [...s.toasts, { id, message, type }] }));
    setTimeout(() => get().remove(id), 3000);
  },
  remove: (id) => set((s) => ({ toasts: s.toasts.filter((t) => t.id !== id) })),
}));

const toastColors = { success: 'green', error: 'red', info: 'steelblue' };

function Toasts() {
  const toasts = useToastStore((s) => s.toasts);
  return (
    <div style={{ position: 'fixed', top: 16, right: 16 }}>
      {toasts.map((t) => (
        <div key={t.id} style={{ background: toastColors[t.type], color: '#fff', padding: 8, marginBottom: 8 }}>
          {t.message}
        </div>
      ))}
    </div>
  );
}

function SaveButton() {
  const add = useToastStore((s) => s.add);
  return <button onClick={() => add('Saved!', 'success')}>Save</button>;
}
```

</details>

---

## Exercises

1. **Todo store:** Zustand store with `addTodo`, `toggleTodo`, `removeTodo`, `clearCompleted`.
2. **Theme store:** persisted `theme: 'light' | 'dark'` + `toggle` + `setTheme`.
3. **useWindowSize hook:** returns `{ width, height }`, updates on resize, cleans up listener.
4. **usePrevious hook:** store a prop's previous value; use it to fetch only when `userId` actually changes.
5. **Context splitting:** take one big `AppContext` (user + theme + settings + notifications) and split into focused contexts; memoize each provider value.
6. **Optimize a list:** wrap items in `React.memo`, stabilize handlers with `useCallback`, measure the difference in React DevTools Profiler.

## Homework

- Add coupon codes to the cart store (`applyCoupon`, `discount`, `getDiscountedTotal`).
- Build a `NotificationStore` (`add/remove/clear` with type: success/error/info) + a toast component.
- Extend the cart project: wishlist store, order history page.
- Research (500 words): Redux Toolkit vs Zustand vs Jotai vs Recoil — pros/cons and when to pick each.

## Review Questions

1. What's the difference between `useState` and `useRef`? When is each correct?
2. Why doesn't `ref.current = x` trigger a re-render?
3. `useMemo` vs `useCallback` vs `React.memo` — what does each memoize?
4. What causes a component to re-render? What doesn't?
5. Why does `{ } === { }` matter for React performance?
6. What's prop drilling? Name three solutions.
7. When should you NOT put data in Context?
8. What problem do Zustand selectors solve?
9. What's the difference between `set()` and `get()` in Zustand?
10. Client state vs server state — why does the distinction matter?

## Interview Questions (with pointers)

1. **useMemo vs useCallback vs React.memo?** Value, function, component — all keyed on dependency/prop changes via shallow comparison.
2. **Why don't ref changes re-render?** Refs are plain objects React doesn't observe; they're for values invisible to the UI (DOM nodes, timers, previous values).
3. **Context vs Zustand — how do you choose?** Context: infrequent updates, few consumers, no extra dep. Zustand: frequent updates, many consumers, selective subscriptions, persistence, no provider.
4. **Common performance mistakes?** Premature memoization, inline objects/functions to memoized children, unmemoized context values, index keys, storing derived state, giant contexts.
5. **How does Zustand avoid re-rendering all consumers?** Per-component selectors — a component re-renders only when its selected slice changes; Context can't do that.

## Common Mistakes to Avoid

- Reading the whole store (`useStore()`) instead of selectors
- Mutating state inside `set` (`state.items.push(...)`) — always return new objects
- `get()` inside render/selectors — actions only
- Memoizing cheap values — measure with the Profiler first
- Unmemoized context `value` objects
- Putting server data in Zustand by hand — that's React Query's job (next session)
- Forgetting `persist` for data that must survive refresh — and persisting throwaway UI state

---

## Additional Resources

- [React Performance — render & commit](https://react.dev/learn/render-and-commit) · [React DevTools Profiler](https://react.dev/learn/react-developer-tools)
- [Zustand Docs](https://zustand-demo.pmnd.rs/) · [Kent C. Dodds — State Management Patterns](https://kentcdodds.com/blog/application-state-management-patterns-with-react-hooks)

---

**Session 5 complete.** Next: Session 6 — React Query, Authentication & Performance.
