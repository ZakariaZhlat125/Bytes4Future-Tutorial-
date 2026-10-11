# Session 4: React API Integration & Routing

**Duration:** 3 hours
**Level:** Beginner → Intermediate
**Prerequisites:** Sessions 1–3 (TypeScript essentials, JSX, props, events, state, forms, useEffect)
**Condensed from:** React Sessions 5 (API Integration) and 6 (React Router)

---

## Session Overview

- **Part 1:** API fundamentals — REST, HTTP methods, status codes, JSON
- **Part 2:** Fetching data — `fetch`, `async/await`, `try/catch`
- **Part 3:** Request states — loading, error, success, empty + `useEffect` + API
- **Part 4:** Axios — instances, base URL, headers, interceptors
- **Part 5:** Architecture — API services, custom hooks, separation of concerns
- **Part 6:** React Router fundamentals — `BrowserRouter`, `Routes`, `Route`
- **Part 7:** Navigation — `Link`, `NavLink`, `useNavigate`, `useParams`, `useLocation`
- **Part 8:** Advanced routing — dynamic, nested routes, layouts, `Outlet`, 404
- **Part 9:** Route protection — protected routes, auth guards, role-based access
- **Part 10:** Project architecture + practice

---

## Part 1: API Fundamentals

### What is an API?

An **API (Application Programming Interface)** is a set of rules that lets applications communicate. The client sends a request, the server processes it and returns a response — usually as JSON.

```ts
// Client requests → server responds with JSON
fetch('https://api.example.com/users')
  .then(res => res.json())
  .then(data => console.log(data));
```

Common API styles: **REST** (most common, uses HTTP methods), GraphQL, SOAP, WebSocket, gRPC.

### REST API

REST (Representational State Transfer) designs APIs around **resources identified by URLs** and standard HTTP methods:

```
GET    /api/users       // list users
GET    /api/users/1     // get one user
POST   /api/users       // create user
PUT    /api/users/1     // replace user
PATCH  /api/users/1     // update some fields
DELETE /api/users/1     // delete user
```

**REST best practices:**
- Use plural nouns for resources (`/users`, not `/getUsers`)
- Version your API (`/v1`, `/v2`)
- Support filtering, sorting, pagination via query params
- Return consistent error responses

### HTTP Methods

| Method | Purpose | Idempotent | Safe | Body |
|--------|---------|------------|------|------|
| GET | Retrieve | Yes | Yes | No |
| POST | Create | No | No | Yes |
| PUT | Replace entirely | Yes | No | Yes |
| PATCH | Update fields | No* | No | Yes |
| DELETE | Remove | Yes | No | No |

**PUT vs PATCH:** PUT replaces the whole resource (send all fields). PATCH updates only the fields you send — less data, more common in modern APIs.

```ts
// POST — create
await fetch('/api/users', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name: 'John', email: 'john@example.com' })
});

// PATCH — partial update
await fetch('/api/users/1', {
  method: 'PATCH',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name: 'Jane' })   // only changed fields
});

// DELETE — remove
await fetch('/api/users/1', { method: 'DELETE' });
```

For file uploads use `FormData` — and **don't** set `Content-Type` manually (the browser adds the boundary):

```ts
const formData = new FormData();
formData.append('image', file);
formData.append('name', 'Product');

await fetch('/api/products/with-image', {
  method: 'POST',
  headers: { 'Authorization': `Bearer ${token}` },
  body: formData
});
```

### HTTP Status Codes

| Range | Category | Examples |
|-------|----------|----------|
| 2xx | Success | 200 OK, 201 Created, 204 No Content |
| 3xx | Redirect | 301 Moved, 304 Not Modified |
| 4xx | Client error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 409 Conflict, 422 Validation, 429 Rate Limited |
| 5xx | Server error | 500 Internal Error, 502 Bad Gateway, 503 Unavailable |

Handle them explicitly rather than just checking `response.ok`:

```ts
const response = await fetch('/api/data');
switch (response.status) {
  case 200: return response.json();
  case 204: return null;
  case 401: throw new Error('Unauthorized: please log in');
  case 404: throw new Error('Resource not found');
  case 429: throw new Error('Too many requests — try again later');
  default:
    if (!response.ok) throw new Error(`Unexpected status: ${response.status}`);
    return response.json();
}
```

### JSON

JSON is the standard data format for APIs. Convert with `JSON.stringify` (object → string) and `JSON.parse` (string → object):

```ts
const json = JSON.stringify(user);         // send to API
const user = JSON.parse(json) as User;     // parse response

// Dates travel as ISO strings — convert at the boundary
const apiUser = { ...user, createdAt: user.createdAt.toISOString() };
const localUser = { ...apiUser, createdAt: new Date(apiUser.createdAt) };
```

Always wrap `JSON.parse` in `try/catch` — it throws on malformed input.

---

## Part 2: Fetching Data

### Fetch API

```ts
const response = await fetch(url, options);
const data = await response.json();
```

- `fetch` returns a `Promise<Response>`
- **Important:** fetch only rejects on *network* failure — HTTP errors like 404 still resolve. Always check `response.ok`.
- Response methods: `.json()`, `.text()`, `.blob()`, `.formData()`

```ts
async function getUsers(): Promise<User[]> {
  const response = await fetch('https://api.example.com/users');
  if (!response.ok) throw new Error(`HTTP ${response.status}`);
  return response.json();
}
```

**Query parameters** — build them safely with `URL`:

```ts
const url = new URL('https://api.example.com/products');
url.searchParams.append('page', '1');
url.searchParams.append('category', 'electronics');
const res = await fetch(url);
```

**Timeout / cancellation** with `AbortController`:

```ts
const controller = new AbortController();
const timeout = setTimeout(() => controller.abort(), 5000);
try {
  const res = await fetch(url, { signal: controller.signal });
} catch (err) {
  if (err.name === 'AbortError') throw new Error('Request timeout');
  throw err;
} finally {
  clearTimeout(timeout);
}
```

### async/await

`async` functions return Promises; `await` pauses until resolution. Equivalent but far more readable than `.then()` chains:

```ts
// Sequential (when each step needs the previous)
const user = await fetchUser(id);
const posts = await fetchUserPosts(user.id);

// Parallel (independent calls — much faster)
const [user, posts] = await Promise.all([fetchUser(id), fetchPosts()]);
```

### try/catch/finally

```ts
async function fetchData() {
  setLoading(true);
  setError(null);
  try {
    const res = await fetch(url);
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    setData(await res.json());
  } catch (err) {
    setError(err instanceof Error ? err.message : 'Something went wrong');
  } finally {
    setLoading(false); // runs either way
  }
}
```

**Retry with exponential backoff** for transient failures (don't retry 4xx client errors):

```ts
async function fetchWithRetry(url: string, retries = 3): Promise<Response> {
  for (let attempt = 1; attempt <= retries; attempt++) {
    try {
      const res = await fetch(url);
      if (res.ok) return res;
      if (res.status >= 400 && res.status < 500) throw new Error(`HTTP ${res.status}`);
      throw new Error(`HTTP ${res.status}`);
    } catch (err) {
      if (attempt === retries) throw err;
      await new Promise(r => setTimeout(r, 2 ** attempt * 500)); // 1s, 2s, 4s...
    }
  }
  throw new Error('unreachable');
}
```

---

## Part 3: Request States & `useEffect`

Every API-driven UI moves through four states. Handle **all of them**:

| State | Trigger | UI |
|-------|---------|-----|
| Loading | request in flight | spinner / skeleton |
| Error | request failed | message + retry button |
| Empty | success but no data | helpful message + action |
| Success | data arrived | render the data |

```tsx
function ProductList() {
  const [products, setProducts] = useState<Product[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const controller = new AbortController();

    const load = async () => {
      setLoading(true);
      setError(null);
      try {
        const res = await fetch('/api/products', { signal: controller.signal });
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        setProducts(await res.json());
      } catch (err) {
        if (err.name !== 'AbortError') {
          setError(err instanceof Error ? err.message : 'Failed to load');
        }
      } finally {
        setLoading(false);
      }
    };

    load();
    return () => controller.abort(); // cancel on unmount / dep change
  }, []);                            // add deps to refetch on change

  if (loading) return <div className="spinner">Loading…</div>;
  if (error)   return <div className="error">{error} <button onClick={() => location.reload()}>Retry</button></div>;
  if (products.length === 0) return <div className="empty">No products yet.</div>;
  return <ul>{products.map(p => <li key={p.id}>{p.name}</li>)}</ul>;
}
```

**The API lifecycle:** component triggers request → set loading → response arrives → success (set data) or error (set message) → clear loading → render UI.

**Common mistakes:**
- Not aborting the request on unmount (memory leaks, state updates on dead components)
- Not ignoring `AbortError` in the catch (it shows as a fake error)
- Missing dependency → stale data when a prop like `userId` changes
- No error/empty handling → blank screens

---

## Part 4: Axios

Axios is a popular HTTP client with a friendlier API than fetch:

| Feature | Axios | Fetch |
|---------|-------|-------|
| JSON parsing | Automatic (`res.data`) | Manual (`res.json()`) |
| HTTP errors | Rejects promise | Must check `res.ok` |
| Interceptors | Built-in | No |
| Timeout | `timeout` config | Manual AbortController |
| Upload progress | Built-in | No |

```bash
npm install axios
```

```ts
import axios from 'axios';

const res = await axios.get<User[]>('/api/users'); // res.data is typed
const created = await axios.post('/api/users', { name: 'John' });
```

### Axios Instance

Create pre-configured clients with `axios.create` — one per API or environment:

```ts
const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL || 'https://api.example.com/api/v1',
  timeout: 10000,
  headers: { 'Content-Type': 'application/json' }
});

api.get('/users');        // GET https://api.example.com/api/v1/users
api.get('/products', { params: { page: 1, category: 'electronics' } });
```

### Interceptors & Auth

Interceptors run middleware on every request/response — perfect for auth tokens and global error handling:

```ts
// Attach the token to every outgoing request
api.interceptors.request.use(config => {
  const token = localStorage.getItem('authToken');
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

// Handle 401s globally — clear the dead token and redirect to login
api.interceptors.response.use(
  response => response,
  error => {
    if (error.response?.status === 401) {
      localStorage.removeItem('authToken');
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);
```

**Token security:** prefer `httpOnly` cookies over `localStorage` when possible, never log tokens, always use HTTPS, clear tokens on logout.

---

## Part 5: Architecture — Services, Hooks, Separation of Concerns

**Rule:** components render UI, services talk to the API, hooks manage state. Never put raw `fetch` calls in a component.

```
src/
├── services/product.service.ts   # API calls only
├── hooks/useFetch.ts             # reusable fetch state
├── hooks/useProducts.ts          # product-specific state
├── components/ProductList.tsx    # UI only
└── types/product.types.ts        # interfaces
```

### Service layer

```ts
// services/product.service.ts
import axios from 'axios';
import type { Product, CreateProductDto } from '../types/product.types';

const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  timeout: 10000
});
api.interceptors.request.use(config => {
  const token = localStorage.getItem('authToken');
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

export const productService = {
  getAll:    ()                          => api.get<Product[]>('/products').then(r => r.data),
  getById:   (id: number)                => api.get<Product>(`/products/${id}`).then(r => r.data),
  create:    (data: CreateProductDto)    => api.post<Product>('/products', data).then(r => r.data),
  update:    (id: number, data: Partial<Product>) =>
                                           api.patch<Product>(`/products/${id}`, data).then(r => r.data),
  remove:    (id: number)                => api.delete(`/products/${id}`),
  search:    (q: string)                 => api.get<Product[]>('/products/search', { params: { q } }).then(r => r.data)
};
```

### `useFetch` custom hook

Encapsulate loading/error/data/refetch once, reuse everywhere:

```ts
// hooks/useFetch.ts
import { useState, useEffect, useCallback } from 'react';

interface UseFetchResult<T> {
  data: T | null;
  loading: boolean;
  error: string | null;
  refetch: () => Promise<void>;
}

export function useFetch<T>(fetcher: () => Promise<T>, deps: unknown[] = []): UseFetchResult<T> {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  const fetchData = useCallback(async () => {
    setLoading(true);
    setError(null);
    try {
      setData(await fetcher());
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Request failed');
    } finally {
      setLoading(false);
    }
  }, deps);

  useEffect(() => { fetchData(); }, [fetchData]);
  return { data, loading, error, refetch: fetchData };
}
```

```tsx
// The component is now pure UI
function ProductList() {
  const { data: products, loading, error, refetch } = useFetch(productService.getAll);

  if (loading) return <LoadingSpinner />;
  if (error) return <div>{error} <button onClick={refetch}>Retry</button></div>;
  if (!products?.length) return <EmptyState />;
  return <ul>{products.map(p => <li key={p.id}>{p.name}</li>)}</ul>;
}
```

**Why bother?** Testability (mock the service), reusability (same service in many components), maintainability (API changes touch one file), cleaner components.

---

## Part 6: React Router Fundamentals

### Why routing?

Without a router you'd swap views with `useState` — but the URL stays the same, so pages can't be bookmarked or shared, and the back button is broken. **SPA routing** syncs the URL to the view with no page reload.

```bash
npm install react-router-dom
```

### Core setup (v6)

```tsx
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="*" element={<NotFound />} />  {/* 404 — always last */}
      </Routes>
    </BrowserRouter>
  );
}
```

- **`BrowserRouter`** — wraps the app once; uses the History API (clean URLs). Alternatives: `HashRouter` (`/#/about`, for static hosting without server config), `MemoryRouter` (tests).
- **`Routes`** — renders the *first* matching `Route`.
- **`Route`** — `path` pattern + `element` to render. Supports `:param`, `*`, `index`.

**v6 changes vs v5:** `Routes` replaces `Switch`, `element={<C />}` replaces `component={C}`, `useNavigate` replaces `useHistory`, nested routes use `Outlet`.

---

## Part 7: Navigation

### Link vs NavLink vs useNavigate

| | Link | NavLink | useNavigate |
|--|------|---------|-------------|
| Type | Component | Component | Hook |
| Active styling | No | Yes | n/a |
| Use for | Normal links | Nav menus | Programmatic (after login, submit, delete) |

```tsx
// NavLink gets isActive — great for menus
<NavLink to="/dashboard" end
  className={({ isActive }) => `nav-item ${isActive ? 'active' : ''}`}>
  Dashboard
</NavLink>

// Link can carry state to the destination
<Link to="/product-details" state={{ productId: product.id }}>View</Link>

// useNavigate for actions
const navigate = useNavigate();
navigate('/dashboard', { replace: true });        // redirect (no history entry)
navigate(-1);                                      // back
navigate('/edit', { state: { product } });         // pass state
```

Use `<a href>` for external URLs; `Link` for internal ones.

### useParams — dynamic segments

```tsx
<Route path="/products/:id" element={<ProductDetails />} />
<Route path="/users/:userId/posts/:postId" element={<PostDetails />} />
<Route path="/category/:name?" element={<Category />} />  {/* optional */}

function ProductDetails() {
  const { id } = useParams<{ id: string }>(); // all params are strings!
  const { data: product, loading, error } = useFetch(
    () => productService.getById(Number(id)),
    [id]                                       // refetch when id changes
  );
  // ...
}
```

Params arrive as **strings** — convert (`Number(id)`) and validate. Re-fetch inside `useEffect` keyed on the param.

### useLocation & query strings

```tsx
const location = useLocation();
location.pathname; // '/products/123'
location.search;   // '?page=2&q=shoes'
location.state;    // state passed via Link/navigate
location.hash;     // '#reviews'

// Read query params
const params = new URLSearchParams(location.search);
const q = params.get('q');
const page = Number(params.get('page') ?? '1');
```

`useSearchParams` is the dedicated hook for reading **and updating** the query string:

```tsx
const [searchParams, setSearchParams] = useSearchParams();
const q = searchParams.get('q') ?? '';
setSearchParams({ q: 'shoes', page: '2' }); // updates URL → /search?q=shoes&page=2
```

---

## Part 8: Advanced Routing

### Nested routes + Outlet

Child routes render inside the parent's `<Outlet />` — perfect for shared layouts:

```tsx
function DashboardLayout() {
  return (
    <div className="dashboard">
      <Sidebar />
      <main><Outlet /></main>   {/* child route renders here */}
    </div>
  );
}

<Routes>
  <Route path="/dashboard" element={<DashboardLayout />}>
    <Route index element={<DashboardHome />} />   {/* /dashboard */}
    <Route path="profile" element={<Profile />} />{/* /dashboard/profile */}
    <Route path="orders" element={<Orders />} />  {/* /dashboard/orders */}
  </Route>
</Routes>
```

**Multiple layouts** for different sections:

```tsx
<Routes>
  <Route path="/" element={<PublicLayout />}>
    <Route index element={<Landing />} />
    <Route path="login" element={<Login />} />
  </Route>
  <Route path="/app" element={<AuthLayout />}>
    <Route index element={<Dashboard />} />
    <Route path="settings" element={<Settings />} />
  </Route>
  <Route path="*" element={<NotFound />} />
</Routes>
```

### 404 page

```tsx
function NotFound() {
  const navigate = useNavigate();
  return (
    <div>
      <h1>404 — Page Not Found</h1>
      <button onClick={() => navigate(-1)}>Go Back</button>
      <Link to="/">Go Home</Link>
    </div>
  );
}
```

Place `<Route path="*" element={<NotFound />} />` last — it matches anything unmatched.

---

## Part 9: Route Protection

### ProtectedRoute component

```tsx
import { Navigate, useLocation } from 'react-router-dom';

function ProtectedRoute({ children, requiredRole }: {
  children: React.ReactNode;
  requiredRole?: string;
}) {
  const { user, loading } = useAuth();
  const location = useLocation();

  if (loading) return <LoadingSpinner />;              // wait for auth check
  if (!user) {
    // remember where they were going → return after login
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  if (requiredRole && user.role !== requiredRole) {
    return <Navigate to="/unauthorized" replace />;
  }
  return <>{children}</>;
}
```

```tsx
<Route path="/dashboard" element={
  <ProtectedRoute><Dashboard /></ProtectedRoute>
} />
<Route path="/admin" element={
  <ProtectedRoute requiredRole="admin"><AdminPanel /></ProtectedRoute>
} />
```

**Redirect back after login** using the stored `from`:

```tsx
function Login() {
  const location = useLocation();
  const navigate = useNavigate();
  const from = location.state?.from?.pathname ?? '/dashboard';

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    await login(credentials);
    navigate(from, { replace: true });
  };
  // ...
}
```

### Minimal auth context

```tsx
const AuthContext = createContext<AuthContextType | null>(null);

export function AuthProvider({ children }: { children: ReactNode }) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => { // restore session on mount
    const saved = localStorage.getItem('user');
    if (saved) setUser(JSON.parse(saved));
    setLoading(false);
  }, []);

  const login = async (email: string, password: string) => {
    const { token, user } = await authService.login(email, password);
    localStorage.setItem('authToken', token);
    setUser(user);
  };

  const logout = () => {
    localStorage.removeItem('authToken');
    setUser(null);
  };

  return (
    <AuthContext.Provider value={{ user, loading, login, logout, isAuthenticated: !!user }}>
      {children}
    </AuthContext.Provider>
  );
}

export const useAuth = () => {
  const ctx = useContext(AuthContext);
  if (!ctx) throw new Error('useAuth must be inside AuthProvider');
  return ctx;
};
```

**Roles vs permissions:** extend `ProtectedRoute` with `allowedRoles={['admin','manager']}` or `allowedPermissions={['view_reports']}` and check `user.role` / `user.permissions`.

---

## Part 10: Project Architecture

A scalable feature folder structure:

```
src/
├── components/
│   ├── common/          # Button, Input, Modal
│   ├── layout/          # Header, Sidebar, Footer
│   └── auth/            # ProtectedRoute
├── pages/               # one folder per page
│   ├── Login.tsx
│   ├── Dashboard/
│   └── Products/
│       ├── index.tsx
│       └── ProductDetails.tsx
├── layouts/             # MainLayout, AuthLayout, AdminLayout
├── routes/              # centralized route config
├── hooks/               # useAuth, useFetch, useProducts
├── services/            # api client, productService, authService
├── types/               # shared interfaces
├── utils/               # pure helpers
└── constants/           # route paths, endpoints
```

**Centralized routes:**

```tsx
// routes/index.tsx
export function AppRoutes() {
  return (
    <Routes>
      <Route path="/" element={<PublicLayout />}>
        <Route index element={<Home />} />
        <Route path="login" element={<Login />} />
      </Route>

      <Route path="/app" element={
        <ProtectedRoute><AuthLayout /></ProtectedRoute>
      }>
        <Route index element={<Dashboard />} />
        <Route path="products" element={<Products />} />
        <Route path="products/:id" element={<ProductDetails />} />
      </Route>

      <Route path="/admin" element={
        <ProtectedRoute requiredRole="admin"><AdminLayout /></ProtectedRoute>
      }>
        <Route index element={<AdminPanel />} />
      </Route>

      <Route path="*" element={<NotFound />} />
    </Routes>
  );
}
```

**Practice project — Products + Admin Dashboard:** combine both halves: a `productService` + `useFetch`-powered product list at `/app/products`, product details at `/app/products/:id`, protected by `ProtectedRoute`, inside an `AuthLayout` with `NavLink` sidebar, plus a styled 404 page.

---

## Exercises

1. **Blog with routing:** home (post list from API), `/posts/:id` details, `/category/:name`, about, contact, 404 — nested under one layout with `NavLink` nav.
2. **Debounced search:** search input that writes `?q=` to the URL, reads it with `useSearchParams`, debounces input 300ms, and refetches via `useFetch`.
3. **User management CRUD:** full list/create/edit/delete with a `userService`, all four request states, and a `useUsers` hook.
4. **Role-based access:** extend ProtectedRoute for `admin`/`editor`/`viewer`; hide UI elements via a `useAuthorization(permission)` hook.
5. **Multi-level nesting:** docs site with main → section → topic layouts (three `Outlet` levels) and a breadcrumb built from `useLocation`.

## Homework

- Refactor a component with inline `fetch` into service + hook + UI layers.
- Add pagination (`?page=`) to the products list.
- Research (500 words): React Query vs SWR vs RTK Query — when would you use each instead of `useFetch`?
- Research (500 words): React Router vs TanStack Router vs Next.js App Router.

## Review Questions

1. What's the difference between PUT and PATCH? When is POST not idempotent?
2. Why doesn't `fetch` reject on a 404? How do you handle it?
3. Name the four request states and why each matters to UX.
4. Why abort requests in `useEffect` cleanup? What error must you ignore?
5. What advantages does Axios have over fetch? What are interceptors for?
6. Why separate API logic into a service layer?
7. Link vs NavLink vs useNavigate — when do you use each?
8. How do `:param` routes work, and how do you read them?
9. What does `<Outlet />` do? How do `index` routes work?
10. How do you redirect an unauthenticated user back to their intended page after login?

## Interview Questions (with pointers)

1. **GET vs POST vs PUT vs PATCH?** GET reads (safe, idempotent, no body); POST creates (not idempotent); PUT replaces whole resource (idempotent); PATCH updates fields only.
2. **How do you handle loading/error/empty states?** `useState` trio + early-return JSX; or a `useFetch` hook. Always check `response.ok` with fetch.
3. **How do you clean up API calls in useEffect?** `AbortController` + return `() => controller.abort()`; ignore `AbortError` in catch.
4. **How do protected routes work?** Wrapper component checks `useAuth()` → `<Navigate to="/login" state={{ from: location }} />` or renders children; role check optional.
5. **useParams vs useLocation?** `useParams` returns matched route params (`:id`); `useLocation` returns `{ pathname, search, hash, state }` for the full URL.

## Common Mistakes to Avoid

- `fetch` without checking `response.ok` — 404s resolve successfully
- No `AbortController` cleanup — state updates on unmounted components
- Missing `useEffect` deps — stale data when params change
- API calls inside components instead of services/hooks
- `useState<any>` — always type API responses
- `<a href>` for internal links — causes full page reloads and loses state
- 404 route placed before other routes (it swallows everything)
- Checking auth without a `loading` state — flashes the login page on refresh
- Storing tokens carelessly — no logging, prefer httpOnly cookies, HTTPS only

---

## Additional Resources

- [Axios Docs](https://axios-http.com/docs/intro) · [Fetch API — MDN](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [React Router Docs](https://reactrouter.com/) · [REST API Best Practices](https://restfulapi.net/)

---

**Session 4 complete.** Next: Session 5 — Performance, Context & State Management.
