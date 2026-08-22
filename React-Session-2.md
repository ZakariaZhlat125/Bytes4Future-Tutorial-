# React.js Session 2: Props, Events, Conditional Rendering, and Lists

**Duration:** 3 hours  
**Level:** Beginner  
**Prerequisites:** Session 1 completed (JSX, Components, Functional Components, Component Composition, Tailwind CSS)

---

## Session Timeline

### Part 1: Props (1 hour)
- **0:00-0:05:** Understanding Props, Parent → Child communication (Topics 1-2)
- **0:05-0:10:** Props Syntax (Topic 3)
- **0:10-0:20:** Basic Props Types - String, Number, Boolean (Topics 4-6)
- **0:20-0:30:** Complex Props Types - Object, Array, Function (Topics 7-9)
- **0:30-0:35:** children Prop (Topic 10)
- **0:35-0:45:** Props Destructuring, TypeScript Interfaces (Topics 11-12)
- **0:45-0:50:** Default Props (Topic 13)
- **0:50-1:00:** Props Practice Exercises

### Part 2: Events (45 minutes)
- **1:00-1:05:** Events in React (Topic 14)
- **1:05-1:15:** onClick Event (Topic 15)
- **1:15-1:25:** onChange Event (Topic 16)
- **1:25-1:30:** onSubmit Event (Topic 17)
- **1:30-1:40:** Event Object (Topic 18)
- **1:40-1:45:** Passing arguments to event handlers (Topic 19)
- **1:45-1:45:** Events Practice Exercises

### Part 3: Conditional Rendering (45 minutes)
- **1:45-1:50:** Conditional Rendering Overview (Topic 20)
- **1:50-1:55:** if statements (Topic 21)
- **1:55-2:00:** Ternary operator (Topic 22)
- **2:00-2:05:** Logical AND operator (Topic 23)
- **2:05-2:15:** Loading UI (Topic 24)
- **2:15-2:20:** Empty State (Topic 25)
- **2:20-2:25:** Error State (Topic 26)
- **2:25-2:30:** Conditional Rendering Practice

### Part 4: Rendering Lists (30 minutes)
- **2:30-2:35:** Rendering Lists Overview (Topic 27)
- **2:35-2:40:** map() method (Topic 28)
- **2:40-2:45:** Keys in React (Topic 29)
- **2:50-2:55:** Why keys are important (Topic 30)
- **2:55-3:00:** Why array index as key is problematic (Topic 31)

---

## Part 1: Props

### 1. Understanding Props

**What is it?**
Props (short for "properties") are the mechanism by which data is passed from parent components to child components in React. They are read-only and help create reusable, dynamic components.

**Why we need it?**
- Enables component reusability with different data
- Creates a clear data flow (unidirectional data flow)
- Makes components predictable and easier to debug
- Allows parent components to control child component behavior
- Essential for component composition

**How it works?**
- Parent component passes data as attributes to child component
- Child component receives data as function parameters
- Props are immutable (cannot be modified by child component)
- Changes in props trigger re-render of child component

**Syntax:**
```tsx
// Parent component
<ChildComponent propName={value} />

// Child component
function ChildComponent({ propName }: { propName: type }) {
  // use propName here
}
```

**Simple Example:**
```tsx
// Child component
function Greeting({ name }: { name: string }) {
  return <h1>Hello, {name}!</h1>;
}

// Parent component
function App() {
  return <Greeting name="John" />;
}
```

**Real-world Example:**
```tsx
// UserCard component
function UserCard({ name, email, avatar }: { name: string; email: string; avatar: string }) {
  return (
    <div className="user-card">
      <img src={avatar} alt={name} />
      <h3>{name}</h3>
      <p>{email}</p>
    </div>
  );
}

// Using UserCard with different data
function App() {
  return (
    <div>
      <UserCard 
        name="John Doe" 
        email="john@example.com" 
        avatar="/avatar1.jpg" 
      />
      <UserCard 
        name="Jane Smith" 
        email="jane@example.com" 
        avatar="/avatar2.jpg" 
      />
    </div>
  );
}
```

**Common Errors:**
- Trying to modify props directly in child component
- Not providing required props
- Passing wrong data types
- Forgetting to destructure props in function parameters

**Best Practices:**
- Always define TypeScript interfaces for props
- Use meaningful prop names
- Provide default values when appropriate
- Keep props minimal and focused
- Document complex props with comments

---

### 2. Parent → Child Communication

**What is it?**
The pattern of data flowing from parent components down to child components through props, establishing a clear hierarchy and data flow direction.

**Why we need it?**
- Establishes predictable data flow
- Makes debugging easier
- Prevents circular dependencies
- Creates clear component hierarchy
- Enables state management patterns

**How it works?**
- Parent component holds the data
- Parent passes data as props to children
- Children receive and display data
- Children cannot modify parent data directly
- Communication is one-way (top-down)

**Syntax:**
```tsx
// Parent
function Parent() {
  const data = "some data";
  return <Child data={data} />;
}

// Child
function Child({ data }: { data: string }) {
  return <div>{data}</div>;
}
```

**Simple Example:**
```tsx
function Parent() {
  const message = "Hello from parent!";
  return <Child message={message} />;
}

function Child({ message }: { message: string }) {
  return <p>{message}</p>;
}
```

**Real-world Example:**
```tsx
// Parent manages user data
function UserProfile() {
  const [user, setUser] = useState({
    name: "John Doe",
    email: "john@example.com",
    role: "Admin"
  });

  return (
    <div className="profile">
      <UserHeader name={user.name} role={user.role} />
      <UserDetails email={user.email} />
      <UserActions role={user.role} />
    </div>
  );
}

// Child components receive specific data
function UserHeader({ name, role }: { name: string; role: string }) {
  return (
    <header>
      <h1>{name}</h1>
      <span className="badge">{role}</span>
    </header>
  );
}

function UserDetails({ email }: { email: string }) {
  return <section>Contact: {email}</section>;
}

function UserActions({ role }: { role: string }) {
  return (
    <div>
      {role === "Admin" && <button>Edit User</button>}
      <button>View Profile</button>
    </div>
  );
}
```

**Common Errors:**
- Trying to pass data from child to parent via props (needs callbacks)
- Overloading parent with too much state
- Not lifting state up when needed
- Creating deeply nested prop chains

**Best Practices:**
- Keep state as close to where it's needed as possible
- Use callback props for child-to-parent communication
- Consider Context API for deeply nested props
- Keep component interfaces clean and focused

---

### 3. Props Syntax

**What is it?**
The syntax rules and patterns for passing and receiving props in React components.

**Why we need it?**
- Ensures proper data passing
- Maintains type safety with TypeScript
- Follows React conventions
- Enables proper component usage
- Prevents runtime errors

**How it works?**
- Props are passed as HTML-like attributes
- Multiple props can be passed to a component
- Props can be static values or JavaScript expressions
- TypeScript ensures type correctness

**Syntax:**
```tsx
// Basic syntax
<Component prop1="value" prop2={123} prop3={true} />

// With expressions
<Component value={variable} result={calculation()} />

// Spread syntax
<Component {...propsObject} />
```

**Simple Example:**
```tsx
function Button({ label, onClick, disabled }: { 
  label: string; 
  onClick: () => void; 
  disabled: boolean 
}) {
  return (
    <button onClick={onClick} disabled={disabled}>
      {label}
    </button>
  );
}

function App() {
  return (
    <Button 
      label="Click me" 
      onClick={() => console.log('clicked')} 
      disabled={false} 
    />
  );
}
```

**Real-world Example:**
```tsx
interface ProductCardProps {
  id: number;
  name: string;
  price: number;
  inStock: boolean;
  onAddToCart: (id: number) => void;
  className?: string;
}

function ProductCard({ 
  id, 
  name, 
  price, 
  inStock, 
  onAddToCart, 
  className = '' 
}: ProductCardProps) {
  return (
    <div className={`product-card ${className}`}>
      <h3>{name}</h3>
      <p>${price}</p>
      <button 
        onClick={() => onAddToCart(id)}
        disabled={!inStock}
      >
        {inStock ? 'Add to Cart' : 'Out of Stock'}
      </button>
    </div>
  );
}

// Usage
function App() {
  const handleAddToCart = (id: number) => {
    console.log(`Added product ${id} to cart`);
  };

  return (
    <ProductCard
      id={1}
      name="Laptop"
      price={999}
      inStock={true}
      onAddToCart={handleAddToCart}
      className="featured"
    />
  );
}
```

**Common Errors:**
- Forgetting curly braces for JavaScript expressions
- Using reserved words as prop names
- Not defining TypeScript interfaces
- Incorrect prop types

**Best Practices:**
- Always define TypeScript interfaces for props
- Use camelCase for prop names
- Provide default values for optional props
- Use spread operator for passing multiple props
- Keep prop names descriptive and consistent

---

### 4. String Props

**What is it?**
Props that accept string values, the most common type of prop in React components.

**Why we need it?**
- Display text content
- Pass CSS class names
- Provide URLs and paths
- Set attribute values
- Configuration strings

**How it works?**
- Can be passed as literal strings or expressions
- Automatically escaped to prevent XSS
- Can be concatenated or manipulated
- TypeScript ensures string type safety

**Syntax:**
```tsx
// Literal string
<Component text="Hello" />

// Expression
<Component text={variable} />

// Template literal
<Component text={`Hello ${name}`} />
```

**Simple Example:**
```tsx
function Alert({ message, type }: { message: string; type: string }) {
  return (
    <div className={`alert alert-${type}`}>
      {message}
    </div>
  );
}

function App() {
  return (
    <Alert message="Operation successful" type="success" />
  );
}
```

**Real-world Example:**
```tsx
interface LinkProps {
  href: string;
  label: string;
  target?: string;
  className?: string;
}

function Link({ href, label, target = '_self', className = '' }: LinkProps) {
  return (
    <a href={href} target={target} className={className}>
      {label}
    </a>
  );
}

function Navigation() {
  return (
    <nav>
      <Link href="/home" label="Home" />
      <Link href="/about" label="About" />
      <Link href="/contact" label="Contact" target="_blank" />
    </nav>
  );
}
```

**Common Errors:**
- Not handling empty strings
- Forgetting to escape user input (React handles this automatically)
- Passing numbers when strings are expected
- Not trimming whitespace

**Best Practices:**
- Validate string formats (emails, URLs)
- Handle empty strings gracefully
- Use template literals for complex strings
- Consider string length limits
- Sanitize user input when necessary

---

### 5. Number Props

**What is it?**
Props that accept numeric values for calculations, counts, sizes, and other quantitative data.

**Why we need it?**
- Perform calculations
- Set counts and limits
- Define sizes and dimensions
- Handle ratings and scores
- Configure numeric parameters

**How it works?**
- Passed as JavaScript expressions (curly braces)
- Can be used in mathematical operations
- TypeScript ensures number type safety
- Can be validated for ranges

**Syntax:**
```tsx
// Number expression
<Component count={5} price={99.99} />

// Calculation
<Component total={price * quantity} />

// Variable
<Component age={userAge} />
```

**Simple Example:**
```tsx
function Counter({ initialCount }: { initialCount: number }) {
  const [count, setCount] = useState(initialCount);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  );
}

function App() {
  return <Counter initialCount={10} />;
}
```

**Real-world Example:**
```tsx
interface ProgressBarProps {
  value: number;
  max: number;
  showPercentage?: boolean;
}

function ProgressBar({ value, max, showPercentage = true }: ProgressBarProps) {
  const percentage = (value / max) * 100;
  
  return (
    <div className="progress-bar">
      <div 
        className="progress-fill" 
        style={{ width: `${percentage}%` }}
      >
        {showPercentage && `${Math.round(percentage)}%`}
      </div>
    </div>
  );
}

function Stats() {
  return (
    <div>
      <ProgressBar value={75} max={100} />
      <ProgressBar value={150} max={200} showPercentage={false} />
    </div>
  );
}
```

**Common Errors:**
- Passing numbers as strings
- Not validating numeric ranges
- Division by zero errors
- Floating point precision issues

**Best Practices:**
- Validate number ranges
- Handle edge cases (zero, negative numbers)
- Use appropriate number types (integer vs float)
- Consider precision for calculations
- Provide fallback values for invalid numbers

---

### 6. Boolean Props

**What is it?**
Props that accept true/false values to control component behavior, visibility, or state.

**Why we need it?**
- Toggle features on/off
- Control visibility
- Enable/disable functionality
- Set configuration flags
- Handle conditional behavior

**How it works?**
- Passed as JavaScript boolean expressions
- Can be used directly in conditional logic
- TypeScript ensures boolean type safety
- Often used for feature flags

**Syntax:**
```tsx
// Boolean literal
<Component isLoading={true} disabled={false} />

// Expression
<Component isVisible={user.isLoggedIn} />

// Negation
<Component isHidden={!isActive} />
```

**Simple Example:**
```tsx
function Button({ label, disabled }: { label: string; disabled: boolean }) {
  return (
    <button disabled={disabled}>
      {label}
    </button>
  );
}

function App() {
  return (
    <div>
      <Button label="Submit" disabled={false} />
      <Button label="Cancel" disabled={true} />
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface FeatureCardProps {
  title: string;
  description: string;
  isAvailable: boolean;
  isPremium: boolean;
}

function FeatureCard({ title, description, isAvailable, isPremium }: FeatureCardProps) {
  return (
    <div className={`feature-card ${!isAvailable ? 'disabled' : ''} ${isPremium ? 'premium' : ''}`}>
      <h3>{title} {isPremium && '⭐'}</h3>
      <p>{description}</p>
      {!isAvailable && <span className="badge">Coming Soon</span>}
      {isAvailable && <button>Enable Feature</button>}
    </div>
  );
}

function Features() {
  return (
    <div>
      <FeatureCard 
        title="Dark Mode" 
        description="Switch to dark theme" 
        isAvailable={true} 
        isPremium={false} 
      />
      <FeatureCard 
        title="AI Assistant" 
        description="Get AI-powered suggestions" 
        isAvailable={false} 
        isPremium={true} 
      />
    </div>
  );
}
```

**Common Errors:**
- Passing strings instead of booleans
- Not handling undefined boolean props
- Complex boolean logic in props
- Inconsistent boolean naming conventions

**Best Practices:**
- Use is/has/should prefixes for boolean props
- Provide default values
- Keep boolean logic simple
- Use descriptive names
- Consider using enums for complex boolean states

---

### 7. Object Props

**What is it?**
Props that accept object values to pass complex data structures or configuration objects.

**Why we need it?**
- Pass related data together
- Reduce prop drilling
- Organize complex configurations
- Pass multiple related values
- Maintain data structure integrity

**How it works?**
- Objects passed as JavaScript expressions
- Can be destructured in child component
- TypeScript interfaces define object structure
- Changes trigger re-renders

**Syntax:**
```tsx
// Object literal
<Component config={{ theme: 'dark', lang: 'en' }} />

// Variable
<Component user={userObject} />

// Spread from object
<Component {...dataObject} />
```

**Simple Example:**
```tsx
function UserCard({ user }: { user: { name: string; email: string; age: number } }) {
  return (
    <div>
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      <p>Age: {user.age}</p>
    </div>
  );
}

function App() {
  const userData = {
    name: "John Doe",
    email: "john@example.com",
    age: 30
  };
  
  return <UserCard user={userData} />;
}
```

**Real-world Example:**
```tsx
interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
  specifications: {
    weight: number;
    dimensions: { width: number; height: number; depth: number };
    material: string;
  };
}

function ProductDetails({ product }: { product: Product }) {
  return (
    <div className="product-details">
      <h2>{product.name}</h2>
      <p className="price">${product.price}</p>
      <p className="category">{product.category}</p>
      <div className="specifications">
        <p>Weight: {product.specifications.weight}kg</p>
        <p>Dimensions: {product.specifications.dimensions.width}x{product.specifications.dimensions.height}x{product.specifications.dimensions.depth}cm</p>
        <p>Material: {product.specifications.material}</p>
      </div>
    </div>
  );
}
```

**Common Errors:**
- Not defining TypeScript interfaces
- Passing undefined or null objects
- Mutating object props
- Overly complex object structures

**Best Practices:**
- Define TypeScript interfaces for object props
- Use readonly when appropriate
- Keep objects focused and related
- Consider splitting large objects
- Validate object structure

---

### 8. Array Props

**What is it?**
Props that accept array values to pass lists of data, multiple items, or collections.

**Why we need it?**
- Pass lists of items
- Handle multiple values
- Enable batch operations
- Support dynamic content
- Simplify data passing

**How it works?**
- Arrays passed as JavaScript expressions
- Can be mapped/filtered in child component
- TypeScript defines array item types
- Changes trigger re-renders

**Syntax:**
```tsx
// Array literal
<Component items={['item1', 'item2', 'item3']} />

// Variable
<Component items={itemsArray} />

// Array methods
<Component items={data.filter(item => item.active)} />
```

**Simple Example:**
```tsx
function TodoList({ items }: { items: string[] }) {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{item}</li>
      ))}
    </ul>
  );
}

function App() {
  const todos = ['Learn React', 'Build project', 'Deploy app'];
  return <TodoList items={todos} />;
}
```

**Real-world Example:**
```tsx
interface MenuItem {
  id: number;
  label: string;
  icon: string;
  path: string;
}

interface NavigationProps {
  items: MenuItem[];
  orientation?: 'horizontal' | 'vertical';
}

function Navigation({ items, orientation = 'horizontal' }: NavigationProps) {
  return (
    <nav className={`nav nav-${orientation}`}>
      {items.map(item => (
        <a key={item.id} href={item.path} className="nav-item">
          <span className="icon">{item.icon}</span>
          <span className="label">{item.label}</span>
        </a>
      ))}
    </nav>
  );
}

function App() {
  const menuItems: MenuItem[] = [
    { id: 1, label: 'Home', icon: '🏠', path: '/' },
    { id: 2, label: 'Products', icon: '📦', path: '/products' },
    { id: 3, label: 'About', icon: 'ℹ️', path: '/about' },
    { id: 4, label: 'Contact', icon: '📧', path: '/contact' }
  ];

  return (
    <div>
      <Navigation items={menuItems} orientation="horizontal" />
      <Navigation items={menuItems} orientation="vertical" />
    </div>
  );
}
```

**Common Errors:**
- Not handling empty arrays
- Forgetting key props when mapping
- Not validating array contents
- Mutating array props

**Best Practices:**
- Handle empty arrays gracefully
- Use TypeScript for array item types
- Provide default empty arrays
- Consider immutability
- Validate array length when needed

---

### 9. Function Props

**What is it?**
Props that accept function values to enable child-to-parent communication and event handling.

**Why we need it?**
- Enable child-to-parent communication
- Handle events in child components
- Pass callbacks for actions
- Implement custom behaviors
- Maintain separation of concerns

**How it works?**
- Functions passed as JavaScript expressions
- Child components call the functions
- Parent components handle the logic
- Enables callback pattern

**Syntax:**
```tsx
// Function reference
<Component onAction={handleAction} />

// Arrow function
<Component onAction={() => handleClick(param)} />

// Inline function
<Component onAction={() => console.log('clicked')} />
```

**Simple Example:**
```tsx
function Button({ onClick, label }: { onClick: () => void; label: string }) {
  return <button onClick={onClick}>{label}</button>;
}

function App() {
  const handleClick = () => {
    console.log('Button clicked!');
  };
  
  return <Button onClick={handleClick} label="Click me" />;
}
```

**Real-world Example:**
```tsx
interface UserFormProps {
  onSubmit: (userData: { name: string; email: string }) => void;
  onCancel: () => void;
}

function UserForm({ onSubmit, onCancel }: UserFormProps) {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    onSubmit({ name, email });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input 
        value={name} 
        onChange={(e) => setName(e.target.value)} 
        placeholder="Name" 
      />
      <input 
        value={email} 
        onChange={(e) => setEmail(e.target.value)} 
        placeholder="Email" 
      />
      <button type="submit">Submit</button>
      <button type="button" onClick={onCancel}>Cancel</button>
    </form>
  );
}

function App() {
  const handleUserSubmit = (userData: { name: string; email: string }) => {
    console.log('User submitted:', userData);
    // API call or state update
  };

  const handleCancel = () => {
    console.log('Form cancelled');
  };

  return (
    <UserForm 
      onSubmit={handleUserSubmit} 
      onCancel={handleCancel} 
    />
  );
}
```

**Common Errors:**
- Calling functions instead of passing references
- Not handling function parameters correctly
- Creating functions in render (performance issues)
- Not typing function props properly

**Best Practices:**
- Use useCallback for expensive functions
- Type function parameters with TypeScript
- Pass function references when possible
- Handle errors in callback functions
- Document expected function behavior

---

### 10. children Prop

**What is it?**
A special prop in React that allows components to render content between their opening and closing tags, enabling flexible composition.

**Why we need it?**
- Create flexible, composable components
- Enable wrapper components
- Support slot-like patterns
- Improve component reusability
- Maintain visual containment

**How it works?**
- Content between component tags becomes children prop
- Children can be any valid React node
- Components can render children anywhere
- Enables composition patterns

**Syntax:**
```tsx
// Parent with children
<Parent>
  <Child />
  <AnotherChild />
</Parent>

// Receiving children
function Parent({ children }: { children: React.ReactNode }) {
  return <div>{children}</div>;
}
```

**Simple Example:**
```tsx
function Card({ children }: { children: React.ReactNode }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

function App() {
  return (
    <Card>
      <h2>Card Title</h2>
      <p>Card content goes here</p>
    </Card>
  );
}
```

**Real-world Example:**
```tsx
interface ModalProps {
  isOpen: boolean;
  onClose: () => void;
  title: string;
  children: React.ReactNode;
}

function Modal({ isOpen, onClose, title, children }: ModalProps) {
  if (!isOpen) return null;

  return (
    <div className="modal-overlay" onClick={onClose}>
      <div className="modal-content" onClick={(e) => e.stopPropagation()}>
        <div className="modal-header">
          <h2>{title}</h2>
          <button onClick={onClose}>&times;</button>
        </div>
        <div className="modal-body">
          {children}
        </div>
      </div>
    </div>
  );
}

function App() {
  const [isModalOpen, setIsModalOpen] = useState(false);

  return (
    <div>
      <button onClick={() => setIsModalOpen(true)}>Open Modal</button>
      <Modal 
        isOpen={isModalOpen} 
        onClose={() => setIsModalOpen(false)}
        title="User Settings"
      >
        <form>
          <input placeholder="Username" />
          <input placeholder="Email" />
          <button>Save Changes</button>
        </form>
      </Modal>
    </div>
  );
}
```

**Common Errors:**
- Not defining children prop type
- Assuming children is always an array
- Not handling null/undefined children
- Overusing children when props would be clearer

**Best Practices:**
- Type children as React.ReactNode
- Handle undefined/null children
- Document when children are expected
- Consider using named slots for complex layouts
- Use children for content, props for configuration

---

### 11. Props Destructuring

**What is it?**
The practice of extracting individual properties from the props object in the function parameter for cleaner code.

**Why we need it?**
- Cleaner, more readable code
- Easier to identify used props
- Reduces props. prefix repetition
- Better for TypeScript inference
- Standard modern React pattern

**How it works?**
- Destructure props in function parameters
- Can provide default values
- Works with TypeScript interfaces
- Can rename properties

**Syntax:**
```tsx
// Without destructuring
function Component(props: { name: string; age: number }) {
  return <div>{props.name} - {props.age}</div>;
}

// With destructuring
function Component({ name, age }: { name: string; age: number }) {
  return <div>{name} - {age}</div>;
}
```

**Simple Example:**
```tsx
function Button({ label, onClick, disabled }: { 
  label: string; 
  onClick: () => void; 
  disabled?: boolean 
}) {
  return (
    <button onClick={onClick} disabled={disabled}>
      {label}
    </button>
  );
}
```

**Real-world Example:**
```tsx
interface UserCardProps {
  user: {
    id: number;
    name: string;
    email: string;
    avatar: string;
  };
  onEdit: (id: number) => void;
  onDelete: (id: number) => void;
  showActions?: boolean;
}

function UserCard({ user, onEdit, onDelete, showActions = true }: UserCardProps) {
  return (
    <div className="user-card">
      <img src={user.avatar} alt={user.name} />
      <div className="user-info">
        <h3>{user.name}</h3>
        <p>{user.email}</p>
      </div>
      {showActions && (
        <div className="user-actions">
          <button onClick={() => onEdit(user.id)}>Edit</button>
          <button onClick={() => onDelete(user.id)}>Delete</button>
        </div>
      )}
    </div>
  );
}

// Advanced destructuring with renaming
function UserProfile({ user: { id, name, email, ...rest } }: { user: any }) {
  console.log('Additional data:', rest);
  return (
    <div>
      <h2>{name}</h2>
      <p>{email}</p>
    </div>
  );
}
```

**Common Errors:**
- Inconsistent destructuring patterns
- Not providing default values for optional props
- Destructuring too deeply
- Forgetting to update destructuring when props change

**Best Practices:**
- Always destructure props in functional components
- Provide default values in destructuring
- Keep destructuring flat and readable
- Use TypeScript interfaces alongside destructuring
- Be consistent across components

---

### 12. TypeScript Interfaces for Props

**What is it?**
TypeScript interfaces that define the structure and types of component props for type safety and better developer experience.

**Why we need it?**
- Type safety prevents runtime errors
- Better IDE autocomplete and hints
- Self-documenting component APIs
- Catches type errors during development
- Enables refactoring with confidence

**How it works?**
- Define interface with prop names and types
- Use interface as type for props parameter
- TypeScript validates prop usage
- Provides compile-time checking

**Syntax:**
```tsx
interface ComponentProps {
  propName: type;
  optionalProp?: type;
}

function Component({ propName, optionalProp }: ComponentProps) {
  // component logic
}
```

**Simple Example:**
```tsx
interface ButtonProps {
  label: string;
  onClick: () => void;
  disabled?: boolean;
  variant?: 'primary' | 'secondary';
}

function Button({ label, onClick, disabled = false, variant = 'primary' }: ButtonProps) {
  return (
    <button 
      onClick={onClick} 
      disabled={disabled}
      className={`btn btn-${variant}`}
    >
      {label}
    </button>
  );
}
```

**Real-world Example:**
```tsx
interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
  inStock: boolean;
  rating: number;
}

interface ProductCardProps {
  product: Product;
  onAddToCart: (productId: number) => void;
  onWishlist: (productId: number) => void;
  showRating?: boolean;
  className?: string;
}

function ProductCard({ 
  product, 
  onAddToCart, 
  onWishlist, 
  showRating = true,
  className = '' 
}: ProductCardProps) {
  return (
    <div className={`product-card ${className}`}>
      <h3>{product.name}</h3>
      <p className="price">${product.price}</p>
      <p className="category">{product.category}</p>
      {showRating && <div className="rating">★ {product.rating}</div>}
      <div className="actions">
        <button 
          onClick={() => onAddToCart(product.id)}
          disabled={!product.inStock}
        >
          {product.inStock ? 'Add to Cart' : 'Out of Stock'}
        </button>
        <button onClick={() => onWishlist(product.id)}>
          ♥ Wishlist
        </button>
      </div>
    </div>
  );
}

// Usage with type safety
function App() {
  const product: Product = {
    id: 1,
    name: 'Laptop',
    price: 999,
    category: 'Electronics',
    inStock: true,
    rating: 4.5
  };

  const handleAddToCart = (id: number) => {
    console.log(`Adding product ${id} to cart`);
  };

  const handleWishlist = (id: number) => {
    console.log(`Adding product ${id} to wishlist`);
  };

  return (
    <ProductCard 
      product={product}
      onAddToCart={handleAddToCart}
      onWishlist={handleWishlist}
      showRating={true}
      className="featured"
    />
  );
}
```

**Common Errors:**
- Not defining interfaces for props
- Using `any` type instead of specific types
- Forgetting optional prop markers (?)
- Not extending common interfaces
- Inconsistent naming conventions

**Best Practices:**
- Always define interfaces for component props
- Use descriptive interface names (ComponentProps)
- Mark optional props with ?
- Use union types for limited options
- Consider extending common interfaces
- Export interfaces for reuse

---

### 13. Default Props

**What is it?**
Default values for props that are used when the parent component doesn't provide a value for optional props.

**Why we need it?**
- Provide sensible defaults
- Make components more flexible
- Prevent undefined errors
- Improve component usability
- Reduce required prop count

**How it works?**
- Set default values in destructuring
- Used when prop is undefined
- Can be calculated values
- TypeScript compatible with defaults

**Syntax:**
```tsx
function Component({ prop = defaultValue }: { prop?: type }) {
  // use prop with default value
}
```

**Simple Example:**
```tsx
function Button({ label = 'Click', disabled = false }: { 
  label?: string; 
  disabled?: boolean 
}) {
  return (
    <button disabled={disabled}>
      {label}
    </button>
  );
}

// Usage
<Button /> {/* Renders "Click" button */}
<Button label="Submit" /> {/* Renders "Submit" button */}
```

**Real-world Example:**
```tsx
interface AlertProps {
  message: string;
  type?: 'success' | 'error' | 'warning' | 'info';
  dismissible?: boolean;
  autoClose?: boolean;
  duration?: number;
}

function Alert({ 
  message, 
  type = 'info', 
  dismissible = true,
  autoClose = false,
  duration = 5000
}: AlertProps) {
  const [isVisible, setIsVisible] = useState(true);

  useEffect(() => {
    if (autoClose) {
      const timer = setTimeout(() => setIsVisible(false), duration);
      return () => clearTimeout(timer);
    }
  }, [autoClose, duration]);

  if (!isVisible) return null;

  return (
    <div className={`alert alert-${type}`}>
      <span className="message">{message}</span>
      {dismissible && (
        <button onClick={() => setIsVisible(false)}>&times;</button>
      )}
    </div>
  );
}

// Usage examples
function App() {
  return (
    <div>
      <Alert message="Operation successful" type="success" />
      <Alert message="An error occurred" type="error" autoClose={true} />
      <Alert message="Warning message" type="warning" dismissible={false} />
      <Alert message="Info message" /> {/* Uses defaults: type='info', dismissible=true */}
    </div>
  );
}
```

**Common Errors:**
- Not providing defaults for optional props
- Using wrong default value types
- Defaults that don't make sense
- Not documenting default behavior

**Best Practices:**
- Provide sensible defaults for optional props
- Keep defaults simple and predictable
- Document default values in comments
- Consider using null for intentional absence
- Test components with default values

---

## Part 2: Events

### 14. Events in React

**What is it?**
React events are synthetic events that wrap browser events to provide consistent behavior across different browsers and enable React's event handling system.

**Why we need it?**
- Cross-browser compatibility
- Consistent event behavior
- Performance optimizations
- Integration with React's rendering system
- Better developer experience

**How it works?**
- React uses synthetic events instead of native browser events
- Events are pooled for performance
- Event handlers are attached to the root, not individual elements
- React's event system normalizes browser differences
- Automatic event delegation

**Syntax:**
```tsx
function Component() {
  const handleClick = (event: React.MouseEvent) => {
    // handle event
  };
  
  return <button onClick={handleClick}>Click me</button>;
}
```

**Simple Example:**
```tsx
function Button() {
  const handleClick = (event: React.MouseEvent<HTMLButtonElement>) => {
    console.log('Button clicked!', event);
  };
  
  return <button onClick={handleClick}>Click me</button>;
}
```

**Real-world Example:**
```tsx
function InteractiveCard() {
  const [isHovered, setIsHovered] = useState(false);
  const [isClicked, setIsClicked] = useState(false);

  const handleMouseEnter = () => setIsHovered(true);
  const handleMouseLeave = () => setIsHovered(false);
  const handleClick = (event: React.MouseEvent) => {
    event.preventDefault(); // Prevent default behavior
    setIsClicked(!isClicked);
  };

  return (
    <div 
      className={`card ${isHovered ? 'hovered' : ''} ${isClicked ? 'clicked' : ''}`}
      onMouseEnter={handleMouseEnter}
      onMouseLeave={handleMouseLeave}
      onClick={handleClick}
    >
      <h3>Interactive Card</h3>
      <p>Hover and click me!</p>
      {isClicked && <p className="feedback">Thanks for clicking!</p>}
    </div>
  );
}
```

**Common Errors:**
- Calling event handlers instead of passing references
- Not preventing default behavior when needed
- Using native event methods instead of React synthetic events
- Forgetting to type event parameters

**Best Practices:**
- Use React synthetic event types
- Type event handlers properly
- Prevent default behavior when necessary
- Use event delegation for many similar elements
- Clean up event listeners in useEffect

---

### 15. onClick

**What is it?**
The onClick event handler in React responds to mouse clicks on elements, similar to the native click event but with React's synthetic event system.

**Why we need it?**
- Handle user interactions
- Trigger actions and state changes
- Navigate between pages
- Toggle UI elements
- Submit forms and data

**How it works?**
- Attached to JSX elements as onClick prop
- Receives synthetic mouse event
- Can prevent default behavior
- Supports both mouse and touch interactions

**Syntax:**
```tsx
function Component() {
  const handleClick = (event: React.MouseEvent) => {
    // handle click
  };
  
  return <button onClick={handleClick}>Click me</button>;
}
```

**Simple Example:**
```tsx
function Counter() {
  const [count, setCount] = useState(0);
  
  const handleClick = () => {
    setCount(count + 1);
  };
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleClick}>Increment</button>
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface ProductCardProps {
  product: {
    id: number;
    name: string;
    price: number;
  };
  onAddToCart: (productId: number) => void;
  onViewDetails: (productId: number) => void;
}

function ProductCard({ product, onAddToCart, onViewDetails }: ProductCardProps) {
  const handleAddToCart = (event: React.MouseEvent) => {
    event.stopPropagation(); // Prevent event bubbling
    onAddToCart(product.id);
  };

  const handleViewDetails = () => {
    onViewDetails(product.id);
  };

  return (
    <div className="product-card" onClick={handleViewDetails}>
      <h3>{product.name}</h3>
      <p className="price">${product.price}</p>
      <div className="actions">
        <button onClick={handleAddToCart}>Add to Cart</button>
        <button onClick={() => onViewDetails(product.id)}>View Details</button>
      </div>
    </div>
  );
}

function App() {
  const handleAddToCart = (id: number) => {
    console.log(`Added product ${id} to cart`);
  };

  const handleViewDetails = (id: number) => {
    console.log(`Viewing details for product ${id}`);
  };

  const product = { id: 1, name: 'Laptop', price: 999 };

  return (
    <ProductCard 
      product={product}
      onAddToCart={handleAddToCart}
      onViewDetails={handleViewDetails}
    />
  );
}
```

**Common Errors:**
- Calling functions instead of passing references
- Not handling event propagation correctly
- Using onClick on non-interactive elements
- Forgetting to prevent default behavior

**Best Practices:**
- Use descriptive handler names
- Prevent default behavior when needed
- Handle event propagation appropriately
- Consider accessibility (keyboard support)
- Separate logic from event handlers

---

### 16. onChange

**What is it?**
The onChange event handler in React responds to changes in form elements like inputs, textareas, and selects, tracking user input.

**Why we need it?**
- Track user input in real-time
- Implement controlled components
- Validate form data
- Filter and search functionality
- Dynamic UI updates based on input

**How it works?**
- Fires when form element value changes
- Receives synthetic change event
- Used with controlled components
- Provides access to new value

**Syntax:**
```tsx
function Component() {
  const [value, setValue] = useState('');
  
  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setValue(event.target.value);
  };
  
  return <input value={value} onChange={handleChange} />;
}
```

**Simple Example:**
```tsx
function SearchBox() {
  const [searchTerm, setSearchTerm] = useState('');
  
  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setSearchTerm(event.target.value);
  };
  
  return (
    <div>
      <input 
        type="text" 
        value={searchTerm} 
        onChange={handleChange}
        placeholder="Search..."
      />
      <p>Searching for: {searchTerm}</p>
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface UserFormProps {
  onSubmit: (userData: { name: string; email: string; age: number }) => void;
}

function UserForm({ onSubmit }: UserFormProps) {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    age: ''
  });

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = event.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
  };

  const handleSubmit = (event: React.FormEvent) => {
    event.preventDefault();
    onSubmit({
      name: formData.name,
      email: formData.email,
      age: parseInt(formData.age) || 0
    });
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="name">Name:</label>
        <input
          id="name"
          name="name"
          type="text"
          value={formData.name}
          onChange={handleChange}
          required
        />
      </div>
      <div>
        <label htmlFor="email">Email:</label>
        <input
          id="email"
          name="email"
          type="email"
          value={formData.email}
          onChange={handleChange}
          required
        />
      </div>
      <div>
        <label htmlFor="age">Age:</label>
        <input
          id="age"
          name="age"
          type="number"
          value={formData.age}
          onChange={handleChange}
          min="0"
          max="120"
        />
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Common Errors:**
- Not using controlled components
- Forgetting to bind event target
- Not handling different input types
- Performance issues with frequent updates

**Best Practices:**
- Use controlled components
- Type event handlers properly
- Implement debouncing for frequent updates
- Validate input values
- Handle different input types appropriately

---

### 17. onSubmit

**What is it?**
The onSubmit event handler in React responds to form submission, typically used to process and validate form data before sending it to a server.

**Why we need it?**
- Handle form submissions
- Validate form data
- Prevent default page reload
- Process user input
- Send data to servers

**How it works?**
- Attached to form element
- Fires when form is submitted
- Receives synthetic form event
- Can prevent default behavior

**Syntax:**
```tsx
function Component() {
  const handleSubmit = (event: React.FormEvent) => {
    event.preventDefault();
    // process form data
  };
  
  return <form onSubmit={handleSubmit}>{/* form fields */}</form>;
}
```

**Simple Example:**
```tsx
function LoginForm() {
  const handleSubmit = (event: React.FormEvent) => {
    event.preventDefault();
    console.log('Form submitted');
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input type="email" placeholder="Email" required />
      <input type="password" placeholder="Password" required />
      <button type="submit">Login</button>
    </form>
  );
}
```

**Real-world Example:**
```tsx
interface ContactFormProps {
  onSubmit: (data: { name: string; email: string; message: string }) => void;
}

function ContactForm({ onSubmit }: ContactFormProps) {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [errors, setErrors] = useState<{ [key: string]: string }>({});

  const handleChange = (event: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
    const { name, value } = event.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    // Clear error when user starts typing
    if (errors[name]) {
      setErrors(prev => ({ ...prev, [name]: '' }));
    }
  };

  const validateForm = () => {
    const newErrors: { [key: string]: string } = {};
    
    if (!formData.name.trim()) {
      newErrors.name = 'Name is required';
    }
    if (!formData.email.trim()) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = 'Email is invalid';
    }
    if (!formData.message.trim()) {
      newErrors.message = 'Message is required';
    }
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = async (event: React.FormEvent) => {
    event.preventDefault();
    
    if (!validateForm()) return;
    
    setIsSubmitting(true);
    
    try {
      await onSubmit(formData);
      // Reset form on success
      setFormData({ name: '', email: '', message: '' });
    } catch (error) {
      console.error('Submission error:', error);
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <form onSubmit={handleSubmit} className="contact-form">
      <div className="form-group">
        <label htmlFor="name">Name:</label>
        <input
          id="name"
          name="name"
          type="text"
          value={formData.name}
          onChange={handleChange}
          className={errors.name ? 'error' : ''}
        />
        {errors.name && <span className="error-message">{errors.name}</span>}
      </div>
      
      <div className="form-group">
        <label htmlFor="email">Email:</label>
        <input
          id="email"
          name="email"
          type="email"
          value={formData.email}
          onChange={handleChange}
          className={errors.email ? 'error' : ''}
        />
        {errors.email && <span className="error-message">{errors.email}</span>}
      </div>
      
      <div className="form-group">
        <label htmlFor="message">Message:</label>
        <textarea
          id="message"
          name="message"
          value={formData.message}
          onChange={handleChange}
          rows={5}
          className={errors.message ? 'error' : ''}
        />
        {errors.message && <span className="error-message">{errors.message}</span>}
      </div>
      
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Sending...' : 'Send Message'}
      </button>
    </form>
  );
}
```

**Common Errors:**
- Forgetting to prevent default behavior
- Not validating form data
- Not handling submission errors
- Not providing feedback during submission

**Best Practices:**
- Always prevent default behavior
- Validate form data before submission
- Provide loading states during submission
- Handle errors gracefully
- Reset form after successful submission

---

### 18. Event Object

**What is it?**
The event object passed to event handlers in React contains information about the event and methods to control its behavior.

**Why we need it?**
- Access event details
- Control event behavior
- Get element information
- Handle keyboard events
- Prevent default actions

**How it works?**
- Synthetic event wraps native browser event
- Provides consistent interface across browsers
- Contains event-specific properties
- Offers methods like preventDefault()

**Syntax:**
```tsx
function handleClick(event: React.MouseEvent) {
  console.log(event.target);
  event.preventDefault();
}
```

**Simple Example:**
```tsx
function ClickInfo() {
  const handleClick = (event: React.MouseEvent<HTMLButtonElement>) => {
    console.log('Event type:', event.type);
    console.log('Target:', event.target);
    console.log('Current target:', event.currentTarget);
    console.log('Client coordinates:', event.clientX, event.clientY);
  };
  
  return <button onClick={handleClick}>Click for info</button>;
}
```

**Real-world Example:**
```tsx
function InteractiveElement() {
  const [position, setPosition] = useState({ x: 0, y: 0 });
  const [keyInfo, setKeyInfo] = useState('');

  const handleMouseMove = (event: React.MouseEvent) => {
    setPosition({
      x: event.clientX,
      y: event.clientY
    });
  };

  const handleKeyDown = (event: React.KeyboardEvent) => {
    setKeyInfo(`Key: ${event.key}, Code: ${event.code}`);
    
    // Handle specific keys
    if (event.key === 'Enter') {
      console.log('Enter pressed');
    }
    
    // Prevent default for certain keys
    if (event.key === ' ' && event.target instanceof HTMLBodyElement) {
      event.preventDefault();
    }
  };

  const handleFocus = (event: React.FocusEvent) => {
    console.log('Element focused:', event.target);
  };

  const handleContextMenu = (event: React.MouseEvent) => {
    event.preventDefault(); // Prevent context menu
    console.log('Custom context menu at:', event.clientX, event.clientY);
  };

  return (
    <div 
      onMouseMove={handleMouseMove}
      onKeyDown={handleKeyDown}
      tabIndex={0}
      className="interactive-area"
    >
      <h3>Interactive Element</h3>
      <p>Mouse position: {position.x}, {position.y}</p>
      <p>Key info: {keyInfo}</p>
      
      <input 
        type="text" 
        placeholder="Focus me" 
        onFocus={handleFocus}
      />
      
      <div onContextMenu={handleContextMenu} className="context-area">
        Right-click here for custom menu
      </div>
    </div>
  );
}
```

**Common Event Object Properties:**
- `event.target` - The element that triggered the event
- `event.currentTarget` - The element the event listener is attached to
- `event.type` - The type of event (click, change, etc.)
- `event.preventDefault()` - Prevent default browser behavior
- `event.stopPropagation()` - Stop event bubbling
- `event.clientX/Y` - Mouse coordinates
- `event.key` - The key pressed (for keyboard events)

**Common Errors:**
- Not typing event parameters
- Confusing target and currentTarget
- Not preventing default when needed
- Accessing event after handler completes (event pooling)

**Best Practices:**
- Always type event parameters
- Use event.currentTarget for the element with the handler
- Prevent default behavior when needed
- Extract needed data synchronously
- Understand event bubbling and capturing

---

### 19. Passing arguments to event handlers

**What is it?**
Techniques for passing additional data or parameters to event handlers beyond the event object itself.

**Why we need it?**
- Pass component-specific data
- Handle dynamic values
- Support multiple similar elements
- Enable flexible event handling
- Maintain component state

**How it works?**
- Use arrow functions in JSX
- Use bind method
- Use data attributes
- Create curried functions

**Syntax:**
```tsx
// Arrow function
<button onClick={() => handleClick(id)} />

// Bind method
<button onClick={handleClick.bind(null, id)} />

// Data attributes
<button data-id={id} onClick={handleClick} />
```

**Simple Example:**
```tsx
function ButtonList() {
  const handleClick = (buttonId: number) => {
    console.log(`Button ${buttonId} clicked`);
  };
  
  return (
    <div>
      <button onClick={() => handleClick(1)}>Button 1</button>
      <button onClick={() => handleClick(2)}>Button 2</button>
      <button onClick={() => handleClick(3)}>Button 3</button>
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface Task {
  id: number;
  title: string;
  completed: boolean;
}

function TaskList() {
  const [tasks, setTasks] = useState<Task[]>([
    { id: 1, title: 'Learn React', completed: false },
    { id: 2, title: 'Build project', completed: false },
    { id: 3, title: 'Deploy app', completed: true }
  ]);

  const toggleTask = (taskId: number) => {
    setTasks(prevTasks =>
      prevTasks.map(task =>
        task.id === taskId
          ? { ...task, completed: !task.completed }
          : task
      )
    );
  };

  const deleteTask = (taskId: number, event: React.MouseEvent) => {
    event.stopPropagation(); // Prevent toggling when deleting
    setTasks(prevTasks => prevTasks.filter(task => task.id !== taskId));
  };

  const handleTaskClick = (taskId: number) => {
    console.log(`Task ${taskId} clicked`);
    toggleTask(taskId);
  };

  return (
    <div className="task-list">
      <h2>Tasks</h2>
      <ul>
        {tasks.map(task => (
          <li 
            key={task.id}
            className={`task ${task.completed ? 'completed' : ''}`}
            onClick={() => handleTaskClick(task.id)}
          >
            <span className="task-title">{task.title}</span>
            <button 
              onClick={(e) => deleteTask(task.id, e)}
              className="delete-btn"
            >
              Delete
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}

// Alternative approach using data attributes
function TaskListWithDataAttributes() {
  const handleTaskClick = (event: React.MouseEvent) => {
    const taskId = parseInt(event.currentTarget.dataset.id || '0');
    console.log(`Task ${taskId} clicked`);
  };

  return (
    <ul>
      <li data-id="1" onClick={handleTaskClick}>Task 1</li>
      <li data-id="2" onClick={handleTaskClick}>Task 2</li>
    </ul>
  );
}
```

**Performance Considerations:**
```tsx
// ❌ Bad - creates new function on every render
<button onClick={() => handleClick(id)}>Click</button>

// ✅ Good - use useCallback for expensive handlers
const handleClick = useCallback((id: number) => {
  console.log(`Clicked ${id}`);
}, []);

<button onClick={() => handleClick(id)}>Click</button>

// ✅ Alternative - create handler for each item
const createHandleClick = (id: number) => () => handleClick(id);
```

**Common Errors:**
- Creating functions in render (performance)
- Not using useCallback for expensive handlers
- Confusing parameter order
- Not typing custom parameters

**Best Practices:**
- Use arrow functions for simple cases
- Use useCallback for expensive handlers
- Consider data attributes for many similar elements
- Type custom parameters properly
- Keep event handlers simple

---

## Part 3: Conditional Rendering

### 20. Conditional Rendering

**What is it?**
Conditional rendering in React allows you to render different UI elements or components based on certain conditions, similar to conditional statements in JavaScript.

**Why we need it?**
- Show/hide elements based on state
- Display different content for different users
- Handle loading and error states
- Create responsive user interfaces
- Implement feature flags

**How it works?**
- Use JavaScript conditional operators within JSX
- Condition can be any JavaScript expression
- React evaluates the condition and renders accordingly
- Multiple approaches: if, ternary, logical AND

**Syntax:**
```tsx
// Various approaches
{condition && <Component />}
{condition ? <TrueComponent /> : <FalseComponent />}
{condition ? <Component /> : null}
```

**Simple Example:**
```tsx
function Greeting({ isLoggedIn }: { isLoggedIn: boolean }) {
  if (isLoggedIn) {
    return <h1>Welcome back!</h1>;
  }
  return <h1>Please sign in</h1>;
}
```

**Real-world Example:**
```tsx
interface User {
  name: string;
  role: 'admin' | 'user' | 'guest';
  avatar: string;
}

interface UserProfileProps {
  user: User | null;
  isLoading: boolean;
}

function UserProfile({ user, isLoading }: UserProfileProps) {
  // Loading state
  if (isLoading) {
    return <div className="loading">Loading user profile...</div>;
  }

  // No user state
  if (!user) {
    return (
      <div className="guest-view">
        <h1>Welcome, Guest</h1>
        <button>Sign In</button>
        <button>Register</button>
      </div>
    );
  }

  // Logged in user with role-based content
  return (
    <div className="user-profile">
      <div className="user-header">
        <img src={user.avatar} alt={user.name} />
        <h2>{user.name}</h2>
        <span className={`role role-${user.role}`}>
          {user.role.toUpperCase()}
        </span>
      </div>
      
      <div className="user-content">
        <p>Welcome to your dashboard!</p>
        
        {user.role === 'admin' && (
          <div className="admin-panel">
            <h3>Admin Panel</h3>
            <button>Manage Users</button>
            <button>System Settings</button>
            <button>View Analytics</button>
          </div>
        )}
        
        {user.role === 'user' && (
          <div className="user-panel">
            <h3>My Account</h3>
            <button>Edit Profile</button>
            <button>My Orders</button>
            <button>Wishlist</button>
          </div>
        )}
      </div>
    </div>
  );
}
```

**Common Errors:**
- Putting conditional logic outside JSX
- Not handling all possible states
- Over-complicating conditional logic
- Using assignments instead of comparisons

**Best Practices:**
- Extract complex conditions to functions
- Handle all possible states
- Keep conditional logic simple
- Use the right operator for the situation
- Consider readability

---

### 21. if

**What is it?**
Using traditional JavaScript if statements for conditional rendering, typically used for early returns or complex conditional logic.

**Why we need it?**
- Early returns for different states
- Complex conditional logic
- Multiple condition branches
- Better readability for complex conditions
- Separate logic from JSX

**How it works?**
- Use if statements before return
- Can return different JSX based on conditions
- Works outside JSX
- Standard JavaScript if/else logic

**Syntax:**
```tsx
function Component({ condition }) {
  if (condition) {
    return <ComponentA />;
  }
  return <ComponentB />;
}
```

**Simple Example:**
```tsx
function StatusMessage({ status }: { status: 'loading' | 'success' | 'error' }) {
  if (status === 'loading') {
    return <div>Loading...</div>;
  }
  if (status === 'success') {
    return <div>Success!</div>;
  }
  return <div>Error occurred</div>;
}
```

**Real-world Example:**
```tsx
interface ProductPageProps {
  productId: number;
}

function ProductPage({ productId }: ProductPageProps) {
  const [product, setProduct] = useState<any>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    fetchProduct(productId)
      .then(data => {
        setProduct(data);
        setLoading(false);
      })
      .catch(err => {
        setError(err.message);
        setLoading(false);
      });
  }, [productId]);

  // Early return for loading state
  if (loading) {
    return (
      <div className="loading-container">
        <div className="spinner"></div>
        <p>Loading product details...</p>
      </div>
    );
  }

  // Early return for error state
  if (error) {
    return (
      <div className="error-container">
        <h2>Error Loading Product</h2>
        <p>{error}</p>
        <button onClick={() => window.location.reload()}>Try Again</button>
      </div>
    );
  }

  // Early return if product not found
  if (!product) {
    return (
      <div className="not-found">
        <h2>Product Not Found</h2>
        <p>The product you're looking for doesn't exist.</p>
        <Link to="/products">Back to Products</Link>
      </div>
    );
  }

  // Main product view
  return (
    <div className="product-page">
      <ProductDetails product={product} />
      <ProductReviews productId={productId} />
      <RelatedProducts category={product.category} />
    </div>
  );
}
```

**Common Errors:**
- Not handling all possible states
- Deeply nested if statements
- Returning undefined instead of JSX
- Not using early returns effectively

**Best Practices:**
- Use early returns for different states
- Keep if statements simple
- Handle loading/error states first
- Extract complex conditions to functions
- Consider switch statements for many conditions

---

### 22. Ternary Operator

**What is it?**
The ternary operator (condition ? true : false) is a concise way to implement conditional rendering with two possible outcomes.

**Why we need it?**
- Concise conditional rendering
- Perfect for either/or scenarios
- Readable inline conditions
- Reduces code verbosity
- Common React pattern

**How it works?**
- Evaluates condition
- Returns first expression if true
- Returns second expression if false
- Can be nested (though not recommended)

**Syntax:**
```tsx
{condition ? <TrueComponent /> : <FalseComponent />}
```

**Simple Example:**
```tsx
function UserStatus({ isLoggedIn }: { isLoggedIn: boolean }) {
  return (
    <div>
      {isLoggedIn ? <LogoutButton /> : <LoginButton />}
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface ProductCardProps {
  product: {
    name: string;
    price: number;
    originalPrice?: number;
    inStock: boolean;
    discount?: number;
  };
}

function ProductCard({ product }: ProductCardProps) {
  return (
    <div className="product-card">
      <h3>{product.name}</h3>
      
      {/* Price with discount handling */}
      <div className="price-section">
        {product.discount ? (
          <div className="discounted-price">
            <span className="original-price">${product.originalPrice || product.price}</span>
            <span className="current-price">${product.price * (1 - product.discount)}</span>
            <span className="discount-badge">-{product.discount * 100}%</span>
          </div>
        ) : (
          <span className="regular-price">${product.price}</span>
        )}
      </div>
      
      {/* Stock status */}
      <div className="stock-status">
        {product.inStock ? (
          <span className="in-stock">✓ In Stock</span>
        ) : (
          <span className="out-stock">✗ Out of Stock</span>
        )}
      </div>
      
      {/* Add to cart button */}
      <button 
        className="add-to-cart"
        disabled={!product.inStock}
      >
        {product.inStock ? 'Add to Cart' : 'Out of Stock'}
      </button>
    </div>
  );
}

// Example with nested ternary (use sparingly)
function UserRoleBadge({ role }: { role: 'admin' | 'moderator' | 'user' | 'guest' }) {
  return (
    <span className={`role-badge role-${role}`}>
      {role === 'admin' ? 'Admin' : 
       role === 'moderator' ? 'Moderator' : 
       role === 'user' ? 'User' : 'Guest'}
    </span>
  );
}
```

**Common Errors:**
- Over-nesting ternary operators
- Using ternary for complex conditions
- Not handling null/undefined cases
- Reducing readability with complex ternaries

**Best Practices:**
- Use for simple either/or conditions
- Avoid nesting beyond 2 levels
- Extract complex ternaries to functions
- Consider if statements for complex logic
- Keep ternary conditions readable

---

### 23. && (Logical AND)

**What is it?**
The logical AND operator (&&) is used for conditional rendering when you want to render something only if a condition is true, with no alternative.

**Why we need it?**
- Render elements conditionally
- Show optional content
- Handle feature flags
- Display conditional UI elements
- Cleaner than ternary for single-sided conditions

**How it works?**
- Evaluates left side first
- If true, renders right side
- If false, renders nothing
- Short-circuits evaluation

**Syntax:**
```tsx
{condition && <Component />}
```

**Simple Example:**
```tsx
function Notification({ showNotification, message }: { showNotification: boolean; message: string }) {
  return (
    <div>
      {showNotification && <div className="notification">{message}</div>}
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface DashboardProps {
  user: {
    name: string;
    role: 'admin' | 'user';
    notifications: number;
    hasNewMessages: boolean;
    premiumFeatures: {
      advancedAnalytics: boolean;
      customReports: boolean;
      apiAccess: boolean;
    };
  };
}

function Dashboard({ user }: DashboardProps) {
  return (
    <div className="dashboard">
      <header className="dashboard-header">
        <h1>Welcome, {user.name}</h1>
        
        {/* Notification badge */}
        {user.notifications > 0 && (
          <span className="notification-badge">
            {user.notifications}
          </span>
        )}
        
        {/* New messages indicator */}
        {user.hasNewMessages && (
          <span className="new-messages">New messages!</span>
        )}
      </header>
      
      <main className="dashboard-content">
        {/* Admin-only section */}
        {user.role === 'admin' && (
          <section className="admin-panel">
            <h2>Admin Panel</h2>
            <button>Manage Users</button>
            <button>System Settings</button>
          </section>
        )}
        
        {/* Premium features */}
        <section className="features">
          <h2>Features</h2>
          
          {user.premiumFeatures.advancedAnalytics && (
            <div className="feature-card">
              <h3>Advanced Analytics</h3>
              <p>View detailed analytics and insights</p>
            </div>
          )}
          
          {user.premiumFeatures.customReports && (
            <div className="feature-card">
              <h3>Custom Reports</h3>
              <p>Create and export custom reports</p>
            </div>
          )}
          
          {user.premiumFeatures.apiAccess && (
            <div className="feature-card">
              <h3>API Access</h3>
              <p>Access our API for integrations</p>
            </div>
          )}
        </section>
      </main>
    </div>
  );
}
```

**Important Note about Falsy Values:**
```tsx
// ⚠️ Be careful with falsy values
{count && <div>{count} items</div>} // Won't render if count is 0

// ✅ Better approach for numbers
{count > 0 && <div>{count} items</div>}

// ✅ Or use explicit comparison
{count !== undefined && count !== null && <div>{count} items</div>}
```

**Common Errors:**
- Not handling falsy values correctly (especially 0)
- Using && when ternary is more appropriate
- Not considering rendering nothing vs rendering null
- Complex conditions with &&

**Best Practices:**
- Use for simple conditional rendering
- Be careful with falsy values (especially 0)
- Use explicit comparisons for numbers
- Consider ternary for either/or scenarios
- Keep conditions simple and readable

---

### 24. Loading UI

**What is it?**
Loading UI refers to the visual feedback shown to users while data is being fetched or operations are in progress.

**Why we need it?**
- Provide user feedback
- Improve perceived performance
- Prevent user confusion
- Set proper expectations
- Enhance user experience

**How it works?**
- Track loading state with boolean
- Show loading indicator when true
- Show content when false
- Can be inline or full-screen

**Syntax:**
```tsx
{isLoading && <LoadingSpinner />}
{isLoading ? <LoadingSpinner /> : <Content />}
```

**Simple Example:**
```tsx
function DataLoader() {
  const [data, setData] = useState(null);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    fetchData().then(result => {
      setData(result);
      setIsLoading(false);
    });
  }, []);

  if (isLoading) return <div>Loading...</div>;
  return <div>{data}</div>;
}
```

**Real-world Example:**
```tsx
interface LoadingSpinnerProps {
  size?: 'small' | 'medium' | 'large';
  message?: string;
  fullScreen?: boolean;
}

function LoadingSpinner({ size = 'medium', message, fullScreen = false }: LoadingSpinnerProps) {
  const sizeClasses = {
    small: 'spinner-small',
    medium: 'spinner-medium',
    large: 'spinner-large'
  };

  const spinner = (
    <div className={`loading-spinner ${sizeClasses[size]}`}>
      <div className="spinner-ring"></div>
      {message && <p className="loading-message">{message}</p>}
    </div>
  );

  if (fullScreen) {
    return (
      <div className="loading-overlay">
        {spinner}
      </div>
    );
  }

  return spinner;
}

// Usage in a data-fetching component
function ProductList() {
  const [products, setProducts] = useState([]);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const fetchProducts = async () => {
      try {
        setIsLoading(true);
        const response = await fetch('/api/products');
        const data = await response.json();
        setProducts(data);
      } catch (err) {
        setError('Failed to load products');
      } finally {
        setIsLoading(false);
      }
    };

    fetchProducts();
  }, []);

  if (isLoading) {
    return (
      <LoadingSpinner 
        size="large" 
        message="Loading products..." 
        fullScreen={true} 
      />
    );
  }

  if (error) {
    return <div className="error">{error}</div>;
  }

  return (
    <div className="product-list">
      {products.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}

// Skeleton loading alternative
function SkeletonLoader() {
  return (
    <div className="skeleton-container">
      {[1, 2, 3, 4].map(i => (
        <div key={i} className="skeleton-card">
          <div className="skeleton-image"></div>
          <div className="skeleton-title"></div>
          <div className="skeleton-text"></div>
          <div className="skeleton-button"></div>
        </div>
      ))}
    </div>
  );
}
```

**Common Errors:**
- Not setting loading state back to false
- No loading feedback for slow operations
- Loading indicators that never disappear
- Not handling loading errors

**Best Practices:**
- Always provide loading feedback
- Use appropriate loading indicator size
- Consider skeleton screens for better UX
- Handle loading errors gracefully
- Set loading state in finally block

---

### 25. Empty State

**What is it?**
Empty state UI is shown when there's no data to display, providing guidance to users on what to do next.

**Why we need it?**
- Guide users when no data exists
- Prevent confusion
- Provide actionable next steps
- Improve user experience
- Maintain consistent UI

**How it works?**
- Check if data array is empty
- Show empty state component when true
- Show data when false
- Can include illustrations and actions

**Syntax:**
```tsx
{data.length === 0 && <EmptyState />}
{!data || data.length === 0 ? <EmptyState /> : <DataList />}
```

**Simple Example:**
```tsx
function TodoList({ todos }: { todos: string[] }) {
  if (todos.length === 0) {
    return (
      <div className="empty-state">
        <p>No todos yet. Add one above!</p>
      </div>
    );
  }
  return <ul>{todos.map(todo => <li key={todo}>{todo}</li>)}</ul>;
}
```

**Real-world Example:**
```tsx
interface EmptyStateProps {
  title: string;
  description: string;
  icon?: string;
  action?: {
    label: string;
    onClick: () => void;
  };
}

function EmptyState({ title, description, icon, action }: EmptyStateProps) {
  return (
    <div className="empty-state">
      {icon && <div className="empty-state-icon">{icon}</div>}
      <h3 className="empty-state-title">{title}</h3>
      <p className="empty-state-description">{description}</p>
      {action && (
        <button onClick={action.onClick} className="empty-state-action">
          {action.label}
        </button>
      )}
    </div>
  );
}

// Usage in various contexts
function Inbox({ emails }: { emails: any[] }) {
  if (emails.length === 0) {
    return (
      <EmptyState
        title="No messages yet"
        description="Your inbox is empty. When you receive messages, they'll appear here."
        icon="📬"
        action={{
          label: "Compose Message",
          onClick: () => console.log('Open compose')
        }}
      />
    );
  }

  return (
    <div className="inbox">
      {emails.map(email => <EmailItem key={email.id} email={email} />)}
    </div>
  );
}

function ShoppingCart({ items }: { items: any[] }) {
  if (items.length === 0) {
    return (
      <EmptyState
        title="Your cart is empty"
        description="Add some products to get started!"
        icon="🛒"
        action={{
          label: "Start Shopping",
          onClick: () => console.log('Go to products')
        }}
      />
    );
  }

  return (
    <div className="cart">
      {items.map(item => <CartItem key={item.id} item={item} />)}
    </div>
  );
}

function SearchResults({ results, query }: { results: any[]; query: string }) {
  if (query && results.length === 0) {
    return (
      <EmptyState
        title="No results found"
        description={`We couldn't find anything matching "${query}"`}
        icon="🔍"
        action={{
          label: "Clear search",
          onClick: () => console.log('Clear search')
        }}
      />
    );
  }

  if (!query) {
    return (
      <EmptyState
        title="Search our products"
        description="Enter a keyword to find what you're looking for"
        icon="🔎"
      />
    );
  }

  return (
    <div className="search-results">
      {results.map(result => <SearchResultItem key={result.id} result={result} />)}
    </div>
  );
}
```

**Common Errors:**
- Not providing empty states
- Confusing empty states with error states
- Not offering actionable next steps
- Inconsistent empty state design

**Best Practices:**
- Always provide empty states for lists
- Include helpful descriptions
- Offer actionable next steps
- Use consistent design patterns
- Consider illustrations for better UX

---

### 26. Error State

**What is it?**
Error state UI is shown when an operation fails, providing feedback about what went wrong and how to recover.

**Why we need it?**
- Inform users about failures
- Provide recovery options
- Prevent user frustration
- Maintain trust and transparency
- Enable debugging

**How it works?**
- Track error state with error object/string
- Show error message when error exists
- Provide retry/cancel options
- Can be inline or full-page

**Syntax:**
```tsx
{error && <ErrorState message={error} />}
{error ? <ErrorState /> : <Content />}
```

**Simple Example:**
```tsx
function DataComponent() {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);

  const fetchData = async () => {
    try {
      const response = await fetch('/api/data');
      if (!response.ok) throw new Error('Failed to fetch');
      const result = await response.json();
      setData(result);
    } catch (err) {
      setError(err.message);
    }
  };

  if (error) {
    return (
      <div className="error">
        <p>Error: {error}</p>
        <button onClick={fetchData}>Retry</button>
      </div>
    );
  }

  return <div>{data}</div>;
}
```

**Real-world Example:**
```tsx
interface ErrorStateProps {
  title?: string;
  message: string;
  onRetry?: () => void;
  onDismiss?: () => void;
  severity?: 'error' | 'warning' | 'info';
}

function ErrorState({ 
  title = 'Something went wrong',
  message,
  onRetry,
  onDismiss,
  severity = 'error'
}: ErrorStateProps) {
  const severityIcons = {
    error: '❌',
    warning: '⚠️',
    info: 'ℹ️'
  };

  return (
    <div className={`error-state error-${severity}`}>
      <div className="error-icon">{severityIcons[severity]}</div>
      <div className="error-content">
        <h3 className="error-title">{title}</h3>
        <p className="error-message">{message}</p>
      </div>
      <div className="error-actions">
        {onRetry && (
          <button onClick={onRetry} className="retry-button">
            Try Again
          </button>
        )}
        {onDismiss && (
          <button onClick={onDismiss} className="dismiss-button">
            Dismiss
          </button>
        )}
      </div>
    </div>
  );
}

// Usage in a comprehensive data component
function UserProfile({ userId }: { userId: number }) {
  const [user, setUser] = useState<any>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  const fetchUser = async () => {
    try {
      setLoading(true);
      setError(null);
      const response = await fetch(`/api/users/${userId}`);
      
      if (!response.ok) {
        if (response.status === 404) {
          throw new Error('User not found');
        } else if (response.status === 403) {
          throw new Error('Access denied');
        } else {
          throw new Error('Failed to load user profile');
        }
      }
      
      const data = await response.json();
      setUser(data);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'An error occurred');
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchUser();
  }, [userId]);

  if (loading) {
    return <LoadingSpinner message="Loading profile..." />;
  }

  if (error) {
    return (
      <ErrorState
        title="Unable to load profile"
        message={error}
        onRetry={fetchUser}
        severity="error"
      />
    );
  }

  if (!user) {
    return (
      <ErrorState
        title="User not found"
        message="The requested user profile doesn't exist."
        severity="warning"
      />
    );
  }

  return (
    <div className="user-profile">
      <h1>{user.name}</h1>
      <p>{user.email}</p>
      {/* User profile content */}
    </div>
  );
}

// Inline error state for forms
function ContactForm() {
  const [submitError, setSubmitError] = useState<string | null>(null);
  const [isSubmitting, setIsSubmitting] = useState(false);

  const handleSubmit = async (formData: any) => {
    try {
      setIsSubmitting(true);
      setSubmitError(null);
      
      const response = await fetch('/api/contact', {
        method: 'POST',
        body: JSON.stringify(formData)
      });
      
      if (!response.ok) {
        throw new Error('Failed to send message');
      }
      
      // Success handling
    } catch (err) {
      setSubmitError('Failed to send your message. Please try again.');
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* Form fields */}
      
      {submitError && (
        <ErrorState
          message={submitError}
          severity="error"
          onDismiss={() => setSubmitError(null)}
        />
      )}
      
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Sending...' : 'Send Message'}
      </button>
    </form>
  );
}
```

**Common Errors:**
- Not providing error recovery options
- Showing technical error messages to users
- Not clearing error states on retry
- Inconsistent error handling

**Best Practices:**
- Provide user-friendly error messages
- Offer recovery options (retry, dismiss)
- Clear error states on retry
- Handle different error types appropriately
- Log technical errors for debugging

---

## Part 4: Rendering Lists

### 27. Rendering Lists

**What is it?**
Rendering lists in React involves displaying arrays of data as repeated UI elements, typically using the map() method to transform data into JSX elements.

**Why we need it?**
- Display dynamic data from APIs
- Show lists of items (products, users, etc.)
- Create menus and navigation
- Handle data-driven UI
- Enable dynamic content

**How it works?**
- Use JavaScript array methods
- Transform data array to JSX array
- React renders each element
- Must include unique keys for each item
- Can filter, sort, and transform data

**Syntax:**
```tsx
{items.map((item, index) => (
  <Component key={item.id} data={item} />
))}
```

**Simple Example:**
```tsx
function ShoppingList({ items }: { items: string[] }) {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{item}</li>
      ))}
    </ul>
  );
}
```

**Real-world Example:**
```tsx
interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
  inStock: boolean;
}

function ProductCatalog({ products }: { products: Product[] }) {
  const [filter, setFilter] = useState('all');
  const [sortBy, setSortBy] = useState('name');

  // Filter products
  const filteredProducts = products.filter(product => {
    if (filter === 'all') return true;
    if (filter === 'in-stock') return product.inStock;
    if (filter === 'out-of-stock') return !product.inStock;
    return product.category === filter;
  });

  // Sort products
  const sortedProducts = [...filteredProducts].sort((a, b) => {
    if (sortBy === 'name') return a.name.localeCompare(b.name);
    if (sortBy === 'price') return a.price - b.price;
    return 0;
  });

  // Get unique categories
  const categories = ['all', ...new Set(products.map(p => p.category))];

  return (
    <div className="product-catalog">
      <div className="catalog-controls">
        <select value={filter} onChange={(e) => setFilter(e.target.value)}>
          <option value="all">All Products</option>
          <option value="in-stock">In Stock</option>
          <option value="out-of-stock">Out of Stock</option>
          {categories.slice(1).map(category => (
            <option key={category} value={category}>
              {category}
            </option>
          ))}
        </select>
        
        <select value={sortBy} onChange={(e) => setSortBy(e.target.value)}>
          <option value="name">Sort by Name</option>
          <option value="price">Sort by Price</option>
        </select>
      </div>

      <div className="product-grid">
        {sortedProducts.map(product => (
          <ProductCard key={product.id} product={product} />
        ))}
      </div>

      {sortedProducts.length === 0 && (
        <EmptyState 
          title="No products found"
          description="Try adjusting your filters"
        />
      )}
    </div>
  );
}
```

**Common Errors:**
- Not using map() for rendering
- Forgetting to add keys
- Using forEach instead of map
- Not handling empty arrays
- Mutating arrays during render

**Best Practices:**
- Always use map() for rendering lists
- Provide unique keys for each item
- Handle empty arrays gracefully
- Filter and sort data before rendering
- Extract complex logic to functions

---

### 28. map()

**What is it?**
The map() method creates a new array by calling a function on every element in the original array, commonly used in React to transform data arrays into JSX elements.

**Why we need it?**
- Transform data to UI elements
- Maintain immutable data patterns
- Chain array operations
- Create new arrays from existing ones
- Standard JavaScript array method

**How it works?**
- Iterates over each array element
- Applies transformation function
- Returns new array with results
- Original array remains unchanged
- Perfect for JSX transformation

**Syntax:**
```tsx
array.map((item, index) => (
  <Component key={item.id} item={item} />
))
```

**Simple Example:**
```tsx
function NumberList({ numbers }: { numbers: number[] }) {
  return (
    <ul>
      {numbers.map((num, index) => (
        <li key={index}>{num * 2}</li>
      ))}
    </ul>
  );
}
```

**Real-world Example:**
```tsx
interface User {
  id: number;
  name: string;
  role: string;
  status: 'active' | 'inactive';
}

function UserTable({ users }: { users: User[] }) {
  const [searchTerm, setSearchTerm] = useState('');

  // Filter and map in one operation
  const filteredUsers = users
    .filter(user => 
      user.name.toLowerCase().includes(searchTerm.toLowerCase())
    )
    .map(user => ({
      ...user,
      displayName: `${user.name} (${user.role})`
    }));

  return (
    <div className="user-table">
      <input
        type="text"
        placeholder="Search users..."
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
      />

      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Role</th>
            <th>Status</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {filteredUsers.map(user => (
            <tr key={user.id}>
              <td>{user.displayName}</td>
              <td>{user.role}</td>
              <td>
                <span className={`status status-${user.status}`}>
                  {user.status}
                </span>
              </td>
              <td>
                <button>Edit</button>
                <button>Delete</button>
              </td>
            </tr>
          ))}
        </tbody>
      </table>

      {filteredUsers.length === 0 && (
        <div className="no-results">
          No users found matching "{searchTerm}"
        </div>
      )}
    </div>
  );
}

// Complex mapping with nested data
interface Comment {
  id: number;
  author: string;
  text: string;
  replies: Comment[];
}

function CommentThread({ comments }: { comments: Comment[] }) {
  const renderComment = (comment: Comment, depth = 0) => (
    <div key={comment.id} className={`comment comment-depth-${depth}`}>
      <div className="comment-header">
        <span className="author">{comment.author}</span>
      </div>
      <p className="comment-text">{comment.text}</p>
      
      {comment.replies.length > 0 && (
        <div className="comment-replies">
          {comment.replies.map(reply => renderComment(reply, depth + 1))}
        </div>
      )}
    </div>
  );

  return (
    <div className="comment-thread">
      {comments.map(comment => renderComment(comment))}
    </div>
  );
}
```

**Common Errors:**
- Using forEach instead of map
- Not returning anything in map function
- Mutating items during map
- Forgetting to handle edge cases
- Over-complicating map functions

**Best Practices:**
- Always return JSX in map
- Keep map functions simple
- Extract complex logic to separate functions
- Handle empty arrays
- Consider performance for large arrays

---

### 29. Keys

**What is it?**
Keys are special string attributes that help React identify which items have changed, been added, or been removed in a list, enabling efficient updates.

**Why we need it?**
- Help React identify item changes
- Enable efficient re-rendering
- Maintain component state correctly
- Improve performance
- Prevent rendering bugs

**How it works?**
- React uses keys to match elements
- Keys should be unique among siblings
- Stable keys prevent unnecessary re-renders
- React can reuse DOM elements with matching keys
- Keys should come from data, not generated

**Syntax:**
```tsx
{items.map(item => (
  <Component key={item.id} item={item} />
))}
```

**Simple Example:**
```tsx
function TodoList({ todos }: { todos: { id: number; text: string }[] }) {
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>{todo.text}</li>
      ))}
    </ul>
  );
}
```

**Real-world Example:**
```tsx
interface Task {
  id: string;
  title: string;
  completed: boolean;
  assignedTo: string;
}

function TaskBoard({ tasks }: { tasks: Task[] }) {
  const [filterUser, setFilterUser] = useState('all');

  const filteredTasks = tasks.filter(task => 
    filterUser === 'all' || task.assignedTo === filterUser
  );

  // Get unique assignees
  const assignees = ['all', ...new Set(tasks.map(t => t.assignedTo))];

  return (
    <div className="task-board">
      <div className="board-controls">
        <select 
          value={filterUser} 
          onChange={(e) => setFilterUser(e.target.value)}
        >
          {assignees.map(user => (
            <option key={user} value={user}>
              {user === 'all' ? 'All Users' : user}
            </option>
          ))}
        </select>
      </div>

      <div className="task-list">
        {filteredTasks.map(task => (
          <TaskCard key={task.id} task={task} />
        ))}
      </div>
    </div>
  );
}

// TaskCard maintains its state even when list reorders
function TaskCard({ task }: { task: Task }) {
  const [isExpanded, setIsExpanded] = useState(false);

  return (
    <div className="task-card">
      <div className="task-header">
        <h4>{task.title}</h4>
        <button onClick={() => setIsExpanded(!isExpanded)}>
          {isExpanded ? '▼' : '▶'}
        </button>
      </div>
      
      {isExpanded && (
        <div className="task-details">
          <p>Assigned to: {task.assignedTo}</p>
          <p>Status: {task.completed ? 'Completed' : 'In Progress'}</p>
        </div>
      )}
    </div>
  );
}
```

**Key Requirements:**
- **Unique:** Keys must be unique among siblings
- **Stable:** Keys should not change between renders
- **Predictable:** Same data should always get same key
- **Meaningful:** Keys should represent the item's identity

**Common Errors:**
- Using array index as key
- Generating random keys
- Using non-unique keys
- Changing keys between renders
- Not providing keys at all

**Best Practices:**
- Use unique IDs from data when available
- Avoid using array index as key
- Never use random keys
- Keep keys stable across renders
- Use compound keys if needed (e.g., `${parentId}-${itemId}`)

---

### 30. Why keys are important

**What is it?**
Understanding the critical role keys play in React's rendering efficiency and correctness, particularly for list operations and component state management.

**Why we need it?**
- Prevent rendering bugs
- Maintain component state
- Optimize performance
- Enable proper reconciliation
- Ensure correct UI behavior

**How it works?**
- React uses keys for reconciliation algorithm
- Keys help match old and new elements
- Proper keys enable DOM reuse
- Keys determine component identity
- Affects component state preservation

**Key Benefits:**
1. **Performance:** React can reuse DOM elements
2. **State Preservation:** Component state stays with correct element
3. **Correct Updates:** Only changed elements re-render
4. **Animations:** Smooth transitions when list changes
5. **Focus Management:** Input focus maintained correctly

**Simple Example:**
```tsx
// Without proper keys - problems with state and focus
function BadList({ items }: { items: string[] }) {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>
          <input type="text" defaultValue={item} />
        </li>
      ))}
    </ul>
  );
}

// With proper keys - state and focus preserved
function GoodList({ items }: { items: { id: string; value: string }[] }) {
  return (
    <ul>
      {items.map(item => (
        <li key={item.id}>
          <input type="text" defaultValue={item.value} />
        </li>
      ))}
    </ul>
  );
}
```

**Real-world Example - Demonstrating Key Importance:**
```tsx
interface TodoItem {
  id: string;
  text: string;
  completed: boolean;
}

function TodoList({ todos }: { todos: TodoItem[] }) {
  const [newTodo, setNewTodo] = useState('');

  const addTodo = () => {
    // This demonstrates why keys matter
    // When we add a new todo at the beginning, 
    // proper keys ensure each todo maintains its state
  };

  return (
    <div>
      {todos.map(todo => (
        <TodoItem key={todo.id} todo={todo} />
      ))}
    </div>
  );
}

function TodoItem({ todo }: { todo: TodoItem }) {
  const [isEditing, setIsEditing] = useState(false);
  const [editText, setEditText] = useState(todo.text);

  // With proper keys, this state stays with the correct todo
  // even if the list order changes
  return (
    <div className="todo-item">
      {isEditing ? (
        <input
          value={editText}
          onChange={(e) => setEditText(e.target.value)}
          onBlur={() => setIsEditing(false)}
        />
      ) : (
        <span onClick={() => setIsEditing(true)}>{todo.text}</span>
      )}
    </div>
  );
}
```

**What Happens Without Proper Keys:**
```tsx
// Problem scenario: List reordering
function ReorderingExample() {
  const [items, setItems] = useState([
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' },
    { id: 3, name: 'Item 3' }
  ]);

  const reverseOrder = () => {
    setItems([...items].reverse());
  };

  return (
    <div>
      <button onClick={reverseOrder}>Reverse Order</button>
      
      {/* Without proper keys, React might reuse components incorrectly */}
      {items.map((item, index) => (
        <Item key={index} data={item} /> // ❌ Bad: using index
      ))}
      
      {/* With proper keys, each item maintains its identity */}
      {items.map(item => (
        <Item key={item.id} data={item} /> // ✅ Good: using ID
      ))}
    </div>
  );
}
```

**Common Errors:**
- Not understanding key impact on state
- Ignoring key warnings in console
- Using temporary keys for permanent data
- Not considering list reordering scenarios

**Best Practices:**
- Always provide stable, unique keys
- Understand key impact on component state
- Test list operations (add, remove, reorder)
- Use keys from data when possible
- Monitor React key warnings

---

### 31. Why using array index as key can be problematic

**What is it?**
Understanding the specific problems that arise when using array indices as keys, particularly in dynamic lists where items can be added, removed, or reordered.

**Why we need it?**
- Prevent state bugs
- Avoid performance issues
- Ensure correct rendering
- Maintain component identity
- Handle list operations correctly

**How it works:**
- Array indices change when list changes
- React can't track item identity correctly
- Component state gets confused
- DOM reuse becomes problematic
- Focus and input state issues

**Problems with Index Keys:**

1. **Reordering Issues:**
```tsx
// Problem: Reordering breaks component state
function TodoList() {
  const [todos, setTodos] = useState([
    { id: 1, text: 'First' },
    { id: 2, text: 'Second' },
    { id: 3, text: 'Third' }
  ]);

  // If we reverse the array, indices change but React
  // might think it's the same components
  const reverseTodos = () => setTodos([...todos].reverse());

  return (
    <ul>
      {todos.map((todo, index) => (
        <TodoItem key={index} todo={todo} /> // ❌ Problematic
      ))}
    </ul>
  );
}
```

2. **Insertion/Deletion Issues:**
```tsx
// Problem: Adding/removing items shifts indices
function DynamicList() {
  const [items, setItems] = useState(['A', 'B', 'C']);

  const addItem = () => {
    setItems(['New', ...items]); // Indices shift for all items
  };

  return (
    <div>
      {items.map((item, index) => (
        <InputItem key={index} value={item} /> // ❌ Loses focus/state
      ))}
    </div>
  );
}
```

3. **Component State Confusion:**
```tsx
// Problem: Component state doesn't follow the actual item
function ListItem({ value }: { value: string }) {
  const [isExpanded, setIsExpanded] = useState(false);
  
  // If list reorders, this state might stay with wrong item
  return (
    <div>
      <span onClick={() => setIsExpanded(!isExpanded)}>
        {value}
      </span>
      {isExpanded && <p>Expanded content</p>}
    </div>
  );
}
```

**Real-world Example - The Problem:**
```tsx
interface User {
  id: string;
  name: string;
  email: string;
}

function UserListBad() {
  const [users, setUsers] = useState<User[]>([
    { id: '1', name: 'Alice', email: 'alice@example.com' },
    { id: '2', name: 'Bob', email: 'bob@example.com' },
    { id: '3', name: 'Charlie', email: 'charlie@example.com' }
  ]);

  const addUser = () => {
    const newUser = {
      id: Date.now().toString(),
      name: 'New User',
      email: 'new@example.com'
    };
    setUsers([newUser, ...users]); // All indices shift
  };

  return (
    <ul>
      {users.map((user, index) => (
        <UserRow key={index} user={user} /> // ❌ Bad: using index
      ))}
    </ul>
  );
}

function UserRow({ user }: { user: User }) {
  const [isEditing, setIsEditing] = useState(false);
  const [editedName, setEditedName] = useState(user.name);

  // When a new user is added at the top, all indices shift
  // React might reuse components incorrectly, causing:
  // - Wrong user in edit mode
  // - Lost input focus
  // - Confused component state
  
  return (
    <li>
      {isEditing ? (
        <input
          value={editedName}
          onChange={(e) => setEditedName(e.target.value)}
        />
      ) : (
        <span onClick={() => setIsEditing(true)}>{user.name}</span>
      )}
    </li>
  );
}
```

**Real-world Example - The Solution:**
```tsx
function UserListGood() {
  const [users, setUsers] = useState<User[]>([
    { id: '1', name: 'Alice', email: 'alice@example.com' },
    { id: '2', name: 'Bob', email: 'bob@example.com' },
    { id: '3', name: 'Charlie', email: 'charlie@example.com' }
  ]);

  const addUser = () => {
    const newUser = {
      id: Date.now().toString(),
      name: 'New User',
      email: 'new@example.com'
    };
    setUsers([newUser, ...users]);
  };

  return (
    <ul>
      {users.map(user => (
        <UserRow key={user.id} user={user} /> // ✅ Good: using stable ID
      ))}
    </ul>
  );
}
```

**When Index Keys Are Acceptable:**
```tsx
// Only when:
// 1. List is static (never reordered)
// 2. Items are never added/removed
// 3. Items have no IDs
// 4. No component state is involved

function StaticList() {
  const staticItems = ['Item 1', 'Item 2', 'Item 3'];
  
  return (
    <ul>
      {staticItems.map((item, index) => (
        <li key={index}>{item}</li> // ✅ Acceptable for static list
      ))}
    </ul>
  );
}
```

**Common Errors:**
- Using index keys for dynamic lists
- Not understanding the impact on component state
- Ignoring React warnings about keys
- Creating complex workarounds instead of proper keys

**Best Practices:**
- Never use index keys for dynamic lists
- Always use stable, unique IDs from data
- Generate IDs if data doesn't have them
- Consider compound keys for nested data
- Test list operations thoroughly

**Key Generation for Data Without IDs:**
```tsx
// If your data doesn't have IDs, generate stable ones
function addStableIds<T>(items: T[]): (T & { id: string })[] {
  return items.map((item, index) => ({
    ...item,
    id: `item-${index}-${Date.now()}` // Or use a proper ID library
  }));
}

// Or use a library like uuid
import { v4 as uuidv4 } from 'uuid';

function addUUIDs<T>(items: T[]): (T & { id: string })[] {
  return items.map(item => ({
    ...item,
    id: uuidv4()
  }));
}
```

---

## Practical Project: Product Listing Application

Let's build a complete Product Listing Application that demonstrates all the concepts we've learned in this session: Props, Events, Conditional Rendering, and Lists.

### Project Overview
We'll create a product catalog with:
- Product data with multiple properties
- ProductCard component with props
- ProductList component with conditional rendering
- Add to cart functionality with events
- Stock status with conditional rendering
- Category filtering with list rendering

### Step 1: Project Setup

```bash
# Create new project
npm create vite@latest product-listing -- --template react-ts

# Navigate to project
cd product-listing

# Install dependencies
npm install

# Start development server
npm run dev
```

### Step 2: Define Product Interface

Create `src/types/product.ts`:

```typescript
// src/types/product.ts
export interface Product {
  id: number;
  name: string;
  price: number;
  originalPrice?: number;
  description: string;
  category: string;
  image: string;
  rating: number;
  reviewCount: number;
  inStock: boolean;
  stockCount: number;
  discount?: number;
  featured?: boolean;
}
```

### Step 3: Create Product Data

Create `src/data/products.ts`:

```typescript
// src/data/products.ts
import { Product } from '../types/product';

export const products: Product[] = [
  {
    id: 1,
    name: 'Premium Wireless Headphones',
    price: 199.99,
    originalPrice: 249.99,
    description: 'High-quality wireless headphones with noise cancellation and premium sound.',
    category: 'Electronics',
    image: 'https://images.unsplash.com/photo-1505740420928-5e560c06d30e?w=400',
    rating: 4.5,
    reviewCount: 128,
    inStock: true,
    stockCount: 45,
    discount: 0.2,
    featured: true
  },
  {
    id: 2,
    name: 'Smart Watch Pro',
    price: 299.99,
    description: 'Advanced smartwatch with health monitoring and GPS tracking.',
    category: 'Electronics',
    image: 'https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=400',
    rating: 4.8,
    reviewCount: 256,
    inStock: true,
    stockCount: 32,
    featured: true
  },
  {
    id: 3,
    name: 'Ergonomic Office Chair',
    price: 449.99,
    description: 'Comfortable ergonomic chair with lumbar support and adjustable height.',
    category: 'Furniture',
    image: 'https://images.unsplash.com/photo-1580480055273-228ff5388ef8?w=400',
    rating: 4.3,
    reviewCount: 89,
    inStock: true,
    stockCount: 18
  },
  {
    id: 4,
    name: 'Mechanical Keyboard',
    price: 129.99,
    description: 'RGB mechanical keyboard with cherry switches and customizable lighting.',
    category: 'Electronics',
    image: 'https://images.unsplash.com/photo-1587829741301-dc798b91a803?w=400',
    rating: 4.6,
    reviewCount: 342,
    inStock: false,
    stockCount: 0
  },
  {
    id: 5,
    name: 'Standing Desk',
    price: 599.99,
    description: 'Electric height-adjustable standing desk with memory presets.',
    category: 'Furniture',
    image: 'https://images.unsplash.com/photo-1538688525198-9b88f6f53126?w=400',
    rating: 4.7,
    reviewCount: 156,
    inStock: true,
    stockCount: 8
  },
  {
    id: 6,
    name: 'Wireless Mouse',
    price: 49.99,
    description: 'Precision wireless mouse with ergonomic design and long battery life.',
    category: 'Electronics',
    image: 'https://images.unsplash.com/photo-1527864550417-7fd91fc51a46?w=400',
    rating: 4.2,
    reviewCount: 512,
    inStock: true,
    stockCount: 120
  },
  {
    id: 7,
    name: 'USB-C Hub',
    price: 39.99,
    description: 'Multi-port USB-C hub with HDMI, USB 3.0, and SD card reader.',
    category: 'Electronics',
    image: 'https://images.unsplash.com/photo-1625948515291-69613efd103f?w=400',
    rating: 4.4,
    reviewCount: 267,
    inStock: true,
    stockCount: 75
  },
  {
    id: 8,
    name: 'Monitor Stand',
    price: 79.99,
    description: 'Adjustable monitor stand with storage drawer and cable management.',
    category: 'Furniture',
    image: 'https://images.unsplash.com/photo-1531297484001-80022131f5a1?w=400',
    rating: 4.1,
    reviewCount: 94,
    inStock: true,
    stockCount: 42
  }
];
```

### Step 4: Create ProductCard Component

Create `src/components/ProductCard.tsx`:

```typescript
// src/components/ProductCard.tsx
import { Product } from '../types/product';

interface ProductCardProps {
  product: Product;
  onAddToCart: (productId: number) => void;
  onToggleWishlist: (productId: number) => void;
  isWishlisted: boolean;
}

function ProductCard({ product, onAddToCart, onToggleWishlist, isWishlisted }: ProductCardProps) {
  const handleAddToCart = (event: React.MouseEvent) => {
    event.stopPropagation();
    onAddToCart(product.id);
  };

  const handleToggleWishlist = (event: React.MouseEvent) => {
    event.stopPropagation();
    onToggleWishlist(product.id);
  };

  const handleCardClick = () => {
    console.log(`Viewing details for ${product.name}`);
  };

  return (
    <div 
      className={`product-card ${product.featured ? 'featured' : ''} ${!product.inStock ? 'out-of-stock' : ''}`}
      onClick={handleCardClick}
    >
      {/* Product Image */}
      <div className="product-image-container">
        <img 
          src={product.image} 
          alt={product.name}
          className="product-image"
          loading="lazy"
        />
        
        {/* Wishlist Button */}
        <button 
          className={`wishlist-button ${isWishlisted ? 'active' : ''}`}
          onClick={handleToggleWishlist}
          aria-label={isWishlisted ? 'Remove from wishlist' : 'Add to wishlist'}
        >
          {isWishlisted ? '♥' : '♡'}
        </button>

        {/* Discount Badge */}
        {product.discount && (
          <span className="discount-badge">
            -{Math.round(product.discount * 100)}%
          </span>
        )}

        {/* Featured Badge */}
        {product.featured && (
          <span className="featured-badge">Featured</span>
        )}
      </div>

      {/* Product Info */}
      <div className="product-info">
        {/* Category */}
        <span className="product-category">{product.category}</span>
        
        {/* Product Name */}
        <h3 className="product-name">{product.name}</h3>
        
        {/* Rating */}
        <div className="product-rating">
          <span className="stars">{'★'.repeat(Math.floor(product.rating))}</span>
          <span className="rating-value">{product.rating}</span>
          <span className="review-count">({product.reviewCount})</span>
        </div>

        {/* Price */}
        <div className="product-price">
          {product.discount ? (
            <>
              <span className="original-price">${product.originalPrice?.toFixed(2)}</span>
              <span className="current-price">${(product.price * (1 - product.discount)).toFixed(2)}</span>
            </>
          ) : (
            <span className="current-price">${product.price.toFixed(2)}</span>
          )}
        </div>

        {/* Stock Status */}
        <div className="stock-status">
          {product.inStock ? (
            <span className="in-stock">
              ✓ In Stock ({product.stockCount} available)
            </span>
          ) : (
            <span className="out-of-stock">
              ✗ Out of Stock
            </span>
          )}
        </div>

        {/* Add to Cart Button */}
        <button 
          className={`add-to-cart-button ${!product.inStock ? 'disabled' : ''}`}
          onClick={handleAddToCart}
          disabled={!product.inStock}
        >
          {product.inStock ? 'Add to Cart' : 'Out of Stock'}
        </button>
      </div>
    </div>
  );
}

export default ProductCard;
```

### Step 5: Add ProductCard Styles

Add to `src/index.css`:

```css
/* Product Card Styles */
.product-card {
  background: white;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s, box-shadow 0.3s;
  cursor: pointer;
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.15);
}

.product-card.featured {
  border: 2px solid #ffd700;
}

.product-card.out-of-stock {
  opacity: 0.7;
}

.product-image-container {
  position: relative;
  height: 200px;
  overflow: hidden;
}

.product-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s;
}

.product-card:hover .product-image {
  transform: scale(1.05);
}

.wishlist-button {
  position: absolute;
  top: 10px;
  right: 10px;
  background: white;
  border: none;
  border-radius: 50%;
  width: 36px;
  height: 36px;
  cursor: pointer;
  font-size: 18px;
  transition: background 0.3s;
  z-index: 10;
}

.wishlist-button:hover {
  background: #f0f0f0;
}

.wishlist-button.active {
  color: #e74c3c;
}

.discount-badge {
  position: absolute;
  top: 10px;
  left: 10px;
  background: #e74c3c;
  color: white;
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 12px;
  font-weight: bold;
}

.featured-badge {
  position: absolute;
  bottom: 10px;
  left: 10px;
  background: #ffd700;
  color: #333;
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
  color: #333;
  line-height: 1.4;
}

.product-rating {
  display: flex;
  align-items: center;
  gap: 4px;
  margin: 8px 0;
}

.stars {
  color: #ffd700;
}

.rating-value {
  font-weight: 600;
  color: #333;
}

.review-count {
  color: #666;
  font-size: 12px;
}

.product-price {
  margin: 12px 0;
  display: flex;
  align-items: center;
  gap: 8px;
}

.original-price {
  text-decoration: line-through;
  color: #999;
  font-size: 14px;
}

.current-price {
  font-size: 20px;
  font-weight: bold;
  color: #2ecc71;
}

.stock-status {
  margin: 8px 0;
  font-size: 12px;
}

.in-stock {
  color: #2ecc71;
}

.out-of-stock {
  color: #e74c3c;
}

.add-to-cart-button {
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

.add-to-cart-button:hover:not(.disabled) {
  background: #2980b9;
}

.add-to-cart-button.disabled {
  background: #bdc3c7;
  cursor: not-allowed;
}
```

### Step 6: Create ProductList Component

Create `src/components/ProductList.tsx`:

```typescript
// src/components/ProductList.tsx
import { useState } from 'react';
import { Product } from '../types/product';
import ProductCard from './ProductCard';

interface ProductListProps {
  products: Product[];
}

function ProductList({ products }: ProductListProps) {
  const [selectedCategory, setSelectedCategory] = useState('all');
  const [sortBy, setSortBy] = useState('featured');
  const [searchQuery, setSearchQuery] = useState('');
  const [wishlist, setWishlist] = useState<Set<number>>(new Set());
  const [cart, setCart] = useState<number[]>([]);

  // Get unique categories
  const categories = ['all', ...new Set(products.map(p => p.category))];

  // Filter products
  const filteredProducts = products.filter(product => {
    const matchesCategory = selectedCategory === 'all' || product.category === selectedCategory;
    const matchesSearch = product.name.toLowerCase().includes(searchQuery.toLowerCase()) ||
                         product.description.toLowerCase().includes(searchQuery.toLowerCase());
    return matchesCategory && matchesSearch;
  });

  // Sort products
  const sortedProducts = [...filteredProducts].sort((a, b) => {
    switch (sortBy) {
      case 'price-low':
        return a.price - b.price;
      case 'price-high':
        return b.price - a.price;
      case 'rating':
        return b.rating - a.rating;
      case 'reviews':
        return b.reviewCount - a.reviewCount;
      case 'featured':
      default:
        return (b.featured ? 1 : 0) - (a.featured ? 1 : 0);
    }
  });

  // Handle add to cart
  const handleAddToCart = (productId: number) => {
    setCart(prev => [...prev, productId]);
    console.log(`Added product ${productId} to cart. Total items: ${cart.length + 1}`);
  };

  // Handle toggle wishlist
  const handleToggleWishlist = (productId: number) => {
    setWishlist(prev => {
      const newWishlist = new Set(prev);
      if (newWishlist.has(productId)) {
        newWishlist.delete(productId);
        console.log(`Removed product ${productId} from wishlist`);
      } else {
        newWishlist.add(productId);
        console.log(`Added product ${productId} to wishlist`);
      }
      return newWishlist;
    });
  };

  // Calculate stats
  const inStockCount = sortedProducts.filter(p => p.inStock).length;
  const outOfStockCount = sortedProducts.length - inStockCount;

  return (
    <div className="product-list">
      {/* Filters and Controls */}
      <div className="product-controls">
        {/* Search */}
        <div className="search-box">
          <input
            type="text"
            placeholder="Search products..."
            value={searchQuery}
            onChange={(e) => setSearchQuery(e.target.value)}
            className="search-input"
          />
        </div>

        {/* Category Filter */}
        <div className="category-filter">
          <label htmlFor="category">Category:</label>
          <select
            id="category"
            value={selectedCategory}
            onChange={(e) => setSelectedCategory(e.target.value)}
            className="category-select"
          >
            {categories.map(category => (
              <option key={category} value={category}>
                {category === 'all' ? 'All Categories' : category}
              </option>
            ))}
          </select>
        </div>

        {/* Sort */}
        <div className="sort-filter">
          <label htmlFor="sort">Sort by:</label>
          <select
            id="sort"
            value={sortBy}
            onChange={(e) => setSortBy(e.target.value)}
            className="sort-select"
          >
            <option value="featured">Featured</option>
            <option value="price-low">Price: Low to High</option>
            <option value="price-high">Price: High to Low</option>
            <option value="rating">Rating</option>
            <option value="reviews">Most Reviews</option>
          </select>
        </div>
      </div>

      {/* Stats */}
      <div className="product-stats">
        <span>{sortedProducts.length} products found</span>
        <span>{inStockCount} in stock</span>
        <span>{outOfStockCount} out of stock</span>
        <span>{cart.length} in cart</span>
        <span>{wishlist.size} in wishlist</span>
      </div>

      {/* Loading State */}
      {products.length === 0 && (
        <div className="loading-state">
          <div className="spinner"></div>
          <p>Loading products...</p>
        </div>
      )}

      {/* Empty State */}
      {products.length > 0 && sortedProducts.length === 0 && (
        <div className="empty-state">
          <div className="empty-icon">🔍</div>
          <h3>No products found</h3>
          <p>Try adjusting your search or filters</p>
          <button onClick={() => {
            setSearchQuery('');
            setSelectedCategory('all');
          }}>
            Clear Filters
          </button>
        </div>
      )}

      {/* Product Grid */}
      {sortedProducts.length > 0 && (
        <div className="product-grid">
          {sortedProducts.map(product => (
            <ProductCard
              key={product.id}
              product={product}
              onAddToCart={handleAddToCart}
              onToggleWishlist={handleToggleWishlist}
              isWishlisted={wishlist.has(product.id)}
            />
          ))}
        </div>
      )}
    </div>
  );
}

export default ProductList;
```

### Step 7: Add ProductList Styles

Add to `src/index.css`:

```css
/* Product List Styles */
.product-list {
  max-width: 1400px;
  margin: 0 auto;
  padding: 20px;
}

.product-controls {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
  margin-bottom: 24px;
  padding: 16px;
  background: #f8f9fa;
  border-radius: 8px;
}

.search-box {
  flex: 1;
  min-width: 200px;
}

.search-input {
  width: 100%;
  padding: 10px 16px;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 14px;
}

.search-input:focus {
  outline: none;
  border-color: #3498db;
}

.category-filter,
.sort-filter {
  display: flex;
  align-items: center;
  gap: 8px;
}

.category-filter label,
.sort-filter label {
  font-weight: 600;
  color: #333;
}

.category-select,
.sort-select {
  padding: 10px 16px;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 14px;
  background: white;
  cursor: pointer;
}

.product-stats {
  display: flex;
  gap: 20px;
  margin-bottom: 24px;
  padding: 12px 16px;
  background: #e8f4f8;
  border-radius: 6px;
  font-size: 14px;
  color: #333;
}

.product-stats span {
  font-weight: 500;
}

.loading-state,
.empty-state {
  text-align: center;
  padding: 60px 20px;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 20px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.empty-icon {
  font-size: 48px;
  margin-bottom: 16px;
}

.empty-state h3 {
  margin-bottom: 8px;
  color: #333;
}

.empty-state p {
  color: #666;
  margin-bottom: 16px;
}

.empty-state button {
  padding: 10px 20px;
  background: #3498db;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 600;
}

.empty-state button:hover {
  background: #2980b9;
}

.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 24px;
}

@media (max-width: 768px) {
  .product-controls {
    flex-direction: column;
  }
  
  .product-stats {
    flex-direction: column;
    gap: 8px;
  }
  
  .product-grid {
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 16px;
  }
}
```

### Step 8: Create Main App Component

Update `src/App.tsx`:

```typescript
// src/App.tsx
import { useState } from 'react';
import ProductList from './components/ProductList';
import { products } from './data/products';

function App() {
  const [isLoading, setIsLoading] = useState(true);

  // Simulate loading
  useState(() => {
    setTimeout(() => setIsLoading(false), 1000);
  });

  return (
    <div className="app">
      {/* Header */}
      <header className="app-header">
        <div className="header-content">
          <h1>🛒 TechStore</h1>
          <p>Your one-stop shop for the latest tech products</p>
        </div>
      </header>

      {/* Main Content */}
      <main className="app-main">
        {isLoading ? (
          <div className="loading-state">
            <div className="spinner"></div>
            <p>Loading products...</p>
          </div>
        ) : (
          <ProductList products={products} />
        )}
      </main>

      {/* Footer */}
      <footer className="app-footer">
        <p>&copy; 2024 TechStore. All rights reserved.</p>
      </footer>
    </div>
  );
}

export default App;
```

### Step 9: Add App Styles

Add to `src/index.css`:

```css
/* App Styles */
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
  padding: 40px 20px;
  text-align: center;
}

.header-content h1 {
  font-size: 36px;
  margin-bottom: 8px;
}

.header-content p {
  font-size: 18px;
  opacity: 0.9;
}

.app-main {
  flex: 1;
  padding: 40px 20px;
}

.app-footer {
  background: #333;
  color: white;
  text-align: center;
  padding: 20px;
  margin-top: auto;
}
```

### Step 10: Test the Application

```bash
# Make sure the dev server is running
npm run dev
```

Open your browser and navigate to `http://localhost:5173` to see your Product Listing Application!

### Step 11: Additional Features

Let's add a few more features to demonstrate the concepts better:

#### Add Cart Summary Component

Create `src/components/CartSummary.tsx`:

```typescript
// src/components/CartSummary.tsx
interface CartSummaryProps {
  cartCount: number;
  onClearCart: () => void;
}

function CartSummary({ cartCount, onClearCart }: CartSummaryProps) {
  return (
    <div className="cart-summary">
      <div className="cart-info">
        <span className="cart-icon">🛒</span>
        <span className="cart-count">{cartCount} items</span>
      </div>
      {cartCount > 0 && (
        <button onClick={onClearCart} className="clear-cart-btn">
          Clear Cart
        </button>
      )}
    </div>
  );
}

export default CartSummary;
```

#### Add to ProductList:

```typescript
// Add this to ProductList component
const handleClearCart = () => {
  setCart([]);
  console.log('Cart cleared');
};

// Add this in the return statement, before product-controls
<CartSummary cartCount={cart.length} onClearCart={handleClearCart} />
```

#### Add Cart Summary Styles:

```css
.cart-summary {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px;
  background: white;
  border-radius: 8px;
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.cart-info {
  display: flex;
  align-items: center;
  gap: 8px;
}

.cart-icon {
  font-size: 24px;
}

.cart-count {
  font-weight: 600;
  color: #333;
}

.clear-cart-btn {
  padding: 8px 16px;
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 600;
}

.clear-cart-btn:hover {
  background: #c0392b;
}
```

### Step 12: Build for Production

```bash
# Build the project
npm run build

# Preview the production build
npm run preview
```

---

## Comprehensive Review

### Session Summary

In this session, we covered:

1. **Props:** Understanding parent-child communication, prop types, destructuring, TypeScript interfaces, and default props
2. **Events:** React's synthetic event system, onClick, onChange, onSubmit, event objects, and passing arguments to handlers
3. **Conditional Rendering:** Using if statements, ternary operators, logical AND, and handling loading/empty/error states
4. **Rendering Lists:** Using map(), the importance of keys, and why array indices as keys can be problematic

### Key Takeaways

- Props enable one-way data flow from parent to child components
- TypeScript interfaces provide type safety for props
- React events are synthetic and cross-browser compatible
- Conditional rendering keeps UI dynamic and responsive
- Keys are crucial for list rendering performance and correctness
- Never use array indices as keys for dynamic lists

---

## 15 Student Questions

1. What are props in React and how do they enable component communication?
2. Explain the difference between passing props as strings versus JavaScript expressions.
3. Why do we use TypeScript interfaces for component props?
4. What is the children prop and when would you use it?
5. How do you handle default values for optional props?
6. What are synthetic events in React and why are they used?
7. Explain the difference between onClick and onChange events.
8. How do you prevent default behavior in React event handlers?
9. What are the different ways to pass arguments to event handlers?
10. When would you use if statements versus ternary operators for conditional rendering?
11. What is the logical AND operator used for in conditional rendering?
12. How do you handle loading, empty, and error states in React components?
13. Why is the map() method preferred over forEach for rendering lists?
14. What is the purpose of keys in React lists?
15. Why can using array indices as keys be problematic in dynamic lists?

---

## 5 Interview Questions

### 1. Explain the difference between controlled and uncontrolled components in React forms.

**Answer:** Controlled components have their form data controlled by React state. The value of the input is set to a state variable, and changes are handled through onChange events that update the state. This gives React complete control over the form data.

Uncontrolled components maintain their own internal state. The DOM itself handles the form data, and you access the values using refs. This is more like traditional HTML forms.

Controlled components are preferred in React because they provide better control, validation, and consistency with React's state management.

### 2. How does React's event system differ from native DOM events?

**Answer:** React uses synthetic events that wrap native browser events. Key differences include:

- **Cross-browser compatibility:** Synthetic events normalize behavior across browsers
- **Event pooling:** React reuses event objects for performance (though this is changing in newer versions)
- **Event delegation:** React attaches event listeners at the root rather than individual elements
- **Consistent API:** Synthetic events provide a consistent interface regardless of the browser
- **Performance:** Delegation and pooling improve performance for many events

Synthetic events have the same interface as native events but with these React-specific optimizations.

### 3. What are the best practices for conditional rendering in React?

**Answer:** Best practices include:

- **Choose the right operator:** Use ternary for either/or, && for optional rendering, if statements for complex logic
- **Extract complex conditions:** Move complex conditional logic to separate functions
- **Handle all states:** Always consider loading, error, empty, and success states
- **Keep it readable:** Avoid deeply nested ternary operators
- **Use early returns:** Simplify components by returning early for different states
- **Consider performance:** Be mindful of expensive operations in conditions
- **Maintain consistency:** Use similar patterns across your codebase

### 4. Explain the concept of keys in React and why they are important for performance.

**Answer:** Keys help React identify which items have changed, been added, or been removed in a list. They are crucial for:

- **Reconciliation:** React uses keys to match elements in the virtual DOM
- **Performance:** Proper keys enable React to reuse DOM elements efficiently
- **State preservation:** Component state stays with the correct element when lists change
- **Focus management:** Input focus is maintained correctly with proper keys
- **Animations:** Smooth transitions when list order changes

Keys should be:
- Unique among siblings
- Stable across renders
- Meaningful (representing item identity)
- From data when possible (not generated)

### 5. How would you optimize a list that renders thousands of items in React?

**Answer:** Optimization strategies include:

- **Virtualization:** Use libraries like react-window or react-virtualized to only render visible items
- **Proper keys:** Use stable, unique keys from data
- **Memoization:** Use React.memo for list items to prevent unnecessary re-renders
- **Pagination:** Implement pagination to reduce the number of items rendered at once
- **Lazy loading:** Load data incrementally as needed
- **Debouncing:** For search/filter operations on large lists
- **Web Workers:** Offload heavy processing to web workers
- **Code splitting:** Split components to reduce initial bundle size

Example with memoization:
```typescript
const ListItem = React.memo(({ item }) => {
  return <div>{item.name}</div>;
});
```

---

## 5 Practical Exercises

### Exercise 1: Dynamic Form with Validation

Create a registration form with the following requirements:
- Fields: name, email, password, confirm password
- Real-time validation with error messages
- Conditional submit button (disabled until valid)
- Show password strength indicator
- Handle form submission with loading state

**Hints:** Use controlled components, conditional rendering for errors, and event handlers for validation.

### Exercise 2: Interactive Todo List

Build a todo list application with:
- Add, edit, and delete todos
- Mark todos as complete/incomplete
- Filter by status (all, active, completed)
- Search functionality
- Persist todos to localStorage
- Show empty state when no todos

**Hints:** Use array methods for filtering, proper keys for list items, and conditional rendering for different states.

### Exercise 3: Product Filter System

Create a product filter system with:
- Multiple filter categories (price range, brand, rating)
- Active filter indicators with remove buttons
- Clear all filters functionality
- Show number of results
- Handle no results state
- Sort functionality

**Hints:** Use complex conditional logic, array filtering, and state management for filters.

### Exercise 4: User Profile with Conditional Features

Build a user profile component that:
- Shows different content based on user role (admin, user, guest)
- Displays edit profile button only for profile owner
- Shows admin panel for admin users
- Handles loading and error states
- Shows empty state for incomplete profiles
- Includes conditional features based on user settings

**Hints:** Use multiple conditional rendering techniques and role-based logic.

### Exercise 5: Image Gallery with Lightbox

Create an image gallery with:
- Grid layout of images
- Click to open lightbox
- Navigation (previous/next) in lightbox
- Keyboard navigation (arrow keys)
- Close on escape key
- Loading state for images
- Error state for failed images
- Empty state when no images

**Hints:** Use event handlers for keyboard events, conditional rendering for states, and proper keys for list items.

---

## Homework

### Reading Assignment
1. Read the official React documentation on "Lists and Keys"
2. Read the React documentation on "Forms" and "Handling Events"
3. Review TypeScript documentation on "Interfaces" and "Type Inference"

### Practice Exercises
1. **Component Library:** Create a set of reusable form components (Input, Select, Checkbox, Radio) with proper TypeScript interfaces and event handling.

2. **State Management:** Enhance the Product Listing Application by:
   - Adding a shopping cart page
   - Implementing quantity controls
   - Adding product comparison feature
   - Creating a checkout process

3. **Performance Optimization:** Optimize the Product Listing Application by:
   - Implementing React.memo for ProductCard
   - Adding virtual scrolling for large product lists
   - Implementing lazy loading for product images
   - Adding code splitting for different routes

### Research Project
Research and write a brief comparison (500 words) of different approaches to form handling in React:
- Controlled components
- Uncontrolled components with refs
- Form libraries (React Hook Form, Formik)
- Server actions (Next.js)

Include pros and cons of each approach and recommend use cases.

---

## Challenge Exercise

### Advanced E-commerce Features

Take the Product Listing Application we built and add the following advanced features:

#### 1. Advanced Filtering System
- Multi-select filters for categories
- Price range slider with min/max values
- Rating filter with interactive star selection
- Brand filter with checkboxes
- Save filter presets
- URL-based filter state (shareable links)

#### 2. Product Comparison
- Select up to 4 products to compare
- Side-by-side comparison view
- Highlight differences between products
- Add/remove products from comparison
- Share comparison link

#### 3. Advanced Cart Features
- Quantity controls with +/- buttons
- Bulk actions (remove all, move to wishlist)
- Cart persistence across sessions
- Estimated shipping calculator
- Promo code system with validation
- Guest checkout vs registered user checkout

#### 4. User Experience Enhancements
- Skeleton loading states for better perceived performance
- Infinite scroll for product listing
- Recently viewed products
- Product recommendations based on browsing history
- Quick view modal for products
- Image zoom functionality

#### 5. Analytics Integration
- Track product views
- Track add to cart events
- Track filter usage
- Track search queries
- Track conversion funnel

**Requirements:**
- Use all concepts from this session (props, events, conditional rendering, lists)
- Implement proper TypeScript typing throughout
- Handle all edge cases (loading, error, empty states)
- Ensure responsive design
- Add proper error handling
- Include loading states for all async operations
- Use proper keys for all list rendering
- Implement accessibility features

**Bonus:** Add unit tests for your components using React Testing Library.

**Evaluation Criteria:**
- Code quality and organization
- Proper use of React concepts
- TypeScript type safety
- User experience
- Performance considerations
- Error handling
- Code reusability

This challenge will test your understanding of all the concepts covered in this session and push you to apply them in a real-world scenario.

---

## Additional Resources

- [React Documentation - Lists and Keys](https://react.dev/learn/rendering-lists)
- [React Documentation - Forms](https://react.dev/learn/adding-interactivity)
- [TypeScript Documentation - Interfaces](https://www.typescriptlang.org/docs/handbook/2/interfaces.html)
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)
- [Web.dev - Performance Patterns](https://web.dev/performance/)

---

**Congratulations on completing Session 2!** You now have a solid understanding of Props, Events, Conditional Rendering, and Lists in React. Continue practicing with the exercises and challenge to reinforce these concepts before moving to the next session.