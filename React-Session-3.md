# React.js Session 3: State Management with useState

**Duration:** 3 hours  
**Level:** Beginner  
**Prerequisites:** Session 1 & 2 completed (JSX, Components, Props, Events, Conditional Rendering, Lists)

---

## Session Timeline

### Part 1: State Fundamentals (45 minutes)
- **0:00-0:05:** What is State?, State vs Variables (Topics 1-2)
- **0:05-0:10:** Props vs State, Why React State exists (Topics 3-4)
- **0:10-0:15:** useState Hook, useState Syntax (Topics 5-6)
- **0:15-0:20:** Initial State, Updating State (Topics 7-8)
- **0:20-0:25:** Re-rendering, State lifecycle (Topics 9-10)
- **0:25-0:45:** State Fundamentals Practice Exercises

### Part 2: State Types (45 minutes)
- **0:45-0:50:** String State (Topic 11)
- **0:50-0:55:** Number State (Topic 12)
- **0:55-1:00:** Boolean State (Topic 13)
- **1:00-1:10:** Array State (Topic 14)
- **1:10-1:20:** Object State (Topic 15)
- **1:20-1:30:** State Types Practice Exercises

### Part 3: State Updates (45 minutes)
- **1:30-1:35:** Updating Objects (Topic 16)
- **1:35-1:40:** Updating Arrays (Topic 17)
- **1:40-1:50:** Functional State Updates, Previous State (Topics 18-19)
- **1:50-1:55:** Multiple State Variables (Topic 20)
- **1:55-2:10:** State Updates Practice Exercises

### Part 4: State Patterns (30 minutes)
- **2:10-2:15:** Derived State (Topic 21)
- **2:15-2:20:** State Duplication (Topic 22)
- **2:20-2:25:** Single Source of Truth (Topic 23)
- **2:25-2:30:** Lifting State Up (Topic 24)
- **2:30-2:35:** Common State mistakes (Topic 25)

### Part 5: Shopping Cart Project (45 minutes)
- **2:35-2:40:** Project Setup and Structure
- **2:40-2:50:** Product Component and Data
- **2:50-3:00:** Cart Component and State Management
- **3:00-3:10:** Add/Remove/Update Quantity Functions
- **3:10-3:20:** Total Price Calculation and Cart Count
- **3:20-3:30:** Prop Drilling Demonstration and Final Integration

---

## Part 1: State Fundamentals

### 1. What is State?

**What is it?**
State in React is a built-in object that allows components to create and manage their own data. It represents the internal data of a component that can change over time and affects the component's rendering.

**Why we need it?**
- Components need to remember information between renders
- User interactions require data to change
- Dynamic content needs to be managed
- Data drives the UI
- Enables interactive applications

**How it works?**
- State is managed within components using hooks
- When state changes, React re-renders the component
- State is private to the component (unless passed via props)
- State changes trigger UI updates automatically

**Syntax:**
```typescript
const [state, setState] = useState(initialValue);
```

**Simple Example:**
```typescript
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

**Real-world Example:**
```typescript
function UserProfile() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [userName, setUserName] = useState('');
  const [userEmail, setUserEmail] = useState('');

  const handleLogin = (name: string, email: string) => {
    setUserName(name);
    setUserEmail(email);
    setIsLoggedIn(true);
  };

  const handleLogout = () => {
    setUserName('');
    setUserEmail('');
    setIsLoggedIn(false);
  };

  return (
    <div>
      {isLoggedIn ? (
        <div>
          <h2>Welcome, {userName}!</h2>
          <p>Email: {userEmail}</p>
          <button onClick={handleLogout}>Logout</button>
        </div>
      ) : (
        <LoginForm onLogin={handleLogin} />
      )}
    </div>
  );
}
```

**Common Errors:**
- Mutating state directly instead of using setState
- Not understanding that state changes are asynchronous
- Using state in calculations without considering it might be stale
- Initializing state with expensive operations

**Best Practices:**
- Initialize state with simple values when possible
- Use setState functions, never mutate state directly
- Keep state minimal and focused
- Understand that state updates trigger re-renders

---

### 2. State vs Variables

**What is it?**
Understanding the crucial difference between React state and regular JavaScript variables, and when to use each.

**Why we need it?**
- Variables don't trigger re-renders
- State is React's way to manage changing data
- Understanding this prevents common bugs
- Helps in choosing the right tool for the job
- Essential for React component behavior

**How it works?**
- Regular variables: changed values don't update UI
- State: changes trigger component re-render
- Variables: reset on every render
- State: persists between renders

**Syntax Comparison:**
```typescript
// Regular variable - doesn't trigger re-render
let count = 0;
count = count + 1; // UI won't update

// State - triggers re-render
const [count, setCount] = useState(0);
setCount(count + 1); // UI updates
```

**Simple Example:**
```typescript
function VariableVsState() {
  // Regular variable - resets on every render
  let variableCount = 0;
  
  // State - persists between renders
  const [stateCount, setStateCount] = useState(0);

  const handleVariableClick = () => {
    variableCount = variableCount + 1;
    console.log('Variable count:', variableCount); // Updates in console
    // But UI won't show the change
  };

  const handleStateClick = () => {
    setStateCount(stateCount + 1);
    // UI will update to show new count
  };

  return (
    <div>
      <div>
        <p>Variable Count: {variableCount}</p>
        <button onClick={handleVariableClick}>Increment Variable</button>
      </div>
      <div>
        <p>State Count: {stateCount}</p>
        <button onClick={handleStateClick}>Increment State</button>
      </div>
    </div>
  );
}
```

**Real-world Example:**
```typescript
function TimerComponent() {
  // ❌ Wrong: Using variable for timer
  let seconds = 0;
  
  useEffect(() => {
    const interval = setInterval(() => {
      seconds = seconds + 1; // This won't update the UI
    }, 1000);
    return () => clearInterval(interval);
  }, []);

  return <div>Seconds: {seconds}</div>; // Always shows 0

  // ✅ Correct: Using state for timer
  const [seconds, setSeconds] = useState(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(prev => prev + 1); // This updates the UI
    }, 1000);
    return () => clearInterval(interval);
  }, []);

  return <div>Seconds: {seconds}</div>; // Updates every second
}
```

**When to Use Variables:**
- Temporary calculations within a render
- Values that don't need to persist between renders
- Derived values that can be calculated from props/state
- Event handler local variables
- Loop counters

**When to Use State:**
- Data that changes over time
- User input
- Data that affects rendering
- Values that need to persist between renders
- Component-internal data

**Common Errors:**
- Using variables when state is needed
- Using state when a variable would suffice
- Expecting variable changes to update the UI
- Not understanding the render cycle

**Best Practices:**
- Use state for data that affects rendering
- Use variables for temporary calculations
- Derive values from state/props when possible
- Keep state minimal to avoid unnecessary re-renders

---

### 3. Props vs State

**What is it?**
Understanding the distinction between props (external data) and state (internal data) in React components.

**Why we need it?**
- Props are for data flow from parent to child
- State is for component-internal data
- Confusion leads to architectural problems
- Essential for component design
- Determines data ownership

**How it works?**
- Props: passed from parent, read-only in child
- State: managed within component, can be changed
- Props: external configuration
- State: internal memory

**Syntax Comparison:**
```typescript
// Props - received from parent
function Child({ propValue }: { propValue: string }) {
  return <div>{propValue}</div>;
}

// State - managed internally
function Parent() {
  const [stateValue, setStateValue] = useState('initial');
  return <Child propValue={stateValue} />;
}
```

**Simple Example:**
```typescript
function Counter({ initialCount }: { initialCount: number }) {
  // Props: received from parent, can't be changed directly
  const [count, setCount] = useState(initialCount); // Initialize with prop
  
  // State: managed internally, can be changed
  const increment = () => setCount(count + 1);
  
  return (
    <div>
      <p>Initial (prop): {initialCount}</p>
      <p>Current (state): {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

function App() {
  return <Counter initialCount={5} />;
}
```

**Real-world Example:**
```typescript
interface UserCardProps {
  // Props: external data passed from parent
  user: {
    id: number;
    name: string;
    email: string;
  };
  onEdit: (userId: number) => void;
  onDelete: (userId: number) => void;
}

function UserCard({ user, onEdit, onDelete }: UserCardProps) {
  // State: internal UI state
  const [isExpanded, setIsExpanded] = useState(false);
  const [showConfirmDelete, setShowConfirmDelete] = useState(false);

  const handleToggleExpand = () => {
    setIsExpanded(!isExpanded); // Changing internal state
  };

  const handleEdit = () => {
    onEdit(user.id); // Using prop callback
  };

  const handleDelete = () => {
    onDelete(user.id); // Using prop callback
    setShowConfirmDelete(false);
  };

  return (
    <div className="user-card">
      <div className="user-header">
        <h3>{user.name}</h3>
        <button onClick={handleToggleExpand}>
          {isExpanded ? '▼' : '▶'}
        </button>
      </div>
      
      {isExpanded && (
        <div className="user-details">
          <p>Email: {user.email}</p>
          <div className="user-actions">
            <button onClick={handleEdit}>Edit</button>
            <button onClick={() => setShowConfirmDelete(true)}>
              Delete
            </button>
          </div>
        </div>
      )}

      {showConfirmDelete && (
        <div className="confirm-dialog">
          <p>Are you sure you want to delete {user.name}?</p>
          <button onClick={handleDelete}>Confirm</button>
          <button onClick={() => setShowConfirmDelete(false)}>Cancel</button>
        </div>
      )}
    </div>
  );
}
```

**Key Differences:**

| Aspect | Props | State |
|--------|-------|-------|
| Source | Passed from parent | Managed within component |
| Mutability | Read-only in child | Can be changed |
| Purpose | External configuration | Internal memory |
| Changes | Triggered by parent | Triggered by component |
| Scope | Available to child component | Private to component |

**Common Errors:**
- Trying to modify props directly
- Using state when props would suffice
- Not lifting state up when needed
- Confusing data ownership

**Best Practices:**
- Use props for data that comes from parent
- Use state for component-internal data
- Keep components pure when possible
- Lift state up when siblings need to share data
- Make data flow clear and predictable

---

### 4. Why React State exists

**What is it?**
Understanding the fundamental reasons why React introduced its own state management system instead of relying on regular JavaScript variables.

**Why we need it?**
- Automatic UI updates when data changes
- Declarative programming model
- Predictable component behavior
- Efficient rendering through virtual DOM
- Clear data flow architecture

**How it works?**
- State changes trigger re-renders
- React's virtual DOM efficiently updates the actual DOM
- State changes are batched for performance
- Provides a clear mental model for data flow

**Without React State:**
```typescript
// ❌ Manual DOM manipulation (old way)
function counterWithoutState() {
  let count = 0;
  const element = document.getElementById('counter');
  
  function updateDisplay() {
    element.textContent = count;
  }
  
  document.getElementById('increment').addEventListener('click', () => {
    count++;
    updateDisplay(); // Manual update required
  });
}
```

**With React State:**
```typescript
// ✅ Declarative React state
function CounterWithState() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
  // UI updates automatically when state changes
}
```

**Real-world Example:**
```typescript
// Without React state - complex manual updates
function formWithoutState() {
  let formData = {
    name: '',
    email: '',
    isValid: false
  };
  
  function updateName(value: string) {
    formData.name = value;
    validateForm(); // Manual validation call
    updateUI(); // Manual UI update
  }
  
  function updateEmail(value: string) {
    formData.email = value;
    validateForm(); // Manual validation call
    updateUI(); // Manual UI update
  }
  
  function validateForm() {
    formData.isValid = formData.name.length > 0 && 
                      formData.email.includes('@');
  }
  
  function updateUI() {
    document.getElementById('name').value = formData.name;
    document.getElementById('email').value = formData.email;
    document.getElementById('submit').disabled = !formData.isValid;
  }
}

// With React state - automatic updates
function FormWithState() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    isValid: false
  });
  
  const updateName = (value: string) => {
    setFormData(prev => ({
      ...prev,
      name: value,
      isValid: value.length > 0 && prev.email.includes('@')
    }));
    // UI updates automatically
  };
  
  const updateEmail = (value: string) => {
    setFormData(prev => ({
      ...prev,
      email: value,
      isValid: prev.name.length > 0 && value.includes('@')
    }));
    // UI updates automatically
  };
  
  return (
    <form>
      <input 
        value={formData.name}
        onChange={(e) => updateName(e.target.value)}
      />
      <input 
        value={formData.email}
        onChange={(e) => updateEmail(e.target.value)}
      />
      <button disabled={!formData.isValid}>Submit</button>
    </form>
  );
}
```

**Benefits of React State:**
1. **Declarative:** Describe what UI should look like, not how to update it
2. **Automatic:** UI updates automatically when state changes
3. **Predictable:** Clear data flow and component behavior
4. **Efficient:** Virtual DOM optimizes updates
5. **Maintainable:** Easier to reason about and debug

**Common Errors:**
- Trying to manually manipulate DOM when state would suffice
- Not understanding that state drives the UI
- Mutating state directly instead of using setState
- Overusing state when derived values would work

**Best Practices:**
- Let state drive your UI
- Use setState for all state changes
- Derive values from state when possible
- Trust React's rendering system
- Focus on what to render, not how to update

---

### 5. useState

**What is it?**
useState is a React Hook that lets you add state to functional components. It's the primary way to manage component-internal state.

**Why we need it?**
- Functional components couldn't have state before hooks
- Provides a clean, simple API for state management
- Enables state in functional components
- Replaces class component state
- Foundation for other hooks

**How it works?**
- Returns an array with current state and setter function
- Initial value is set only on first render
- Setter function triggers re-render
- Can hold any type of data

**Syntax:**
```typescript
const [state, setState] = useState(initialValue);
```

**Simple Example:**
```typescript
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

**Real-world Example:**
```typescript
import { useState } from 'react';

function TaskManager() {
  const [tasks, setTasks] = useState<string[]>([]);
  const [newTask, setNewTask] = useState('');
  const [filter, setFilter] = useState<'all' | 'active' | 'completed'>('all');

  const addTask = () => {
    if (newTask.trim()) {
      setTasks([...tasks, { text: newTask, completed: false }]);
      setNewTask('');
    }
  };

  const toggleTask = (index: number) => {
    setTasks(tasks.map((task, i) => 
      i === index ? { ...task, completed: !task.completed } : task
    ));
  };

  const deleteTask = (index: number) => {
    setTasks(tasks.filter((_, i) => i !== index));
  };

  const filteredTasks = tasks.filter(task => {
    if (filter === 'active') return !task.completed;
    if (filter === 'completed') return task.completed;
    return true;
  });

  return (
    <div>
      <input 
        value={newTask}
        onChange={(e) => setNewTask(e.target.value)}
        placeholder="Add new task"
      />
      <button onClick={addTask}>Add Task</button>
      
      <div>
        <button onClick={() => setFilter('all')}>All</button>
        <button onClick={() => setFilter('active')}>Active</button>
        <button onClick={() => setFilter('completed')}>Completed</button>
      </div>
      
      <ul>
        {filteredTasks.map((task, index) => (
          <li key={index}>
            <input 
              type="checkbox" 
              checked={task.completed}
              onChange={() => toggleTask(index)}
            />
            <span style={{ textDecoration: task.completed ? 'line-through' : 'none' }}>
              {task.text}
            </span>
            <button onClick={() => deleteTask(index)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

**useState Patterns:**

1. **Simple state:**
```typescript
const [count, setCount] = useState(0);
```

2. **Object state:**
```typescript
const [user, setUser] = useState({ name: '', email: '' });
```

3. **Array state:**
```typescript
const [items, setItems] = useState<string[]>([]);
```

4. **Lazy initialization:**
```typescript
const [expensiveValue, setExpensiveValue] = useState(() => 
  computeExpensiveValue()
);
```

5. **Functional updates:**
```typescript
const [count, setCount] = useState(0);
setCount(prev => prev + 1);
```

**Common Errors:**
- Not importing useState from 'react'
- Calling useState outside component/function
- Mutating state directly instead of using setState
- Not understanding the array destructuring syntax

**Best Practices:**
- Always import useState from 'react'
- Call useState at the top level of component
- Use setState, never mutate state directly
- Use functional updates when new state depends on old state
- Consider lazy initialization for expensive initial values

---

### 6. useState Syntax

**What is it?**
The specific syntax and patterns for using useState in React components with TypeScript.

**Why we need it?**
- Proper syntax prevents errors
- TypeScript provides type safety
- Understanding syntax enables proper usage
- Different patterns for different use cases
- Foundation for advanced state management

**How it works?**
- Array destructuring extracts state and setter
- TypeScript generics for type safety
- Initial value determines state type
- Setter function follows naming convention

**Syntax:**
```typescript
// Basic syntax
const [state, setState] = useState<Type>(initialValue);

// Type inference
const [count, setCount] = useState(0); // Type inferred as number

// Explicit typing
const [count, setCount] = useState<number>(0);

// Union types
const [status, setStatus] = useState<'idle' | 'loading' | 'success' | 'error'>('idle');

// Complex types
const [user, setUser] = useState<User | null>(null);
```

**Simple Example:**
```typescript
interface CounterProps {
  initialValue?: number;
}

function Counter({ initialValue = 0 }: CounterProps) {
  // Basic useState with type inference
  const [count, setCount] = useState(initialValue);
  
  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  const reset = () => setCount(initialValue);
  
  return (
    <div>
      <h2>Counter: {count}</h2>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
      <button onClick={increment}>+</button>
    </div>
  );
}
```

**Real-world Example:**
```typescript
interface User {
  id: number;
  name: string;
  email: string;
  role: 'admin' | 'user' | 'guest';
}

interface FormState {
  user: User | null;
  isLoading: boolean;
  error: string | null;
  formData: {
    name: string;
    email: string;
    role: User['role'];
  };
}

function UserForm() {
  // Complex state with explicit typing
  const [state, setState] = useState<FormState>({
    user: null,
    isLoading: false,
    error: null,
    formData: {
      name: '',
      email: '',
      role: 'user'
    }
  });

  const handleInputChange = (field: keyof FormState['formData'], value: string) => {
    setState(prev => ({
      ...prev,
      formData: {
        ...prev.formData,
        [field]: value
      }
    }));
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setState(prev => ({ ...prev, isLoading: true, error: null }));

    try {
      const response = await fetch('/api/users', {
        method: 'POST',
        body: JSON.stringify(state.formData)
      });
      
      if (!response.ok) throw new Error('Submission failed');
      
      const user = await response.json();
      setState(prev => ({
        ...prev,
        user,
        isLoading: false,
        formData: { name: '', email: '', role: 'user' }
      }));
    } catch (error) {
      setState(prev => ({
        ...prev,
        isLoading: false,
        error: error instanceof Error ? error.message : 'An error occurred'
      }));
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        value={state.formData.name}
        onChange={(e) => handleInputChange('name', e.target.value)}
        placeholder="Name"
      />
      <input
        value={state.formData.email}
        onChange={(e) => handleInputChange('email', e.target.value)}
        placeholder="Email"
      />
      <select
        value={state.formData.role}
        onChange={(e) => handleInputChange('role', e.target.value)}
      >
        <option value="user">User</option>
        <option value="admin">Admin</option>
        <option value="guest">Guest</option>
      </select>
      
      {state.isLoading && <p>Submitting...</p>}
      {state.error && <p className="error">{state.error}</p>}
      {state.user && <p>Success! Created user: {state.user.name}</p>}
      
      <button type="submit" disabled={state.isLoading}>
        {state.isLoading ? 'Submitting...' : 'Submit'}
      </button>
    </form>
  );
}
```

**Advanced Syntax Patterns:**

1. **Lazy Initialization:**
```typescript
const [expensiveValue, setExpensiveValue] = useState(() => {
  console.log('Computed only once');
  return computeExpensiveInitialValue();
});
```

2. **Multiple useState calls:**
```typescript
const [name, setName] = useState('');
const [email, setEmail] = useState('');
const [age, setAge] = useState(0);
```

3. **Generic type parameters:**
```typescript
function useCustomState<T>(initial: T) {
  return useState<T>(initial);
}

const [data, setData] = useCustomState<string>('initial');
```

**Common Errors:**
- Not using TypeScript generics when needed
- Incorrect type definitions
- Not handling null/undefined in types
- Confusing type inference with explicit typing

**Best Practices:**
- Let TypeScript infer types when possible
- Use explicit types for complex state
- Handle null/undefined in type definitions
- Use union types for limited options
- Keep type definitions close to usage

---

### 7. Initial State

**What is it?**
The initial value passed to useState that sets the starting state of a component. This value is only used during the first render.

**Why we need it?**
- Components need starting values
- Prevents undefined state
- Sets up component initial configuration
- Determines state type through inference
- Essential for component behavior

**How it works?**
- Passed as argument to useState
- Only used on first render
- Can be any valid JavaScript value
- Type is inferred from initial value
- Can be computed lazily for expensive operations

**Syntax:**
```typescript
// Direct value
const [count, setCount] = useState(0);

// Lazy initialization
const [expensiveValue, setExpensiveValue] = useState(() => 
  computeExpensiveValue()
);
```

**Simple Example:**
```typescript
function Counter() {
  // Initial state set to 0
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

**Real-world Example:**
```typescript
function UserProfile({ userId }: { userId: number }) {
  // Initial state with different patterns
  const [user, setUser] = useState<User | null>(null);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  const [editMode, setEditMode] = useState(false);
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    bio: ''
  });

  // Lazy initialization for expensive computation
  const [computedData, setComputedData] = useState(() => {
    console.log('Computing initial data...');
    return performExpensiveComputation();
  });

  useEffect(() => {
    fetchUser(userId)
      .then(data => {
        setUser(data);
        setFormData({
          name: data.name,
          email: data.email,
          bio: data.bio || ''
        });
      })
      .catch(err => setError(err.message))
      .finally(() => setIsLoading(false));
  }, [userId]);

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!user) return <div>User not found</div>;

  return (
    <div>
      {editMode ? (
        <form>
          <input
            value={formData.name}
            onChange={(e) => setFormData({...formData, name: e.target.value})}
          />
          <input
            value={formData.email}
            onChange={(e) => setFormData({...formData, email: e.target.value})}
          />
          <textarea
            value={formData.bio}
            onChange={(e) => setFormData({...formData, bio: e.target.value})}
          />
          <button onClick={() => setEditMode(false)}>Cancel</button>
          <button onClick={() => {/* save logic */}}>Save</button>
        </form>
      ) : (
        <div>
          <h2>{user.name}</h2>
          <p>{user.email}</p>
          <p>{user.bio}</p>
          <button onClick={() => setEditMode(true)}>Edit Profile</button>
        </div>
      )}
    </div>
  );
}
```

**Initial State Patterns:**

1. **Simple values:**
```typescript
const [count, setCount] = useState(0);
const [name, setName] = useState('');
const [isActive, setIsActive] = useState(false);
```

2. **Complex objects:**
```typescript
const [user, setUser] = useState({
  id: 0,
  name: '',
  email: '',
  preferences: {
    theme: 'light',
    notifications: true
  }
});
```

3. **Arrays:**
```typescript
const [items, setItems] = useState<string[]>([]);
const [numbers, setNumbers] = useState([1, 2, 3]);
```

4. **Null/undefined:**
```typescript
const [data, setData] = useState<Data | null>(null);
const [error, setError] = useState<string | undefined>(undefined);
```

5. **Lazy initialization:**
```typescript
const [largeDataset, setLargeDataset] = useState(() => 
  loadLargeDatasetFromStorage()
);
```

**Common Errors:**
- Using expensive computations in direct initialization
- Not handling null/undefined initial states
- Incorrect initial state types
- Mutating initial state objects
- Not considering default values for optional props

**Best Practices:**
- Use lazy initialization for expensive operations
- Set appropriate null/undefined initial states
- Match initial state type with expected usage
- Keep initial state simple when possible
- Consider default props for configuration

---

### 8. Updating State

**What is it?**
The process of changing component state using the setter function provided by useState, which triggers component re-rendering.

**Why we need it?**
- Components need to respond to changes
- User interactions require state updates
- Data changes over time
- UI needs to reflect current state
- Enables interactive applications

**How it works?**
- Call setter function with new value
- React schedules a re-render
- New state is used in next render
- Updates are batched for performance
- Previous state is available during update

**Syntax:**
```typescript
// Direct update
setState(newValue);

// Functional update
setState(prevState => prevState + 1);
```

**Simple Example:**
```typescript
function Counter() {
  const [count, setCount] = useState(0);
  
  const increment = () => {
    setCount(count + 1); // Direct update
  };
  
  const decrement = () => {
    setCount(prev => prev - 1); // Functional update
  };
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </div>
  );
}
```

**Real-world Example:**
```typescript
function TodoApp() {
  const [todos, setTodos] = useState<Todo[]>([]);
  const [newTodo, setNewTodo] = useState('');
  const [filter, setFilter] = useState<'all' | 'active' | 'completed'>('all');

  // Add new todo
  const addTodo = () => {
    if (newTodo.trim()) {
      setTodos(prev => [
        ...prev,
        {
          id: Date.now(),
          text: newTodo,
          completed: false,
          createdAt: new Date()
        }
      ]);
      setNewTodo('');
    }
  };

  // Toggle todo completion
  const toggleTodo = (id: number) => {
    setTodos(prev => 
      prev.map(todo => 
        todo.id === id 
          ? { ...todo, completed: !todo.completed }
          : todo
      )
    );
  };

  // Delete todo
  const deleteTodo = (id: number) => {
    setTodos(prev => prev.filter(todo => todo.id !== id));
  };

  // Update todo text
  const updateTodoText = (id: number, newText: string) => {
    setTodos(prev =>
      prev.map(todo =>
        todo.id === id
          ? { ...todo, text: newText }
          : todo
      )
    );
  };

  // Clear completed todos
  const clearCompleted = () => {
    setTodos(prev => prev.filter(todo => !todo.completed));
  };

  // Filter todos
  const filteredTodos = todos.filter(todo => {
    if (filter === 'active') return !todo.completed;
    if (filter === 'completed') return todo.completed;
    return true;
  });

  return (
    <div>
      <div className="todo-input">
        <input
          value={newTodo}
          onChange={(e) => setNewTodo(e.target.value)}
          placeholder="Add a new todo"
          onKeyPress={(e) => e.key === 'Enter' && addTodo()}
        />
        <button onClick={addTodo}>Add</button>
      </div>

      <div className="todo-filters">
        <button 
          className={filter === 'all' ? 'active' : ''}
          onClick={() => setFilter('all')}
        >
          All
        </button>
        <button 
          className={filter === 'active' ? 'active' : ''}
          onClick={() => setFilter('active')}
        >
          Active
        </button>
        <button 
          className={filter === 'completed' ? 'active' : ''}
          onClick={() => setFilter('completed')}
        >
          Completed
        </button>
      </div>

      <ul className="todo-list">
        {filteredTodos.map(todo => (
          <li key={todo.id} className={todo.completed ? 'completed' : ''}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => toggleTodo(todo.id)}
            />
            <span
              contentEditable
              onBlur={(e) => updateTodoText(todo.id, e.currentTarget.textContent || '')}
            >
              {todo.text}
            </span>
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>

      {todos.some(todo => todo.completed) && (
        <button onClick={clearCompleted}>Clear Completed</button>
      )}
    </div>
  );
}
```

**State Update Patterns:**

1. **Replacing state:**
```typescript
setValue(newValue);
```

2. **Functional update:**
```typescript
setValue(prev => prev + 1);
```

3. **Object update:**
```typescript
setUser(prev => ({ ...prev, name: newName }));
```

4. **Array update:**
```typescript
setItems(prev => [...prev, newItem]);
```

5. **Conditional update:**
```typescript
setValue(prev => shouldUpdate ? newValue : prev);
```

**Common Errors:**
- Mutating state directly instead of using setter
- Not using functional updates when depending on previous state
- Expecting immediate state updates
- Calling setter outside component/hook
- Incorrectly updating nested state

**Best Practices:**
- Always use setter function, never mutate directly
- Use functional updates when new state depends on old state
- Understand that state updates are asynchronous
- Keep update logic simple and predictable
- Use immutable patterns for objects and arrays

---

### 9. Re-rendering

**What is it?**
The process React uses to update the UI when state or props change, involving virtual DOM comparison and efficient DOM updates.

**Why we need it?**
- UI needs to reflect current state
- User interactions require visual feedback
- Data changes need to be displayed
- Application needs to be responsive
- Enables dynamic interfaces

**How it works?**
- State/prop change triggers re-render
- React creates new virtual DOM
- Compares with previous virtual DOM
- Calculates minimal DOM changes
- Updates actual DOM efficiently

**Simple Example:**
```typescript
function Counter() {
  const [count, setCount] = useState(0);
  
  console.log('Component rendered'); // Logs on every render
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment (triggers re-render)
      </button>
    </div>
  );
}
```

**Real-world Example:**
```typescript
function DataComponent() {
  const [data, setData] = useState(null);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);
  const renderCount = useRef(0);

  // Track renders
  renderCount.current++;
  console.log(`Component rendered ${renderCount.current} times`);

  const fetchData = async () => {
    setIsLoading(true);
    setError(null);
    
    try {
      const response = await fetch('/api/data');
      const result = await response.json();
      setData(result); // Triggers re-render
    } catch (err) {
      setError(err.message); // Triggers re-render
    } finally {
      setIsLoading(false); // Triggers re-render
    }
  };

  return (
    <div>
      <div className="debug-info">
        <p>Render count: {renderCount.current}</p>
        <p>Loading: {isLoading ? 'Yes' : 'No'}</p>
        <p>Error: {error || 'None'}</p>
        <p>Data: {data ? 'Loaded' : 'None'}</p>
      </div>

      {isLoading && <div>Loading...</div>}
      
      {error && <div className="error">Error: {error}</div>}
      
      {data && (
        <div>
          <h2>Data Loaded</h2>
          <pre>{JSON.stringify(data, null, 2)}</pre>
        </div>
      )}
      
      <button onClick={fetchData}>
        Fetch Data (triggers multiple re-renders)
      </button>
    </div>
  );
}
```

**Re-render Triggers:**

1. **State changes:**
```typescript
const [count, setCount] = useState(0);
setCount(1); // Triggers re-render
```

2. **Prop changes:**
```typescript
function Child({ value }: { value: number }) {
  return <div>{value}</div>;
}
// Parent changing value prop triggers Child re-render
```

3. **Parent re-renders:**
```typescript
function Parent() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <Child /> {/* Re-renders when Parent re-renders */}
    </div>
  );
}
```

4. **Context changes:**
```typescript
const ThemeContext = createContext('light');
// Context value change triggers consumer re-renders
```

**Optimizing Re-renders:**

1. **React.memo:**
```typescript
const ExpensiveChild = React.memo(function ExpensiveChild({ data }) {
  // Only re-renders when data prop changes
  return <div>{/* expensive rendering */}</div>;
});
```

2. **useMemo:**
```typescript
const expensiveValue = useMemo(() => 
  computeExpensiveValue(data),
  [data]
);
```

3. **useCallback:**
```typescript
const handleClick = useCallback(() => {
  doSomething(dependency);
}, [dependency]);
```

**Common Errors:**
- Causing unnecessary re-renders
- Not understanding when re-renders occur
- Optimizing prematurely
- Creating functions in render (causes child re-renders)
- Not using memoization for expensive operations

**Best Practices:**
- Understand what triggers re-renders
- Optimize only when performance issues exist
- Use React.memo for expensive components
- Use useMemo/useCallback for expensive computations
- Keep render functions pure

---

### 10. State lifecycle (Simplified)

**What is it?**
The simplified lifecycle of state in a functional component, from initialization through updates to component unmounting.

**Why we need it?**
- Understand when state is created and destroyed
- Know when state updates are processed
- Handle side effects at the right time
- Prevent memory leaks
- Manage component lifecycle properly

**How it works?**
- State initialized on first render
- Updates are queued and processed
- State persists between renders
- State is destroyed when component unmounts
- Side effects handled through useEffect

**Simple Lifecycle:**
```typescript
function Component() {
  // 1. Initialization (first render only)
  const [state, setState] = useState(initialValue);
  
  // 2. Render with current state
  return <div>{state}</div>;
  
  // 3. State update triggers re-render
  // setState(newValue);
  
  // 4. Cleanup on unmount
  useEffect(() => {
    return () => {
      // Cleanup logic
    };
  }, []);
}
```

**Real-world Example:**
```typescript
function Timer() {
  const [seconds, setSeconds] = useState(0);
  const [isRunning, setIsRunning] = useState(false);
  const intervalRef = useRef<NodeJS.Timeout | null>(null);

  // State lifecycle phases:
  
  // 1. Initialization
  useEffect(() => {
    console.log('Component mounted, state initialized');
    console.log('Initial state:', { seconds, isRunning });
  }, []);

  // 2. State updates and re-renders
  useEffect(() => {
    console.log('State changed:', { seconds, isRunning });
  }, [seconds, isRunning]);

  // 3. Start/stop timer based on state
  useEffect(() => {
    if (isRunning) {
      console.log('Starting timer...');
      intervalRef.current = setInterval(() => {
        setSeconds(prev => {
          console.log('Updating seconds:', prev + 1);
          return prev + 1;
        });
      }, 1000);
    } else {
      console.log('Stopping timer...');
      if (intervalRef.current) {
        clearInterval(intervalRef.current);
        intervalRef.current = null;
      }
    }

    // 4. Cleanup on unmount or dependency change
    return () => {
      console.log('Cleaning up timer...');
      if (intervalRef.current) {
        clearInterval(intervalRef.current);
      }
    };
  }, [isRunning]);

  // 5. Component unmount cleanup
  useEffect(() => {
    return () => {
      console.log('Component unmounting, cleaning up...');
      if (intervalRef.current) {
        clearInterval(intervalRef.current);
      }
    };
  }, []);

  const toggleTimer = () => {
    setIsRunning(prev => {
      console.log('Toggling timer:', !prev);
      return !prev;
    });
  };

  const resetTimer = () => {
    console.log('Resetting timer to 0');
    setSeconds(0);
    setIsRunning(false);
  };

  return (
    <div>
      <h2>Timer: {seconds}s</h2>
      <p>Status: {isRunning ? 'Running' : 'Stopped'}</p>
      <button onClick={toggleTimer}>
        {isRunning ? 'Stop' : 'Start'}
      </button>
      <button onClick={resetTimer}>Reset</button>
    </div>
  );
}
```

**State Lifecycle Phases:**

1. **Initialization:**
```typescript
const [state, setState] = useState(initialValue);
// Happens once on first render
```

2. **Update Phase:**
```typescript
setState(newValue);
// Queues update, triggers re-render
// New state used in next render
```

3. **Render Phase:**
```typescript
return <div>{state}</div>;
// Component renders with current state
```

4. **Cleanup Phase:**
```typescript
useEffect(() => {
  return () => {
    // Cleanup when component unmounts
  };
}, []);
```

**Common State Lifecycle Issues:**

1. **Stale closures:**
```typescript
// ❌ Problem: Stale closure
useEffect(() => {
  const interval = setInterval(() => {
    console.log(count); // Always shows initial count
  }, 1000);
  return () => clearInterval(interval);
}, []); // Empty dependency array

// ✅ Solution: Functional update
useEffect(() => {
  const interval = setInterval(() => {
    setCount(prev => {
      console.log(prev); // Shows current count
      return prev + 1;
    });
  }, 1000);
  return () => clearInterval(interval);
}, []);
```

2. **Memory leaks:**
```typescript
// ❌ Problem: Not cleaning up
useEffect(() => {
  const interval = setInterval(() => setCount(c => c + 1), 1000);
  // No cleanup - memory leak if component unmounts
}, []);

// ✅ Solution: Proper cleanup
useEffect(() => {
  const interval = setInterval(() => setCount(c => c + 1), 1000);
  return () => clearInterval(interval);
}, []);
```

**Common Errors:**
- Not understanding when state updates occur
- Creating memory leaks with timers/subscriptions
- Stale closures in useEffect
- Not cleaning up side effects
- Assuming synchronous state updates

**Best Practices:**
- Understand the render cycle
- Always cleanup side effects
- Use functional updates to avoid stale closures
- Be aware of asynchronous state updates
- Test component mounting/unmounting

---

## Part 2: State Types

### 11. String State

**What is it?**
State that holds string values, used for text data like names, messages, search queries, and user input.

**Why we need it?**
- Most common form of user input
- Text content needs to be managed
- Search and filtering functionality
- Form input handling
- Display dynamic text

**How it works?**
- Initialize with string value
- Update with new string values
- Can be empty string
- Type safety with TypeScript

**Syntax:**
```typescript
const [text, setText] = useState('');
const [message, setMessage] = useState('Hello');
```

**Simple Example:**
```typescript
function TextInput() {
  const [text, setText] = useState('');
  
  return (
    <div>
      <input 
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Type something..."
      />
      <p>You typed: {text}</p>
    </div>
  );
}
```

**Real-world Example:**
```typescript
function SearchComponent() {
  const [searchQuery, setSearchQuery] = useState('');
  const [searchResults, setSearchResults] = useState<string[]>([]);
  const [isSearching, setIsSearching] = useState(false);

  const handleSearch = async (query: string) => {
    setSearchQuery(query);
    
    if (query.length < 2) {
      setSearchResults([]);
      return;
    }

    setIsSearching(true);
    
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 500));
    
    // Mock search results
    const mockResults = [
      'Apple', 'Banana', 'Cherry', 'Date', 'Elderberry'
    ].filter(item => 
      item.toLowerCase().includes(query.toLowerCase())
    );
    
    setSearchResults(mockResults);
    setIsSearching(false);
  };

  const clearSearch = () => {
    setSearchQuery('');
    setSearchResults([]);
  };

  return (
    <div className="search-component">
      <div className="search-input">
        <input
          type="text"
          value={searchQuery}
          onChange={(e) => handleSearch(e.target.value)}
          placeholder="Search fruits..."
          className="search-field"
        />
        {searchQuery && (
          <button onClick={clearSearch} className="clear-button">
            ×
          </button>
        )}
      </div>

      {isSearching && <div className="searching">Searching...</div>}

      {searchResults.length > 0 && (
        <ul className="search-results">
          {searchResults.map((result, index) => (
            <li key={index} className="result-item">
              {result}
            </li>
          ))}
        </ul>
      )}

      {searchQuery.length >= 2 && searchResults.length === 0 && !isSearching && (
        <div className="no-results">No results found for "{searchQuery}"</div>
      )}
    </div>
  );
}
```

**String State Patterns:**

1. **Controlled input:**
```typescript
const [value, setValue] = useState('');
<input value={value} onChange={(e) => setValue(e.target.value)} />
```

2. **Multi-line text:**
```typescript
const [content, setContent] = useState('');
<textarea value={content} onChange={(e) => setContent(e.target.value)} />
```

3. **String concatenation:**
```typescript
const [message, setMessage] = useState('Hello');
setMessage(prev => prev + ' World');
```

4. **String transformation:**
```typescript
const [text, setText] = useState('');
setText(prev => prev.toUpperCase());
setText(prev => prev.trim());
```

**Common Errors:**
- Not handling empty strings
- String vs number confusion
- Not trimming whitespace
- Case sensitivity issues

**Best Practices:**
- Handle empty strings appropriately
- Trim user input when needed
- Consider case sensitivity for comparisons
- Validate string formats (emails, URLs)
- Use string templates for complex strings

---

### 12. Number State

**What is it?**
State that holds numeric values, used for counts, measurements, calculations, and any quantitative data.

**Why we need it?**
- Counters and totals
- Measurements and dimensions
- Calculations and computations
- Indexes and positions
- Quantitative data

**How it works?**
- Initialize with number value
- Update with new numeric values
- Can be negative, zero, or positive
- Supports all numeric operations

**Syntax:**
```typescript
const [count, setCount] = useState(0);
const [price, setPrice] = useState(99.99);
```

**Simple Example:**
```typescript
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}-</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

**Real-world Example:**
```typescript
function ShoppingCart() {
  const [items, setItems] = useState(0);
  const [total, setTotal] = useState(0);
  const [discount, setDiscount] = useState(0);
  const [taxRate, setTaxRate] = useState(0.08);

  const addItem = (price: number) => {
    setItems(prev => prev + 1);
    setTotal(prev => prev + price);
  };

  const removeItem = (price: number) => {
    if (items > 0) {
      setItems(prev => prev - 1);
      setTotal(prev => Math.max(0, prev - price));
    }
  };

  const applyDiscount = (percentage: number) => {
    setDiscount(percentage);
  };

  const calculateFinalTotal = () => {
    const discountAmount = total * (discount / 100);
    const subtotal = total - discountAmount;
    const taxAmount = subtotal * taxRate;
    return subtotal + taxAmount;
  };

  const finalTotal = calculateFinalTotal();

  return (
    <div className="shopping-cart">
      <div className="cart-summary">
        <div className="cart-item">
          <span>Items:</span>
          <span>{items}</span>
        </div>
        <div className="cart-item">
          <span>Subtotal:</span>
          <span>${total.toFixed(2)}</span>
        </div>
        {discount > 0 && (
          <div className="cart-item discount">
            <span>Discount ({discount}%):</span>
            <span>-${(total * discount / 100).toFixed(2)}</span>
          </div>
        )}
        <div className="cart-item">
          <span>Tax ({(taxRate * 100).toFixed(0)}%):</span>
          <span>${((total - total * discount / 100) * taxRate).toFixed(2)}</span>
        </div>
        <div className="cart-item total">
          <span>Total:</span>
          <span>${finalTotal.toFixed(2)}</span>
        </div>
      </div>

      <div className="cart-actions">
        <button onClick={() => addItem(10)}>Add $10 Item</button>
        <button onClick={() => removeItem(10)}>Remove $10 Item</button>
        <button onClick={() => setDiscount(10)}>Apply 10% Discount</button>
        <button onClick={() => setDiscount(0)}>Remove Discount</button>
      </div>
    </div>
  );
}
```

**Number State Patterns:**

1. **Counter:**
```typescript
const [count, setCount] = useState(0);
setCount(prev => prev + 1);
```

2. **Range slider:**
```typescript
const [value, setValue] = useState(50);
<input type="range" value={value} onChange={(e) => setValue(Number(e.target.value))} />
```

3. **Calculations:**
```typescript
const [result, setResult] = useState(0);
setResult(prev => prev * 2);
```

4. **Boundaries:**
```typescript
const [value, setValue] = useState(0);
setValue(prev => Math.max(0, Math.min(100, prev + 1)));
```

**Common Errors:**
- String vs number confusion (input values)
- Division by zero
- Floating point precision issues
- Not validating number ranges
- NaN propagation

**Best Practices:**
- Convert input strings to numbers
- Handle division by zero
- Use appropriate precision for calculations
- Validate number ranges
- Handle NaN and Infinity cases

---

### 13. Boolean State

**What is it?**
State that holds true/false values, used for toggles, flags, visibility controls, and binary conditions.

**Why we need it?**
- Toggle UI elements
- Show/hide components
- Enable/disable features
- Track binary states
- Control conditional rendering

**How it works?**
- Initialize with true or false
- Toggle between true and false
- Often used with conditional rendering
- Simple and efficient

**Syntax:**
```typescript
const [isVisible, setIsVisible] = useState(true);
const [isLoading, setIsLoading] = useState(false);
```

**Simple Example:**
```typescript
function ToggleComponent() {
  const [isVisible, setIsVisible] = useState(true);
  
  return (
    <div>
      <button onClick={() => setIsVisible(!isVisible)}>
        {isVisible ? 'Hide' : 'Show'}
      </button>
      {isVisible && <p>This content is visible</p>}
    </div>
  );
}
```

**Real-world Example:**
```typescript
function UserPreferences() {
  const [darkMode, setDarkMode] = useState(false);
  const [notifications, setNotifications] = useState(true);
  const [autoSave, setAutoSave] = useState(true);
  const [showAdvanced, setShowAdvanced] = useState(false);
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [isAdmin, setIsAdmin] = useState(false);

  const toggleDarkMode = () => setDarkMode(prev => !prev);
  const toggleNotifications = () => setNotifications(prev => !prev);
  const toggleAutoSave = () => setAutoSave(prev => !prev);
  const toggleAdvanced = () => setShowAdvanced(prev => !prev);
  const toggleLogin = () => setIsLoggedIn(prev => !prev);

  return (
    <div className={`preferences ${darkMode ? 'dark-mode' : 'light-mode'}`}>
      <h2>User Preferences</h2>

      {/* Login Status */}
      <div className="preference-item">
        <span>Status: {isLoggedIn ? 'Logged In' : 'Guest'}</span>
        {isAdmin && <span className="admin-badge">Admin</span>}
        <button onClick={toggleLogin}>
          {isLoggedIn ? 'Logout' : 'Login'}
        </button>
      </div>

      {/* Basic Preferences */}
      <div className="preference-section">
        <h3>Appearance</h3>
        <div className="preference-item">
          <label>
            <input
              type="checkbox"
              checked={darkMode}
              onChange={toggleDarkMode}
            />
            Dark Mode
          </label>
        </div>
      </div>

      <div className="preference-section">
        <h3>Notifications</h3>
        <div className="preference-item">
          <label>
            <input
              type="checkbox"
              checked={notifications}
              onChange={toggleNotifications}
            />
            Enable Notifications
          </label>
        </div>
      </div>

      <div className="preference-section">
        <h3>Editing</h3>
        <div className="preference-item">
          <label>
            <input
              type="checkbox"
              checked={autoSave}
              onChange={toggleAutoSave}
            />
            Auto-save
          </label>
        </div>
      </div>

      {/* Advanced Settings */}
      <div className="preference-section">
        <button onClick={toggleAdvanced}>
          {showAdvanced ? 'Hide Advanced' : 'Show Advanced'}
        </button>

        {showAdvanced && (
          <div className="advanced-settings">
            <div className="preference-item">
              <label>
                <input
                  type="checkbox"
                  checked={isAdmin}
                  onChange={(e) => setIsAdmin(e.target.checked)}
                />
                Admin Mode
              </label>
            </div>
            <div className="preference-item">
              <label>
                <input type="checkbox" defaultChecked={true} />
                Debug Mode
              </label>
            </div>
            <div className="preference-item">
              <label>
                <input type="checkbox" defaultChecked={false} />
                Beta Features
              </label>
            </div>
          </div>
        )}
      </div>

      {/* Conditional Features */}
      {notifications && (
        <div className="notification-banner">
          Notifications are enabled
        </div>
      )}

      {autoSave && (
        <div className="auto-save-indicator">
          Auto-save active
        </div>
      )}
    </div>
  );
}
```

**Boolean State Patterns:**

1. **Simple toggle:**
```typescript
const [isActive, setIsActive] = useState(false);
setIsActive(prev => !prev);
```

2. **Multiple booleans:**
```typescript
const [featureA, setFeatureA] = useState(true);
const [featureB, setFeatureB] = useState(false);
const [featureC, setFeatureC] = useState(true);
```

3. **Derived boolean:**
```typescript
const [items, setItems] = useState([]);
const hasItems = items.length > 0; // Derived, not state
```

4. **Boolean from condition:**
```typescript
const [value, setValue] = useState('');
const isValid = value.length > 0; // Derived boolean
```

**Common Errors:**
- Using strings 'true'/'false' instead of booleans
- Not handling undefined/null as boolean
- Complex boolean logic in state
- Overusing booleans when enums would be better

**Best Practices:**
- Use actual boolean values, not strings
- Consider enums for multiple states
- Derive booleans when possible
- Use descriptive names (isVisible, hasPermission)
- Group related booleans in objects

---

### 14. Array State

**What is it?**
State that holds array values, used for lists, collections, and any data that needs to be stored as an ordered collection.

**Why we need it?**
- Lists of items (todos, products, users)
- Multiple selections
- Ordered data
- Collections of similar items
- Data that can grow/shrink

**How it works?**
- Initialize with array
- Update using immutable patterns
- Never mutate directly
- Use array methods for updates

**Syntax:**
```typescript
const [items, setItems] = useState<string[]>([]);
const [numbers, setNumbers] = useState([1, 2, 3]);
```

**Simple Example:**
```typescript
function TodoList() {
  const [todos, setTodos] = useState<string[]>([]);
  const [newTodo, setNewTodo] = useState('');
  
  const addTodo = () => {
    if (newTodo.trim()) {
      setTodos([...todos, newTodo]);
      setNewTodo('');
    }
  };
  
  const removeTodo = (index: number) => {
    setTodos(todos.filter((_, i) => i !== index));
  };
  
  return (
    <div>
      <input 
        value={newTodo}
        onChange={(e) => setNewTodo(e.target.value)}
      />
      <button onClick={addTodo}>Add</button>
      <ul>
        {todos.map((todo, index) => (
          <li key={index}>
            {todo}
            <button onClick={() => removeTodo(index)}>Remove</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

**Real-world Example:**
```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
}

function ProductManager() {
  const [products, setProducts] = useState<Product[]>([
    { id: 1, name: 'Laptop', price: 999, category: 'Electronics' },
    { id: 2, name: 'Mouse', price: 29, category: 'Electronics' },
    { id: 3, name: 'Desk', price: 299, category: 'Furniture' }
  ]);
  
  const [selectedProducts, setSelectedProducts] = useState<number[]>([]);
  const [filterCategory, setFilterCategory] = useState<string>('all');

  // Add product
  const addProduct = (product: Product) => {
    setProducts(prev => [...prev, { ...product, id: Date.now() }]);
  };

  // Remove product
  const removeProduct = (id: number) => {
    setProducts(prev => prev.filter(p => p.id !== id));
  };

  // Update product
  const updateProduct = (id: number, updates: Partial<Product>) => {
    setProducts(prev =>
      prev.map(p => p.id === id ? { ...p, ...updates } : p)
    );
  };

  // Toggle selection
  const toggleSelection = (id: number) => {
    setSelectedProducts(prev =>
      prev.includes(id)
        ? prev.filter(pId => pid !== id)
        : [...prev, id]
    );
  };

  // Filter products
  const filteredProducts = products.filter(product =>
    filterCategory === 'all' || product.category === filterCategory
  );

  // Bulk actions
  const deleteSelected = () => {
    setProducts(prev => prev.filter(p => !selectedProducts.includes(p.id)));
    setSelectedProducts([]);
  };

  const selectAll = () => {
    setSelectedProducts(filteredProducts.map(p => p.id));
  };

  const clearSelection = () => {
    setSelectedProducts([]);
  };

  return (
    <div className="product-manager">
      <div className="controls">
        <select 
          value={filterCategory}
          onChange={(e) => setFilterCategory(e.target.value)}
        >
          <option value="all">All Categories</option>
          <option value="Electronics">Electronics</option>
          <option value="Furniture">Furniture</option>
        </select>

        <button onClick={selectAll}>Select All</button>
        <button onClick={clearSelection}>Clear Selection</button>
        {selectedProducts.length > 0 && (
          <button onClick={deleteSelected}>
            Delete Selected ({selectedProducts.length})
          </button>
        )}
      </div>

      <div className="product-list">
        {filteredProducts.map(product => (
          <div 
            key={product.id}
            className={`product-item ${selectedProducts.includes(product.id) ? 'selected' : ''}`}
          >
            <input
              type="checkbox"
              checked={selectedProducts.includes(product.id)}
              onChange={() => toggleSelection(product.id)}
            />
            <div className="product-info">
              <h4>{product.name}</h4>
              <p>${product.price}</p>
              <span className="category">{product.category}</span>
            </div>
            <button onClick={() => removeProduct(product.id)}>Delete</button>
          </div>
        ))}
      </div>

      {filteredProducts.length === 0 && (
        <div className="empty-state">No products found</div>
      )}
    </div>
  );
}
```

**Array State Update Patterns:**

1. **Add item:**
```typescript
setItems(prev => [...prev, newItem]);
```

2. **Remove item:**
```typescript
setItems(prev => prev.filter(item => item.id !== id));
```

3. **Update item:**
```typescript
setItems(prev =>
  prev.map(item => item.id === id ? { ...item, ...updates } : item)
);
```

4. **Reorder array:**
```typescript
setItems(prev => {
  const newArray = [...prev];
  const [removed] = newArray.splice(fromIndex, 1);
  newArray.splice(toIndex, 0, removed);
  return newArray;
});
```

5. **Clear array:**
```typescript
setItems([]);
```

**Common Errors:**
- Mutating array directly
- Using push/splice instead of immutable patterns
- Forgetting to use spread operator
- Not handling empty arrays
- Index-based removal issues

**Best Practices:**
- Always use immutable update patterns
- Use filter for removals
- Use map for updates
- Provide unique keys for rendering
- Handle empty array states

---

### 15. Object State

**What is it?**
State that holds object values, used for complex data structures, configurations, and related data that should be grouped together.

**Why we need it?**
- Group related data
- Complex data structures
- Configuration objects
- User profiles
- Form data

**How it works?**
- Initialize with object
- Update using spread operator
- Never mutate directly
- Use TypeScript interfaces for type safety

**Syntax:**
```typescript
const [user, setUser] = useState({ name: '', email: '' });
const [config, setConfig] = useState({ theme: 'light', lang: 'en' });
```

**Simple Example:**
```typescript
function UserProfile() {
  const [user, setUser] = useState({
    name: '',
    email: '',
    age: 0
  });
  
  const updateName = (name: string) => {
    setUser(prev => ({ ...prev, name }));
  };
  
  return (
    <div>
      <input 
        value={user.name}
        onChange={(e) => updateName(e.target.value)}
      />
      <p>Name: {user.name}</p>
    </div>
  );
}
```

**Real-world Example:**
```typescript
interface UserSettings {
  profile: {
    username: string;
    email: string;
    avatar: string;
    bio: string;
  };
  preferences: {
    theme: 'light' | 'dark';
    language: string;
    timezone: string;
    notifications: {
      email: boolean;
      push: boolean;
      sms: boolean;
    };
  };
  privacy: {
    profileVisible: boolean;
    showEmail: boolean;
    showActivity: boolean;
  };
}

function SettingsPanel() {
  const [settings, setSettings] = useState<UserSettings>({
    profile: {
      username: '',
      email: '',
      avatar: '',
      bio: ''
    },
    preferences: {
      theme: 'light',
      language: 'en',
      timezone: 'UTC',
      notifications: {
        email: true,
        push: true,
        sms: false
      }
    },
    privacy: {
      profileVisible: true,
      showEmail: false,
      showActivity: true
    }
  });

  const [activeTab, setActiveTab] = useState<'profile' | 'preferences' | 'privacy'>('profile');
  const [hasChanges, setHasChanges] = useState(false);

  // Update nested profile fields
  const updateProfile = (field: keyof UserSettings['profile'], value: string) => {
    setSettings(prev => ({
      ...prev,
      profile: { ...prev.profile, [field]: value }
    }));
    setHasChanges(true);
  };

  // Update preferences
  const updatePreferences = (field: keyof UserSettings['preferences'], value: any) => {
    setSettings(prev => ({
      ...prev,
      preferences: { ...prev.preferences, [field]: value }
    }));
    setHasChanges(true);
  };

  // Update notification settings
  const updateNotification = (type: keyof UserSettings['preferences']['notifications'], value: boolean) => {
    setSettings(prev => ({
      ...prev,
      preferences: {
        ...prev.preferences,
        notifications: {
          ...prev.preferences.notifications,
          [type]: value
        }
      }
    }));
    setHasChanges(true);
  };

  // Update privacy settings
  const updatePrivacy = (field: keyof UserSettings['privacy'], value: boolean) => {
    setSettings(prev => ({
      ...prev,
      privacy: { ...prev.privacy, [field]: value }
    }));
    setHasChanges(true);
  };

  const saveSettings = () => {
    console.log('Saving settings:', settings);
    setHasChanges(false);
    // API call would go here
  };

  const resetSettings = () => {
    setSettings({
      profile: { username: '', email: '', avatar: '', bio: '' },
      preferences: { theme: 'light', language: 'en', timezone: 'UTC', notifications: { email: true, push: true, sms: false } },
      privacy: { profileVisible: true, showEmail: false, showActivity: true }
    });
    setHasChanges(false);
  };

  return (
    <div className={`settings-panel ${settings.preferences.theme}`}>
      <div className="settings-tabs">
        <button 
          className={activeTab === 'profile' ? 'active' : ''}
          onClick={() => setActiveTab('profile')}
        >
          Profile
        </button>
        <button 
          className={activeTab === 'preferences' ? 'active' : ''}
          onClick={() => setActiveTab('preferences')}
        >
          Preferences
        </button>
        <button 
          className={activeTab === 'privacy' ? 'active' : ''}
          onClick={() => setActiveTab('privacy')}
        >
          Privacy
        </button>
      </div>

      {activeTab === 'profile' && (
        <div className="tab-content">
          <h3>Profile Settings</h3>
          <div className="form-group">
            <label>Username</label>
            <input
              value={settings.profile.username}
              onChange={(e) => updateProfile('username', e.target.value)}
            />
          </div>
          <div className="form-group">
            <label>Email</label>
            <input
              type="email"
              value={settings.profile.email}
              onChange={(e) => updateProfile('email', e.target.value)}
            />
          </div>
          <div className="form-group">
            <label>Bio</label>
            <textarea
              value={settings.profile.bio}
              onChange={(e) => updateProfile('bio', e.target.value)}
            />
          </div>
        </div>
      )}

      {activeTab === 'preferences' && (
        <div className="tab-content">
          <h3>Preferences</h3>
          <div className="form-group">
            <label>Theme</label>
            <select
              value={settings.preferences.theme}
              onChange={(e) => updatePreferences('theme', e.target.value)}
            >
              <option value="light">Light</option>
              <option value="dark">Dark</option>
            </select>
          </div>
          <div className="form-group">
            <label>Language</label>
            <select
              value={settings.preferences.language}
              onChange={(e) => updatePreferences('language', e.target.value)}
            >
              <option value="en">English</option>
              <option value="es">Spanish</option>
              <option value="fr">French</option>
            </select>
          </div>
          <div className="form-group">
            <h4>Notifications</h4>
            <label>
              <input
                type="checkbox"
                checked={settings.preferences.notifications.email}
                onChange={(e) => updateNotification('email', e.target.checked)}
              />
              Email Notifications
            </label>
            <label>
              <input
                type="checkbox"
                checked={settings.preferences.notifications.push}
                onChange={(e) => updateNotification('push', e.target.checked)}
              />
              Push Notifications
            </label>
            <label>
              <input
                type="checkbox"
                checked={settings.preferences.notifications.sms}
                onChange={(e) => updateNotification('sms', e.target.checked)}
              />
              SMS Notifications
            </label>
          </div>
        </div>
      )}

      {activeTab === 'privacy' && (
        <div className="tab-content">
          <h3>Privacy Settings</h3>
          <label>
            <input
              type="checkbox"
              checked={settings.privacy.profileVisible}
              onChange={(e) => updatePrivacy('profileVisible', e.target.checked)}
            />
            Make Profile Visible
          </label>
          <label>
            <input
              type="checkbox"
              checked={settings.privacy.showEmail}
              onChange={(e) => updatePrivacy('showEmail', e.target.checked)}
            />
            Show Email Address
          </label>
          <label>
            <input
              type="checkbox"
              checked={settings.privacy.showActivity}
              onChange={(e) => updatePrivacy('showActivity', e.target.checked)}
            />
            Show Activity Status
          </label>
        </div>
      )}

      {hasChanges && (
        <div className="settings-actions">
          <button onClick={saveSettings}>Save Changes</button>
          <button onClick={resetSettings}>Reset</button>
        </div>
      )}
    </div>
  );
}
```

**Object State Update Patterns:**

1. **Update single property:**
```typescript
setUser(prev => ({ ...prev, name: newName }));
```

2. **Update nested property:**
```typescript
setUser(prev => ({
  ...prev,
  profile: { ...prev.profile, name: newName }
}));
```

3. **Update multiple properties:**
```typescript
setUser(prev => ({
  ...prev,
  name: newName,
  email: newEmail
}));
```

4. **Reset object:**
```typescript
setUser(initialState);
```

**Common Errors:**
- Mutating object directly
- Not using spread operator
- Forgetting to copy nested objects
- Overwriting entire object instead of updating
- Type errors with TypeScript

**Best Practices:**
- Always use spread operator for updates
- Update nested objects carefully
- Use TypeScript interfaces
- Consider splitting large objects
- Use functional updates for complex changes

---

## Part 3: State Updates

### 16. Updating Objects

**What is it?**
The proper way to update object state in React using immutable patterns, ensuring React can detect changes and trigger re-renders.

**Why we need it?**
- React needs immutable updates to detect changes
- Prevents unexpected behavior
- Maintains data integrity
- Enables proper re-rendering
- Follows React best practices

**How it works?**
- Use spread operator to create new object
- Copy existing properties
- Override changed properties
- Never mutate original object
- React compares object references

**Syntax:**
```typescript
// ❌ Wrong - mutation
user.name = 'New Name';
setUser(user);

// ✅ Correct - immutable update
setUser(prev => ({ ...prev, name: 'New Name' }));
```

**Simple Example:**
```typescript
function UserProfile() {
  const [user, setUser] = useState({
    name: 'John',
    email: 'john@example.com',
    age: 30
  });

  const updateName = (newName: string) => {
    setUser(prev => ({ ...prev, name: newName }));
  };

  return (
    <div>
      <input 
        value={user.name}
        onChange={(e) => updateName(e.target.value)}
      />
      <p>{user.name}</p>
    </div>
  );
}
```

**Real-world Example:**
```typescript
interface User {
  id: number;
  profile: {
    firstName: string;
    lastName: string;
    email: string;
    phone: string;
  };
  address: {
    street: string;
    city: string;
    state: string;
    zipCode: string;
    country: string;
  };
  preferences: {
    theme: 'light' | 'dark';
    language: string;
    notifications: boolean;
  };
}

function UserEditor() {
  const [user, setUser] = useState<User>({
    id: 1,
    profile: {
      firstName: 'John',
      lastName: 'Doe',
      email: 'john@example.com',
      phone: '555-1234'
    },
    address: {
      street: '123 Main St',
      city: 'Springfield',
      state: 'IL',
      zipCode: '62701',
      country: 'USA'
    },
    preferences: {
      theme: 'light',
      language: 'en',
      notifications: true
    }
  });

  // Update top-level property
  const updateUserId = (id: number) => {
    setUser(prev => ({ ...prev, id }));
  };

  // Update nested profile property
  const updateProfileField = (field: keyof User['profile'], value: string) => {
    setUser(prev => ({
      ...prev,
      profile: { ...prev.profile, [field]: value }
    }));
  };

  // Update nested address property
  const updateAddressField = (field: keyof User['address'], value: string) => {
    setUser(prev => ({
      ...prev,
      address: { ...prev.address, [field]: value }
    }));
  };

  // Update nested preferences property
  const updatePreference = <K extends keyof User['preferences']>(
    field: K,
    value: User['preferences'][K]
  ) => {
    setUser(prev => ({
      ...prev,
      preferences: { ...prev.preferences, [field]: value }
    }));
  };

  // Update multiple fields at once
  const updateProfile = (updates: Partial<User['profile']>) => {
    setUser(prev => ({
      ...prev,
      profile: { ...prev.profile, ...updates }
    }));
  };

  // Complete object replacement
  const resetUser = () => {
    setUser({
      id: 1,
      profile: {
        firstName: '',
        lastName: '',
        email: '',
        phone: ''
      },
      address: {
        street: '',
        city: '',
        state: '',
        zipCode: '',
        country: ''
      },
      preferences: {
        theme: 'light',
        language: 'en',
        notifications: true
      }
    });
  };

  return (
    <div className="user-editor">
      <h2>Edit User Profile</h2>

      {/* Profile Section */}
      <section className="form-section">
        <h3>Profile Information</h3>
        <div className="form-group">
          <label>First Name</label>
          <input
            value={user.profile.firstName}
            onChange={(e) => updateProfileField('firstName', e.target.value)}
          />
        </div>
        <div className="form-group">
          <label>Last Name</label>
          <input
            value={user.profile.lastName}
            onChange={(e) => updateProfileField('lastName', e.target.value)}
          />
        </div>
        <div className="form-group">
          <label>Email</label>
          <input
            type="email"
            value={user.profile.email}
            onChange={(e) => updateProfileField('email', e.target.value)}
          />
        </div>
        <div className="form-group">
          <label>Phone</label>
          <input
            type="tel"
            value={user.profile.phone}
            onChange={(e) => updateProfileField('phone', e.target.value)}
          />
        </div>
      </section>

      {/* Address Section */}
      <section className="form-section">
        <h3>Address Information</h3>
        <div className="form-group">
          <label>Street</label>
          <input
            value={user.address.street}
            onChange={(e) => updateAddressField('street', e.target.value)}
          />
        </div>
        <div className="form-group">
          <label>City</label>
          <input
            value={user.address.city}
            onChange={(e) => updateAddressField('city', e.target.value)}
          />
        </div>
        <div className="form-group">
          <label>State</label>
          <input
            value={user.address.state}
            onChange={(e) => updateAddressField('state', e.target.value)}
          />
        </div>
        <div className="form-group">
          <label>ZIP Code</label>
          <input
            value={user.address.zipCode}
            onChange={(e) => updateAddressField('zipCode', e.target.value)}
          />
        </div>
      </section>

      {/* Preferences Section */}
      <section className="form-section">
        <h3>Preferences</h3>
        <div className="form-group">
          <label>Theme</label>
          <select
            value={user.preferences.theme}
            onChange={(e) => updatePreference('theme', e.target.value as 'light' | 'dark')}
          >
            <option value="light">Light</option>
            <option value="dark">Dark</option>
          </select>
        </div>
        <div className="form-group">
          <label>Language</label>
          <select
            value={user.preferences.language}
            onChange={(e) => updatePreference('language', e.target.value)}
          >
            <option value="en">English</option>
            <option value="es">Spanish</option>
            <option value="fr">French</option>
          </select>
        </div>
        <div className="form-group">
          <label>
            <input
              type="checkbox"
              checked={user.preferences.notifications}
              onChange={(e) => updatePreference('notifications', e.target.checked)}
            />
            Enable Notifications
          </label>
        </div>
      </section>

      <div className="form-actions">
        <button onClick={resetUser}>Reset</button>
        <button onClick={() => console.log('Saving:', user)}>Save Changes</button>
      </div>
    </div>
  );
}
```

**Object Update Patterns:**

1. **Single property update:**
```typescript
setUser(prev => ({ ...prev, name: newName }));
```

2. **Multiple properties update:**
```typescript
setUser(prev => ({
  ...prev,
  name: newName,
  email: newEmail,
  age: newAge
}));
```

3. **Nested property update:**
```typescript
setUser(prev => ({
  ...prev,
  profile: { ...prev.profile, name: newName }
}));
```

4. **Deep nested update:**
```typescript
setUser(prev => ({
  ...prev,
  settings: {
    ...prev.settings,
    notifications: {
      ...prev.settings.notifications,
      email: true
    }
  }
}));
```

5. **Conditional property update:**
```typescript
setUser(prev => ({
  ...prev,
  ...(shouldUpdate && { name: newName })
}));
```

**Common Errors:**
- Mutating object directly
- Forgetting spread operator
- Not copying nested objects
- Overwriting entire object accidentally
- Losing other properties during update

**Best Practices:**
- Always use spread operator
- Copy nested objects explicitly
- Use TypeScript for type safety
- Consider helper functions for complex updates
- Test object updates thoroughly

---

### 17. Updating Arrays

**What is it?**
The proper way to update array state in React using immutable patterns, ensuring React can detect changes and trigger re-renders.

**Why we need it?**
- React needs immutable updates to detect changes
- Prevents unexpected behavior
- Maintains data integrity
- Enables proper re-rendering
- Follows React best practices

**How it works?**
- Use array methods that return new arrays
- Never mutate original array
- Use spread operator for additions
- Use filter for removals
- Use map for updates

**Syntax:**
```typescript
// ❌ Wrong - mutation
items.push(newItem);
setItems(items);

// ✅ Correct - immutable update
setItems(prev => [...prev, newItem]);
```

**Simple Example:**
```typescript
function TodoList() {
  const [todos, setTodos] = useState<string[]>([]);
  
  const addTodo = (todo: string) => {
    setTodos(prev => [...prev, todo]);
  };
  
  const removeTodo = (index: number) => {
    setTodos(prev => prev.filter((_, i) => i !== index));
  };
  
  return (
    <div>
      <button onClick={() => addTodo('New Todo')}>Add</button>
      <ul>
        {todos.map((todo, index) => (
          <li key={index}>
            {todo}
            <button onClick={() => removeTodo(index)}>Remove</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

**Real-world Example:**
```typescript
interface Task {
  id: number;
  title: string;
  description: string;
  completed: boolean;
  priority: 'low' | 'medium' | 'high';
  dueDate: string;
  assignee: string;
}

function TaskManager() {
  const [tasks, setTasks] = useState<Task[]>([
    {
      id: 1,
      title: 'Complete project',
      description: 'Finish the React project',
      completed: false,
      priority: 'high',
      dueDate: '2024-12-31',
      assignee: 'John'
    },
    {
      id: 2,
      title: 'Review code',
      description: 'Review pull requests',
      completed: true,
      priority: 'medium',
      dueDate: '2024-12-15',
      assignee: 'Jane'
    }
  ]);

  const [filter, setFilter] = useState<'all' | 'active' | 'completed'>('all');
  const [sortBy, setSortBy] = useState<'dueDate' | 'priority' | 'title'>('dueDate');

  // Add new task
  const addTask = (task: Omit<Task, 'id'>) => {
    setTasks(prev => [
      ...prev,
      { ...task, id: Date.now() }
    ]);
  };

  // Remove task
  const removeTask = (taskId: number) => {
    setTasks(prev => prev.filter(task => task.id !== taskId));
  };

  // Update task
  const updateTask = (taskId: number, updates: Partial<Task>) => {
    setTasks(prev =>
      prev.map(task =>
        task.id === taskId ? { ...task, ...updates } : task
      )
    );
  };

  // Toggle task completion
  const toggleTask = (taskId: number) => {
    setTasks(prev =>
      prev.map(task =>
        task.id === taskId
          ? { ...task, completed: !task.completed }
          : task
      )
    );
  };

  // Reorder tasks (move up)
  const moveTaskUp = (index: number) => {
    setTasks(prev => {
      if (index === 0) return prev;
      const newTasks = [...prev];
      [newTasks[index - 1], newTasks[index]] = [newTasks[index], newTasks[index - 1]];
      return newTasks;
    });
  };

  // Reorder tasks (move down)
  const moveTaskDown = (index: number) => {
    setTasks(prev => {
      if (index === prev.length - 1) return prev;
      const newTasks = [...prev];
      [newTasks[index], newTasks[index + 1]] = [newTasks[index + 1], newTasks[index]];
      return newTasks;
    });
  };

  // Bulk operations
  const completeAllTasks = () => {
    setTasks(prev => prev.map(task => ({ ...task, completed: true })));
  };

  const clearCompletedTasks = () => {
    setTasks(prev => prev.filter(task => !task.completed));
  };

  const duplicateTask = (taskId: number) => {
    setTasks(prev => {
      const taskToDuplicate = prev.find(task => task.id === taskId);
      if (!taskToDuplicate) return prev;
      return [
        ...prev,
        { ...taskToDuplicate, id: Date.now(), title: `${taskToDuplicate.title} (copy)` }
      ];
    });
  };

  // Filter and sort
  const filteredTasks = tasks.filter(task => {
    if (filter === 'active') return !task.completed;
    if (filter === 'completed') return task.completed;
    return true;
  });

  const sortedTasks = [...filteredTasks].sort((a, b) => {
    switch (sortBy) {
      case 'dueDate':
        return new Date(a.dueDate).getTime() - new Date(b.dueDate).getTime();
      case 'priority':
        const priorityOrder = { high: 0, medium: 1, low: 2 };
        return priorityOrder[a.priority] - priorityOrder[b.priority];
      case 'title':
        return a.title.localeCompare(b.title);
      default:
        return 0;
    }
  });

  return (
    <div className="task-manager">
      <div className="task-controls">
        <select value={filter} onChange={(e) => setFilter(e.target.value as any)}>
          <option value="all">All Tasks</option>
          <option value="active">Active</option>
          <option value="completed">Completed</option>
        </select>

        <select value={sortBy} onChange={(e) => setSortBy(e.target.value as any)}>
          <option value="dueDate">Sort by Due Date</option>
          <option value="priority">Sort by Priority</option>
          <option value="title">Sort by Title</option>
        </select>

        <button onClick={completeAllTasks}>Complete All</button>
        <button onClick={clearCompletedTasks}>Clear Completed</button>
      </div>

      <div className="task-list">
        {sortedTasks.map((task, index) => (
          <div key={task.id} className={`task-item ${task.completed ? 'completed' : ''}`}>
            <div className="task-content">
              <input
                type="checkbox"
                checked={task.completed}
                onChange={() => toggleTask(task.id)}
              />
              <div className="task-details">
                <h4>{task.title}</h4>
                <p>{task.description}</p>
                <div className="task-meta">
                  <span className={`priority priority-${task.priority}`}>
                    {task.priority}
                  </span>
                  <span>Due: {task.dueDate}</span>
                  <span>Assignee: {task.assignee}</span>
                </div>
              </div>
            </div>
            <div className="task-actions">
              <button onClick={() => moveTaskUp(index)} disabled={index === 0}>
                ↑
              </button>
              <button onClick={() => moveTaskDown(index)} disabled={index === sortedTasks.length - 1}>
                ↓
              </button>
              <button onClick={() => duplicateTask(task.id)}>Duplicate</button>
              <button onClick={() => removeTask(task.id)}>Delete</button>
            </div>
          </div>
        ))}
      </div>

      {sortedTasks.length === 0 && (
        <div className="empty-state">No tasks found</div>
      )}
    </div>
  );
}
```

**Array Update Patterns:**

1. **Add item:**
```typescript
setItems(prev => [...prev, newItem]);
```

2. **Add item at beginning:**
```typescript
setItems(prev => [newItem, ...prev]);
```

3. **Add item at specific index:**
```typescript
setItems(prev => [
  ...prev.slice(0, index),
  newItem,
  ...prev.slice(index)
]);
```

4. **Remove item by index:**
```typescript
setItems(prev => prev.filter((_, i) => i !== index));
```

5. **Remove item by id:**
```typescript
setItems(prev => prev.filter(item => item.id !== id));
```

6. **Update item:**
```typescript
setItems(prev =>
  prev.map((item, i) =>
    i === index ? { ...item, ...updates } : item
  )
);
```

7. **Reorder array:**
```typescript
setItems(prev => {
  const newArray = [...prev];
  const [removed] = newArray.splice(fromIndex, 1);
  newArray.splice(toIndex, 0, removed);
  return newArray;
});
```

8. **Clear array:**
```typescript
setItems([]);
```

**Common Errors:**
- Using push/splice directly
- Forgetting spread operator
- Not handling empty arrays
- Index-based removal issues
- Mutating array items

**Best Practices:**
- Always use immutable array methods
- Use filter for removals
- Use map for updates
- Provide unique keys for rendering
- Handle empty array states

---

### 18. Functional State Updates

**What is it?**
Using functional updates with useState where the setter function receives the previous state as an argument, ensuring you always work with the most current state.

**Why we need it?**
- Prevents stale state issues
- Ensures correct state when multiple updates occur
- Handles asynchronous state updates
- Prevents race conditions
- More reliable state updates

**How it works?**
- Pass function to setState
- Function receives previous state
- Return new state based on previous
- React batches functional updates
- Always uses most recent state

**Syntax:**
```typescript
// ❌ Problematic with multiple updates
setCount(count + 1);
setCount(count + 1);
setCount(count + 1);
// Result: count + 1 (not count + 3)

// ✅ Correct with functional updates
setCount(prev => prev + 1);
setCount(prev => prev + 1);
setCount(prev => prev + 1);
// Result: count + 3
```

**Simple Example:**
```typescript
function Counter() {
  const [count, setCount] = useState(0);
  
  const incrementThreeTimes = () => {
    // ❌ This won't work as expected
    // setCount(count + 1);
    // setCount(count + 1);
    // setCount(count + 1);
    
    // ✅ This works correctly
    setCount(prev => prev + 1);
    setCount(prev => prev + 1);
    setCount(prev => prev + 1);
  };
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={incrementThreeTimes}>Add 3</button>
    </div>
  );
}
```

**Real-world Example:**
```typescript
function ShoppingCart() {
  const [cart, setCart] = useState<{ id: number; quantity: number }[]>([]);
  const [total, setTotal] = useState(0);

  // Add item with functional update
  const addItem = (productId: number) => {
    setCart(prevCart => {
      const existingItem = prevCart.find(item => item.id === productId);
      
      if (existingItem) {
        // Update existing item quantity
        return prevCart.map(item =>
          item.id === productId
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      } else {
        // Add new item
        return [...prevCart, { id: productId, quantity: 1 }];
      }
    });
  };

  // Remove item with functional update
  const removeItem = (productId: number) => {
    setCart(prevCart => prevCart.filter(item => item.id !== productId));
  };

  // Update quantity with functional update
  const updateQuantity = (productId: number, newQuantity: number) => {
    setCart(prevCart =>
      prevCart.map(item =>
        item.id === productId
          ? { ...item, quantity: Math.max(0, newQuantity) }
          : item
      )
    );
  };

  // Increment quantity (multiple clicks)
  const incrementQuantity = (productId: number) => {
    setCart(prevCart =>
      prevCart.map(item =>
        item.id === productId
          ? { ...item, quantity: item.quantity + 1 }
          : item
      )
    );
  };

  // Decrement quantity
  const decrementQuantity = (productId: number) => {
    setCart(prevCart =>
      prevCart.map(item =>
        item.id === productId
          ? { ...item, quantity: Math.max(0, item.quantity - 1) }
          : item
      )
    );
  };

  // Clear cart with functional update
  const clearCart = () => {
    setCart(() => []);
  };

  // Calculate total with derived state (not state)
  const cartTotal = cart.reduce(
    (sum, item) => sum + (item.quantity * getProductPrice(item.id)),
    0
  );

  return (
    <div className="shopping-cart">
      <h2>Shopping Cart ({cart.length} items)</h2>
      
      {cart.length === 0 ? (
        <p>Your cart is empty</p>
      ) : (
        <>
          <div className="cart-items">
            {cart.map(item => (
              <div key={item.id} className="cart-item">
                <span>Product {item.id}</span>
                <div className="quantity-controls">
                  <button onClick={() => decrementQuantity(item.id)}>-</button>
                  <span>{item.quantity}</span>
                  <button onClick={() => incrementQuantity(item.id)}>+</button>
                </div>
                <button onClick={() => removeItem(item.id)}>Remove</button>
              </div>
            ))}
          </div>
          
          <div className="cart-summary">
            <p>Total: ${cartTotal.toFixed(2)}</p>
            <button onClick={clearCart}>Clear Cart</button>
            <button>Checkout</button>
          </div>
        </>
      )}
    </div>
  );
}

// Helper function (would normally come from API)
function getProductPrice(productId: number): number {
  const prices: Record<number, number> = {
    1: 10,
    2: 20,
    3: 30
  };
  return prices[productId] || 0;
}
```

**Functional Update Patterns:**

1. **Simple increment:**
```typescript
setCount(prev => prev + 1);
```

2. **Complex calculation:**
```typescript
setValue(prev => {
  const newValue = complexCalculation(prev);
  return newValue;
});
```

3. **Array operations:**
```typescript
setItems(prev => [...prev, newItem]);
```

4. **Object updates:**
```typescript
setUser(prev => ({ ...prev, name: newName }));
```

5. **Conditional updates:**
```typescript
setValue(prev => shouldUpdate ? newValue : prev);
```

**Common Errors:**
- Not using functional updates when needed
- Assuming synchronous state updates
- Stale closures in event handlers
- Multiple rapid updates without functional form
- Not understanding batching behavior

**Best Practices:**
- Use functional updates when new state depends on old state
- Always use functional updates in event handlers
- Understand React's batching behavior
- Don't assume synchronous updates
- Test rapid update scenarios

---

### 19. Previous State

**What is it?**
Accessing the previous state value in functional updates, allowing you to base new state on the most recent state value.

**Why we need it?**
- Prevents stale state issues
- Ensures correct incremental updates
- Handles rapid state changes
- Prevents race conditions
- Reliable state transitions

**How it works?**
- Functional update receives previous state
- Previous state is guaranteed to be current
- Can be used for calculations
- Prevents closure staleness
- Works with React's batching

**Syntax:**
```typescript
setState(prevState => {
  // prevState is guaranteed to be current
  return newState;
});
```

**Simple Example:**
```typescript
function Counter() {
  const [count, setCount] = useState(0);
  
  const handleRapidClicks = () => {
    // Each update uses the most recent count
    setCount(prev => prev + 1);
    setCount(prev => prev + 1);
    setCount(prev => prev + 1);
  };
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleRapidClicks}>Rapid Clicks</button>
    </div>
  );
}
```

**Real-world Example:**
```typescript
function VoteSystem() {
  const [votes, setVotes] = useState({
    optionA: 0,
    optionB: 0,
    optionC: 0
  });

  const [hasVoted, setHasVoted] = useState(false);

  const vote = (option: keyof typeof votes) => {
    if (hasVoted) {
      alert('You have already voted!');
      return;
    }

    // Use functional update to ensure we're using current votes
    setVotes(prevVotes => ({
      ...prevVotes,
      [option]: prevVotes[option] + 1
    }));

    setHasVoted(true);
  };

  const resetVotes = () => {
    setVotes(() => ({
      optionA: 0,
      optionB: 0,
      optionC: 0
    }));
    setHasVoted(false);
  };

  const totalVotes = votes.optionA + votes.optionB + votes.optionC;
  const leadingOption = Object.entries(votes).reduce((a, b) =>
    b[1] > a[1] ? b : a
  );

  return (
    <div className="vote-system">
      <h2>Vote for your favorite option</h2>
      
      <div className="vote-options">
        <button 
          onClick={() => vote('optionA')}
          disabled={hasVoted}
          className={votes.optionA === leadingOption[1] ? 'leading' : ''}
        >
          Option A ({votes.optionA} votes)
        </button>
        <button 
          onClick={() => vote('optionB')}
          disabled={hasVoted}
          className={votes.optionB === leadingOption[1] ? 'leading' : ''}
        >
          Option B ({votes.optionB} votes)
        </button>
        <button 
          onClick={() => vote('optionC')}
          disabled={hasVoted}
          className={votes.optionC === leadingOption[1] ? 'leading' : ''}
        >
          Option C ({votes.optionC} votes)
        </button>
      </div>

      <div className="vote-results">
        <p>Total votes: {totalVotes}</p>
        {totalVotes > 0 && (
          <p>Currently leading: {leadingOption[0]} with {leadingOption[1]} votes</p>
        )}
        {hasVoted && (
          <p className="voted-message">Thank you for voting!</p>
        )}
      </div>

      <button onClick={resetVotes}>Reset Votes</button>
    </div>
  );
}
```

**Previous State Use Cases:**

1. **Incremental updates:**
```typescript
setCount(prev => prev + 1);
```

2. **Toggle boolean:**
```typescript
setIsActive(prev => !prev);
```

3. **Array operations:**
```typescript
setItems(prev => [...prev, newItem]);
```

4. **Conditional updates:**
```typescript
setValue(prev => prev > 10 ? 10 : prev + 1);
```

5. **Complex calculations:**
```typescript
setData(prev => {
  const calculated = complexFunction(prev);
  return calculated;
});
```

**Common Mistakes with Previous State:**

1. **❌ Stale closure:**
```typescript
useEffect(() => {
  const interval = setInterval(() => {
    setCount(count + 1); // Always uses initial count
  }, 1000);
  return () => clearInterval(interval);
}, []); // Empty dependency array

// ✅ Correct: Functional update
useEffect(() => {
  const interval = setInterval(() => {
    setCount(prev => prev + 1); // Uses current count
  }, 1000);
  return () => clearInterval(interval);
}, []);
```

2. **❌ Multiple rapid updates:**
```typescript
const handleClick = () => {
  setCount(count + 1); // Uses stale count
  setCount(count + 1); // Uses stale count
  setCount(count + 1); // Uses stale count
};

// ✅ Correct: Functional updates
const handleClick = () => {
  setCount(prev => prev + 1); // Uses current count
  setCount(prev => prev + 1); // Uses current count
  setCount(prev => prev + 1); // Uses current count
};
```

**Common Errors:**
- Not using functional updates when needed
- Assuming state updates are synchronous
- Stale closures in useEffect
- Multiple rapid updates without functional form
- Not understanding React's batching

**Best Practices:**
- Use functional updates when new state depends on old state
- Always use functional updates in intervals/timeouts
- Understand that state updates are batched
- Don't assume immediate state updates
- Test rapid update scenarios

---

### 20. Multiple State Variables

**What is it?**
Using multiple useState calls to manage different pieces of state independently within a single component.

**Why we need it?**
- Separate concerns logically
- Better code organization
- Easier to understand
- More targeted re-renders
- Clearer state structure

**How it works?**
- Multiple useState calls in one component
- Each state variable independent
- Each has its own setter function
- Updates don't affect other state variables
- Can be related or unrelated

**Syntax:**
```typescript
const [state1, setState1] = useState(initial1);
const [state2, setState2] = useState(initial2);
const [state3, setState3] = useState(initial3);
```

**Simple Example:**
```typescript
function Form() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [age, setAge] = useState(0);
  
  return (
    <form>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <input value={email} onChange={(e) => setEmail(e.target.value)} />
      <input value={age} onChange={(e) => setAge(Number(e.target.value))} />
    </form>
  );
}
```

**Real-world Example:**
```typescript
function UserRegistration() {
  // Personal information
  const [firstName, setFirstName] = useState('');
  const [lastName, setLastName] = useState('');
  const [email, setEmail] = useState('');
  const [phone, setPhone] = useState('');

  // Account information
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [confirmPassword, setConfirmPassword] = useState('');

  // Form state
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);
  const [success, setSuccess] = useState(false);

  // UI state
  const [showPassword, setShowPassword] = useState(false);
  const [acceptTerms, setAcceptTerms] = useState(false);
  const [newsletter, setNewsletter] = useState(false);

  // Validation state
  const [errors, setErrors] = useState<{
    firstName?: string;
    lastName?: string;
    email?: string;
    username?: string;
    password?: string;
  }>({});

  const validateForm = () => {
    const newErrors: typeof errors = {};

    if (!firstName.trim()) newErrors.firstName = 'First name is required';
    if (!lastName.trim()) newErrors.lastName = 'Last name is required';
    if (!email.trim() || !/\S+@\S+\.\S+/.test(email)) {
      newErrors.email = 'Valid email is required';
    }
    if (!username.trim()) newErrors.username = 'Username is required';
    if (password.length < 8) {
      newErrors.password = 'Password must be at least 8 characters';
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    if (!validateForm()) return;
    if (password !== confirmPassword) {
      setError('Passwords do not match');
      return;
    }
    if (!acceptTerms) {
      setError('You must accept the terms and conditions');
      return;
    }

    setIsLoading(true);
    setError(null);

    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 2000));

      console.log('Registration data:', {
        firstName,
        lastName,
        email,
        phone,
        username,
        password,
        newsletter
      });

      setSuccess(true);
    } catch (err) {
      setError('Registration failed. Please try again.');
    } finally {
      setIsLoading(false);
    }
  };

  const resetForm = () => {
    setFirstName('');
    setLastName('');
    setEmail('');
    setPhone('');
    setUsername('');
    setPassword('');
    setConfirmPassword('');
    setErrors({});
    setError(null);
    setSuccess(false);
    setAcceptTerms(false);
    setNewsletter(false);
  };

  if (success) {
    return (
      <div className="success-message">
        <h2>Registration Successful!</h2>
        <p>Welcome, {firstName} {lastName}!</p>
        <button onClick={resetForm}>Register Another User</button>
      </div>
    );
  }

  return (
    <div className="registration-form">
      <h2>Create Account</h2>

      {error && <div className="error-message">{error}</div>}

      <form onSubmit={handleSubmit}>
        {/* Personal Information */}
        <fieldset>
          <legend>Personal Information</legend>
          <div className="form-group">
            <label>First Name</label>
            <input
              value={firstName}
              onChange={(e) => setFirstName(e.target.value)}
              className={errors.firstName ? 'error' : ''}
            />
            {errors.firstName && <span className="error-text">{errors.firstName}</span>}
          </div>

          <div className="form-group">
            <label>Last Name</label>
            <input
              value={lastName}
              onChange={(e) => setLastName(e.target.value)}
              className={errors.lastName ? 'error' : ''}
            />
            {errors.lastName && <span className="error-text">{errors.lastName}</span>}
          </div>

          <div className="form-group">
            <label>Email</label>
            <input
              type="email"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              className={errors.email ? 'error' : ''}
            />
            {errors.email && <span className="error-text">{errors.email}</span>}
          </div>

          <div className="form-group">
            <label>Phone (optional)</label>
            <input
              type="tel"
              value={phone}
              onChange={(e) => setPhone(e.target.value)}
            />
          </div>
        </fieldset>

        {/* Account Information */}
        <fieldset>
          <legend>Account Information</legend>
          <div className="form-group">
            <label>Username</label>
            <input
              value={username}
              onChange={(e) => setUsername(e.target.value)}
              className={errors.username ? 'error' : ''}
            />
            {errors.username && <span className="error-text">{errors.username}</span>}
          </div>

          <div className="form-group">
            <label>Password</label>
            <div className="password-input">
              <input
                type={showPassword ? 'text' : 'password'}
                value={password}
                onChange={(e) => setPassword(e.target.value)}
                className={errors.password ? 'error' : ''}
              />
              <button
                type="button"
                onClick={() => setShowPassword(!showPassword)}
              >
                {showPassword ? 'Hide' : 'Show'}
              </button>
            </div>
            {errors.password && <span className="error-text">{errors.password}</span>}
          </div>

          <div className="form-group">
            <label>Confirm Password</label>
            <input
              type="password"
              value={confirmPassword}
              onChange={(e) => setConfirmPassword(e.target.value)}
            />
          </div>
        </fieldset>

        {/* Preferences */}
        <fieldset>
          <legend>Preferences</legend>
          <label className="checkbox-label">
            <input
              type="checkbox"
              checked={acceptTerms}
              onChange={(e) => setAcceptTerms(e.target.checked)}
            />
            I accept the terms and conditions
          </label>

          <label className="checkbox-label">
            <input
              type="checkbox"
              checked={newsletter}
              onChange={(e) => setNewsletter(e.target.checked)}
            />
            Subscribe to newsletter
          </label>
        </fieldset>

        <button type="submit" disabled={isLoading}>
          {isLoading ? 'Creating Account...' : 'Create Account'}
        </button>

        <button type="button" onClick={resetForm}>
          Reset Form
        </button>
      </form>
    </div>
  );
}
```

**Multiple State Patterns:**

1. **Related state:**
```typescript
const [firstName, setFirstName] = useState('');
const [lastName, setLastName] = useState('');
const [email, setEmail] = useState('');
```

2. **UI state:**
```typescript
const [isLoading, setIsLoading] = useState(false);
const [isModalOpen, setIsModalOpen] = useState(false);
const [currentTab, setCurrentTab] = useState('home');
```

3. **Form state:**
```typescript
const [formData, setFormData] = useState(initialData);
const [validationErrors, setValidationErrors] = useState({});
const [isSubmitting, setIsSubmitting] = useState(false);
```

4. **Data state:**
```typescript
const [users, setUsers] = useState([]);
const [selectedUser, setSelectedUser] = useState(null);
const [filter, setFilter] = useState('');
```

**When to Combine vs Separate:**

**Combine when:**
- Data is logically related
- Updates happen together
- Represents a single entity
- Better as an object for clarity

**Separate when:**
- Updates happen independently
- Different update frequencies
- Unrelated concerns
- Simpler to manage separately

**Common Errors:**
- Too many unrelated state variables
- Not grouping related state
- Inconsistent state management
- Prop drilling too many state variables
- Not considering derived state

**Best Practices:**
- Group related state together
- Separate unrelated state
- Consider derived state
- Keep state minimal
- Use consistent naming conventions

---

## Part 4: State Patterns

### 21. Derived State

**What is it?**
State that is calculated from other state or props, rather than being stored separately. It's computed on-demand during rendering.

**Why we need it?**
- Avoids state duplication
- Reduces complexity
- Prevents synchronization issues
- More maintainable code
- Single source of truth

**How it works?**
- Calculate during render
- Based on existing state/props
- No need to store separately
- Automatically updates when dependencies change
- Computed using JavaScript

**Syntax:**
```typescript
// ❌ Wrong: Storing derived state
const [items, setItems] = useState([]);
const [itemCount, setItemCount] = useState(0);
// Need to manually keep itemCount in sync

// ✅ Correct: Derived state
const [items, setItems] = useState([]);
const itemCount = items.length; // Calculated on render
```

**Simple Example:**
```typescript
function TodoList() {
  const [todos, setTodos] = useState([
    { id: 1, text: 'Learn React', completed: false },
    { id: 2, text: 'Build app', completed: true }
  ]);

  // Derived state - not stored separately
  const completedCount = todos.filter(t => t.completed).length;
  const totalCount = todos.length;
  const completionPercentage = totalCount > 0 
    ? (completedCount / totalCount) * 100 
    : 0;

  return (
    <div>
      <p>Progress: {completionPercentage.toFixed(0)}%</p>
      <p>{completedCount} of {totalCount} tasks completed</p>
    </div>
  );
}
```

**Real-world Example:**
```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
  inStock: boolean;
}

function ProductDashboard() {
  const [products, setProducts] = useState<Product[]>([
    { id: 1, name: 'Laptop', price: 999, category: 'Electronics', inStock: true },
    { id: 2, name: 'Mouse', price: 29, category: 'Electronics', inStock: true },
    { id: 3, name: 'Desk', price: 299, category: 'Furniture', inStock: false },
    { id: 4, name: 'Chair', price: 199, category: 'Furniture', inStock: true }
  ]);

  const [searchQuery, setSearchQuery] = useState('');
  const [selectedCategory, setSelectedCategory] = useState('all');
  const [sortBy, setSortBy] = useState<'name' | 'price' | 'category'>('name');

  // Derived state - filtered products
  const filteredProducts = products.filter(product => {
    const matchesSearch = product.name.toLowerCase().includes(searchQuery.toLowerCase());
    const matchesCategory = selectedCategory === 'all' || product.category === selectedCategory;
    return matchesSearch && matchesCategory;
  });

  // Derived state - sorted products
  const sortedProducts = [...filteredProducts].sort((a, b) => {
    switch (sortBy) {
      case 'price':
        return a.price - b.price;
      case 'category':
        return a.category.localeCompare(b.category);
      case 'name':
      default:
        return a.name.localeCompare(b.name);
    }
  });

  // Derived state - statistics
  const stats = {
    totalProducts: products.length,
    inStockCount: products.filter(p => p.inStock).length,
    outOfStockCount: products.filter(p => !p.inStock).length,
    totalValue: products.reduce((sum, p) => sum + p.price, 0),
    averagePrice: products.length > 0 
      ? products.reduce((sum, p) => sum + p.price, 0) / products.length 
      : 0,
    categories: [...new Set(products.map(p => p.category))],
    categoryCounts: products.reduce((acc, product) => {
      acc[product.category] = (acc[product.category] || 0) + 1;
      return acc;
    }, {} as Record<string, number>)
  };

  // Derived state - availability status
  const getAvailabilityStatus = () => {
    if (stats.outOfStockCount === 0) return 'All items in stock';
    if (stats.inStockCount === 0) return 'All items out of stock';
    return `${stats.inStockCount} in stock, ${stats.outOfStockCount} out of stock`;
  };

  // Derived state - low stock items
  const lowStockItems = products.filter(p => p.inStock && p.price < 50);

  return (
    <div className="product-dashboard">
      {/* Search and Filter Controls */}
      <div className="controls">
        <input
          type="text"
          placeholder="Search products..."
          value={searchQuery}
          onChange={(e) => setSearchQuery(e.target.value)}
        />
        
        <select
          value={selectedCategory}
          onChange={(e) => setSelectedCategory(e.target.value)}
        >
          <option value="all">All Categories</option>
          {stats.categories.map(category => (
            <option key={category} value={category}>
              {category} ({stats.categoryCounts[category]})
            </option>
          ))}
        </select>

        <select
          value={sortBy}
          onChange={(e) => setSortBy(e.target.value as any)}
        >
          <option value="name">Sort by Name</option>
          <option value="price">Sort by Price</option>
          <option value="category">Sort by Category</option>
        </select>
      </div>

      {/* Statistics Dashboard */}
      <div className="statistics">
        <h3>Statistics</h3>
        <div className="stat-grid">
          <div className="stat-card">
            <h4>Total Products</h4>
            <p>{stats.totalProducts}</p>
          </div>
          <div className="stat-card">
            <h4>In Stock</h4>
            <p>{stats.inStockCount}</p>
          </div>
          <div className="stat-card">
            <h4>Out of Stock</h4>
            <p>{stats.outOfStockCount}</p>
          </div>
          <div className="stat-card">
            <h4>Total Value</h4>
            <p>${stats.totalValue.toLocaleString()}</p>
          </div>
          <div className="stat-card">
            <h4>Average Price</h4>
            <p>${stats.averagePrice.toFixed(2)}</p>
          </div>
          <div className="stat-card">
            <h4>Availability</h4>
            <p>{getAvailabilityStatus()}</p>
          </div>
        </div>

        {lowStockItems.length > 0 && (
          <div className="low-stock-alert">
            <h4>Low Stock Alert (Under $50)</h4>
            <ul>
              {lowStockItems.map(item => (
                <li key={item.id}>{item.name} - ${item.price}</li>
              ))}
            </ul>
          </div>
        )}
      </div>

      {/* Product List */}
      <div className="product-list">
        <h3>Products ({sortedProducts.length})</h3>
        {sortedProducts.map(product => (
          <div key={product.id} className={`product-item ${!product.inStock ? 'out-of-stock' : ''}`}>
            <h4>{product.name}</h4>
            <p>${product.price}</p>
            <span className="category">{product.category}</span>
            <span className={`stock-status ${product.inStock ? 'in-stock' : 'out-of-stock'}`}>
              {product.inStock ? 'In Stock' : 'Out of Stock'}
            </span>
          </div>
        ))}

        {sortedProducts.length === 0 && (
          <div className="empty-state">
            No products found matching your criteria
          </div>
        )}
      </div>
    </div>
  );
}
```

**Derived State Patterns:**

1. **Filtered data:**
```typescript
const filteredItems = items.filter(item => item.active);
```

2. **Sorted data:**
```typescript
const sortedItems = [...items].sort((a, b) => a.value - b.value);
```

3. **Aggregated data:**
```typescript
const total = items.reduce((sum, item) => sum + item.value, 0);
```

4. **Conditional values:**
```typescript
const status = items.length > 0 ? 'has items' : 'empty';
```

5. **Formatted data:**
```typescript
const formattedPrice = price.toFixed(2);
```

**Common Errors:**
- Storing derived state as separate state
- Keeping derived state in sync manually
- Complex derived state causing performance issues
- Not memoizing expensive calculations

**Best Practices:**
- Calculate derived state during render
- Use useMemo for expensive calculations
- Keep derived state simple
- Avoid storing what can be calculated
- Document derived state logic

---

### 22. State Duplication

**What is it?**
The anti-pattern of storing the same data in multiple state variables, leading to synchronization issues and bugs.

**Why we need to avoid it:**
- Prevents data inconsistency
- Reduces complexity
- Eliminates synchronization bugs
- Easier to maintain
- Single source of truth

**How it works:**
- Same data stored in multiple places
- Updates must keep all copies in sync
- Leads to bugs and inconsistencies
- Difficult to maintain
- Violates DRY principle

**Syntax:**
```typescript
// ❌ Wrong: State duplication
const [items, setItems] = useState([]);
const [itemCount, setItemCount] = useState(0);
const [filteredItems, setFilteredItems] = useState([]);

// ✅ Correct: Single source of truth
const [items, setItems] = useState([]);
const itemCount = items.length; // Derived
const filteredItems = items.filter(item => item.active); // Derived
```

**Simple Example:**
```typescript
function BadCounter() {
  const [count, setCount] = useState(0);
  const [doubleCount, setDoubleCount] = useState(0); // ❌ Duplicated state

  const increment = () => {
    setCount(count + 1);
    setDoubleCount(count + 2); // Must keep in sync manually
  };

  return (
    <div>
      <p>Count: {count}</p>
      <p>Double: {doubleCount}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

function GoodCounter() {
  const [count, setCount] = useState(0);
  const doubleCount = count * 2; // ✅ Derived state

  const increment = () => {
    setCount(count + 1);
    // doubleCount updates automatically
  };

  return (
    <div>
      <p>Count: {count}</p>
      <p>Double: {doubleCount}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}
```

**Real-world Example:**
```typescript
// ❌ BAD: State duplication example
function BadUserForm() {
  const [users, setUsers] = useState<User[]>([]);
  const [activeUsers, setActiveUsers] = useState<User[]>([]); // Duplicated
  const [inactiveUsers, setInactiveUsers] = useState<User[]>([]); // Duplicated
  const [userCount, setUserCount] = useState(0); // Duplicated
  const [averageAge, setAverageAge] = useState(0); // Duplicated

  const addUser = (user: User) => {
    setUsers(prev => [...prev, user]);
    // Must manually update all duplicated state
    setActiveUsers(prev => [...prev, user]);
    setUserCount(prev => prev + 1);
    // Calculate new average age
    const newAverage = [...users, user].reduce((sum, u) => sum + u.age, 0) / (users.length + 1);
    setAverageAge(newAverage);
  };

  const toggleUserStatus = (userId: number) => {
    setUsers(prev => prev.map(u => 
      u.id === userId ? { ...u, active: !u.active } : u
    ));
    // Must manually update duplicated arrays
    setActiveUsers(prev => prev.filter(u => u.id !== userId));
    setInactiveUsers(prev => prev.filter(u => u.id !== userId));
    // This is error-prone and complex
  };
}

// ✅ GOOD: Single source of truth
function GoodUserForm() {
  const [users, setUsers] = useState<User[]>([]);

  // All derived from single source of truth
  const activeUsers = users.filter(u => u.active);
  const inactiveUsers = users.filter(u => !u.active);
  const userCount = users.length;
  const averageAge = users.length > 0 
    ? users.reduce((sum, u) => sum + u.age, 0) / users.length 
    : 0;

  const addUser = (user: User) => {
    setUsers(prev => [...prev, user]);
    // All derived values update automatically
  };

  const toggleUserStatus = (userId: number) => {
    setUsers(prev => prev.map(u => 
      u.id === userId ? { ...u, active: !u.active } : u
    ));
    // All derived values update automatically
  };
}
```

**Common State Duplication Scenarios:**

1. **Array + count:**
```typescript
// ❌ Duplicated
const [items, setItems] = useState([]);
const [itemCount, setItemCount] = useState(0);

// ✅ Derived
const [items, setItems] = useState([]);
const itemCount = items.length;
```

2. **Array + filtered array:**
```typescript
// ❌ Duplicated
const [items, setItems] = useState([]);
const [filteredItems, setFilteredItems] = useState([]);

// ✅ Derived
const [items, setItems] = useState([]);
const filteredItems = items.filter(item => item.active);
```

3. **Object + individual properties:**
```typescript
// ❌ Duplicated
const [user, setUser] = useState({ name: '', email: '' });
const [userName, setUserName] = useState('');
const [userEmail, setUserEmail] = useState('');

// ✅ Single source
const [user, setUser] = useState({ name: '', email: '' });
// Access user.name, user.email directly
```

4. **Original + transformed data:**
```typescript
// ❌ Duplicated
const [data, setData] = useState([]);
const [formattedData, setFormattedData] = useState([]);

// ✅ Derived
const [data, setData] = useState([]);
const formattedData = data.map(item => formatItem(item));
```

**Common Errors:**
- Storing filtered/transformed versions of data
- Keeping counts alongside arrays
- Duplicating object properties
- Manual synchronization attempts
- Complex update logic to maintain duplication

**Best Practices:**
- Store data once
- Derive everything else
- Use useMemo for expensive derivations
- Keep state minimal
- Trust React's rendering

---

### 23. Single Source of Truth

**What is it?**
The principle that each piece of data should have a single authoritative source, with all other data derived from it.

**Why we need it:**
- Prevents data inconsistency
- Simplifies state management
- Reduces bugs
- Easier to debug
- Clearer data flow

**How it works?**
- Identify the primary data source
- Derive all other data from it
- Update only the primary source
- Let derivations happen automatically
- Avoid data duplication

**Syntax:**
```typescript
// ✅ Single source of truth
const [items, setItems] = useState<Item[]>([]);
const total = items.reduce((sum, item) => sum + item.value, 0);
const average = items.length > 0 ? total / items.length : 0;
```

**Simple Example:**
```typescript
function TemperatureConverter() {
  const [celsius, setCelsius] = useState(0);
  // Fahrenheit is derived from celsius
  const fahrenheit = (celsius * 9/5) + 32;
  const kelvin = celsius + 273.15;

  return (
    <div>
      <input 
        type="number" 
        value={celsius}
        onChange={(e) => setCelsius(Number(e.target.value))}
      />
      <p>{celsius}°C = {fahrenheit.toFixed(1)}°F = {kelvin.toFixed(1)}K</p>
    </div>
  );
}
```

**Real-world Example:**
```typescript
interface Todo {
  id: number;
  text: string;
  completed: boolean;
  priority: 'low' | 'medium' | 'high';
  dueDate: string;
  createdAt: string;
}

function TodoApp() {
  // Single source of truth: the todos array
  const [todos, setTodos] = useState<Todo[]>([]);
  const [filter, setFilter] = useState<'all' | 'active' | 'completed'>('all');
  const [sortBy, setSortBy] = useState<'dueDate' | 'priority' | 'created'>('created');

  // All other state is derived from todos
  const filteredTodos = todos.filter(todo => {
    if (filter === 'active') return !todo.completed;
    if (filter === 'completed') return todo.completed;
    return true;
  });

  const sortedTodos = [...filteredTodos].sort((a, b) => {
    switch (sortBy) {
      case 'dueDate':
        return new Date(a.dueDate).getTime() - new Date(b.dueDate).getTime();
      case 'priority':
        const priorityOrder = { high: 0, medium: 1, low: 2 };
        return priorityOrder[a.priority] - priorityOrder[b.priority];
      case 'created':
      default:
        return new Date(a.createdAt).getTime() - new Date(b.createdAt).getTime();
    }
  });

  // Statistics derived from todos
  const stats = {
    total: todos.length,
    completed: todos.filter(t => t.completed).length,
    active: todos.filter(t => !t.completed).length,
    highPriority: todos.filter(t => t.priority === 'high').length,
    overdue: todos.filter(t => !t.completed && new Date(t.dueDate) < new Date()).length
  };

  // Derived UI state
  const completionRate = stats.total > 0 
    ? (stats.completed / stats.total) * 100 
    : 0;

  const hasOverdueTasks = stats.overdue > 0;
  const hasHighPriorityTasks = stats.highPriority > 0;

  // Add todo - only update single source of truth
  const addTodo = (text: string, priority: Todo['priority'], dueDate: string) => {
    setTodos(prev => [...prev, {
      id: Date.now(),
      text,
      completed: false,
      priority,
      dueDate,
      createdAt: new Date().toISOString()
    }]);
    // All derived values update automatically
  };

  // Toggle todo - only update single source of truth
  const toggleTodo = (id: number) => {
    setTodos(prev => prev.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
    // All derived values update automatically
  };

  // Delete todo - only update single source of truth
  const deleteTodo = (id: number) => {
    setTodos(prev => prev.filter(todo => todo.id !== id));
    // All derived values update automatically
  };

  return (
    <div className="todo-app">
      {/* Statistics Dashboard */}
      <div className="stats">
        <h3>Statistics</h3>
        <div className="stat-grid">
          <div className="stat">
            <span>Total</span>
            <strong>{stats.total}</strong>
          </div>
          <div className="stat">
            <span>Completed</span>
            <strong>{stats.completed}</strong>
          </div>
          <div className="stat">
            <span>Active</span>
            <strong>{stats.active}</strong>
          </div>
          <div className="stat">
            <span>Progress</span>
            <strong>{completionRate.toFixed(0)}%</strong>
          </div>
        </div>

        {hasOverdueTasks && (
          <div className="alert alert-warning">
            ⚠️ {stats.overdue} overdue task(s)
          </div>
        )}

        {hasHighPriorityTasks && (
          <div className="alert alert-info">
            🔥 {stats.highPriorityTasks} high priority task(s)
          </div>
        )}
      </div>

      {/* Controls */}
      <div className="controls">
        <select value={filter} onChange={(e) => setFilter(e.target.value as any)}>
          <option value="all">All</option>
          <option value="active">Active</option>
          <option value="completed">Completed</option>
        </select>

        <select value={sortBy} onChange={(e) => setSortBy(e.target.value as any)}>
          <option value="created">Sort by Created</option>
          <option value="dueDate">Sort by Due Date</option>
          <option value="priority">Sort by Priority</option>
        </select>
      </div>

      {/* Todo List */}
      <div className="todo-list">
        {sortedTodos.map(todo => (
          <div key={todo.id} className={`todo-item ${todo.completed ? 'completed' : ''}`}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => toggleTodo(todo.id)}
            />
            <div className="todo-content">
              <span className={`priority priority-${todo.priority}`}>
                {todo.priority}
              </span>
              <span className="text">{todo.text}</span>
              <span className="due-date">{todo.dueDate}</span>
            </div>
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
          </div>
        ))}
      </div>
    </div>
  );
}
```

**Single Source of Truth Patterns:**

1. **Identify primary data:**
```typescript
const [items, setItems] = useState([]); // Primary source
```

2. **Derive everything else:**
```typescript
const filtered = items.filter(item => item.active);
const sorted = [...filtered].sort((a, b) => a.value - b.value);
const total = items.reduce((sum, item) => sum + item.value, 0);
```

3. **Update only primary source:**
```typescript
const addItem = (item) => setItems(prev => [...prev, item]);
```

4. **Let React handle rendering:**
```typescript
// Derived values automatically update when items change
```

**Common Errors:**
- Multiple sources for same data
- Keeping derived state as separate state
- Manual synchronization attempts
- Complex update logic
- Unclear data ownership

**Best Practices:**
- Identify the single source of truth
- Derive all other data from it
- Update only the primary source
- Document data flow
- Avoid state duplication

---

### 24. Lifting State Up

**What is it?**
The pattern of moving state from child components to their nearest common parent, enabling sibling components to share data.

**Why we need it:**
- Share data between sibling components
- Centralize state management
- Enable component communication
- Avoid prop drilling
- Clear data ownership

**How it works:**
- Identify shared state
- Move to common parent
- Pass down via props
- Pass up via callbacks
- Parent becomes the source of truth

**Syntax:**
```typescript
// Before: State in child components
function ChildA() {
  const [value, setValue] = useState('');
  return <input value={value} onChange={(e) => setValue(e.target.value)} />;
}

function ChildB() {
  const [value, setValue] = useState('');
  return <div>{value}</div>;
}

// After: State lifted to parent
function Parent() {
  const [value, setValue] = useState('');
  return (
    <>
      <ChildA value={value} onChange={setValue} />
      <ChildB value={value} />
    </>
  );
}
```

**Simple Example:**
```typescript
function TemperatureConverter() {
  const [celsius, setCelsius] = useState(0);

  return (
    <div>
      <CelsiusInput value={celsius} onChange={setCelsius} />
      <FahrenheitDisplay celsius={celsius} />
    </div>
  );
}

function CelsiusInput({ value, onChange }: { value: number; onChange: (value: number) => void }) {
  return (
    <input
      type="number"
      value={value}
      onChange={(e) => onChange(Number(e.target.value))}
    />
  );
}

function FahrenheitDisplay({ celsius }: { celsius: number }) {
  const fahrenheit = (celsius * 9/5) + 32;
  return <div>{fahrenheit.toFixed(1)}°F</div>;
}
```

**Real-world Example:**
```typescript
// Product listing with cart functionality
function ProductPage() {
  // State lifted to parent
  const [cart, setCart] = useState<{ productId: number; quantity: number }[]>([]);
  const [selectedCategory, setSelectedCategory] = useState('all');
  const [searchQuery, setSearchQuery] = useState('');

  // Cart operations
  const addToCart = (productId: number) => {
    setCart(prev => {
      const existing = prev.find(item => item.productId === productId);
      if (existing) {
        return prev.map(item =>
          item.productId === productId
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      return [...prev, { productId, quantity: 1 }];
    });
  };

  const removeFromCart = (productId: number) => {
    setCart(prev => prev.filter(item => item.productId !== productId));
  };

  const updateQuantity = (productId: number, quantity: number) => {
    setCart(prev =>
      prev.map(item =>
        item.productId === productId
          ? { ...item, quantity: Math.max(0, quantity) }
          : item
      )
    );
  };

  const cartTotal = cart.reduce((sum, item) => {
    const product = products.find(p => p.id === item.productId);
    return sum + (product ? product.price * item.quantity : 0);
  }, 0);

  return (
    <div className="product-page">
      <div className="main-content">
        <ProductFilters
          selectedCategory={selectedCategory}
          onCategoryChange={setSelectedCategory}
          searchQuery={searchQuery}
          onSearchChange={setSearchQuery}
        />
        <ProductList
          selectedCategory={selectedCategory}
          searchQuery={searchQuery}
          onAddToCart={addToCart}
        />
      </div>
      <ShoppingCart
        items={cart}
        onRemove={removeFromCart}
        onUpdateQuantity={updateQuantity}
        total={cartTotal}
      />
    </div>
  );
}

function ProductFilters({
  selectedCategory,
  onCategoryChange,
  searchQuery,
  onSearchChange
}: {
  selectedCategory: string;
  onCategoryChange: (category: string) => void;
  searchQuery: string;
  onSearchChange: (query: string) => void;
}) {
  return (
    <div className="filters">
      <input
        type="text"
        placeholder="Search products..."
        value={searchQuery}
        onChange={(e) => onSearchChange(e.target.value)}
      />
      <select
        value={selectedCategory}
        onChange={(e) => onCategoryChange(e.target.value)}
      >
        <option value="all">All Categories</option>
        <option value="electronics">Electronics</option>
        <option value="clothing">Clothing</option>
      </select>
    </div>
  );
}

function ProductList({
  selectedCategory,
  searchQuery,
  onAddToCart
}: {
  selectedCategory: string;
  searchQuery: string;
  onAddToCart: (productId: number) => void;
}) {
  const filteredProducts = products.filter(product => {
    const matchesCategory = selectedCategory === 'all' || product.category === selectedCategory;
    const matchesSearch = product.name.toLowerCase().includes(searchQuery.toLowerCase());
    return matchesCategory && matchesSearch;
  });

  return (
    <div className="product-list">
      {filteredProducts.map(product => (
        <ProductCard
          key={product.id}
          product={product}
          onAddToCart={onAddToCart}
        />
      ))}
    </div>
  );
}

function ProductCard({
  product,
  onAddToCart
}: {
  product: Product;
  onAddToCart: (productId: number) => void;
}) {
  return (
    <div className="product-card">
      <h3>{product.name}</h3>
      <p>${product.price}</p>
      <button onClick={() => onAddToCart(product.id)}>Add to Cart</button>
    </div>
  );
}

function ShoppingCart({
  items,
  onRemove,
  onUpdateQuantity,
  total
}: {
  items: { productId: number; quantity: number }[];
  onRemove: (productId: number) => void;
  onUpdateQuantity: (productId: number, quantity: number) => void;
  total: number;
}) {
  return (
    <div className="shopping-cart">
      <h2>Shopping Cart ({items.length})</h2>
      {items.map(item => {
        const product = products.find(p => p.id === item.productId);
        if (!product) return null;

        return (
          <div key={item.productId} className="cart-item">
            <span>{product.name}</span>
            <div className="quantity-controls">
              <button onClick={() => onUpdateQuantity(item.productId, item.quantity - 1)}>-</button>
              <span>{item.quantity}</span>
              <button onClick={() => onUpdateQuantity(item.productId, item.quantity + 1)}>+</button>
            </div>
            <span>${(product.price * item.quantity).toFixed(2)}</span>
            <button onClick={() => onRemove(item.productId)}>Remove</button>
          </div>
        );
      })}
      <div className="cart-total">
        <strong>Total: ${total.toFixed(2)}</strong>
      </div>
    </div>
  );
}
```

**Lifting State Up Patterns:**

1. **Identify shared state:**
```typescript
// Two siblings need the same data
```

2. **Move to common parent:**
```typescript
function Parent() {
  const [sharedState, setSharedState] = useState(initialValue);
  return (
    <>
      <ChildA data={sharedState} />
      <ChildB data={sharedState} />
    </>
  );
}
```

3. **Pass down state:**
```typescript
<ChildA data={sharedState} />
```

4. **Pass up updates:**
```typescript
<ChildA onUpdate={setSharedState} />
```

**Common Errors:**
- Not lifting state high enough
- Over-lifting state to unnecessary parents
- Complex prop drilling
- Not identifying the right common parent
- Inconsistent data flow

**Best Practices:**
- Lift state to the lowest common parent
- Keep state as close to where it's used as possible
- Use callbacks for child-to-parent communication
- Consider Context API for deep prop drilling
- Document state ownership

---

### 25. Common State Mistakes

**What is it?**
Understanding and avoiding the most common mistakes developers make when working with React state.

**Why we need to avoid them:**
- Prevents bugs and errors
- Improves performance
- Makes code maintainable
- Follows React best practices
- Creates better user experience

**How to identify them:**
- Learn the patterns
- Understand React's rendering model
- Know the anti-patterns
- Review code regularly
- Use linting tools

**Common Mistakes and Solutions:**

### 1. Mutating State Directly

**❌ Wrong:**
```typescript
const [user, setUser] = useState({ name: 'John', age: 30 });
user.name = 'Jane'; // Direct mutation
setUser(user); // Won't trigger re-render
```

**✅ Correct:**
```typescript
const [user, setUser] = useState({ name: 'John', age: 30 });
setUser(prev => ({ ...prev, name: 'Jane' })); // Immutable update
```

### 2. Not Using Functional Updates

**❌ Wrong:**
```typescript
const [count, setCount] = useState(0);
const handleClick = () => {
  setCount(count + 1);
  setCount(count + 1);
  setCount(count + 1);
}; // Only increments by 1
```

**✅ Correct:**
```typescript
const [count, setCount] = useState(0);
const handleClick = () => {
  setCount(prev => prev + 1);
  setCount(prev => prev + 1);
  setCount(prev => prev + 1);
}; // Increments by 3
```

### 3. State Duplication

**❌ Wrong:**
```typescript
const [items, setItems] = useState([]);
const [itemCount, setItemCount] = useState(0);
const updateItems = (newItems) => {
  setItems(newItems);
  setItemCount(newItems.length); // Manual sync
};
```

**✅ Correct:**
```typescript
const [items, setItems] = useState([]);
const itemCount = items.length; // Derived
const updateItems = (newItems) => {
  setItems(newItems); // itemCount updates automatically
};
```

### 4. Initializing State with Expensive Operations

**❌ Wrong:**
```typescript
const [data, setData] = useState(expensiveFunction()); // Runs on every render
```

**✅ Correct:**
```typescript
const [data, setData] = useState(() => expensiveFunction()); // Runs once
```

### 5. Using State for Derived Values

**❌ Wrong:**
```typescript
const [firstName, setFirstName] = useState('');
const [lastName, setLastName] = useState('');
const [fullName, setFullName] = useState(''); // Derived state

const updateFirstName = (name) => {
  setFirstName(name);
  setFullName(`${name} ${lastName}`); // Manual sync
};
```

**✅ Correct:**
```typescript
const [firstName, setFirstName] = useState('');
const [lastName, setLastName] = useState('');
const fullName = `${firstName} ${lastName}`; // Derived

const updateFirstName = (name) => {
  setFirstName(name); // fullName updates automatically
};
```

### 6. Not Cleaning Up Side Effects

**❌ Wrong:**
```typescript
useEffect(() => {
  const interval = setInterval(() => setCount(c => c + 1), 1000);
  // No cleanup - memory leak
}, []);
```

**✅ Correct:**
```typescript
useEffect(() => {
  const interval = setInterval(() => setCount(c => c + 1), 1000);
  return () => clearInterval(interval); // Cleanup
}, []);
```

### 7. Overusing State

**❌ Wrong:**
```typescript
const [isHovered, setIsHovered] = useState(false);
const [isFocused, setIsFocused] = useState(false);
const [isVisible, setIsVisible] = useState(false);
// Many UI states that could be derived or handled differently
```

**✅ Correct:**
```typescript
// Use CSS for hover/focus
// Use refs for DOM measurements
// Keep only essential state
```

### 8. Storing Non-Serializable Data in State

**❌ Wrong:**
```typescript
const [data, setData] = useState(null);
useEffect(() => {
  const response = fetch('/api/data');
  setData(response); // Storing Promise
}, []);
```

**✅ Correct:**
```typescript
const [data, setData] = useState(null);
const [loading, setLoading] = useState(true);
useEffect(() => {
  fetch('/api/data')
    .then(response => response.json())
    .then(data => {
      setData(data);
      setLoading(false);
    });
}, []);
```

### 9. Complex State Updates in One Call

**❌ Wrong:**
```typescript
const [user, setUser] = useState({ name: '', email: '', age: 0 });
const updateUser = () => {
  setUser(prev => {
    const newState = { ...prev };
    newState.name = 'Jane';
    newState.email = 'jane@example.com';
    newState.age = 25;
    return newState;
  });
};
```

**✅ Correct:**
```typescript
const [user, setUser] = useState({ name: '', email: '', age: 0 });
const updateUser = () => {
  setUser(prev => ({
    ...prev,
    name: 'Jane',
    email: 'jane@example.com',
    age: 25
  }));
};
```

### 10. Ignoring TypeScript Type Safety

**❌ Wrong:**
```typescript
const [data, setData] = useState<any>(null); // Using any
```

**✅ Correct:**
```typescript
interface UserData {
  name: string;
  email: string;
}
const [data, setData] = useState<UserData | null>(null); // Proper typing
```

**How to Avoid These Mistakes:**

1. **Use ESLint with React rules:**
```json
{
  "rules": {
    "react-hooks/exhaustive-deps": "warn",
    "no-unused-vars": "warn"
  }
}
```

2. **Use TypeScript strict mode:**
```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

3. **Follow React best practices:**
- Always use functional updates when depending on previous state
- Never mutate state directly
- Keep state minimal and focused
- Use derived state when possible
- Clean up side effects

4. **Code review checklist:**
- [ ] Am I mutating state directly?
- [ ] Should I use functional updates?
- [ ] Am I duplicating state?
- [ ] Is this state derived from other state?
- [ ] Am I cleaning up side effects?
- [ ] Is my TypeScript typing correct?

**Common Errors:**
- Mutating state directly
- Not using functional updates
- State duplication
- Overusing state
- Not cleaning up side effects

**Best Practices:**
- Always use immutable updates
- Use functional updates when needed
- Avoid state duplication
- Keep state minimal
- Follow React conventions

---

## Functional Updates vs Direct Updates

### Why `setCount(count + 1)` Can Be Problematic

The issue with direct updates like `setCount(count + 1)` is that they use the **current** value of `count` at the time the function is called. If multiple state updates happen in quick succession, or if the component re-renders before the update is processed, you might be working with **stale** (outdated) state.

### The Problem with Direct Updates

```typescript
// ❌ PROBLEMATIC: Direct updates
function Counter() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(count + 1);  // Uses count = 0
    setCount(count + 1);  // Uses count = 0 (stale!)
    setCount(count + 1);  // Uses count = 0 (stale!)
  };
  
  // Result: count becomes 1, not 3!
}
```

### Why This Happens

1. **React Batching:** React batches multiple state updates together for performance
2. **Closure Capture:** The function captures the current value of `count` when it's created
3. **Asynchronous Updates:** State updates are not immediate
4. **Stale Closures:** By the time the update processes, `count` might have changed

### The Solution: Functional Updates

```typescript
// ✅ CORRECT: Functional updates
function Counter() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(prev => prev + 1);  // Always uses latest count
    setCount(prev => prev + 1);  // Always uses latest count
    setCount(prev => prev + 1);  // Always uses latest count
  };
  
  // Result: count becomes 3 as expected!
}
```

### Why Functional Updates Work

1. **Latest State:** React guarantees that `prev` is the most recent state
2. **No Stale Closures:** The function receives the current state when it executes
3. **Batching Safe:** Works correctly even when updates are batched
4. **Race Condition Free:** Multiple rapid updates work correctly

### Real-World Example: The Problem

```typescript
function ShoppingCartProblem() {
  const [cart, setCart] = useState({ items: [], total: 0 });

  const addMultipleItems = (itemsToAdd: number[]) => {
    itemsToAdd.forEach(item => {
      // ❌ PROBLEM: Uses stale cart.total
      setCart({
        items: [...cart.items, item],
        total: cart.total + getItemPrice(item)
      });
    });
  };

  // If called with [1, 2, 3], total might be calculated incorrectly
  // because cart.total is captured once at the start
}
```

### Real-World Example: The Solution

```typescript
function ShoppingCartSolution() {
  const [cart, setCart] = useState({ items: [], total: 0 });

  const addMultipleItems = (itemsToAdd: number[]) => {
    itemsToAdd.forEach(item => {
      // ✅ CORRECT: Always uses latest cart state
      setCart(prev => ({
        items: [...prev.items, item],
        total: prev.total + getItemPrice(item)
      }));
    });
  };

  // Each update uses the latest cart state
}
```

### Comparison Table

| Aspect | Direct Update | Functional Update |
|--------|---------------|-------------------|
| Syntax | `setState(newValue)` | `setState(prev => newValue)` |
| State Used | Current at function call | Latest when update processes |
| Multiple Updates | ❌ Problematic | ✅ Works correctly |
| Rapid Updates | ❌ Race conditions | ✅ Safe |
| Batched Updates | ❌ Stale state | ✅ Always current |
| Performance | ✅ Slightly faster | ✅ Negligible difference |
| Best Practice | ❌ Avoid when possible | ✅ Preferred |

### When to Use Each

**Use Direct Updates When:**
- New state doesn't depend on previous state
- Setting a completely new value
- Simple one-time updates
- Performance is critical (rarely the case)

**Use Functional Updates When:**
- New state depends on previous state
- Multiple rapid updates possible
- In event handlers or callbacks
- In setTimeout/setInterval
- Any time you're unsure

### Practical Examples

#### 1. Counter with Multiple Updates

```typescript
// ❌ Wrong
const handleRapidClicks = () => {
  setCount(count + 1);
  setCount(count + 1);
  setCount(count + 1);
}; // Only increments by 1

// ✅ Correct
const handleRapidClicks = () => {
  setCount(prev => prev + 1);
  setCount(prev => prev + 1);
  setCount(prev => prev + 1);
}; // Increments by 3
```

#### 2. Toggle Button

```typescript
// ❌ Wrong
const toggle = () => {
  setIsActive(!isActive); // Might use stale isActive
};

// ✅ Correct
const toggle = () => {
  setIsActive(prev => !prev); // Always uses latest
};
```

#### 3. Array Operations

```typescript
// ❌ Wrong
const addItem = (item) => {
  setItems([...items, item]); // Uses stale items
};

// ✅ Correct
const addItem = (item) => {
  setItems(prev => [...prev, item]); // Uses latest items
};
```

#### 4. Object Updates

```typescript
// ❌ Wrong
const updateUser = (field, value) => {
  setUser({ ...user, [field]: value }); // Uses stale user
};

// ✅ Correct
const updateUser = (field, value) => {
  setUser(prev => ({ ...prev, [field]: value })); // Uses latest
};
```

#### 5. Complex Calculations

```typescript
// ❌ Wrong
const complexUpdate = () => {
  const newValue = complexCalculation(count);
  setCount(newValue); // Might use stale count
};

// ✅ Correct
const complexUpdate = () => {
  setCount(prev => {
    const newValue = complexCalculation(prev);
    return newValue;
  }); // Always uses latest count
};
```

### The Golden Rule

**If your new state depends on the previous state, use functional updates.**

```typescript
// ✅ Good rule of thumb
const updateState = () => {
  // If you reference current state in the update
  if (needsCurrentState) {
    setState(prev => calculateNewState(prev));
  } else {
    setState(completelyNewValue);
  }
};
```

### Common Mistakes to Avoid

1. **❌ Mixing both approaches incorrectly:**
```typescript
setCount(count + 1);  // Direct
setCount(prev => prev + 1);  // Functional
// This can be confusing and error-prone
```

2. **❌ Assuming synchronous updates:**
```typescript
setCount(count + 1);
console.log(count); // Still shows old value!
```

3. **❌ Not using functional updates in loops:**
```typescript
for (let i = 0; i < 5; i++) {
  setCount(count + 1); // ❌ Uses same count 5 times
}

for (let i = 0; i < 5; i++) {
  setCount(prev => prev + 1); // ✅ Each uses latest
}
```

### Performance Considerations

Functional updates have negligible performance overhead. The benefits far outweigh any minimal performance cost. React optimizes functional updates internally, so you shouldn't worry about performance unless you're doing extremely complex calculations.

---

## Immutable Updates

### Why Immutability Matters in React

React uses **reference equality** to determine if state has changed. If you mutate an object or array directly, React won't detect the change and won't re-render your component.

### Object Immutability

**❌ Wrong - Direct Mutation:**
```typescript
const [user, setUser] = useState({ name: 'John', age: 30 });

// Direct mutation - React won't detect this!
user.name = 'Jane';
user.age = 25;
setUser(user); // Same reference, no re-render
```

**✅ Correct - Immutable Update:**
```typescript
const [user, setUser] = useState({ name: 'John', age: 30 });

// Create new object - React will detect this!
setUser({
  ...user,         // Copy existing properties
  name: 'Jane',   // Override name
  age: 25         // Override age
});
```

### Spread Operator for Objects

The spread operator (`...`) creates a shallow copy of an object:

```typescript
const original = { a: 1, b: 2, c: 3 };
const copy = { ...original };        // Shallow copy
const updated = { ...original, b: 3 }; // Copy with update
```

### Nested Object Updates

For nested objects, you need to copy each level:

```typescript
const [user, setUser] = useState({
  name: 'John',
  address: {
    street: '123 Main St',
    city: 'Springfield'
  }
});

// ❌ Wrong - mutating nested object
user.address.city = 'New York';
setUser(user);

// ✅ Correct - immutable nested update
setUser({
  ...user,
  address: {
    ...user.address,    // Copy nested object
    city: 'New York'    // Update nested property
  }
});
```

### Array Immutability

**❌ Wrong - Array Mutation:**
```typescript
const [items, setItems] = useState([1, 2, 3]);

// Direct mutation - React won't detect this!
items.push(4);
items.splice(1, 1);
items[0] = 99;
setItems(items); // Same reference, no re-render
```

**✅ Correct - Immutable Array Updates:**
```typescript
const [items, setItems] = useState([1, 2, 3]);

// Create new array - React will detect this!
setItems([...items, 4]);           // Add item
setItems(items.filter(i => i !== 2)); // Remove item
setItems(items.map(i => i === 1 ? 99 : i)); // Update item
```

### Common Array Operations

#### Add Item
```typescript
// Add to end
setItems(prev => [...prev, newItem]);

// Add to beginning
setItems(prev => [newItem, ...prev]);

// Add at specific index
setItems(prev => [
  ...prev.slice(0, index),
  newItem,
  ...prev.slice(index)
]);
```

#### Remove Item
```typescript
// Remove by index
setItems(prev => prev.filter((_, i) => i !== index));

// Remove by id
setItems(prev => prev.filter(item => item.id !== id));

// Remove first item
setItems(prev => prev.slice(1));

// Remove last item
setItems(prev => prev.slice(0, -1));
```

#### Update Item
```typescript
// Update by index
setItems(prev =>
  prev.map((item, i) =>
    i === index ? { ...item, ...updates } : item
  )
);

// Update by id
setItems(prev =>
  prev.map(item =>
    item.id === id ? { ...item, ...updates } : item
  )
);
```

#### Reorder Array
```typescript
// Move item from one index to another
setItems(prev => {
  const newArray = [...prev];
  const [removed] = newArray.splice(fromIndex, 1);
  newArray.splice(toIndex, 0, removed);
  return newArray;
});
```

### Nested State Updates

When you have deeply nested state, immutable updates can become complex. Here are some strategies:

#### Strategy 1: Manual Spread
```typescript
const [state, setState] = useState({
  user: {
    profile: {
      name: 'John',
      address: {
        city: 'Springfield'
      }
    }
  }
});

// Update nested city
setState(prev => ({
  ...prev,
  user: {
    ...prev.user,
    profile: {
      ...prev.user.profile,
      address: {
        ...prev.user.profile.address,
        city: 'New York'
      }
    }
  }
}));
```

#### Strategy 2: Helper Functions
```typescript
function updateNestedState<T>(state: T, updates: Partial<T>): T {
  return { ...state, ...updates };
}

// Usage
setState(prev => updateNestedState(prev.user.profile, { name: 'Jane' }));
```

#### Strategy 3: Immer Library (recommended for complex state)
```typescript
import { produce } from 'immer';

const [state, setState] = useState({
  user: {
    profile: {
      name: 'John',
      address: {
        city: 'Springfield'
      }
    }
  }
});

// Immer allows "mutation" syntax but handles immutability
setState(prev => produce(prev, draft => {
  draft.user.profile.address.city = 'New York';
}));
```

### Performance Considerations

For large objects or arrays, creating copies can be expensive. Consider:

1. **Use useMemo for expensive derived state**
2. **Consider normalization for complex data**
3. **Use Immer for complex nested updates**
4. **Profile performance before optimizing**

### Immutable Update Checklist

- [ ] Never mutate state directly
- [ ] Use spread operator for objects
- [ ] Use array methods that return new arrays
- [ ] Copy nested objects explicitly
- [ ] Use functional updates when depending on previous state
- [ ] Consider Immer for complex nested state

---

## Practical Project: Shopping Cart

Let's build a complete Shopping Cart application that demonstrates all the state management concepts we've learned.

### Project Overview

We'll create a shopping cart with:
- Product listing with state management
- Add/remove products to cart
- Quantity controls (increase/decrease)
- Total price calculation
- Cart count display
- Empty cart state
- Prop drilling demonstration
- Single source of truth

### Step 1: Project Setup

```bash
# Create new project
npm create vite@latest shopping-cart -- --template react-ts

# Navigate to project
cd shopping-cart

# Install dependencies
npm install

# Start development server
npm run dev
```

### Step 2: Define Types

Create `src/types/index.ts`:

```typescript
// src/types/index.ts
export interface Product {
  id: number;
  name: string;
  price: number;
  image: string;
  category: string;
  description: string;
  inStock: boolean;
  stock: number;
}

export interface CartItem {
  productId: number;
  quantity: number;
}

export interface CartState {
  items: CartItem[];
  total: number;
  count: number;
}
```

### Step 3: Create Product Data

Create `src/data/products.ts`:

```typescript
// src/data/products.ts
import { Product } from '../types';

export const products: Product[] = [
  {
    id: 1,
    name: 'Wireless Headphones',
    price: 99.99,
    image: 'https://images.unsplash.com/photo-1505740420928-5e560c06d30e?w=300',
    category: 'Electronics',
    description: 'High-quality wireless headphones with noise cancellation',
    inStock: true,
    stock: 15
  },
  {
    id: 2,
    name: 'Smart Watch',
    price: 199.99,
    image: 'https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=300',
    category: 'Electronics',
    description: 'Advanced smartwatch with health monitoring',
    inStock: true,
    stock: 8
  },
  {
    id: 3,
    name: 'Laptop Stand',
    price: 49.99,
    image: 'https://images.unsplash.com/photo-1527864550417-7fd91fc51a46?w=300',
    category: 'Accessories',
    description: 'Ergonomic aluminum laptop stand',
    inStock: true,
    stock: 20
  },
  {
    id: 4,
    name: 'Mechanical Keyboard',
    price: 149.99,
    image: 'https://images.unsplash.com/photo-1587829741301-dc798b91a803?w=300',
    category: 'Electronics',
    description: 'RGB mechanical keyboard with Cherry MX switches',
    inStock: false,
    stock: 0
  },
  {
    id: 5,
    name: 'Wireless Mouse',
    price: 29.99,
    image: 'https://images.unsplash.com/photo-1527864550417-7fd91fc51a46?w=300',
    category: 'Accessories',
    description: 'Ergonomic wireless mouse with precision tracking',
    inStock: true,
    stock: 25
  }
];
```

### Step 4: Create ProductCard Component

Create `src/components/ProductCard.tsx`:

```typescript
// src/components/ProductCard.tsx
import { Product } from '../types';

interface ProductCardProps {
  product: Product;
  onAddToCart: (productId: number) => void;
  isInCart: (productId: number) => boolean;
}

function ProductCard({ product, onAddToCart, isInCart }: ProductCardProps) {
  const inCart = isInCart(product.id);

  return (
    <div className={`product-card ${!product.inStock ? 'out-of-stock' : ''}`}>
      <div className="product-image">
        <img src={product.image} alt={product.name} />
        {!product.inStock && <div className="out-of-stock-badge">Out of Stock</div>}
      </div>
      
      <div className="product-info">
        <span className="product-category">{product.category}</span>
        <h3 className="product-name">{product.name}</h3>
        <p className="product-description">{product.description}</p>
        <div className="product-price-stock">
          <span className="product-price">${product.price.toFixed(2)}</span>
          <span className="product-stock">
            {product.inStock ? `${product.stock} in stock` : 'Out of stock'}
          </span>
        </div>
        
        <button
          className={`add-to-cart-btn ${inCart ? 'in-cart' : ''}`}
          onClick={() => onAddToCart(product.id)}
          disabled={!product.inStock}
        >
          {inCart ? '✓ In Cart' : 'Add to Cart'}
        </button>
      </div>
    </div>
  );
}

export default ProductCard;
```

### Step 5: Create ProductList Component

Create `src/components/ProductList.tsx`:

```typescript
// src/components/ProductList.tsx
import { Product } from '../types';
import ProductCard from './ProductCard';

interface ProductListProps {
  products: Product[];
  onAddToCart: (productId: number) => void;
  isInCart: (productId: number) => boolean;
}

function ProductList({ products, onAddToCart, isInCart }: ProductListProps) {
  return (
    <div className="product-list">
      <h2>Products</h2>
      <div className="products-grid">
        {products.map(product => (
          <ProductCard
            key={product.id}
            product={product}
            onAddToCart={onAddToCart}
            isInCart={isInCart}
          />
        ))}
      </div>
    </div>
  );
}

export default ProductList;
```

### Step 6: Create CartItem Component

Create `src/components/CartItem.tsx`:

```typescript
// src/components/CartItem.tsx
import { Product } from '../types';

interface CartItemProps {
  product: Product;
  quantity: number;
  onIncrease: (productId: number) => void;
  onDecrease: (productId: number) => void;
  onRemove: (productId: number) => void;
}

function CartItem({ product, quantity, onIncrease, onDecrease, onRemove }: CartItemProps) {
  return (
    <div className="cart-item">
      <div className="cart-item-image">
        <img src={product.image} alt={product.name} />
      </div>
      
      <div className="cart-item-details">
        <h4>{product.name}</h4>
        <p className="cart-item-price">${product.price.toFixed(2)}</p>
      </div>
      
      <div className="cart-item-quantity">
        <button
          onClick={() => onDecrease(product.id)}
          disabled={quantity <= 1}
          className="quantity-btn"
        >
          -
        </button>
        <span className="quantity">{quantity}</span>
        <button
          onClick={() => onIncrease(product.id)}
          disabled={quantity >= product.stock}
          className="quantity-btn"
        >
          +
        </button>
      </div>
      
      <div className="cart-item-total">
        <p>${(product.price * quantity).toFixed(2)}</p>
      </div>
      
      <button
        onClick={() => onRemove(product.id)}
        className="remove-btn"
      >
        Remove
      </button>
    </div>
  );
}

export default CartItem;
```

### Step 7: Create Cart Component

Create `src/components/Cart.tsx`:

```typescript
// src/components/Cart.tsx
import { Product, CartItem as CartItemType } from '../types';
import CartItem from './CartItem';

interface CartProps {
  cartItems: CartItemType[];
  products: Product[];
  onIncrease: (productId: number) => void;
  onDecrease: (productId: number) => void;
  onRemove: (productId: number) => void;
  onClearCart: () => void;
}

function Cart({ cartItems, products, onIncrease, onDecrease, onRemove, onClearCart }: CartProps) {
  // Calculate cart total - derived state
  const cartTotal = cartItems.reduce((total, cartItem) => {
    const product = products.find(p => p.id === cartItem.productId);
    return total + (product ? product.price * cartItem.quantity : 0);
  }, 0);

  // Calculate cart count - derived state
  const cartCount = cartItems.reduce((count, item) => count + item.quantity, 0);

  if (cartItems.length === 0) {
    return (
      <div className="cart empty-cart">
        <div className="empty-cart-icon">🛒</div>
        <h3>Your cart is empty</h3>
        <p>Add some products to get started!</p>
      </div>
    );
  }

  return (
    <div className="cart">
      <div className="cart-header">
        <h2>Shopping Cart ({cartCount} items)</h2>
        <button onClick={onClearCart} className="clear-cart-btn">
          Clear Cart
        </button>
      </div>

      <div className="cart-items">
        {cartItems.map(cartItem => {
          const product = products.find(p => p.id === cartItem.productId);
          if (!product) return null;

          return (
            <CartItem
              key={cartItem.productId}
              product={product}
              quantity={cartItem.quantity}
              onIncrease={onIncrease}
              onDecrease={onDecrease}
              onRemove={onRemove}
            />
          );
        })}
      </div>

      <div className="cart-footer">
        <div className="cart-summary">
          <div className="summary-row">
            <span>Subtotal</span>
            <span>${cartTotal.toFixed(2)}</span>
          </div>
          <div className="summary-row">
            <span>Tax (8%)</span>
            <span>${(cartTotal * 0.08).toFixed(2)}</span>
          </div>
          <div className="summary-row total">
            <span>Total</span>
            <span>${(cartTotal * 1.08).toFixed(2)}</span>
          </div>
        </div>

        <button className="checkout-btn">
          Proceed to Checkout
        </button>
      </div>
    </div>
  );
}

export default Cart;
```

### Step 8: Create Main App Component

Create `src/App.tsx`:

```typescript
// src/App.tsx
import { useState, useMemo } from 'react';
import { Product, CartItem } from './types';
import { products } from './data/products';
import ProductList from './components/ProductList';
import Cart from './components/Cart';

function App() {
  // Single source of truth for cart state
  const [cartItems, setCartItems] = useState<CartItem[]>([]);

  // Check if product is in cart - derived state
  const isInCart = (productId: number): boolean => {
    return cartItems.some(item => item.productId === productId);
  };

  // Add product to cart
  const addToCart = (productId: number) => {
    setCartItems(prevCartItems => {
      const existingItem = prevCartItems.find(item => item.productId === productId);
      
      if (existingItem) {
        // Product already in cart, increase quantity
        return prevCartItems.map(item =>
          item.productId === productId
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      } else {
        // Product not in cart, add it
        return [...prevCartItems, { productId, quantity: 1 }];
      }
    });
  };

  // Remove product from cart
  const removeFromCart = (productId: number) => {
    setCartItems(prevCartItems => 
      prevCartItems.filter(item => item.productId !== productId)
    );
  };

  // Increase quantity
  const increaseQuantity = (productId: number) => {
    setCartItems(prevCartItems => {
      const product = products.find(p => p.id === productId);
      const cartItem = prevCartItems.find(item => item.productId === productId);
      
      // Check if we can increase (not exceeding stock)
      if (product && cartItem && cartItem.quantity < product.stock) {
        return prevCartItems.map(item =>
          item.productId === productId
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      
      return prevCartItems;
    });
  };

  // Decrease quantity
  const decreaseQuantity = (productId: number) => {
    setCartItems(prevCartItems => {
      const cartItem = prevCartItems.find(item => item.productId === productId);
      
      if (cartItem && cartItem.quantity > 1) {
        return prevCartItems.map(item =>
          item.productId === productId
            ? { ...item, quantity: item.quantity - 1 }
            : item
        );
      }
      
      // If quantity would go to 0, remove the item
      return prevCartItems.filter(item => item.productId !== productId);
    });
  };

  // Clear entire cart
  const clearCart = () => {
    setCartItems([]);
  };

  // Calculate cart count - derived state
  const cartCount = useMemo(() => 
    cartItems.reduce((count, item) => count + item.quantity, 0),
    [cartItems]
  );

  return (
    <div className="app">
      <header className="app-header">
        <h1>🛒 Shopping Cart Demo</h1>
        <div className="cart-indicator">
          <span className="cart-icon">🛒</span>
          <span className="cart-count">{cartCount}</span>
        </div>
      </header>

      <main className="app-main">
        <div className="content-container">
          <div className="products-section">
            <ProductList
              products={products}
              onAddToCart={addToCart}
              isInCart={isInCart}
            />
          </div>

          <div className="cart-section">
            <Cart
              cartItems={cartItems}
              products={products}
              onIncrease={increaseQuantity}
              onDecrease={decreaseQuantity}
              onRemove={removeFromCart}
              onClearCart={clearCart}
            />
          </div>
        </div>
      </main>

      <footer className="app-footer">
        <p>State Management Demo - React Session 3</p>
      </footer>
    </div>
  );
}

export default App;
```

### Step 9: Add Styling

Add to `src/index.css`:

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: #f5f5f5;
  color: #333;
}

.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.app-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.app-header h1 {
  font-size: 24px;
}

.cart-indicator {
  display: flex;
  align-items: center;
  gap: 8px;
  background: rgba(255, 255, 255, 0.2);
  padding: 8px 16px;
  border-radius: 20px;
}

.cart-icon {
  font-size: 20px;
}

.cart-count {
  font-weight: bold;
}

.app-main {
  flex: 1;
  padding: 20px;
}

.content-container {
  max-width: 1400px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 20px;
}

.products-section {
  background: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.cart-section {
  background: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  height: fit-content;
  position: sticky;
  top: 20px;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

.product-card {
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.3s, box-shadow 0.3s;
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.product-card.out-of-stock {
  opacity: 0.6;
}

.product-image {
  position: relative;
  height: 200px;
  overflow: hidden;
}

.product-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.out-of-stock-badge {
  position: absolute;
  top: 10px;
  right: 10px;
  background: #e74c3c;
  color: white;
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 12px;
  font-weight: bold;
}

.product-info {
  padding: 16px;
}

.product-category {
  color: #666;
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.product-name {
  margin: 8px 0;
  font-size: 16px;
  font-weight: 600;
}

.product-description {
  color: #666;
  font-size: 14px;
  margin-bottom: 12px;
  line-height: 1.4;
}

.product-price-stock {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.product-price {
  font-size: 20px;
  font-weight: bold;
  color: #2ecc71;
}

.product-stock {
  font-size: 12px;
  color: #666;
}

.add-to-cart-btn {
  width: 100%;
  padding: 12px;
  background: #3498db;
  color: white;
  border: none;
  border-radius: 6px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.3s;
}

.add-to-cart-btn:hover:not(:disabled) {
  background: #2980b9;
}

.add-to-cart-btn:disabled {
  background: #bdc3c7;
  cursor: not-allowed;
}

.add-to-cart-btn.in-cart {
  background: #2ecc71;
}

.cart h2 {
  margin-bottom: 20px;
}

.cart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.clear-cart-btn {
  padding: 8px 16px;
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.cart-items {
  max-height: 400px;
  overflow-y: auto;
  margin-bottom: 20px;
}

.cart-item {
  display: grid;
  grid-template-columns: 60px 1fr auto auto auto;
  gap: 12px;
  align-items: center;
  padding: 12px;
  border-bottom: 1px solid #e0e0e0;
}

.cart-item-image img {
  width: 60px;
  height: 60px;
  object-fit: cover;
  border-radius: 4px;
}

.cart-item-details h4 {
  font-size: 14px;
  margin-bottom: 4px;
}

.cart-item-price {
  color: #666;
  font-size: 14px;
}

.cart-item-quantity {
  display: flex;
  align-items: center;
  gap: 8px;
}

.quantity-btn {
  width: 30px;
  height: 30px;
  border: 1px solid #ddd;
  background: white;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.quantity-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.quantity {
  font-weight: bold;
  min-width: 30px;
  text-align: center;
}

.cart-item-total {
  font-weight: bold;
  min-width: 80px;
  text-align: right;
}

.remove-btn {
  padding: 6px 12px;
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
}

.empty-cart {
  text-align: center;
  padding: 40px 20px;
}

.empty-cart-icon {
  font-size: 48px;
  margin-bottom: 16px;
}

.cart-footer {
  border-top: 2px solid #e0e0e0;
  padding-top: 20px;
}

.cart-summary {
  margin-bottom: 20px;
}

.summary-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
}

.summary-row.total {
  font-size: 18px;
  font-weight: bold;
  margin-top: 12px;
  padding-top: 12px;
  border-top: 1px solid #e0e0e0;
}

.checkout-btn {
  width: 100%;
  padding: 16px;
  background: #2ecc71;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.3s;
}

.checkout-btn:hover {
  background: #27ae60;
}

.app-footer {
  background: #333;
  color: white;
  text-align: center;
  padding: 20px;
  margin-top: auto;
}

@media (max-width: 768px) {
  .content-container {
    grid-template-columns: 1fr;
  }
  
  .cart-section {
    position: static;
  }
  
  .cart-item {
    grid-template-columns: 1fr;
    text-align: center;
  }
  
  .cart-item-quantity,
  .cart-item-total {
    justify-self: center;
  }
}
```

### Step 10: Prop Drilling Demonstration

Notice how we're passing cart state and functions through multiple component levels:

```
App (holds cart state)
  ↓ props
ProductList
  ↓ props
ProductCard (receives onAddToCart and isInCart)

App (holds cart state)
  ↓ props
Cart
  ↓ props
CartItem (receives onIncrease, onDecrease, onRemove)
```

This demonstrates **prop drilling** - passing data through intermediate components that don't need it. In a real application, you might use Context API or state management libraries to avoid this.

### Step 11: Key State Management Concepts Demonstrated

1. **Single Source of Truth:** Cart state is managed in App component only
2. **Derived State:** cartCount, cartTotal are calculated from cartItems
3. **Functional Updates:** All cart updates use functional forms
4. **Immutable Updates:** Array operations use filter, map, spread operator
5. **State Duplication Avoided:** No separate cartCount or cartTotal state
6. **Lifting State Up:** Cart state lifted to App to share between components

### Step 12: Testing the Application

```bash
# Make sure the dev server is running
npm run dev
```

Open your browser and navigate to `http://localhost:5173` to test your shopping cart!

### Step 13: Exercise for Students

Try these modifications to reinforce your understanding:

1. **Add a quantity selector in ProductCard** to add multiple items at once
2. **Implement a wishlist feature** with its own state
3. **Add product filtering** by category with derived state
4. **Implement a discount code system** that affects the total
5. **Add a search function** for products

---

## Comprehensive Review

### Session Summary

In this session, we covered:

1. **State Fundamentals:** What state is, how it differs from variables and props, and why React state exists
2. **useState Hook:** Syntax, initial state, and how to update state properly
3. **State Types:** String, number, boolean, array, and object state management
4. **State Updates:** Proper immutable updates for objects and arrays
5. **Functional Updates:** Why and when to use functional updates vs direct updates
6. **State Patterns:** Derived state, avoiding duplication, single source of truth, lifting state up
7. **Common Mistakes:** Anti-patterns to avoid and best practices to follow
8. **Practical Application:** Built a complete shopping cart demonstrating all concepts

### Key Takeaways

- State is component-internal data that triggers re-renders when changed
- Always use functional updates when new state depends on previous state
- Never mutate state directly - always use immutable patterns
- Avoid state duplication - use derived state instead
- Maintain single source of truth for your data
- Lift state up to the lowest common parent when sharing between components
- Use TypeScript interfaces for type safety
- State updates are asynchronous and batched by React

---

## 15 Student Questions

1. What is the difference between state and regular variables in React?
2. Why should you use functional updates instead of direct updates?
3. What happens when you mutate state directly instead of using setState?
4. Explain the concept of derived state and why it's useful.
5. What is state duplication and why should you avoid it?
6. What does "single source of truth" mean in React state management?
7. When should you lift state up to a parent component?
8. How do you properly update an object in React state?
9. How do you properly update an array in React state?
10. What is the difference between props and state?
11. Why does React need its own state management system?
12. What are the benefits of using TypeScript interfaces for state?
13. How do you handle nested object updates in React state?
14. What is the purpose of the useState hook?
15. Why are state updates asynchronous in React?

---

## 5 Interview Questions

### 1. Explain the difference between functional updates and direct updates in React, and provide examples of when to use each.

**Answer:** Functional updates (`setState(prev => prev + 1)`) receive the previous state as an argument and are guaranteed to work with the most recent state. Direct updates (`setState(value + 1)`) use the state value at the time the function is called, which can be stale if multiple updates occur rapidly.

Use functional updates when:
- New state depends on previous state
- Multiple rapid updates might occur
- In event handlers or callbacks
- When working with timers or intervals

Use direct updates when:
- New state doesn't depend on previous state
- Setting a completely new value
- Simple one-time updates

### 2. How does React's rendering system work with state changes, and why are state updates asynchronous?

**Answer:** When state changes, React schedules a re-render of the component. The update is asynchronous because React batches multiple state updates together for performance. This means if you call setState multiple times, React will process them together and only trigger one re-render.

The process:
1. setState is called
2. React schedules an update
3. React batches multiple updates
4. Re-render occurs with new state
5. Virtual DOM is compared with previous
6. Minimal DOM updates are applied

This batching improves performance and prevents unnecessary re-renders.

### 3. Explain the concept of immutability in React and why it's important for state management.

**Answer:** Immutability means never modifying existing data structures directly. In React, this is crucial because React uses reference equality to detect changes. If you mutate an object or array directly, React won't detect the change and won't re-render.

Immutable updates create new references:
- Objects: Use spread operator (`{ ...obj, key: value }`)
- Arrays: Use methods that return new arrays (`filter`, `map`, `[...arr, item]`)
- Nested: Copy each level explicitly

Benefits:
- Predictable state changes
- Easy to track changes
- Enables time-travel debugging
- Optimizes React's reconciliation

### 4. What is derived state and how does it help avoid state duplication?

**Answer:** Derived state is calculated from other state or props during rendering rather than being stored separately. It eliminates state duplication by ensuring there's only one source of truth for each piece of data.

Example:
```typescript
// ❌ State duplication
const [items, setItems] = useState([]);
const [itemCount, setItemCount] = useState(0);

// ✅ Derived state
const [items, setItems] = useState([]);
const itemCount = items.length; // Derived
```

Benefits:
- No synchronization issues
- Simpler code
- Easier to maintain
- Automatically stays in sync
- Reduces complexity

### 5. Describe the "lifting state up" pattern and when you would use it.

**Answer:** Lifting state up involves moving state from child components to their nearest common parent when multiple components need to share or access the same data.

When to use it:
- Sibling components need to share data
- Parent needs to control child data
- Centralizing state management
- Avoiding prop drilling (though Context might be better for deep nesting)

Process:
1. Identify shared state
2. Move to common parent
3. Pass down via props
4. Pass up via callback functions
5. Parent becomes the single source of truth

Example:
```typescript
function Parent() {
  const [value, setValue] = useState('');
  return (
    <>
      <ChildA value={value} onChange={setValue} />
      <ChildB value={value} />
    </>
  );
}
```

---

## 5 Practical Exercises

### Exercise 1: Todo App with State Management

Create a todo application with:
- Add, remove, and toggle todos
- Filter by status (all, active, completed)
- Statistics (total, completed, active)
- Use functional updates for all state changes
- Use derived state for statistics
- Implement proper TypeScript interfaces

**Requirements:**
- No state duplication
- Immutable array updates
- Functional state updates
- Single source of truth

### Exercise 2: Form with Complex State

Build a registration form with:
- Multiple form fields (name, email, password, confirm password)
- Real-time validation
- Password strength indicator
- Form submission with loading state
- Error handling
- State for form data, validation errors, loading, and success

**Requirements:**
- Object state management
- Immutable object updates
- Derived validation state
- Conditional state updates

### Exercise 3: Product Filter System

Create a product filter system with:
- Product list with multiple properties
- Filter by category, price range, and search query
- Sort by different criteria
- Statistics (filtered count, average price)
- Active filter indicators
- Clear all filters functionality

**Requirements:**
- Array state management
- Derived filtered and sorted arrays
- Multiple state variables
- Complex derived state

### Exercise 4: Counter with Multiple Features

Build an advanced counter with:
- Increment, decrement, reset
- Step size control
- Min/max values
- History of changes
- Undo/redo functionality
- Persist to localStorage

**Requirements:**
- Number state management
- Array state for history
- Functional updates
- Complex state logic
- Side effects with localStorage

### Exercise 5: User Management System

Create a user management system with:
- Add, edit, delete users
- User search and filter
- User role management
- Bulk operations
- Pagination
- Statistics dashboard

**Requirements:**
- Complex object state
- Array operations
- Multiple state variables
- Derived state for filtering/pagination
- Immutable updates throughout

---

## Homework

### Reading Assignment
1. Read the official React documentation on "State and Lifecycle"
2. Read the React documentation on "Hooks API Reference - useState"
3. Review TypeScript documentation on "Interfaces" and "Type Inference"

### Practice Exercises
1. **State Management Refactoring:** Take a component with state duplication and refactor it to use derived state instead.

2. **Functional Updates Practice:** Convert a component with direct state updates to use functional updates throughout.

3. **Immutable Updates:** Create a complex nested state object and implement proper immutable update functions for all nested properties.

4. **Shopping Cart Enhancement:** Enhance the shopping cart project we built by adding:
   - Product categories with filtering
   - Search functionality
   - Wishlist feature
   - Quantity quick-add in product cards
   - Discount code system

### Research Project
Research and write a brief comparison (500 words) of different state management approaches in React:
- useState (local component state)
- Context API (global state)
- Redux (centralized state management)
- Zustand (modern state management)
- Recoil (experimental state management)

Include pros and cons of each approach and recommend use cases for each.

---

## Challenge Exercise

### Advanced E-commerce Application

Build a complete e-commerce application that demonstrates advanced state management concepts:

#### Requirements:

1. **Product Management:**
   - Product listing with multiple categories
   - Advanced filtering (price range, brand, rating, features)
   - Sorting options
   - Search with autocomplete
   - Product comparison feature

2. **Shopping Cart:**
   - Add/remove products
   - Quantity controls with stock limits
   - Bulk operations
   - Save for later functionality
   - Cart persistence across sessions
   - Promo code system with validation

3. **User Experience:**
   - Recently viewed products
   - Product recommendations
   - User reviews and ratings
   - Wishlist management
   - Compare products side-by-side

4. **State Management Requirements:**
   - No state duplication
   - All derived state properly calculated
   - Functional updates for all state changes
   - Immutable updates for all objects and arrays
   - Single source of truth for all data
   - Proper TypeScript typing throughout

5. **Advanced Features:**
   - Virtual scrolling for large product lists
   - Lazy loading for product images
   - Optimistic UI updates
   - Error boundaries for state errors
   - Performance monitoring

#### Technical Requirements:

- Use functional updates for all state changes
- Implement proper immutable update patterns
- Use derived state wherever possible
- Avoid prop drilling where reasonable
- Implement proper error handling
- Add loading states for async operations
- Use TypeScript with strict mode
- Follow React best practices

#### Bonus Features:
- Add unit tests for state management logic
- Implement time-travel debugging for cart operations
- Add analytics tracking for user interactions
- Implement A/B testing for product displays
- Add internationalization support

#### Evaluation Criteria:
- Code quality and organization
- Proper state management patterns
- TypeScript type safety
- User experience
- Performance considerations
- Error handling
- Scalability of the solution

This challenge will test your understanding of all state management concepts and push you to apply them in a real-world scenario.

---

## Common Mistakes to Avoid

### 1. Mutating State Directly
```typescript
// ❌ WRONG
user.name = 'Jane';
setUser(user);

// ✅ CORRECT
setUser(prev => ({ ...prev, name: 'Jane' }));
```

### 2. Not Using Functional Updates
```typescript
// ❌ WRONG
setCount(count + 1);
setCount(count + 1);

// ✅ CORRECT
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

### 3. State Duplication
```typescript
// ❌ WRONG
const [items, setItems] = useState([]);
const [itemCount, setItemCount] = useState(0);

// ✅ CORRECT
const [items, setItems] = useState([]);
const itemCount = items.length;
```

### 4. Expensive Initial State
```typescript
// ❌ WRONG
const [data, setData] = useState(expensiveFunction());

// ✅ CORRECT
const [data, setData] = useState(() => expensiveFunction());
```

### 5. Not Cleaning Up Side Effects
```typescript
// ❌ WRONG
useEffect(() => {
  const interval = setInterval(() => {}, 1000);
}, []);

// ✅ CORRECT
useEffect(() => {
  const interval = setInterval(() => {}, 1000);
  return () => clearInterval(interval);
}, []);
```

### 6. Using `any` Type
```typescript
// ❌ WRONG
const [data, setData] = useState<any>(null);

// ✅ CORRECT
interface DataType {
  // proper types
}
const [data, setData] = useState<DataType | null>(null);
```

### 7. Complex State in Single useState
```typescript
// ❌ WRONG
const [state, setState] = useState({
  users: [],
  products: [],
  cart: [],
  filters: {},
  ui: {}
});

// ✅ CORRECT
const [users, setUsers] = useState([]);
const [products, setProducts] = useState([]);
const [cart, setCart] = useState([]);
```

### 8. Not Handling Loading/Error States
```typescript
// ❌ WRONG
const [data, setData] = useState(null);
fetch('/api/data').then(setData);

// ✅ CORRECT
const [data, setData] = useState(null);
const [loading, setLoading] = useState(true);
const [error, setError] = useState(null);

fetch('/api/data')
  .then(setData)
  .catch(setError)
  .finally(() => setLoading(false));
```

---

## Additional Resources

- [React Documentation - State and Lifecycle](https://react.dev/learn/state-a-components-memory)
- [React Documentation - Hooks API Reference](https://react.dev/reference/react)
- [TypeScript Documentation - Interfaces](https://www.typescriptlang.org/docs/handbook/2/interfaces.html)
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)
- [Immer Library for Immutable Updates](https://immerjs.github.io/immer/)

---

**Congratulations on completing Session 3!** You now have a solid understanding of React state management with useState. Continue practicing with the exercises and challenge to reinforce these concepts before moving to the next session on useEffect and other hooks.