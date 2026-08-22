# الجلسة التاسعة: React Query, Authentication & Performance

## 📋 نظرة عامة
- **المدة**: 3 ساعات (180 دقيقة)
- **المستوى**: متقدم - Production Ready
- **التقنيات**: React + TypeScript + React Query + Zustand + Tailwind CSS
- **الموضوع**: تطبيقات React الإنتاجية

---

## ⏰ تقسيم الجلسة بالدقائق

### PART 1 — React Query (60 دقيقة)
- **0-5 دقيقة**: مقدمة Server State vs Client State
- **5-10 دقيقة**: Why React Query؟
- **10-20 دقيقة**: useQuery الأساسيات
- **20-30 دقيقة**: Caching & Stale Data
- **30-40 دقيقة**: useMutation & CRUD Operations
- **40-50 دقيقة**: invalidateQueries & Optimistic Updates
- **50-60 دقيقة**: أمثلة عملية

### PART 2 — Authentication (50 دقيقة)
- **60-65 دقيقة**: Login Flow & Tokens
- **65-70 دقيقة**: Access Token & Refresh Token
- **70-75 دقيقة**: Authentication vs Authorization
- **75-85 دقيقة**: Protected Routes
- **85-90 دقيقة**: User Roles (Admin/User)
- **90-100 دقيقة**: Logout Implementation
- **100-110 دقيقة**: أمثلة عملية

### PART 3 — Performance (50 دقيقة)
- **110-115 دقيقة**: Re-rendering المشاكل
- **115-125 دقيقة**: React.memo, useMemo, useCallback
- **125-135 دقيقة**: Lazy Loading & Code Splitting
- **135-145 دقيقة**: Bundle Size Optimization
- **145-155 دقيقة**: React DevTools & Profiler
- **155-160 دقيقة**: أمثلة عملية

### المشروع العملي (20 دقيقة)
- **160-170 دقيقة**: Admin Dashboard Architecture
- **170-180 دقيقة**: Implementation & Best Practices

---

## 🎯 الأهداف
- فهم وإتقان React Query لإدارة Server State
- بناء نظام Authentication قوي
- تحسين أداء التطبيقات React
- بناء Admin Dashboard production-ready
- فهم متى تستخدم React Query ومتى تستخدم Zustand

---

## PART 1 — React Query

### 1. What is Server State?

**التعريف**: Server State هو البيانات القادمة من خادم خارجي (API, Database) تحتاج إلى مزامنة وإدارة.

**خصائص Server State**:
- 📍 مخزنة في مكان خارجي (خادم)
- 🔄 تحتاج sync مع الخادم
- ⏱️ قد تصبح stale (قديمة)
- 🌐 تحتاج requests للوصول إليها
- ⚠️ قد تفشل (network errors)
- 📦 تحتاج caching
- 🔄 تحتاج refetching

**أمثلة على Server State**:
- User profiles من API
- Products list من database
- Posts و comments
- Orders و transactions
- أي بيانات من backend

### 2. Client State vs Server State

#### Client State
```tsx
// البيانات المولدة والمخزنة على العميل
function Form() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });
  
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [theme, setTheme] = useState('light');
  
  // هذه client state - لا تحتاج sync مع خادم
}
```

**خصائص Client State**:
- 💾 مخزنة محلياً في المتصفح
- 🎛️ يتحكم بها المستخدم مباشرة
- ⚡ فورية (no network latency)
- 🔄 لا تحتاج sync
- 📱 يمكن حفظها في localStorage

**متى تستخدم Client State؟**
- Form inputs
- UI state (modals, dropdowns, tabs)
- User preferences (theme, language)
- Temporary data
- Animation states

#### Server State
```tsx
// البيانات القادمة من الخادم
function UsersList() {
  const { data: users, isLoading, error } = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers
  });
  
  // هذه server state - تحتاج sync مع الخادم
}
```

**خصائص Server State**:
- 🌐 مخزنة على خادم
- 🔄 تحتاج مزامنة
- ⏱️ تحتاج تحديث دوري
- 📦 تحتاج caching
- ⚠️ قد تفشل requests

**متى تستخدم Server State؟**
- أي بيانات من API
- User data من database
- Products, orders, posts
- Real-time data
- Analytics data

### 3. Why React Query?

#### المشاكل بدون React Query

```tsx
// ❌ المشاكل مع fetch يدوي
function UsersList() {
  const [users, setUsers] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchUsers = async () => {
      setIsLoading(true);
      try {
        const response = await fetch('/api/users');
        const data = await response.json();
        setUsers(data);
      } catch (err) {
        setError(err);
      } finally {
        setIsLoading(false);
      }
    };
    
    fetchUsers();
  }, []);
  
  // ❌ لا يوجد caching
  // ❌ لا يوجد deduplication
  // ❌ لا يوجد auto-refetch
  // ❌ لا يوجد optimistic updates
  // ❌ كود كثير للتكرار
}
```

#### الحل مع React Query

```tsx
// ✅ مع React Query
function UsersList() {
  const { data: users, isLoading, error } = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers
  });
  
  // ✅ caching تلقائي
  // ✅ deduplication للطلبات
  // ✅ auto-refetch
  // ✅ optimistic updates
  // ✅ كود بسيط ونظيف
}
```

#### مميزات React Query

1. **Caching تلقائي**: يحفظ البيانات تلقائياً
2. **Deduplication**: يجمع الطلبات المتطابقة
3. **Auto-refetch**: يعيد تحميل البيانات عند الحاجة
4. **Loading & Error states**: مدمجة وجاهزة
5. **Optimistic Updates**: تحديثات تفاؤلية
6. **Pagination & Infinite Scroll**: دعم مدمج
7. **Background Refetch**: تحديث في الخلفية
8. **DevTools**: أدوات تطوير قوية

#### مقارنة الحجم والأداء

| المكتبة | الحجم | الغرض |
|---------|-------|-------|
| React Query | ~13KB | Server State |
| Zustand | ~1KB | Client State |
| Redux | ~15KB | Global State |
| SWR | ~10KB | Server State |

### 4. useQuery

#### البنية الأساسية

```tsx
import { useQuery } from '@tanstack/react-query';

const { data, isLoading, error, refetch } = useQuery({
  queryKey: ['key'],
  queryFn: fetchFunction,
  options: {}
});
```

#### مثال بسيط

```tsx
// دالة fetch
async function fetchUsers() {
  const response = await fetch('https://api.example.com/users');
  if (!response.ok) {
    throw new Error('Failed to fetch users');
  }
  return response.json();
}

// في المكون
function UsersList() {
  const {
    data: users,
    isLoading,
    error,
    isSuccess,
    isError,
    refetch
  } = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers
  });
  
  if (isLoading) return <div>Loading...</div>;
  
  if (error) return <div>Error: {error.message}</div>;
  
  return (
    <div>
      <h1>Users</h1>
      <button onClick={() => refetch()}>Refresh</button>
      <ul>
        {users?.map(user => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

#### القيم المرجعة من useQuery

```tsx
const {
  // البيانات
  data,           // البيانات المسترجعة
  dataUpdatedAt,  // وقت آخر تحديث للبيانات
  
  // الحالات
  isLoading,      // جاري التحميل (أول مرة)
  isFetching,     // جاري التحديث (refetch)
  isSuccess,      // نجح الطلب
  isError,        // فشل الطلب
  
  // الأخطاء
  error,          // كائن الخطأ
  
  // الأحداث
  refetch,        // إعادة تحميل البيانات
  
  // معلومات إضافية
  isStale,        // البيانات قديمة
  isPlaceholderData, // بيانات مؤقتة
} = useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers
});
```

### 5. queryKey

#### ما هو queryKey؟

queryKey هو معرف فريد لكل query، يستخدم في:
- 📦 Caching
- 🔄 Refetching
- 🗑️ Invalidating
- 📊 Tracking

#### أنواع queryKey

```tsx
// 1. Simple key
useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers
})

// 2. Key with parameters
useQuery({
  queryKey: ['user', userId],
  queryFn: () => fetchUser(userId)
})

// 3. Key with multiple parameters
useQuery({
  queryKey: ['users', { page: 1, limit: 10 }],
  queryFn: () => fetchUsers({ page: 1, limit: 10 })
})

// 4. Key with filters
useQuery({
  queryKey: ['products', { category: 'electronics', minPrice: 100 }],
  queryFn: () => fetchProducts({ category: 'electronics', minPrice: 100 })
})

// 5. Complex key
useQuery({
  queryKey: ['posts', 'draft', { authorId: 5, sortBy: 'date' }],
  queryFn: () => fetchDraftPosts(5, 'date')
})
```

#### أفضل الممارسات لـ queryKey

```tsx
// ✅ جيد - مفصل ودقيق
useQuery({
  queryKey: ['users', { page, limit, search }],
  queryFn: () => fetchUsers({ page, limit, search })
})

// ❌ سيء - عام جداً
useQuery({
  queryKey: ['data'],
  queryFn: fetchUsers
})

// ✅ جيد - يستخدم constants
const QUERY_KEYS = {
  USERS: 'users',
  USER: (id: number) => ['user', id],
  PRODUCTS: (filters: ProductFilters) => ['products', filters],
} as const;

useQuery({
  queryKey: QUERY_KEYS.USERS,
  queryFn: fetchUsers
})
```

### 6. queryFn

#### ما هو queryFn؟

queryFn هي الدالة التي تجلب البيانات من الخادم.

```tsx
// queryFn يجب أن return Promise
async function fetchUsers(): Promise<User[]> {
  const response = await fetch('/api/users');
  return response.json();
}

useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers
})
```

#### queryFn مع parameters

```tsx
// ✅ جيد - تستخدم parameters من queryKey
useQuery({
  queryKey: ['user', userId],
  queryFn: ({ queryKey }) => {
    const [, id] = queryKey;
    return fetchUser(id);
  }
})

// ✅ أفضل - تستخدم context
useQuery({
  queryKey: ['user', userId],
  queryFn: async ({ queryKey }) => {
    const [, id] = queryKey;
    const response = await fetch(`/api/users/${id}`);
    return response.json();
  }
})
```

#### queryFn مع Axios

```tsx
import axios from 'axios';

async function fetchUsers(): Promise<User[]> {
  const { data } = await axios.get('/api/users');
  return data;
}

useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers
})
```

### 7. Loading States

#### أنواع Loading

```tsx
function UsersList() {
  const { data, isLoading, isFetching } = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers
  });
  
  // isLoading = أول مرة فقط
  if (isLoading) {
    return <div>Loading users...</div>;
  }
  
  // isFetching = أي تحديث (refetch)
  return (
    <div>
      {isFetching && <div>Refreshing...</div>}
      <ul>
        {data?.map(user => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

#### Loading Components

```tsx
function LoadingSpinner() {
  return (
    <div className="flex items-center justify-center">
      <div className="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-500" />
    </div>
  );
}

function UsersList() {
  const { data, isLoading } = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers
  });
  
  if (isLoading) return <LoadingSpinner />;
  
  return <UserList users={data} />;
}
```

#### Skeleton Loading

```tsx
function UserSkeleton() {
  return (
    <div className="animate-pulse">
      <div className="h-4 bg-gray-200 rounded w-3/4 mb-2" />
      <div className="h-4 bg-gray-200 rounded w-1/2" />
    </div>
  );
}

function UsersList() {
  const { data, isLoading } = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers
  });
  
  if (isLoading) {
    return (
      <div>
        <UserSkeleton />
        <UserSkeleton />
        <UserSkeleton />
      </div>
    );
  }
  
  return <UserList users={data} />;
}
```

### 8. Error Handling

#### Error States

```tsx
function UsersList() {
  const { data, isLoading, error, isError } = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers
  });
  
  if (isLoading) return <div>Loading...</div>;
  
  if (isError) {
    return (
      <div className="text-red-500">
        Error: {error.message}
        <button onClick={() => refetch()}>Retry</button>
      </div>
    );
  }
  
  return <UserList users={data} />;
}
```

#### Error Component

```tsx
function ErrorFallback({ error, resetErrorBoundary }: any) {
  return (
    <div className="p-4 bg-red-50 border border-red-200 rounded">
      <h2 className="text-red-700 font-bold">Something went wrong</h2>
      <p className="text-red-600">{error.message}</p>
      <button
        onClick={resetErrorBoundary}
        className="mt-2 px-4 py-2 bg-red-500 text-white rounded"
      >
        Try again
      </button>
    </div>
  );
}

function UsersList() {
  const { data, isLoading, error } = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers,
    onError: (error) => {
      console.error('Failed to fetch users:', error);
    }
  });
  
  if (error) {
    return <ErrorFallback error={error} resetErrorBoundary={() => refetch()} />;
  }
  
  // ...
}
```

#### Retry Logic

```tsx
useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers,
  retry: 3,              // عدد المحاولات
  retryDelay: 1000,       // تأخير بين المحاولات (ms)
  retryDelay: (attemptIndex) => Math.min(1000 * 2 ** attemptIndex, 30000), // exponential backoff
})
```

### 9. Refetch

#### Manual Refetch

```tsx
function UsersList() {
  const { data, refetch, isFetching } = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers
  });
  
  return (
    <div>
      <button 
        onClick={() => refetch()}
        disabled={isFetching}
      >
        {isFetching ? 'Refreshing...' : 'Refresh'}
      </button>
      <UserList users={data} />
    </div>
  );
}
```

#### Auto Refetch Options

```tsx
useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers,
  
  // إعادة تحميل عند تركيز النافذة
  refetchOnWindowFocus: true,
  
  // إعادة تحميل عند reconnect
  refetchOnReconnect: true,
  
  // إعادة تحميل عند mount
  refetchOnMount: true,
  
  // إعادة تحميل دوري
  refetchInterval: 60000, // كل دقيقة
  
  // إعادة تحميل دوري فقط عندما تكون النافذة مركزة
  refetchIntervalInBackground: false,
})
```

#### Conditional Refetch

```tsx
function UsersList({ enabled }: { enabled: boolean }) {
  const { data } = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers,
    enabled: enabled, // تشغيل/إيقاف الquery
  });
  
  // ...
}
```

### 10. Caching

#### كيف يعمل Caching؟

```tsx
// الطلب الأول - يجلب من الخادم
useQuery({ queryKey: ['users'], queryFn: fetchUsers })
// ↳ Loading... → Data fetched → Cached

// الطلب الثاني - يأخذ من الـ cache
useQuery({ queryKey: ['users'], queryFn: fetchUsers })
// ↳ Instant data from cache
```

#### Cache Time Options

```tsx
useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers,
  
  // المدة التي تبقى البيانات في الـ cache
  gcTime: 1000 * 60 * 5, // 5 دقائق (افتراضي)
  
  // المدة التي تعتبر البيانات fresh
  staleTime: 1000 * 60 * 2, // دقيقتين
})
```

#### Cache Behavior

```tsx
// staleTime: 0 (افتراضي)
// - البيانات stale فوراً
// - سيتم refetch عند mount أو window focus

// staleTime: Infinity
// - البيانات دائماً fresh
// - لن يتم refetch تلقائياً

// gcTime: 0
// - البيانات تُحذف فوراً من الـ cache
```

### 11. Stale Data

#### ما هو Stale Data؟

Stale data هي بيانات محفوظة في الـ cache لكن قد تكون قديمة.

```tsx
useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers,
  staleTime: 1000 * 60, // دقيقة واحدة
})

// خلال الدقيقة: البيانات fresh، لا refetch
// بعد الدقيقة: البيانات stale، سيتم refetch
```

#### Stale Time Strategies

```tsx
// 1. استخدم staleTime طويل للبيانات نادرة التغيير
useQuery({
  queryKey: ['countries'],
  queryFn: fetchCountries,
  staleTime: 1000 * 60 * 60, // ساعة
})

// 2. استخدم staleTime قصير للبيانات المتغيرة
useQuery({
  queryKey: ['orders'],
  queryFn: fetchOrders,
  staleTime: 0, // فوراً
})

// 3. استخدم staleTime متوسط للبيانات المعتادة
useQuery({
  queryKey: ['products'],
  queryFn: fetchProducts,
  staleTime: 1000 * 60 * 5, // 5 دقائق
})
```

#### Mark Data as Stale

```tsx
const queryClient = useQueryClient();

// جعل بيانات معينة stale
queryClient.invalidateQueries({ queryKey: ['users'] });

// جعل كل البيانات stale
queryClient.invalidateQueries();
```

### 12. useMutation

#### ما هو useMutation؟

useMutation يستخدم للعمليات التي تغير البيانات (POST, PUT, DELETE).

```tsx
import { useMutation } from '@tanstack/react-query';

const mutation = useMutation({
  mutationFn: (data) => createPost(data),
  onSuccess: () => {
    // عند النجاح
  },
  onError: (error) => {
    // عند الخطأ
  },
});
```

#### مثال POST

```tsx
function CreateUserForm() {
  const mutation = useMutation({
    mutationFn: (userData: UserData) => 
      axios.post('/api/users', userData),
    onSuccess: () => {
      alert('User created successfully!');
    },
    onError: (error) => {
      alert(`Error: ${error.message}`);
    },
  });
  
  const handleSubmit = (e: FormEvent) => {
    e.preventDefault();
    const formData = new FormData(e.target as HTMLFormElement);
    const userData = Object.fromEntries(formData);
    
    mutation.mutate(userData);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input name="name" placeholder="Name" />
      <input name="email" placeholder="Email" />
      <button type="submit" disabled={mutation.isPending}>
        {mutation.isPending ? 'Creating...' : 'Create User'}
      </button>
    </form>
  );
}
```

### 13. POST Request

#### إنشاء مورد جديد

```tsx
const createUserMutation = useMutation({
  mutationFn: (userData: UserData) => 
    axios.post('/api/users', userData),
  onSuccess: (data) => {
    console.log('User created:', data);
  },
});
```

#### مع TypeScript

```tsx
interface CreateUserInput {
  name: string;
  email: string;
  role: 'admin' | 'user';
}

interface User {
  id: number;
  name: string;
  email: string;
  role: string;
}

const createUserMutation = useMutation<User, Error, CreateUserInput>({
  mutationFn: (userData) => 
    axios.post<User>('/api/users', userData).then(res => res.data),
  onSuccess: (user) => {
    console.log('Created user:', user);
  },
});
```

### 14. PUT Request

#### تحديث مورد موجود

```tsx
const updateUserMutation = useMutation({
  mutationFn: ({ id, data }: { id: number; data: Partial<UserData> }) =>
    axios.put(`/api/users/${id}`, data),
  onSuccess: (data, variables) => {
    console.log(`User ${variables.id} updated:`, data);
  },
});

// الاستخدام
updateUserMutation.mutate({
  id: 1,
  data: { name: 'New Name' }
});
```

### 15. DELETE Request

#### حذف مورد

```tsx
const deleteUserMutation = useMutation({
  mutationFn: (id: number) =>
    axios.delete(`/api/users/${id}`),
  onSuccess: (data, id) => {
    console.log(`User ${id} deleted`);
  },
});

// الاستخدام
deleteUserMutation.mutate(1);
```

### 16. invalidateQueries

#### ما هو invalidateQueries؟

يجعل البيانات stale ويعيد تحميلها.

```tsx
const queryClient = useQueryClient();

// بعد mutation، قم بتحديث البيانات
const createUserMutation = useMutation({
  mutationFn: (userData) => axios.post('/api/users', userData),
  onSuccess: () => {
    // جعل users query stale وإعادة تحميله
    queryClient.invalidateQueries({ queryKey: ['users'] });
  },
});
```

#### أنواع Invalidate

```tsx
// 1. invalidate query محدد
queryClient.invalidateQueries({ queryKey: ['users'] });

// 2. invalidate queries matching pattern
queryClient.invalidateQueries({ queryKey: ['users'] }); // يطابق ['users', 1], ['users', 2], etc.

// 3. invalidate كل الـ queries
queryClient.invalidateQueries();

// 4. invalidate مع filters
queryClient.invalidateQueries({
  queryKey: ['users'],
  refetchType: 'active', // فقط الـ active queries
});

// 5. invalidate without refetch
queryClient.invalidateQueries({
  queryKey: ['users'],
  refetchType: 'none',
});
```

#### مثال عملي

```tsx
function CreateUserForm() {
  const queryClient = useQueryClient();
  
  const createUserMutation = useMutation({
    mutationFn: (userData) => axios.post('/api/users', userData),
    onSuccess: () => {
      // تحديث قائمة المستخدمين
      queryClient.invalidateQueries({ queryKey: ['users'] });
      
      // إظهار رسالة نجاح
      toast.success('User created successfully!');
    },
  });
  
  // ...
}
```

### 17. Optimistic Updates (مبسط)

#### ما هو Optimistic Update؟

تحديث الواجهة فوراً قبل تأكيد الخادم، للتجربة الأسرع.

```tsx
function TodoList() {
  const queryClient = useQueryClient();
  
  const addTodoMutation = useMutation({
    mutationFn: (text: string) => axios.post('/api/todos', { text }),
    
    // قبل الطلب - تحديث مبدئي
    onMutate: async (newTodo) => {
      // Cancel أي refetch قيد التنفيذ
      await queryClient.cancelQueries({ queryKey: ['todos'] });
      
      // حفظ البيانات القديمة
      const previousTodos = queryClient.getQueryData(['todos']);
      
      // تحديث optimistically
      queryClient.setQueryData(['todos'], (old: any[]) => [
        ...old,
        { id: Date.now(), text: newTodo, completed: false }
      ]);
      
      return { previousTodos };
    },
    
    // عند الخطأ - تراجع
    onError: (err, newTodo, context) => {
      queryClient.setQueryData(['todos'], context?.previousTodos);
    },
    
    // دائماً - refetch للتأكد
    onSettled: () => {
      queryClient.invalidateQueries({ queryKey: ['todos'] });
    },
  });
  
  const handleAdd = (text: string) => {
    addTodoMutation.mutate(text);
  };
  
  // ...
}
```

#### Optimistic Delete

```tsx
const deleteTodoMutation = useMutation({
  mutationFn: (id: number) => axios.delete(`/api/todos/${id}`),
  
  onMutate: async (id) => {
    await queryClient.cancelQueries({ queryKey: ['todos'] });
    const previousTodos = queryClient.getQueryData(['todos']);
    
    queryClient.setQueryData(['todos'], (old: any[]) =>
      old.filter(todo => todo.id !== id)
    );
    
    return { previousTodos };
  },
  
  onError: (err, id, context) => {
    queryClient.setQueryData(['todos'], context?.previousTodos);
  },
  
  onSettled: () => {
    queryClient.invalidateQueries({ queryKey: ['todos'] });
  },
});
```

---

## 📚 PART 1 Summary

### النقاط الرئيسية

1. **Server State vs Client State**:
   - Server State: من الخادم، يحتاج sync و caching
   - Client State: محلي، UI state، لا يحتاج sync

2. **React Query مميزات**:
   - Caching تلقائي
   - Deduplication
   - Auto-refetch
   - Error handling
   - Optimistic updates

3. **useQuery**: لجلب البيانات (GET)
4. **useMutation**: لتغيير البيانات (POST, PUT, DELETE)
5. **invalidateQueries**: لتحديث البيانات بعد mutations
6. **Optimistic Updates**: لتجربة أسرع

---

## 🏗️ المشروع العملي: Admin Dashboard

### Project Overview

Admin Dashboard production-ready يحتوي على:
- نظام Authentication كامل
- إدارة المستخدمين (CRUD)
- إدارة المنتجات (CRUD)
- إدارة الطلبات
- Dashboard مع إحصائيات

### لماذا React Query + Zustand؟

```tsx
// ❌ خاطئ - استخدام Zustand لكل شيء
const useUsersStore = create((set) => ({
  users: [],
  isLoading: false,
  error: null,
  fetchUsers: async () => {
    set({ isLoading: true });
    try {
      const response = await fetch('/api/users');
      const data = await response.json();
      set({ users: data, isLoading: false });
    } catch (error) {
      set({ error, isLoading: false });
    }
  },
}));

// المشاكل:
// - لا يوجد caching
// - لا يوجد deduplication
// - لا يوجد auto-refetch
// - لا يوجد optimistic updates
// - كود تكراري
```

```tsx
// ✅ صحيح - React Query للـ Server State
const { data: users, isLoading, error } = useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers,
});

// ✅ Zustand للـ Client State فقط
const useUIStore = create((set) => ({
  sidebarOpen: true,
  toggleSidebar: () => set((state) => ({ sidebarOpen: !state.sidebarOpen })),
  selectedTab: 'dashboard',
  setSelectedTab: (tab: string) => set({ selectedTab: tab }),
}));

// الفوائد:
// - Caching تلقائي
// - Deduplication
// - Auto-refetch
// - Optimistic updates
// - كود بسيط ونظيف
```

### Architecture

```
┌─────────────────────────────────────┐
│         React Components            │
│  (Dashboard, Users, Products, etc.) │
└──────────────┬──────────────────────┘
               │
               ├──────────────────────┬─────────────────┐
               │                      │                 │
               ▼                      ▼                 ▼
    ┌──────────────────┐    ┌──────────────┐  ┌──────────────┐
    │   React Query    │    │   Zustand    │  │   React Router│
    │  (Server State)  │    │(Client State)│  │   (Routing)   │
    └────────┬─────────┘    └──────────────┘  └──────────────┘
             │
             ▼
    ┌──────────────────┐
    │     API Layer    │
    │  (Axios/Fetch)   │
    └────────┬─────────┘
             │
             ▼
    ┌──────────────────┐
    │   Backend API    │
    └──────────────────┘
```

### Setup

```bash
# Install dependencies
npm install @tanstack/react-query zustand react-router-dom axios
npm install -D @types/react-router-dom
```

### QueryClient Setup

```tsx
// lib/queryClient.ts
import { QueryClient } from '@tanstack/react-query';

export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 1000 * 60 * 5, // 5 دقائق
      gcTime: 1000 * 60 * 10,   // 10 دقائق
      retry: 1,
      refetchOnWindowFocus: false,
    },
  },
});
```

### API Client

```tsx
// lib/api.ts
import axios from 'axios';

const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL || '/api',
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor - إضافة token
api.interceptors.request.use((config) => {
  const token = localStorage.getItem('access_token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Response interceptor - refresh token
api.interceptors.response.use(
  (response) => response,
  async (error) => {
    const originalRequest = error.config;
    
    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;
      
      try {
        const refreshToken = localStorage.getItem('refresh_token');
        const response = await axios.post('/api/auth/refresh', {
          refresh_token: refreshToken,
        });
        
        const { access_token } = response.data;
        localStorage.setItem('access_token', access_token);
        
        originalRequest.headers.Authorization = `Bearer ${access_token}`;
        return api(originalRequest);
      } catch (refreshError) {
        localStorage.removeItem('access_token');
        localStorage.removeItem('refresh_token');
        window.location.href = '/login';
      }
    }
    
    return Promise.reject(error);
  }
);

export default api;
```

### Auth Store (Zustand - Client State)

```tsx
// stores/authStore.ts
import { create } from 'zustand';

interface User {
  id: number;
  name: string;
  email: string;
  role: 'admin' | 'user';
}

interface AuthStore {
  user: User | null;
  isAuthenticated: boolean;
  login: (user: User) => void;
  logout: () => void;
}

export const useAuthStore = create<AuthStore>((set) => ({
  user: null,
  isAuthenticated: false,
  login: (user) => set({ user, isAuthenticated: true }),
  logout: () => set({ user: null, isAuthenticated: false }),
}));
```

### UI Store (Zustand - Client State)

```tsx
// stores/uiStore.ts
import { create } from 'zustand';

interface UIStore {
  sidebarOpen: boolean;
  selectedTab: string;
  toggleSidebar: () => void;
  setSelectedTab: (tab: string) => void;
}

export const useUIStore = create<UIStore>((set) => ({
  sidebarOpen: true,
  selectedTab: 'dashboard',
  toggleSidebar: () => set((state) => ({ sidebarOpen: !state.sidebarOpen })),
  setSelectedTab: (tab) => set({ selectedTab: tab }),
}));
```

### API Functions

```tsx
// lib/api/users.ts
import api from '../api';

export interface User {
  id: number;
  name: string;
  email: string;
  role: 'admin' | 'user';
  createdAt: string;
}

export const fetchUsers = async (): Promise<User[]> => {
  const { data } = await api.get('/users');
  return data;
};

export const fetchUser = async (id: number): Promise<User> => {
  const { data } = await api.get(`/users/${id}`);
  return data;
};

export const createUser = async (userData: Omit<User, 'id' | 'createdAt'>): Promise<User> => {
  const { data } = await api.post('/users', userData);
  return data;
};

export const updateUser = async (id: number, userData: Partial<User>): Promise<User> => {
  const { data } = await api.put(`/users/${id}`, userData);
  return data;
};

export const deleteUser = async (id: number): Promise<void> => {
  await api.delete(`/users/${id}`);
};
```

```tsx
// lib/api/products.ts
import api from '../api';

export interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
  stock: number;
  description: string;
  createdAt: string;
}

export const fetchProducts = async (): Promise<Product[]> => {
  const { data } = await api.get('/products');
  return data;
};

export const createProduct = async (productData: Omit<Product, 'id' | 'createdAt'>): Promise<Product> => {
  const { data } = await api.post('/products', productData);
  return data;
};

export const updateProduct = async (id: number, productData: Partial<Product>): Promise<Product> => {
  const { data } = await api.put(`/products/${id}`, productData);
  return data;
};

export const deleteProduct = async (id: number): Promise<void> => {
  await api.delete(`/products/${id}`);
};
```

```tsx
// lib/api/orders.ts
import api from '../api';

export interface Order {
  id: number;
  userId: number;
  status: 'pending' | 'processing' | 'shipped' | 'delivered' | 'cancelled';
  total: number;
  items: OrderItem[];
  createdAt: string;
}

export interface OrderItem {
  productId: number;
  quantity: number;
  price: number;
}

export const fetchOrders = async (): Promise<Order[]> => {
  const { data } = await api.get('/orders');
  return data;
};

export const fetchOrder = async (id: number): Promise<Order> => {
  const { data } = await api.get(`/orders/${id}`);
  return data;
};

export const updateOrderStatus = async (id: number, status: Order['status']): Promise<Order> => {
  const { data } = await api.patch(`/orders/${id}/status`, { status });
  return data;
};
```

### Query Keys

```tsx
// lib/queryKeys.ts
export const QUERY_KEYS = {
  // Auth
  AUTH: 'auth',
  
  // Users
  USERS: 'users',
  USER: (id: number) => ['users', id],
  
  // Products
  PRODUCTS: 'products',
  PRODUCT: (id: number) => ['products', id],
  
  // Orders
  ORDERS: 'orders',
  ORDER: (id: number) => ['orders', id],
  
  // Dashboard
  STATS: 'stats',
} as const;
```

### Login Page

```tsx
// pages/Login.tsx
import { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import { useMutation } from '@tanstack/react-query';
import { useAuthStore } from '../stores/authStore';
import api from '../lib/api';

function Login() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const navigate = useNavigate();
  const login = useAuthStore((state) => state.login);
  
  const loginMutation = useMutation({
    mutationFn: async (credentials: { email: string; password: string }) => {
      const { data } = await api.post('/auth/login', credentials);
      return data;
    },
    onSuccess: (data) => {
      localStorage.setItem('access_token', data.access_token);
      localStorage.setItem('refresh_token', data.refresh_token);
      login(data.user);
      navigate('/dashboard');
    },
  });
  
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    loginMutation.mutate({ email, password });
  };
  
  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-100">
      <div className="bg-white p-8 rounded-lg shadow-md w-96">
        <h1 className="text-2xl font-bold mb-6 text-center">Admin Login</h1>
        
        <form onSubmit={handleSubmit} className="space-y-4">
          <div>
            <label className="block mb-2">Email</label>
            <input
              type="email"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              className="w-full p-2 border rounded"
              required
            />
          </div>
          
          <div>
            <label className="block mb-2">Password</label>
            <input
              type="password"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
              className="w-full p-2 border rounded"
              required
            />
          </div>
          
          {loginMutation.error && (
            <div className="text-red-500 text-sm">
              {loginMutation.error.message}
            </div>
          )}
          
          <button
            type="submit"
            disabled={loginMutation.isPending}
            className="w-full bg-blue-500 text-white py-2 rounded hover:bg-blue-600 disabled:bg-gray-400"
          >
            {loginMutation.isPending ? 'Logging in...' : 'Login'}
          </button>
        </form>
      </div>
    </div>
  );
}

export default Login;
```

### Protected Route

```tsx
// components/ProtectedRoute.tsx
import { Navigate } from 'react-router-dom';
import { useAuthStore } from '../stores/authStore';

function ProtectedRoute({ children }: { children: React.ReactNode }) {
  const isAuthenticated = useAuthStore((state) => state.isAuthenticated);
  
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }
  
  return <>{children}</>;
}

export default ProtectedRoute;
```

### Layout Component

```tsx
// components/Layout.tsx
import { Outlet } from 'react-router-dom';
import { useUIStore } from '../stores/uiStore';
import { useAuthStore } from '../stores/authStore';

function Layout() {
  const sidebarOpen = useUIStore((state) => state.sidebarOpen);
  const toggleSidebar = useUIStore((state) => state.toggleSidebar);
  const selectedTab = useUIStore((state) => state.selectedTab);
  const setSelectedTab = useUIStore((state) => state.setSelectedTab);
  const user = useAuthStore((state) => state.user);
  const logout = useAuthStore((state) => state.logout);
  
  const handleLogout = () => {
    localStorage.removeItem('access_token');
    localStorage.removeItem('refresh_token');
    logout();
    window.location.href = '/login';
  };
  
  return (
    <div className="flex min-h-screen bg-gray-100">
      {/* Sidebar */}
      <aside className={`${sidebarOpen ? 'w-64' : 'w-0'} transition-all duration-300 bg-gray-800 text-white`}>
        <div className="p-4">
          <h2 className="text-xl font-bold mb-6">Admin Panel</h2>
          
          <nav className="space-y-2">
            <button
              onClick={() => setSelectedTab('dashboard')}
              className={`w-full text-left p-2 rounded ${selectedTab === 'dashboard' ? 'bg-gray-700' : 'hover:bg-gray-700'}`}
            >
              Dashboard
            </button>
            
            <button
              onClick={() => setSelectedTab('users')}
              className={`w-full text-left p-2 rounded ${selectedTab === 'users' ? 'bg-gray-700' : 'hover:bg-gray-700'}`}
            >
              Users
            </button>
            
            <button
              onClick={() => setSelectedTab('products')}
              className={`w-full text-left p-2 rounded ${selectedTab === 'products' ? 'bg-gray-700' : 'hover:bg-gray-700'}`}
            >
              Products
            </button>
            
            <button
              onClick={() => setSelectedTab('orders')}
              className={`w-full text-left p-2 rounded ${selectedTab === 'orders' ? 'bg-gray-700' : 'hover:bg-gray-700'}`}
            >
              Orders
            </button>
          </nav>
        </div>
      </aside>
      
      {/* Main Content */}
      <div className="flex-1 flex flex-col">
        {/* Header */}
        <header className="bg-white shadow p-4 flex items-center justify-between">
          <button onClick={toggleSidebar} className="p-2 hover:bg-gray-100 rounded">
            ☰
          </button>
          
          <div className="flex items-center gap-4">
            <span>{user?.name}</span>
            <span className="text-sm text-gray-500">{user?.role}</span>
            <button
              onClick={handleLogout}
              className="px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600"
            >
              Logout
            </button>
          </div>
        </header>
        
        {/* Content */}
        <main className="flex-1 p-6">
          <Outlet />
        </main>
      </div>
    </div>
  );
}

export default Layout;
```

### Dashboard Page

```tsx
// pages/Dashboard.tsx
import { useQuery } from '@tanstack/react-query';
import { QUERY_KEYS } from '../lib/queryKeys';
import { fetchUsers, fetchProducts, fetchOrders } from '../lib/api';

function Dashboard() {
  const { data: users, isLoading: usersLoading } = useQuery({
    queryKey: [QUERY_KEYS.USERS],
    queryFn: fetchUsers,
  });
  
  const { data: products, isLoading: productsLoading } = useQuery({
    queryKey: [QUERY_KEYS.PRODUCTS],
    queryFn: fetchProducts,
  });
  
  const { data: orders, isLoading: ordersLoading } = useQuery({
    queryKey: [QUERY_KEYS.ORDERS],
    queryFn: fetchOrders,
  });
  
  const stats = [
    {
      title: 'Total Users',
      value: users?.length || 0,
      loading: usersLoading,
      color: 'bg-blue-500',
    },
    {
      title: 'Total Products',
      value: products?.length || 0,
      loading: productsLoading,
      color: 'bg-green-500',
    },
    {
      title: 'Total Orders',
      value: orders?.length || 0,
      loading: ordersLoading,
      color: 'bg-purple-500',
    },
    {
      title: 'Revenue',
      value: orders?.reduce((sum, order) => sum + order.total, 0) || 0,
      loading: ordersLoading,
      color: 'bg-yellow-500',
    },
  ];
  
  return (
    <div>
      <h1 className="text-3xl font-bold mb-6">Dashboard</h1>
      
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
        {stats.map((stat) => (
          <div key={stat.title} className={`${stat.color} text-white p-6 rounded-lg`}>
            <h3 className="text-lg font-semibold mb-2">{stat.title}</h3>
            <p className="text-3xl font-bold">
              {stat.loading ? '...' : stat.value}
            </p>
          </div>
        ))}
      </div>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <div className="bg-white p-6 rounded-lg shadow">
          <h2 className="text-xl font-bold mb-4">Recent Orders</h2>
          {ordersLoading ? (
            <div>Loading...</div>
          ) : (
            <div className="space-y-2">
              {orders?.slice(0, 5).map((order) => (
                <div key={order.id} className="flex justify-between p-2 border-b">
                  <span>Order #{order.id}</span>
                  <span className="font-bold">${order.total}</span>
                </div>
              ))}
            </div>
          )}
        </div>
        
        <div className="bg-white p-6 rounded-lg shadow">
          <h2 className="text-xl font-bold mb-4">Recent Users</h2>
          {usersLoading ? (
            <div>Loading...</div>
          ) : (
            <div className="space-y-2">
              {users?.slice(0, 5).map((user) => (
                <div key={user.id} className="flex justify-between p-2 border-b">
                  <span>{user.name}</span>
                  <span className="text-sm text-gray-500">{user.role}</span>
                </div>
              ))}
            </div>
          )}
        </div>
      </div>
    </div>
  );
}

export default Dashboard;
```

### App Component

```tsx
// App.tsx
import { QueryClientProvider } from '@tanstack/react-query';
import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom';
import { queryClient } from './lib/queryClient';
import Login from './pages/Login';
import Layout from './components/Layout';
import ProtectedRoute from './components/ProtectedRoute';
import Dashboard from './pages/Dashboard';
import Users from './pages/Users';
import Products from './pages/Products';
import Orders from './pages/Orders';

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <BrowserRouter>
        <Routes>
          <Route path="/login" element={<Login />} />
          
          <Route
            path="/"
            element={
              <ProtectedRoute>
                <Layout />
              </ProtectedRoute>
            }
          >
            <Route index element={<Navigate to="/dashboard" replace />} />
            <Route path="dashboard" element={<Dashboard />} />
            <Route path="users" element={<Users />} />
            <Route path="products" element={<Products />} />
            <Route path="orders" element={<Orders />} />
          </Route>
        </Routes>
      </BrowserRouter>
    </QueryClientProvider>
  );
}

export default App;
```

---

## 🎯 لماذا لا تستخدم Zustand لكل شيء؟

### المشاكل مع استخدام Zustand للـ Server State

```tsx
// ❌ سيء - Zustand للـ Server State
const useUsersStore = create((set) => ({
  users: [],
  isLoading: false,
  error: null,
  
  fetchUsers: async () => {
    set({ isLoading: true, error: null });
    try {
      const response = await fetch('/api/users');
      const data = await response.json();
      set({ users: data, isLoading: false });
    } catch (error) {
      set({ error, isLoading: false });
    }
  },
}));

// المشاكل:
// 1. ❌ لا يوجد caching - كل استدعاء يجلب بيانات جديدة
// 2. ❌ لا يوجد deduplication - نفس الطلب يُرسل عدة مرات
// 3. ❌ لا يوجد auto-refetch - البيانات قد تصبح stale
// 4. ❌ لا يوجد optimistic updates - التجربة بطيئة
// 5. ❌ لا يوجد loading/error states مدمجة
// 6. ❌ كود تكراري لكل API call
// 7. ❌ صعب الإدارة مع الـ mutations
```

### الحل الصحيح: React Query للـ Server State

```tsx
// ✅ جيد - React Query للـ Server State
const { data: users, isLoading, error } = useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers,
});

// الفوائد:
// 1. ✅ Caching تلقائي - البيانات محفوظة ومشاركة
// 2. ✅ Deduplication - نفس الطلب يُرسل مرة واحدة
// 3. ✅ Auto-refetch - تحديث تلقائي عند الحاجة
// 4. ✅ Optimistic updates - تجربة سريعة
// 5. ✅ Loading/error states مدمجة
// 6. ✅ كود بسيط ونظيف
// 7. ✅ إدارة mutations سهلة
```

### متى تستخدم Zustand؟

```tsx
// ✅ Zustand للـ Client State فقط
const useUIStore = create((set) => ({
  sidebarOpen: true,
  selectedTab: 'dashboard',
  theme: 'light',
  
  toggleSidebar: () => set((state) => ({ sidebarOpen: !state.sidebarOpen })),
  setSelectedTab: (tab) => set({ selectedTab: tab }),
  setTheme: (theme) => set({ theme }),
}));

// استخدم Zustand لـ:
// - UI state (modals, dropdowns, tabs)
// - User preferences (theme, language)
// - Form state (temporarily)
// - Navigation state
// - Client-side filtering/sorting
```

### مقارنة سريعة

| الميزة | Zustand | React Query |
|--------|---------|-------------|
| **الغرض** | Client State | Server State |
| **Caching** | يدوي (localStorage) | تلقائي |
| **Deduplication** | لا يوجد | مدمج |
| **Auto-refetch** | يدوي | تلقائي |
| **Optimistic Updates** | يدوي | مدمج |
| **Loading States** | يدوي | مدمج |
| **Error Handling** | يدوي | مدمج |
| **Background Refetch** | لا يوجد | مدمج |
| **الحجم** | ~1KB | ~13KB |

### الاستراتيجية المثلى

```tsx
// ✅ الاستراتيجية المثلى:
// - React Query للـ Server State (API data)
// - Zustand للـ Client State (UI, preferences)
// - React Router للـ Navigation
// - React Hook Form للـ Forms

// Server State (React Query)
const { data: users } = useQuery({ queryKey: ['users'], queryFn: fetchUsers });
const { data: products } = useQuery({ queryKey: ['products'], queryFn: fetchProducts });

// Client State (Zustand)
const sidebarOpen = useUIStore((state) => state.sidebarOpen);
const theme = useUIStore((state) => state.theme);

// Navigation (React Router)
const navigate = useNavigate();

// Forms (React Hook Form)
const { register, handleSubmit } = useForm();
```

## PART 2 — Authentication

### 1. Login Flow

#### ما هو Login Flow؟

Login Flow هو عملية مصادقة المستخدم للوصول إلى النظام.

#### الخطوات الأساسية

```
1. المستخدم يدخل credentials (email, password)
   ↓
2. إرسال البيانات إلى الخادم
   ↓
3. الخادم يتحقق من البيانات
   ↓
4. إذا صحيح: إرجاع access token
   ↓
5. حفظ token في المتصفح
   ↓
6. تحديث حالة التطبيق (authenticated)
   ↓
7. توجيه المستخدم للصفحة المحمية
```

#### مثال Login Form

```tsx
function LoginForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const loginMutation = useMutation({
    mutationFn: (credentials: { email: string; password: string }) =>
      axios.post('/api/auth/login', credentials),
    onSuccess: (data) => {
      // حفظ token
      localStorage.setItem('access_token', data.access_token);
      
      // تحديث حالة auth
      useAuthStore.getState().login(data.user);
      
      // توجيه للصفحة الرئيسية
      router.push('/dashboard');
    },
  });
  
  const handleSubmit = (e: FormEvent) => {
    e.preventDefault();
    loginMutation.mutate({ email, password });
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
      />
      <input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        placeholder="Password"
      />
      <button type="submit" disabled={loginMutation.isPending}>
        {loginMutation.isPending ? 'Logging in...' : 'Login'}
      </button>
    </form>
  );
}
```

### 2. Access Token

#### ما هو Access Token؟

Access Token هو string يستخدم لإثبات هوية المستخدم في كل طلب.

#### خصائص Access Token

- 🔑 فريد لكل مستخدم
- ⏱️ له صلاحية محددة (عادة 15-30 دقيقة)
- 🔒 مشفر (JWT)
- 📤 يُرسل في header كل طلب

#### مثال JWT Token

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
}
```

#### استخدام Access Token

```tsx
// إضافة token لكل طلب
const api = axios.create({
  baseURL: '/api',
});

api.interceptors.request.use((config) => {
  const token = localStorage.getItem('access_token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// مثال طلب محمي
async function fetchUserProfile() {
  const response = await api.get('/user/profile');
  return response.data;
}
```

### 3. Refresh Token (مبسط)

#### ما هو Refresh Token؟

Refresh Token يستخدم للحصول على access token جديد عند انتهاء الصلاحية.

#### لماذا نحتاجه؟

- Access قصير العمر (أمان)
- Refresh طويل العمر (راحة)
- تجنب تسجيل الدخول المتكرر

#### Flow مبسط

```
1. Access Token منتهي
   ↓
2. استخدام Refresh Token للحصول على Access جديد
   ↓
3. إذا Refresh صالح: إرجاع Access جديد
   ↓
4. إذا Refresh منتهي: تسجيل خروج وإعادة login
```

#### مثال Refresh Token

```tsx
api.interceptors.response.use(
  (response) => response,
  async (error) => {
    const originalRequest = error.config;
    
    // إذا الخطأ 401 ولم نحاول refresh
    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;
      
      try {
        const refreshToken = localStorage.getItem('refresh_token');
        const response = await axios.post('/api/auth/refresh', {
          refresh_token: refreshToken,
        });
        
        const { access_token } = response.data;
        localStorage.setItem('access_token', access_token);
        
        // إعادة المحاولة بالـ token الجديد
        originalRequest.headers.Authorization = `Bearer ${access_token}`;
        return api(originalRequest);
      } catch (refreshError) {
        // Refresh فشل - تسجيل خروج
        localStorage.removeItem('access_token');
        localStorage.removeItem('refresh_token');
        window.location.href = '/login';
      }
    }
    
    return Promise.reject(error);
  }
);
```

### 4. Authentication vs Authorization

#### Authentication (المصادقة)

**السؤال**: من أنت؟

**الطرق**:
- Email/Password
- OAuth (Google, Facebook)
- JWT Tokens
- Session Cookies

**الهدف**: التحقق من هوية المستخدم

```tsx
// Authentication: تحقق من الهوية
if (user.isAuthenticated) {
  // المستخدم معروف
}
```

#### Authorization (التفويض)

**السؤال**: ماذا يمكنك أن تفعل؟

**الطرق**:
- Roles (Admin, User, Guest)
- Permissions (read, write, delete)
- Access Control Lists

**الهدف**: تحديد الصلاحيات

```tsx
// Authorization: تحقق من الصلاحيات
if (user.role === 'admin') {
  // يمكنه حذف المستخدمين
}

if (user.permissions.includes('products.write')) {
  // يمكنه تعديل المنتجات
}
```

#### مثال عملي

```tsx
function DeleteUserButton({ userId }: { userId: number }) {
  const user = useAuthStore((state) => state.user);
  
  // Authentication: هل المستخدم مسجل؟
  if (!user) {
    return <Link to="/login">Login to delete</Link>;
  }
  
  // Authorization: هل لديه صلاحية الحذف؟
  if (user.role !== 'admin') {
    return <p>You don't have permission</p>;
  }
  
  // لديه الصلاحية - إظهار الزر
  return (
    <button onClick={() => deleteUser(userId)}>
      Delete User
    </button>
  );
}
```

### 5. Protected Routes

#### ما هي Protected Routes؟

Routes التي لا يمكن الوصول إليها إلا من المستخدمين المصادقين.

#### Protected Route Component

```tsx
function ProtectedRoute({ children }: { children: React.ReactNode }) {
  const isAuthenticated = useAuthStore((state) => state.isAuthenticated);
  const { isLoading } = useQuery({
    queryKey: ['auth'],
    queryFn: checkAuth,
  });
  
  if (isLoading) {
    return <div>Loading...</div>;
  }
  
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }
  
  return <>{children}</>;
}

// الاستخدام
function App() {
  return (
    <Routes>
      <Route path="/login" element={<Login />} />
      <Route
        path="/dashboard"
        element={
          <ProtectedRoute>
            <Dashboard />
          </ProtectedRoute>
        }
      />
    </Routes>
  );
}
```

#### Protected Route مع Roles

```tsx
interface ProtectedRouteProps {
  children: React.ReactNode;
  allowedRoles?: string[];
}

function ProtectedRoute({ children, allowedRoles }: ProtectedRouteProps) {
  const user = useAuthStore((state) => state.user);
  const isAuthenticated = useAuthStore((state) => state.isAuthenticated);
  
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }
  
  if (allowedRoles && !allowedRoles.includes(user?.role || '')) {
    return <Navigate to="/unauthorized" replace />;
  }
  
  return <>{children}</>;
}

// الاستخدام
<Route
  path="/admin"
  element={
    <ProtectedRoute allowedRoles={['admin']}>
      <AdminPanel />
    </ProtectedRoute>
  }
/>
```

### 6. User Roles

#### أنواع Roles الشائعة

```tsx
enum UserRole {
  GUEST = 'guest',
  USER = 'user',
  MODERATOR = 'moderator',
  ADMIN = 'admin',
  SUPER_ADMIN = 'super_admin',
}
```

#### Permissions per Role

```tsx
const ROLE_PERMISSIONS = {
  guest: ['products.read'],
  user: ['products.read', 'orders.create', 'orders.read'],
  moderator: ['products.read', 'products.write', 'orders.read', 'orders.update'],
  admin: ['*'], // كل الصلاحيات
};
```

#### Check Permissions Helper

```tsx
function hasPermission(user: User, permission: string): boolean {
  if (user.role === 'admin') return true;
  
  const permissions = ROLE_PERMISSIONS[user.role] || [];
  return permissions.includes(permission) || permissions.includes('*');
}

// الاستخدام
function CreateProductButton() {
  const user = useAuthStore((state) => state.user);
  
  if (!hasPermission(user, 'products.write')) {
    return null;
  }
  
  return <button>Create Product</button>;
}
```

### 7. Admin vs User

#### Admin Capabilities

```tsx
// Admin يمكنه:
- إدارة المستخدمين
- إدارة المنتجات
- مشاهدة كل الطلبات
- تعديل الإعدادات
- عرض التقارير
- حذف البيانات
```

#### User Capabilities

```tsx
// User يمكنه:
- مشاهدة المنتجات
- إنشاء طلبات
- مشاهدة طلباته فقط
- تعديل ملفه الشخصي
```

#### مثال Admin Panel

```tsx
function AdminPanel() {
  const user = useAuthStore((state) => state.user);
  
  if (user?.role !== 'admin') {
    return <Navigate to="/dashboard" replace />;
  }
  
  return (
    <div>
      <AdminNavigation />
      <Routes>
        <Route path="users" element={<UsersManagement />} />
        <Route path="products" element={<ProductsManagement />} />
        <Route path="orders" element={<OrdersManagement />} />
        <Route path="settings" element={<SystemSettings />} />
      </Routes>
    </div>
  );
}
```

### 8. Logout

#### Logout Flow

```
1. المستخدم يضغط Logout
   ↓
2. إرسال طلب logout للخادم (اختياري)
   ↓
3. حذف tokens من المتصفح
   ↓
4. مسح auth state
   ↓
5. توجيه لصفحة login
```

#### Logout Implementation

```tsx
function LogoutButton() {
  const logout = useAuthStore((state) => state.logout);
  const queryClient = useQueryClient();
  
  const handleLogout = () => {
    // 1. حذف tokens
    localStorage.removeItem('access_token');
    localStorage.removeItem('refresh_token');
    
    // 2. مسح auth state
    logout();
    
    // 3. مسح جميع queries
    queryClient.clear();
    
    // 4. توجيه لصفحة login
    window.location.href = '/login';
  };
  
  return (
    <button onClick={handleLogout}>
      Logout
    </button>
  );
}
```

#### Logout مع API Call

```tsx
const logoutMutation = useMutation({
  mutationFn: () => axios.post('/api/auth/logout'),
  onSuccess: () => {
    // مسح tokens
    localStorage.removeItem('access_token');
    localStorage.removeItem('refresh_token');
    
    // مسح state
    logout();
    
    // مسح queries
    queryClient.clear();
    
    // توجيه
    router.push('/login');
  },
});

function LogoutButton() {
  return (
    <button onClick={() => logoutMutation.mutate()}>
      Logout
    </button>
  );
}
```

---

## 📚 PART 2 Summary

### النقاط الرئيسية

1. **Login Flow**: إدخال credentials → verify → receive token → save → redirect
2. **Access Token**: لإثبات الهوية في كل طلب
3. **Refresh Token**: للحصول على access token جديد
4. **Authentication**: من أنت؟ (verify identity)
5. **Authorization**: ماذا يمكنك أن تفعل؟ (check permissions)
6. **Protected Routes**: حماية الصفحات من غير المصادقين
7. **User Roles**: تحديد الصلاحيات حسب الدور
8. **Logout**: مسح tokens و state وتوجيه لـ login

---

## PART 3 — Performance

### 1. Re-rendering

#### ما هو Re-rendering؟

Re-rendering هو عملية إعادة رسم المكون عند تغيير state أو props.

#### متذا يحدث Re-render؟

```tsx
// 1. عند تغيير state
function Counter() {
  const [count, setCount] = useState(0);
  
  // سيحدث re-render عند كل setCount
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}

// 2. عند تغيير props
function UserCard({ user }: { user: User }) {
  // سيحدث re-render عند تغيير user prop
  return <div>{user.name}</div>;
}

// 3. عند تغيير context
function ThemeConsumer() {
  const theme = useTheme();
  // سيحدث re-render عند تغيير theme context
  return <div className={theme}>Content</div>;
}

// 4. عند تغيير parent
function Parent() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <Child /> {/* سيحدث re-render أيضاً! */}
    </div>
  );
}
```

#### مشكلة Unnecessary Re-renders

```tsx
// ❌ سيء - Child يعيد render بدون سبب
function Parent() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>
        Count: {count}
      </button>
      <ExpensiveComponent /> {/* يعيد render مع كل تغيير في count! */}
    </div>
  );
}

function ExpensiveComponent() {
  // حسابات ثقيلة
  const result = heavyCalculation();
  return <div>{result}</div>;
}
```

### 2. React.memo

#### ما هو React.memo؟

Component عالي المستوى يتخطى re-render إذا لم تتغير props.

#### الاستخدام الأساسي

```tsx
// ✅ جيد - لن يعيد render إذا لم تتغير props
const ExpensiveComponent = React.memo(function ExpensiveComponent({ data }: { data: any }) {
  const result = heavyCalculation(data);
  return <div>{result}</div>;
});

function Parent() {
  const [count, setCount] = useState(0);
  const [data] = useState(someData);
  
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>
        Count: {count}
      </button>
      <ExpensiveComponent data={data} /> {/* لن يعيد render! */}
    </div>
  );
}
```

#### React.memo مع custom comparison

```tsx
const UserCard = React.memo(
  function UserCard({ user }: { user: User }) {
    return <div>{user.name}</div>;
  },
  (prevProps, nextProps) => {
    // custom comparison
    return prevProps.user.id === nextProps.user.id;
  }
);
```

#### متى تستخدم React.memo؟

```tsx
// ✅ استخدم عندما:
// - المكون مكلف جداً (heavy calculations)
// - يعيد render كثيراً بدون داعي
// - props نادراً ما تتغير

// ❌ لا تستخدم عندما:
// - المكون بسيط وسريع
// - props تتغير كثيراً
// - التطبيق صغير
```

### 3. useMemo

#### ما هو useMemo؟

Hook يحفظ نتيجة حساب معين ويعيدها فقط عند تغيير dependencies.

#### الاستخدام الأساسي

```tsx
function ProductList({ products, filter }: { products: Product[]; filter: string }) {
  // ❌ سيء - يعيد الحساب في كل render
  const filteredProducts = products.filter(p => 
    p.name.toLowerCase().includes(filter.toLowerCase())
  );
  
  // ✅ جيد - يحفظ النتيجة
  const filteredProducts = useMemo(() => 
    products.filter(p => 
      p.name.toLowerCase().includes(filter.toLowerCase())
    ),
    [products, filter]
  );
  
  return (
    <div>
      {filteredProducts.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}
```

#### أمثلة useMemo

```tsx
// 1. الحسابات الثقيلة
const sortedProducts = useMemo(() => {
  return [...products].sort((a, b) => a.price - b.price);
}, [products]);

// 2. تشكيل data معقدة
const chartData = useMemo(() => {
  return {
    labels: products.map(p => p.name),
    values: products.map(p => p.sales),
  };
}, [products]);

// 3. filtering
const activeUsers = useMemo(() => {
  return users.filter(u => u.isActive);
}, [users]);
```

#### متى تستخدم useMemo؟

```tsx
// ✅ استخدم عندما:
// - الحسابات مكلفة
// - النتيجة تُستخدم كـ prop لمكون memoized
// - تعتمد على dependencies نادرة التغيير

// ❌ لا تستخدم عندما:
// - الحسابات بسيطة وسريعة
// - تحاول optimization قبل الأوان
// - تزيد التعقيد دون فائدة
```

### 4. useCallback

#### ما هو useCallback؟

Hook يحفظ function definition ويعيدها فقط عند تغيير dependencies.

#### الاستخدام الأساسي

```tsx
function Parent() {
  const [count, setCount] = useState(0);
  
  // ❌ سيء - function جديدة في كل render
  const handleClick = () => {
    console.log('Clicked');
  };
  
  // ✅ جيد - نفس function ما لم تتغير dependencies
  const handleClick = useCallback(() => {
    console.log('Clicked');
  }, []);
  
  return <Child onClick={handleClick} />;
}

const Child = React.memo(function Child({ onClick }: { onClick: () => void }) {
  console.log('Child rendered');
  return <button onClick={onClick}>Click me</button>;
});
```

#### أمثلة useCallback

```tsx
// 1. Event handlers
const handleSubmit = useCallback((data: FormData) => {
  submitForm(data);
}, []);

// 2. Callbacks لـ child components
const onUserSelect = useCallback((userId: number) => {
  setSelectedUser(userId);
}, []);

// 3. في useMutation callbacks
const mutation = useMutation({
  mutationFn: createUser,
  onSuccess: useCallback(() => {
    queryClient.invalidateQueries({ queryKey: ['users'] });
  }, [queryClient]),
});
```

#### متى تستخدم useCallback؟

```tsx
// ✅ استخدم عندما:
// - تمرر function كـ prop لمكون memoized
// - function تُستخدم كـ dependency في useEffect/useMemo
// - تريد منع re-renders غير ضرورية

// ❌ لا تستخدم عندما:
// - المكون ليس memoized
// - تمرر function لـ native DOM elements
// - تزيد التعقيد دون فائدة
```

### 5. Lazy Loading

#### ما هو Lazy Loading؟

تحميل components أو resources عند الحاجة فقط، وليس فوراً.

#### الفوائد

- ⚡ تحميل أولي أسرع
- 📦 bundle size أصغر
- 💾 استهلاك memory أقل

#### React.lazy

```tsx
import { lazy, Suspense } from 'react';

// ❌ سيء - يحمل كل شيء فوراً
import Dashboard from './Dashboard';
import Settings from './Settings';
import Profile from './Profile';

// ✅ جيد - يحمل عند الحاجة
const Dashboard = lazy(() => import('./Dashboard'));
const Settings = lazy(() => import('./Settings'));
const Profile = lazy(() => import('./Profile'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/settings" element={<Settings />} />
        <Route path="/profile" element={<Profile />} />
      </Routes>
    </Suspense>
  );
}
```

### 6. React.lazy

#### الاستخدام المتقدم

```tsx
// Lazy مع named exports
const Dashboard = lazy(() => 
  import('./Dashboard').then(module => ({ default: module.Dashboard }))
);

// Lazy مع error boundary
function App() {
  return (
    <ErrorBoundary>
      <Suspense fallback={<LoadingSpinner />}>
        <Routes>
          <Route path="/dashboard" element={<Dashboard />} />
        </Routes>
      </Suspense>
    </ErrorBoundary>
  );
}
```

#### Lazy لـ Modals

```tsx
const UserModal = lazy(() => import('./UserModal'));

function UsersList() {
  const [isModalOpen, setIsModalOpen] = useState(false);
  
  return (
    <div>
      <button onClick={() => setIsModalOpen(true)}>
        Add User
      </button>
      
      {isModalOpen && (
        <Suspense fallback={<div>Loading modal...</div>}>
          <UserModal onClose={() => setIsModalOpen(false)} />
        </Suspense>
      )}
    </div>
  );
}
```

### 7. Suspense

#### ما هو Suspense؟

Component يعرض fallback content أثناء تحميل components lazy.

#### الاستخدام الأساسي

```tsx
import { Suspense } from 'react';

function App() {
  return (
    <div>
      <Header />
      
      <Suspense fallback={<PageLoader />}>
        <LazyComponent />
      </Suspense>
      
      <Footer />
    </div>
  );
}
```

#### Suspense متعدد

```tsx
function App() {
  return (
    <div>
      <Suspense fallback={<div>Loading layout...</div>}>
        <Layout>
          <Suspense fallback={<div>Loading content...</div>}>
            <LazyContent />
          </Suspense>
        </Layout>
      </Suspense>
    </div>
  );
}
```

#### Custom Suspense Fallback

```tsx
function PageLoader() {
  return (
    <div className="flex items-center justify-center min-h-screen">
      <div className="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-500" />
      <p className="ml-4">Loading...</p>
    </div>
  );
}
```

### 8. Code Splitting

#### ما هو Code Splitting؟

تقسيم الـ bundle إلى عدة ملفات صغيرة يتم تحميلها عند الحاجة.

#### أنواع Code Splitting

```tsx
// 1. Route-based splitting
const Dashboard = lazy(() => import('./pages/Dashboard'));
const Settings = lazy(() => import('./pages/Settings'));

// 2. Component-based splitting
const HeavyChart = lazy(() => import('./components/HeavyChart'));

// 3. Feature-based splitting
const AdminPanel = lazy(() => import('./features/admin/AdminPanel'));
```

#### Code Splitting مع Vite/Webpack

```tsx
// Vite/Webpack يقومان بالـ code splitting تلقائياً مع lazy imports
const Dashboard = lazy(() => import('./pages/Dashboard'));
// سيُنشئ ملف منفصل: Dashboard.[hash].js
```

### 9. Bundle Size

#### تحليل Bundle Size

```bash
# مع Vite
npm run build
# سيُظهر حجم كل chunk

# مع Webpack
npm run build
# سيُنشئ report.html مع تفاصيل الحجم
```

#### تقليل Bundle Size

```tsx
// 1. Tree shaking
// ✅ استخدم imports محددة
import { Button } from 'my-ui-library';
// ❌ لا تستخدم
import * as UI from 'my-ui-library';

// 2. Dynamic imports للـ libraries الكبيرة
const Chart = lazy(() => import('chart.js/auto'));

// 3. استخدم alternatives أصغر
// ❌ moment.js (67KB)
// ✅ date-fns (tree-shakable)
// ✅ dayjs (2KB)

// 4. استخدم compression
// gzip, brotli في production
```

#### Bundle Analysis Tools

```bash
# Webpack Bundle Analyzer
npm install --save-dev @webpack-bundle-analyzer

# Vite Bundle Visualizer
npm install --save-dev rollup-plugin-visualizer
```

### 10. Avoid Unnecessary Renders

#### استراتيجيات لتجنب Re-renders

```tsx
// 1. State lifting
// ❌ سيء - state في parent
function Parent() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      <Child />
    </div>
  );
}

// ✅ جيد - state في component منفصل
function Parent() {
  return (
    <div>
      <Counter />
      <Child />
    </div>
  );
}

// 2. Component composition
// ❌ سيء - props كثيرة
function Parent() {
  const [data, setData] = useState(null);
  return <Child data={data} onChange={setData} />;
}

// ✅ جيد - render props أو children
function Parent() {
  const [data, setData] = useState(null);
  return (
    <DataProvider value={data} onChange={setData}>
      <Child />
    </DataProvider>
  );
}

// 3. Use keys بشكل صحيح
// ❌ سيء - index كـ key
{items.map((item, index) => (
  <Item key={index} data={item} />
))}

// ✅ جيد - unique ID كـ key
{items.map((item) => (
  <Item key={item.id} data={item} />
))}
```

### 11. React DevTools

#### تثبيت React DevTools

```bash
# Chrome/Edge
https://chrome.google.com/webstore/detail/react-developer-tools/

# Firefox
https://addons.mozilla.org/firefox/addon/react-devtools/
```

#### استخدام React DevTools

```
1. Components Tab
   - عرض شجرة المكونات
   - inspection لـ props و state
   - highlight المكونات في الصفحة

2. Profiler Tab
   - تسجيل performance
   - تحليل re-renders
   - قياس render times

3. Settings
   - تفعيل Profiler
   - ضبط update highlights
```

#### تحديد Re-renders

```tsx
// في React DevTools:
// 1. افتح Components tab
// 2. اختر "Highlight updates when components render"
// 3. المكونات التي تعيد render ستضيء باللون الأخضر
```

### 12. Profiler

#### ما هو Profiler؟

أداة لقياس أداء المكونات وتحديد المشاكل.

#### استخدام Profiler

```tsx
import { Profiler } from 'react';

function onRenderCallback(
  id: string,
  phase: 'mount' | 'update',
  actualDuration: number,
  baseDuration: number,
  startTime: number,
  commitTime: number
) {
  console.log({
    id,
    phase,
    actualDuration,    // الوقت الفعلي
    baseDuration,      // الوقت الأساسي بدون optimization
  });
}

function App() {
  return (
    <Profiler id="App" onRender={onRenderCallback}>
      <Dashboard />
    </Profiler>
  );
}
```

#### Profiler في React DevTools

```
1. افتح Profiler tab
2. اضغط "Start profiling"
3. تفاعل مع التطبيق
4. اضغط "Stop profiling"
5. راجع النتائج:
   - Ranked: المكونات الأبطأ
   - Timeline: timeline للـ renders
   - Flame graph: شجرة المكونات مع الأوقات
```

---

## 📚 PART 3 Summary

### النقاط الرئيسية

1. **Re-rendering**: يحدث عند تغيير state, props, context
2. **React.memo**: يتخطى re-renders إذا لم تتغير props
3. **useMemo**: يحفظ نتائج الحسابات الثقيلة
4. **useCallback**: يحفظ function definitions
5. **Lazy Loading**: تحميل عند الحاجة
6. **React.lazy**: lazy loading للـ components
7. **Suspense**: fallback أثناء التحميل
8. **Code Splitting**: تقسيم الـ bundle
9. **Bundle Size**: تحليل وتقليل الحجم
10. **React DevTools**: أدوات التطوير
11. **Profiler**: قياس الأداء

---

## 📝 Summary

### النقاط الرئيسية للجلسة

#### PART 1 — React Query
1. **Server State vs Client State**: Server State من الخادم يحتاج sync، Client State محلي
2. **React Query مميزات**: Caching, deduplication, auto-refetch, optimistic updates
3. **useQuery**: لجلب البيانات (GET requests)
4. **useMutation**: لتغيير البيانات (POST, PUT, DELETE)
5. **invalidateQueries**: لتحديث البيانات بعد mutations
6. **Optimistic Updates**: تحديث فوري قبل تأكيد الخادم

#### PART 2 — Authentication
1. **Login Flow**: credentials → verify → token → save → redirect
2. **Access Token**: لإثبات الهوية في كل طلب
3. **Refresh Token**: للحصول على access token جديد
4. **Authentication**: من أنت؟ (verify identity)
5. **Authorization**: ماذا يمكنك أن تفعل؟ (check permissions)
6. **Protected Routes**: حماية الصفحات
7. **User Roles**: تحديد الصلاحيات
8. **Logout**: مسح tokens و state

#### PART 3 — Performance
1. **Re-rendering**: يحدث عند تغيير state, props, context
2. **React.memo**: يتخطى re-renders
3. **useMemo**: يحفظ الحسابات الثقيلة
4. **useCallback**: يحفظ functions
5. **Lazy Loading**: تحميل عند الحاجة
6. **Code Splitting**: تقسيم الـ bundle
7. **Bundle Size**: تحليل وتقليل الحجم
8. **React DevTools & Profiler**: أدوات التطوير

---

## 🏗️ Architecture

### Architecture المثلى

```
┌─────────────────────────────────────────┐
│           UI Layer                      │
│  (Components, Pages, Layouts)           │
└──────────────┬──────────────────────────┘
               │
               ├──────────────┬──────────────┬──────────────┐
               │              │              │              │
               ▼              ▼              ▼              ▼
    ┌─────────────────┐ ┌──────────┐ ┌──────────┐ ┌──────────────┐
    │  React Query    │ │ Zustand  │ │ Router   │ │  React Hook  │
    │ (Server State)  │ │(Client   │ │(Routing) │ │     Form      │
    │                 │ │ State)   │ │          │ │              │
    └────────┬─────────┘ └──────────┘ └──────────┘ └──────────────┘
             │
             ▼
    ┌─────────────────┐
    │   API Layer     │
    │  (Axios/Fetch)  │
    └────────┬─────────┘
             │
             ▼
    ┌─────────────────┐
    │  Backend API    │
    │  (REST/GraphQL) │
    └─────────────────┘
```

### مسؤوليات كل طبقة

#### UI Layer (React Components)
```tsx
// المسؤوليات:
// - عرض البيانات
// - التعامل مع user interactions
// - إدارة layout و navigation
// - UI state محلي (useState)

// أمثلة:
- Dashboard component
- Users list component
- Product card component
- Forms و modals
```

#### React Query (Server State)
```tsx
// المسؤوليات:
// - جلب البيانات من API
// - Caching و deduplication
// - Auto-refetch
// - Error handling
// - Optimistic updates

// أمثلة:
- useQuery لـ GET requests
- useMutation لـ POST/PUT/DELETE
- invalidateQueries لتحديث البيانات
```

#### Zustand (Client State)
```tsx
// المسؤوليات:
// - UI state عامة
// - User preferences
// - Navigation state
// - Form state مؤقت

// أمثلة:
- Sidebar open/close
- Theme selection
- Selected tab
- Modal states
```

#### React Router (Routing)
```tsx
// المسؤوليات:
// - Navigation بين الصفحات
// - Protected routes
// - URL parameters
// - Browser history

// أمثلة:
- /dashboard
- /users/:id
- /products/new
- /orders/:id
```

#### API Layer (Axios)
```tsx
// المسؤوليات:
// - HTTP requests
// - Request/response interceptors
// - Token management
// - Error handling عام

// أمثلة:
- إضافة access token للـ headers
- Refresh token logic
- Global error handling
- Request/response logging
```

---

## ❓ 15 Questions

### أسئلة الفهم

1. **ما الفرق بين Server State و Client State؟**
   - Server State: من الخادم، يحتاج sync و caching، يُدار بـ React Query
   - Client State: محلي، UI state، يُدار بـ useState أو Zustand

2. **لماذا React Query أفضل من fetch يدوي؟**
   - Caching تلقائي
   - Deduplication للطلبات
   - Auto-refetch
   - Error handling مدمج
   - Optimistic updates
   - كود أقل

3. **ما هو queryKey وما فائدته؟**
   - معرف فريد لكل query
   - يُستخدم في caching و refetching
   - يُحدد متى البيانات stale

4. **متى تستخدم useQuery ومتى useMutation؟**
   - useQuery: لجلب البيانات (GET)
   - useMutation: لتغيير البيانات (POST, PUT, DELETE)

5. **ما هو invalidateQueries؟**
   - يجعل البيانات stale
   - يعيد تحميل البيانات من الخادم
   - يُستخدم بعد mutations

6. **ما هو Optimistic Update؟**
   - تحديث الواجهة فوراً قبل تأكيد الخادم
   - للتجربة الأسرع
   - يتراجع عند الخطأ

7. **ما الفرق بين Access Token و Refresh Token؟**
   - Access Token: قصير العمر، يُستخدم في كل طلب
   - Refresh Token: طويل العمر، للحصول على access token جديد

8. **ما الفرق بين Authentication و Authorization؟**
   - Authentication: من أنت؟ (verify identity)
   - Authorization: ماذا يمكنك أن تفعل؟ (check permissions)

9. **كيف تحمي routes في React؟**
   - استخدام Protected Route component
   - التحقق من isAuthenticated
   - توجيه لـ login إذا غير مصادق

10. **متى تستخدم React.memo؟**
    - عندما المكون مكلف جداً
    - عندما يعيد render كثيراً بدون داعي
    - عندما props نادراً ما تتغير

11. **ما الفرق بين useMemo و useCallback؟**
    - useMemo: يحفظ نتائج الحسابات
    - useCallback: يحفظ function definitions

12. **ما هو Code Splitting؟**
    - تقسيم الـ bundle إلى ملفات صغيرة
    - تحميل عند الحاجة
    - يقلل initial load time

13. **لماذا لا تستخدم Zustand للـ Server State؟**
    - لا يوجد caching
    - لا يوجد deduplication
    - لا يوجد auto-refetch
    - React Query مصمم خصيصاً لهذا

14. **ما هو staleTime في React Query؟**
    - المدة التي تعتبر البيانات fresh
    - خلالها لن يتم refetch
    - بعدها البيانات تصبح stale

15. **كيف تحسن أداء React app؟**
    - استخدام React.memo, useMemo, useCallback
    - Lazy loading و code splitting
    - تقليل bundle size
    - تجنب unnecessary re-renders
    - استخدام React DevTools و Profiler

---

## 🎯 Interview Questions

### أسئلة المقابلات

1. **شرح architecture التطبيق React الخاص بك؟**
   **الإجابة**: 
   - React Query للـ Server State (API data)
   - Zustand للـ Client State (UI, preferences)
   - React Router للـ Navigation
   - React Hook Form للـ Forms
   - كل طبقة مسؤولة عن شيء محدد

2. **كيف تتعامل مع caching في React؟**
   **الإجابة**: 
   - React Query للـ server state caching تلقائي
   - staleTime و gcTime للتحكم
   - localStorage/Zustand persist للـ client state
   - Service Worker للـ offline caching

3. **كيف تحسن أداء المكونات الثقيلة؟**
   **الإجابة**: 
   - React.memo لتخطي re-renders
   - useMemo للحسابات الثقيلة
   - useCallback للـ functions
   - Lazy loading للمكونات الكبيرة
   - Code splitting للـ routes

4. **كيف تتعامل مع authentication في React؟**
   **الإجابة**: 
   - Access token في localStorage
   - Refresh token للتجديد
   - Protected routes component
   - Axios interceptors لإضافة tokens
   - Zustand لـ auth state

5. **متى تختار React Query ومتى تختار Redux؟**
   **الإجابة**: 
   - React Query: للـ server state (API data)
   - Redux: للـ complex client state
   - غالباً تستخدمهما معاً
   - React Query للـ API، Redux للـ UI state المعقدة

---

## 💪 Exercises

### تمرين 1: React Query Basics

أنشئ query لجلب users مع error handling:

```tsx
// المطلوب:
interface User {
  id: number;
  name: string;
  email: string;
}

// أنشئ:
- fetchUsers function
- useUsers hook مع error handling
- Loading و error states
- Retry logic
```

### تمرين 2: React Query Mutation

أنشئ mutation لإنشاء user:

```tsx
// المطلوب:
- createUser mutation
- onSuccess callback (invalidate users query)
- onError callback (show error message)
- Loading state للـ button
```

### تمرين 3: Protected Route

أنشئ protected route مع role-based access:

```tsx
// المطلوب:
- ProtectedRoute component
- التحقق من isAuthenticated
- التحقق من user role
- توجيه لـ login أو unauthorized
```

### تمرين 4: Performance Optimization

حسّن أداء المكون التالي:

```tsx
// المطلوب تحسين:
function ProductList({ products, filter }: { products: Product[]; filter: string }) {
  const [sortBy, setSortBy] = useState('name');
  
  const filtered = products.filter(p => 
    p.name.toLowerCase().includes(filter.toLowerCase())
  );
  
  const sorted = [...filtered].sort((a, b) => 
    a[sortBy] > b[sortBy] ? 1 : -1
  );
  
  return (
    <div>
      <select value={sortBy} onChange={(e) => setSortBy(e.target.value)}>
        <option value="name">Name</option>
        <option value="price">Price</option>
      </select>
      {sorted.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}
```

### تمرين 5: Lazy Loading

طبّق lazy loading للـ pages:

```tsx
// المطلوب:
- Lazy load Dashboard, Users, Products, Orders
- إضافة Suspense مع loading fallback
- Error boundary للـ errors
```

---

## 📚 Homework

### الواجب المنزلي

1. **أكمل المشروع العملي**:
   - أضف Users CRUD كامل
   - أضف Products CRUD كامل
   - أضف Orders management
   - أضف Analytics dashboard

2. **أضف الميزات التالية**:
   - Search و filters للـ users و products
   - Pagination للـ lists
   - Bulk operations (delete multiple)
   - Export data (CSV, Excel)
   - Image upload للـ products

3. **تحسين الأداء**:
   - طبّق React.memo للمكونات
   - استخدم useMemo للحسابات
   - استخدم useCallback للـ event handlers
   - طبّق lazy loading للـ routes
   - قلل bundle size

4. **Testing**:
   - اكتب tests للـ API functions
   - اكتب tests للـ components
   - اكتب tests للـ mutations
   - استخدم Jest و React Testing Library

5. **Documentation**:
   - وثق كل API endpoint
   - وثق كل component
   - أضف README للمشروع
   - أنشئ architecture diagram

---

## ✅ Production Checklist

### Before Going to Production

#### Security
- [ ] HTTPS enabled
- [ ] Environment variables secured
- [ ] API keys في server-side فقط
- [ ] CORS configured properly
- [ ] Rate limiting enabled
- [ ] Input validation
- [ ] SQL injection prevention
- [ ] XSS prevention
- [ ] CSRF protection
- [ ] Secure headers (CSP, HSTS, etc.)

#### Performance
- [ ] Bundle size analyzed و optimized
- [ ] Code splitting implemented
- [ ] Lazy loading for heavy components
- [ ] Images optimized
- [ ] Caching strategy configured
- [ ] CDN configured
- [ ] Gzip/Brotli compression
- [ ] Minification enabled
- [ ] Tree shaking working
- [ ] Unused code removed

#### SEO
- [ ] Meta tags configured
- [ ] Open Graph tags
- [ ] Structured data (JSON-LD)
- [ ] Sitemap.xml
- [ ] Robots.txt
- [ ] Canonical URLs
- [ ] Semantic HTML
- [ ] Alt text for images
- [ ] Mobile-friendly

#### Accessibility
- [ ] ARIA labels
- [ ] Keyboard navigation
- [ ] Screen reader compatible
- [ ] Color contrast (WCAG AA)
- [ ] Focus indicators
- [ ] Alt text for images
- [ ] Semantic HTML
- [ ] Form labels
- [ ] Error messages accessible

#### Testing
- [ ] Unit tests written
- [ ] Integration tests written
- [ ] E2E tests written
- [ ] Test coverage > 80%
- [ ] Manual testing completed
- [ ] Cross-browser testing
- [ ] Mobile testing
- [ ] Performance testing
- [ ] Load testing
- [ ] Security testing

#### Monitoring
- [ ] Error tracking (Sentry, etc.)
- [ ] Analytics (Google Analytics, etc.)
- [ ] Performance monitoring
- [ ] Uptime monitoring
- [ ] Logging configured
- [ ] Alerts configured
- [ ] Backup strategy
- [ ] Disaster recovery plan

#### Deployment
- [ ] CI/CD pipeline configured
- [ ] Automated testing in CI/CD
- [ ] Staging environment
- [ ] Blue-green deployment
- [ ] Rollback plan
- [ ] Database migrations
- [ ] Environment-specific configs
- [ ] API versioning
- [ ] Feature flags
- [ ] A/B testing capability

#### Code Quality
- [ ] Code review process
- [ ] Linting configured
- [ ] Formatting (Prettier)
- [ ] TypeScript strict mode
- [ ] No console.logs in production
- [ ] No debug code
- [ ] Comments for complex logic
- [ ] Documentation updated
- [ ] Git history clean
- [ ] meaningful commit messages

#### User Experience
- [ ] Loading states
- [ ] Error states
- [ ] Empty states
- [ ] Offline support
- [ ] Responsive design
- [ ] Fast initial load
- [ ] Smooth animations
- [ ] Clear feedback
- [ ] Intuitive navigation
- [ ] Help documentation

---

## 🎓 الخاتمة

في هذه الجلسة تعلمنا:

1. ✅ React Query لإدارة Server State
2. ✅ Authentication و Authorization
3. ✅ Performance optimization techniques
4. ✅ بناء Admin Dashboard production-ready
5. ✅ الفرق بين Client State و Server State
6. ✅ متى تستخدم React Query ومتى Zustand

**الخطوات القادمة**:
- تطبيق ما تعلمته في مشاريع حقيقية
- استكشاف React Query المتقدم (pagination, infinite scroll)
- تعلم SSR مع Next.js
- استكشاف PWA capabilities

**موارد إضافية**:
- [React Query Documentation](https://tanstack.com/query/latest)
- [Zustand Documentation](https://zustand-demo.pmnd.rs/)
- [React Performance](https://react.dev/learn/render-and-commit)
- [Web Performance](https://web.dev/performance/)

---

**تمنياتي بالتوفيق في رحلتك مع React Production Applications! 🚀**