# React.js Session 7: Performance Optimization & Context API

**Duration:** 3 hours  
**Level:** Intermediate  
**Prerequisites:** Session 1-6 completed (Components, Props, Events, State, Hooks, Forms, API Integration, Routing)  
**Styling:** Tailwind CSS

---

## Session Timeline

### Part 1: useRef (30 minutes)
- **0:00-0:05:** What is useRef? (Topic 1)
- **0:05-0:10:** DOM reference (Topic 1a)
- **0:10-0:15:** Focus input (Topic 1b)
- **0:15-0:20:** Previous value (Topic 1c)
- **0:20-0:25:** Mutable value (Topic 1d)
- **0:25-0:30:** Why ref changes don't cause re-render (Topic 1e)
- **0:30-0:35:** useRef Practice

### Part 2: Performance Hooks (45 minutes)
- **0:35-0:40:** useMemo (Topic 2)
- **0:40-0:45:** useCallback (Topic 3)
- **0:45-0:50:** React.memo (Topic 4)
- **0:50-1:00:** Difference between useMemo, useCallback, React.memo
- **1:00-1:10:** When to use each
- **1:10-1:20:** Performance Hooks Practice

### Part 3: Context API (40 minutes)
- **1:20-1:25:** useContext (Topic 5)
- **1:25-1:30:** Context Provider (Topic 6)
- **1:30-1:35:** Context Consumer (Topic 7)
- **1:35-1:45:** Context API detailed explanation
- **1:45-1:50:** Context Best Practices
- **1:50-2:00:** Context Practice

### Part 4: Custom Hooks (25 minutes)
- **2:00-2:05:** Custom Hooks (Topic 8)
- **2:05-2:10:** useAuth (Topic 9)
- **2:10-2:15:** useTheme (Topic 10)
- **2:15-2:20:** useLocalStorage (Topic 11)
- **2:20-2:25:** useDebounce (Topic 12)
- **2:25-2:30:** Custom Hooks Practice

### Part 5: Performance Optimization (35 minutes)
- **2:30-2:35:** Performance Optimization (Topic 13)
- **2:35-2:40:** Re-rendering (Topic 14)
- **2:40-2:45:** Referential Equality (Topic 15)
- **2:45-2:50:** Common Performance Mistakes (Topic 16)
- **2:50-3:00:** Dashboard Project Implementation
- **3:00-3:15:** Review and Summary

---

## Part 1: useRef

### 1. What is useRef?

**What is it?**
useRef is a React hook that returns a mutable ref object whose `.current` property is initialized to the passed argument. The returned object persists for the full lifetime of the component.

**Why we need it?**
- Access DOM elements directly
- Store mutable values without re-renders
- Keep track of previous values
- Focus on elements programmatically
- Store values that persist across renders

**How it works?**
- Returns a plain JavaScript object `{ current: value }`
- Changing `.current` doesn't trigger re-render
- Value persists across component renders
- Can hold any value (not just DOM references)
- Unlike state, ref changes are synchronous

**Syntax:**
```typescript
const ref = useRef(initialValue);
// Access: ref.current
// Update: ref.current = newValue
```

**Simple Example:**
```typescript
function Counter() {
  const countRef = useRef(0);
  
  const increment = () => {
    countRef.current += 1;
    console.log(countRef.current); // Updates without re-render
  };
  
  return (
    <div>
      <button onClick={increment}>Increment</button>
      <p>Ref count: {countRef.current}</p>
    </div>
  );
}
```

---

### 1a. DOM Reference

**What is it?**
Using useRef to get a direct reference to a DOM element, allowing you to interact with it directly using standard DOM APIs.

**Why we need it?**
- Focus elements programmatically
- Measure element dimensions
- Scroll to elements
- Play/pause media
- Access element properties

**How it works?**
- Attach ref to element via `ref` prop
- Element reference stored in `ref.current`
- Access DOM methods through ref
- Automatically set when component mounts
- Set to null when component unmounts

**Simple Example:**
```typescript
function InputFocus() {
  const inputRef = useRef<HTMLInputElement>(null);
  
  const handleClick = () => {
    inputRef.current?.focus();
  };
  
  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={handleClick}>Focus Input</button>
    </div>
  );
}
```

**Real-world Example:**
```typescript
function LoginForm() {
  const emailRef = useRef<HTMLInputElement>(null);
  const passwordRef = useRef<HTMLInputElement>(null);
  
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    
    // Access DOM values directly
    const email = emailRef.current?.value;
    const password = passwordRef.current?.value;
    
    console.log({ email, password });
  };
  
  const focusPassword = () => {
    passwordRef.current?.focus();
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        ref={emailRef}
        type="email"
        placeholder="Email"
        onKeyDown={(e) => {
          if (e.key === 'Enter') focusPassword();
        }}
      />
      <input
        ref={passwordRef}
        type="password"
        placeholder="Password"
      />
      <button type="submit">Login</button>
    </form>
  );
}

// Scroll to element
function ScrollToElement() {
  const targetRef = useRef<HTMLDivElement>(null);
  
  const scrollToTarget = () => {
    targetRef.current?.scrollIntoView({ behavior: 'smooth' });
  };
  
  return (
    <div>
      <button onClick={scrollToTarget}>Scroll to Target</button>
      <div style={{ height: '1000px' }}>Content</div>
      <div ref={targetRef}>Target Element</div>
    </div>
  );
}

// Measure element
function MeasureElement() {
  const boxRef = useRef<HTMLDivElement>(null);
  const [dimensions, setDimensions] = useState({ width: 0, height: 0 });
  
  useEffect(() => {
    if (boxRef.current) {
      setDimensions({
        width: boxRef.current.offsetWidth,
        height: boxRef.current.offsetHeight
      });
    }
  }, []);
  
  return (
    <div>
      <div ref={boxRef} className="box">
        Measured Box
      </div>
      <p>Width: {dimensions.width}px</p>
      <p>Height: {dimensions.height}px</p>
    </div>
  );
}
```

**DOM Reference Best Practices:**
- Use ref for imperative DOM operations
- Prefer props/state for declarative updates
- Null-check before accessing ref.current
- Clean up refs in useEffect if needed
- Don't overuse - stick to React patterns when possible

---

### 1b. Focus Input

**What is it?**
Using useRef to programmatically focus on input elements, which is essential for user experience (auto-focus, error field focus, etc.).

**Why we need it?**
- Auto-focus on form load
- Focus on error fields
- Focus after navigation
- Improve accessibility
- Better UX

**How it works?**
- Get reference to input element
- Call `.focus()` method on ref
- Can focus on mount or on event
- Works with all focusable elements

**Simple Example:**
```typescript
function AutoFocusInput() {
  const inputRef = useRef<HTMLInputElement>(null);
  
  useEffect(() => {
    inputRef.current?.focus();
  }, []);
  
  return <input ref={inputRef} type="text" placeholder="Auto-focused" />;
}
```

**Real-world Example:**
```typescript
function RegisterForm() {
  const nameRef = useRef<HTMLInputElement>(null);
  const emailRef = useRef<HTMLInputElement>(null);
  const passwordRef = useRef<HTMLInputElement>(null);
  const [errors, setErrors] = useState<Record<string, string>>({});
  
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    const newErrors: Record<string, string> = {};
    
    // Validation
    if (!nameRef.current?.value) newErrors.name = 'Name required';
    if (!emailRef.current?.value) newErrors.email = 'Email required';
    if (!passwordRef.current?.value) newErrors.password = 'Password required';
    
    setErrors(newErrors);
    
    // Focus on first error
    if (newErrors.name) nameRef.current?.focus();
    else if (newErrors.email) emailRef.current?.focus();
    else if (newErrors.password) passwordRef.current?.focus();
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        ref={nameRef}
        type="text"
        placeholder="Name"
        className={errors.name ? 'error' : ''}
      />
      <input
        ref={emailRef}
        type="email"
        placeholder="Email"
        className={errors.email ? 'error' : ''}
      />
      <input
        ref={passwordRef}
        type="password"
        placeholder="Password"
        className={errors.password ? 'error' : ''}
      />
      <button type="submit">Register</button>
    </form>
  );
}

// Focus on mount
function SearchInput() {
  const searchRef = useRef<HTMLInputElement>(null);
  
  useEffect(() => {
    // Focus when component mounts
    searchRef.current?.focus();
  }, []);
  
  return (
    <input
      ref={searchRef}
      type="text"
      placeholder="Search..."
      className="search-input"
    />
  );
}

// Focus on modal open
function Modal({ isOpen, onClose }) {
  const inputRef = useRef<HTMLInputElement>(null);
  
  useEffect(() => {
    if (isOpen) {
      setTimeout(() => inputRef.current?.focus(), 100);
    }
  }, [isOpen]);
  
  if (!isOpen) return null;
  
  return (
    <div className="modal">
      <input ref={inputRef} type="text" placeholder="Enter value" />
      <button onClick={onClose}>Close</button>
    </div>
  );
}
```

**Focus Best Practices:**
- Focus on first input in forms
- Focus on error fields after validation
- Add slight delay for modals/animations
- Consider accessibility (aria attributes)
- Don't force focus (let user control)

---

### 1c. Previous Value

**What is it?**
Using useRef to store the previous value of a prop or state, useful for comparing current vs previous values.

**Why we need it?**
- Detect value changes
- Compare previous vs current
- Trigger effects only on specific changes
- Debug value changes
- Implement logic based on changes

**How it works?**
- Store previous value in ref
- Update ref in useEffect
- Compare current with previous
- Ref persists across renders

**Simple Example:**
```typescript
function PreviousValue({ value }) {
  const prevValue = useRef(value);
  
  useEffect(() => {
    prevValue.current = value;
  }, [value]);
  
  return (
    <div>
      <p>Current: {value}</p>
      <p>Previous: {prevValue.current}</p>
    </div>
  );
}
```

**Real-world Example:**
```typescript
function usePrevious<T>(value: T): T {
  const ref = useRef<T>(value);
  
  useEffect(() => {
    ref.current = value;
  }, [value]);
  
  return ref.current;
}

// Usage in component
function UserProfile({ userId }: { userId: string }) {
  const prevUserId = usePrevious(userId);
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    if (userId !== prevUserId) {
      // Only fetch when userId actually changes
      fetchUser(userId).then(setUser);
    }
  }, [userId, prevUserId]);
  
  return <div>{user?.name}</div>;
}

// Detect scroll direction
function ScrollDirection() {
  const [scrollY, setScrollY] = useState(0);
  const prevScrollY = usePrevious(scrollY);
  const [scrollDirection, setScrollDirection] = useState<'up' | 'down'>('down');
  
  useEffect(() => {
    const handleScroll = () => {
      const currentScrollY = window.scrollY;
      setScrollY(currentScrollY);
      
      if (prevScrollY !== undefined) {
        setScrollDirection(currentScrollY > prevScrollY ? 'down' : 'up');
      }
    };
    
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, [prevScrollY]);
  
  return <div>Scrolling {scrollDirection}</div>;
}

// Track component updates
function ComponentUpdateTracker({ data }) {
  const prevData = usePrevious(data);
  const updateCount = useRef(0);
  
  useEffect(() => {
    updateCount.current += 1;
    console.log(`Component updated ${updateCount.current} times`);
    console.log('Previous data:', prevData);
    console.log('Current data:', data);
  }, [data, prevData]);
  
  return <div>{/* Component content */}</div>;
}
```

**Previous Value Best Practices:**
- Create custom hook for reusability
- Use for debugging value changes
- Compare before triggering effects
- Keep logic simple
- Document why you need previous value

---

### 1d. Mutable Value

**What is it?**
Using useRef to store mutable values that persist across renders without triggering re-renders when updated.

**Why we need it?**
- Store values that don't need to trigger UI updates
- Track values across renders
- Store timers, intervals, subscriptions
- Keep values that persist after unmount
- Avoid unnecessary re-renders

**How it works?**
- Initialize ref with value
- Update ref.current directly
- Value persists across renders
- No re-render triggered
- Access anywhere in component

**Simple Example:**
```typescript
function Timer() {
  const timerRef = useRef<number | null>(null);
  const [seconds, setSeconds] = useState(0);
  
  const startTimer = () => {
    timerRef.current = window.setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);
  };
  
  const stopTimer = () => {
    if (timerRef.current) {
      clearInterval(timerRef.current);
      timerRef.current = null;
    }
  };
  
  useEffect(() => {
    return () => stopTimer(); // Cleanup on unmount
  }, []);
  
  return (
    <div>
      <p>Seconds: {seconds}</p>
      <button onClick={startTimer}>Start</button>
      <button onClick={stopTimer}>Stop</button>
    </div>
  );
}
```

**Real-world Example:**
```typescript
// Store interval ID
function Countdown({ seconds }: { seconds: number }) {
  const intervalRef = useRef<number | null>(null);
  const [remaining, setRemaining] = useState(seconds);
  
  useEffect(() => {
    intervalRef.current = window.setInterval(() => {
      setRemaining(prev => {
        if (prev <= 1) {
          clearInterval(intervalRef.current!);
          return 0;
        }
        return prev - 1;
      });
    }, 1000);
    
    return () => {
      if (intervalRef.current) {
        clearInterval(intervalRef.current);
      }
    };
  }, [seconds]);
  
  return <div>Time remaining: {remaining}s</div>;
}

// Store request cancellation
function DataFetcher() {
  const abortControllerRef = useRef<AbortController | null>(null);
  const [data, setData] = useState(null);
  
  const fetchData = async () => {
    // Cancel previous request
    if (abortControllerRef.current) {
      abortControllerRef.current.abort();
    }
    
    const controller = new AbortController();
    abortControllerRef.current = controller;
    
    try {
      const response = await fetch('/api/data', {
        signal: controller.signal
      });
      const result = await response.json();
      setData(result);
    } catch (error) {
      if (error.name !== 'AbortError') {
        console.error('Fetch error:', error);
      }
    }
  };
  
  useEffect(() => {
    return () => {
      // Cleanup on unmount
      if (abortControllerRef.current) {
        abortControllerRef.current.abort();
      }
    };
  }, []);
  
  return <div>{/* Render data */}</div>;
}

// Store render count (for debugging)
function RenderCounter() {
  const renderCount = useRef(0);
  renderCount.current += 1;
  
  console.log(`Component rendered ${renderCount.current} times`);
  
  return <div>Render count: {renderCount.current}</div>;
}

// Store previous props without re-render
function PropsLogger({ data }) {
  const prevDataRef = useRef(data);
  
  useEffect(() => {
    if (prevDataRef.current !== data) {
      console.log('Data changed from:', prevDataRef.current);
      console.log('Data changed to:', data);
      prevDataRef.current = data;
    }
  }, [data]);
  
  return <div>{/* Component */}</div>;
}
```

**Mutable Value Best Practices:**
- Use for values that don't need UI updates
- Always cleanup intervals/subscriptions
- Use for timers and event listeners
- Store IDs for cleanup
- Don't use for values that need to trigger renders

---

### 1e. Why Ref Changes Don't Cause Re-render

**What is it?**
Understanding why changing a ref's value doesn't trigger a component re-render, which is the key difference between refs and state.

**Why it's important:**
- Choose the right tool (ref vs state)
- Avoid unnecessary re-renders
- Optimize performance
- Understand React's rendering model
- Debug performance issues

**How it works:**
- React only re-renders when state/props change
- Refs are not tracked by React
- Ref.current is just a property
- React doesn't know when it changes
- Ref changes are synchronous

**Explanation:**

**State Triggers Re-render:**
```typescript
function Counter() {
  const [count, setCount] = useState(0);
  
  console.log('Component rendered');
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}

// Every click → setCount → Re-render → "Component rendered" logged
```

**Ref Does NOT Trigger Re-render:**
```typescript
function Counter() {
  const countRef = useRef(0);
  
  console.log('Component rendered');
  
  const increment = () => {
    countRef.current += 1;
    console.log('Ref updated:', countRef.current);
    // No re-render happens
  };
  
  return (
    <div>
      <p>Ref count: {countRef.current}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

// Click → countRef.current updated → "Ref updated" logged
// But "Component rendered" NOT logged (no re-render)
```

**Visual Comparison:**

```
State Change:
setCount(5) → React detects change → Component re-renders → UI updates

Ref Change:
ref.current = 5 → React doesn't detect → No re-render → UI doesn't update
```

**When to Use Each:**

**Use State When:**
- Value needs to trigger UI update
- Value displayed in JSX
- Value affects component output
- Need to track changes for effects

**Use Ref When:**
- Value doesn't need UI update
- Storing DOM element references
- Tracking timers/intervals
- Storing previous values
- Need mutable value without re-render

**Real-world Example:**

```typescript
function Form() {
  const [inputValue, setInputValue] = useState(''); // State - triggers re-render
  const inputRef = useRef<HTMLInputElement>(null); // Ref - no re-render
  const submitCountRef = useRef(0); // Ref - no re-render
  
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setInputValue(e.target.value); // Triggers re-render, UI updates
  };
  
  const handleSubmit = () => {
    submitCountRef.current += 1; // No re-render, just tracking
    console.log(`Submitted ${submitCountRef.current} times`);
    inputRef.current?.focus(); // No re-render, just DOM operation
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        ref={inputRef}
        value={inputValue}
        onChange={handleChange}
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Key Takeaway:**
- **State:** Changes → Re-render → UI updates
- **Ref:** Changes → No re-render → No UI updates
- Choose based on whether you need UI to update

---

## Part 2: Performance Hooks

### 2. useMemo

**What is it?**
useMemo is a React hook that memoizes a computed value, only recalculating it when its dependencies change. It helps optimize performance by avoiding expensive recalculations on every render.

**Why we need it?**
- Optimize expensive calculations
- Avoid unnecessary recalculations
- Improve performance
- Reduce CPU usage
- Cache computed values

**How it works?**
- Accepts a function and dependencies
- Runs function on mount
- Re-runs only when dependencies change
- Returns cached value otherwise
- Comparison based on referential equality

**Syntax:**
```typescript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

**Simple Example:**
```typescript
function ExpensiveCalculation({ a, b }) {
  const result = useMemo(() => {
    console.log('Calculating...');
    return a * b * 1000; // Expensive calculation
  }, [a, b]);
  
  return <div>Result: {result}</div>;
}
```

**Real-world Example:**
```typescript
function ProductList({ products, filter }) {
  const filteredProducts = useMemo(() => {
    console.log('Filtering products...');
    return products.filter(product => 
      product.name.toLowerCase().includes(filter.toLowerCase())
    );
  }, [products, filter]);
  
  return (
    <ul>
      {filteredProducts.map(product => (
        <li key={product.id}>{product.name}</li>
      ))}
    </ul>
  );
}

// Complex calculation
function Statistics({ data }) {
  const statistics = useMemo(() => {
    // Expensive calculation
    const mean = data.reduce((sum, val) => sum + val, 0) / data.length;
    const median = calculateMedian(data);
    const stdDev = calculateStdDev(data, mean);
    
    return { mean, median, stdDev };
  }, [data]);
  
  return <div>{/* Render statistics */}</div>;
}

// Sorting large arrays
function SortedList({ items }) {
  const sortedItems = useMemo(() => {
    return [...items].sort((a, b) => a.value - b.value);
  }, [items]);
  
  return (
    <ul>
      {sortedItems.map(item => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}

// Derived state (when NOT to use useMemo)
function UserForm({ firstName, lastName }) {
  // ❌ WRONG - simple calculation, useMemo adds overhead
  const fullName = useMemo(() => `${firstName} ${lastName}`, [firstName, lastName]);
  
  // ✅ CORRECT - calculate during render
  const fullName = `${firstName} ${lastName}`;
  
  return <input value={fullName} />;
}
```

**When to Use useMemo:**
- Expensive calculations
- Filtering/sorting large arrays
- Complex transformations
- Referential equality needed
- Object/array creation passed to child

**When NOT to Use useMemo:**
- Simple calculations
- Primitive values
- When dependencies change often
- When calculation is cheap
- When you need to ensure latest value

---

### 3. useCallback

**What is it?**
useCallback is a React hook that returns a memoized callback function that only changes when its dependencies change. It helps prevent unnecessary re-renders of child components that receive functions as props.

**Why we need it?**
- Prevent unnecessary re-renders
- Maintain function identity
- Optimize child components
- Work with React.memo
- Pass stable references

**How it works?**
- Accepts a function and dependencies
- Returns same function instance if dependencies unchanged
- Creates new function only when dependencies change
- Useful for functions passed as props
- Works with dependency array

**Syntax:**
```typescript
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

**Simple Example:**
```typescript
function Parent() {
  const [count, setCount] = useState(0);
  
  const handleClick = useCallback(() => {
    setCount(prev => prev + 1);
  }, []); // Empty deps = function never changes
  
  return <Child onClick={handleClick} />;
}

const Child = React.memo(({ onClick }) => {
  console.log('Child rendered');
  return <button onClick={onClick}>Click me</button>;
});
```

**Real-world Example:**
```typescript
function Parent() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('');
  
  // Without useCallback - new function on every render
  const handleClickBad = () => {
    setCount(prev => prev + 1);
  };
  
  // With useCallback - same function unless deps change
  const handleClickGood = useCallback(() => {
    setCount(prev => prev + 1);
  }, []); // Never changes
  
  // With dependencies
  const handleGreet = useCallback(() => {
    console.log(`Hello, ${name}!`);
  }, [name]); // Changes when name changes
  
  return (
    <div>
      <ChildButton onClick={handleClickGood} />
      <input value={name} onChange={(e) => setName(e.target.value)} />
    </div>
  );
}

const ChildButton = React.memo(({ onClick }) => {
  console.log('ChildButton rendered');
  return <button onClick={onClick}>Click me</button>;
});

// API calls with useCallback
function UserList({ userId }) {
  const fetchUser = useCallback(async () => {
    const data = await fetch(`/api/users/${userId}`);
    return data.json();
  }, [userId]);
  
  useEffect(() => {
    fetchUser().then(setUser);
  }, [fetchUser]);
  
  return <div>{/* Render user */}</div>;
}

// Event handlers
function Form() {
  const [value, setValue] = useState('');
  
  const handleChange = useCallback((e: React.ChangeEvent<HTMLInputElement>) => {
    setValue(e.target.value);
  }, []);
  
  const handleSubmit = useCallback((e: React.FormEvent) => {
    e.preventDefault();
    console.log('Submitted:', value);
  }, [value]);
  
  return (
    <form onSubmit={handleSubmit}>
      <input value={value} onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**When to Use useCallback:**
- Function passed to memoized child
- Function used as dependency in useEffect
- Function used in useCallback itself
- Maintaining referential equality
- Performance-critical components

**When NOT to Use useCallback:**
- Simple functions
- Not passed to memoized children
- Function called every render anyway
- When dependencies change frequently
- When overhead outweighs benefit

---

### 4. React.memo

**What is it?**
React.memo is a higher-order component that memoizes a component, preventing it from re-rendering if its props haven't changed. It's similar to PureComponent but for functional components.

**Why we need it?**
- Prevent unnecessary re-renders
- Optimize child components
- Improve performance
- Reduce render cycles
- Skip expensive renders

**How it works?**
- Wraps component
- Shallow compares props
- Skips render if props unchanged
- Re-renders only when props change
- Can customize comparison function

**Syntax:**
```typescript
const MemoizedComponent = React.memo(Component);
```

**Simple Example:**
```typescript
const ExpensiveChild = React.memo(({ value }) => {
  console.log('ExpensiveChild rendered');
  return <div>{value}</div>;
});

function Parent() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <ExpensiveChild value="constant" />
    </div>
  );
}
```

**Real-world Example:**
```typescript
// Memoized child component
const ProductCard = React.memo(({ product, onAddToCart }) => {
  console.log('ProductCard rendered:', product.id);
  
  return (
    <div className="product-card">
      <h3>{product.name}</h3>
      <p>${product.price}</p>
      <button onClick={() => onAddToCart(product.id)}>
        Add to Cart
      </button>
    </div>
  );
});

// Custom comparison
const ProductCard = React.memo(
  ({ product, onAddToCart }) => {
    return <div>{/* Component */}</div>;
  },
  (prevProps, nextProps) => {
    // Custom comparison logic
    return (
      prevProps.product.id === nextProps.product.id &&
      prevProps.product.price === nextProps.product.price
    );
  }
);

// Parent component
function ProductList({ products }) {
  const [cart, setCart] = useState([]);
  
  const addToCart = useCallback((productId) => {
    setCart(prev => [...prev, productId]);
  }, []);
  
  return (
    <div>
      {products.map(product => (
        <ProductCard
          key={product.id}
          product={product}
          onAddToCart={addToCart}
        />
      ))}
    </div>
  );
}

// Memoizing list items
const ListItem = React.memo(({ item, onSelect }) => {
  return (
    <div onClick={() => onSelect(item.id)}>
      {item.name}
    </div>
  );
});

function List({ items }) {
  const [selectedId, setSelectedId] = useState(null);
  
  const handleSelect = useCallback((id) => {
    setSelectedId(id);
  }, []);
  
  return (
    <ul>
      {items.map(item => (
        <ListItem
          key={item.id}
          item={item}
          onSelect={handleSelect}
        />
      ))}
    </ul>
  );
}
```

**When to Use React.memo:**
- Component renders often
- Props change infrequently
- Component is expensive to render
- Performance bottleneck
- Child of frequently re-rendering parent

**When NOT to Use React.memo:**
- Component rarely re-renders
- Props change frequently
- Component is cheap to render
- Memoization overhead > benefit
- When you always want re-render

---

### Difference Between useMemo, useCallback, and React.memo

**useMemo**
- **Purpose:** Memoize a computed value
- **Returns:** The memoized value
- **Use Case:** Expensive calculations, filtering, sorting
- **Dependency:** Recalculates when dependencies change
- **Example:**
```typescript
const filtered = useMemo(() => items.filter(item => item.active), [items]);
```

**useCallback**
- **Purpose:** Memoize a function
- **Returns:** The memoized function
- **Use Case:** Functions passed as props, useEffect dependencies
- **Dependency:** New function when dependencies change
- **Example:**
```typescript
const handleClick = useCallback(() => setCount(c => c + 1), []);
```

**React.memo**
- **Purpose:** Memoize a component
- **Returns:** Memoized component
- **Use Case:** Prevent unnecessary component re-renders
- **Dependency:** Re-renders when props change
- **Example:**
```typescript
const MemoizedComponent = React.memo(Component);
```

**Comparison Table:**

| Feature | useMemo | useCallback | React.memo |
|----------|---------|-------------|------------|
| Memoizes | Value | Function | Component |
| Returns | Computed value | Function | Component |
| For | Calculations | Functions | Re-renders |
| Triggers | Deps change | Deps change | Props change |
| Scope | Inside component | Inside component | Component wrapper |

**When to Use Each:**

**useMemo:**
- Filtering/sorting arrays
- Expensive calculations
- Creating objects/arrays passed to children
- Deriving state from props/state

**useCallback:**
- Functions passed to memoized children
- Functions in useEffect dependencies
- Maintaining function identity
- Event handlers passed down

**React.memo:**
- Expensive child components
- Components with stable props
- Performance bottlenecks
- Lists with many items

---

## Part 3: Context API

### 5. useContext

**What is it?**
useContext is a React hook that allows you to consume context values without wrapping components in a Consumer component. It provides a simpler way to access context in functional components.

**Why we need it?**
- Access context values directly
- Avoid prop drilling
- Share global state
- Simplify context consumption
- Cleaner component code

**How it works?**
- Accepts a context object
- Returns current context value
- Subscribes to context changes
- Re-renders when context changes
- Must be used within Provider

**Syntax:**
```typescript
const value = useContext(MyContext);
```

**Simple Example:**
```typescript
const ThemeContext = createContext('light');

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Child />
    </ThemeContext.Provider>
  );
}

function Child() {
  const theme = useContext(ThemeContext);
  return <div>Current theme: {theme}</div>;
}
```

**Real-world Example:**
```typescript
// Define context
interface AuthContextType {
  user: User | null;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
}

const AuthContext = createContext<AuthContextType | null>(null);

// Provider
function AuthProvider({ children }) {
  const [user, setUser] = useState<User | null>(null);
  
  const login = async (email: string, password: string) => {
    const userData = await api.login(email, password);
    setUser(userData);
  };
  
  const logout = () => {
    setUser(null);
  };
  
  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

// Consumer
function UserProfile() {
  const auth = useContext(AuthContext);
  
  if (!auth) return null;
  
  return (
    <div>
      <p>Welcome, {auth.user?.name}</p>
      <button onClick={auth.logout}>Logout</button>
    </div>
  );
}

// With default value
const ThemeContext = createContext('light');

function ThemeDisplay() {
  const theme = useContext(ThemeContext);
  return <div>Theme: {theme}</div>;
}
```

**useContext Best Practices:**
- Create context in separate file
- Provide default value
- Use with Provider at app root
- Consume where needed
- Consider performance for large contexts

---

### 6. Context Provider

**What is it?**
Context Provider is a component that supplies context values to its descendants. It wraps the part of the component tree that needs access to the context.

**Why we need it?**
- Provide context values
- Wrap component tree
- Update context values
- Share state across components
- Centralize state management

**How it works?**
- Wraps child components
- Accepts value prop
- Provides value to consumers
- Updates propagate to consumers
- Can be nested for overrides

**Syntax:**
```typescript
<MyContext.Provider value={someValue}>
  {/* Child components */}
</MyContext.Provider>
```

**Simple Example:**
```typescript
const UserContext = createContext({ name: 'Guest' });

function App() {
  return (
    <UserContext.Provider value={{ name: 'John' }}>
      <Header />
      <Main />
      <Footer />
    </UserContext.Provider>
  );
}
```

**Real-world Example:**
```typescript
// Multiple providers
function AppProviders({ children }) {
  return (
    <AuthProvider>
      <ThemeProvider>
        <NotificationProvider>
          {children}
        </NotificationProvider>
      </ThemeProvider>
    </AuthProvider>
  );
}

// Provider with state
function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(false);
  
  const login = async (credentials) => {
    setLoading(true);
    try {
      const userData = await api.login(credentials);
      setUser(userData);
    } finally {
      setLoading(false);
    }
  };
  
  const value = {
    user,
    loading,
    login,
    logout: () => setUser(null)
  };
  
  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
}

// Nested providers for override
function App() {
  return (
    <ThemeContext.Provider value="light">
      <Dashboard />
      <ThemeContext.Provider value="dark">
        <AdminPanel />
      </ThemeContext.Provider>
    </ThemeContext.Provider>
  );
}
```

**Provider Best Practices:**
- Create custom provider component
- Include state in provider
- Separate context creation from provider
- Provide memoized callbacks
- Document context structure

---

### 7. Context Consumer

**What is it:**
Context Consumer is the traditional way to consume context values, especially in class components. In functional components, useContext is preferred.

**Why we need it:**
- Consume context in class components
- Alternative to useContext
- Conditional rendering based on context
- Legacy support
- Fine-grained control

**How it works:**
- Wraps component subtree
- Receives context value as render prop
- Re-renders when context changes
- Can be used multiple times
- Functional API

**Syntax:**
```typescript
<MyContext.Consumer>
  {value => <Component value={value} />}
</MyContext.Consumer>
```

**Simple Example:**
```typescript
const ThemeContext = createContext('light');

function ThemedComponent() {
  return (
    <ThemeContext.Consumer>
      {theme => <div className={theme}>Content</div>}
    </ThemeContext.Consumer>
  );
}
```

**Real-world Example:**
```typescript
// Class component using Consumer
class UserProfile extends React.Component {
  render() {
    return (
      <AuthContext.Consumer>
        {({ user, logout }) => (
          <div>
            <p>Welcome, {user?.name}</p>
            <button onClick={logout}>Logout</button>
          </div>
        )}
      </AuthContext.Consumer>
    );
  }
}

// Multiple contexts
function CombinedConsumer() {
  return (
    <>
      <ThemeContext.Consumer>
        {theme => (
          <AuthContext.Consumer>
            {({ user }) => (
              <div className={theme}>
                User: {user?.name}
              </div>
            )}
          </AuthContext.Consumer>
        )}
      </ThemeContext.Consumer>
    </>
  );
}

// Conditional rendering
function ConditionalConsumer() {
  return (
    <AuthContext.Consumer>
      {user => user ? <Dashboard /> : <Login />}
    </AuthContext.Consumer>
  );
}
```

**Consumer vs useContext:**

**Consumer (Legacy):**
```typescript
<MyContext.Consumer>
  {value => <Component value={value} />}
</MyContext.Consumer>
```

**useContext (Modern):**
```typescript
const value = useContext(MyContext);
return <Component value={value} />;
```

**Consumer Best Practices:**
- Use useContext in functional components
- Use Consumer for class components
- Can use both in same app
- Prefer useContext for new code
- Keep render prop functions simple

---

### Context API Detailed Explanation

**What is Context API?**
Context API is React's solution for passing data through the component tree without having to pass props manually at every level (prop drilling). It allows you to share global state across components.

**When to Use Context:**
- Global state (theme, language, auth)
- User information
- Settings/preferences
- Application configuration
- Cross-component communication

**When NOT to Use Context:**
- Component-specific state
- Props that change frequently
- Small component trees
- When simple props suffice
- Performance-critical data

**Context Creation Pattern:**

```typescript
// 1. Create context with default value
const MyContext = createContext(defaultValue);

// 2. Create provider component
function MyProvider({ children }) {
  const [state, setState] = useState(initialState);
  
  const value = {
    state,
    setState,
    // ... other methods
  };
  
  return (
    <MyContext.Provider value={value}>
      {children}
    </MyContext.Provider>
  );
}

// 3. Create custom hook for consumption
function useMyContext() {
  const context = useContext(MyContext);
  if (!context) {
    throw new Error('useMyContext must be used within MyProvider');
  }
  return context;
}

// 4. Use in components
function MyComponent() {
  const { state, setState } = useMyContext();
  return <div>{/* Use context */}</div>;
}
```

**Context Performance Considerations:**
- Context updates cause all consumers to re-render
- Split context into smaller contexts if needed
- Use useMemo for context values
- Consider using libraries like Redux for complex state
- Memoize context provider value

---

## Part 4: Custom Hooks

### 8. Custom Hooks

**What is it?**
Custom hooks are reusable functions that start with "use" and can call other hooks. They allow you to extract and reuse stateful logic between components.

**Why we need it?**
- Reuse stateful logic
- Separate concerns
- Test logic independently
- Reduce code duplication
- Share logic across components

**How it works:**
- Function starting with "use"
- Can use other hooks
- Returns state and functions
- Encapsulates logic
- Can be shared across components

**Syntax:**
```typescript
function useCustomHook() {
  const [state, setState] = useState(initialValue);
  
  const doSomething = () => {
    // Logic here
  };
  
  return { state, doSomething };
}
```

**Simple Example:**
```typescript
function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);
  
  const increment = () => setCount(prev => prev + 1);
  const decrement = () => setCount(prev => prev - 1);
  const reset = () => setCount(initialValue);
  
  return { count, increment, decrement, reset };
}

function Counter() {
  const { count, increment, decrement, reset } = useCounter();
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}
```

**Real-world Example:**
```typescript
// useWindowSize
function useWindowSize() {
  const [size, setSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight
  });
  
  useEffect(() => {
    const handleResize = () => {
      setSize({
        width: window.innerWidth,
        height: window.innerHeight
      });
    };
    
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);
  
  return size;
}

// useToggle
function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);
  
  const toggle = useCallback(() => setValue(prev => !prev), []);
  const setTrue = useCallback(() => setValue(true), []);
  const setFalse = useCallback(() => setValue(false), []);
  
  return { value, toggle, setTrue, setFalse };
}

// useFetch
function useFetch<T>(url: string) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);
  
  useEffect(() => {
    const controller = new AbortController();
    
    const fetchData = async () => {
      setLoading(true);
      setError(null);
      
      try {
        const response = await fetch(url, {
          signal: controller.signal
        });
        
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const result = await response.json();
        setData(result);
      } catch (err) {
        if (err.name !== 'AbortError') {
          setError(err instanceof Error ? err.message : 'An error occurred');
        }
      } finally {
        setLoading(false);
      }
    };
    
    fetchData();
    
    return () => controller.abort();
  }, [url]);
  
  return { data, loading, error };
}
```

**Custom Hook Best Practices:**
- Start with "use" prefix
- Return consistent interface
- Handle cleanup in useEffect
- Use TypeScript for types
- Keep hooks focused
- Document hook usage

---

### 9. useAuth

**What is it?**
A custom hook that encapsulates authentication logic, providing login, logout, and user state management across the application.

**Why we need it?**
- Centralize auth logic
- Share auth state globally
- Simplify auth operations
- Provide consistent auth API
- Integrate with routing

**How it works:**
- Wraps AuthContext
- Provides auth methods
- Manages auth state
- Handles token storage
- Provides auth helpers

**Implementation:**
```typescript
// Auth Context
interface AuthContextType {
  user: User | null;
  loading: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
  isAuthenticated: boolean;
}

const AuthContext = createContext<AuthContextType | null>(null);

// Provider
function AuthProvider({ children }) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    const token = localStorage.getItem('authToken');
    if (token) {
      validateToken(token).then(setUser).finally(() => setLoading(false));
    } else {
      setLoading(false);
    }
  }, []);
  
  const login = async (email: string, password: string) => {
    const { token, user } = await api.login(email, password);
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

// Custom Hook
function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
}

// Usage
function LoginForm() {
  const { login, loading } = useAuth();
  const navigate = useNavigate();
  
  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    await login(email, password);
    navigate('/dashboard');
  };
  
  return <form onSubmit={handleSubmit}>{/* ... */}</form>;
}

function ProtectedRoute({ children }) {
  const { isAuthenticated, loading } = useAuth();
  
  if (loading) return <LoadingSpinner />;
  if (!isAuthenticated) return <Navigate to="/login" />;
  
  return <>{children}</>;
}
```

---

### 10. useTheme

**What is it?**
A custom hook that manages theme state (light/dark mode) and provides theme-related utilities across the application.

**Why we need it?**
- Centralize theme logic
- Provide theme switching
- Persist theme preference
- Share theme globally
- Enable theme-based styling

**Implementation:**
```typescript
// Theme Context
type Theme = 'light' | 'dark';

interface ThemeContextType {
  theme: Theme;
  toggleTheme: () => void;
  setTheme: (theme: Theme) => void;
}

const ThemeContext = createContext<ThemeContextType | null>(null);

// Provider
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState<Theme>(() => {
    const saved = localStorage.getItem('theme') as Theme;
    return saved || 'light';
  });
  
  useEffect(() => {
    localStorage.setItem('theme', theme);
    document.documentElement.className = theme;
  }, [theme]);
  
  const toggleTheme = useCallback(() => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  }, []);
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// Custom Hook
function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
}

// Usage
function ThemeToggle() {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <button onClick={toggleTheme}>
      {theme === 'light' ? '🌙' : '☀️'}
    </button>
  );
}

function ThemedComponent() {
  const { theme } = useTheme();
  
  return (
    <div className={`p-4 rounded ${theme === 'dark' ? 'bg-gray-800 text-white' : 'bg-white text-gray-900'}`}>
      Themed content
    </div>
  );
}
```

---

### 11. useLocalStorage

**What is it?**
A custom hook that synchronizes state with localStorage, providing persistence across page reloads and browser sessions.

**Why we need it?**
- Persist state across reloads
- Save user preferences
- Cache data locally
- Offline functionality
- Improved UX

**Implementation:**
```typescript
function useLocalStorage<T>(key: string, initialValue: T) {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error('Error reading localStorage:', error);
      return initialValue;
    }
  });
  
  const setValue = useCallback((value: T | ((val: T) => T)) => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error('Error setting localStorage:', error);
    }
  }, [key, storedValue]);
  
  return [storedValue, setValue] as const;
}

// Usage
function Settings() {
  const [theme, setTheme] = useLocalStorage('theme', 'light');
  const [language, setLanguage] = useLocalStorage('language', 'en');
  
  return (
    <div>
      <select value={theme} onChange={(e) => setTheme(e.target.value)}>
        <option value="light">Light</option>
        <option value="dark">Dark</option>
      </select>
      
      <select value={language} onChange={(e) => setLanguage(e.target.value)}>
        <option value="en">English</option>
        <option value="es">Spanish</option>
      </select>
    </div>
  );
}

// Persist form data
function ContactForm() {
  const [formData, setFormData] = useLocalStorage('contactForm', {
    name: '',
    email: '',
    message: ''
  });
  
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };
  
  return (
    <form>
      <input
        name="name"
        value={formData.name}
        onChange={handleChange}
      />
      {/* ... other fields */}
    </form>
  );
}
```

---

### 12. useDebounce

**What is it?**
A custom hook that delays updating a value until after a specified delay has passed since the last update, useful for search inputs and API calls.

**Why we need it?**
- Reduce API calls
- Improve performance
- Optimize search functionality
- Debounce user input
- Prevent excessive updates

**Implementation:**
```typescript
function useDebounce<T>(value: T, delay: number = 500): T {
  const [debouncedValue, setDebouncedValue] = useState<T>(value);
  
  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);
    
    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);
  
  return debouncedValue;
}

// Usage
function SearchComponent() {
  const [searchQuery, setSearchQuery] = useState('');
  const debouncedQuery = useDebounce(searchQuery, 500);
  
  useEffect(() => {
    if (debouncedQuery) {
      searchProducts(debouncedQuery);
    }
  }, [debouncedQuery]);
  
  return (
    <input
      value={searchQuery}
      onChange={(e) => setSearchQuery(e.target.value)}
      placeholder="Search..."
    />
  );
}

// Debounced function
function useDebouncedCallback<T extends (...args: any[]) => any>(
  callback: T,
  delay: number = 500
): T {
  const callbackRef = useRef(callback);
  const timeoutRef = useRef<number | null>(null);
  
  useEffect(() => {
    callbackRef.current = callback;
  }, [callback]);
  
  return useCallback(
    (...args: Parameters<T>) => {
      if (timeoutRef.current) {
        clearTimeout(timeoutRef.current);
      }
      
      timeoutRef.current = window.setTimeout(() => {
        callbackRef.current(...args);
      }, delay);
    },
    [delay]
  ) as T;
}

// Usage
function AutoSaveForm() {
  const [formData, setFormData] = useState({ name: '', email: '' });
  
  const saveForm = useDebouncedCallback(async (data) => {
    await api.saveForm(data);
  }, 1000);
  
  useEffect(() => {
    saveForm(formData);
  }, [formData, saveForm]);
  
  return (
    <form>
      <input
        value={formData.name}
        onChange={(e) => setFormData({ ...formData, name: e.target.value })}
      />
    </form>
  );
}
```

---

## Part 5: Performance Optimization

### 13. Performance Optimization

**What is it?**
Performance optimization in React involves techniques to reduce unnecessary re-renders, minimize computational overhead, and improve the overall speed and responsiveness of your application.

**Why we need it?**
- Faster load times
- Better user experience
- Lower CPU usage
- Smoother interactions
- Better mobile performance

**Key Techniques:**
- Memoization (useMemo, useCallback, React.memo)
- Code splitting
- Virtualization for long lists
- Lazy loading
- Optimizing dependencies
- Avoiding inline functions/objects

**Simple Example:**
```typescript
// Before optimization - re-renders on every parent update
function Parent() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <button onClick={() => setCount(c => c + 1)}>Increment</button>
      <ExpensiveChild />
    </div>
  );
}

// After optimization - child only re-renders when needed
const ExpensiveChild = React.memo(() => {
  console.log('Child rendered');
  return <div>Expensive Component</div>;
});
```

**Real-world Example:**
```typescript
// Optimized list rendering
function OptimizedList({ items }) {
  const renderItem = useCallback((item: Item) => (
    <ListItem key={item.id} item={item} />
  ), []);
  
  return (
    <ul>
      {items.map(renderItem)}
    </ul>
  );
}

// Memoized list item
const ListItem = React.memo(({ item }) => {
  return <li>{item.name}</li>;
});

// Virtual scrolling for long lists
import { FixedSizeList } from 'react-window';

function VirtualizedList({ items }) {
  const Row = ({ index, style }) => (
    <div style={style}>
      {items[index].name}
    </div>
  );
  
  return (
    <FixedSizeList
      height={400}
      itemCount={items.length}
      itemSize={35}
      width={300}
    >
      {Row}
    </FixedSizeList>
  );
}

// Code splitting
const HeavyComponent = React.lazy(() => import('./HeavyComponent'));

function App() {
  const [showHeavy, setShowHeavy] = useState(false);
  
  return (
    <div>
      <button onClick={() => setShowHeavy(true)}>
        Load Heavy Component
      </button>
      {showHeavy && (
        <Suspense fallback={<Loading />}>
          <HeavyComponent />
        </Suspense>
      )}
    </div>
  );
}
```

**Performance Optimization Best Practices:**
- Profile before optimizing
- Optimize only bottlenecks
- Use React DevTools Profiler
- Measure actual performance
- Consider trade-offs
- Keep code readable

---

### 14. Re-rendering

**What is it?**
Re-rendering is the process where React updates the DOM by calling component functions again when state or props change. Understanding and controlling re-renders is crucial for performance.

**Why it's important:**
- Too many re-renders = slow app
- Unnecessary re-renders waste resources
- Understanding triggers helps optimization
- Critical for smooth UX
- Mobile performance

**What Triggers Re-renders:**
- State changes (useState)
- Prop changes
- Parent re-renders
- Context changes
- Force updates

**What Does NOT Trigger Re-render:**
- Ref changes
- Regular variable changes
- External state changes
- Local mutations

**Simple Example:**
```typescript
function Counter() {
  const [count, setCount] = useState(0);
  const countRef = useRef(0);
  
  console.log('Component rendered');
  
  return (
    <div>
      <p>State: {count}</p>
      <p>Ref: {countRef.current}</p>
      <button onClick={() => setCount(c => c + 1)}>
        Update State (triggers re-render)
      </button>
      <button onClick={() => { countRef.current += 1 }}>
        Update Ref (no re-render)
      </button>
    </div>
  );
}
```

**Real-world Example:**
```typescript
// Parent re-renders → all children re-render
function Parent() {
  const [count, setCount] = useState(0);
  
  console.log('Parent rendered');
  
  return (
    <div>
      <button onClick={() => setCount(c => c + 1)}>Increment</button>
      <Child1 />
      <Child2 />
      <Child3 />
    </div>
  );
}

function Child1() {
  console.log('Child1 rendered');
  return <div>Child 1</div>;
}

// Optimized - child only re-renders when props change
const Child2 = React.memo(() => {
  console.log('Child2 rendered');
  return <div>Child 2</div>;
});

// Optimized - child with stable props
function Child3() {
  console.log('Child3 rendered');
  return <div>Child 3</div>;
}

function OptimizedParent() {
  const [count, setCount] = useState(0);
  const handleClick = useCallback(() => setCount(c => c + 1), []);
  
  console.log('Parent rendered');
  
  return (
    <div>
      <button onClick={handleClick}>Increment</button>
      <Child1 />
      <Child2 />
      <Child3 onClick={handleClick} />
    </div>
  );
}
```

**Reducing Re-renders:**
- Use React.memo for expensive children
- Use useCallback for function props
- Use useMemo for expensive calculations
- Lift state up only when needed
- Split components to reduce affected area

---

### 15. Referential Equality

**What is it?**
Referential equality in JavaScript/React means checking if two references point to the same object in memory. React uses shallow comparison (referential equality) to determine if props/state have changed.

**Why it's important:**
- React uses reference equality for change detection
- New object/array references trigger re-renders
- Understanding helps prevent unnecessary renders
- Critical for useMemo/useCallback
- Key for React.memo effectiveness

**How it works:**
```typescript
// Primitive values - value equality
1 === 1 // true
'string' === 'string' // true

// Objects/arrays - reference equality
{} === {} // false (different references)
[] === [] // false (different references)
const obj = {};
obj === obj // true (same reference)
```

**Simple Example:**
```typescript
function Component() {
  const [count, setCount] = useState(0);
  
  // New object on every render
  const config = { value: count };
  
  // Same object reference
  const configRef = useRef({ value: count });
  
  console.log('Component rendered');
  
  return <div>{/* ... */}</div>;
}
```

**Real-world Example:**
```typescript
// Problem - new object every render
function Parent() {
  const [count, setCount] = useState(0);
  
  const config = { count }; // New object every render
  
  return <Child config={config} />; // Child re-renders every time
}

// Solution 1 - useMemo
function Parent() {
  const [count, setCount] = useState(0);
  
  const config = useMemo(() => ({ count }), [count]);
  
  return <Child config={config} />; // Child re-renders only when count changes
}

// Solution 2 - useCallback for functions
function Parent() {
  const [count, setCount] = useState(0);
  
  const handleClick = () => setCount(c => c + 1); // New function every render
  
  const memoizedHandleClick = useCallback(() => setCount(c => c + 1), []);
  
  return <Child onClick={memoizedHandleClick} />;
}

// Solution 3 - Move state down
function Parent() {
  return <Child />; // Child manages its own state
}

function Child() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(c => c + 1)}>Increment</button>
    </div>
  );
}
```

**Referential Equality Best Practices:**
- Use useMemo for objects/arrays passed to children
- Use useCallback for functions passed to children
- Move state down when possible
- Understand React's shallow comparison
- Profile to verify optimization impact

---

### 16. Common Performance Mistakes

**1. Optimizing Too Early**
```typescript
// ❌ WRONG - Premature optimization
const sum = useMemo(() => a + b, [a, b]); // Addition is cheap

// ✅ CORRECT - Let React handle it
const sum = a + b;
```

**2. Creating Functions in Render**
```typescript
// ❌ WRONG - New function every render
function Parent() {
  return <Child onClick={() => console.log('click')} />;
}

// ✅ CORRECT - Use useCallback
function Parent() {
  const handleClick = useCallback(() => console.log('click'), []);
  return <Child onClick={handleClick} />;
}
```

**3. Not Memoizing Context Values**
```typescript
// ❌ WRONG - New object every render
function Provider({ children }) {
  const [user, setUser] = useState(null);
  return (
    <Context.Provider value={{ user, setUser }}>
      {children}
    </Context.Provider>
  );
}

// ✅ CORRECT - Memoize context value
function Provider({ children }) {
  const [user, setUser] = useState(null);
  const value = useMemo(() => ({ user, setUser }), [user]);
  return (
    <Context.Provider value={value}>
      {children}
    </Context.Provider>
  );
}
```

**4. Using Index as Key**
```typescript
// ❌ WRONG - Index as key causes issues
{items.map((item, index) => (
  <div key={index}>{item.name}</div>
))}

// ✅ CORRECT - Use unique ID
{items.map(item => (
  <div key={item.id}>{item.name}</div>
))}
```

**5. Inline Styles**
```typescript
// ❌ WRONG - New object every render
<div style={{ color: 'red', fontSize: '16px' }} />

// ✅ CORRECT - Use CSS classes or memoize
<div className="text-red text-lg" />
// or
const style = useMemo(() => ({ color: 'red', fontSize: '16px' }), []);
<div style={style} />
```

**6. Large Context**
```typescript
// ❌ WRONG - Everything in one context
const AppContext = createContext({
  user: null,
  theme: 'light',
  language: 'en',
  settings: {},
  // ... many more
});

// ✅ CORRECT - Split into smaller contexts
const UserContext = createContext({ user: null });
const ThemeContext = createContext({ theme: 'light' });
const SettingsContext = createContext({ settings: {} });
```

**7. Unnecessary State**
```typescript
// ❌ WRONG - State for derived value
function Component({ items }) {
  const [filteredItems, setFilteredItems] = useState(items);
  
  useEffect(() => {
    setFilteredItems(items.filter(item => item.active));
  }, [items]);
  
  return <div>{/* ... */}</div>;
}

// ✅ CORRECT - Derive during render
function Component({ items }) {
  const filteredItems = items.filter(item => item.active);
  return <div>{/* ... */}</div>;
}
```

**8. Over-optimizing**
```typescript
// ❌ WRONG - Over-optimizing simple components
const SimpleComponent = React.memo(({ text }) => {
  return <div>{text}</div>;
});

// ✅ CORRECT - Let it re-render, it's cheap
function SimpleComponent({ text }) {
  return <div>{text}</div>;
}
```

---

## Practical Project: Dashboard with Theme & Auth

Let's build a complete Dashboard application demonstrating all the concepts we've learned.

### Project Overview

**Features:**
- Theme switcher (Light/Dark mode)
- User authentication state
- Profile page
- Settings page
- Performance optimizations
- Custom hooks

### Project Structure

```
src/
├── components/
│   ├── common/
│   │   ├── Button.tsx
│   │   ├── Card.tsx
│   │   └── Input.tsx
│   ├── layout/
│   │   ├── Header.tsx
│   │   ├── Sidebar.tsx
│   │   └── Footer.tsx
├── context/
│   ├── AuthContext.tsx
│   └── ThemeContext.tsx
├── hooks/
│   ├── useAuth.ts
│   ├── useTheme.ts
│   └── useLocalStorage.ts
├── pages/
│   ├── Login.tsx
│   ├── Dashboard.tsx
│   ├── Profile.tsx
│   └── Settings.tsx
├── types/
│   └── index.ts
└── App.tsx
```

### Step 1: Define Types

Create `src/types/index.ts`:
```typescript
export interface User {
  id: string;
  name: string;
  email: string;
  avatar?: string;
}

export interface LoginCredentials {
  email: string;
  password: string;
}

export type Theme = 'light' | 'dark';
```

### Step 2: Create Theme Context

Create `src/context/ThemeContext.tsx`:
```typescript
import { createContext, useContext, useState, useCallback, useEffect, ReactNode } from 'react';
import { Theme } from '../types';

interface ThemeContextType {
  theme: Theme;
  toggleTheme: () => void;
  setTheme: (theme: Theme) => void;
}

const ThemeContext = createContext<ThemeContextType | null>(null);

export function ThemeProvider({ children }: { children: ReactNode }) {
  const [theme, setTheme] = useState<Theme>(() => {
    const savedTheme = localStorage.getItem('theme') as Theme;
    return savedTheme || 'light';
  });

  useEffect(() => {
    localStorage.setItem('theme', theme);
    document.documentElement.className = theme;
  }, [theme]);

  const toggleTheme = useCallback(() => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  }, []);

  const value: ThemeContextType = {
    theme,
    toggleTheme,
    setTheme
  };

  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}

export function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
}
```

### Step 3: Create Auth Context

Create `src/context/AuthContext.tsx`:
```typescript
import { createContext, useContext, useState, useCallback, useEffect, ReactNode } from 'react';
import { User, LoginCredentials } from '../types';

interface AuthContextType {
  user: User | null;
  loading: boolean;
  login: (credentials: LoginCredentials) => Promise<void>;
  logout: () => void;
  isAuthenticated: boolean;
}

const AuthContext = createContext<AuthContextType | null>(null);

export function AuthProvider({ children }: { children: ReactNode }) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const savedUser = localStorage.getItem('user');
    if (savedUser) {
      setUser(JSON.parse(savedUser));
    }
    setLoading(false);
  }, []);

  const login = useCallback(async (credentials: LoginCredentials) => {
    setLoading(true);
    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 1000));
      
      const mockUser: User = {
        id: '1',
        name: 'Admin User',
        email: credentials.email,
        avatar: 'https://via.placeholder.com/150'
      };
      
      setUser(mockUser);
      localStorage.setItem('user', JSON.stringify(mockUser));
    } finally {
      setLoading(false);
    }
  }, []);

  const logout = useCallback(() => {
    setUser(null);
    localStorage.removeItem('user');
  }, []);

  const value: AuthContextType = {
    user,
    loading,
    login,
    logout,
    isAuthenticated: !!user
  };

  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
}
```

### Step 4: Create useLocalStorage Hook

Create `src/hooks/useLocalStorage.ts`:
```typescript
import { useState, useEffect, useCallback } from 'react';

function useLocalStorage<T>(key: string, initialValue: T) {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error('Error reading localStorage:', error);
      return initialValue;
    }
  });

  const setValue = useCallback((value: T | ((val: T) => T)) => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error('Error setting localStorage:', error);
    }
  }, [key, storedValue]);

  return [storedValue, setValue] as const;
}

export default useLocalStorage;
```

### Step 5: Create Layout Components

Create `src/components/layout/Header.tsx`:
```typescript
import { useAuth } from '../../context/AuthContext';
import { useTheme } from '../../context/ThemeContext';

export function Header() {
  const { user, logout } = useAuth();
  const { theme, toggleTheme } = useTheme();

  return (
    <header className={`bg-white shadow ${theme === 'dark' ? 'dark:bg-gray-800' : ''}`}>
      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div className="flex justify-between items-center h-16">
          <h1 className="text-xl font-bold text-gray-900">
            Dashboard
          </h1>
          
          <div className="flex items-center space-x-4">
            <button
              onClick={toggleTheme}
              className="p-2 rounded-lg hover:bg-gray-100"
            >
              {theme === 'light' ? '🌙' : '☀️'}
            </button>
            
            {user && (
              <>
                <span className="text-gray-700">{user.name}</span>
                <button
                  onClick={logout}
                  className="px-4 py-2 bg-red-600 text-white rounded-lg hover:bg-red-700"
                >
                  Logout
                </button>
              </>
            )}
          </div>
        </div>
      </div>
    </header>
  );
}
```

Create `src/components/layout/Sidebar.tsx`:
```typescript
import { NavLink } from 'react-router-dom';

export function Sidebar() {
  const navItems = [
    { path: '/dashboard', label: 'Dashboard', icon: '📊' },
    { path: '/dashboard/profile', label: 'Profile', icon: '👤' },
    { path: '/dashboard/settings', label: 'Settings', icon: '⚙️' },
  ];

  return (
    <aside className="w-64 bg-white shadow-lg min-h-screen">
      <nav className="p-4">
        <ul className="space-y-2">
          {navItems.map((item) => (
            <li key={item.path}>
              <NavLink
                to={item.path}
                className={({ isActive }) =>
                  `flex items-center space-x-3 px-4 py-2 rounded-lg transition-colors ${
                    isActive
                      ? 'bg-blue-600 text-white'
                      : 'text-gray-700 hover:bg-gray-100'
                  }`
                }
              >
                <span>{item.icon}</span>
                <span>{item.label}</span>
              </NavLink>
            </li>
          ))}
        </ul>
      </nav>
    </aside>
  );
}
```

### Step 6: Create Page Components

Create `src/pages/Login.tsx`:
```typescript
import { useState } from 'react';
import { useNavigate, useLocation } from 'react-router-dom';
import { useAuth } from '../context/AuthContext';
import { LoginCredentials } from '../types';

export function Login() {
  const [credentials, setCredentials] = useState<LoginCredentials>({
    email: '',
    password: ''
  });
  const [error, setError] = useState('');
  const [loading, setLoading] = useState(false);
  
  const { login } = useAuth();
  const navigate = useNavigate();
  const location = useLocation();
  
  const from = location.state?.from?.pathname || '/dashboard';

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setError('');
    setLoading(true);

    try {
      await login(credentials);
      navigate(from, { replace: true });
    } catch (err) {
      setError('Invalid credentials');
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-50">
      <div className="max-w-md w-full bg-white rounded-lg shadow-md p-8">
        <h2 className="text-2xl font-bold mb-6 text-center">Login</h2>
        
        {error && (
          <div className="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded mb-4">
            {error}
          </div>
        )}

        <form onSubmit={handleSubmit}>
          <div className="mb-4">
            <label className="block text-gray-700 mb-2">Email</label>
            <input
              type="email"
              value={credentials.email}
              onChange={(e) => setCredentials({ ...credentials, email: e.target.value })}
              className="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
              required
            />
          </div>

          <div className="mb-6">
            <label className="block text-gray-700 mb-2">Password</label>
            <input
              type="password"
              value={credentials.password}
              onChange={(e) => setCredentials({ ...credentials, password: e.target.value })}
              className="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
              required
            />
          </div>

          <button
            type="submit"
            disabled={loading}
            className="w-full bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 disabled:bg-gray-400"
          >
            {loading ? 'Logging in...' : 'Login'}
          </button>
        </form>
      </div>
    </div>
  );
}
```

Create `src/pages/Dashboard.tsx`:
```typescript
import { useMemo } from 'react';
import { useAuth } from '../context/AuthContext';

export function Dashboard() {
  const { user } = useAuth();

  const stats = useMemo(() => ({
    totalUsers: 156,
    totalOrders: 89,
    revenue: 12450
  }), []);

  return (
    <div>
      <h1 className="text-3xl font-bold text-gray-900 mb-6">Dashboard</h1>
      
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
        <div className="bg-white rounded-lg shadow p-6">
          <h3 className="text-lg font-semibold text-gray-700">Total Users</h3>
          <p className="text-3xl font-bold text-blue-600 mt-2">{stats.totalUsers}</p>
        </div>
        
        <div className="bg-white rounded-lg shadow p-6">
          <h3 className="text-lg font-semibold text-gray-700">Total Orders</h3>
          <p className="text-3xl font-bold text-green-600 mt-2">{stats.totalOrders}</p>
        </div>
        
        <div className="bg-white rounded-lg shadow p-6">
          <h3 className="text-lg font-semibold text-gray-700">Revenue</h3>
          <p className="text-3xl font-bold text-purple-600 mt-2">${stats.revenue.toLocaleString()}</p>
        </div>
      </div>

      <div className="bg-white rounded-lg shadow p-6">
        <h2 className="text-xl font-bold text-gray-900 mb-4">Welcome, {user?.name}!</h2>
        <p className="text-gray-600">This is your dashboard overview.</p>
      </div>
    </div>
  );
}
```

Create `src/pages/Profile.tsx`:
```typescript
import { useState, useCallback } from 'react';
import { useAuth } from '../context/AuthContext';
import useLocalStorage from '../hooks/useLocalStorage';

export function Profile() {
  const { user } = useAuth();
  const [name, setName] = useLocalStorage('userName', user?.name || '');
  const [bio, setBio] = useLocalStorage('userBio', '');

  const handleSave = useCallback(() => {
    alert('Profile saved!');
  }, []);

  return (
    <div>
      <h1 className="text-3xl font-bold text-gray-900 mb-6">Profile</h1>
      
      <div className="bg-white rounded-lg shadow p-6">
        <div className="space-y-4">
          <div>
            <label className="block text-gray-700 font-semibold mb-2">Name</label>
            <input
              type="text"
              value={name}
              onChange={(e) => setName(e.target.value)}
              className="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
          </div>
          
          <div>
            <label className="block text-gray-700 font-semibold mb-2">Email</label>
            <p className="text-gray-900">{user?.email}</p>
          </div>
          
          <div>
            <label className="block text-gray-700 font-semibold mb-2">Bio</label>
            <textarea
              value={bio}
              onChange={(e) => setBio(e.target.value)}
              rows={4}
              className="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
          </div>
          
          <button
            onClick={handleSave}
            className="px-6 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700"
          >
            Save Profile
          </button>
        </div>
      </div>
    </div>
  );
}
```

Create `src/pages/Settings.tsx`:
```typescript
import { useTheme } from '../context/ThemeContext';
import useLocalStorage from '../hooks/useLocalStorage';

export function Settings() {
  const { theme, toggleTheme } = useTheme();
  const [notifications, setNotifications] = useLocalStorage('notifications', true);
  const [language, setLanguage] = useLocalStorage('language', 'en');

  return (
    <div>
      <h1 className="text-3xl font-bold text-gray-900 mb-6">Settings</h1>
      
      <div className="space-y-6">
        <div className="bg-white rounded-lg shadow p-6">
          <h2 className="text-xl font-bold text-gray-900 mb-4">Appearance</h2>
          <div className="space-y-4">
            <div className="flex items-center justify-between">
              <div>
                <p className="font-semibold text-gray-900">Theme</p>
                <p className="text-sm text-gray-600">Current: {theme}</p>
              </div>
              <button
                onClick={toggleTheme}
                className="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700"
              >
                Toggle Theme
              </button>
            </div>
          </div>
        </div>

        <div className="bg-white rounded-lg shadow p-6">
          <h2 className="text-xl font-bold text-gray-900 mb-4">Notifications</h2>
          <div className="space-y-4">
            <label className="flex items-center space-x-3">
              <input
                type="checkbox"
                checked={notifications}
                onChange={(e) => setNotifications(e.target.checked)}
                className="w-4 h-4"
              />
              <span className="text-gray-700">Enable email notifications</span>
            </label>
          </div>
        </div>

        <div className="bg-white rounded-lg shadow p-6">
          <h2 className="text-xl font-bold text-gray-900 mb-4">Language</h2>
          <div className="space-y-4">
            <select
              value={language}
              onChange={(e) => setLanguage(e.target.value)}
              className="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            >
              <option value="en">English</option>
              <option value="es">Spanish</option>
              <option value="fr">French</option>
              <option value="de">German</option>
            </select>
          </div>
        </div>
      </div>
    </div>
  );
}
```

### Step 7: Create Main App Component

Create `src/App.tsx`:
```typescript
import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom';
import { AuthProvider, useAuth } from './context/AuthContext';
import { ThemeProvider } from './context/ThemeContext';
import { Header } from './components/layout/Header';
import { Sidebar } from './components/layout/Sidebar';
import { Login } from './pages/Login';
import { Dashboard } from './pages/Dashboard';
import { Profile } from './pages/Profile';
import { Settings } from './pages/Settings';

function Layout({ children }: { children: React.ReactNode }) {
  const { isAuthenticated } = useAuth();
  
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }

  return (
    <div className="min-h-screen bg-gray-50">
      <Header />
      <div className="flex">
        <Sidebar />
        <main className="flex-1 p-8">
          {children}
        </main>
      </div>
    </div>
  );
}

function App() {
  return (
    <BrowserRouter>
      <ThemeProvider>
        <AuthProvider>
          <Routes>
            <Route path="/login" element={<Login />} />
            
            <Route path="/dashboard" element={<Layout />}>
              <Route index element={<Dashboard />} />
              <Route path="profile" element={<Profile />} />
              <Route path="settings" element={<Settings />} />
            </Route>
            
            <Route path="/" element={<Navigate to="/dashboard" replace />} />
            <Route path="*" element={<Navigate to="/login" replace />} />
          </Routes>
        </AuthProvider>
      </ThemeProvider>
    </BrowserRouter>
  );
}

export default App;
```

### Step 8: Add Tailwind CSS Configuration

Add to `tailwind.config.js`:
```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  darkMode: 'class',
  theme: {
    extend: {},
  },
  plugins: [],
}
```

Add to `src/index.css`:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

.dark {
  color-scheme: dark;
}

.dark .bg-white {
  background-color: #1f2937;
}

.dark .text-gray-900 {
  color: #f3f4f6;
}

.dark .text-gray-700 {
  color: #d1d5db;
}
```

### Step 9: Key Concepts Demonstrated

**useRef:**
- DOM element references
- Focus management
- Mutable values without re-renders

**Performance Hooks:**
- useMemo for expensive calculations
- useCallback for function memoization
- React.memo for component memoization

**Context API:**
- ThemeContext for theme management
- AuthContext for authentication
- Provider pattern
- Custom hooks for context consumption

**Custom Hooks:**
- useAuth for authentication
- useTheme for theme switching
- useLocalStorage for persistence

**Performance Optimization:**
- Memoized calculations
- Stable function references
- Efficient re-render management

---

## Comprehensive Review

### Session Summary

In this session, we covered:

1. **useRef:** DOM references, focus management, previous values, mutable values, why ref changes don't cause re-renders
2. **Performance Hooks:** useMemo, useCallback, React.memo - differences and when to use each
3. **Context API:** useContext, Context Provider, Context Consumer, detailed explanation
4. **Custom Hooks:** useAuth, useTheme, useLocalStorage, useDebounce
5. **Performance Optimization:** Re-rendering, referential equality, common mistakes
6. **Practical Project:** Dashboard with theme switcher, authentication, and settings

### Key Takeaways

- useRef stores mutable values without triggering re-renders
- useMemo memoizes values, useCallback memoizes functions, React.memo memoizes components
- Context API solves prop drilling and enables global state
- Custom hooks encapsulate reusable logic
- Performance optimization requires understanding when to optimize
- Avoid premature optimization - measure first

---

## 15 Student Questions

1. What is the difference between useState and useRef?
2. Why doesn't changing a ref trigger a re-render?
3. When should you use useMemo instead of regular calculation?
4. What is the difference between useMemo and useCallback?
5. When should you use React.memo?
6. What is prop drilling and how does Context API solve it?
7. How do you create a custom hook?
8. When should you use Context API?
9. What are the performance implications of using Context API?
10. What triggers a component re-render?
11. What is referential equality in React?
12. How can you prevent unnecessary re-renders?
13. What are common performance mistakes in React?
14. How do you persist state using localStorage?
15. What is the purpose of custom hooks?

---

## 5 Interview Questions

### 1. Explain the difference between useMemo, useCallback, and React.memo.

**Answer:**
- **useMemo:** Memoizes a computed value. Returns the cached value unless dependencies change. Used for expensive calculations, filtering, sorting.
- **useCallback:** Memoizes a function. Returns the same function instance unless dependencies change. Used for functions passed as props or in useEffect dependencies.
- **React.memo:** Memoizes a component. Prevents re-render unless props change. Used for expensive child components with stable props.

Example:
```typescript
// useMemo - memoize value
const filtered = useMemo(() => items.filter(item => item.active), [items]);

// useCallback - memoize function
const handleClick = useCallback(() => setCount(c => c + 1), []);

// React.memo - memoize component
const MemoizedComponent = React.memo(Component);
```

### 2. Why doesn't changing a ref trigger a re-render?

**Answer:**
React only re-renders components when state or props change. Refs are plain JavaScript objects `{ current: value }` that React doesn't track. When you update `ref.current`, React has no way of knowing it changed, so no re-render occurs. This is intentional - refs are designed for values that don't need to trigger UI updates.

State changes:
```typescript
setState(5) → React detects → Re-render → UI updates
```

Ref changes:
```typescript
ref.current = 5 → React doesn't detect → No re-render → UI doesn't update
```

Use refs when you need mutable values without re-renders (DOM references, timers, previous values).

### 3. When should you use Context API vs props?

**Answer:**
Use **Context API** when:
- Data is needed by many components at different nesting levels
- Data represents global state (theme, auth, language)
- You want to avoid prop drilling
- Data changes infrequently

Use **Props** when:
- Data is local to a component tree
- Data changes frequently
- You need clear data flow
- The component tree is shallow
- You want explicit data dependencies

Context is not a replacement for props - use it for truly global state, not for all state.

### 4. What are common performance mistakes in React?

**Answer:**
1. **Premature optimization:** Optimizing before measuring performance
2. **Creating functions in render:** New function on every render breaks memoization
3. **Not memoizing context values:** New object on every render causes all consumers to re-render
4. **Using index as key:** Causes issues with list updates
5. **Inline styles:** New object on every render
6. **Large contexts:** Everything in one context causes excessive re-renders
7. **Unnecessary state:** State for derived values
8. **Over-optimizing:** Memoizing cheap operations adds overhead

### 5. How do you create a custom hook and when should you create one?

**Answer:**
Create a custom hook when:
- You have reusable stateful logic
- Multiple components need the same logic
- You want to separate concerns
- Logic is complex and could be tested independently

Pattern:
```typescript
function useCustomHook(initialValue) {
  const [state, setState] = useState(initialValue);
  
  const doSomething = useCallback(() => {
    // Logic
  }, []);
  
  useEffect(() => {
    // Effect
  }, []);
  
  return { state, doSomething };
}
```

Custom hooks start with "use", can use other hooks, and return state/functions. They encapsulate logic and make it reusable across components.

---

## Exercises

### Exercise 1: Create useWindowSize Hook

Create a custom hook that tracks window size and re-renders when the window is resized.

**Requirements:**
- Return width and height
- Update on resize
- Clean up event listener
- Use TypeScript

### Exercise 2: Optimize a Product List

Take a product list component and optimize it using:
- React.memo for product cards
- useMemo for filtering
- useCallback for event handlers
- Measure performance improvement

### Exercise 3: Create Multi-Context App

Create an app with multiple contexts:
- ThemeContext (light/dark)
- LanguageContext (en/es/fr)
- UserContext (user data)
- Demonstrate context nesting

### Exercise 4: Implement usePrevious Hook

Create a usePrevious hook that stores the previous value of a prop or state, useful for detecting changes.

### Exercise 5: Optimize Context with Splitting

Take a large context with many values and split it into smaller, focused contexts to improve performance.

---

## Homework

### Reading Assignment
1. Read React documentation on hooks
2. Read about Context API best practices
3. Learn about React performance optimization

### Practice Exercises
1. **Refactor:** Take an existing component and add appropriate optimizations
2. **Create Hooks:** Build useWindowSize, useOnlineStatus, useMediaQuery
3. **Context:** Create NotificationContext with toast notifications
4. **Optimize:** Profile and optimize a slow component
5. **Memoization:** Add useMemo/useCallback where appropriate

### Research Project
Research and write a brief comparison (500 words) of different state management solutions:
- Context API
- Redux Toolkit
- Zustand
- Recoil
- Jotai

Include pros and cons of each and recommend use cases.

---

## Performance Challenge

### Advanced Performance Optimization Challenge

Build a complex dashboard that demonstrates advanced performance optimization techniques.

#### Requirements:

1. **Data Visualization:**
   - Large dataset (10,000+ items)
   - Charts and graphs
   - Filtering and sorting
   - Virtual scrolling
   - Memoized calculations

2. **Real-time Updates:**
   - WebSocket connection
   - Live data updates
   - Optimized re-renders
   - Efficient state updates
   - Batch updates

3. **Multiple Contexts:**
   - Separate contexts for different concerns
   - Optimize context values
   - Prevent unnecessary consumer re-renders
   - Context selector pattern

4. **Advanced Memoization:**
   - Deep memoization
   - Custom comparison functions
   - Memoized component trees
   - Render props optimization

5. **Code Splitting:**
   - Route-based code splitting
   - Lazy loading components
   - Suspense boundaries
   - Error boundaries
   - Loading states

#### Technical Requirements:
- Use all performance hooks appropriately
- Measure and document performance improvements
- Use React DevTools Profiler
- Implement proper cleanup
- Type-safe with TypeScript
- Handle all edge cases

#### Bonus Features:
- Add performance monitoring
- Implement render caching
- Create performance dashboard
- Add automated performance tests
- Document optimization decisions

#### Evaluation Criteria:
- Measurable performance improvements
- Proper use of optimization techniques
- Code quality and organization
- TypeScript type safety
- Documentation of optimizations
- Real-world applicability

This challenge will test your understanding of performance optimization and push you to apply advanced techniques in a complex scenario.

---

## Common Mistakes Summary

1. **Optimizing too early** - Measure first, optimize later
2. **Creating functions in render** - Use useCallback
3. **Not memoizing context values** - Use useMemo
4. **Using index as key** - Use unique IDs
5. **Inline styles** - Use CSS classes or memoize
6. **Large contexts** - Split into smaller contexts
7. **Unnecessary state** - Derive during render
8. **Over-optimizing** - Don't memoize cheap operations
9. **Forgetting cleanup** - Always cleanup effects
10. **Ignoring dependencies** - Include all dependencies

---

## Additional Resources

- [React Hooks Documentation](https://react.dev/reference/react)
- [React Context Documentation](https://react.dev/learn/scaling-up-with-context)
- [React Performance](https://react.dev/learn/render-and-commit)
- [React DevTools Profiler](https://react.dev/learn/react-developer-tools#profiling-components-with-the-react-profiler)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)

---

**Congratulations on completing Session 7!** You now have a solid understanding of performance optimization and Context API in React. Continue practicing with the exercises and homework to reinforce these concepts before moving to the next session.