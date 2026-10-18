# Session 4: React API Integration & Routing

**Duration:** 3 hours
**Level:** Beginner → Intermediate
**Prerequisites:** Sessions 1–3 (TypeScript essentials, JSX, props, events, state, forms, useEffect)
**Condensed from:** React Sessions 5 (API Integration) and 6 (React Router)

---

> **How to use this session**
> - **💡 In simple words**: a plain-language explanation under each topic. Read it first, then the code.
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.
>
> All exercises use the free practice API **https://jsonplaceholder.typicode.com**, so no backend is needed.

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

> 💡 **In simple words:** An API is like a waiter in a restaurant. You (the frontend) can't go into the kitchen (the server and its database). You give your order to the waiter (a **request**), and the waiter brings back the food (a **response**). Orders and food usually travel as JSON text.

An **API (Application Programming Interface)** is a set of rules that lets applications communicate. The client sends a request, the server processes it and returns a response — usually as JSON.

```ts
// Client requests → server responds with JSON
fetch('https://api.example.com/users')
  .then(res => res.json())
  .then(data => console.log(data));
```

Common API styles: **REST** (most common, uses HTTP methods), GraphQL, SOAP, WebSocket, gRPC.

### REST API

> 💡 **In simple words:** REST is a common way to design APIs. Every **thing** (users, products) has its own URL, and the HTTP method says **what to do** with it. `GET /users/1` reads user 1 and `DELETE /users/1` deletes it: same URL, different action.

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

> 💡 **In simple words:** The method is the **verb** of the request: GET = read, POST = create, PUT = replace, PATCH = change some fields, DELETE = remove. **Safe** means the request never changes data. **Idempotent** means doing it twice has the same result as doing it once.

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

> 💡 **In simple words:** Every response comes with a number that says how it went. Remember the first digit:
> - **2xx**: success
> - **3xx**: go somewhere else
> - **4xx**: you (the client) made a mistake
> - **5xx**: the server made a mistake

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

> 💡 **In simple words:** JSON is text that looks like a JavaScript object. A network can only send text, so before sending we turn the object into text (`JSON.stringify`), and after receiving we turn the text back into an object (`JSON.parse`).

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

### ✍️ Your Turn: Part 1

**1.** Which HTTP method?

| Action | Method | URL |
|--------|--------|-----|
| Get all books | ____ | `/api/books` |
| Get book number 5 | ____ | `/api/books/5` |
| Add a new book | ____ | `/api/books` |
| Change only the price of book 5 | ____ | `/api/books/5` |
| Remove book 5 | ____ | `/api/books/5` |

**2.** Which status code?

| Situation | Code |
|-----------|------|
| Everything is OK | ____ |
| The new book was created | ____ |
| Book 99 does not exist | ____ |
| You are not logged in | ____ |
| The server crashed | ____ |

**3.** Complete the request that creates a book:

```ts
await fetch('/api/books', {
  method: '____',
  headers: { 'Content-Type': '____' },
  body: ____({ title: 'Clean Code', price: 30 }),
});
```

<details>
<summary>✅ Solution</summary>

1. `GET`, `GET`, `POST`, `PATCH`, `DELETE`
2. `200`, `201`, `404`, `401`, `500`
3.
```ts
await fetch('/api/books', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ title: 'Clean Code', price: 30 }),
});
```

</details>

---

## Part 2: Fetching Data

### Fetch API

> 💡 **In simple words:** `fetch` is the browser's built-in function for sending requests. It returns a **Promise**, a value that arrives later. Watch out: fetch only fails when there is no network. A 404 or 500 still counts as success, so check `response.ok` yourself.

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

> 💡 **In simple words:** Requests take time. `await` means "wait here until the answer arrives, then continue", so asynchronous code reads top to bottom like normal code. You can only use `await` inside a function marked `async`.

`async` functions return Promises; `await` pauses until resolution. Equivalent but far more readable than `.then()` chains:

```ts
// Sequential (when each step needs the previous)
const user = await fetchUser(id);
const posts = await fetchUserPosts(user.id);

// Parallel (independent calls — much faster)
const [user, posts] = await Promise.all([fetchUser(id), fetchPosts()]);
```

### try/catch/finally

> 💡 **In simple words:**
> - `try`: run this code.
> - `catch`: if anything fails, run this instead (for example, show an error message).
> - `finally`: always run this at the end (for example, turn off the loading spinner).

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

### ✍️ Your Turn: Part 2

Run this in your Session 1 TypeScript project (Node 18+ has `fetch` built in).

```ts
interface Todo {
  userId: number;
  id: number;
  title: string;
  completed: boolean;
}

const BASE = 'https://jsonplaceholder.typicode.com';

// 1. Complete the function
async function getTodos(): Promise<____> {
  const res = await fetch(`${BASE}/todos?_limit=5`);
  if (____) throw new Error(`HTTP ${res.status}`);
  return ____;
}

// 2. TODO: getTodo(id: number): Promise<Todo>
//    → GET /todos/:id   (don't forget to check res.ok)

// 3. TODO: createTodo(title: string): Promise<Todo>
//    → POST /todos with body { title, completed: false, userId: 1 }

async function main() {
  try {
    // 4. TODO: print the 5 todo titles
    // 5. TODO: get todo 1 and todo 2 AT THE SAME TIME (they don't depend on each other)
    const [todo1, todo2] = await ____([getTodo(1), getTodo(2)]);
    // 6. TODO: create a todo and print its new id
  } catch (err) {
    console.error('Failed:', err instanceof Error ? ____ : err);
  }
}

main();
```

<details>
<summary>✅ Solution</summary>

```ts
async function getTodos(): Promise<Todo[]> {
  const res = await fetch(`${BASE}/todos?_limit=5`);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
}

async function getTodo(id: number): Promise<Todo> {
  const res = await fetch(`${BASE}/todos/${id}`);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
}

async function createTodo(title: string): Promise<Todo> {
  const res = await fetch(`${BASE}/todos`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ title, completed: false, userId: 1 }),
  });
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
}

async function main() {
  try {
    const todos = await getTodos();
    todos.forEach(t => console.log(t.title));

    const [todo1, todo2] = await Promise.all([getTodo(1), getTodo(2)]);
    console.log(todo1.title, '|', todo2.title);

    const created = await createTodo('Learn fetch');
    console.log('New id:', created.id);   // 201 (fake API)
  } catch (err) {
    console.error('Failed:', err instanceof Error ? err.message : err);
  }
}
```

</details>

---

## Part 3: Request States & `useEffect`

> 💡 **In simple words:** A request has a life: it starts (**loading**), then it either fails (**error**) or succeeds (**data**, which may be **empty**). Your component keeps these in state and shows a different screen for each one.

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

### ✍️ Your Turn: Part 3

Use `getTodos()` from the Part 2 exercise.

```tsx
function TodoList() {
  const [todos, setTodos] = useState<Todo[]>([]);
  const [loading, setLoading] = useState(____);
  const [error, setError] = useState<____>(null);

  useEffect(() => {
    const load = async () => {
      try {
        // TODO: call getTodos() and save the result
      } catch (err) {
        // TODO: save the error message
      } finally {
        // TODO: stop loading
      }
    };
    load();
  }, ____);

  // TODO: loading → <p>Loading...</p>
  // TODO: error   → <p>Error: ...</p>
  // TODO: empty   → <p>No todos</p>
  return (
    <ul>
      {/* TODO: show each title, with ✅ in front if completed */}
    </ul>
  );
}
```

**Test the error state:** change the URL in `getTodos` to `/todoss` (wrong) and check that you see the error.

<details>
<summary>✅ Solution</summary>

```tsx
function TodoList() {
  const [todos, setTodos] = useState<Todo[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const load = async () => {
      try {
        setTodos(await getTodos());
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Failed to load');
      } finally {
        setLoading(false);
      }
    };
    load();
  }, []);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;
  if (todos.length === 0) return <p>No todos</p>;

  return (
    <ul>
      {todos.map(t => (
        <li key={t.id}>{t.completed && '✅ '}{t.title}</li>
      ))}
    </ul>
  );
}
```

</details>

---

## Part 4: Axios

> 💡 **In simple words:** Axios is a library that does the same job as `fetch`, with less code. It converts JSON automatically, throws an error on 4xx/5xx, and lets you set shared settings once.

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

> 💡 **In simple words:** An instance is a pre-configured copy of Axios. You set the base URL, timeout and headers once, and after that every request only needs the short path (`/users`).

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

> 💡 **In simple words:** An interceptor is a checkpoint that every request or response passes through. It's perfect for jobs you'd otherwise repeat everywhere, like adding the login token to every request, or sending the user to the login page whenever a response says 401.

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

> 💡 **In simple words:** "Separation of concerns" means each file has **one job**:
> - **Components** draw the UI.
> - **Services** talk to the server.
> - **Hooks** hold the loading/error/data logic.
>
> When the API changes, you edit one service file instead of 20 components.

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

> 💡 **In simple words:** A service is an object with one function per API action (`getAll`, `create`, ...). Components call `productService.getAll()` and don't need to know about URLs, headers or Axios.

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

> 💡 **In simple words:** A custom hook is your own function that uses other hooks, so you can reuse logic that has state. `useFetch` hides the three `useState` calls and the `useEffect`; the component just receives `{ data, loading, error }`.

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

### ✍️ Your Turn: Parts 4 & 5

Run `npm install axios` first.

```ts
// src/services/api.ts
import axios from 'axios';

export const api = axios.____({
  baseURL: '____',          // https://jsonplaceholder.typicode.com
  timeout: 10000,
});

// Attach a token to every request (if we have one)
api.interceptors.request.use(config => {
  const token = localStorage.getItem('authToken');
  if (token) config.headers.Authorization = `____ ${token}`;
  return ____;
});
```

```ts
// src/services/post.service.ts
import { api } from './api';

export interface Post {
  id: number;
  userId: number;
  title: string;
  body: string;
}

export const postService = {
  getAll: () => api.get<Post[]>('/posts', { params: { _limit: 10 } }).then(r => r.____),
  getById: (id: number) => /* TODO: GET /posts/:id */,
  create: (data: Omit<Post, 'id'>) => /* TODO: POST /posts */,
  remove: (id: number) => /* TODO: DELETE /posts/:id */,
};
```

```tsx
// src/components/PostList.tsx: use the useFetch hook from Part 5
function PostList() {
  const { data: posts, loading, error, refetch } = useFetch(____);

  // TODO: loading → <p>Loading...</p>
  // TODO: error   → message + <button onClick={refetch}>Retry</button>
  // TODO: list of post titles
}
```

<details>
<summary>✅ Solution</summary>

```ts
// api.ts
export const api = axios.create({
  baseURL: 'https://jsonplaceholder.typicode.com',
  timeout: 10000,
});

api.interceptors.request.use(config => {
  const token = localStorage.getItem('authToken');
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

// post.service.ts
export const postService = {
  getAll: () => api.get<Post[]>('/posts', { params: { _limit: 10 } }).then(r => r.data),
  getById: (id: number) => api.get<Post>(`/posts/${id}`).then(r => r.data),
  create: (data: Omit<Post, 'id'>) => api.post<Post>('/posts', data).then(r => r.data),
  remove: (id: number) => api.delete(`/posts/${id}`),
};
```

```tsx
function PostList() {
  const { data: posts, loading, error, refetch } = useFetch(postService.getAll);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>{error} <button onClick={refetch}>Retry</button></p>;

  return (
    <ul>
      {posts?.map(p => <li key={p.id}>{p.title}</li>)}
    </ul>
  );
}
```

</details>

---

## Part 6: React Router Fundamentals

### Why routing?

> 💡 **In simple words:** Routing means showing a different page for each URL (`/`, `/about`, `/products/5`) without reloading the browser. Users can bookmark pages, share links and use the Back button.

Without a router you'd swap views with `useState` — but the URL stays the same, so pages can't be bookmarked or shared, and the back button is broken. **SPA routing** syncs the URL to the view with no page reload.

```bash
npm install react-router-dom
```

### Core setup (v6)

> 💡 **In simple words:**
> - `BrowserRouter` watches the URL.
> - `Routes` looks at the current URL and picks the matching `Route`.
> - Each `Route` says: "for this path, show this component".
> - `path="*"` matches everything else, which makes it the 404 page.

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

> 💡 **In simple words:**
> - `Link` is the React version of `<a>`: it changes the page without reloading.
> - `NavLink` is a `Link` that knows when it points to the current page, so you can highlight menu items.
> - `useNavigate` changes the page from your code, for example after a successful login.

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

> 💡 **In simple words:** `:id` in a path is a placeholder. For the URL `/products/42`, `useParams()` gives `{ id: '42' }`, so one `ProductDetails` page works for every product. The value is always a **string**.

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

> 💡 **In simple words:** The query string is the part of the URL after `?`, as in `?page=2&q=shoes`. It holds optional settings like search text and filters. `useSearchParams` reads and changes it, so the search stays in the URL and survives a refresh.

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

### ✍️ Your Turn: Parts 6 & 7

Run `npm install react-router-dom` first.

```tsx
import { BrowserRouter, Routes, Route, Link, NavLink, useParams, useNavigate } from 'react-router-dom';

function Home() { return <h1>Home</h1>; }
function About() { return <h1>About</h1>; }
function NotFound() { return <h1>404: Page not found</h1>; }

// 1. Complete the router
function App() {
  return (
    <____>
      <nav>
        {/* TODO: links to "/", "/about", "/users/1" and "/users/2" */}
      </nav>
      <Routes>
        <Route path="/" element={____} />
        <Route path="____" element={<About />} />
        <Route path="/users/____" element={<UserPage />} />
        <Route path="____" element={<NotFound />} />
      </Routes>
    </____>
  );
}

// 2. Read the id from the URL
function UserPage() {
  const { id } = ____<{ id: string }>();
  const navigate = ____();
  return (
    <div>
      <h2>User number {id}</h2>
      {/* TODO: a "Back" button that goes to the previous page */}
      {/* TODO: a "Next user" button that goes to /users/<id + 1> */}
    </div>
  );
}

// 3. TODO: change the nav links to NavLink, and make the active link bold
```

<details>
<summary>✅ Solution</summary>

```tsx
function App() {
  const linkStyle = ({ isActive }: { isActive: boolean }) =>
    ({ fontWeight: isActive ? 'bold' : 'normal', marginRight: 8 });

  return (
    <BrowserRouter>
      <nav>
        <NavLink to="/" end style={linkStyle}>Home</NavLink>
        <NavLink to="/about" style={linkStyle}>About</NavLink>
        <NavLink to="/users/1" style={linkStyle}>User 1</NavLink>
        <NavLink to="/users/2" style={linkStyle}>User 2</NavLink>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/users/:id" element={<UserPage />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}

function UserPage() {
  const { id } = useParams<{ id: string }>();
  const navigate = useNavigate();
  return (
    <div>
      <h2>User number {id}</h2>
      <button onClick={() => navigate(-1)}>Back</button>
      {/* params are strings → convert with Number() before adding */}
      <button onClick={() => navigate(`/users/${Number(id) + 1}`)}>Next user</button>
    </div>
  );
}
```

</details>

---

## Part 8: Advanced Routing

### Nested routes + Outlet

> 💡 **In simple words:** A **layout** is the part of the page that stays the same, like a sidebar or header. **Nested routes** are pages shown **inside** the layout. `<Outlet />` marks the spot in the layout where the child page appears.

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

> 💡 **In simple words:** When no route matches the URL, show a friendly "page not found" screen with a way back, never a blank page.

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

> 💡 **In simple words:** A protected route is a guard at the door. Before showing the page it asks: is the user logged in? If not, it redirects to `/login` and remembers where the user wanted to go.

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

> 💡 **In simple words:** Many components need the logged-in user: the header, the protected routes, the profile page. **Context** keeps the user in one place that any component can read. Session 5 explains Context in detail.

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

### ✍️ Your Turn: Parts 8 & 9

```tsx
import { Routes, Route, Link, Outlet, Navigate, useNavigate } from 'react-router-dom';

// 1. Layout: the sidebar stays, and the page on the right changes
function DashboardLayout() {
  return (
    <div style={{ display: 'flex', gap: 16 }}>
      <aside>
        <Link to="/dashboard">Home</Link><br />
        <Link to="/dashboard/profile">Profile</Link><br />
        <Link to="/dashboard/settings">Settings</Link>
      </aside>
      <main>{/* TODO: child pages appear here */}</main>
    </div>
  );
}

// 2. A simple protected route (the user is saved in localStorage)
function ProtectedRoute({ children }: { children: React.ReactNode }) {
  const isLoggedIn = localStorage.getItem('user') !== null;
  if (____) {
    return <Navigate to="____" replace />;
  }
  return <>{children}</>;
}

// 3. TODO: Login page: a "Login" button that saves 'user' = 'demo' in localStorage
//    and then navigates to /dashboard

// 4. Complete the routes and PROTECT /dashboard
function AppRoutes() {
  return (
    <Routes>
      <Route path="/login" element={<Login />} />
      <Route path="/dashboard" element={/* TODO: protect DashboardLayout */}>
        <Route ____ element={<DashboardHome />} />         {/* /dashboard */}
        <Route path="____" element={<Profile />} />         {/* /dashboard/profile */}
        {/* TODO: settings route */}
      </Route>
      <Route path="*" element={<h1>404</h1>} />
    </Routes>
  );
}

// 5. TODO: in Settings, add a "Logout" button that removes 'user' and goes to /login
```

<details>
<summary>✅ Solution</summary>

```tsx
function DashboardLayout() {
  return (
    <div style={{ display: 'flex', gap: 16 }}>
      <aside>
        <Link to="/dashboard">Home</Link><br />
        <Link to="/dashboard/profile">Profile</Link><br />
        <Link to="/dashboard/settings">Settings</Link>
      </aside>
      <main><Outlet /></main>
    </div>
  );
}

function ProtectedRoute({ children }: { children: React.ReactNode }) {
  const isLoggedIn = localStorage.getItem('user') !== null;
  if (!isLoggedIn) {
    return <Navigate to="/login" replace />;
  }
  return <>{children}</>;
}

function Login() {
  const navigate = useNavigate();
  const login = () => {
    localStorage.setItem('user', 'demo');
    navigate('/dashboard');
  };
  return <button onClick={login}>Login</button>;
}

function DashboardHome() { return <h2>Dashboard home</h2>; }
function Profile() { return <h2>My profile</h2>; }

function Settings() {
  const navigate = useNavigate();
  const logout = () => {
    localStorage.removeItem('user');
    navigate('/login');
  };
  return (
    <div>
      <h2>Settings</h2>
      <button onClick={logout}>Logout</button>
    </div>
  );
}

function AppRoutes() {
  return (
    <Routes>
      <Route path="/login" element={<Login />} />
      <Route
        path="/dashboard"
        element={
          <ProtectedRoute>
            <DashboardLayout />
          </ProtectedRoute>
        }
      >
        <Route index element={<DashboardHome />} />
        <Route path="profile" element={<Profile />} />
        <Route path="settings" element={<Settings />} />
      </Route>
      <Route path="*" element={<h1>404</h1>} />
    </Routes>
  );
}
```

</details>

---

## Part 10: Project Architecture

> 💡 **In simple words:** As an app grows, a clear folder structure saves time, because everyone knows where to find and put things. Group the files by their role: components, pages, services, hooks, types.

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

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. What is an API? Explain it with a restaurant example.
2. What is the difference between `GET` and `POST`?
3. What is the difference between `PUT` and `PATCH`?
4. What do the status codes `200`, `201`, `401`, `404` and `500` mean?
5. `fetch` does **not** throw an error on a 404. How do you detect it?
6. Why do we put API calls in a service file instead of inside the component?
7. What are the four states of a request that the UI must handle?
8. Why use `<Link>` instead of `<a href>` for pages inside the app?
9. What type does `useParams()` always give you? Why does that matter?
10. What does `<Outlet />` do?

<details>
<summary>✅ Solution</summary>

1. A set of rules that lets two programs talk. You (the client) give an order to the waiter (the API), the kitchen (the server) prepares it, and the waiter brings the food (the response).
2. `GET` reads data and has no body. `POST` sends data in the body to create something new.
3. `PUT` replaces the whole resource. `PATCH` changes only the fields you send.
4. 200 OK · 201 Created · 401 not logged in · 404 not found · 500 server error.
5. Check `response.ok` (or `response.status`) and throw your own error when it is `false`.
6. The code can be reused, it's easier to test and change, and the component only cares about the UI.
7. Loading, error, empty, success.
8. `<Link>` changes the page without reloading, so the app keeps its state. `<a href>` reloads the whole page.
9. Always a `string`. Convert it with `Number(id)` before you compare it with numeric ids or do math.
10. It is the place in a layout route where the child route's page is shown.

</details>

### B. Find the Bug

```tsx
// Bug 1: the server returned 404, but no error is shown
const res = await fetch('/api/users/99');
const user = await res.json();

// Bug 2: the server receives "[object Object]"
fetch('/api/users', { method: 'POST', body: { name: 'Ali' } });

// Bug 3: inside a React Router app, every click reloads the whole page
<a href="/about">About</a>

// Bug 4: the product is never found (products have numeric ids)
const { id } = useParams();
const product = products.find(p => p.id === id);

// Bug 5: when userId changes, the old user stays on the screen
useEffect(() => {
  fetch(`/api/users/${userId}`).then(r => r.json()).then(setUser);
}, []);
```

<details>
<summary>✅ Solution</summary>

```tsx
// Bug 1: check res.ok
const res = await fetch('/api/users/99');
if (!res.ok) throw new Error(`HTTP ${res.status}`);
const user = await res.json();

// Bug 2: convert the object to JSON and set the header
fetch('/api/users', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name: 'Ali' }),
});

// Bug 3: use Link
<Link to="/about">About</Link>

// Bug 4: params are strings, so convert first
const product = products.find(p => p.id === Number(id));

// Bug 5: add userId to the dependency array
useEffect(() => {
  fetch(`/api/users/${userId}`).then(r => r.json()).then(setUser);
}, [userId]);
```

</details>

### C. Coding Tasks

**Task 1: Users list (easy).** Fetch `https://jsonplaceholder.typicode.com/users` and show each name and email. Handle loading, error and empty.

**Task 2: User details page (medium).** Add React Router. `/` shows the list from Task 1, and each name is a `Link` to `/users/:id`. The details page reads the id with `useParams`, fetches `/users/:id`, shows name, phone and website, and has a **Back** button.

**Task 3: Post service + create form (medium).** Write `postService` with `getAll`, `getById` and `create` using an Axios instance. Build a form with a title input that creates a post and shows `Created post #101`.

**Task 4: Search in the URL (medium).** A `/search` page with an input. Typing writes `?q=...` to the URL using `useSearchParams`, and the list of users is filtered by `q`. Refresh the page: the search must stay.

**Task 5: Protected profile (medium).** Pages `/login` and `/profile`. `/profile` is protected: if `localStorage.user` is missing, redirect to `/login`. Login saves the user and goes to `/profile`. Profile has a **Logout** button.

**Challenge: Mini blog.** A layout with `NavLink` navigation (Home, About). Home lists 10 posts. `/posts/:id` shows the post **and** its comments (`/posts/:id/comments`), fetched at the same time with `Promise.all`. Add a 404 page.

<details>
<summary>✅ Solution: Tasks 1–5 and Challenge</summary>

```tsx
import { useEffect, useState } from 'react';
import {
  BrowserRouter, Routes, Route, Link, NavLink, Navigate, Outlet,
  useParams, useNavigate, useSearchParams,
} from 'react-router-dom';

const BASE = 'https://jsonplaceholder.typicode.com';
interface User { id: number; name: string; email: string; phone: string; website: string; }

// Task 1 (+ Links for Task 2)
function UsersPage() {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const load = async () => {
      try {
        const res = await fetch(`${BASE}/users`);
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        setUsers(await res.json());
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Failed to load');
      } finally {
        setLoading(false);
      }
    };
    load();
  }, []);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;
  if (users.length === 0) return <p>No users</p>;

  return (
    <ul>
      {users.map(u => (
        <li key={u.id}>
          <Link to={`/users/${u.id}`}>{u.name}</Link> ({u.email})
        </li>
      ))}
    </ul>
  );
}

// Task 2
function UserDetails() {
  const { id } = useParams<{ id: string }>();
  const navigate = useNavigate();
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const load = async () => {
      setLoading(true);
      setError(null);
      try {
        const res = await fetch(`${BASE}/users/${id}`);
        if (!res.ok) throw new Error(res.status === 404 ? 'User not found' : `HTTP ${res.status}`);
        setUser(await res.json());
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Failed to load');
      } finally {
        setLoading(false);
      }
    };
    load();
  }, [id]);

  if (loading) return <p>Loading...</p>;
  if (error || !user) return <p>Error: {error}</p>;

  return (
    <div>
      <h2>{user.name}</h2>
      <p>📞 {user.phone}</p>
      <p>🌐 {user.website}</p>
      <button onClick={() => navigate(-1)}>Back</button>
    </div>
  );
}

// Task 3: postService is the solution of "Your Turn: Parts 4 & 5"
function NewPostForm() {
  const [title, setTitle] = useState('');
  const [message, setMessage] = useState('');

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    if (!title.trim()) return setMessage('Title is required');
    try {
      const post = await postService.create({ title, body: 'Hello', userId: 1 });
      setMessage(`Created post #${post.id}`);
      setTitle('');
    } catch {
      setMessage('Could not create the post');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input value={title} onChange={e => setTitle(e.target.value)} placeholder="Post title" />
      <button type="submit">Create</button>
      {message && <p>{message}</p>}
    </form>
  );
}

// Task 4
function SearchUsers() {
  const [searchParams, setSearchParams] = useSearchParams();
  const q = searchParams.get('q') ?? '';
  const [users, setUsers] = useState<User[]>([]);

  useEffect(() => {
    fetch(`${BASE}/users`).then(r => r.json()).then(setUsers);
  }, []);

  const results = users.filter(u => u.name.toLowerCase().includes(q.toLowerCase()));

  return (
    <div>
      <input value={q} onChange={e => setSearchParams({ q: e.target.value })} placeholder="Search users" />
      <ul>{results.map(u => <li key={u.id}>{u.name}</li>)}</ul>
    </div>
  );
}

// Task 5
function ProtectedRoute({ children }: { children: React.ReactNode }) {
  if (localStorage.getItem('user') === null) return <Navigate to="/login" replace />;
  return <>{children}</>;
}
function LoginPage() {
  const navigate = useNavigate();
  const login = () => {
    localStorage.setItem('user', 'demo');
    navigate('/profile');
  };
  return <button onClick={login}>Login</button>;
}
function ProfilePage() {
  const navigate = useNavigate();
  const logout = () => {
    localStorage.removeItem('user');
    navigate('/login');
  };
  return (
    <div>
      <h1>My profile</h1>
      <button onClick={logout}>Logout</button>
    </div>
  );
}

// Challenge
interface Post { id: number; title: string; body: string; }
interface Comment { id: number; name: string; body: string; }

function BlogLayout() {
  return (
    <>
      <nav>
        <NavLink to="/" end>Home</NavLink> | <NavLink to="/about">About</NavLink>
      </nav>
      <Outlet />
    </>
  );
}

function PostsHome() {
  const [posts, setPosts] = useState<Post[]>([]);
  useEffect(() => {
    fetch(`${BASE}/posts?_limit=10`).then(r => r.json()).then(setPosts);
  }, []);
  return (
    <ul>
      {posts.map(p => <li key={p.id}><Link to={`/posts/${p.id}`}>{p.title}</Link></li>)}
    </ul>
  );
}

function PostDetails() {
  const { id } = useParams<{ id: string }>();
  const [post, setPost] = useState<Post | null>(null);
  const [comments, setComments] = useState<Comment[]>([]);

  useEffect(() => {
    const load = async () => {
      const [postRes, commentsRes] = await Promise.all([
        fetch(`${BASE}/posts/${id}`),
        fetch(`${BASE}/posts/${id}/comments`),
      ]);
      setPost(await postRes.json());
      setComments(await commentsRes.json());
    };
    load();
  }, [id]);

  if (!post) return <p>Loading...</p>;
  return (
    <article>
      <h2>{post.title}</h2>
      <p>{post.body}</p>
      <h3>Comments ({comments.length})</h3>
      {comments.map(c => <p key={c.id}><b>{c.name}</b>: {c.body}</p>)}
    </article>
  );
}

function BlogApp() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<BlogLayout />}>
          <Route index element={<PostsHome />} />
          <Route path="about" element={<h2>About this blog</h2>} />
          <Route path="posts/:id" element={<PostDetails />} />
        </Route>
        <Route path="*" element={<h1>404: Page not found</h1>} />
      </Routes>
    </BrowserRouter>
  );
}
```

</details>

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
