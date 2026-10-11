# Session 6: React Query, Authentication & Production Performance

**Duration:** 3 hours
**Level:** Advanced — Production Ready
**Tech:** React + TypeScript + React Query + Zustand + Tailwind CSS
**Condensed from:** React Session 9

---

## Session Overview

- **Part 1:** React Query — server state, `useQuery`, caching, `useMutation`, optimistic updates
- **Part 2:** Authentication — tokens, protected routes, roles, logout
- **Part 3:** Performance — lazy loading, code splitting, bundle size, profiling
- **Project:** Production-ready Admin Dashboard architecture

---

## Part 1: React Query — Server State

### Server State vs Client State

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

Mark cache entries stale so they refetch:

```tsx
const queryClient = useQueryClient();

queryClient.invalidateQueries({ queryKey: ['users'] });           // matches ['users', 1], etc.
queryClient.invalidateQueries({ queryKey: ['users'], refetchType: 'none' }); // mark stale, don't refetch
queryClient.invalidateQueries();                                  // everything
```

### Optimistic updates

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

## Part 2: Authentication

### Login flow

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

## Part 3: Performance — Beyond Memoization

Session 5 covered `useMemo` / `useCallback` / `React.memo`. This part adds **load-time** performance.

### Lazy loading & Suspense

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

- **Route-based** (best ROI): one lazy chunk per page
- **Component-based**: charts, rich editors, admin panels
- **Feature-based**: whole feature folders (`./features/admin`)
- Vite/Webpack emit separate `.js` chunks automatically for `import()`

### Bundle size

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

## Project: Admin Dashboard Architecture

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
