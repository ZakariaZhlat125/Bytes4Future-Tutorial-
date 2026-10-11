# Session 5: React Performance, Context & State Management

**Duration:** 3 hours
**Level:** Intermediate
**Prerequisites:** Sessions 1–4 (TypeScript, JSX, props, state, forms, effects, API integration, routing)
**Condensed from:** React Sessions 7 (Performance + Context API) and 8 (State Management + Zustand)

---

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

## Part 2: Performance Hooks

### The trio

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

**Re-renders are triggered by:** state changes, prop changes, parent re-rendering, context value changes.
**Not triggered by:** ref changes, regular variable mutation.

### Referential equality

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

- ❌ `useMemo` on cheap calculations (`a + b`) — overhead > benefit
- ❌ Inline functions/objects as props to memoized children — defeats `React.memo`
- ❌ Non-memoized context `value` object — re-renders all consumers every render
- ❌ `key={index}` on dynamic lists — use stable IDs
- ❌ State for derived values — compute during render (or `useMemo` if expensive)
- ❌ One giant context — split by concern so consumers only re-render for what they use
- ✅ Move state down — keep state as close to where it's used as possible

---

## Part 4: Context API

Context solves **prop drilling** — passing the same props through many intermediate components that don't use them.

```tsx
// ❌ Prop drilling — Main and Content don't use `user`, they just forward it
<App user={user}> → <Main user={user}> → <Content user={user}> → <UserProfile user={user} />
```

### The standard pattern

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

## Part 5: Types of State — Picking the Right Tool

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

## Part 7: Custom Hooks Library

Custom hooks are functions prefixed `use` that compose built-in hooks into reusable logic.

### `useLocalStorage` — state that survives reloads

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
