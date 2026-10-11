# Session 7: Final Project — Food Ordering Application

**Duration:** 3 hours + homework
**Level:** Advanced — Production Ready
**Type:** Capstone project guide
**Condensed from:** React Session 10
**Stack:** React + TypeScript + Vite + React Router + Zustand + React Query + Axios + Tailwind

---

## Project: Food Ordering App (like UberEats/DoorDash)

Users can browse meals, search & filter, manage a cart, place orders, track order status, and manage their profile — with auth, protected routes, and production-grade architecture.

This session is a **blueprint**: it defines requirements, architecture, data models, and implementation patterns. You build it applying everything from Sessions 1–6.

---

## 1. Requirements

### Functional

| Feature | Capabilities |
|---------|-------------|
| Auth | Login, register, logout, token refresh, protected pages |
| Home | Header nav, hero, categories, popular meals |
| Meals | Search, category filter, sort, pagination, loading/error/empty states |
| Meal Details | Image, description, price, ingredients, nutrition, quantity → cart |
| Cart | List items, quantity controls, remove, totals, clear, persist |
| Checkout | Customer info, address, payment method, order summary, submit |
| Orders | List with status badges, pagination, details with timeline |
| Profile | View/edit name & phone, account info |

### Non-functional

- **Performance:** first paint < 1.5s, TTI < 3.5s, bundle < 500KB gzipped, 60fps
- **Security:** HTTPS, JWT, input validation, XSS/CSRF protection, CORS
- **Accessibility:** WCAG AA, keyboard nav, screen-reader friendly
- **Responsive:** mobile-first, touch-friendly

---

## 2. Routes

```
Public:     /, /login, /register, /meals, /meals/:id, /cart, /404
Protected:  /checkout, /orders, /orders/:id, /profile
```

```tsx
// routes/index.tsx — createBrowserRouter with MainLayout
{
  path: '/',
  element: <Layout />,
  children: [
    { index: true, element: <Home /> },
    { path: 'login', element: <Login /> },
    { path: 'meals', element: <Meals /> },
    { path: 'meals/:id', element: <MealDetails /> },
    { path: 'cart', element: <Cart /> },
    { path: 'checkout', element: <ProtectedRoute><Checkout /></ProtectedRoute> },
    { path: 'orders', element: <ProtectedRoute><Orders /></ProtectedRoute> },
    { path: 'orders/:id', element: <ProtectedRoute><OrderDetails /></ProtectedRoute> },
    { path: 'profile', element: <ProtectedRoute><Profile /></ProtectedRoute> },
    { path: '*', element: <NotFound /> },
  ],
}
```

---

## 3. State Management Strategy

| State type | Tool | Used for |
|-----------|------|----------|
| **Server state** | React Query | meals, categories, orders, profile, cart (server-synced) |
| **Client state** | Zustand | auth user, cart items (local), UI (sidebar, modals) |
| **Local state** | `useState` | form inputs, quantity pickers, selection |
| **Global simple** | Context | theme, language, toasts |

```tsx
// stores/authStore.ts — persisted auth
export const useAuthStore = create<AuthStore>()(
  persist(
    (set) => ({
      user: null,
      isAuthenticated: false,
      setAuth: (user, tokens) => {
        localStorage.setItem('access_token', tokens.access_token);
        localStorage.setItem('refresh_token', tokens.refresh_token);
        set({ user, isAuthenticated: true });
      },
      logout: () => {
        localStorage.clear();
        set({ user: null, isAuthenticated: false });
      },
      updateUser: (updates) =>
        set((s) => ({ user: s.user ? { ...s.user, ...updates } : null })),
    }),
    { name: 'auth-storage', partialize: (s) => ({ user: s.user, isAuthenticated: s.isAuthenticated }) }
  )
);
```

```tsx
// stores/cartStore.ts — persisted cart
export const useCartStore = create<CartStore>()(
  persist(
    (set, get) => ({
      items: [],
      addItem: (item) => set((s) => {
        const existing = s.items.find((i) => i.meal_id === item.meal_id);
        if (existing) {
          return { items: s.items.map((i) =>
            i.meal_id === item.meal_id ? { ...i, quantity: i.quantity + item.quantity } : i) };
        }
        return { items: [...s.items, item] };
      }),
      removeItem: (mealId) =>
        set((s) => ({ items: s.items.filter((i) => i.meal_id !== mealId) })),
      updateQuantity: (mealId, quantity) =>
        set((s) => ({
          items: s.items
            .map((i) => (i.meal_id === mealId ? { ...i, quantity } : i))
            .filter((i) => i.quantity > 0),          // qty 0 → remove
        })),
      clearCart: () => set({ items: [] }),
      getTotal: () => get().items.reduce((t, i) => t + i.meal.price * i.quantity, 0),
      getItemCount: () => get().items.reduce((n, i) => n + i.quantity, 0),
    }),
    { name: 'cart-storage' }
  )
);
```

---

## 4. API Layer

```tsx
// services/apiClient.ts — Axios instance + token attach + refresh-on-401
const apiClient = axios.create({ baseURL: import.meta.env.VITE_API_URL });

apiClient.interceptors.request.use((config) => {
  const token = localStorage.getItem('access_token');
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});
// response interceptor: on 401 → POST /auth/refresh → replay request → else logout
```

```tsx
// services/queryClient.ts
export const queryClient = new QueryClient({
  defaultOptions: { queries: { staleTime: 5 * 60_000, gcTime: 10 * 60_000, retry: 1, refetchOnWindowFocus: false } },
});
```

**Service modules** (`services/api/`): `auth.ts`, `meals.ts`, `cart.ts`, `orders.ts`, `user.ts` — each exporting a typed service object:

```tsx
export const mealsService = {
  getMeals: (f: MealsFilters) => apiClient.get<PaginatedResponse<Meal>>('/meals', { params: f }).then(r => r.data),
  getMeal:  (id: number) => apiClient.get<Meal>(`/meals/${id}`).then(r => r.data),
  getPopularMeals: () => apiClient.get<Meal[]>('/meals/popular').then(r => r.data),
  getCategories: () => apiClient.get<Category[]>('/categories').then(r => r.data),
};
```

### Key endpoints

| Endpoint | Purpose |
|----------|---------|
| `POST /auth/login` / `register` / `logout` / `refresh` | Auth, returns `{ access_token, refresh_token, user }` |
| `GET /meals?page=&limit=&category=&search=` | Paginated meal list |
| `GET /meals/:id`, `GET /meals/popular` | Details / popular |
| `GET /categories` | Category list |
| `GET/POST /cart`, `PUT/DELETE /cart/items/:id` | Cart ops |
| `POST /orders`, `GET /orders`, `GET /orders/:id` | Create + list + details |
| `PUT /user/profile`, `PUT /user/password` | Profile updates |

### Core data models

```ts
interface Meal {
  id: number; name: string; description: string; price: number;
  image: string; category_id: number; rating: number;
  ingredients: string[]; preparation_time: number;
  nutrition?: { calories: number; protein: number; carbs: number; fat: number };
}

interface Order {
  id: number; customer_name: string; phone: string; address: string;
  status: 'pending' | 'preparing' | 'ready' | 'delivered' | 'cancelled';
  total: number; items: OrderItem[];
  payment_method: 'cash' | 'card'; notes?: string;
  created_at: string; estimated_delivery?: string;
}

interface PaginatedResponse<T> {
  data: T[];
  meta: { total: number; page: number; limit: number; total_pages: number };
}
```

---

## 5. Folder Structure

```
src/
├── components/
│   ├── common/        Button, Input, Select, Modal, Toast, LoadingSpinner, EmptyState
│   ├── layout/        Header, Footer, Sidebar
│   ├── auth/          LoginForm, ProtectedRoute
│   ├── meals/         MealCard, MealFilters, MealCardSkeleton, CategoryCard
│   ├── cart/          CartItem, CartSummary, CartBadge
│   ├── orders/        OrderCard, OrderStatusBadge, OrderTimeline
│   └── profile/       ProfileForm
├── pages/             Home, Login, Register, Meals, MealDetails, Cart,
│                      Checkout, Orders, OrderDetails, Profile, NotFound
├── layouts/           MainLayout, AuthLayout
├── services/          apiClient.ts, queryClient.ts, api/{auth,meals,cart,orders,user}.ts
├── stores/            authStore.ts, cartStore.ts, uiStore.ts
├── hooks/             useAuth.ts, useCart.ts, useDebounce.ts
├── types/             user.ts, meal.ts, order.ts, cart.ts, index.ts
├── utils/             format.ts, validation.ts, constants.ts
├── routes/            index.tsx, protectedRoutes.tsx
├── App.tsx / main.tsx
```

Path aliases (`tsconfig` + `vite.config`): `@/components`, `@/pages`, `@/services`, `@/stores`, `@/types`, `@/hooks`.

---

## 6. Setup

```bash
npm create vite@latest food-ordering-app -- --template react-ts
cd food-ordering-app
npm install react-router-dom zustand @tanstack/react-query axios react-hot-toast
npm install -D tailwindcss postcss autoprefixer @types/node
```

`.env`: `VITE_API_URL=http://localhost:5000/api`
`vite.config.ts`: dev proxy `/api` → `http://localhost:5000` + `@` aliases.

```tsx
// App.tsx — two providers, that's all
<QueryClientProvider client={queryClient}>
  <RouterProvider router={router} />
  <Toaster position="top-right" />
</QueryClientProvider>
```

---

## 7. Implementation Patterns

### Reusable base components

- **`Button`** — `variant` (primary/secondary/danger/ghost), `size`, `loading` spinner, `fullWidth`, `forwardRef`.
- **`Input`** — `label`, `error`, `helperText` props; error border styling.
- **`EmptyState`** — icon + title + description + optional CTA button.
- **`OrderStatusBadge`** — `Record<status, {color, label}>` lookup → colored pill.
- **`LoadingSpinner` / `MealCardSkeleton`** — consistent loading UX.

### Login page pattern

```tsx
const loginMutation = useMutation({
  mutationFn: authService.login,
  onSuccess: (data) => {
    setAuth(data.user, { access_token: data.access_token, refresh_token: data.refresh_token });
    toast.success('Login successful!');
    navigate('/');
  },
  onError: (err: any) => toast.error(err.response?.data?.message || 'Login failed'),
});
// form → local validation → loginMutation.mutate(credentials)
```

### Meals list pattern (filters + pagination via query key)

```tsx
const [filters, setFilters] = useState<MealsFilters>({ page: 1, limit: 12, /* from searchParams */ });

const { data, isLoading, error } = useQuery({
  queryKey: ['meals', filters],          // changing filters = new key = auto refetch
  queryFn: () => mealsService.getMeals(filters),
});
```

`MealFilters` owns search input + category select + sort select (seeded from `useSearchParams` so `/meals?category=2` deep-links work) and calls `onFilterChange`. Every state is handled: `isLoading` → spinner, `error` → EmptyState + retry, empty `data.data` → EmptyState, else grid + pagination.

### Cart page

```tsx
// CartItem: +/- quantity, remove at qty 0
// CartSummary: subtotal → 10% tax → total → "Proceed to Checkout" / "Clear Cart"
const subtotal = getTotal();
const tax = subtotal * 0.1;
const total = subtotal + tax;
```

### Checkout flow

```tsx
const createOrderMutation = useMutation({
  mutationFn: ordersService.createOrder,
  onSuccess: (order) => {
    clearCart();
    toast.success('Order placed successfully!');
    navigate(`/orders/${order.id}`);
  },
  onError: (err) => toast.error(err.response?.data?.message || 'Failed to place order'),
});
```

Form: customer info + address + payment method + notes → validate → `mutate` → clear cart → redirect to order details. Empty cart → EmptyState with "Browse Meals".

### Header — multi-store consumption

```tsx
const user = useAuthStore((s) => s.user);
const isAuthenticated = useAuthStore((s) => s.isAuthenticated);
const itemCount = useCartStore((s) => s.getItemCount());
const { mobileMenuOpen, toggleMobileMenu, closeMobileMenu } = useUIStore();
// cart badge, conditional Login/Register vs Orders/Profile/Logout, mobile menu
```

---

## 8. Performance Pass

Apply Session 5–6 techniques where they actually matter:

```tsx
const MealCard = memo(function MealCard({ meal }: MealCardProps) { /* … */ });
const CartItem = memo(/* … */);

const handleAddToCart = useCallback(() => addItem({ … }), [meal, addItem]);

const subtotal = useMemo(() =>
  items.reduce((s, i) => s + i.meal.price * i.quantity, 0), [items]);

// Lazy-load protected/rarely-visited routes
const Checkout = lazy(() => import('@/pages/Checkout'));
const Orders   = lazy(() => import('@/pages/Orders'));
const Profile  = lazy(() => import('@/pages/Profile'));
```

Wrap lazy elements in `<Suspense fallback={<LoadingSpinner />}>` inside the route config.

---

## 9. Checklists

### Code review
- Conventions followed, no `console.log`, no `any`, strict TS, all loading/error/empty states, reusable components, no duplication, clean API layer.

### Security
- JWT stored correctly, refresh flow, secure logout (tokens + `queryClient.clear()`), protected routes, HTTPS-only prod, no secrets client-side, input validation, error messages don't leak internals.

### Performance
- Route-based code splitting, images optimized, memoization where measured, bundle analyzed (<500KB gz), compression enabled.

### Production
- Error tracking (Sentry), analytics, uptime monitoring, CI/CD with tests, staging env, rollback plan, env configs.

---

## 10. Extension Ideas (Final Assignment)

- **Real-time:** WebSocket order status updates + notifications
- **Reviews & ratings** on meals
- **Payment:** Stripe integration, receipts
- **Admin panel:** meal/order management + analytics dashboard
- **PWA:** service worker, offline support
- **Mobile:** React Native version
- **Testing:** unit + integration + E2E, >80% coverage
- **Deploy:** CI/CD → staging → production

Post-course ideas: loyalty program, delivery tracking, i18n/RTL, subscriptions, gift cards, restaurant partnerships.

---

## Review: How to Present This Project (interview prep)

- **Tech choices:** React (ecosystem), TypeScript (safety at scale), Vite (fast HMR), Zustand (tiny client state), React Query (server state: cache/dedup/refetch), Axios (interceptors).
- **State strategy:** React Query = server, Zustand = client, `useState` = local, Context = simple globals.
- **Error handling:** boundaries + try/catch + UI error states + toasts + central API interceptor.
- **Performance story:** Profiler → memoization → lazy routes → bundle analysis.
- **Security story:** JWT + refresh + protected routes + validation + env vars.
- **Testing story:** RTL for components, Jest for logic, E2E for user flows.

---

**Session 7 complete — the React track is done.** Next: Session 8 begins the Node.js backend track — you'll build the API this frontend consumes.
