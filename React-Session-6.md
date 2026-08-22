# React.js Session 6: Routing with React Router

**Duration:** 3 hours  
**Level:** Beginner  
**Prerequisites:** Session 1-5 completed (Components, Props, Events, State, Hooks, Forms, API Integration)  
**Styling:** Tailwind CSS

---

## Session Timeline

### Part 1: Routing Fundamentals (30 minutes)
- **0:00-0:05:** Why Routing? (Topic 1)
- **0:05-0:10:** SPA Routing (Topic 2)
- **0:10-0:15:** React Router Introduction (Topic 3)
- **0:15-0:20:** BrowserRouter (Topic 4)
- **0:20-0:25:** Routes & Route (Topics 5-6)
- **0:25-0:30:** Routing Fundamentals Practice

### Part 2: Navigation Components (40 minutes)
- **0:30-0:35:** Link vs NavLink (Topics 7-8)
- **0:35-0:40:** useNavigate Hook (Topic 9)
- **0:40-0:45:** useParams Hook (Topic 10)
- **0:45-0:50:** useLocation Hook (Topic 11)
- **0:50-1:00:** Navigation Components Practice
- **1:00-1:10:** Break

### Part 3: Advanced Routing (40 minutes)
- **1:10-1:15:** Dynamic Routes (Topic 12)
- **1:15-1:20:** Nested Routes (Topic 13)
- **1:20-1:25:** Layouts (Topic 14)
- **1:25-1:30:** Outlet (Topic 15)
- **1:30-1:40:** Advanced Routing Practice
- **1:40-1:50:** Break

### Part 4: Route Protection (40 minutes)
- **1:50-1:55:** 404 Page (Topic 16)
- **1:55-2:00:** Protected Routes (Topic 17)
- **2:00-2:05:** Authentication Guard (Topic 18)
- **2:05-2:10:** Route-based Authorization (Topic 19)
- **2:10-2:30:** Route Protection Practice

### Part 5: Project Architecture & Implementation (30 minutes)
- **2:30-2:35:** Project Architecture (Topic 20)
- **2:35-3:00:** Admin Dashboard Implementation

---

## Part 1: Routing Fundamentals

### 1. Why Routing?

**What is it?**
Routing is the mechanism that determines which component to display based on the URL in the browser's address bar. It enables navigation between different views/pages in a single-page application (SPA).

**Why we need it?**
- Navigate between different views without page reload
- Maintain application state during navigation
- Provide bookmarkable URLs
- Enable browser back/forward button functionality
- Create meaningful URLs for SEO
- Shareable links to specific views

**How it works?**
- URL changes trigger route matching
- Router matches URL to route configuration
- Corresponding component renders
- Browser history is managed
- Navigation updates URL without reload

**Traditional Multi-Page vs SPA Routing:**

**Traditional Multi-Page App:**
```
User clicks link → Browser navigates to new URL → Server sends new HTML page → Page reloads → State lost
```

**Single-Page App with Routing:**
```
User clicks link → Router updates URL → Router matches route → New component renders → No page reload → State preserved
```

**Simple Example:**
```typescript
// Without routing - all in one page
function App() {
  const [view, setView] = useState('home');
  
  return (
    <div>
      <nav>
        <button onClick={() => setView('home')}>Home</button>
        <button onClick={() => setView('about')}>About</button>
      </nav>
      {view === 'home' && <Home />}
      {view === 'about' && <About />}
    </div>
  );
}

// With routing - URL-based navigation
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
      </Routes>
    </BrowserRouter>
  );
}
```

**Real-world Example:**
```typescript
// E-commerce site without routing - state-based
function EcommerceApp() {
  const [currentPage, setCurrentPage] = useState('products');
  const [selectedProduct, setSelectedProduct] = useState(null);
  
  // Cannot bookmark specific product
  // Cannot share product URL
  // Back button doesn't work properly
  // URL doesn't reflect current view
}

// E-commerce site with routing - URL-based
function EcommerceApp() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<ProductList />} />
        <Route path="/products/:id" element={<ProductDetails />} />
        <Route path="/cart" element={<Cart />} />
        <Route path="/checkout" element={<Checkout />} />
      </Routes>
    </BrowserRouter>
  );
}

// Benefits:
// /products/123 - bookmarkable, shareable
// Back button works naturally
// URL reflects current view
// SEO-friendly URLs
```

**Routing Benefits:**
- **User Experience:** Seamless navigation without page reloads
- **State Preservation:** Application state maintained during navigation
- **Bookmarking:** Users can bookmark specific views
- **Sharing:** URLs can be shared to specific content
- **SEO:** Clean, meaningful URLs for search engines
- **Browser Integration:** Back/forward buttons work naturally

---

### 2. SPA Routing

**What is it?**
Single-Page Application (SPA) routing is a client-side routing mechanism where the application handles navigation entirely in the browser without making requests to the server for new HTML pages.

**Why we need it?**
- Faster navigation (no server round-trip)
- Better user experience (no page refresh)
- Maintains application state
- Offline-capable applications
- Reduced server load
- Modern web application standard

**How it works?**
- Router intercepts URL changes
- Matches URL to client-side routes
- Renders corresponding component
- Updates browser history
- All done in JavaScript

**Traditional Routing vs SPA Routing:**

| Aspect | Traditional Routing | SPA Routing |
|--------|-------------------|-------------|
| Page Reload | Yes | No |
| Server Request | Every navigation | Initial load only |
| State Preservation | Lost on navigation | Preserved |
| Speed | Slower | Faster |
| User Experience | Page flicker | Smooth transitions |
| Browser History | Server-managed | Client-managed |

**Simple Example:**
```typescript
// Traditional routing (server-side)
// User visits /about
// Browser requests /about from server
// Server sends about.html
// Browser renders new page
// Page reloads, state lost

// SPA routing (client-side)
// User visits /about
// Router detects URL change
// Router matches /about to About component
// About component renders
// No page reload, state preserved
```

**Real-world Example:**
```typescript
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <div className="app">
        <Navbar />
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
          <Route path="/products" element={<Products />} />
          <Route path="/products/:id" element={<ProductDetails />} />
          <Route path="/cart" element={<Cart />} />
          <Route path="/checkout" element={<Checkout />} />
        </Routes>
        <Footer />
      </div>
    </BrowserRouter>
  );
}

// Navigation flow:
// 1. User on /
// 2. User clicks link to /products/123
// 3. URL changes to /products/123
// 4. Router matches /products/:id
// 5. ProductDetails component renders
// 6. No page reload, cart state preserved
// 7. Back button returns to /products
```

**SPA Routing Techniques:**

1. **Hash-based Routing:**
```typescript
// URL: http://example.com/#/about
// Uses hash (#) for routing
// Works in older browsers
// Less SEO-friendly
```

2. **History API Routing:**
```typescript
// URL: http://example.com/about
// Uses HTML5 History API
// Clean URLs
// Better SEO
// Requires server configuration
```

**SPA Routing Best Practices:**
- Use history API for production
- Configure server for SPA support
- Implement loading states
- Handle 404 routes
- Optimize for SEO with proper meta tags
- Test back/forward navigation

---

### 3. React Router

**What is it?**
React Router is the standard routing library for React applications, providing a complete routing solution with declarative routing components and hooks.

**Why we need it?**
- Declarative routing syntax
- Component-based navigation
- Access to routing data via hooks
- Code splitting support
- Nested routes support
- Widely used and well-maintained

**How it works?**
- Wraps application with Router provider
- Defines routes with Route components
- Provides navigation components
- Exposes routing hooks
- Manages browser history

**Installation:**
```bash
npm install react-router-dom
# or
yarn add react-router-dom
```

**Core Components:**
- `BrowserRouter`: Router provider using HTML5 History API
- `Routes`: Container for route definitions
- `Route`: Individual route definition
- `Link`: Navigation component
- `NavLink`: Active link component
- `Outlet`: Container for nested routes

**Core Hooks:**
- `useNavigate`: Programmatic navigation
- `useParams`: Access route parameters
- `useLocation`: Access location information
- `useSearchParams`: Access query parameters
- `useRoutes`: Hook-based routing

**Simple Example:**
```typescript
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/contact">Contact</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </BrowserRouter>
  );
}
```

**Real-world Example:**
```typescript
import { BrowserRouter, Routes, Route, Link, useNavigate, useParams } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Layout>
        <Routes>
          <Route path="/" element={<Dashboard />} />
          <Route path="/products" element={<ProductList />} />
          <Route path="/products/:id" element={<ProductDetails />} />
          <Route path="/orders" element={<OrderList />} />
          <Route path="/orders/:id" element={<OrderDetails />} />
          <Route path="/settings" element={<Settings />} />
          <Route path="*" element={<NotFound />} />
        </Routes>
      </Layout>
    </BrowserRouter>
  );
}

function ProductDetails() {
  const { id } = useParams<{ id: string }>();
  const navigate = useNavigate();
  
  return (
    <div>
      <h1>Product {id}</h1>
      <button onClick={() => navigate('/products')}>Back to Products</button>
    </div>
  );
}
```

**React Router Versions:**
- **v6:** Current version (2022+)
  - Simplified API
  - No Switch component
  - Routes instead of Switch
  - Nested routes with Outlet
  - useNavigate instead of useHistory

- **v5:** Previous version (2020-2022)
  - Switch component
  - useHistory hook
  - Different nested route syntax

**React Router Best Practices:**
- Use BrowserRouter for production
- Define routes in a central location
- Use Route components for structure
- Leverage hooks for programmatic navigation
- Implement 404 route with `*` path
- Use nested routes for layouts

---

### 4. BrowserRouter

**What is it?**
BrowserRouter is the most common router component in React Router that uses the HTML5 History API to keep your UI in sync with the URL.

**Why we need it?**
- Clean URLs (no hash)
- Standard browser navigation
- SEO-friendly
- Back/forward button support
- Best for production applications

**How it works?**
- Wraps entire application
- Listens to URL changes
- Manages browser history
- Provides routing context
- Enables all routing features

**Syntax:**
```typescript
import { BrowserRouter } from 'react-router-dom';

<BrowserRouter>
  {/* Your app components */}
</BrowserRouter>
```

**Simple Example:**
```typescript
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

**Real-world Example:**
```typescript
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter basename="/admin">
      <div className="app">
        <Sidebar />
        <main className="main-content">
          <Routes>
            <Route path="/" element={<Dashboard />} />
            <Route path="/users" element={<Users />} />
            <Route path="/products" element={<Products />} />
            <Route path="/settings" element={<Settings />} />
          </Routes>
        </main>
      </div>
    </BrowserRouter>
  );
}

// With custom history
import { createBrowserHistory } from 'history';
import { Router } from 'react-router-dom';

const history = createBrowserHistory();

function App() {
  return (
    <Router location={history.location} navigator={history}>
      {/* Routes */}
    </Router>
  );
}
```

**BrowserRouter Props:**
- `basename`: Base URL for all routes (useful for subdirectories)
- `future`: Enable future v7 features
- `window`: Custom window object (testing)

**Router Types:**

1. **BrowserRouter:** Production router (clean URLs)
```typescript
<BrowserRouter>
  {/* Routes */}
</BrowserRouter>
```

2. **HashRouter:** Fallback router (hash URLs)
```typescript
<HashRouter>
  {/* Routes */}
</HashRouter>
// URL: http://example.com/#/about
```

3. **MemoryRouter:** Testing router (no URL)
```typescript
<MemoryRouter>
  {/* Routes */}
</MemoryRouter>
// No URL changes, for testing
```

**BrowserRouter Best Practices:**
- Use BrowserRouter for production
- Set basename for subdirectory deployments
- Wrap only once at the root
- Consider HashRouter for static hosting
- Use MemoryRouter for testing

---

### 5. Routes

**What is it?**
Routes is a container component that renders the first child Route that matches the current URL. It replaced the Switch component in React Router v6.

**Why we need it?**
- Container for route definitions
- Renders first matching route
- Handles 404 with wildcard route
- Supports nested routes
- Enables route matching logic

**How it works?**
- Receives Route children
- Evaluates each Route
- Renders first match
- No multiple matches (by default)
- Supports nested routes

**Syntax:**
```typescript
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
</Routes>
```

**Simple Example:**
```typescript
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </BrowserRouter>
  );
}
```

**Real-world Example:**
```typescript
function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Public routes */}
        <Route path="/" element={<Landing />} />
        <Route path="/login" element={<Login />} />
        <Route path="/register" element={<Register />} />
        
        {/* Protected routes */}
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/profile" element={<Profile />} />
        <Route path="/settings" element={<Settings />} />
        
        {/* 404 route - must be last */}
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}
```

**Routes vs Switch (v5):**

**v5 (Switch):**
```typescript
<Switch>
  <Route exact path="/" component={Home} />
  <Route path="/about" component={About} />
</Switch>
```

**v6 (Routes):**
```typescript
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
</Routes>
```

**Routes Best Practices:**
- Include 404 route with `*` at the end
- Order routes from specific to general
- Use Routes only once per router
- Nest Routes for layouts
- Keep route definitions organized

---

### 6. Route

**What is it?**
Route is a component that defines a mapping between a URL path and a React component to render when that path is matched.

**Why we need it?**
- Define route-to-component mapping
- Support dynamic parameters
- Enable nested routes
- Provide route-specific data
- Enable code splitting

**How it works?**
- Defines path pattern
- Matches current URL
- Renders element if match
- Passes route parameters
- Supports nesting

**Syntax:**
```typescript
<Route path="/about" element={<About />} />
```

**Simple Example:**
```typescript
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
  <Route path="/contact" element={<Contact />} />
</Routes>
```

**Real-world Example:**
```typescript
function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Static routes */}
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        
        {/* Dynamic routes with parameters */}
        <Route path="/users/:id" element={<UserDetails />} />
        <Route path="/products/:id" element={<ProductDetails />} />
        
        {/* Optional parameters */}
        <Route path="/search/:query?" element={<SearchResults />} />
        
        {/* Multiple parameters */}
        <Route path="/category/:category/product/:id" element={<Product />} />
        
        {/* Nested routes */}
        <Route path="/dashboard" element={<DashboardLayout />}>
          <Route index element={<DashboardHome />} />
          <Route path="profile" element={<Profile />} />
          <Route path="settings" element={<Settings />} />
        </Route>
        
        {/* 404 route */}
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}
```

**Route Props:**
- `path`: URL path pattern
- `element`: Component to render
- `index`: Index route for nested routes
- `caseSensitive`: Case-sensitive matching (default: false)

**Route Path Patterns:**

1. **Static path:**
```typescript
<Route path="/about" element={<About />} />
```

2. **Dynamic parameter:**
```typescript
<Route path="/users/:id" element={<UserDetails />} />
```

3. **Optional parameter:**
```typescript
<Route path="/search/:query?" element={<Search />} />
```

4. **Wildcard:**
```typescript
<Route path="*" element={<NotFound />} />
```

5. **Multiple parameters:**
```typescript
<Route path="/users/:userId/posts/:postId" element={<Post />} />
```

**Route Best Practices:**
- Use specific paths before general ones
- Use descriptive parameter names
- Handle 404 with wildcard route
- Use index routes for default nested routes
- Consider code splitting for large routes

---

## Part 2: Navigation Components

### 7. Link

**What is it?**
Link is a React Router component that provides declarative navigation around the application, similar to HTML's `<a>` tag but without causing a full page reload.

**Why we need it?**
- Navigate without page reload
- Preserve application state
- Update browser history
- SEO-friendly
- Works like HTML anchor tag

**How it works?**
- Renders as `<a>` tag
- Intercepts click events
- Updates URL via router
- Prevents page reload
- Maintains state

**Syntax:**
```typescript
<Link to="/about">About</Link>
```

**Simple Example:**
```typescript
function Navbar() {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
      <Link to="/contact">Contact</Link>
    </nav>
  );
}
```

**Real-world Example:**
```typescript
function Navbar() {
  return (
    <nav className="navbar">
      <div className="logo">
        <Link to="/">
          <img src="/logo.png" alt="Logo" />
        </Link>
      </div>
      
      <ul className="nav-links">
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/products">Products</Link>
        </li>
        <li>
          <Link to="/about">About</Link>
        </li>
        <li>
          <Link to="/contact">Contact</Link>
        </li>
      </ul>
      
      <div className="nav-actions">
        <Link to="/cart" className="cart-link">
          <ShoppingCart />
          <span>Cart</span>
        </Link>
        <Link to="/login" className="login-link">
          Login
        </Link>
      </div>
    </nav>
  );
}

// Link with state
function ProductCard({ product }) {
  return (
    <div className="product-card">
      <h3>{product.name}</h3>
      <Link 
        to="/product-details" 
        state={{ productId: product.id }}
      >
        View Details
      </Link>
    </div>
  );
}

// Link with query parameters
function SearchForm() {
  const [query, setQuery] = useState('');
  
  return (
    <form>
      <input 
        value={query}
        onChange={(e) => setQuery(e.target.value)}
      />
      <Link to={`/search?q=${encodeURIComponent(query)}`}>
        Search
      </Link>
    </form>
  );
}
```

**Link Props:**
- `to`: Destination path or location object
- `replace`: Replace current history entry
- `state`: State to pass to route
- `target`: Target attribute (e.g., "_blank")

**Link vs HTML Anchor:**

```typescript
// HTML anchor - causes page reload
<a href="/about">About</a>

// React Router Link - no page reload
<Link to="/about">About</Link>
```

**Link Best Practices:**
- Use Link for internal navigation
- Use `<a>` for external links
- Pass state when needed
- Use replace for redirects
- Consider accessibility (aria-labels)

---

### 8. NavLink

**What is it?**
NavLink is a special version of Link that adds styling attributes to the rendered element when it matches the current URL, making it ideal for navigation menus.

**Why we need it?**
- Highlight active navigation
- Show current page in menu
- Better UX for navigation
- Automatic active state
- Customizable styling

**How it works?**
- Extends Link component
- Checks if path matches URL
- Adds className for active state
- Provides style object
- Supports custom matching logic

**Syntax:**
```typescript
<NavLink to="/about" className="nav-link">
  About
</NavLink>
```

**Simple Example:**
```typescript
function Navbar() {
  return (
    <nav>
      <NavLink to="/" className="nav-link">Home</NavLink>
      <NavLink to="/about" className="nav-link">About</NavLink>
      <NavLink to="/contact" className="nav-link">Contact</NavLink>
    </nav>
  );
}

// CSS
.nav-link {
  color: #333;
  text-decoration: none;
}

.nav-link.active {
  color: #667eea;
  font-weight: bold;
}
```

**Real-world Example:**
```typescript
function Sidebar() {
  return (
    <aside className="sidebar">
      <NavLink 
        to="/dashboard" 
        className={({ isActive }) => 
          `nav-item ${isActive ? 'active' : ''}`
        }
      >
        <DashboardIcon />
        <span>Dashboard</span>
      </NavLink>
      
      <NavLink 
        to="/products" 
        className={({ isActive }) => 
          `nav-item ${isActive ? 'active' : ''}`
        }
      >
        <ProductsIcon />
        <span>Products</span>
      </NavLink>
      
      <NavLink 
        to="/orders" 
        className={({ isActive }) => 
          `nav-item ${isActive ? 'active' : ''}`
        }
      >
        <OrdersIcon />
        <span>Orders</span>
      </NavLink>
      
      <NavLink 
        to="/settings" 
        className={({ isActive }) => 
          `nav-item ${isActive ? 'active' : ''}`
        }
      >
        <SettingsIcon />
        <span>Settings</span>
      </NavLink>
    </aside>
  );
}

// With custom matching
function AdminNav() {
  return (
    <nav>
      <NavLink 
        to="/users" 
        end
        className={({ isActive }) => 
          `nav-link ${isActive ? 'active' : ''}`
        }
      >
        Users
      </NavLink>
      
      <NavLink 
        to="/users/:id" 
        className={({ isActive }) => 
          `nav-link ${isActive ? 'active' : ''}`
        }
      >
        User Details
      </NavLink>
    </nav>
  );
}
```

**NavLink Props:**
- All Link props
- `className`: Function or string for class
- `style`: Function or object for styles
- `end`: Match exactly (not parent routes)
- `caseSensitive`: Case-sensitive matching

**NavLink vs Link:**

```typescript
// Link - no active state
<Link to="/about">About</Link>

// NavLink - adds active class
<NavLink to="/about" className="nav-link">About</NavLink>
// Renders: <a href="/about" class="nav-link active">About</a>
```

**NavLink Best Practices:**
- Use for navigation menus
- Provide active styling
- Use `end` for exact matching
- Consider nested route behavior
- Keep styling consistent

---

### 9. useNavigate

**What is it?**
useNavigate is a React Router hook that provides programmatic navigation, allowing you to navigate to different routes imperatively (e.g., after form submission, authentication, etc.).

**Why we need it?**
- Navigate programmatically
- Navigate after actions
- Navigate with state
- Replace history entry
- Navigate in event handlers

**How it works?**
- Returns navigate function
- Call with path to navigate
- Can pass options
- Can pass state
- Updates browser history

**Syntax:**
```typescript
const navigate = useNavigate();
navigate('/about');
```

**Simple Example:**
```typescript
function LoginForm() {
  const navigate = useNavigate();
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    // Login logic
    navigate('/dashboard');
  };
  
  return <form onSubmit={handleSubmit}>{/* ... */}</form>;
}
```

**Real-world Example:**
```typescript
function LoginForm() {
  const navigate = useNavigate();
  const [loading, setLoading] = useState(false);
  
  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setLoading(true);
    
    try {
      await login(formData);
      navigate('/dashboard', { replace: true });
    } catch (error) {
      setError('Login failed');
    } finally {
      setLoading(false);
    }
  };
  
  return <form onSubmit={handleSubmit}>{/* ... */}</form>;
}

function ProductCard({ product }) {
  const navigate = useNavigate();
  
  const handleDelete = async () => {
    if (confirm('Delete this product?')) {
      await deleteProduct(product.id);
      navigate('/products', { replace: true });
    }
  };
  
  const handleEdit = () => {
    navigate(`/products/${product.id}/edit`, {
      state: { product }
    });
  };
  
  return (
    <div className="product-card">
      <h3>{product.name}</h3>
      <button onClick={handleEdit}>Edit</button>
      <button onClick={handleDelete}>Delete</button>
    </div>
  );
}

function OrderSuccess() {
  const navigate = useNavigate();
  const location = useLocation();
  const { orderId } = location.state || {};
  
  const continueShopping = () => {
    navigate('/products');
  };
  
  const viewOrder = () => {
    navigate(`/orders/${orderId}`);
  };
  
  return (
    <div>
      <h1>Order Successful!</h1>
      <button onClick={continueShopping}>Continue Shopping</button>
      <button onClick={viewOrder}>View Order</button>
    </div>
  );
}

// Navigate with delay
function RedirectTimer() {
  const navigate = useNavigate();
  
  useEffect(() => {
    const timer = setTimeout(() => {
      navigate('/dashboard');
    }, 3000);
    
    return () => clearTimeout(timer);
  }, [navigate]);
  
  return <div>Redirecting in 3 seconds...</div>;
}

// Navigate back
function BackButton() {
  const navigate = useNavigate();
  
  return (
    <button onClick={() => navigate(-1)}>
      Back
    </button>
  );
}

// Navigate forward
function ForwardButton() {
  const navigate = useNavigate();
  
  return (
    <button onClick={() => navigate(1)}>
      Forward
    </button>
  );
}
```

**useNavigate Options:**
- `replace`: Replace current history entry
- `state`: Pass state to route
- Number: Navigate in history (-1, 1, etc.)

**useNavigate vs useHistory (v5):**

**v5 (useHistory):**
```typescript
const history = useHistory();
history.push('/about');
history.replace('/about');
history.goBack();
```

**v6 (useNavigate):**
```typescript
const navigate = useNavigate();
navigate('/about');
navigate('/about', { replace: true });
navigate(-1);
```

**useNavigate Best Practices:**
- Use for programmatic navigation
- Use Link for declarative navigation
- Replace history for redirects
- Pass state when needed
- Handle navigation errors

---

### 10. useParams

**What is it?**
useParams is a React Router hook that returns an object of key/value pairs of the dynamic parameters from the current URL that were matched by the `<Route path>`.

**Why we need it?**
- Access route parameters
- Fetch data based on ID
- Build dynamic pages
- Handle resource identifiers
- Type-safe parameter access

**How it works?**
- Returns params object
- Keys match route parameters
- Extracted from URL
- Updates on route change
- Can be typed with TypeScript

**Syntax:**
```typescript
const { id } = useParams();
```

**Simple Example:**
```typescript
// Route: <Route path="/users/:id" element={<UserDetails />} />

function UserDetails() {
  const { id } = useParams<{ id: string }>();
  
  return <div>User ID: {id}</div>;
}
```

**Real-world Example:**
```typescript
// Route definitions
<Routes>
  <Route path="/products/:id" element={<ProductDetails />} />
  <Route path="/users/:userId/posts/:postId" element={<PostDetails />} />
  <Route path="/category/:categoryName" element={<CategoryPage />} />
</Routes>

// Product Details
function ProductDetails() {
  const { id } = useParams<{ id: string }>();
  const [product, setProduct] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    const fetchProduct = async () => {
      try {
        const data = await productService.getProduct(id);
        setProduct(data);
      } catch (error) {
        console.error('Failed to fetch product:', error);
      } finally {
        setLoading(false);
      }
    };
    
    fetchProduct();
  }, [id]);
  
  if (loading) return <LoadingSpinner />;
  if (!product) return <NotFound />;
  
  return (
    <div className="product-details">
      <h1>{product.name}</h1>
      <p>{product.description}</p>
      <p>${product.price}</p>
    </div>
  );
}

// Post Details with multiple parameters
function PostDetails() {
  const { userId, postId } = useParams<{
    userId: string;
    postId: string;
  }>();
  
  return (
    <div>
      <p>User ID: {userId}</p>
      <p>Post ID: {postId}</p>
    </div>
  );
}

// Category Page
function CategoryPage() {
  const { categoryName } = useParams<{ categoryName: string }>();
  const [products, setProducts] = useState([]);
  
  useEffect(() => {
    const fetchCategoryProducts = async () => {
      const data = await productService.getProductsByCategory(categoryName);
      setProducts(data);
    };
    
    fetchCategoryProducts();
  }, [categoryName]);
  
  return (
    <div>
      <h1>{categoryName}</h1>
      <ProductList products={products} />
    </div>
  );
}

// Optional parameters
function SearchPage() {
  const { query } = useParams<{ query?: string }>();
  
  // query might be undefined if not provided
  if (!query) {
    return <div>Please enter a search query</div>;
  }
  
  return <SearchResults query={query} />;
}
```

**useParams with TypeScript:**
```typescript
// Define parameter types
interface ProductParams {
  id: string;
}

function ProductDetails() {
  const { id } = useParams<ProductParams>();
  // id is typed as string
}
```

**useParams Best Practices:**
- Type parameters with TypeScript
- Handle undefined for optional params
- Fetch data based on params
- Update when params change
- Validate params before use

---

### 11. useLocation

**What is it?**
useLocation is a React Router hook that returns the location object that represents the current URL, similar to window.location but with additional React Router-specific information.

**Why we need it?**
- Access current URL
- Get query parameters
- Track URL changes
- Implement URL-based features
- Access navigation state

**How it works?**
- Returns location object
- Contains URL information
- Updates on route change
- Includes search params
- Includes navigation state

**Syntax:**
```typescript
const location = useLocation();
```

**Simple Example:**
```typescript
function CurrentPath() {
  const location = useLocation();
  
  return <div>Current path: {location.pathname}</div>;
}
```

**Real-world Example:**
```typescript
// Get current path
function Breadcrumb() {
  const location = useLocation();
  const pathnames = location.pathname.split('/').filter(x => x);
  
  return (
    <nav className="breadcrumb">
      <Link to="/">Home</Link>
      {pathnames.map((name, index) => {
        const routeTo = `/${pathnames.slice(0, index + 1).join('/')}`;
        const isLast = index === pathnames.length - 1;
        
        return (
          <span key={name}>
            {' / '}
            {isLast ? (
              <span>{name}</span>
            ) : (
              <Link to={routeTo}>{name}</Link>
            )}
          </span>
        );
      })}
    </nav>
  );
}

// Get query parameters
function SearchResults() {
  const location = useLocation();
  const searchParams = new URLSearchParams(location.search);
  const query = searchParams.get('q');
  const page = searchParams.get('page') || '1';
  
  useEffect(() => {
    fetchSearchResults(query, page);
  }, [query, page]);
  
  return <div>{/* Results */}</div>;
}

// Track URL changes for analytics
function PageTracker() {
  const location = useLocation();
  
  useEffect(() => {
    // Track page view
    analytics.track('page_view', {
      path: location.pathname,
      search: location.search,
      hash: location.hash
    });
  }, [location]);
  
  return null;
}

// Access navigation state
function OrderConfirmation() {
  const location = useLocation();
  const state = location.state as { orderId?: string };
  const orderId = state?.orderId;
  
  return (
    <div>
      <h1>Order Confirmed!</h1>
      <p>Order ID: {orderId}</p>
    </div>
  );
}

// Combine with useNavigate
function RedirectBasedOnRole() {
  const location = useLocation();
  const navigate = useNavigate();
  const { user } = useAuth();
  
  useEffect(() => {
    if (user.role === 'admin' && location.pathname === '/dashboard') {
      navigate('/admin/dashboard');
    }
  }, [user, location, navigate]);
  
  return <Dashboard />;
}

// URL-based tabs
function Tabs() {
  const location = useLocation();
  const activeTab = location.hash || '#overview';
  
  return (
    <div>
      <nav>
        <Link to="#overview">Overview</Link>
        <Link to="#details">Details</Link>
        <Link to="#reviews">Reviews</Link>
      </nav>
      
      {activeTab === '#overview' && <Overview />}
      {activeTab === '#details' && <Details />}
      {activeTab === '#reviews' && <Reviews />}
    </div>
  );
}
```

**Location Object Properties:**
- `pathname`: URL path
- `search`: Query string
- `hash`: URL hash
- `state`: Navigation state
- `key`: Unique key for location

**useLocation Best Practices:**
- Use for URL-based logic
- Parse query parameters
- Track location changes
- Access navigation state
- Avoid excessive re-renders

---

## Part 3: Advanced Routing

### 12. Dynamic Routes

**What is it?**
Dynamic routes are routes that include variable segments in their path, allowing you to match multiple URLs with a single route definition and extract the variable values as parameters.

**Why we need it?**
- Handle resource IDs
- Create reusable components
- Build flexible URLs
- Support dynamic content
- Reduce route duplication

**How it works?**
- Use `:parameter` syntax
- Router extracts parameter value
- Available via useParams
- Matches any value for parameter
- Supports multiple parameters

**Syntax:**
```typescript
<Route path="/users/:id" element={<UserDetails />} />
```

**Simple Example:**
```typescript
<Routes>
  <Route path="/users/:id" element={<UserDetails />} />
</Routes>

// /users/1 → UserDetails with id="1"
// /users/123 → UserDetails with id="123"
// /users/abc → UserDetails with id="abc"
```

**Real-world Example:**
```typescript
function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Single dynamic parameter */}
        <Route path="/products/:id" element={<ProductDetails />} />
        
        {/* Multiple dynamic parameters */}
        <Route path="/users/:userId/posts/:postId" element={<PostDetails />} />
        
        {/* Dynamic segment with optional */}
        <Route path="/category/:category?" element={<CategoryPage />} />
        
        {/* Multiple dynamic segments */}
        <Route path="/blog/:year/:month/:slug" element={<BlogPost />} />
        
        {/* Mixed static and dynamic */}
        <Route path="/admin/users/:userId/edit" element={<EditUser />} />
      </Routes>
    </BrowserRouter>
  );
}

// Product Details
function ProductDetails() {
  const { id } = useParams<{ id: string }>();
  
  useEffect(() => {
    // Fetch product with ID
    fetchProduct(id);
  }, [id]);
  
  return <div>Product {id}</div>;
}

// Blog Post
function BlogPost() {
  const { year, month, slug } = useParams<{
    year: string;
    month: string;
    slug: string;
  }>();
  
  return (
    <article>
      <h1>{slug}</h1>
      <p>Published: {month}/{year}</p>
    </article>
  );
}

// Category with optional
function CategoryPage() {
  const { category } = useParams<{ category?: string }>();
  
  if (!category) {
    return <AllCategories />;
  }
  
  return <CategoryProducts categoryName={category} />;
}
```

**Dynamic Route Patterns:**

1. **Single parameter:**
```typescript
<Route path="/users/:id" element={<User />} />
```

2. **Multiple parameters:**
```typescript
<Route path="/users/:userId/posts/:postId" element={<Post />} />
```

3. **Optional parameter:**
```typescript
<Route path="/search/:query?" element={<Search />} />
```

4. **Wildcard parameter:**
```typescript
<Route path="/files/*" element={<FileBrowser />} />
```

5. **Multiple wildcards:**
```typescript
<Route path="/files/*/:filename" element={<FileViewer />} />
```

**Dynamic Route Best Practices:**
- Use descriptive parameter names
- Validate parameter values
- Handle missing parameters
- Use TypeScript for type safety
- Consider SEO implications

---

### 13. Nested Routes

**What is it?**
Nested routes allow you to define routes within other routes, creating a hierarchy where child routes render within a parent component's Outlet. This is useful for layouts and shared UI.

**Why we need it?**
- Shared layouts
- Consistent UI structure
- Hierarchical navigation
- Code organization
- Reduced duplication

**How it works?**
- Define child routes in parent
- Parent renders Outlet component
- Child routes render in Outlet
- URL reflects full path
- Parent stays mounted

**Syntax:**
```typescript
<Route path="/dashboard" element={<DashboardLayout />}>
  <Route index element={<DashboardHome />} />
  <Route path="profile" element={<Profile />} />
</Route>
```

**Simple Example:**
```typescript
function DashboardLayout() {
  return (
    <div className="dashboard">
      <Sidebar />
      <main>
        <Outlet />
      </main>
    </div>
  );
}

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/dashboard" element={<DashboardLayout />}>
          <Route index element={<DashboardHome />} />
          <Route path="profile" element={<Profile />} />
          <Route path="settings" element={<Settings />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}
```

**Real-world Example:**
```typescript
// Layout with nested routes
function AdminLayout() {
  return (
    <div className="admin-layout">
      <Header />
      <div className="admin-content">
        <Sidebar />
        <main className="main-content">
          <Outlet />
        </main>
      </div>
      <Footer />
    </div>
  );
}

function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Public routes */}
        <Route path="/" element={<Landing />} />
        <Route path="/login" element={<Login />} />
        
        {/* Admin nested routes */}
        <Route path="/admin" element={<AdminLayout />}>
          <Route index element={<AdminDashboard />} />
          <Route path="users" element={<Users />} />
          <Route path="users/:id" element={<UserDetails />} />
          <Route path="products" element={<Products />} />
          <Route path="products/:id" element={<ProductDetails />} />
          <Route path="orders" element={<Orders />} />
          <Route path="settings" element={<Settings />} />
        </Route>
        
        {/* 404 */}
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}

// Multi-level nesting
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<MainLayout />}>
          <Route index element={<Home />} />
          <Route path="shop" element={<ShopLayout />}>
            <Route index element={<ShopHome />} />
            <Route path="products" element={<Products />} />
            <Route path="products/:id" element={<ProductDetails />} />
            <Route path="cart" element={<Cart />} />
          </Route>
          <Route path="account" element={<AccountLayout />}>
            <Route index element={<AccountOverview />} />
            <Route path="orders" element={<Orders />} />
            <Route path="orders/:id" element={<OrderDetails />} />
            <Route path="settings" element={<AccountSettings />} />
          </Route>
        </Route>
      </Routes>
    </BrowserRouter>
  );
}
```

**Index Routes:**
```typescript
<Route path="/dashboard" element={<DashboardLayout />}>
  <Route index element={<DashboardHome />} />
  <Route path="profile" element={<Profile />} />
</Route>

// /dashboard → renders DashboardHome
// /dashboard/profile → renders Profile
```

**Nested Route Best Practices:**
- Use Outlet in parent component
- Define index routes for defaults
- Keep routes logically organized
- Share common UI in layouts
- Consider route depth complexity

---

### 14. Layouts

**What is it?**
Layouts are components that provide a consistent structure and shared UI (like navigation, sidebar, footer) across multiple pages, typically implemented using nested routes and the Outlet component.

**Why we need it?**
- Consistent user interface
- Shared navigation elements
- Reduced code duplication
- Better organization
- Easier maintenance

**How it works?**
- Define layout component
- Include shared UI elements
- Render Outlet for child content
- Wrap routes with layout
- Child routes render in Outlet

**Simple Example:**
```typescript
function MainLayout() {
  return (
    <div className="layout">
      <Navbar />
      <main>
        <Outlet />
      </main>
      <Footer />
    </div>
  );
}

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<MainLayout />}>
          <Route index element={<Home />} />
          <Route path="about" element={<About />} />
          <Route path="contact" element={<Contact />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}
```

**Real-world Example:**
```typescript
// Public Layout
function PublicLayout() {
  return (
    <div className="public-layout">
      <PublicNavbar />
      <main className="content">
        <Outlet />
      </main>
      <PublicFooter />
    </div>
  );
}

// Authenticated Layout
function AuthLayout() {
  return (
    <div className="auth-layout">
      <Sidebar />
      <Header />
      <main className="main-content">
        <Outlet />
      </main>
    </div>
  );
}

// Admin Layout
function AdminLayout() {
  return (
    <div className="admin-layout">
      <AdminHeader />
      <div className="admin-body">
        <AdminSidebar />
        <main className="admin-main">
          <Outlet />
        </main>
      </div>
      <AdminFooter />
    </div>
  );
}

// App with multiple layouts
function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Public routes with public layout */}
        <Route path="/" element={<PublicLayout />}>
          <Route index element={<Landing />} />
          <Route path="about" element={<About />} />
          <Route path="contact" element={<Contact />} />
          <Route path="login" element={<Login />} />
          <Route path="register" element={<Register />} />
        </Route>
        
        {/* Authenticated routes with auth layout */}
        <Route path="/app" element={<AuthLayout />}>
          <Route index element={<Dashboard />} />
          <Route path="profile" element={<Profile />} />
          <Route path="settings" element={<Settings />} />
        </Route>
        
        {/* Admin routes with admin layout */}
        <Route path="/admin" element={<AdminLayout />}>
          <Route index element={<AdminDashboard />} />
          <Route path="users" element={<Users />} />
          <Route path="products" element={<Products />} />
          <Route path="orders" element={<Orders />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

// Conditional layout based on auth
function App() {
  const { user } = useAuth();
  
  return (
    <BrowserRouter>
      <Routes>
        {user ? (
          <Route path="/" element={<AuthLayout />}>
            <Route index element={<Dashboard />} />
            <Route path="profile" element={<Profile />} />
          </Route>
        ) : (
          <Route path="/" element={<PublicLayout />}>
            <Route index element={<Landing />} />
            <Route path="login" element={<Login />} />
          </Route>
        )}
      </Routes>
    </BrowserRouter>
  );
}
```

**Layout Patterns:**

1. **Single layout:**
```typescript
<Route path="/" element={<Layout />}>
  <Route index element={<Home />} />
  <Route path="about" element={<About />} />
</Route>
```

2. **Multiple layouts:**
```typescript
<Route path="/public" element={<PublicLayout />}>
  <Route index element={<Home />} />
</Route>
<Route path="/private" element={<PrivateLayout />}>
  <Route index element={<Dashboard />} />
</Route>
```

3. **Nested layouts:**
```typescript
<Route path="/" element={<MainLayout />}>
  <Route path="admin" element={<AdminLayout />}>
    <Route index element={<AdminDashboard />} />
  </Route>
</Route>
```

**Layout Best Practices:**
- Keep layouts focused on structure
- Use Outlet for child content
- Separate concerns (layout vs content)
- Reuse layouts where possible
- Consider layout complexity

---

### 15. Outlet

**What is it?**
Outlet is a component used in parent route components to render their child routes. It acts as a placeholder where the matched child route's element will be rendered.

**Why we need it?**
- Render child routes in parent
- Enable nested routing
- Maintain parent context
- Share layout components
- Control child rendering

**How it works?**
- Placed in parent component
- Renders matched child route
- Passes context to children
- Updates on route change
- Supports multiple outlets

**Syntax:**
```typescript
import { Outlet } from 'react-router-dom';

function ParentLayout() {
  return (
    <div>
      <Navbar />
      <Outlet />
      <Footer />
    </div>
  );
}
```

**Simple Example:**
```typescript
function DashboardLayout() {
  return (
    <div className="dashboard">
      <Sidebar />
      <main>
        <Outlet />
      </main>
    </div>
  );
}

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/dashboard" element={<DashboardLayout />}>
          <Route index element={<DashboardHome />} />
          <Route path="profile" element={<Profile />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}
```

**Real-world Example:**
```typescript
// Layout with Outlet
function AdminLayout() {
  const location = useLocation();
  
  return (
    <div className="admin-layout">
      <Header />
      <div className="admin-body">
        <Sidebar currentPath={location.pathname} />
        <main className="admin-main">
          <Outlet />
        </main>
      </div>
      <Footer />
    </div>
  );
}

// Context provider with Outlet
function AuthLayout() {
  return (
    <AuthProvider>
      <Navbar />
      <main>
        <Outlet />
      </main>
      <Footer />
    </AuthProvider>
  );
}

// Conditional Outlet
function ConditionalLayout() {
  const { user } = useAuth();
  
  if (!user) {
    return <Navigate to="/login" />;
  }
  
  return (
    <div className="layout">
      <Sidebar />
      <Outlet />
    </div>
  );
}

// Multiple Outlet locations
function ThreeColumnLayout() {
  return (
    <div className="three-column">
      <aside className="left-sidebar">
        <Outlet name="left" />
      </aside>
      <main className="main-content">
        <Outlet />
      </main>
      <aside className="right-sidebar">
        <Outlet name="right" />
      </aside>
    </div>
  );
}

// App with nested outlets
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<MainLayout />}>
          <Route index element={<Home />} />
          <Route path="shop" element={<ShopLayout />}>
            <Route index element={<ShopHome />} />
            <Route path="products" element={<Products />} />
          </Route>
        </Route>
      </Routes>
    </BrowserRouter>
  );
}
```

**Outlet Best Practices:**
- Always include Outlet in parent
- Place Outlet where content should render
- Can pass context via parent
- Use index routes for defaults
- Consider outlet placement carefully

---

## Part 4: Route Protection

### 16. 404 Page

**What is it?**
A 404 page (Not Found) is displayed when a user navigates to a URL that doesn't match any defined route in your application. It provides a better user experience than a blank page.

**Why we need it?**
- Handle unknown routes gracefully
- Provide helpful navigation
- Improve user experience
- Maintain brand consistency
- Guide users back to valid content

**How it works?**
- Define route with `*` path
- Matches any unmatched URL
- Renders 404 component
- Should be last in routes list
- Provides navigation options

**Syntax:**
```typescript
<Route path="*" element={<NotFound />} />
```

**Simple Example:**
```typescript
function NotFound() {
  return (
    <div className="not-found">
      <h1>404 - Page Not Found</h1>
      <p>The page you're looking for doesn't exist.</p>
      <Link to="/">Go Home</Link>
    </div>
  );
}

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}
```

**Real-world Example:**
```typescript
function NotFound() {
  const navigate = useNavigate();
  
  return (
    <div className="not-found-page">
      <div className="not-found-content">
        <h1 className="error-code">404</h1>
        <h2 className="error-title">Page Not Found</h2>
        <p className="error-message">
          The page you're looking for doesn't exist or has been moved.
        </p>
        
        <div className="not-found-actions">
          <button onClick={() => navigate(-1)} className="btn-secondary">
            Go Back
          </button>
          <Link to="/" className="btn-primary">
            Go Home
          </Link>
        </div>
        
        <div className="helpful-links">
          <h3>Helpful Links</h3>
          <ul>
            <li><Link to="/products">Products</Link></li>
            <li><Link to="/about">About Us</Link></li>
            <li><Link to="/contact">Contact</Link></li>
          </ul>
        </div>
      </div>
    </div>
  );
}

// With suggestions
function NotFound() {
  const location = useLocation();
  const pathname = location.pathname;
  
  // Try to suggest correct route
  const suggestedRoute = getSuggestedRoute(pathname);
  
  return (
    <div className="not-found">
      <h1>404</h1>
      <p>Page not found: {pathname}</p>
      
      {suggestedRoute && (
        <p>
          Did you mean to go to{' '}
          <Link to={suggestedRoute}>{suggestedRoute}</Link>?
        </p>
      )}
      
      <Link to="/">Go Home</Link>
    </div>
  );
}

// 404 in nested routes
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<MainLayout />}>
          <Route index element={<Home />} />
          <Route path="about" element={<About />} />
          <Route path="*" element={<NotFound />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

// 404 with search
function NotFound() {
  const [searchQuery, setSearchQuery] = useState('');
  const navigate = useNavigate();
  
  const handleSearch = (e) => {
    e.preventDefault();
    navigate(`/search?q=${encodeURIComponent(searchQuery)}`);
  };
  
  return (
    <div className="not-found">
      <h1>404</h1>
      <p>Page not found</p>
      
      <form onSubmit={handleSearch}>
        <input
          value={searchQuery}
          onChange={(e) => setSearchQuery(e.target.value)}
          placeholder="Search our site"
        />
        <button type="submit">Search</button>
      </form>
      
      <Link to="/">Go Home</Link>
    </div>
  );
}
```

**404 Page Best Practices:**
- Place 404 route last
- Provide navigation options
- Maintain brand consistency
- Add helpful suggestions
- Keep it user-friendly

---

### 17. Protected Routes

**What is it?**
Protected routes are routes that require authentication or specific permissions to access. If a user tries to access a protected route without the required credentials, they are redirected to a login page or shown an error.

**Why we need it?**
- Secure sensitive pages
- Implement access control
- Redirect unauthorized users
- Protect admin areas
- Enforce user permissions

**How it works?**
- Wrap route in protection component
- Check authentication status
- Redirect if not authenticated
- Render content if authenticated
- Can check permissions too

**Syntax:**
```typescript
<Route
  path="/dashboard"
  element={
    <ProtectedRoute>
      <Dashboard />
    </ProtectedRoute>
  }
/>
```

**Simple Example:**
```typescript
function ProtectedRoute({ children }) {
  const { user } = useAuth();
  
  if (!user) {
    return <Navigate to="/login" />;
  }
  
  return children;
}

function App() {
  return (
    <BrowserRouter>
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
    </BrowserRouter>
  );
}
```

**Real-world Example:**
```typescript
interface ProtectedRouteProps {
  children: React.ReactNode;
  requiredRole?: string;
}

function ProtectedRoute({ children, requiredRole }: ProtectedRouteProps) {
  const { user, loading } = useAuth();
  const location = useLocation();
  
  if (loading) {
    return <LoadingSpinner />;
  }
  
  if (!user) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  
  if (requiredRole && user.role !== requiredRole) {
    return <Navigate to="/unauthorized" replace />;
  }
  
  return <>{children}</>;
}

// Usage
function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Public routes */}
        <Route path="/" element={<Landing />} />
        <Route path="/login" element={<Login />} />
        <Route path="/register" element={<Register />} />
        
        {/* Protected routes */}
        <Route
          path="/dashboard"
          element={
            <ProtectedRoute>
              <Dashboard />
            </ProtectedRoute>
          }
        />
        
        {/* Role-protected routes */}
        <Route
          path="/admin"
          element={
            <ProtectedRoute requiredRole="admin">
              <AdminPanel />
            </ProtectedRoute>
          }
        />
        
        <Route path="/unauthorized" element={<Unauthorized />} />
      </Routes>
    </BrowserRouter>
  );
}

// With redirect back after login
function Login() {
  const location = useLocation();
  const navigate = useNavigate();
  const from = location.state?.from?.pathname || '/dashboard';
  
  const handleLogin = async () => {
    await login(credentials);
    navigate(from, { replace: true });
  };
  
  return <form onSubmit={handleLogin}>{/* ... */}</form>;
}

// Protected route with multiple roles
function ProtectedRoute({ children, allowedRoles = [] }) {
  const { user } = useAuth();
  
  if (!user) {
    return <Navigate to="/login" />;
  }
  
  if (allowedRoles.length > 0 && !allowedRoles.includes(user.role)) {
    return <Navigate to="/unauthorized" />;
  }
  
  return <>{children}</>;
}

// Usage
<Route
  path="/reports"
  element={
    <ProtectedRoute allowedRoles={['admin', 'manager']}>
      <Reports />
    </ProtectedRoute>
  }
/>
```

**Protected Route Best Practices:**
- Check authentication first
- Handle loading state
- Redirect with state
- Check permissions
- Provide fallback UI

---

### 18. Authentication Guard

**What is it?**
An authentication guard is a component or mechanism that checks if a user is authenticated before allowing access to certain routes or features, redirecting unauthenticated users to a login page.

**Why we need it?**
- Protect application routes
- Enforce authentication
- Redirect to login
- Remember intended destination
- Secure user data

**How it works?**
- Intercept route access
- Check authentication status
- Redirect if not authenticated
- Allow access if authenticated
- Store intended destination

**Simple Example:**
```typescript
function AuthGuard({ children }) {
  const { isAuthenticated } = useAuth();
  
  if (!isAuthenticated) {
    return <Navigate to="/login" />;
  }
  
  return children;
}
```

**Real-world Example:**
```typescript
// Auth guard with redirect
function AuthGuard({ children }) {
  const { user, loading } = useAuth();
  const location = useLocation();
  
  if (loading) {
    return <LoadingSpinner />;
  }
  
  if (!user) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  
  return <>{children}</>;
}

// Auth guard for entire app
function App() {
  const { user, loading } = useAuth();
  
  if (loading) {
    return <LoadingSpinner />;
  }
  
  return (
    <BrowserRouter>
      <Routes>
        {!user ? (
          <>
            <Route path="/login" element={<Login />} />
            <Route path="*" element={<Navigate to="/login" replace />} />
          </>
        ) : (
          <>
            <Route path="/" element={<Dashboard />} />
            <Route path="/profile" element={<Profile />} />
            <Route path="*" element={<Navigate to="/" replace />} />
          </>
        )}
      </Routes>
    </BrowserRouter>
  );
}

// Auth context
const AuthContext = createContext<AuthContextType | null>(null);

function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    checkAuth();
  }, []);
  
  const checkAuth = async () => {
    try {
      const token = localStorage.getItem('token');
      if (token) {
        const userData = await validateToken(token);
        setUser(userData);
      }
    } catch (error) {
      localStorage.removeItem('token');
    } finally {
      setLoading(false);
    }
  };
  
  const login = async (credentials) => {
    const { token, user } = await api.login(credentials);
    localStorage.setItem('token', token);
    setUser(user);
  };
  
  const logout = () => {
    localStorage.removeItem('token');
    setUser(null);
  };
  
  return (
    <AuthContext.Provider value={{ user, loading, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
}

// Route-level auth guard
function PrivateRoute({ children, ...rest }) {
  const { user } = useAuth();
  
  return (
    <Route
      {...rest}
      element={
        user ? children : <Navigate to="/login" state={{ from: location }} />
      }
    />
  );
}
```

**Authentication Guard Best Practices:**
- Handle loading state
- Store intended destination
- Clear auth on logout
- Validate tokens
- Provide feedback

---

### 19. Route-based Authorization

**What is it?**
Route-based authorization is the practice of controlling access to specific routes based on user roles, permissions, or other criteria beyond just authentication.

**Why we need it?**
- Control access by role
- Implement permissions
- Protect admin features
- Multi-level access control
- Security enforcement

**How it works?**
- Check user role/permissions
- Compare against route requirements
- Allow or deny access
- Redirect if unauthorized
- Show error if needed

**Simple Example:**
```typescript
function RoleRoute({ children, allowedRoles }) {
  const { user } = useAuth();
  
  if (!allowedRoles.includes(user.role)) {
    return <Navigate to="/unauthorized" />;
  }
  
  return children;
}
```

**Real-world Example:**
```typescript
interface RoleRouteProps {
  children: React.ReactNode;
  allowedRoles: string[];
}

function RoleRoute({ children, allowedRoles }: RoleRouteProps) {
  const { user, loading } = useAuth();
  
  if (loading) {
    return <LoadingSpinner />;
  }
  
  if (!user) {
    return <Navigate to="/login" />;
  }
  
  if (!allowedRoles.includes(user.role)) {
    return <Navigate to="/unauthorized" />;
  }
  
  return <>{children}</>;
}

// Permission-based authorization
function PermissionRoute({ children, requiredPermission }) {
  const { user } = useAuth();
  
  if (!user.permissions.includes(requiredPermission)) {
    return <Navigate to="/unauthorized" />;
  }
  
  return <>{children}</>;
}

// Combined auth and role check
function ProtectedRoute({ 
  children, 
  requireAuth = true,
  allowedRoles = [],
  allowedPermissions = []
}) {
  const { user, loading } = useAuth();
  
  if (loading) {
    return <LoadingSpinner />;
  }
  
  if (requireAuth && !user) {
    return <Navigate to="/login" />;
  }
  
  if (allowedRoles.length > 0 && !allowedRoles.includes(user?.role)) {
    return <Navigate to="/unauthorized" />;
  }
  
  if (allowedPermissions.length > 0) {
    const hasPermission = allowedPermissions.every(perm =>
      user?.permissions.includes(perm)
    );
    if (!hasPermission) {
      return <Navigate to="/unauthorized" />;
    }
  }
  
  return <>{children}</>;
}

// Usage
function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Public */}
        <Route path="/" element={<Home />} />
        
        {/* Authenticated */}
        <Route
          path="/dashboard"
          element={
            <ProtectedRoute requireAuth>
              <Dashboard />
            </ProtectedRoute>
          }
        />
        
        {/* Admin only */}
        <Route
          path="/admin"
          element={
            <ProtectedRoute requireAuth allowedRoles={['admin']}>
              <AdminPanel />
            </ProtectedRoute>
          }
        />
        
        {/* Specific permission */}
        <Route
          path="/reports"
          element={
            <ProtectedRoute requireAuth allowedPermissions={['view_reports']}>
              <Reports />
            </ProtectedRoute>
          }
        />
        
        {/* Multiple roles */}
        <Route
          path="/settings"
          element={
            <ProtectedRoute requireAuth allowedRoles={['admin', 'manager']}>
              <Settings />
            </ProtectedRoute>
          }
        />
      </Routes>
    </BrowserRouter>
  );
}

// Authorization hook
function useAuthorization(requiredPermission: string) {
  const { user } = useAuth();
  
  const hasPermission = user?.permissions.includes(requiredPermission);
  
  return { hasPermission, user };
}

// Component-level authorization
function DeleteButton() {
  const { hasPermission } = useAuthorization('delete_posts');
  
  if (!hasPermission) {
    return null;
  }
  
  return <button>Delete</button>;
}
```

**Authorization Best Practices:**
- Separate auth from authorization
- Use role-based access control
- Implement permission checks
- Provide feedback for unauthorized access
- Keep authorization logic centralized

---

## Part 5: Project Architecture

### 20. Project Architecture

**What is it?**
Project architecture refers to the overall structure and organization of your React application, including how files and folders are arranged, how concerns are separated, and how different parts of the application interact.

**Why we need it?**
- Scalable codebase
- Maintainable structure
- Easy onboarding
- Clear separation of concerns
- Better collaboration

**Professional Structure:**

```
src/
├── components/          # Reusable UI components
│   ├── common/         # Shared components (Button, Input, etc.)
│   ├── layout/         # Layout components (Header, Sidebar, etc.)
│   └── features/       # Feature-specific components
├── pages/              # Page-level components
│   ├── Home/
│   ├── About/
│   ├── Dashboard/
│   └── Products/
├── layouts/            # Layout wrappers
│   ├── MainLayout.tsx
│   ├── AuthLayout.tsx
│   └── AdminLayout.tsx
├── routes/             # Route configurations
│   ├── publicRoutes.tsx
│   ├── protectedRoutes.tsx
│   └── adminRoutes.tsx
├── hooks/              # Custom React hooks
│   ├── useAuth.ts
│   ├── useProducts.ts
│   └── useUsers.ts
├── services/           # API services
│   ├── api.ts
│   ├── authService.ts
│   └── productService.ts
├── stores/             # State management
│   ├── authStore.ts
│   ├── cartStore.ts
│   └── userStore.ts
├── types/              # TypeScript types
│   ├── api.types.ts
│   ├── auth.types.ts
│   └── product.types.ts
├── utils/              # Utility functions
│   ├── formatters.ts
│   ├── validators.ts
│   └── helpers.ts
├── constants/          # Constants
│   ├── routes.ts
│   └── api.ts
├── assets/             # Static assets
│   ├── images/
│   ├── fonts/
│   └── styles/
├── App.tsx             # Root component
└── main.tsx            # Entry point
```

**Folder Explanations:**

**components/**: Reusable UI components
- **common/**: Shared components (Button, Input, Modal, etc.)
- **layout/**: Layout components (Header, Footer, Sidebar)
- **features/**: Feature-specific components (ProductCard, UserCard)

**pages/**: Page-level components
- Each page in its own folder
- Contains page-specific logic
- May use multiple components

**layouts/**: Layout wrappers
- Provides consistent structure
- Includes navigation, headers, footers
- Uses Outlet for child content

**routes/**: Route configurations
- Centralized route definitions
- Grouped by access level
- Easy to manage and modify

**hooks/**: Custom React hooks
- Reusable stateful logic
- Encapsulates complex behavior
- Can be shared across components

**services/**: API services
- Handles all API calls
- Separates API logic
- Type-safe with TypeScript

**stores/**: State management
- Global application state
- Could be Redux, Zustand, Context
- Manages shared state

**types/**: TypeScript types
- Interface definitions
- Type aliases
- Shared across application

**utils/**: Utility functions
- Helper functions
- Pure functions
- No side effects

**constants/**: Constants
- Route paths
- API endpoints
- Configuration values

**Real-world Example:**

```typescript
// routes/index.tsx
import { Routes, Route } from 'react-router-dom';
import { MainLayout } from '../layouts/MainLayout';
import { AuthLayout } from '../layouts/AuthLayout';
import { AdminLayout } from '../layouts/AdminLayout';
import { ProtectedRoute } from '../components/ProtectedRoute';

// Pages
import { Home } from '../pages/Home';
import { Login } from '../pages/Login';
import { Dashboard } from '../pages/Dashboard';
import { Products } from '../pages/Products';
import { ProductDetails } from '../pages/ProductDetails';
import { AdminPanel } from '../pages/AdminPanel';

export function AppRoutes() {
  return (
    <Routes>
      {/* Public routes */}
      <Route path="/" element={<MainLayout />}>
        <Route index element={<Home />} />
      </Route>
      
      {/* Auth routes */}
      <Route path="/auth" element={<AuthLayout />}>
        <Route path="login" element={<Login />} />
        <Route path="register" element={<Register />} />
      </Route>
      
      {/* Protected routes */}
      <Route path="/app" element={<ProtectedRoute><AuthLayout /></ProtectedRoute>}>
        <Route index element={<Dashboard />} />
        <Route path="products" element={<Products />} />
        <Route path="products/:id" element={<ProductDetails />} />
      </Route>
      
      {/* Admin routes */}
      <Route path="/admin" element={<ProtectedRoute requiredRole="admin"><AdminLayout /></ProtectedRoute>}>
        <Route index element={<AdminPanel />} />
      </Route>
      
      {/* 404 */}
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
}
```

**Architecture Best Practices:**
- Separate concerns clearly
- Keep folder structure consistent
- Use barrel exports (index.ts)
- Group related files
- Document structure decisions

---

## Practical Project: Admin Dashboard

Let's build a complete Admin Dashboard application that demonstrates all the routing concepts we've learned.

### Project Overview

**Features:**
- Login page
- Dashboard with sidebar navigation
- Products management (list, details)
- Orders management
- User profile
- Settings
- Protected routes
- Dynamic routes
- Nested routes
- Layouts with Outlet

### Project Structure

```
src/
├── components/
│   ├── common/
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   └── Card.tsx
│   ├── layout/
│   │   ├── Header.tsx
│   │   ├── Sidebar.tsx
│   │   └── Footer.tsx
│   └── auth/
│       └── ProtectedRoute.tsx
├── pages/
│   ├── Login.tsx
│   ├── Dashboard/
│   │   ├── index.tsx
│   │   ├── Profile.tsx
│   │   ├── Orders.tsx
│   │   └── Settings.tsx
│   └── Products/
│       ├── index.tsx
│       └── ProductDetails.tsx
├── layouts/
│   ├── MainLayout.tsx
│   ├── AuthLayout.tsx
│   └── AdminLayout.tsx
├── hooks/
│   ├── useAuth.ts
│   └── useProducts.ts
├── services/
│   ├── api.ts
│   └── authService.ts
├── types/
│   └── index.ts
├── utils/
│   └── index.ts
├── App.tsx
└── main.tsx
```

### Step 1: Project Setup

```bash
# Create new project
npm create vite@latest admin-dashboard -- --template react-ts

# Navigate to project
cd admin-dashboard

# Install dependencies
npm install react-router-dom
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# Start development server
npm run dev
```

### Step 2: Configure Tailwind CSS

Add to `tailwind.config.js`:
```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
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
```

### Step 3: Define Types

Create `src/types/index.ts`:
```typescript
export interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'user';
}

export interface Product {
  id: string;
  name: string;
  description: string;
  price: number;
  category: string;
}

export interface Order {
  id: string;
  userId: string;
  status: 'pending' | 'completed' | 'cancelled';
  total: number;
  createdAt: string;
}
```

### Step 4: Create Auth Context and Hook

Create `src/hooks/useAuth.ts`:
```typescript
import { createContext, useContext, useState, useEffect, ReactNode } from 'react';
import { User } from '../types';

interface AuthContextType {
  user: User | null;
  loading: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
  isAuthenticated: boolean;
}

const AuthContext = createContext<AuthContextType | null>(null);

export function AuthProvider({ children }: { children: ReactNode }) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Check for existing session
    const savedUser = localStorage.getItem('user');
    if (savedUser) {
      setUser(JSON.parse(savedUser));
    }
    setLoading(false);
  }, []);

  const login = async (email: string, password: string) => {
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1000));
    
    const mockUser: User = {
      id: '1',
      name: 'Admin User',
      email,
      role: 'admin'
    };
    
    setUser(mockUser);
    localStorage.setItem('user', JSON.stringify(mockUser));
  };

  const logout = () => {
    setUser(null);
    localStorage.removeItem('user');
  };

  return (
    <AuthContext.Provider 
      value={{ 
        user, 
        loading, 
        login, 
        logout,
        isAuthenticated: !!user 
      }}
    >
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

### Step 5: Create Protected Route Component

Create `src/components/auth/ProtectedRoute.tsx`:
```typescript
import { Navigate, useLocation } from 'react-router-dom';
import { useAuth } from '../../hooks/useAuth';

interface ProtectedRouteProps {
  children: React.ReactNode;
  requiredRole?: 'admin' | 'user';
}

export function ProtectedRoute({ children, requiredRole }: ProtectedRouteProps) {
  const { user, loading, isAuthenticated } = useAuth();
  const location = useLocation();

  if (loading) {
    return (
      <div className="flex items-center justify-center min-h-screen">
        <div className="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-600"></div>
      </div>
    );
  }

  if (!isAuthenticated) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }

  if (requiredRole && user?.role !== requiredRole) {
    return <Navigate to="/unauthorized" replace />;
  }

  return <>{children}</>;
}
```

### Step 6: Create Layout Components

Create `src/layouts/MainLayout.tsx`:
```typescript
import { Outlet, Link } from 'react-router-dom';

export function MainLayout() {
  return (
    <div className="min-h-screen bg-gray-50">
      <header className="bg-white shadow">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="flex justify-between items-center h-16">
            <Link to="/" className="text-xl font-bold text-gray-900">
              Admin Dashboard
            </Link>
            <nav className="flex space-x-4">
              <Link to="/login" className="text-gray-700 hover:text-gray-900">
                Login
              </Link>
            </nav>
          </div>
        </div>
      </header>
      <main className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        <Outlet />
      </main>
    </div>
  );
}
```

Create `src/layouts/AuthLayout.tsx`:
```typescript
import { Outlet } from 'react-router-dom';
import { useAuth } from '../hooks/useAuth';
import Header from '../components/layout/Header';
import Sidebar from '../components/layout/Sidebar';

export function AuthLayout() {
  const { user, logout } = useAuth();

  return (
    <div className="min-h-screen bg-gray-50">
      <Header user={user} onLogout={logout} />
      <div className="flex">
        <Sidebar />
        <main className="flex-1 p-8">
          <Outlet />
        </main>
      </div>
    </div>
  );
}
```

Create `src/components/layout/Header.tsx`:
```typescript
import { User } from '../../types';

interface HeaderProps {
  user: User | null;
  onLogout: () => void;
}

export function Header({ user, onLogout }: HeaderProps) {
  return (
    <header className="bg-white shadow">
      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div className="flex justify-between items-center h-16">
          <h1 className="text-xl font-bold text-gray-900">
            Admin Dashboard
          </h1>
          {user && (
            <div className="flex items-center space-x-4">
              <span className="text-gray-700">
                {user.name}
              </span>
              <button
                onClick={onLogout}
                className="px-4 py-2 bg-red-600 text-white rounded hover:bg-red-700"
              >
                Logout
              </button>
            </div>
          )}
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
    { path: '/dashboard/orders', label: 'Orders', icon: '📦' },
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

### Step 7: Create Page Components

Create `src/pages/Login.tsx`:
```typescript
import { useState } from 'react';
import { useNavigate, useLocation } from 'react-router-dom';
import { useAuth } from '../hooks/useAuth';

export function Login() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
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
      await login(email, password);
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
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              className="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
              required
            />
          </div>

          <div className="mb-6">
            <label className="block text-gray-700 mb-2">Password</label>
            <input
              type="password"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
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

        <p className="mt-4 text-center text-gray-600">
          Use any email and password to login
        </p>
      </div>
    </div>
  );
}
```

Create `src/pages/Dashboard/index.tsx`:
```typescript
export function Dashboard() {
  return (
    <div>
      <h1 className="text-3xl font-bold text-gray-900 mb-6">Dashboard</h1>
      
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
        <div className="bg-white rounded-lg shadow p-6">
          <h3 className="text-lg font-semibold text-gray-700">Total Products</h3>
          <p className="text-3xl font-bold text-blue-600 mt-2">156</p>
        </div>
        
        <div className="bg-white rounded-lg shadow p-6">
          <h3 className="text-lg font-semibold text-gray-700">Total Orders</h3>
          <p className="text-3xl font-bold text-green-600 mt-2">89</p>
        </div>
        
        <div className="bg-white rounded-lg shadow p-6">
          <h3 className="text-lg font-semibold text-gray-700">Revenue</h3>
          <p className="text-3xl font-bold text-purple-600 mt-2">$12,450</p>
        </div>
      </div>

      <div className="bg-white rounded-lg shadow p-6">
        <h2 className="text-xl font-bold text-gray-900 mb-4">Recent Activity</h2>
        <ul className="space-y-3">
          <li className="flex items-center justify-between py-2 border-b">
            <span>New order #1234</span>
            <span className="text-gray-500">2 minutes ago</span>
          </li>
          <li className="flex items-center justify-between py-2 border-b">
            <span>Product updated</span>
            <span className="text-gray-500">15 minutes ago</span>
          </li>
          <li className="flex items-center justify-between py-2">
            <span>New user registered</span>
            <span className="text-gray-500">1 hour ago</span>
          </li>
        </ul>
      </div>
    </div>
  );
}
```

Create `src/pages/Dashboard/Profile.tsx`:
```typescript
import { useAuth } from '../../hooks/useAuth';

export function Profile() {
  const { user } = useAuth();

  return (
    <div>
      <h1 className="text-3xl font-bold text-gray-900 mb-6">Profile</h1>
      
      <div className="bg-white rounded-lg shadow p-6">
        <div className="space-y-4">
          <div>
            <label className="block text-gray-700 font-semibold mb-2">Name</label>
            <p className="text-gray-900">{user?.name}</p>
          </div>
          
          <div>
            <label className="block text-gray-700 font-semibold mb-2">Email</label>
            <p className="text-gray-900">{user?.email}</p>
          </div>
          
          <div>
            <label className="block text-gray-700 font-semibold mb-2">Role</label>
            <p className="text-gray-900 capitalize">{user?.role}</p>
          </div>
        </div>
      </div>
    </div>
  );
}
```

Create `src/pages/Dashboard/Orders.tsx`:
```typescript
import { Order } from '../../types';

export function Orders() {
  const orders: Order[] = [
    { id: '1', userId: '1', status: 'pending', total: 150, createdAt: '2024-01-15' },
    { id: '2', userId: '2', status: 'completed', total: 200, createdAt: '2024-01-14' },
    { id: '3', userId: '3', status: 'cancelled', total: 75, createdAt: '2024-01-13' },
  ];

  return (
    <div>
      <h1 className="text-3xl font-bold text-gray-900 mb-6">Orders</h1>
      
      <div className="bg-white rounded-lg shadow overflow-hidden">
        <table className="min-w-full">
          <thead className="bg-gray-50">
            <tr>
              <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                Order ID
              </th>
              <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                Status
              </th>
              <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                Total
              </th>
              <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                Date
              </th>
            </tr>
          </thead>
          <tbody className="bg-white divide-y divide-gray-200">
            {orders.map((order) => (
              <tr key={order.id}>
                <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-900">
                  #{order.id}
                </td>
                <td className="px-6 py-4 whitespace-nowrap">
                  <span className={`px-2 py-1 text-xs rounded-full ${
                    order.status === 'completed'
                      ? 'bg-green-100 text-green-800'
                      : order.status === 'pending'
                      ? 'bg-yellow-100 text-yellow-800'
                      : 'bg-red-100 text-red-800'
                  }`}>
                    {order.status}
                  </span>
                </td>
                <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-900">
                  ${order.total}
                </td>
                <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                  {order.createdAt}
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    </div>
  );
}
```

Create `src/pages/Dashboard/Settings.tsx`:
```typescript
export function Settings() {
  return (
    <div>
      <h1 className="text-3xl font-bold text-gray-900 mb-6">Settings</h1>
      
      <div className="space-y-6">
        <div className="bg-white rounded-lg shadow p-6">
          <h2 className="text-xl font-bold text-gray-900 mb-4">Account Settings</h2>
          <div className="space-y-4">
            <div>
              <label className="block text-gray-700 mb-2">Email Notifications</label>
              <input type="checkbox" className="w-4 h-4" />
            </div>
            <div>
              <label className="block text-gray-700 mb-2">Two-Factor Authentication</label>
              <input type="checkbox" className="w-4 h-4" />
            </div>
          </div>
        </div>

        <div className="bg-white rounded-lg shadow p-6">
          <h2 className="text-xl font-bold text-gray-900 mb-4">Appearance</h2>
          <div className="space-y-4">
            <div>
              <label className="block text-gray-700 mb-2">Theme</label>
              <select className="w-full px-3 py-2 border rounded-lg">
                <option>Light</option>
                <option>Dark</option>
                <option>System</option>
              </select>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
```

Create `src/pages/Products/index.tsx`:
```typescript
import { Link } from 'react-router-dom';
import { Product } from '../../types';

export function Products() {
  const products: Product[] = [
    { id: '1', name: 'Product 1', description: 'Description 1', price: 99, category: 'Electronics' },
    { id: '2', name: 'Product 2', description: 'Description 2', price: 149, category: 'Clothing' },
    { id: '3', name: 'Product 3', description: 'Description 3', price: 199, category: 'Home' },
  ];

  return (
    <div>
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-3xl font-bold text-gray-900">Products</h1>
        <button className="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700">
          Add Product
        </button>
      </div>
      
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
        {products.map((product) => (
          <Link
            key={product.id}
            to={`/products/${product.id}`}
            className="bg-white rounded-lg shadow p-6 hover:shadow-lg transition-shadow"
          >
            <h3 className="text-lg font-semibold text-gray-900 mb-2">
              {product.name}
            </h3>
            <p className="text-gray-600 mb-4">{product.description}</p>
            <div className="flex justify-between items-center">
              <span className="text-2xl font-bold text-blue-600">
                ${product.price}
              </span>
              <span className="text-sm text-gray-500">{product.category}</span>
            </div>
          </Link>
        ))}
      </div>
    </div>
  );
}
```

Create `src/pages/Products/ProductDetails.tsx`:
```typescript
import { useParams, useNavigate } from 'react-router-dom';
import { Product } from '../../types';

export function ProductDetails() {
  const { id } = useParams<{ id: string }>();
  const navigate = useNavigate();

  const product: Product = {
    id: id || '1',
    name: 'Product Details',
    description: 'This is a detailed description of the product.',
    price: 99,
    category: 'Electronics'
  };

  return (
    <div>
      <button
        onClick={() => navigate('/products')}
        className="mb-6 text-blue-600 hover:text-blue-800"
      >
        ← Back to Products
      </button>
      
      <div className="bg-white rounded-lg shadow p-6">
        <h1 className="text-3xl font-bold text-gray-900 mb-4">
          {product.name}
        </h1>
        
        <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div>
            <div className="bg-gray-200 rounded-lg h-64 flex items-center justify-center">
              <span className="text-gray-500">Product Image</span>
            </div>
          </div>
          
          <div>
            <p className="text-gray-600 mb-4">{product.description}</p>
            
            <div className="space-y-4">
              <div>
                <span className="text-gray-700">Price:</span>
                <span className="text-2xl font-bold text-blue-600 ml-2">
                  ${product.price}
                </span>
              </div>
              
              <div>
                <span className="text-gray-700">Category:</span>
                <span className="ml-2">{product.category}</span>
              </div>
              
              <div>
                <span className="text-gray-700">ID:</span>
                <span className="ml-2">{product.id}</span>
              </div>
            </div>
            
            <div className="mt-6 space-x-4">
              <button className="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700">
                Edit Product
              </button>
              <button className="bg-red-600 text-white px-6 py-2 rounded-lg hover:bg-red-700">
                Delete Product
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
```

Create `src/pages/NotFound.tsx`:
```typescript
import { Link, useNavigate } from 'react-router-dom';

export function NotFound() {
  const navigate = useNavigate();

  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-50">
      <div className="text-center">
        <h1 className="text-6xl font-bold text-gray-900 mb-4">404</h1>
        <p className="text-xl text-gray-600 mb-8">Page not found</p>
        <div className="space-x-4">
          <button
            onClick={() => navigate(-1)}
            className="px-6 py-2 bg-gray-200 text-gray-700 rounded-lg hover:bg-gray-300"
          >
            Go Back
          </button>
          <Link
            to="/"
            className="px-6 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700"
          >
            Go Home
          </Link>
        </div>
      </div>
    </div>
  );
}
```

### Step 8: Create Main App Component

Create `src/App.tsx`:
```typescript
import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom';
import { AuthProvider } from './hooks/useAuth';
import { ProtectedRoute } from './components/auth/ProtectedRoute';

// Layouts
import { MainLayout } from './layouts/MainLayout';
import { AuthLayout } from './layouts/AuthLayout';

// Pages
import { Login } from './pages/Login';
import { Dashboard } from './pages/Dashboard';
import { Profile } from './pages/Dashboard/Profile';
import { Orders } from './pages/Dashboard/Orders';
import { Settings } from './pages/Dashboard/Settings';
import { Products } from './pages/Products';
import { ProductDetails } from './pages/Products/ProductDetails';
import { NotFound } from './pages/NotFound';

function App() {
  return (
    <BrowserRouter>
      <AuthProvider>
        <Routes>
          {/* Public routes */}
          <Route path="/" element={<MainLayout />}>
            <Route index element={<Navigate to="/dashboard" replace />} />
          </Route>
          
          <Route path="/login" element={<Login />} />
          
          {/* Protected routes with nested layout */}
          <Route
            path="/dashboard"
            element={
              <ProtectedRoute>
                <AuthLayout />
              </ProtectedRoute>
            }
          >
            <Route index element={<Dashboard />} />
            <Route path="profile" element={<Profile />} />
            <Route path="orders" element={<Orders />} />
            <Route path="settings" element={<Settings />} />
          </Route>
          
          {/* Product routes */}
          <Route
            path="/products"
            element={
              <ProtectedRoute>
                <AuthLayout />
              </ProtectedRoute>
            }
          >
            <Route index element={<Products />} />
            <Route path=":id" element={<ProductDetails />} />
          </Route>
          
          {/* 404 */}
          <Route path="*" element={<NotFound />} />
        </Routes>
      </AuthProvider>
    </BrowserRouter>
  );
}

export default App;
```

### Step 9: Key Concepts Demonstrated

**Routing Fundamentals:**
- BrowserRouter for routing
- Routes and Route components
- Link for navigation
- NavLink for active state

**Navigation:**
- useNavigate for programmatic navigation
- useParams for dynamic routes
- useLocation for URL information

**Advanced Routing:**
- Dynamic routes with parameters
- Nested routes with layouts
- Outlet for child route rendering
- Index routes for defaults

**Route Protection:**
- ProtectedRoute component
- Authentication checks
- Role-based authorization
- Redirect after login

**Project Structure:**
- Professional folder organization
- Separation of concerns
- Reusable components
- Centralized routing

---

## Link vs NavLink vs useNavigate

### Link

**What is it?**
Declarative navigation component, similar to HTML anchor tag but without page reload.

**When to use:**
- Standard navigation in JSX
- Navigation in navigation menus
- When you don't need active state

**Example:**
```typescript
<Link to="/about">About</Link>
```

### NavLink

**What is it?**
Special version of Link that adds styling when the route is active.

**When to use:**
- Navigation menus
- Sidebar links
- When you need active state styling

**Example:**
```typescript
<NavLink 
  to="/about" 
  className={({ isActive }) => isActive ? 'active' : ''}
>
  About
</NavLink>
```

### useNavigate

**What is it?**
Hook for programmatic navigation.

**When to use:**
- After form submission
- After authentication
- In event handlers
- Conditional navigation

**Example:**
```typescript
const navigate = useNavigate();
navigate('/dashboard');
```

**Comparison:**

| Feature | Link | NavLink | useNavigate |
|---------|------|---------|-------------|
| Type | Component | Component | Hook |
| Active State | No | Yes | N/A |
| Use Case | Navigation | Menus | Programmatic |
| State Passing | Yes | Yes | Yes |
| Best For | Links | Menus | Actions |

---

## useParams vs useLocation

### useParams

**What is it?**
Hook to access route parameters from dynamic routes.

**When to use:**
- Accessing IDs from URL
- Building dynamic pages
- Fetching data based on URL

**Example:**
```typescript
// Route: /products/:id
const { id } = useParams();
// /products/123 → id = "123"
```

### useLocation

**What is it?**
Hook to access the current location object with URL information.

**When to use:**
- Getting current path
- Accessing query parameters
- Tracking URL changes
- Accessing navigation state

**Example:**
```typescript
const location = useLocation();
console.log(location.pathname); // /products/123
console.log(location.search);   // ?page=1
console.log(location.state);    // { from: '/dashboard' }
```

**Comparison:**

| Feature | useParams | useLocation |
|---------|-----------|------------|
| Returns | Route parameters | Location object |
| Use Case | Dynamic routes | URL information |
| Properties | :paramName | pathname, search, hash, state |
| Updates | On param change | On URL change |

---

## Comprehensive Review

### Session Summary

In this session, we covered:

1. **Routing Fundamentals:** Why routing, SPA routing, React Router, BrowserRouter, Routes, Route
2. **Navigation Components:** Link, NavLink, useNavigate, useParams, useLocation
3. **Advanced Routing:** Dynamic routes, nested routes, layouts, Outlet
4. **Route Protection:** 404 pages, protected routes, authentication guards, route-based authorization
5. **Project Architecture:** Professional folder structure and organization
6. **Practical Project:** Complete Admin Dashboard with all routing concepts

### Key Takeaways

- Routing enables navigation without page reloads in SPAs
- React Router provides declarative routing with components and hooks
- Link is for navigation, NavLink for active state, useNavigate for programmatic navigation
- useParams accesses route parameters, useLocation accesses URL information
- Nested routes with Outlet enable shared layouts
- Protected routes secure sensitive pages
- Project architecture organizes code for scalability and maintainability

---

## 15 Student Questions

1. What is the difference between traditional routing and SPA routing?
2. Why do we need routing in React applications?
3. What is the difference between Link and NavLink?
4. When should you use useNavigate instead of Link?
5. How do you access route parameters in React Router?
6. What is the purpose of the Outlet component?
7. How do you create nested routes in React Router?
8. What is a protected route and why do we need it?
9. How do you implement a 404 page in React Router?
10. What is the difference between BrowserRouter and HashRouter?
11. How do you pass state through navigation?
12. What is the purpose of the index route?
13. How do you implement role-based authorization?
14. What is the benefit of using layouts in routing?
15. How do you organize routes in a large application?

---

## 5 Interview Questions

### 1. Explain the difference between Link, NavLink, and useNavigate in React Router.

**Answer:**
- **Link:** Declarative component for navigation, similar to `<a>` tag but without page reload. Use for standard navigation in JSX.
- **NavLink:** Special version of Link that adds styling attributes when the route is active. Use for navigation menus where you need to highlight the current page.
- **useNavigate:** Hook for programmatic navigation. Use when you need to navigate after an action (form submission, authentication, etc.) or in event handlers.

Example:
```typescript
// Link - standard navigation
<Link to="/about">About</Link>

// NavLink - with active state
<NavLink to="/about" className={({ isActive }) => isActive ? 'active' : ''}>
  About
</NavLink>

// useNavigate - programmatic
const navigate = useNavigate();
navigate('/about');
```

### 2. How do you implement protected routes in React Router?

**Answer:**
Protected routes wrap components with a protection mechanism that checks authentication/authorization before rendering. Create a ProtectedRoute component that checks auth status and redirects if unauthorized.

Example:
```typescript
function ProtectedRoute({ children, requiredRole }) {
  const { user, loading } = useAuth();
  const location = useLocation();

  if (loading) return <LoadingSpinner />;
  if (!user) return <Navigate to="/login" state={{ from: location }} />;
  if (requiredRole && user.role !== requiredRole) return <Navigate to="/unauthorized" />;

  return <>{children}</>;
}

// Usage
<Route
  path="/admin"
  element={
    <ProtectedRoute requiredRole="admin">
      <AdminPanel />
    </ProtectedRoute>
  }
/>
```

### 3. Explain nested routes and the Outlet component.

**Answer:**
Nested routes allow you to define routes within other routes, creating a hierarchy where child routes render within a parent component's Outlet. The Outlet component acts as a placeholder where the matched child route's element will be rendered.

Example:
```typescript
function DashboardLayout() {
  return (
    <div>
      <Sidebar />
      <main>
        <Outlet />
      </main>
    </div>
  );
}

<Routes>
  <Route path="/dashboard" element={<DashboardLayout />}>
    <Route index element={<DashboardHome />} />
    <Route path="profile" element={<Profile />} />
  </Route>
</Routes>
```

Benefits:
- Shared layout components
- Consistent UI structure
- Reduced code duplication
- Hierarchical navigation

### 4. What is the difference between useParams and useLocation?

**Answer:**
- **useParams:** Returns an object of key/value pairs of the dynamic parameters from the current URL that were matched by the Route path. Used to access route parameters like IDs.
- **useLocation:** Returns the location object representing the current URL, including pathname, search (query params), hash, and state. Used to access full URL information.

Example:
```typescript
// Route: /products/:id
const { id } = useParams(); // id = "123"

const location = useLocation();
console.log(location.pathname); // "/products/123"
console.log(location.search);   // "?page=1"
console.log(location.state);    // { from: '/dashboard' }
```

### 5. How do you organize routes in a large React application?

**Answer:**
In large applications, organize routes by grouping them logically, using separate files for different route groups, and centralizing route configuration.

Example structure:
```
src/
├── routes/
│   ├── index.tsx          # Main routes export
│   ├── publicRoutes.tsx   # Public routes
│   ├── protectedRoutes.tsx # Protected routes
│   └── adminRoutes.tsx    # Admin routes
├── layouts/
│   ├── MainLayout.tsx
│   ├── AuthLayout.tsx
│   └── AdminLayout.tsx
```

Example:
```typescript
// routes/index.tsx
export function AppRoutes() {
  return (
    <Routes>
      <Route path="/" element={<MainLayout />}>
        <Route index element={<Home />} />
      </Route>
      <Route path="/app" element={<ProtectedRoute><AuthLayout /></ProtectedRoute>}>
        <Route index element={<Dashboard />} />
      </Route>
    </Routes>
  );
}
```

Benefits:
- Clear separation by access level
- Easy to maintain
- Scalable structure
- Clear ownership

---

## Exercises

### Exercise 1: Create a Blog with Routing

Create a blog application with:
- Home page with post list
- Post details page with dynamic route
- Category page with dynamic route
- About page
- Contact page
- 404 page

**Requirements:**
- Use nested routes with layout
- Implement NavLink for navigation
- Use useParams for post details
- Create proper 404 page

### Exercise 2: Implement Search with Query Parameters

Add search functionality to the Admin Dashboard:
- Search input in header
- Query parameters in URL (?q=search)
- useLocation to read query params
- useNavigate to update query params
- Debounce search input

### Exercise 3: Create Multi-level Nested Routes

Build a documentation site with:
- Main layout
- Section layout (nested)
- Topic layout (double nested)
- Breadcrumb navigation
- Three levels of routing

### Exercise 4: Implement Role-based Access Control

Add role-based authorization to the Admin Dashboard:
- Multiple user roles (admin, editor, viewer)
- Different access levels for different routes
- Permission-based component rendering
- Unauthorized page

### Exercise 5: Create Route-based Code Splitting

Implement code splitting for routes:
- Lazy load route components
- Loading states for each route
- Error boundaries for failed loads
- Optimize bundle size

---

## Homework

### Reading Assignment
1. Read React Router documentation
2. Read about SPA routing concepts
3. Learn about authentication patterns

### Practice Exercises
1. **Refactor:** Take an existing component-based navigation and refactor to use React Router
2. **Add Routes:** Add new pages to the Admin Dashboard (Users, Analytics)
3. **Implement Search:** Add search functionality with query parameters
4. **Create Layouts:** Create different layouts for different route groups
5. **Add Protection:** Add protected routes with different permission levels

### Research Project
Research and write a brief comparison (500 words) of different routing solutions:
- React Router
- TanStack Router
- Next.js App Router
- Remix Router

Include pros and cons of each and recommend use cases.

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                         App Component                         │
│  Wraps application with BrowserRouter and AuthProvider      │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                      BrowserRouter                           │
│  Manages browser history and URL routing                   │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                         Routes                               │
│  Container for all route definitions                        │
└────────────────────────┬────────────────────────────────────┘
                         │
            ┌────────────┴────────────┐
            │                         │
            ▼                         ▼
┌─────────────────────┐  ┌─────────────────────┐
│   Public Routes     │  │  Protected Routes   │
│  - Login            │  │  - Dashboard       │
│  - Register         │  │  - Profile         │
│  - Landing          │  │  - Settings        │
└─────────────────────┘  └─────────────────────┘
            │                         │
            │                         ▼
            │              ┌─────────────────────┐
            │              │   ProtectedRoute    │
            │              │  Auth Check         │
            │              │  Role Check         │
            │              └──────────┬──────────┘
            │                         │
            │                         ▼
            │              ┌─────────────────────┐
            │              │     Layouts         │
            │              │  - AuthLayout       │
            │              │  - AdminLayout      │
            │              └──────────┬──────────┘
            │                         │
            │                         ▼
            │              ┌─────────────────────┐
            │              │      Outlet         │
            │              │  Child Route Render │
            │              └──────────┬──────────┘
            │                         │
            └─────────────────────────┼──────────────────────┐
                                      │                      │
                                      ▼                      ▼
                          ┌──────────────────┐    ┌──────────────────┐
                          │    Pages         │    │   Components     │
                          │  - Dashboard     │    │  - Header        │
                          │  - Products      │    │  - Sidebar       │
                          │  - Orders        │    │  - Footer        │
                          └──────────────────┘    └──────────────────┘
```

---

## Additional Resources

- [React Router Documentation](https://reactrouter.com/)
- [React Router Tutorial](https://reactrouter.com/en/main/start/tutorial)
- [SPA Routing Concepts](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Next_steps/Client-side_routing)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)

---

**Congratulations on completing Session 6!** You now have a solid understanding of routing with React Router, including navigation, nested routes, protection, and project architecture. Continue practicing with the exercises and homework to reinforce these concepts before moving to the next session.