# Session 7: Final Project — Food Ordering Application

**Duration:** 3 hours + homework
**Level:** Advanced — Production Ready
**Type:** Capstone project guide
**Condensed from:** React Session 10
**Stack:** React + TypeScript + Vite + React Router + Zustand + React Query + Axios + Tailwind

---

> **How to use this session**
> - **💡 In simple words**: a plain-language explanation under each topic. Read it first, then the code.
> - **Complete examples**: every normal code block. Read it, type it yourself, run it.
> - **✍️ Your Turn**: unfinished starter code for each milestone. Fill every `____` and finish every `// TODO`.
> - **📝 Student Worksheet** (end of session): planning questions, a code-review exercise, the milestone plan and a grading rubric. Answers are hidden under **✅ Solution**. Delete those blocks before sharing if students should not see them.

## Project: Food Ordering App (like UberEats/DoorDash)

Users can browse meals, search & filter, manage a cart, place orders, track order status, and manage their profile — with auth, protected routes, and production-grade architecture.

This session is a **blueprint**: it defines requirements, architecture, data models, and implementation patterns. You build it applying everything from Sessions 1–6.

---

## 1. Requirements

> 💡 **In simple words:** Before writing code, professionals write down **what** the app must do (functional requirements: the features) and **how well** it must do it (non-functional requirements: speed, security, accessibility). This list is your to-do list and your definition of "done".

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

> 💡 **In simple words:** List every page and its URL first. **Public** pages are for anyone; **protected** pages need a login. This list becomes your router configuration.

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

> 💡 **In simple words:** Decide where each piece of data lives **before** you code:
> - Data from the server → React Query.
> - Data many components share → Zustand.
> - Data used by one component → `useState`.

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

### ✍️ Your Turn: Milestone 1 (Types + Cart Store)

```ts
// src/types/meal.ts
export interface Meal {
  id: ____;
  name: ____;
  description: string;
  price: ____;
  image: string;
  category_id: number;
  rating: number;
  ingredients: ____;          // a list of strings
  preparation_time: number;   // minutes
}

export interface Category {
  id: number;
  name: string;
}

// src/types/cart.ts
import type { Meal } from './meal';

export interface CartItem {
  meal_id: number;
  meal: ____;                 // the full meal object
  quantity: number;
}
```

```ts
// src/stores/cartStore.ts
import { create } from 'zustand';
import type { CartItem } from '@/types/cart';

interface CartStore {
  items: CartItem[];
  addItem: (item: CartItem) => void;
  removeItem: (mealId: number) => void;
  updateQuantity: (mealId: number, quantity: number) => void;
  clearCart: () => void;
  getTotal: () => number;
  getItemCount: () => number;
}

export const useCartStore = create<CartStore>()((set, get) => ({
  items: [],

  addItem: (item) =>
    set((s) => {
      const existing = s.items.find((i) => i.meal_id === ____);
      if (existing) {
        // TODO: return items where this meal's quantity is increased by item.quantity
      }
      return { items: ____ };
    }),

  removeItem: (mealId) => /* TODO: keep every item except this meal */,

  updateQuantity: (mealId, quantity) => /* TODO: change the quantity; if it becomes 0, remove the item */,

  clearCart: () => set({ items: ____ }),

  getTotal: () => get().items.reduce((total, i) => total + ____, 0),

  getItemCount: () => /* TODO: sum of all quantities */,
}));

// LAST STEP: wrap the store with persist(..., { name: 'cart-storage' }).
// Add something to the cart, refresh the page: the cart must still be there.
```

<details>
<summary>✅ Solution</summary>

```ts
export interface Meal {
  id: number;
  name: string;
  description: string;
  price: number;
  image: string;
  category_id: number;
  rating: number;
  ingredients: string[];
  preparation_time: number;
}

export interface CartItem {
  meal_id: number;
  meal: Meal;
  quantity: number;
}
```

```ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

export const useCartStore = create<CartStore>()(
  persist(
    (set, get) => ({
      items: [],

      addItem: (item) =>
        set((s) => {
          const existing = s.items.find((i) => i.meal_id === item.meal_id);
          if (existing) {
            return {
              items: s.items.map((i) =>
                i.meal_id === item.meal_id ? { ...i, quantity: i.quantity + item.quantity } : i
              ),
            };
          }
          return { items: [...s.items, item] };
        }),

      removeItem: (mealId) =>
        set((s) => ({ items: s.items.filter((i) => i.meal_id !== mealId) })),

      updateQuantity: (mealId, quantity) =>
        set((s) => ({
          items: s.items
            .map((i) => (i.meal_id === mealId ? { ...i, quantity } : i))
            .filter((i) => i.quantity > 0),
        })),

      clearCart: () => set({ items: [] }),

      getTotal: () => get().items.reduce((total, i) => total + i.meal.price * i.quantity, 0),

      getItemCount: () => get().items.reduce((n, i) => n + i.quantity, 0),
    }),
    { name: 'cart-storage' }
  )
);
```

</details>

---

## 4. API Layer

> 💡 **In simple words:** All communication with the backend goes through one Axios client and a few service files. Components never write URLs themselves; they call functions like `mealsService.getMeals()`.

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

### 🧰 No Backend Yet? Use a Fake API (json-server)

You build the real backend in Sessions 8–12. Until then, **json-server** turns one JSON file into a REST API that really saves data.

Create `db.json` in the project root:

```json
{
  "categories": [
    { "id": 1, "name": "Burgers" },
    { "id": 2, "name": "Pizza" },
    { "id": 3, "name": "Salads" }
  ],
  "meals": [
    { "id": 1, "name": "Classic Burger", "description": "Beef, cheese and lettuce", "price": 8.5, "image": "https://placehold.co/400x300?text=Burger", "category_id": 1, "rating": 4.5, "ingredients": ["beef", "cheese", "lettuce", "bun"], "preparation_time": 15 },
    { "id": 2, "name": "Chicken Burger", "description": "Crispy chicken with sauce", "price": 7.5, "image": "https://placehold.co/400x300?text=Chicken", "category_id": 1, "rating": 4.2, "ingredients": ["chicken", "sauce", "bun"], "preparation_time": 15 },
    { "id": 3, "name": "Margherita", "description": "Tomato, mozzarella, basil", "price": 10, "image": "https://placehold.co/400x300?text=Pizza", "category_id": 2, "rating": 4.7, "ingredients": ["dough", "tomato", "mozzarella", "basil"], "preparation_time": 20 },
    { "id": 4, "name": "Pepperoni", "description": "Lots of pepperoni", "price": 12, "image": "https://placehold.co/400x300?text=Pepperoni", "category_id": 2, "rating": 4.6, "ingredients": ["dough", "tomato", "cheese", "pepperoni"], "preparation_time": 20 },
    { "id": 5, "name": "Greek Salad", "description": "Fresh and light", "price": 6, "image": "https://placehold.co/400x300?text=Salad", "category_id": 3, "rating": 4.3, "ingredients": ["tomato", "cucumber", "feta", "olives"], "preparation_time": 10 }
  ],
  "orders": []
}
```

```bash
npx json-server@0.17.4 --watch db.json --port 5000
```

| Request | Result |
|---------|--------|
| `GET http://localhost:5000/meals` | all meals |
| `GET /meals/3` | one meal |
| `GET /meals?category_id=2` | filter |
| `GET /meals?q=burger` | search |
| `GET /meals?_page=1&_limit=6` | pagination |
| `POST /orders` | saves a new order into `db.json` |

In `.env` set `VITE_API_URL=http://localhost:5000`. json-server returns plain arrays (not `PaginatedResponse`), so the services below use `Meal[]`.

### ✍️ Your Turn: Milestone 2 (API Services)

```ts
// src/services/apiClient.ts
import axios from 'axios';

export const apiClient = axios.create({
  baseURL: import.meta.env.____,
});
```

```ts
// src/services/api/meals.ts
import { apiClient } from '../apiClient';
import type { Meal, Category } from '@/types/meal';

export interface MealsFilters {
  page: number;
  limit: number;
  category_id?: number;
  search?: string;
}

export const mealsService = {
  getMeals: (f: MealsFilters) =>
    apiClient
      .get<Meal[]>('/meals', {
        params: { _page: f.page, _limit: f.limit, category_id: f.category_id, q: ____ },
      })
      .then((r) => r.data),

  getMeal: (id: number) => /* TODO: GET /meals/:id */,

  getCategories: () => /* TODO: GET /categories */,
};
```

```ts
// src/services/api/orders.ts
import { apiClient } from '../apiClient';
import type { Order } from '@/types/order';

export type NewOrder = Omit<Order, 'id'>;

export const ordersService = {
  createOrder: (order: NewOrder) => /* TODO: POST /orders, return the created order */,
  getOrders: () => /* TODO: GET /orders */,
  getOrder: (id: number) => /* TODO: GET /orders/:id */,
};
```

<details>
<summary>✅ Solution</summary>

```ts
export const apiClient = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
});

export const mealsService = {
  getMeals: (f: MealsFilters) =>
    apiClient
      .get<Meal[]>('/meals', {
        params: { _page: f.page, _limit: f.limit, category_id: f.category_id, q: f.search },
      })
      .then((r) => r.data),
  getMeal: (id: number) => apiClient.get<Meal>(`/meals/${id}`).then((r) => r.data),
  getCategories: () => apiClient.get<Category[]>('/categories').then((r) => r.data),
};

export const ordersService = {
  createOrder: (order: NewOrder) => apiClient.post<Order>('/orders', order).then((r) => r.data),
  getOrders: () => apiClient.get<Order[]>('/orders').then((r) => r.data),
  getOrder: (id: number) => apiClient.get<Order>(`/orders/${id}`).then((r) => r.data),
};
```

Axios skips `undefined` params, so an empty search or "all categories" sends no filter.

</details>

---

## 5. Folder Structure

> 💡 **In simple words:** A predictable folder structure means you always know where a file belongs. `@/` is a shortcut (an **alias**) for the `src/` folder, so imports stay short: `@/stores/cartStore` instead of `../../stores/cartStore`.

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

> 💡 **In simple words:** These commands create the project and install every library from Sessions 2–6. `.env` holds settings that change from one computer to another, like the address of the API.

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

> 💡 **In simple words:** These are the reusable "recipes" of the app. Every page follows the same pattern:
> 1. Get the data with React Query.
> 2. Handle loading, error and empty.
> 3. Render the components.
> 4. Change data with a mutation.

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

### ✍️ Your Turn: Milestone 3 (Meal Card + Meals Page)

```tsx
// src/components/meals/MealCard.tsx
import { Link } from 'react-router-dom';
import toast from 'react-hot-toast';
import { useCartStore } from '@/stores/cartStore';
import type { Meal } from '@/types/meal';

export function MealCard({ meal }: { meal: Meal }) {
  const addItem = useCartStore((s) => s.____);

  const handleAdd = () => {
    // TODO: addItem({ meal_id: meal.id, meal, quantity: 1 })
    // TODO: toast.success(`${meal.name} added to cart`)
  };

  return (
    <div className="meal-card">
      <img src={____} alt={____} />
      <h3>{meal.name}</h3>
      <p>${meal.price.toFixed(2)}</p>
      <p>⭐ {meal.rating} · ⏱ {meal.preparation_time} min</p>
      {/* TODO: a Link to /meals/<id> with the text "Details" */}
      <button onClick={handleAdd}>Add to cart</button>
    </div>
  );
}
```

```tsx
// src/pages/Meals.tsx
import { useState } from 'react';
import { useQuery } from '@tanstack/react-query';
import { mealsService, type MealsFilters } from '@/services/api/meals';
import { MealCard } from '@/components/meals/MealCard';

export default function Meals() {
  const [filters, setFilters] = useState<MealsFilters>({ page: 1, limit: 6 });

  const { data: meals, isLoading, error } = useQuery({
    queryKey: ['meals', ____],                 // new filters = new key = new request
    queryFn: () => mealsService.getMeals(____),
  });

  const { data: categories } = useQuery({
    queryKey: ['categories'],
    queryFn: mealsService.getCategories,
  });

  return (
    <div>
      <input
        placeholder="Search meals"
        onChange={(e) => setFilters((f) => ({ ...f, search: e.target.value || undefined, page: 1 }))}
      />
      <select
        onChange={(e) =>
          setFilters((f) => ({ ...f, category_id: e.target.value ? Number(e.target.value) : undefined, page: 1 }))
        }
      >
        <option value="">All categories</option>
        {/* TODO: one <option> per category */}
      </select>

      {/* TODO: isLoading → "Loading meals..." */}
      {/* TODO: error → "Could not load meals" */}
      {/* TODO: empty → "No meals found" */}
      {/* TODO: a grid with one MealCard per meal */}

      {/* TODO: Prev / Next buttons that change filters.page.
               Prev is disabled on page 1. Next is disabled when fewer than `limit` meals came back. */}
    </div>
  );
}
```

```tsx
// src/components/cart/CartSummary.tsx: fill in the numbers
const subtotal = useCartStore((s) => s.getTotal());
const tax = ____;      // 10% of subtotal
const total = ____;
```

<details>
<summary>✅ Solution</summary>

```tsx
export function MealCard({ meal }: { meal: Meal }) {
  const addItem = useCartStore((s) => s.addItem);

  const handleAdd = () => {
    addItem({ meal_id: meal.id, meal, quantity: 1 });
    toast.success(`${meal.name} added to cart`);
  };

  return (
    <div className="meal-card">
      <img src={meal.image} alt={meal.name} />
      <h3>{meal.name}</h3>
      <p>${meal.price.toFixed(2)}</p>
      <p>⭐ {meal.rating} · ⏱ {meal.preparation_time} min</p>
      <Link to={`/meals/${meal.id}`}>Details</Link>
      <button onClick={handleAdd}>Add to cart</button>
    </div>
  );
}
```

```tsx
export default function Meals() {
  const [filters, setFilters] = useState<MealsFilters>({ page: 1, limit: 6 });

  const { data: meals, isLoading, error } = useQuery({
    queryKey: ['meals', filters],
    queryFn: () => mealsService.getMeals(filters),
  });

  const { data: categories } = useQuery({
    queryKey: ['categories'],
    queryFn: mealsService.getCategories,
  });

  return (
    <div>
      <input
        placeholder="Search meals"
        onChange={(e) => setFilters((f) => ({ ...f, search: e.target.value || undefined, page: 1 }))}
      />
      <select
        onChange={(e) =>
          setFilters((f) => ({ ...f, category_id: e.target.value ? Number(e.target.value) : undefined, page: 1 }))
        }
      >
        <option value="">All categories</option>
        {categories?.map((c) => (
          <option key={c.id} value={c.id}>{c.name}</option>
        ))}
      </select>

      {isLoading && <p>Loading meals...</p>}
      {error && <p>Could not load meals</p>}
      {meals && meals.length === 0 && <p>No meals found</p>}

      <div className="grid">
        {meals?.map((meal) => <MealCard key={meal.id} meal={meal} />)}
      </div>

      <button
        disabled={filters.page === 1}
        onClick={() => setFilters((f) => ({ ...f, page: f.page - 1 }))}
      >
        Prev
      </button>
      <span> Page {filters.page} </span>
      <button
        disabled={!meals || meals.length < filters.limit}
        onClick={() => setFilters((f) => ({ ...f, page: f.page + 1 }))}
      >
        Next
      </button>
    </div>
  );
}
```

```tsx
const subtotal = useCartStore((s) => s.getTotal());
const tax = subtotal * 0.1;
const total = subtotal + tax;
```

</details>

### ✍️ Your Turn: Milestone 4 (Checkout Validation)

```tsx
// src/pages/Checkout.tsx: validation only
interface CheckoutForm {
  customer_name: string;
  phone: string;
  address: string;
  payment_method: 'cash' | 'card';
  notes: string;
}

type CheckoutErrors = Partial<Record<keyof CheckoutForm, string>>;

function validate(form: CheckoutForm): CheckoutErrors {
  const errors: CheckoutErrors = {};
  // TODO: customer_name is required and must have at least 3 characters
  // TODO: phone must be exactly 10 digits (hint: /^\d{10}$/)
  // TODO: address is required
  return errors;
}

// In handleSubmit:
//   const errors = validate(form);
//   setErrors(errors);
//   if (Object.keys(errors).length > 0) return;
//   createOrderMutation.mutate({ ...form, items, total, status: 'pending', created_at: new Date().toISOString() });
```

<details>
<summary>✅ Solution</summary>

```tsx
function validate(form: CheckoutForm): CheckoutErrors {
  const errors: CheckoutErrors = {};
  if (form.customer_name.trim().length < 3) {
    errors.customer_name = 'Name must have at least 3 characters';
  }
  if (!/^\d{10}$/.test(form.phone)) {
    errors.phone = 'Phone must be 10 digits';
  }
  if (!form.address.trim()) {
    errors.address = 'Address is required';
  }
  return errors;
}
```

</details>

---

## 8. Performance Pass

> 💡 **In simple words:** Build first, optimize last. Once everything works, add memoization and lazy loading only where they really help.

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

> 💡 **In simple words:** Use these lists before you submit: go through each line and tick it off. They are the same checks a senior developer does in a code review.

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

> 💡 **In simple words:** These features are optional, for students who finish early. Pick one, plan it, and build it on a new git branch.

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

## 📝 Student Worksheet

### A. Planning Questions (answer BEFORE you start coding)

1. Which routes are public and which are protected? Why is `/cart` public but `/checkout` protected?
2. Which tool stores each piece of data: **React Query**, **Zustand**, **useState** or **Context**?
   a) the meals list · b) the cart items · c) the logged-in user · d) the text typed in the checkout form · e) the user's orders
3. Why does the meals query key include the filters (`['meals', filters]`)?
4. What must happen to the cart after an order is placed successfully?
5. Why do we persist the cart in `localStorage` but not the meals list?
6. Which three extra states must every list page show, besides the data itself?
7. Why do we lazy-load Checkout, Orders and Profile, but not Home?
8. Why does the header's cart badge use a selector: `useCartStore((s) => s.getItemCount())`?

<details>
<summary>✅ Solution</summary>

1. Public: `/`, `/login`, `/register`, `/meals`, `/meals/:id`, `/cart`. Protected: `/checkout`, `/orders`, `/orders/:id`, `/profile`. Anyone can fill a cart, but placing an order needs to know who the customer is.
2. a) React Query · b) Zustand (persisted) · c) Zustand (persisted) · d) useState · e) React Query
3. Different filters are different data. A new key makes React Query fetch and cache each combination separately.
4. Clear the cart, show a success toast, and navigate to the new order's details page.
5. The cart exists only in the browser, so we would lose it on refresh. The meals list lives on the server and can always be fetched again (React Query caches it).
6. Loading, error and empty.
7. Most visitors open Home first, so it should load immediately. The other pages are needed later or only by logged-in users, so their code can download when they're opened.
8. With a selector, the header re-renders only when the item count changes, not on every store change.

</details>

### B. Code Review: Find at Least 5 Problems

```tsx
function Meals() {
  const [meals, setMeals] = useState<any>([]);

  useEffect(() => {
    fetch('http://localhost:5000/meals')
      .then(r => r.json())
      .then(data => setMeals(data));
  });

  return (
    <div>
      {meals.map((m, i) => (
        <div key={i}>
          <img src={m.image} />
          <h3>{m.name}</h3>
          <button onClick={useCartStore.getState().addItem({ meal_id: m.id, meal: m, quantity: 1 })}>
            Add
          </button>
        </div>
      ))}
    </div>
  );
}
```

<details>
<summary>✅ Solution</summary>

1. `any`: no type safety. Use `Meal[]`.
2. `useEffect` has no dependency array, so it fetches after every render. `setMeals` causes a render, which fetches again, forever.
3. Raw `fetch` with a hardcoded URL inside the component. Use `mealsService` + React Query (and `VITE_API_URL`).
4. No `res.ok` check, and no loading, error or empty states.
5. `key={i}`: use `key={m.id}`.
6. `<img>` without `alt`: bad for accessibility.
7. `onClick={addItem(...)}` **calls** the function during render (it adds to the cart on every render). Pass a function: `onClick={() => addItem(...)}`, and get `addItem` with a selector.

```tsx
export default function Meals() {
  const addItem = useCartStore((s) => s.addItem);
  const { data: meals, isLoading, error } = useQuery({
    queryKey: ['meals'],
    queryFn: () => mealsService.getMeals({ page: 1, limit: 12 }),
  });

  if (isLoading) return <LoadingSpinner />;
  if (error) return <EmptyState title="Could not load meals" />;
  if (!meals?.length) return <EmptyState title="No meals found" />;

  return (
    <div className="grid">
      {meals.map((m) => (
        <div key={m.id}>
          <img src={m.image} alt={m.name} />
          <h3>{m.name}</h3>
          <button onClick={() => addItem({ meal_id: m.id, meal: m, quantity: 1 })}>Add</button>
        </div>
      ))}
    </div>
  );
}
```

</details>

### C. Milestone Plan (give one milestone per class or per day)

| # | Milestone | Done when… |
|---|-----------|------------|
| 1 | Setup + types + cart store | Project runs; `Meal`, `CartItem`, `Order` types exist; cart store passes a manual test in the console |
| 2 | json-server + services | `db.json` running; `mealsService` / `ordersService` return data |
| 3 | Layout + routes | Header with links and cart badge, all routes render a placeholder page, 404 works |
| 4 | Meals page | Search, category filter, pagination, loading/error/empty states, MealCard adds to cart |
| 5 | Meal details | `/meals/:id` shows image, ingredients and a quantity picker → add to cart |
| 6 | Cart page | +/− quantity, remove, subtotal/tax/total, clear cart, empty state, survives refresh |
| 7 | Fake auth + protected routes | Login stores the user; `/checkout`, `/orders`, `/profile` redirect guests to `/login` |
| 8 | Checkout + orders | Validated form → `POST /orders` → cart cleared → order details page; orders list page |
| 9 | Polish | Lazy routes, `memo` on MealCard, responsive layout, toasts, README |

### D. Grading Rubric (100 points)

| Area | Points | What we check |
|------|--------|---------------|
| Features work | 30 | Browse, filter, cart, checkout, orders, protected routes |
| State management | 15 | React Query for server data, Zustand for cart/auth, no duplicated state |
| Code structure | 15 | Services, components, pages, types in the right folders; no API calls in components |
| UI states | 10 | Loading, error and empty on every list page |
| TypeScript | 10 | No `any`, typed props, typed API responses |
| UX & responsive | 10 | Works on mobile, toasts, disabled buttons while saving |
| Git & README | 10 | Small clear commits, README with setup steps and screenshots |

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
