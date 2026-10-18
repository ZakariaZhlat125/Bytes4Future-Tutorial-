# Session 6: React Query, Authentication & Production Performance

**Duration:** 3 hours
**Level:** Advanced — Production Ready
**Tech:** React + TypeScript + React Query + Zustand + Tailwind CSS
**Condensed from:** React Session 9

---

> **How to use this session**
> - **💡 In simple words**: a plain-language explanation under each topic. Read it first, then the code.
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished code after each part. Fill every `____` and finish every `// TODO`, then run it.
> - **📝 Student Worksheet** (end of session): questions to give students. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.
>
> Exercises use the free practice API **https://jsonplaceholder.typicode.com**. It accepts POST/PUT/DELETE but does **not** really save the data, so a refetched list won't change. Check the **Network** tab to see that the requests happen.

## Session Overview

- **Part 1:** React Query — server state, `useQuery`, caching, `useMutation`, optimistic updates
- **Part 2:** Authentication — tokens, protected routes, roles, logout
- **Part 3:** Performance — lazy loading, code splitting, bundle size, profiling
- **Project:** Production-ready Admin Dashboard architecture

---

## Part 1: React Query — Server State

### Server State vs Client State

> 💡 **In simple words:** Client state belongs to the browser (is the menu open?). Server state is **borrowed**: the real data lives on the server, and you only hold a copy that can become old. Borrowed data needs extra care (caching, refreshing, handling failures), and that is exactly what React Query does.

**Client state** lives in the browser, is instant, and never needs syncing: form inputs, modals, theme, UI preferences → `useState` / Zustand.

**Server state** lives on a backend, can go stale, can fail, and needs caching/refetching/deduplication: users, products, orders → **React Query**.

```tsx
// ❌ Hand-rolled server state — no cache, no dedup, no refetch, lots of boilerplate
const [users, setUsers] = useState([]);
const [isLoading, setIsLoading] = useState(false);
const [error, setError] = useState(null);
useEffect(() => { setIsLoading(true); fetch('/api/users').then(...) }, []);

// ✅ React Query — all of that built in
const { data: users, isLoading, error } = useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers,
});
```

**React Query gives you:** automatic caching, request deduplication, auto-refetch, loading/error states, optimistic updates, pagination support, background refetch, DevTools (~13KB).

> **Rule:** React Query for server state, Zustand for client state. Don't put API data in Zustand — you'd re-implement caching, dedup, and refetching by hand.

### `useQuery`

> 💡 **In simple words:** `useQuery` means "get this data for me and keep it fresh". You give it a name (`queryKey`) and a function that fetches (`queryFn`). It returns the data plus flags like `isLoading` and `isError`, so you don't write `useState`/`useEffect` yourself.

```tsx
import { useQuery } from '@tanstack/react-query';

function UsersList() {
  const { data: users, isLoading, isFetching, isError, error, refetch } = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers,   // must return a Promise; throw on failure
  });

  if (isLoading) return <LoadingSpinner />;               // first fetch only
  if (isError) return <ErrorFallback error={error} onRetry={refetch} />;

  return (
    <div>
      {isFetching && <span>Refreshing…</span>}            {/* background refetch */}
      <button onClick={() => refetch()}>Refresh</button>
      <ul>{users?.map(u => <li key={u.id}>{u.name}</li>)}</ul>
    </div>
  );
}
```

| Returned flag | Meaning |
|---------------|---------|
| `isLoading` | First-ever fetch in progress (no cached data yet) |
| `isFetching` | Any fetch in progress, including background refetches |
| `isError` / `error` | Query threw |
| `isSuccess` | Query resolved |
| `refetch` | Manual refetch function |
| `isStale` | Cached data is stale |

### `queryKey` — the cache identifier

> 💡 **In simple words:** The query key is the **name** of the data in React Query's memory (the cache). Two components that use the same key share the same data and the same request. Put every value the request depends on (like an id or filters) in the key.

Unique key per query; used for caching, dedup, refetching, invalidating.

```tsx
useQuery({ queryKey: ['users'],                          queryFn: fetchUsers });
useQuery({ queryKey: ['user', userId],                   queryFn: () => fetchUser(userId) });
useQuery({ queryKey: ['products', { category, page }],   queryFn: () => fetchProducts({ category, page }) });

// Centralize keys — prevents typos and makes invalidation predictable
const QUERY_KEYS = {
  USERS: 'users',
  USER: (id: number) => ['users', id],
  PRODUCTS: (filters: ProductFilters) => ['products', filters],
} as const;
```

When `queryKey` changes, React Query treats it as a *different* query and refetches automatically — no `useEffect` needed for filters/pagination.

### Options: retry, refetch, caching

> 💡 **In simple words:** These settings control behavior: how many times to retry after a failure, when to refresh automatically, and how long data counts as "fresh". The defaults are fine to start with; change them only when you have a reason.

```tsx
useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers,

  retry: 3,                                                // or (failureCount, err) => boolean
  retryDelay: (attempt) => Math.min(1000 * 2 ** attempt, 30000), // exponential backoff

  refetchOnWindowFocus: true,
  refetchOnReconnect: true,
  refetchOnMount: true,
  refetchInterval: 60_000,                                 // polling
  enabled: isLoggedIn,                                     // conditional query

  staleTime: 2 * 60_000,   // data considered fresh → no refetch for 2 min
  gcTime: 5 * 60_000,      // unused cache entries kept 5 min (default)
});
```

**`staleTime` strategy:** long for rarely-changing data (countries → 1h), short/zero for volatile data (orders → 0), medium for normal data (products → 5min).

### `useMutation` — POST/PUT/DELETE

> 💡 **In simple words:** Queries **read** data; mutations **change** it (create, update, delete). `mutate(data)` sends the request, `isPending` tells you it's in progress, and `onSuccess`/`onError` let you react to the result.

```tsx
const createUser = useMutation({
  mutationFn: (userData: CreateUserInput) => api.post('/users', userData),
  onSuccess: () => queryClient.invalidateQueries({ queryKey: ['users'] }),
  onError: (err) => toast.error(err.message),
});

createUser.mutate({ name, email, role: 'user' });
createUser.isPending;   // in-flight flag
```

Update/delete follow the same shape:

```tsx
const updateUser = useMutation({
  mutationFn: ({ id, data }: { id: number; data: Partial<User> }) =>
    api.put(`/users/${id}`, data),
});

const deleteUser = useMutation({
  mutationFn: (id: number) => api.delete(`/users/${id}`),
});
```

### `invalidateQueries`

> 💡 **In simple words:** After you change data on the server, the cached copy is old. `invalidateQueries` says "this data is outdated, fetch it again", so the list on the screen updates.

Mark cache entries stale so they refetch:

```tsx
const queryClient = useQueryClient();

queryClient.invalidateQueries({ queryKey: ['users'] });           // matches ['users', 1], etc.
queryClient.invalidateQueries({ queryKey: ['users'], refetchType: 'none' }); // mark stale, don't refetch
queryClient.invalidateQueries();                                  // everything
```

### Optimistic updates

> 💡 **In simple words:** Normally the UI waits for the server. With an optimistic update, you update the screen **immediately** as if the request worked (it usually does). If the server says no, you put the old data back. The app feels instant.

Update the UI instantly, roll back on failure:

```tsx
const addTodo = useMutation({
  mutationFn: (text: string) => api.post('/todos', { text }),

  onMutate: async (newText) => {
    await queryClient.cancelQueries({ queryKey: ['todos'] });      // stop in-flight refetches
    const previous = queryClient.getQueryData(['todos']);          // snapshot for rollback
    queryClient.setQueryData(['todos'], (old: Todo[] = []) => [
      ...old, { id: Date.now(), text: newText, completed: false },
    ]);
    return { previous };                                           // → context
  },

  onError: (_err, _vars, ctx) =>
    queryClient.setQueryData(['todos'], ctx?.previous),            // rollback

  onSettled: () =>
    queryClient.invalidateQueries({ queryKey: ['todos'] }),        // reconcile with server
});
```

---

### ✍️ Your Turn: Part 1

Run `npm install @tanstack/react-query` first.

```tsx
// main.tsx: 1. Wrap the app with the provider
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

const queryClient = new ____();

ReactDOM.createRoot(document.getElementById('root')!).render(
  <____ client={____}>
    <App />
  </____>
);
```

```tsx
// UsersList.tsx
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';

const BASE = 'https://jsonplaceholder.typicode.com';
interface User { id: number; name: string; email: string; }

const fetchUsers = async (): Promise<User[]> => {
  const res = await fetch(`${BASE}/users`);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
};

// 2. Load all users
function UsersList() {
  const { data: users, isLoading, isError, error } = useQuery({
    queryKey: [____],
    queryFn: ____,
  });

  // TODO: isLoading → <p>Loading...</p>
  // TODO: isError   → <p>Error: {error.message}</p>
  // TODO: show the names in a list
}

// 3. TODO: write fetchUser(id: number): Promise<User>   → GET /users/:id

// 4. One user: the key MUST include the id (each user is cached separately)
function UserCard({ id }: { id: number }) {
  const { data: user } = useQuery({
    queryKey: ['user', ____],
    queryFn: () => fetchUser(____),
  });
  return <p>{user?.name}</p>;
}

// 5. Create a post, then mark the posts list as old so it refetches
function NewPostButton() {
  const queryClient = ____();
  const createPost = useMutation({
    mutationFn: (title: string) =>
      fetch(`${BASE}/posts`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ title, body: '...', userId: 1 }),
      }).then(r => r.json()),
    onSuccess: () => {
      // TODO: invalidate the ['posts'] query
    },
  });

  return (
    <button onClick={() => createPost.____('Hello')} disabled={createPost.____}>
      {createPost.isPending ? 'Saving...' : 'Create post'}
    </button>
  );
}
```

<details>
<summary>✅ Solution</summary>

```tsx
const queryClient = new QueryClient();

ReactDOM.createRoot(document.getElementById('root')!).render(
  <QueryClientProvider client={queryClient}>
    <App />
  </QueryClientProvider>
);
```

```tsx
function UsersList() {
  const { data: users, isLoading, isError, error } = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers,
  });

  if (isLoading) return <p>Loading...</p>;
  if (isError) return <p>Error: {error.message}</p>;
  return <ul>{users?.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}

const fetchUser = async (id: number): Promise<User> => {
  const res = await fetch(`${BASE}/users/${id}`);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
};

function UserCard({ id }: { id: number }) {
  const { data: user } = useQuery({
    queryKey: ['user', id],
    queryFn: () => fetchUser(id),
  });
  return <p>{user?.name}</p>;
}

// inside NewPostButton
onSuccess: () => {
  queryClient.invalidateQueries({ queryKey: ['posts'] });
},
// ...
<button onClick={() => createPost.mutate('Hello')} disabled={createPost.isPending}>
```

</details>

---

## Part 2: Authentication

### Login flow

> 💡 **In simple words:** Logging in means sending the email and password to the server. If they're correct, the server sends back a **token**, a digital pass. The app saves the token and shows it with every later request to prove who you are.

```
credentials → POST /auth/login → verify → { access_token, refresh_token, user }
→ save tokens (localStorage) → update auth store → redirect to intended page
```

```tsx
function Login() {
  const navigate = useNavigate();
  const login = useAuthStore((s) => s.login);

  const loginMutation = useMutation({
    mutationFn: (creds: { email: string; password: string }) =>
      api.post('/auth/login', creds),
    onSuccess: (data) => {
      localStorage.setItem('access_token', data.access_token);
      localStorage.setItem('refresh_token', data.refresh_token);
      login(data.user);
      navigate('/dashboard');
    },
  });
  // form → loginMutation.mutate({ email, password })
}
```

### Access & refresh tokens

> 💡 **In simple words:** The **access token** is like a short-term visitor badge: it opens the doors but expires quickly, so a stolen one is useful only briefly. The **refresh token** is used only to get a new badge without typing the password again.

- **Access token** (JWT, ~15–30 min): sent as `Authorization: Bearer <token>` on every request.
- **Refresh token** (long-lived): exchanged for a new access token when the old one expires — avoids forcing re-login.

Attach + auto-refresh via Axios interceptors:

```tsx
// lib/api.ts
api.interceptors.request.use((config) => {
  const token = localStorage.getItem('access_token');
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

api.interceptors.response.use(
  (res) => res,
  async (error) => {
    const req = error.config;
    if (error.response?.status === 401 && !req._retry) {
      req._retry = true;                                   // retry once
      try {
        const { data } = await axios.post('/auth/refresh', {
          refresh_token: localStorage.getItem('refresh_token'),
        });
        localStorage.setItem('access_token', data.access_token);
        req.headers.Authorization = `Bearer ${data.access_token}`;
        return api(req);                                   // replay original request
      } catch {
        localStorage.clear();
        window.location.href = '/login';
      }
    }
    return Promise.reject(error);
  }
);
```

### Authentication vs Authorization

> 💡 **In simple words:** **Authentication** is showing your ID at the entrance: who are you? **Authorization** is which rooms your ID card opens: what are you allowed to do?

- **Authentication** — *who are you?* (login, tokens)
- **Authorization** — *what may you do?* (roles, permissions)

```tsx
function DeleteUserButton({ userId }: { userId: number }) {
  const user = useAuthStore((s) => s.user);
  if (!user) return <Link to="/login">Login required</Link>;   // authN
  if (user.role !== 'admin') return <p>No permission</p>;      // authZ
  return <button onClick={() => deleteUser(userId)}>Delete</button>;
}
```

### Protected routes + roles

> 💡 **In simple words:** It's the same guard idea as in Session 4, with a second check: a page can also require a **role**. A normal user who tries to open `/admin` is sent to an "unauthorized" page.

```tsx
function ProtectedRoute({ children, allowedRoles }: Props) {
  const { isAuthenticated, user } = useAuthStore();
  if (!isAuthenticated) return <Navigate to="/login" replace />;
  if (allowedRoles && !allowedRoles.includes(user?.role ?? '')) {
    return <Navigate to="/unauthorized" replace />;
  }
  return <>{children}</>;
}

<Route path="/admin" element={
  <ProtectedRoute allowedRoles={['admin']}>
    <AdminPanel />
  </ProtectedRoute>
} />
```

Role-based permission helper:

```tsx
const ROLE_PERMISSIONS: Record<string, string[]> = {
  user: ['products.read', 'orders.create', 'orders.read'],
  moderator: ['products.read', 'products.write', 'orders.read', 'orders.update'],
  admin: ['*'],
};

const hasPermission = (user: User | null, perm: string) =>
  !!user && (ROLE_PERMISSIONS[user.role]?.includes('*') ||
             ROLE_PERMISSIONS[user.role]?.includes(perm));
```

### Logout

> 💡 **In simple words:** Logging out must remove everything that belongs to the user: the tokens, the user in the store and the cached server data. Then the next person on the same computer sees nothing private.

```tsx
const handleLogout = () => {
  localStorage.removeItem('access_token');
  localStorage.removeItem('refresh_token');
  logout();                    // clear auth store
  queryClient.clear();         // drop all cached server data
  navigate('/login');
};
```

(Optionally `POST /auth/logout` first to revoke the refresh token server-side.)

---

### ✍️ Your Turn: Part 2

A fake login (no backend) with roles, using Zustand + React Router.

```tsx
import { create } from 'zustand';
import { Navigate, useNavigate } from 'react-router-dom';
import { useQueryClient } from '@tanstack/react-query';

interface User { id: number; name: string; role: 'user' | 'admin'; }

// 1. Complete the auth store
interface AuthStore {
  user: User | null;
  isAuthenticated: boolean;
  login: (user: User) => void;
  logout: () => void;
}

const useAuthStore = create<AuthStore>((set) => ({
  user: ____,
  isAuthenticated: ____,
  login: (user) => set({ ____ }),
  logout: () => set({ ____ }),
}));

// 2. Login page: choose a role
function LoginPage() {
  const login = useAuthStore((s) => s.login);
  const navigate = useNavigate();

  const loginAs = (role: User['role']) => {
    // TODO: login({ id: 1, name: 'Sara', role }), then go to /dashboard
  };

  return (
    <div>
      <button onClick={() => loginAs('user')}>Login as user</button>
      <button onClick={() => loginAs('admin')}>Login as admin</button>
    </div>
  );
}

// 3. Protected route with optional roles
function ProtectedRoute({ children, allowedRoles }: {
  children: React.ReactNode;
  allowedRoles?: User['role'][];
}) {
  const isAuthenticated = useAuthStore((s) => s.isAuthenticated);
  const user = useAuthStore((s) => s.user);

  if (____) return <Navigate to="/login" replace />;
  if (allowedRoles && user && !allowedRoles.____(user.role)) {
    return <Navigate to="/unauthorized" replace />;
  }
  return <>{children}</>;
}

// 4. TODO: routes → /login, /dashboard (any logged-in user),
//    /admin (admin only), /unauthorized

// 5. TODO: LogoutButton → logout() + queryClient.clear() + go to /login

// 6. Authentication or authorization?
//    a) checking the email and password   → ____
//    b) only admins can delete users      → ____
```

<details>
<summary>✅ Solution</summary>

```tsx
const useAuthStore = create<AuthStore>((set) => ({
  user: null,
  isAuthenticated: false,
  login: (user) => set({ user, isAuthenticated: true }),
  logout: () => set({ user: null, isAuthenticated: false }),
}));

const loginAs = (role: User['role']) => {
  login({ id: 1, name: 'Sara', role });
  navigate('/dashboard');
};

// in ProtectedRoute
if (!isAuthenticated) return <Navigate to="/login" replace />;
if (allowedRoles && user && !allowedRoles.includes(user.role)) {
  return <Navigate to="/unauthorized" replace />;
}

// 4
<Routes>
  <Route path="/login" element={<LoginPage />} />
  <Route path="/dashboard" element={<ProtectedRoute><h1>Dashboard</h1></ProtectedRoute>} />
  <Route path="/admin" element={<ProtectedRoute allowedRoles={['admin']}><h1>Admin panel</h1></ProtectedRoute>} />
  <Route path="/unauthorized" element={<h1>⛔ You don't have permission</h1>} />
</Routes>

// 5
function LogoutButton() {
  const logout = useAuthStore((s) => s.logout);
  const queryClient = useQueryClient();
  const navigate = useNavigate();

  const handleLogout = () => {
    logout();
    queryClient.clear();   // the next user must not see old data
    navigate('/login');
  };
  return <button onClick={handleLogout}>Logout</button>;
}
```

6. a) authentication (who are you?) · b) authorization (what are you allowed to do?)

</details>

---

## Part 3: Performance — Beyond Memoization

Session 5 covered `useMemo` / `useCallback` / `React.memo`. This part adds **load-time** performance.

### Lazy loading & Suspense

> 💡 **In simple words:** Normally the browser downloads the code of every page at the start, even pages the user never opens. `lazy` downloads a page's code only when that page is opened. `Suspense` shows a fallback (like "Loading...") while it downloads.

```tsx
import { lazy, Suspense } from 'react';

const Dashboard = lazy(() => import('./pages/Dashboard'));   // route-based code splitting
const Settings  = lazy(() => import('./pages/Settings'));

function App() {
  return (
    <Suspense fallback={<PageLoader />}>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/settings" element={<Settings />} />
      </Routes>
    </Suspense>
  );
}
```

Also great for modals/heavy components — split them out so they only load when opened:

```tsx
{isModalOpen && (
  <Suspense fallback={<div>Loading…</div>}>
    <UserModal onClose={close} />
  </Suspense>
)}
```

Named exports need a `.then` remap:

```tsx
const Dashboard = lazy(() => import('./Dashboard').then(m => ({ default: m.Dashboard })));
```

### Code splitting strategies

> 💡 **In simple words:** Code splitting means cutting your app's JavaScript into smaller files called **chunks**. The best place to cut is usually per page (per route).

- **Route-based** (best ROI): one lazy chunk per page
- **Component-based**: charts, rich editors, admin panels
- **Feature-based**: whole feature folders (`./features/admin`)
- Vite/Webpack emit separate `.js` chunks automatically for `import()`

### Bundle size

> 💡 **In simple words:** The **bundle** is the JavaScript the user must download. A bigger bundle means a slower first load, especially on phones. Check its size after `npm run build`, and avoid a heavy library when a small one does the job.

```bash
npm run build                                  # Vite shows chunk sizes
npm i -D rollup-plugin-visualizer              # interactive treemap
```

Reduction tactics:
- Named imports for tree-shaking (`import { Button }` not `import * as UI`)
- Lazy-load heavy libs (`lazy(() => import('chart.js/auto'))`)
- Prefer small alternatives (`dayjs` 2KB vs `moment` 67KB; `date-fns` is tree-shakable)
- Enable gzip/brotli on the server

### React DevTools & Profiler

> 💡 **In simple words:** React DevTools is a browser extension that shows your components with their props and state. Its **Profiler** shows which components rendered and how long each one took. Always measure before you optimize.

- **Components tab:** inspect props/state, enable *"Highlight updates when components render"* to see unnecessary re-renders live.
- **Profiler tab:** record an interaction → ranked list of slowest components, flame graph, per-render timings.

Programmatic profiling:

```tsx
<Profiler id="App" onRender={(id, phase, actualDuration) =>
  console.log({ id, phase, actualDuration })}>
  <Dashboard />
</Profiler>
```

---

### ✍️ Your Turn: Part 3

```tsx
import { lazy, Suspense } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';

// 1. Load these pages ONLY when the user opens them.
//    (lazy needs a default export: `export default function Dashboard() {...}`)
const Dashboard = ____(() => import('./pages/Dashboard'));
const Settings = ____(() => ____('./pages/Settings'));

function App() {
  return (
    <BrowserRouter>
      <____ fallback={<p>Loading page...</p>}>
        <Routes>
          <Route path="/dashboard" element={<Dashboard />} />
          <Route path="/settings" element={<Settings />} />
        </Routes>
      </____>
    </BrowserRouter>
  );
}

// 2. This page uses a NAMED export: export function Reports() {...}
//    Complete the lazy import:
const Reports = lazy(() => import('./pages/Reports').then(m => ({ default: ____ })));

// 3. Investigate:
//    a) Run `npm run build`. How many .js files are in dist/assets?
//       Change the pages back to normal imports and build again. What changed?
//    b) Run the app, open DevTools → Network → JS, and go to /settings.
//       Which new file is downloaded?
```

<details>
<summary>✅ Solution</summary>

```tsx
const Dashboard = lazy(() => import('./pages/Dashboard'));
const Settings = lazy(() => import('./pages/Settings'));

<Suspense fallback={<p>Loading page...</p>}>
  {/* routes */}
</Suspense>

const Reports = lazy(() => import('./pages/Reports').then(m => ({ default: m.Reports })));
```

3. a) With `lazy` you get extra small `.js` files (one chunk per lazy page). Without it everything is in one bigger file.
   b) A file like `Settings-xxxx.js` is downloaded only when you open `/settings`.

</details>

---

## Project: Admin Dashboard Architecture

> 💡 **In simple words:** Each tool has one job:
> - **React Query**: server data.
> - **Zustand**: shared client data.
> - **The Router**: pages.
> - **The API layer**: HTTP.
>
> Keeping these jobs separate makes a big app easy to understand.

```
┌─────────────────────────────────────────┐
│   UI Layer (Pages, Components, Layouts) │
└───────┬──────────────┬──────────┬───────┘
        ▼              ▼          ▼
  React Query      Zustand    React Router
  (server state) (client UI)  (navigation)
        │
        ▼
   API Layer (Axios + interceptors)
        │
        ▼
   Backend API
```

**Responsibilities:**

| Layer | Owns | Examples |
|-------|------|----------|
| Components | Rendering, interaction, local UI state | Dashboard, tables, forms |
| React Query | Fetching, caching, mutations, refetch | `useQuery(['users'])`, `useMutation` |
| Zustand | Global *client* state | `sidebarOpen`, `selectedTab`, `theme`, auth user |
| Router | Navigation, guards, params | `/users/:id`, `ProtectedRoute` |
| API layer | HTTP, tokens, global errors | interceptors, refresh logic |

### Setup

```bash
npm install @tanstack/react-query zustand react-router-dom axios
```

```tsx
// lib/queryClient.ts
export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60_000,
      gcTime: 10 * 60_000,
      retry: 1,
      refetchOnWindowFocus: false,
    },
  },
});

// App.tsx — wrap the tree
<QueryClientProvider client={queryClient}>
  <BrowserRouter>{/* routes */}</BrowserRouter>
</QueryClientProvider>
```

### API layer (`lib/api/*.ts`)

```tsx
// lib/api/users.ts — typed API functions
export const fetchUsers  = async (): Promise<User[]> => (await api.get('/users')).data;
export const createUser  = async (u: CreateUserInput) => (await api.post('/users', u)).data;
export const updateUser  = async (id: number, u: Partial<User>) => (await api.put(`/users/${id}`, u)).data;
export const deleteUser  = async (id: number) => api.delete(`/users/${id}`);
```

### Stores — Zustand for client state only

```tsx
const useAuthStore = create<AuthStore>((set) => ({
  user: null,
  isAuthenticated: false,
  login: (user) => set({ user, isAuthenticated: true }),
  logout: () => set({ user: null, isAuthenticated: false }),
}));

const useUIStore = create<UIStore>((set) => ({
  sidebarOpen: true,
  selectedTab: 'dashboard',
  toggleSidebar: () => set((s) => ({ sidebarOpen: !s.sidebarOpen })),
  setSelectedTab: (tab) => set({ selectedTab: tab }),
}));
```

### Pages pattern (Users as template)

```tsx
function Users() {
  const queryClient = useQueryClient();
  const { data: users, isLoading } = useQuery({ queryKey: [QUERY_KEYS.USERS], queryFn: fetchUsers });
  const deleteMutation = useMutation({
    mutationFn: deleteUser,
    onSuccess: () => queryClient.invalidateQueries({ queryKey: [QUERY_KEYS.USERS] }),
  });
  // table + <button onClick={() => deleteMutation.mutate(user.id)}>Delete</button>
}
```

Dashboard page composes several `useQuery` calls (users, products, orders) into stat cards — requests run in parallel and are deduplicated automatically.

---

## Production Checklist (condensed)

- **Security:** HTTPS, env vars, CORS, input validation, XSS/CSRF protection, secure headers
- **Performance:** bundle analysis, code splitting, image optimization, CDN, compression, tree-shaking
- **SEO/a11y:** meta/OG tags, semantic HTML, ARIA, keyboard nav, WCAG AA contrast
- **Testing:** unit + integration + E2E, >80% coverage, cross-browser
- **Monitoring:** error tracking (Sentry), analytics, uptime, logging, alerts
- **Deployment:** CI/CD, staging env, rollback plan, env configs
- **UX:** loading/error/empty states, responsive, fast first load

---

## 📝 Student Worksheet

### A. Quick Questions (answer in 1–2 sentences)

1. What is the difference between server state and client state? Which tool do we use for each?
2. What is the `queryKey` used for?
3. What is the difference between `isLoading` and `isFetching`?
4. When do you use `useQuery`, and when do you use `useMutation`?
5. What does `invalidateQueries` do? When do you call it?
6. What is the difference between `staleTime` and `gcTime`?
7. What is the difference between an access token and a refresh token?
8. What is the difference between authentication and authorization?
9. Why do we call `queryClient.clear()` on logout?
10. What does `lazy()` + `<Suspense>` do, and why does it make the app faster?

<details>
<summary>✅ Solution</summary>

1. Client state lives in the browser (theme, modal open), so we use `useState` or Zustand. Server state comes from an API, so we use React Query.
2. It is the name of the cached data. React Query uses it for caching, sharing data between components, and refetching. When the key changes, it fetches again.
3. `isLoading` is true only for the first load (no data yet). `isFetching` is true for any request, including background refreshes.
4. `useQuery` reads data (GET). `useMutation` changes data (POST, PUT, PATCH, DELETE).
5. It marks cached data as old so it gets fetched again. Call it after a mutation succeeds.
6. `staleTime`: how long the data counts as fresh (no refetch). `gcTime`: how long unused data stays in memory before it is deleted.
7. The access token is short-lived and sent with every request. The refresh token is long-lived and only used to get a new access token.
8. Authentication: **who are you?** (login). Authorization: **what are you allowed to do?** (roles and permissions).
9. So the next person who logs in on the same browser doesn't see the previous user's cached data.
10. It loads a page's code only when that page is opened, so the first download is smaller and the app starts faster.

</details>

### B. Find the Bug

```tsx
// Bug 1: opening user 2 after user 1 still shows user 1
function UserCard({ id }: { id: number }) {
  const { data } = useQuery({ queryKey: ['user'], queryFn: () => fetchUser(id) });
  return <p>{data?.name}</p>;
}

// Bug 2: after creating a todo, the list doesn't refresh
const addTodo = useMutation({
  mutationFn: (title: string) => api.post('/todos', { title }),
});

// Bug 3: the server returns 500, but React Query shows no error
const fetchUsers = () => fetch('/api/users').then(r => r.json());

// Bug 4: after logout, the next user sees the previous user's orders for a moment
const handleLogout = () => {
  logout();
  navigate('/login');
};

// Bug 5: the page crashes with "Element type is invalid"
// pages/Settings.tsx has:  export function Settings() { ... }
const Settings = lazy(() => import('./pages/Settings'));
```

<details>
<summary>✅ Solution</summary>

```tsx
// Bug 1: put the id in the key
useQuery({ queryKey: ['user', id], queryFn: () => fetchUser(id) });

// Bug 2: invalidate the list after success
const addTodo = useMutation({
  mutationFn: (title: string) => api.post('/todos', { title }),
  onSuccess: () => queryClient.invalidateQueries({ queryKey: ['todos'] }),
});

// Bug 3: fetch doesn't throw on HTTP errors, so throw yourself
const fetchUsers = async () => {
  const res = await fetch('/api/users');
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
};

// Bug 4: clear the cache
const handleLogout = () => {
  logout();
  queryClient.clear();
  navigate('/login');
};

// Bug 5: lazy needs a default export, so map the named export
const Settings = lazy(() => import('./pages/Settings').then(m => ({ default: m.Settings })));
```

</details>

### C. Coding Tasks

**Task 1: Posts with React Query (easy).** Show the first 10 posts (`/posts?_limit=10`) with loading and error states. Add a **Refresh** button (`refetch`) and show `Refreshing...` while `isFetching` is true.

**Task 2: Post viewer (medium).** A `<select>` with post ids 1–10 (start with "Choose a post"). When an id is chosen, load that post with `queryKey: ['post', id]`. The query must not run while nothing is chosen (use `enabled`).

**Task 3: Create post form (medium).** A title input + **Create** button using `useMutation`. While saving, the button shows `Saving...` and is disabled. On success, show `Post #101 created`, clear the input and invalidate `['posts']`.

**Task 4: Role-based button (medium).** `DeleteUserButton` uses the auth store from "Your Turn: Part 2": guests see `Login required`, users see `No permission`, admins see the **Delete** button.

**Task 5: Lazy pages (easy).** Make Home, About and Contact lazy pages with one `Suspense` fallback. Confirm in the Network tab that each page file loads only when opened.

**Challenge: Pagination.** Show posts 5 per page (`/posts?_page=1&_limit=5`) with **Prev** and **Next** buttons and `Page 2`. Use `queryKey: ['posts', page]` and `placeholderData: keepPreviousData` so the old page stays visible while the next one loads. Disable **Prev** on page 1 and **Next** on page 20.

<details>
<summary>✅ Solution: Tasks 1–5 and Challenge</summary>

```tsx
import { lazy, Suspense, useState } from 'react';
import { useQuery, useMutation, useQueryClient, keepPreviousData } from '@tanstack/react-query';
import { Routes, Route } from 'react-router-dom';

const BASE = 'https://jsonplaceholder.typicode.com';
interface Post { id: number; title: string; body: string; }

async function getJson<T>(url: string): Promise<T> {
  const res = await fetch(url);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
}

// Task 1
function PostsList() {
  const { data: posts, isLoading, isError, error, isFetching, refetch } = useQuery({
    queryKey: ['posts'],
    queryFn: () => getJson<Post[]>(`${BASE}/posts?_limit=10`),
  });

  if (isLoading) return <p>Loading...</p>;
  if (isError) return <p>Error: {error.message}</p>;

  return (
    <div>
      <button onClick={() => refetch()}>Refresh</button>
      {isFetching && <span> Refreshing...</span>}
      <ul>{posts?.map(p => <li key={p.id}>{p.title}</li>)}</ul>
    </div>
  );
}

// Task 2
function PostViewer() {
  const [id, setId] = useState<number | null>(null);

  const { data: post, isLoading } = useQuery({
    queryKey: ['post', id],
    queryFn: () => getJson<Post>(`${BASE}/posts/${id}`),
    enabled: id !== null,          // don't fetch until an id is chosen
  });

  return (
    <div>
      <select value={id ?? ''} onChange={e => setId(e.target.value ? Number(e.target.value) : null)}>
        <option value="">Choose a post</option>
        {Array.from({ length: 10 }, (_, i) => i + 1).map(n => (
          <option key={n} value={n}>Post {n}</option>
        ))}
      </select>
      {isLoading && <p>Loading...</p>}
      {post && (
        <article>
          <h3>{post.title}</h3>
          <p>{post.body}</p>
        </article>
      )}
    </div>
  );
}

// Task 3
function CreatePostForm() {
  const [title, setTitle] = useState('');
  const [message, setMessage] = useState('');
  const queryClient = useQueryClient();

  const createPost = useMutation({
    mutationFn: async (newTitle: string): Promise<Post> => {
      const res = await fetch(`${BASE}/posts`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ title: newTitle, body: '...', userId: 1 }),
      });
      if (!res.ok) throw new Error(`HTTP ${res.status}`);
      return res.json();
    },
    onSuccess: (post) => {
      setMessage(`Post #${post.id} created`);
      setTitle('');
      queryClient.invalidateQueries({ queryKey: ['posts'] });
    },
    onError: (err) => setMessage(`Failed: ${err.message}`),
  });

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    if (title.trim()) createPost.mutate(title.trim());
  };

  return (
    <form onSubmit={handleSubmit}>
      <input value={title} onChange={e => setTitle(e.target.value)} placeholder="Title" />
      <button type="submit" disabled={createPost.isPending}>
        {createPost.isPending ? 'Saving...' : 'Create'}
      </button>
      {message && <p>{message}</p>}
    </form>
  );
}

// Task 4
function DeleteUserButton({ userId }: { userId: number }) {
  const user = useAuthStore((s) => s.user);
  if (!user) return <p>Login required</p>;
  if (user.role !== 'admin') return <p>No permission</p>;
  return <button onClick={() => console.log('delete', userId)}>Delete</button>;
}

// Task 5
const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
const Contact = lazy(() => import('./pages/Contact'));

function LazyApp() {
  return (
    <Suspense fallback={<p>Loading page...</p>}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </Suspense>
  );
}

// Challenge
function PagedPosts() {
  const [page, setPage] = useState(1);
  const lastPage = 20;   // 100 posts / 5 per page

  const { data: posts, isLoading, isFetching } = useQuery({
    queryKey: ['posts', page],
    queryFn: () => getJson<Post[]>(`${BASE}/posts?_page=${page}&_limit=5`),
    placeholderData: keepPreviousData,
  });

  if (isLoading) return <p>Loading...</p>;

  return (
    <div>
      <ul>{posts?.map(p => <li key={p.id}>{p.id}. {p.title}</li>)}</ul>
      <button onClick={() => setPage(p => p - 1)} disabled={page === 1}>Prev</button>
      <span> Page {page} {isFetching && '⏳'} </span>
      <button onClick={() => setPage(p => p + 1)} disabled={page === lastPage}>Next</button>
    </div>
  );
}
```

</details>

---

## Exercises

1. **Users query:** `fetchUsers` + `useQuery` with loading/error states and `retry: 3`.
2. **Create-user mutation:** `useMutation` + `onSuccess` → `invalidateQueries`, pending-state button.
3. **Role-guarded route:** `ProtectedRoute` checking `isAuthenticated` + `user.role` → `/login` or `/unauthorized`.
4. **Optimize:** memoize a filtered+sorted product list (`useMemo`), memoize `ProductCard` (`React.memo` + `useCallback` handlers).
5. **Lazy pages:** lazy-load Dashboard/Users/Products/Orders behind `Suspense` + error boundary.

## Homework

- Finish the admin panel: full Users/Products/Orders CRUD with React Query mutations + optimistic updates.
- Add search/filters/pagination (query keys + `enabled`), bulk delete, CSV export.
- Lazy-load all routes; analyze and shrink the bundle.
- Write tests for API functions and mutations (Jest + React Testing Library).
- Document endpoints and draw the architecture diagram.

## Review Questions

1. Server state vs client state — and which tool manages each?
2. What does `queryKey` control? Why include params in it?
3. `isLoading` vs `isFetching`?
4. `useQuery` vs `useMutation` — which HTTP verbs belong where?
5. What does `invalidateQueries` do?
6. Walk through an optimistic update lifecycle (`onMutate`/`onError`/`onSettled`).
7. Access token vs refresh token?
8. Authentication vs authorization?
9. `staleTime` vs `gcTime`?
10. Why not store API data in Zustand?

## Interview Questions (with pointers)

1. **Describe your React app architecture:** React Query (server) + Zustand (client) + Router (navigation) + API layer — each with one responsibility.
2. **How do you handle caching?** React Query for server data (`staleTime`/`gcTime`), `persist` for client state.
3. **How do you optimize a slow component?** Profiler first → `React.memo`/`useMemo`/`useCallback` → lazy loading → code splitting.
4. **Auth implementation?** Access token in storage, Axios interceptor attaches it, refresh-on-401 in the response interceptor, `ProtectedRoute` + role checks, tokens cleared on logout.
5. **React Query or Redux?** Different jobs — React Query for server state, Redux/Zustand for complex client state; often used together.

## Common Mistakes

- ❌ Storing API data in Zustand/manual state (no cache/dedup/refetch)
- ❌ Forgetting to invalidate related queries after a mutation
- ❌ Vague query keys (`['data']`) — collisions and unpredictable caching
- ❌ Checking auth on mount only, without loading state → flicker or security gap
- ❌ Not rolling back optimistic updates on error
- ❌ Wrapping everything in `React.memo`/`useMemo` without profiling
- ❌ Shipping a monolithic bundle — no lazy routes

---

## Additional Resources

- [TanStack Query Docs](https://tanstack.com/query/latest) · [Zustand](https://zustand-demo.pmnd.rs/)
- [React Performance](https://react.dev/learn/render-and-commit) · [web.dev/performance](https://web.dev/performance/)

---

**Session 6 complete.** Next: Session 7 — React Final Project.
