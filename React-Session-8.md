# الجلسة الثامنة: State Management في React

## 📋 نظرة عامة
- **المدة**: 3 ساعات
- **المستوى**: متقدم
- **التقنيات**: React + TypeScript + Tailwind CSS + Zustand
- **الموضوع**: إدارة الحالة في React (State Management)

---

## 🎯 الأهداف
- فهم مشكلة Prop Drilling وكيفية حلها
- التعرف على أنواع State المختلفة في React
- إتقان استخدام Zustand لإدارة الحالة العالمية
- بناء مشروع عملي (Shopping Cart) كامل

---

## ❓ المشكلة: Prop Drilling

### ما هو Prop Drilling؟
Prop Drilling هو مشكلة شائعة في React تحدث عندما تحتاج لتمرير البيانات عبر عدة مستويات من المكونات للوصول إلى مكون عميق.

### مثال على المشكلة

```tsx
// المكون الرئيسي
function App() {
  const [user, setUser] = useState({ name: 'Ahmed', role: 'admin' });
  
  return (
    <div>
      <Header user={user} />
      <Main user={user} setUser={setUser} />
      <Footer user={user} />
    </div>
  );
}

// Main Component
function Main({ user, setUser }) {
  return (
    <div>
      <Sidebar user={user} />
      <Content user={user} setUser={setUser} />
    </div>
  );
}

// Content Component
function Content({ user, setUser }) {
  return (
    <div>
      <Article user={user} />
      <UserProfile user={user} setUser={setUser} />
    </div>
  );
}

// UserProfile Component - المكون الذي يحتاج البيانات فعلياً
function UserProfile({ user, setUser }) {
  return (
    <div>
      <h1>{user.name}</h1>
      <button onClick={() => setUser({ ...user, name: 'New Name' })}>
        Change Name
      </button>
    </div>
  );
}
```

### المشاكل الناتجة عن Prop Drilling

1. **كود متكرر**: تمرير نفس الـ props عبر مكونات لا تستخدمها
2. **صعوبة الصيانة**: تغيير هيكل المكونات يتطلب تعديل多处
3. **تسرب الاعتمادية**: المكونات الوسيطة تعتمد على props لا تحتاجها
4. **صعوبة الاختبار**: يحتاج لتمرير props كثيرة للاختبار

### الحلول الممكنة
- ✅ Context API
- ✅ Global State Management (Zustand, Redux)
- ✅ Composition Pattern
- ✅ Render Props

---

## 📦 أنواع State في React

### 1. Local State (الحالة المحلية)

**التعريف**: الحالة الخاصة بمكون واحد ولا يتم مشاركتها مع مكونات أخرى.

**متى تستخدم**: 
- بيانات خاصة بالمكون فقط (form inputs, toggles, modals)
- UI temporary state

**الأداة**: `useState`, `useReducer`

```tsx
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

**المميزات**:
- ✅ بسيط وسهل الاستخدام
- ✅ أداء عالي (re-render للمكون فقط)
- ✅ عزل البيانات

**العيوب**:
- ❌ لا يمكن مشاركته بين المكونات
- ❌ صعب الإدارة مع الحالة المعقدة

---

### 2. Lifted State (الحالة المرفوعة)

**التعريف**: رفع الحالة إلى أقرب مكون مشترك بين المكونات التي تحتاجها.

**متى تستخدم**:
- عندما يحتاج أكثر من مكون للوصول لنفس البيانات
- للمكونات القريبة من بعضها في الشجرة

```tsx
// رفع الحالة للمكون الأب
function Parent() {
  const [value, setValue] = useState('');
  
  return (
    <div>
      <ChildA value={value} onChange={setValue} />
      <ChildB value={value} />
    </div>
  );
}

function ChildA({ value, onChange }) {
  return <input value={value} onChange={(e) => onChange(e.target.value)} />;
}

function ChildB({ value }) {
  return <p>Value: {value}</p>;
}
```

**المميزات**:
- ✅ بسيط للمكونات القريبة
- ✅ واضح وسهل الفهم
- ✅ لا يحتاج مكتبات خارجية

**العيوب**:
- ❌ Prop Drilling للمكونات البعيدة
- ❌ صعب الإدارة مع شجرة معقدة

---

### 3. Context API

**التعريف**: طريقة رسمية في React لمشاركة البيانات بين المكونات بدون prop drilling.

**متى تستخدم**:
- للبيانات العالمية (theme, language, user auth)
- تجنب prop drilling للمكونات البعيدة

```tsx
// إنشاء Context
const UserContext = createContext<User | null>(null);

// Provider Component
function UserProvider({ children }) {
  const [user, setUser] = useState<User | null>(null);
  
  return (
    <UserContext.Provider value={{ user, setUser }}>
      {children}
    </UserContext.Provider>
  );
}

// استخدام Context
function UserProfile() {
  const { user, setUser } = useContext(UserContext);
  
  return (
    <div>
      <h1>{user?.name}</h1>
      <button onClick={() => setUser({ ...user, name: 'New' })}>
        Update
      </button>
    </div>
  );
}
```

**المميزات**:
- ✅ حل مشكلة Prop Drilling
- ✅ جزء من React الرسمي
- ✅ سهل الاستخدام للحالات البسيطة

**العيوب**:
- ❌ Re-render لكل المكونات المستهلكة
- ❌ صعب الإدارة للحالات المعقدة
- ❌ لا يوجد دعم مدمج للـ async actions

---

### 4. Global State (الحالة العالمية)

**التعريف**: حالة مخزنة خارج شجرة React ويمكن الوصول إليها من أي مكون.

**متى تستخدم**:
- للتطبيقات الكبيرة والمعقدة
- للبيانات التي يحتاجها كامل التطبيق
- للحالات التي تحتاج actions معقدة

**الأدوات**: Zustand, Redux, Jotai, Recoil

```tsx
// مثال باستخدام Zustand
import { create } from 'zustand';

interface UserStore {
  user: User | null;
  setUser: (user: User) => void;
  clearUser: () => void;
}

const useUserStore = create<UserStore>((set) => ({
  user: null,
  setUser: (user) => set({ user }),
  clearUser: () => set({ user: null }),
}));

// الاستخدام في أي مكون
function ComponentA() {
  const user = useUserStore((state) => state.user);
  return <h1>{user?.name}</h1>;
}

function ComponentB() {
  const setUser = useUserStore((state) => state.setUser);
  return <button onClick={() => setUser({ name: 'Ahmed' })}>Set User</button>;
}
```

**المميزات**:
- ✅ الوصول من أي مكون بدون prop drilling
- ✅ أداء عالي (re-render للمكونات المتأثرة فقط)
- ✅ دعم للحالات المعقدة
- ✅ سهل الاختبار والصيانة

**العيوب**:
- ❌ يحتاج مكتبة خارجية
- ❌ قد يكون overkill للتطبيقات البسيطة

---

### 5. Client State (حالة العميل)

**التعريف**: البيانات المولدة والمخزنة على جانب العميل.

**أمثلة**:
- Form inputs
- UI state (modals, dropdowns, tabs)
- Local settings (theme, language)
- Temporary data

**مكان التخزين**:
- Component state (useState)
- Global state (Zustand)
- LocalStorage/SessionStorage
- Cookies

```tsx
// Client State مثال
function Form() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });
  
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [showSuccess, setShowSuccess] = useState(false);
  
  const handleSubmit = async (e: FormEvent) => {
    e.preventDefault();
    setIsSubmitting(true);
    
    await submitForm(formData);
    
    setIsSubmitting(false);
    setShowSuccess(true);
  };
  
  // ...
}
```

---

### 6. Server State (حالة الخادم)

**التعريف**: البيانات القادمة من الخادم تحتاج sync و caching.

**أمثلة**:
- User data من API
- Products list
- Posts and comments
- أي بيانات من backend

**الأدوات**: React Query, SWR, Apollo Client

```tsx
// مثال باستخدام React Query
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';

function ProductsList() {
  const { data: products, isLoading, error } = useQuery({
    queryKey: ['products'],
    queryFn: fetchProducts
  });
  
  const queryClient = useQueryClient();
  
  const addProduct = useMutation({
    mutationFn: createProduct,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['products'] });
    }
  });
  
  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error loading products</div>;
  
  return (
    <div>
      {products?.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}
```

**المميزات**:
- ✅ Caching تلقائي
- ✅ Auto-refetching
- ✅ Optimistic updates
- ✅ Loading و error states مدمجة
- ✅ Deduplication للطلبات

**العيوب**:
- ❌ تعقيد إضافي
- ❌ يحتاج setup مبدئي

---

## ⚖️ مقارنة أدوات State Management

### useState vs Context vs Zustand vs React Query

| الميزة | useState | Context API | Zustand | React Query |
|--------|----------|-------------|---------|------------|
| **الاستخدام** | Local state فقط | Global state بسيط | Global state متقدم | Server state |
| **التعقيد** | منخفض جداً | منخفض | متوسط | متوسط-عالي |
| **Boilerplate** | لا يوجد | قليل | قليل جداً | متوسط |
| **Performance** | ممتاز | جيد (re-render للكل) | ممتاز (selective) | ممتاز |
| **TypeScript** | ممتاز | جيد | ممتاز | ممتاز |
| **DevTools** | React DevTools | React DevTools | Zustand DevTools | React Query DevTools |
| **Learning Curve** | سهل جداً | سهل | سهل | متوسط |
| **Bundle Size** | جزء من React | جزء من React | ~1KB | ~13KB |
| **Async Actions** | يدعم (manual) | يدعم (manual) | يدعم ( manual) | مدمج |
| **Caching** | لا يوجد | لا يوجد | لا يوجد | مدمج |
| **Auto-refetch** | لا يوجد | لا يوجد | لا يوجد | مدمج |
| **Persistence** | يدعم (manual) | يدعم (manual) | مدمج (middleware) | يدعم (manual) |

### متى تستخدم كل أداة؟

#### 🎯 useState
```tsx
// ✅ استخدم عندما:
// - الحالة خاصة بمكون واحد
// - بيانات بسيطة (boolean, string, number)
// - UI temporary state

function ToggleButton() {
  const [isOn, setIsOn] = useState(false);
  return <button onClick={() => setIsOn(!isOn)}>{isOn ? 'ON' : 'OFF'}</button>;
}
```

#### 🎯 Context API
```tsx
// ✅ استخدم عندما:
// - بيانات عالمية بسيطة (theme, language)
// - تريد تجنب prop drilling
// - الحالة ليست معقدة
// - المكونات المستهلكة قليلة

const ThemeContext = createContext('light');

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Header />
      <Main />
    </ThemeContext.Provider>
  );
}
```

#### 🎯 Zustand
```tsx
// ✅ استخدم عندما:
// - global state معقدة
// - تحتاج selective re-renders
// - تريد API بسيط
// - تحتاج persistence
// - الكثير من المكونات تستخدم الحالة

const useStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
}));
```

#### 🎯 React Query
```tsx
// ✅ استخدم عندما:
// - تعامل مع server data
// - تحتاج caching
// - تحتاج auto-refetching
// - optimistic updates
// - loading/error states

const { data, isLoading } = useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers,
});
```

### مثال مقارنة عملي

#### المشكلة: Shopping Cart

#### ❌ الحل بـ useState (Prop Drilling)
```tsx
function App() {
  const [cart, setCart] = useState([]);
  
  return (
    <Layout cart={cart} setCart={setCart}>
      <Products cart={cart} setCart={setCart} />
      <Cart cart={cart} setCart={setCart} />
    </Layout>
  );
}

// كل مكون يحتاج تمرير cart و setCart
```

#### ⚠️ الحل بـ Context API
```tsx
const CartContext = createContext();

function App() {
  const [cart, setCart] = useState([]);
  
  return (
    <CartContext.Provider value={{ cart, setCart }}>
      <Layout>
        <Products />
        <Cart />
      </Layout>
    </CartContext.Provider>
  );
}

// كل مكون يستخدم Context سيعيد re-render
```

#### ✅ الحل بـ Zustand
```tsx
const useCartStore = create((set) => ({
  cart: [],
  addItem: (item) => set((state) => ({ cart: [...state.cart, item] })),
  removeItem: (id) => set((state) => ({ cart: state.cart.filter(i => i.id !== id) })),
}));

// استخدام مباشر في أي مكون
function Products() {
  const addItem = useCartStore((state) => state.addItem);
  // ...
}

function Cart() {
  const cart = useCartStore((state) => state.cart);
  // ...
}
```

#### ✅ الحل بـ React Query (للـ products من server)
```tsx
// للبيانات من الخادم
const { data: products } = useQuery({
  queryKey: ['products'],
  queryFn: fetchProducts,
});

// للـ cart (client state) استخدم Zustand
const cart = useCartStore((state) => state.cart);
```

---

## 🚀 Zustand: Global State Management

### 1. What is Zustand?

**التعريف**: مكتبة خفيفة وسريعة لإدارة الحالة العالمية في React.

**المميزات**:
- 🎯 صغير جداً (~1KB gzipped)
- ⚡ سريع和高性能
- 🔧 API بسيط وسهل
- 📝 دعم ممتاز لـ TypeScript
- 🔌 Middlewares مدمجة
- 🛠️ DevTools متاحة
- 📱 لا يوجد Provider needed

**مقارنة الحجم**:
- Redux: ~15KB
- MobX: ~16KB
- Recoil: ~22KB
- **Zustand: ~1KB** ⭐

---

### 2. Why Zustand?

#### مقارنة مع Redux

```tsx
// ❌ Redux - كثير من boilerplate
// actions.js
export const increment = () => ({ type: 'INCREMENT' });
export const decrement = () => ({ type: 'DECREMENT' });

// reducer.js
const counterReducer = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT': return state + 1;
    case 'DECREMENT': return state - 1;
    default: return state;
  }
};

// store.js
const store = createStore(counterReducer);

// Component
import { useSelector, useDispatch } from 'react-redux';
function Counter() {
  const count = useSelector(state => state);
  const dispatch = useDispatch();
  return (
    <div>
      <p>{count}</p>
      <button onClick={() => dispatch(increment())}>+</button>
    </div>
  );
}
```

```tsx
// ✅ Zustand - بسيط وواضح
import { create } from 'zustand';

const useCounterStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
}));

// Component
function Counter() {
  const { count, increment } = useCounterStore();
  return (
    <div>
      <p>{count}</p>
      <button onClick={increment}>+</button>
    </div>
  );
}
```

#### لماذا Zustand أفضل؟

1. **أقل كود**: 80% أقل من Redux
2. **أسرع**: لا Context overhead
3. **أسهل**: API بسيط جداً
4. **أكثر مرونة**: middlewaves قوية
5. **TypeScript أصلية**: دعم ممتاز من البداية

---

### 3. Creating Store

#### البداية الأساسية

```tsx
import { create } from 'zustand';

// تعريف الـ interface
interface CounterStore {
  count: number;
  increment: () => void;
  decrement: () => void;
  reset: () => void;
}

// إنشاء الـ store
const useCounterStore = create<CounterStore>((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}));
```

#### في المكونات

```tsx
function Counter() {
  // استخدام كامل الـ store
  const { count, increment, decrement, reset } = useCounterStore();
  
  return (
    <div className="p-4">
      <h1>Count: {count}</h1>
      <button onClick={increment} className="px-4 py-2 bg-green-500 text-white">
        +
      </button>
      <button onClick={decrement} className="px-4 py-2 bg-red-500 text-white">
        -
      </button>
      <button onClick={reset} className="px-4 py-2 bg-gray-500 text-white">
        Reset
      </button>
    </div>
  );
}
```

---

### 4. State

#### تعريف الحالة

```tsx
interface UserStore {
  // Primitive types
  name: string;
  age: number;
  isActive: boolean;
  
  // Complex types
  user: User | null;
  tags: string[];
  metadata: Record<string, any>;
  
  // Nested objects
  profile: {
    firstName: string;
    lastName: string;
    address: {
      street: string;
      city: string;
    };
  };
}

const useUserStore = create<UserStore>((set) => ({
  name: '',
  age: 0,
  isActive: false,
  user: null,
  tags: [],
  metadata: {},
  profile: {
    firstName: '',
    lastName: '',
    address: {
      street: '',
      city: '',
    },
  },
}));
```

#### قراءة الحالة

```tsx
// ❌ سيء - يقرأ كل الـ store ويعيد re-render لكل تغيير
function Component() {
  const store = useUserStore();
  return <div>{store.name}</div>;
}

// ✅ جيد - يقرأ فقط ما يحتاج
function Component() {
  const name = useUserStore((state) => state.name);
  return <div>{name}</div>;
}

// ✅ أفضل - مع selector function
function Component() {
  const name = useUserStore((state) => state.name);
  const age = useUserStore((state) => state.age);
  return <div>{name} - {age}</div>;
}
```

---

### 5. Actions

#### أنواع الـ Actions

```tsx
interface Store {
  // Simple action
  setName: (name: string) => void;
  
  // Action with parameters
  updateUser: (user: User) => void;
  
  // Async action
  fetchUser: (id: number) => Promise<void>;
  
  // Action مع logic معقد
  complexAction: (data: Data) => void;
}

const useStore = create<Store>((set, get) => ({
  // Simple action
  setName: (name) => set({ name }),
  
  // Action مع parameters
  updateUser: (user) => set({ user }),
  
  // Async action
  fetchUser: async (id) => {
    set({ loading: true });
    try {
      const user = await api.fetchUser(id);
      set({ user, loading: false });
    } catch (error) {
      set({ error, loading: false });
    }
  },
  
  // Complex action باستخدام get()
  complexAction: (data) => {
    const currentState = get();
    set({
      ...currentState,
      ...data,
      updatedAt: new Date(),
    });
  },
}));
```

#### استخدام get() للوصول للحالة الحالية

```tsx
const useCounterStore = create((set, get) => ({
  count: 0,
  
  // يمكن الوصول للحالة الحالية باستخدام get()
  incrementBy: (amount: number) => {
    const currentCount = get().count;
    set({ count: currentCount + amount });
  },
  
  // أو داخل set callback
  incrementByAlt: (amount: number) => set((state) => ({
    count: state.count + amount
  })),
}));
```

---

### 6. Selectors

#### ما هي Selectors؟

Selectors هي functions تستخرج جزء محدد من الحالة.

```tsx
interface ProductStore {
  products: Product[];
  filter: string;
  
  // Direct state
  allProducts: () => Product[];
  
  // Filtered state
  filteredProducts: () => Product[];
  
  // Computed values
  totalProducts: () => number;
  expensiveProducts: () => Product[];
}

const useProductStore = create<ProductStore>((set, get) => ({
  products: [],
  filter: '',
  
  allProducts: () => get().products,
  
  filteredProducts: () => {
    const { products, filter } = get();
    return products.filter(p => 
      p.name.toLowerCase().includes(filter.toLowerCase())
    );
  },
  
  totalProducts: () => get().products.length,
  
  expensiveProducts: () => 
    get().products.filter(p => p.price > 100),
}));
```

#### استخدام Selectors في المكونات

```tsx
function ProductList() {
  // استخدام selector مباشرة
  const filteredProducts = useProductStore(
    (state) => state.filteredProducts()
  );
  
  const totalProducts = useProductStore(
    (state) => state.totalProducts()
  );
  
  return (
    <div>
      <p>Total: {totalProducts()}</p>
      {filteredProducts().map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}
```

#### Selector خارجي (reusable)

```tsx
// تعريف selector خارجي
const selectExpensiveProducts = (state: ProductStore) => 
  state.products.filter(p => p.price > 100);

const selectProductById = (id: number) => (state: ProductStore) =>
  state.products.find(p => p.id === id);

// الاستخدام
function ExpensiveProducts() {
  const expensiveProducts = useProductStore(selectExpensiveProducts);
  // ...
}

function ProductDetail({ id }: { id: number }) {
  const product = useProductStore(selectProductById(id));
  // ...
}
```

---

### 7. Multiple Stores

#### إنشاء متعدد stores

```tsx
// Store 1: User
interface UserStore {
  user: User | null;
  setUser: (user: User) => void;
  clearUser: () => void;
}

const useUserStore = create<UserStore>((set) => ({
  user: null,
  setUser: (user) => set({ user }),
  clearUser: () => set({ user: null }),
}));

// Store 2: Cart
interface CartStore {
  items: CartItem[];
  addItem: (item: CartItem) => void;
  removeItem: (id: number) => void;
}

const useCartStore = create<CartStore>((set) => ({
  items: [],
  addItem: (item) => set((state) => ({ 
    items: [...state.items, item] 
  })),
  removeItem: (id) => set((state) => ({ 
    items: state.items.filter(i => i.id !== id) 
  })),
}));

// Store 3: Theme
interface ThemeStore {
  theme: 'light' | 'dark';
  toggleTheme: () => void;
}

const useThemeStore = create<ThemeStore>((set) => ({
  theme: 'light',
  toggleTheme: () => set((state) => ({ 
    theme: state.theme === 'light' ? 'dark' : 'light' 
  })),
}));
```

#### استخدام متعدد stores في مكون واحد

```tsx
function Header() {
  const user = useUserStore((state) => state.user);
  const cartItems = useCartStore((state) => state.items);
  const theme = useThemeStore((state) => state.theme);
  const toggleTheme = useThemeStore((state) => state.toggleTheme);
  
  return (
    <header className={`header ${theme}`}>
      <span>{user?.name || 'Guest'}</span>
      <span>Cart: {cartItems.length}</span>
      <button onClick={toggleTheme}>
        {theme === 'light' ? '🌙' : '☀️'}
      </button>
    </header>
  );
}
```

#### مزج stores (combining stores)

```tsx
// يمكنك استخدام store من داخل store آخر
const useAppStore = create((set, get) => ({
  // ... state
  
  // Action يستخدم store آخر
  checkout: async () => {
    const cartItems = useCartStore.getState().items;
    const user = useUserStore.getState().user;
    
    if (!user) {
      throw new Error('User not authenticated');
    }
    
    await api.createOrder({ userId: user.id, items: cartItems });
    
    // clear cart
    useCartStore.getState().clearCart();
  },
}));
```

---

### 8. Updating State

#### طرق تحديث الحالة

```tsx
interface Store {
  count: number;
  user: User | null;
  items: string[];
  
  // Method 1: Direct replacement
  setCount: (count: number) => void;
  
  // Method 2: Functional update
  increment: () => void;
  
  // Method 3: Partial update
  updateUser: (updates: Partial<User>) => void;
  
  // Method 4: Array operations
  addItem: (item: string) => void;
  removeItem: (item: string) => void;
}

const useStore = create<Store>((set) => ({
  count: 0,
  user: null,
  items: [],
  
  // Method 1: Direct replacement
  setCount: (count) => set({ count }),
  
  // Method 2: Functional update (آمن للمتابعة)
  increment: () => set((state) => ({ 
    count: state.count + 1 
  })),
  
  // Method 3: Partial update
  updateUser: (updates) => set((state) => ({
    user: state.user ? { ...state.user, ...updates } : null
  })),
  
  // Method 4: Array operations
  addItem: (item) => set((state) => ({
    items: [...state.items, item]
  })),
  
  removeItem: (item) => set((state) => ({
    items: state.items.filter(i => i !== item)
  })),
}));
```

#### أمثلة عملية

```tsx
// تحديث object
const useProfileStore = create((set) => ({
  profile: {
    name: '',
    email: '',
    age: 0,
  },
  
  updateName: (name: string) => set((state) => ({
    profile: { ...state.profile, name }
  })),
  
  updateProfile: (updates: Partial<Profile>) => set((state) => ({
    profile: { ...state.profile, ...updates }
  })),
}));

// تحديث nested object
const useSettingsStore = create((set) => ({
  settings: {
    theme: {
      mode: 'light',
      primaryColor: '#000',
    },
    notifications: {
      email: true,
      push: false,
    },
  },
  
  updateTheme: (theme: Partial<Theme>) => set((state) => ({
    settings: {
      ...state.settings,
      theme: { ...state.settings.theme, ...theme }
    }
  })),
}));
```

---

### 9. Arrays

#### عمليات Arrays الشائعة

```tsx
interface TodoStore {
  todos: Todo[];
  
  // Add
  addTodo: (todo: Todo) => void;
  
  // Remove
  removeTodo: (id: number) => void;
  
  // Update
  updateTodo: (id: number, updates: Partial<Todo>) => void;
  
  // Toggle
  toggleTodo: (id: number) => void;
  
  // Clear all
  clearTodos: () => void;
  
  // Reorder
  reorderTodos: (fromIndex: number, toIndex: number) => void;
}

const useTodoStore = create<TodoStore>((set) => ({
  todos: [],
  
  // Add to end
  addTodo: (todo) => set((state) => ({
    todos: [...state.todos, todo]
  })),
  
  // Add to beginning
  addTodoFirst: (todo) => set((state) => ({
    todos: [todo, ...state.todos]
  })),
  
  // Remove by id
  removeTodo: (id) => set((state) => ({
    todos: state.todos.filter(todo => todo.id !== id)
  })),
  
  // Update by id
  updateTodo: (id, updates) => set((state) => ({
    todos: state.todos.map(todo =>
      todo.id === id ? { ...todo, ...updates } : todo
    )
  })),
  
  // Toggle boolean
  toggleTodo: (id) => set((state) => ({
    todos: state.todos.map(todo =>
      todo.id === id 
        ? { ...todo, completed: !todo.completed }
        : todo
    )
  })),
  
  // Clear all
  clearTodos: () => set({ todos: [] }),
  
  // Reorder (drag and drop)
  reorderTodos: (fromIndex, toIndex) => set((state) => {
    const newTodos = [...state.todos];
    const [removed] = newTodos.splice(fromIndex, 1);
    newTodos.splice(toIndex, 0, removed);
    return { todos: newTodos };
  }),
}));
```

#### أمثلة استخدام

```tsx
function TodoList() {
  const todos = useTodoStore((state) => state.todos);
  const addTodo = useTodoStore((state) => state.addTodo);
  const toggleTodo = useTodoStore((state) => state.toggleTodo);
  const removeTodo = useTodoStore((state) => state.removeTodo);
  
  const handleAdd = () => {
    addTodo({
      id: Date.now(),
      text: 'New Todo',
      completed: false,
    });
  };
  
  return (
    <div>
      <button onClick={handleAdd}>Add Todo</button>
      {todos.map(todo => (
        <div key={todo.id}>
          <input
            type="checkbox"
            checked={todo.completed}
            onChange={() => toggleTodo(todo.id)}
          />
          <span className={todo.completed ? 'line-through' : ''}>
            {todo.text}
          </span>
          <button onClick={() => removeTodo(todo.id)}>Delete</button>
        </div>
      ))}
    </div>
  );
}
```

---

### 10. Objects

#### عمليات Objects الشائعة

```tsx
interface UserStore {
  users: Record<number, User>;
  
  // Add user
  addUser: (user: User) => void;
  
  // Update user
  updateUser: (id: number, updates: Partial<User>) => void;
  
  // Remove user
  removeUser: (id: number) => void;
  
  // Get user by id
  getUser: (id: number) => User | undefined;
}

const useUserStore = create<UserStore>((set, get) => ({
  users: {},
  
  // Add user to object
  addUser: (user) => set((state) => ({
    users: { ...state.users, [user.id]: user }
  })),
  
  // Update user in object
  updateUser: (id, updates) => set((state) => ({
    users: {
      ...state.users,
      [id]: { ...state.users[id], ...updates }
    }
  })),
  
  // Remove user from object
  removeUser: (id) => set((state) => {
    const { [id]: removed, ...rest } = state.users;
    return { users: rest };
  }),
  
  // Get user (selector)
  getUser: (id) => get().users[id],
}));
```

#### Nested Objects

```tsx
interface NestedStore {
  data: {
    user: {
      profile: {
        name: string;
        email: string;
      };
      settings: {
        theme: string;
        language: string;
      };
    };
  };
  
  updateProfile: (updates: Partial<Profile>) => void;
  updateSettings: (updates: Partial<Settings>) => void;
}

const useNestedStore = create<NestedStore>((set) => ({
  data: {
    user: {
      profile: { name: '', email: '' },
      settings: { theme: 'light', language: 'en' },
    },
  },
  
  updateProfile: (updates) => set((state) => ({
    data: {
      ...state.data,
      user: {
        ...state.data.user,
        profile: {
          ...state.data.user.profile,
          ...updates
        }
      }
    }
  })),
  
  updateSettings: (updates) => set((state) => ({
    data: {
      ...state.data,
      user: {
        ...state.data.user,
        settings: {
          ...state.data.user.settings,
          ...updates
        }
      }
    }
  })),
}));
```

---

### 11. Derived State

#### ما هو Derived State؟

Derived state هو حالة محسوبة من حالة أخرى، ولا تحتاج تخزين منفصل.

```tsx
interface CartStore {
  items: CartItem[];
  
  // Actions
  addItem: (item: CartItem) => void;
  removeItem: (id: number) => void;
  
  // Derived state (computed values)
  subtotal: () => number;
  tax: () => number;
  total: () => number;
  itemCount: () => number;
  isEmpty: () => boolean;
}

const useCartStore = create<CartStore>((set, get) => ({
  items: [],
  
  addItem: (item) => set((state) => ({
    items: [...state.items, item]
  })),
  
  removeItem: (id) => set((state) => ({
    items: state.items.filter(i => i.id !== id)
  })),
  
  // Derived state functions
  subtotal: () => {
    return get().items.reduce((sum, item) => sum + item.price * item.quantity, 0);
  },
  
  tax: () => {
    return get().subtotal() * 0.1; // 10% tax
  },
  
  total: () => {
    return get().subtotal() + get().tax();
  },
  
  itemCount: () => {
    return get().items.reduce((sum, item) => sum + item.quantity, 0);
  },
  
  isEmpty: () => {
    return get().items.length === 0;
  },
}));
```

#### استخدام Derived State

```tsx
function CartSummary() {
  const subtotal = useCartStore((state) => state.subtotal);
  const tax = useCartStore((state) => state.tax);
  const total = useCartStore((state) => state.total);
  const itemCount = useCartStore((state) => state.itemCount);
  const isEmpty = useCartStore((state) => state.isEmpty);
  
  if (isEmpty()) {
    return <p>Your cart is empty</p>;
  }
  
  return (
    <div className="p-4 bg-gray-100 rounded">
      <p>Items: {itemCount()}</p>
      <p>Subtotal: ${subtotal().toFixed(2)}</p>
      <p>Tax (10%): ${tax().toFixed(2)}</p>
      <p className="font-bold">Total: ${total().toFixed(2)}</p>
    </div>
  );
}
```

#### Derived State مع useMemo (للأداء)

```tsx
import { useMemo } from 'react';

function ExpensiveComponent() {
  const items = useCartStore((state) => state.items);
  
  // Derived state مع caching
  const sortedItems = useMemo(() => {
    return [...items].sort((a, b) => a.price - b.price);
  }, [items]);
  
  const categories = useMemo(() => {
    return [...new Set(items.map(item => item.category))];
  }, [items]);
  
  return (
    <div>
      <h3>Categories: {categories.join(', ')}</h3>
      {sortedItems.map(item => (
        <ProductCard key={item.id} product={item} />
      ))}
    </div>
  );
}
```

---

### 12. Persist Middleware

#### ما هو Persist Middleware؟

يحفظ الحالة تلقائياً في localStorage أو sessionStorage.

```tsx
import { create } from 'zustand';
import { persist, createJSONStorage } from 'zustand/middleware';

interface UserStore {
  user: User | null;
  setUser: (user: User) => void;
  clearUser: () => void;
}

const useUserStore = create<UserStore>()(
  persist(
    (set) => ({
      user: null,
      setUser: (user) => set({ user }),
      clearUser: () => set({ user: null }),
    }),
    {
      name: 'user-storage', // اسم المفتاح في localStorage
      storage: createJSONStorage(() => localStorage), // أو sessionStorage
    }
  )
);
```

#### خيارات Persist المتقدمة

```tsx
const useStore = create()(
  persist(
    (set) => ({
      // ... store
    }),
    {
      name: 'app-storage',
      
      // تخزين جزء من الحالة فقط
      partialize: (state) => ({
        user: state.user,
        theme: state.theme,
        // يتم تخزين فقط user و theme
      }),
      
      // version للتعامل مع التغييرات
      version: 1,
      
      // migration للتعامل مع التغييرات في هيكل البيانات
      migrate: (persistedState: any, version: number) => {
        if (version === 0) {
          // migration من version 0 إلى 1
          return {
            ...persistedState,
            newUserField: 'default value',
          };
        }
        return persistedState;
      },
      
      // custom storage
      storage: {
        getItem: (name) => {
          const str = localStorage.getItem(name);
          return str ? JSON.parse(str) : null;
        },
        setItem: (name, value) => {
          localStorage.setItem(name, JSON.stringify(value));
        },
        removeItem: (name) => {
          localStorage.removeItem(name);
        },
      },
      
      // onRehydrateStorage callback
      onRehydrateStorage: () => (state) => {
        console.log('Hydration complete', state);
      },
    }
  )
);
```

---

### 13. LocalStorage

#### استخدام LocalStorage مباشرة

```tsx
// بدون persist middleware
const useLocalStorageStore = create((set) => ({
  count: 0,
  increment: () => set((state) => {
    const newCount = state.count + 1;
    localStorage.setItem('count', newCount.toString());
    return { count: newCount };
  }),
  decrement: () => set((state) => {
    const newCount = state.count - 1;
    localStorage.setItem('count', newCount.toString());
    return { count: newCount };
  }),
}));

// تحميل من localStorage عند البداية
const initialCount = parseInt(localStorage.getItem('count') || '0');
```

#### استخدام Persist Middleware (الأفضل)

```tsx
import { persist } from 'zustand/middleware';

const useCartStore = create(
  persist(
    (set) => ({
      items: [],
      addItem: (item) => set((state) => ({
        items: [...state.items, item]
      })),
      clearCart: () => set({ items: [] }),
    }),
    {
      name: 'cart-storage',
      storage: createJSONStorage(() => localStorage),
    }
  )
);
```

#### SessionStorage بدلاً من LocalStorage

```tsx
const useSessionStore = create(
  persist(
    (set) => ({
      // ... store
    }),
    {
      name: 'session-storage',
      storage: createJSONStorage(() => sessionStorage),
    }
  )
);
```

#### Cookie Storage

```tsx
import Cookies from 'js-cookie';

const cookieStorage = {
  getItem: (name: string) => {
    const value = Cookies.get(name);
    return value ? JSON.parse(value) : null;
  },
  setItem: (name: string, value: any) => {
    Cookies.set(name, JSON.stringify(value), { expires: 7 });
  },
  removeItem: (name: string) => {
    Cookies.remove(name);
  },
};

const useCookieStore = create(
  persist(
    (set) => ({
      // ... store
    }),
    {
      name: 'cookie-storage',
      storage: cookieStorage,
    }
  )
);
```

---

### 14. Best Practices

#### 1. استخدم Selectors للأداء

```tsx
// ❌ سيء - يسبب re-render غير ضروري
function Component() {
  const store = useStore();
  return <div>{store.name}</div>;
}

// ✅ جيد - يقرأ فقط ما يحتاج
function Component() {
  const name = useStore((state) => state.name);
  return <div>{name}</div>;
}
```

#### 2. افصل Stores حسب المسؤولية

```tsx
// ❌ سيء - store كبير جداً
const useAppStore = create((set) => ({
  // User
  user: null,
  setUser: (user) => set({ user }),
  
  // Cart
  cart: [],
  addItem: (item) => set((state) => ({ cart: [...state.cart, item] })),
  
  // Theme
  theme: 'light',
  toggleTheme: () => set((state) => ({ 
    theme: state.theme === 'light' ? 'dark' : 'light' 
  })),
  
  // ... المزيد
}));

// ✅ جيد - stores منفصلة
const useUserStore = create((set) => ({
  user: null,
  setUser: (user) => set({ user }),
}));

const useCartStore = create((set) => ({
  cart: [],
  addItem: (item) => set((state) => ({ cart: [...state.cart, item] })),
}));

const useThemeStore = create((set) => ({
  theme: 'light',
  toggleTheme: () => set((state) => ({ 
    theme: state.theme === 'light' ? 'dark' : 'light' 
  })),
}));
```

#### 3. استخدم TypeScript Interfaces

```tsx
// ✅ دائماً عرّف interfaces
interface User {
  id: number;
  name: string;
  email: string;
}

interface UserStore {
  user: User | null;
  setUser: (user: User) => void;
  clearUser: () => void;
}

const useUserStore = create<UserStore>((set) => ({
  user: null,
  setUser: (user) => set({ user }),
  clearUser: () => set({ user: null }),
}));
```

#### 4. استخدم Persist للبيانات المهمة

```tsx
// ✅ احفظ البيانات المهمة
const useUserStore = create(
  persist(
    (set) => ({
      user: null,
      setUser: (user) => set({ user }),
    }),
    { name: 'user-storage' }
  )
);

// ❌ لا تحفظ البيانات المؤقتة
const useModalStore = create((set) => ({
  isOpen: false,
  open: () => set({ isOpen: true }),
  close: () => set({ isOpen: false }),
}));
```

#### 5. تجنب Logic المعقد في Store

```tsx
// ❌ سيء - logic معقد في store
const useStore = create((set) => ({
  data: [],
  processData: async () => {
    const rawData = await fetch('/api/data');
    const processed = rawData
      .filter(item => item.active)
      .map(item => ({
        ...item,
        value: item.value * 2,
        formatted: new Date(item.date).toLocaleDateString(),
      }))
      .sort((a, b) => a.value - b.value);
    set({ data: processed });
  },
}));

// ✅ جيد - منطق منفصل
const processData = (rawData: any[]) => {
  return rawData
    .filter(item => item.active)
    .map(item => ({
      ...item,
      value: item.value * 2,
      formatted: new Date(item.date).toLocaleDateString(),
    }))
    .sort((a, b) => a.value - b.value);
};

const useStore = create((set) => ({
  data: [],
  fetchData: async () => {
    const rawData = await fetch('/api/data');
    const processed = processData(rawData);
    set({ data: processed });
  },
}));
```

#### 6. استخدم DevTools للتطوير

```tsx
import { devtools } from 'zustand/middleware';

const useStore = create(
  devtools(
    (set) => ({
      // ... store
    }),
    { name: 'AppStore' } // اسم للـ DevTools
  )
);
```

---

### 15. Common Mistakes

#### 1. عدم استخدام Selectors

```tsx
// ❌ سيء
function Component() {
  const store = useStore();
  return <div>{store.count}</div>;
}

// ✅ صحيح
function Component() {
  const count = useStore((state) => state.count);
  return <div>{count}</div>;
}
```

#### 2. Mutation المباشر للحالة

```tsx
// ❌ سيء - mutation مباشر
const useStore = create((set) => ({
  items: [],
  addItem: (item) => {
    const state = useStore.getState();
    state.items.push(item); // ❌ mutation مباشر
  },
}));

// ✅ صحيح - immutable update
const useStore = create((set) => ({
  items: [],
  addItem: (item) => set((state) => ({
    items: [...state.items, item]
  })),
}));
```

#### 3. نسيان Persist للبيانات المهمة

```tsx
// ❌ سيء - البيانات تضيع عند refresh
const useCartStore = create((set) => ({
  items: [],
  addItem: (item) => set((state) => ({ items: [...state.items, item] })),
}));

// ✅ صحيح - البيانات محفوظة
const useCartStore = create(
  persist(
    (set) => ({
      items: [],
      addItem: (item) => set((state) => ({ items: [...state.items, item] })),
    }),
    { name: 'cart-storage' }
  )
);
```

#### 4. Store واحد كبير

```tsx
// ❌ سيء - store واحد يحتوي كل شيء
const useAppStore = create((set) => ({
  // 50+ fields and actions
}));

// ✅ صحيح - stores منفصلة
const useUserStore = create(/* ... */);
const useCartStore = create(/* ... */);
const useThemeStore = create(/* ... */);
```

#### 5. عدم معالجة Async Errors

```tsx
// ❌ سيء - لا error handling
const useStore = create((set) => ({
  user: null,
  fetchUser: async (id) => {
    const user = await api.fetchUser(id);
    set({ user });
  },
}));

// ✅ صحيح - error handling
const useStore = create((set) => ({
  user: null,
  error: null,
  loading: false,
  fetchUser: async (id) => {
    set({ loading: true, error: null });
    try {
      const user = await api.fetchUser(id);
      set({ user, loading: false });
    } catch (error) {
      set({ error, loading: false });
    }
  },
}));
```

#### 6. استخدام get() بشكل خاطئ

```tsx
// ❌ سيء - get() في render
function Component() {
  const count = useStore((state) => {
    return get().count; // ❌ لا تستخدم get() هنا
  });
  return <div>{count}</div>;
}

// ✅ صحيح - استخدم state parameter
function Component() {
  const count = useStore((state) => state.count);
  return <div>{count}</div>;
}

// ✅ صحيح - استخدم get() في actions فقط
const useStore = create((set, get) => ({
  count: 0,
  increment: () => {
    const current = get().count; // ✅ صحيح هنا
    set({ count: current + 1 });
  },
}));
```

---

## 🛒 المشروع العملي: Complete Shopping Cart

### useCartStore Implementation

```tsx
// stores/cartStore.ts
import { create } from 'zustand';
import { persist, createJSONStorage } from 'zustand/middleware';

// Types
export interface CartItem {
  id: number;
  name: string;
  price: number;
  quantity: number;
  image: string;
  category: string;
}

interface CartStore {
  items: CartItem[];
  
  // Actions
  addItem: (item: Omit<CartItem, 'quantity'>) => void;
  removeItem: (id: number) => void;
  increaseQuantity: (id: number) => void;
  decreaseQuantity: (id: number) => void;
  clearCart: () => void;
  
  // Derived state
  getTotal: () => number;
  getItemCount: () => number;
  isEmpty: () => boolean;
}

export const useCartStore = create<CartStore>()(
  persist(
    (set, get) => ({
      items: [],
      
      // Add item to cart
      addItem: (item) => set((state) => {
        const existingItem = state.items.find((i) => i.id === item.id);
        
        if (existingItem) {
          return {
            items: state.items.map((i) =>
              i.id === item.id
                ? { ...i, quantity: i.quantity + 1 }
                : i
            ),
          };
        }
        
        return {
          items: [...state.items, { ...item, quantity: 1 }],
        };
      }),
      
      // Remove item from cart
      removeItem: (id) => set((state) => ({
        items: state.items.filter((item) => item.id !== id),
      })),
      
      // Increase quantity
      increaseQuantity: (id) => set((state) => ({
        items: state.items.map((item) =>
          item.id === id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        ),
      })),
      
      // Decrease quantity
      decreaseQuantity: (id) => set((state) => ({
        items: state.items
          .map((item) =>
            item.id === id
              ? { ...item, quantity: item.quantity - 1 }
              : item
          )
          .filter((item) => item.quantity > 0),
      })),
      
      // Clear cart
      clearCart: () => set({ items: [] }),
      
      // Get total price
      getTotal: () => {
        return get().items.reduce(
          (total, item) => total + item.price * item.quantity,
          0
        );
      },
      
      // Get total items count
      getItemCount: () => {
        return get().items.reduce((count, item) => count + item.quantity, 0);
      },
      
      // Check if cart is empty
      isEmpty: () => {
        return get().items.length === 0;
      },
    }),
    {
      name: 'cart-storage',
      storage: createJSONStorage(() => localStorage),
    }
  )
);
```

### useAuthStore Implementation

```tsx
// stores/authStore.ts
import { create } from 'zustand';
import { persist, createJSONStorage } from 'zustand/middleware';

// Types
export interface User {
  id: number;
  name: string;
  email: string;
  avatar?: string;
}

interface AuthStore {
  user: User | null;
  isAuthenticated: boolean;
  
  // Actions
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
  updateUser: (updates: Partial<User>) => void;
}

export const useAuthStore = create<AuthStore>()(
  persist(
    (set) => ({
      user: null,
      isAuthenticated: false,
      
      // Login action
      login: async (email, password) => {
        // Simulate API call
        const response = await fetch('/api/login', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ email, password }),
        });
        
        if (!response.ok) {
          throw new Error('Login failed');
        }
        
        const user = await response.json();
        
        set({
          user,
          isAuthenticated: true,
        });
      },
      
      // Logout action
      logout: () => {
        set({
          user: null,
          isAuthenticated: false,
        });
      },
      
      // Update user
      updateUser: (updates) => set((state) => ({
        user: state.user ? { ...state.user, ...updates } : null,
      })),
    }),
    {
      name: 'auth-storage',
      storage: createJSONStorage(() => localStorage),
    }
  )
);
```

---

## 🏗️ Project Architecture

```
Components
    ↓
Zustand Store
    ↓
State
```

### Before Zustand (Prop Drilling)

```
App (state: cart, user)
  ↓
Layout (props: cart, user)
  ↓
Header (props: cart, user)
  ↓
Products (props: cart, setCart)
  ↓
ProductCard (props: cart, setCart)
  ↓
AddToCartButton (props: cart, setCart)
```

### After Zustand (No Prop Drilling)

```
App
  ↓
Layout
  ↓
Header (useCartStore, useAuthStore)
  ↓
Products (useCartStore)
  ↓
ProductCard (useCartStore)
  ↓
AddToCartButton (useCartStore)
```

---

## 📄 صفحات المشروع

### 1. Products Page

```tsx
// pages/Products.tsx
import { useCartStore } from '../stores/cartStore';
import { useAuthStore } from '../stores/authStore';

// Mock data
const PRODUCTS = [
  {
    id: 1,
    name: 'Laptop',
    price: 999,
    image: '/laptop.jpg',
    category: 'Electronics',
  },
  {
    id: 2,
    name: 'Headphones',
    price: 199,
    image: '/headphones.jpg',
    category: 'Electronics',
  },
  {
    id: 3,
    name: 'Mouse',
    price: 49,
    image: '/mouse.jpg',
    category: 'Electronics',
  },
  {
    id: 4,
    name: 'Keyboard',
    price: 79,
    image: '/keyboard.jpg',
    category: 'Electronics',
  },
];

export function Products() {
  const addItem = useCartStore((state) => state.addItem);
  const user = useAuthStore((state) => state.user);
  
  return (
    <div className="container mx-auto p-4">
      <h1 className="text-3xl font-bold mb-6">Products</h1>
      
      {user && (
        <p className="mb-4">Welcome, {user.name}!</p>
      )}
      
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
        {PRODUCTS.map((product) => (
          <ProductCard
            key={product.id}
            product={product}
            onAddToCart={() => addItem(product)}
          />
        ))}
      </div>
    </div>
  );
}

function ProductCard({ product, onAddToCart }: any) {
  return (
    <div className="border rounded-lg p-4 shadow-sm hover:shadow-md transition">
      <img
        src={product.image}
        alt={product.name}
        className="w-full h-48 object-cover mb-4 rounded"
      />
      <h3 className="font-semibold text-lg">{product.name}</h3>
      <p className="text-gray-600">{product.category}</p>
      <p className="text-2xl font-bold mt-2">${product.price}</p>
      <button
        onClick={onAddToCart}
        className="mt-4 w-full bg-blue-500 text-white py-2 rounded hover:bg-blue-600 transition"
      >
        Add to Cart
      </button>
    </div>
  );
}
```

### 2. Product Details Page

```tsx
// pages/ProductDetails.tsx
import { useParams } from 'react-router-dom';
import { useCartStore } from '../stores/cartStore';

const PRODUCTS = [
  {
    id: 1,
    name: 'Laptop',
    price: 999,
    image: '/laptop.jpg',
    category: 'Electronics',
    description: 'High-performance laptop for professionals',
  },
  // ... other products
];

export function ProductDetails() {
  const { id } = useParams<{ id: string }>();
  const addItem = useCartStore((state) => state.addItem);
  
  const product = PRODUCTS.find((p) => p.id === Number(id));
  
  if (!product) {
    return <div>Product not found</div>;
  }
  
  return (
    <div className="container mx-auto p-4">
      <div className="grid grid-cols-1 md:grid-cols-2 gap-8">
        <div>
          <img
            src={product.image}
            alt={product.name}
            className="w-full rounded-lg shadow-lg"
          />
        </div>
        
        <div>
          <h1 className="text-3xl font-bold mb-4">{product.name}</h1>
          <p className="text-gray-600 mb-4">{product.category}</p>
          <p className="text-4xl font-bold mb-6">${product.price}</p>
          
          <p className="text-gray-700 mb-6">{product.description}</p>
          
          <button
            onClick={() => addItem(product)}
            className="bg-blue-500 text-white px-8 py-3 rounded-lg hover:bg-blue-600 transition"
          >
            Add to Cart
          </button>
        </div>
      </div>
    </div>
  );
}
```

### 3. Cart Page

```tsx
// pages/Cart.tsx
import { useCartStore } from '../stores/cartStore';
import { useAuthStore } from '../stores/authStore';

export function Cart() {
  const items = useCartStore((state) => state.items);
  const removeItem = useCartStore((state) => state.removeItem);
  const increaseQuantity = useCartStore((state) => state.increaseQuantity);
  const decreaseQuantity = useCartStore((state) => state.decreaseQuantity);
  const clearCart = useCartStore((state) => state.clearCart);
  const getTotal = useCartStore((state) => state.getTotal);
  const isEmpty = useCartStore((state) => state.isEmpty);
  
  const isAuthenticated = useAuthStore((state) => state.isAuthenticated);
  
  if (isEmpty()) {
    return (
      <div className="container mx-auto p-4 text-center">
        <h1 className="text-2xl font-bold mb-4">Your cart is empty</h1>
        <a href="/products" className="text-blue-500 hover:underline">
          Continue Shopping
        </a>
      </div>
    );
  }
  
  return (
    <div className="container mx-auto p-4">
      <h1 className="text-3xl font-bold mb-6">Shopping Cart</h1>
      
      <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
        {/* Cart Items */}
        <div className="lg:col-span-2">
          {items.map((item) => (
            <CartItem
              key={item.id}
              item={item}
              onRemove={() => removeItem(item.id)}
              onIncrease={() => increaseQuantity(item.id)}
              onDecrease={() => decreaseQuantity(item.id)}
            />
          ))}
          
          <button
            onClick={clearCart}
            className="mt-4 text-red-500 hover:underline"
          >
            Clear Cart
          </button>
        </div>
        
        {/* Cart Summary */}
        <div className="bg-gray-100 p-6 rounded-lg h-fit">
          <h2 className="text-xl font-bold mb-4">Order Summary</h2>
          
          <div className="space-y-2 mb-4">
            <div className="flex justify-between">
              <span>Subtotal</span>
              <span>${getTotal().toFixed(2)}</span>
            </div>
            <div className="flex justify-between">
              <span>Tax (10%)</span>
              <span>${(getTotal() * 0.1).toFixed(2)}</span>
            </div>
            <div className="flex justify-between font-bold text-lg border-t pt-2">
              <span>Total</span>
              <span>${(getTotal() * 1.1).toFixed(2)}</span>
            </div>
          </div>
          
          {isAuthenticated ? (
            <a
              href="/checkout"
              className="block w-full bg-green-500 text-white text-center py-3 rounded-lg hover:bg-green-600 transition"
            >
              Proceed to Checkout
            </a>
          ) : (
            <a
              href="/login"
              className="block w-full bg-blue-500 text-white text-center py-3 rounded-lg hover:bg-blue-600 transition"
            >
              Login to Checkout
            </a>
          )}
        </div>
      </div>
    </div>
  );
}

function CartItem({ item, onRemove, onIncrease, onDecrease }: any) {
  return (
    <div className="flex items-center gap-4 p-4 border rounded-lg mb-4">
      <img
        src={item.image}
        alt={item.name}
        className="w-24 h-24 object-cover rounded"
      />
      
      <div className="flex-1">
        <h3 className="font-semibold">{item.name}</h3>
        <p className="text-gray-600">${item.price}</p>
      </div>
      
      <div className="flex items-center gap-2">
        <button
          onClick={onDecrease}
          className="w-8 h-8 bg-gray-200 rounded hover:bg-gray-300"
        >
          -
        </button>
        <span className="w-8 text-center">{item.quantity}</span>
        <button
          onClick={onIncrease}
          className="w-8 h-8 bg-gray-200 rounded hover:bg-gray-300"
        >
          +
        </button>
      </div>
      
      <div className="text-right">
        <p className="font-bold">
          ${(item.price * item.quantity).toFixed(2)}
        </p>
        <button
          onClick={onRemove}
          className="text-red-500 text-sm hover:underline"
        >
          Remove
        </button>
      </div>
    </div>
  );
}
```

### 4. Checkout Page

```tsx
// pages/Checkout.tsx
import { useCartStore } from '../stores/cartStore';
import { useAuthStore } from '../stores/authStore';
import { useState } from 'react';

export function Checkout() {
  const items = useCartStore((state) => state.items);
  const getTotal = useCartStore((state) => state.getTotal);
  const clearCart = useCartStore((state) => state.clearCart);
  
  const user = useAuthStore((state) => state.user);
  const isAuthenticated = useAuthStore((state) => state.isAuthenticated);
  
  const [isProcessing, setIsProcessing] = useState(false);
  const [orderComplete, setOrderComplete] = useState(false);
  
  if (!isAuthenticated) {
    return (
      <div className="container mx-auto p-4 text-center">
        <h1 className="text-2xl font-bold mb-4">Please login to checkout</h1>
        <a href="/login" className="text-blue-500 hover:underline">
          Login
        </a>
      </div>
    );
  }
  
  if (orderComplete) {
    return (
      <div className="container mx-auto p-4 text-center">
        <div className="max-w-md mx-auto bg-green-100 p-8 rounded-lg">
          <h1 className="text-3xl font-bold text-green-700 mb-4">
            Order Complete!
          </h1>
          <p className="text-gray-700 mb-6">
            Thank you for your purchase, {user?.name}!
          </p>
          <a
            href="/products"
            className="inline-block bg-blue-500 text-white px-6 py-3 rounded-lg hover:bg-blue-600 transition"
          >
            Continue Shopping
          </a>
        </div>
      </div>
    );
  }
  
  const handleCheckout = async () => {
    setIsProcessing(true);
    
    // Simulate API call
    await new Promise((resolve) => setTimeout(resolve, 2000));
    
    clearCart();
    setOrderComplete(true);
    setIsProcessing(false);
  };
  
  return (
    <div className="container mx-auto p-4">
      <h1 className="text-3xl font-bold mb-6">Checkout</h1>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
        {/* Shipping Information */}
        <div className="bg-gray-100 p-6 rounded-lg">
          <h2 className="text-xl font-bold mb-4">Shipping Information</h2>
          
          <form className="space-y-4">
            <div>
              <label className="block mb-2">Name</label>
              <input
                type="text"
                defaultValue={user?.name}
                className="w-full p-2 border rounded"
                readOnly
              />
            </div>
            
            <div>
              <label className="block mb-2">Email</label>
              <input
                type="email"
                defaultValue={user?.email}
                className="w-full p-2 border rounded"
                readOnly
              />
            </div>
            
            <div>
              <label className="block mb-2">Address</label>
              <input
                type="text"
                placeholder="Enter your address"
                className="w-full p-2 border rounded"
                required
              />
            </div>
            
            <div>
              <label className="block mb-2">City</label>
              <input
                type="text"
                placeholder="Enter your city"
                className="w-full p-2 border rounded"
                required
              />
            </div>
          </form>
        </div>
        
        {/* Order Summary */}
        <div className="bg-gray-100 p-6 rounded-lg">
          <h2 className="text-xl font-bold mb-4">Order Summary</h2>
          
          <div className="space-y-2 mb-4">
            {items.map((item) => (
              <div key={item.id} className="flex justify-between">
                <span>
                  {item.name} x {item.quantity}
                </span>
                <span>${(item.price * item.quantity).toFixed(2)}</span>
              </div>
            ))}
          </div>
          
          <div className="border-t pt-4 space-y-2">
            <div className="flex justify-between">
              <span>Subtotal</span>
              <span>${getTotal().toFixed(2)}</span>
            </div>
            <div className="flex justify-between">
              <span>Tax (10%)</span>
              <span>${(getTotal() * 0.1).toFixed(2)}</span>
            </div>
            <div className="flex justify-between font-bold text-lg">
              <span>Total</span>
              <span>${(getTotal() * 1.1).toFixed(2)}</span>
            </div>
          </div>
          
          <button
            onClick={handleCheckout}
            disabled={isProcessing}
            className="mt-6 w-full bg-green-500 text-white py-3 rounded-lg hover:bg-green-600 transition disabled:bg-gray-400"
          >
            {isProcessing ? 'Processing...' : 'Place Order'}
          </button>
        </div>
      </div>
    </div>
  );
}
```

### Header Component (Using Both Stores)

```tsx
// components/Header.tsx
import { Link } from 'react-router-dom';
import { useCartStore } from '../stores/cartStore';
import { useAuthStore } from '../stores/authStore';

export function Header() {
  const itemCount = useCartStore((state) => state.getItemCount());
  const user = useAuthStore((state) => state.user);
  const isAuthenticated = useAuthStore((state) => state.isAuthenticated);
  const logout = useAuthStore((state) => state.logout);
  
  return (
    <header className="bg-white shadow-md">
      <div className="container mx-auto px-4 py-4">
        <div className="flex items-center justify-between">
          <Link to="/" className="text-2xl font-bold text-blue-500">
            Shop
          </Link>
          
          <nav className="flex items-center gap-6">
            <Link to="/products" className="hover:text-blue-500">
              Products
            </Link>
            
            <Link to="/cart" className="relative hover:text-blue-500">
              Cart
              {itemCount() > 0 && (
                <span className="absolute -top-2 -right-2 bg-red-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center">
                  {itemCount()}
                </span>
              )}
            </Link>
            
            {isAuthenticated ? (
              <div className="flex items-center gap-4">
                <span>Welcome, {user?.name}</span>
                <button
                  onClick={logout}
                  className="text-red-500 hover:underline"
                >
                  Logout
                </button>
              </div>
            ) : (
              <Link to="/login" className="hover:text-blue-500">
                Login
              </Link>
            )}
          </nav>
        </div>
      </div>
    </header>
  );
}
```

---

## 📊 Comparison: Before vs After Zustand

### Before Zustand (Prop Drilling)

```tsx
// ❌ Prop Drilling Hell
function App() {
  const [cart, setCart] = useState([]);
  const [user, setUser] = useState(null);
  
  return (
    <div>
      <Header cart={cart} user={user} />
      <Products cart={cart} setCart={setCart} user={user} />
      <Cart cart={cart} setCart={setCart} user={user} />
    </div>
  );
}

function Header({ cart, user }) {
  const itemCount = cart.reduce((sum, item) => sum + item.quantity, 0);
  return (
    <header>
      <span>Cart: {itemCount}</span>
      <span>User: {user?.name}</span>
    </header>
  );
}

function Products({ cart, setCart, user }) {
  return (
    <div>
      {products.map(product => (
        <ProductCard 
          key={product.id} 
          product={product}
          cart={cart}
          setCart={setCart}
          user={user}
        />
      ))}
    </div>
  );
}

function ProductCard({ product, cart, setCart, user }) {
  const addToCart = () => {
    setCart([...cart, { ...product, quantity: 1 }]);
  };
  
  return (
    <div>
      <h3>{product.name}</h3>
      <button onClick={addToCart}>Add to Cart</button>
    </div>
  );
}
```

**المشاكل**:
- ❌ تمرير props عبر كل مستوى
- ❌ كل تغيير في الحالة يسبب re-render للكل
- ❌ صعب الصيانة والإضافة
- ❌ code تكراري

### After Zustand (No Prop Drilling)

```tsx
// ✅ Clean and Simple
function App() {
  return (
    <div>
      <Header />
      <Products />
      <Cart />
    </div>
  );
}

function Header() {
  const itemCount = useCartStore((state) => state.getItemCount());
  const user = useAuthStore((state) => state.user);
  
  return (
    <header>
      <span>Cart: {itemCount()}</span>
      <span>User: {user?.name}</span>
    </header>
  );
}

function Products() {
  const addItem = useCartStore((state) => state.addItem);
  
  return (
    <div>
      {products.map(product => (
        <ProductCard 
          key={product.id} 
          product={product}
          onAddToCart={() => addItem(product)}
        />
      ))}
    </div>
  );
}

function ProductCard({ product, onAddToCart }) {
  return (
    <div>
      <h3>{product.name}</h3>
      <button onClick={onAddToCart}>Add to Cart</button>
    </div>
  );
}
```

**المميزات**:
- ✅ لا prop drilling
- ✅ selective re-renders
- ✅ سهل الصيانة والإضافة
- ✅ code نظيف ومنظم

---

## 📝 Summary

### النقاط الرئيسية

1. **Prop Drilling**: مشكلة شائعة تحدث عند تمرير البيانات عبر مستويات متعددة
2. **Local State**: للمكونات المنفصلة باستخدام `useState`
3. **Lifted State**: رفع الحالة لأقرب مكون مشترك
4. **Context API**: حل رسمي لتجنب prop drilling
5. **Global State**: لإدارة الحالة المعقدة باستخدام Zustand
6. **Server State**: للبيانات من الخادم باستخدام React Query

### Zustand Key Points

- 🎯 صغير جداً (~1KB)
- ⚡ سريع和高性能
- 🔧 API بسيط
- 📝 TypeScript دعم ممتاز
- 🔌 Middlewares قوية
- 🛠️ DevTools متاحة

### Best Practices

1. استخدم Selectors للأداء
2. افصل Stores حسب المسؤولية
3. استخدم TypeScript Interfaces
4. استخدم Persist للبيانات المهمة
5. تجنب Logic المعقد في Store
6. استخدم DevTools للتطوير

---

## 📊 Comparison Table

| Feature | useState | Context | Zustand | React Query |
|---------|----------|---------|---------|-------------|
| **Use Case** | Local state | Global simple state | Global complex state | Server state |
| **Bundle Size** | Part of React | Part of React | ~1KB | ~13KB |
| **Learning Curve** | Very easy | Easy | Easy | Medium |
| **Boilerplate** | None | Low | Very low | Medium |
| **Performance** | Excellent | Good (re-renders all) | Excellent (selective) | Excellent |
| **TypeScript** | Excellent | Good | Excellent | Excellent |
| **DevTools** | React DevTools | React DevTools | Zustand DevTools | React Query DevTools |
| **Persistence** | Manual | Manual | Built-in | Manual |
| **Async Actions** | Manual | Manual | Manual | Built-in |
| **Caching** | No | No | No | Built-in |
| **Auto-refetch** | No | No | No | Built-in |
| **Best For** | Simple local state | Theme, language, auth | Complex app state | API data |

---

## ❓ 15 Questions

### أسئلة الفهم

1. **ما هو Prop Drilling؟**
   - Prop Drilling هو مشكلة تحدث عندما تحتاج لتمرير البيانات عبر عدة مستويات من المكونات للوصول إلى مكون عميق.

2. **متى تستخدم useState؟**
   - عندما تحتاج حالة خاصة بمكون واحد فقط.
   - للبيانات البسيطة (boolean, string, number).
   - للـ UI temporary state.

3. **ما الفرق بين Local State و Global State؟**
   - Local State: خاصة بمكون واحد ولا تشارك.
   - Global State: متاحة لكل التطبيق.

4. **ما هي مشكلة Context API الرئيسية؟**
   - Re-render لكل المكونات المستهلكة عند أي تغيير في الحالة.

5. **لماذا Zustand أفضل من Redux؟**
   - أقل كود (80% أقل).
   - API بسيط جداً.
   - حجم أصغر (~1KB vs ~15KB).
   - لا يحتاج Provider.

6. **ما هو Selector في Zustand؟**
   - Function تستخرج جزء محدد من الحالة لتحسين الأداء.

7. **كيف تحافظ على البيانات في Zustand؟**
   - باستخدام persist middleware.

8. **ما الفرق بين Client State و Server State؟**
   - Client State: بيانات مولدة على العميل (forms, UI state).
   - Server State: بيانات قادمة من الخادم (API data).

9. **متى تستخدم React Query؟**
   - عند التعامل مع بيانات من الخادم.
   - عندما تحتاج caching.
   - عندما تحتاج auto-refetching.

10. **ما هي Derived State؟**
    - حالة محسوبة من حالة أخرى (مثل total من cart items).

11. **كيف تتجنب re-renders غير ضرورية في Zustand؟**
    - باستخدام selectors لقراءة فقط ما تحتاج.

12. **ما هي أفضل ممارسة لتقسيم Stores؟**
    - تقسيم حسب المسؤولية (user store, cart store, theme store).

13. **كيف تتعامل مع Async Actions في Zustand؟**
    - باستخدام async/await داخل action مع loading و error states.

14. **ما هو الفرق بين set() و get() في Zustand؟**
    - set(): لتحديث الحالة.
    - get(): لقراءة الحالة الحالية (يستخدم في actions فقط).

15. **متى لا تستخدم Zustand؟**
    - للتطبيقات البسيطة جداً.
    - عندما الحالة محلية فقط.
    - عندما تحتاج server state (استخدم React Query).

---

## 🎯 5 Interview Questions

### أسئلة المقابلات

1. **شرح Prop Drilling وكيف تحلها؟**
   **الإجابة**: Prop Drilling هو تمرير props عبر مستويات متعددة. الحلول:
   - Context API للبيانات العالمية البسيطة
   - Zustand/Redux للحالات المعقدة
   - Composition Pattern

2. **متى تختار Context API ومتى تختار Zustand؟**
   **الإجابة**: 
   - Context API: للبيانات البسيطة (theme, language) عندما المكونات المستهلكة قليلة.
   - Zustand: للحالات المعقدة عندما تحتاج selective re-renders وأداء عالي.

3. **كيف يحسن Zustand الأداء مقارنة بـ Context؟**
   **الإجابة**: Zustand يستخدم selectors لـ re-render المكونات المتأثرة فقط، بينما Context يعيد render كل المكونات المستهلكة.

4. **ما الفرق بين Client State و Server State؟**
   **الإجابة**: 
   - Client State: مولدة على العميل (forms, UI) تستخدم useState/Zustand.
   - Server State: من الخادم (API) تستخدم React Query للـ caching و sync.

5. **كيف تصمم architecture لإدارة الحالة في تطبيق كبير؟**
   **الإجابة**: 
   - Local State: useState للمكونات المنفصلة
   - Global State: Zustand للحالة المشتركة المعقدة
   - Server State: React Query للبيانات من API
   - تقسيم stores حسب المسؤولية

---

## 💪 Exercises

### تمرين 1: إنشاء Todo Store

أنشئ store لإدارة todos باستخدام Zustand:

```tsx
// المطلوب:
interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

// Actions:
- addTodo(text: string)
- toggleTodo(id: number)
- removeTodo(id: number)
- clearCompleted()
```

### تمرين 2: إنشاء Theme Store

أنشئ store لإدارة theme مع persist:

```tsx
// المطلوب:
interface ThemeStore {
  theme: 'light' | 'dark'
  toggleTheme()
  setTheme(theme: 'light' | 'dark')
}
// يجب حفظ في localStorage
```

### تمرين 3: تحسين Cart Store

أضف الميزات التالية لـ cart store:

```tsx
// المطلوب:
- coupon: string | null
- discount: number
- applyCoupon(code: string)
- removeCoupon()
- getDiscountedTotal()
```

### تمرين 4: إنشاء Notification Store

أنشئ store لإدارة notifications:

```tsx
// المطلوب:
interface Notification {
  id: number;
  message: string;
  type: 'success' | 'error' | 'warning' | 'info';
}

// Actions:
- addNotification(message, type)
- removeNotification(id)
- clearNotifications()
```

### تمرين 5: دمج React Query مع Zustand

أنشئ store يجمع بين server state و client state:

```tsx
// المطلوب:
interface ProductStore {
  // من React Query
  products: Product[]
  isLoading: boolean
  error: Error | null
  
  // من Zustand
  filters: {
    category: string
    priceRange: [number, number]
    search: string
  }
  
  // Actions
  setFilters(filters)
  filteredProducts(): Product[]
}
```

---

## 📚 Homework

### الواجب المنزلي

1. **أكمل المشروع العملي**:
   - أضف صفحة Login/Register
   - أضف صفحة Order History
   - أضف Wishlist functionality
   - أضف Product Reviews

2. **أضف الميزات التالية**:
   - Coupon codes system
   - Product search and filters
   - User profile management
   - Order tracking

3. **تحسين الأداء**:
   - استخدم useMemo للحسابات الثقيلة
   - أضف lazy loading للصفحات
   - استخدم React.memo للمكونات

4. **Testing**:
   - اكتب unit tests للـ stores
   - اكتب integration tests للمكونات
   - استخدم Jest و React Testing Library

5. **Documentation**:
   - وثق كل store و actions
   - أضف أمثلة استخدام
   - أنشئ README للمشروع

---

## 🏆 Challenge

### التحدي المتقدم

أنشئ تطبيق E-commerce كامل بالميزات التالية:

**المتطلبات**:

1. **Authentication System**:
   - Login/Register/Logout
   - Password reset
   - Email verification
   - Social login (Google, Facebook)

2. **Product Management**:
   - Product categories
   - Product search and filters
   - Product sorting
   - Product comparison
   - Product reviews and ratings

3. **Shopping Cart**:
   - Add/remove items
   - Quantity management
   - Coupon codes
   - Save cart for later
   - Multiple addresses

4. **Checkout Process**:
   - Multi-step checkout
   - Payment integration (Stripe)
   - Order confirmation
   - Email notifications

5. **User Dashboard**:
   - Order history
   - Order tracking
   - Wishlist management
   - Profile settings
   - Address book

6. **Admin Panel**:
   - Product management
   - Order management
   - User management
   - Analytics dashboard
   - Reports generation

**التقنيات المطلوبة**:
- React + TypeScript
- Zustand (state management)
- React Query (server state)
- Tailwind CSS (styling)
- React Router (routing)
- Firebase/Auth0 (authentication)
- Stripe (payments)

**معايير النجاح**:
- ✅ Architecture نظيفة ومنظمة
- ✅ Code قابل للصيانة
- ✅ Performance ممتاز
- ✅ Responsive design
- ✅ Accessibility (WCAG AA)
- ✅ Error handling كامل
- ✅ Testing coverage > 80%
- ✅ Documentation كاملة

---

## 🎓 الخاتمة

في هذه الجلسة تعلمنا:

1. ✅ فهم مشكلة Prop Drilling وحلولها
2. ✅ أنواع State المختلفة في React
3. ✅ مقارنة أدوات State Management
4. ✅ إتقان Zustand من الأساسيات للمتقدم
5. ✅ بناء مشروع عملي كامل
6. ✅ أفضل الممارسات والتجنب للأخطاء الشائعة

**الخطوات القادمة**:
- تطبيق ما تعلمته في مشاريع حقيقية
- استكشاف middlewares متقدمة في Zustand
- تعلم Redux Toolkit للمقارنة
- استكشاف حلول أخرى مثل Jotai و Recoil

**موارد إضافية**:
- [Zustand Documentation](https://zustand-demo.pmnd.rs/)
- [React Query Documentation](https://tanstack.com/query/latest)
- [State Management Patterns](https://kentcdodds.com/blog/application-state-management-patterns-with-react-hooks)

---

**تمنياتي بالتوفيق في رحلتك مع React و State Management! 🚀**

