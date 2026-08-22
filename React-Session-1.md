# React.js Session 1: Introduction to React and JSX

**Duration:** 3 hours (1.5 hours theory + 1.5 hours practice)  
**Level:** Beginner  
**Prerequisites:** HTML, CSS, JavaScript ES6+ basics

---

## Session Timeline

### Theory Part (1.5 hours)
- **0:00-0:15:** Introduction to React, What is React, Why React (Topics 1-3)
- **0:15-0:25:** React Library vs Framework, React Ecosystem (Topics 4-5)
- **0:25-0:35:** SPA Concept, CSR Concept (Topics 6-7)
- **0:35-0:45:** React vs Vanilla JavaScript, React vs Next.js (Topics 8-9)
- **0:45-0:55:** Installing Node.js, Creating React Project with Vite (Topics 10-11)
- **0:55-1:05:** Project Structure, package.json (Topics 12-13)
- **1:05-1:15:** src folder, public folder (Topics 14-15)
- **1:15-1:25:** main.tsx, App.tsx (Topics 16-17)
- **1:25-1:30:** JSX Introduction (Topic 18)

### Practice Part (1.5 hours)
- **1:30-1:40:** JSX vs HTML, JSX Expressions (Topics 19-20)
- **1:40-1:50:** Variables & JavaScript inside JSX (Topics 21-22)
- **1:50-2:00:** className, htmlFor, Self-closing tags (Topics 23-25)
- **2:00-2:10:** Fragments (Topic 26)
- **2:10-2:20:** Functional Components, Component Naming (Topics 27-28)
- **2:20-2:30:** Component Composition, Reusable Components (Topics 29-30)
- **2:30-3:00:** Practical Project - Landing Page

---

## Detailed Topics

### 1. Introduction to React

**Definition:** React is a JavaScript library for building user interfaces, developed and maintained by Meta (Facebook). It's focused on creating interactive UIs with a component-based architecture.

**Why we need it:**
- Makes building complex UIs easier and more maintainable
- Provides a declarative approach to programming
- Enables reusable components
- Offers excellent performance with virtual DOM
- Large community and ecosystem

**How it works:** React uses a virtual DOM to efficiently update only the parts of the actual DOM that have changed, rather than re-rendering the entire page.

**Syntax:** React uses JSX (JavaScript XML) syntax that looks similar to HTML but is actually JavaScript.

**Simple Example:**
```tsx
const element = <h1>Hello, React!</h1>;
```

**Practical Example:** A simple counter that updates when clicked
```tsx
import { useState } from 'react';

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

**Common Errors:**
- Forgetting to import React components
- Not understanding the component lifecycle
- Misusing state management

**Best Practices:**
- Keep components small and focused
- Use functional components with hooks
- Follow naming conventions
- Write clean, readable code

**Student Question:** What do you think makes React different from traditional JavaScript DOM manipulation?

---

### 2. What is React?

**Definition:** React is an open-source JavaScript library created by Facebook in 2013 for building user interfaces, particularly single-page applications where data changes over time.

**Why we need it:**
- Simplifies the process of building interactive UIs
- Provides a structured way to organize code
- Makes state management predictable
- Enables efficient updates through virtual DOM

**How it works:** React breaks down UIs into reusable components and manages the state of each component independently. When state changes, React efficiently updates only the necessary parts of the UI.

**Syntax:** React uses a declarative syntax where you describe what the UI should look like, and React handles the how.

**Simple Example:**
```tsx
// Declarative approach
function Welcome({ name }: { name: string }) {
  return <h1>Hello, {name}!</h1>;
}
```

**Practical Example:** A greeting component that displays different messages based on time
```tsx
function Greeting() {
  const hour = new Date().getHours();
  const message = hour < 12 ? 'Good morning' : hour < 18 ? 'Good afternoon' : 'Good evening';
  
  return <h1>{message}!</h1>;
}
```

**Common Errors:**
- Treating React as a full framework
- Not understanding the component-based architecture
- Overcomplicating simple components

**Best Practices:**
- Think in components
- Keep components pure when possible
- Use props for data flow
- Understand the virtual DOM concept

**Student Question:** How do you think organizing code into components helps with large applications?

---

### 3. Why React?

**Definition:** React's popularity stems from its ability to solve common frontend development challenges efficiently.

**Why we need it:**
- **Performance:** Virtual DOM minimizes expensive DOM operations
- **Component Reusability:** Write once, use everywhere
- **Strong Ecosystem:** Huge library of third-party tools
- **Developer Experience:** Excellent tooling and debugging
- **Industry Standard:** Used by major companies (Facebook, Netflix, Airbnb)
- **Easy Learning Curve:** Gentle slope for JavaScript developers

**How it works:** React provides a set of tools and patterns that make frontend development more predictable and maintainable.

**Syntax:** Standard JavaScript with JSX extension.

**Simple Example:**
```tsx
// Reusable button component
function Button({ children, onClick }: { children: React.ReactNode; onClick: () => void }) {
  return <button onClick={onClick}>{children}</button>;
}
```

**Practical Example:** Multiple buttons using the same component
```tsx
function App() {
  return (
    <div>
      <Button onClick={() => console.log('Clicked 1')}>Button 1</Button>
      <Button onClick={() => console.log('Clicked 2')}>Button 2</Button>
      <Button onClick={() => console.log('Clicked 3')}>Button 3</Button>
    </div>
  );
}
```

**Common Errors:**
- Using React for simple static websites
- Not leveraging component reusability
- Ignoring performance considerations

**Best Practices:**
- Choose React for interactive, data-driven applications
- Leverage the ecosystem
- Follow React's principles
- Stay updated with best practices

**Student Question:** What type of applications would you choose React for, and why?

---

### 4. React Library vs Framework

**Definition:** React is a library, not a framework. This is a crucial distinction that affects how you build applications.

**Why we need to understand this:**
- Libraries focus on specific functionality
- Frameworks provide complete solutions
- React gives you flexibility to choose your own tools
- Understanding this helps in architectural decisions

**How it works:**
- **Library:** You call library functions when you need them
- **Framework:** The framework calls your code (inversion of control)
- React only handles the view layer
- You need to add other libraries for routing, state management, etc.

**Syntax:** No special syntax, but affects how you structure your project.

**Simple Example:**
```tsx
// React as a library - you choose when to use it
import { useState } from 'react'; // Import only what you need
```

**Practical Example:** React with additional libraries
```tsx
import { useState } from 'react';
// You might add:
// react-router for routing
// redux for state management
// axios for API calls
```

**Common Errors:**
- Expecting React to provide everything out of the box
- Not understanding when to use additional libraries
- Over-engineering simple applications

**Best Practices:**
- Start with core React
- Add libraries only when needed
- Understand the trade-offs of each library
- Keep the architecture simple

**Student Question:** What are the advantages and disadvantages of using a library versus a framework?

---

### 5. React Ecosystem

**Definition:** The React ecosystem consists of numerous libraries, tools, and utilities built around React to enhance its functionality.

**Why we need it:**
- React alone doesn't provide complete application solutions
- Ecosystem fills gaps in functionality
- Provides battle-tested solutions
- Accelerates development

**How it works:** The ecosystem is modular - you pick and choose the tools you need for your specific requirements.

**Key Ecosystem Tools:**
- **Routing:** React Router
- **State Management:** Redux, Zustand, Context API
- **Form Handling:** React Hook Form, Formik
- **HTTP Clients:** Axios, Fetch API
- **Styling:** Tailwind CSS, Styled Components, CSS Modules
- **Testing:** Jest, React Testing Library
- **Build Tools:** Vite, Webpack

**Syntax:** Each library has its own API and usage patterns.

**Simple Example:**
```tsx
// Using React Router
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

**Practical Example:** Multiple ecosystem tools working together
```tsx
import { useState } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import axios from 'axios';

function App() {
  const [data, setData] = useState(null);
  
  // Using multiple ecosystem tools
  const fetchData = async () => {
    const response = await axios.get('/api/data');
    setData(response.data);
  };
  
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home data={data} fetchData={fetchData} />} />
      </Routes>
    </BrowserRouter>
  );
}
```

**Common Errors:**
- Adding too many unnecessary libraries
- Not understanding library compatibility
- Ignoring bundle size implications

**Best Practices:**
- Start simple, add complexity as needed
- Choose popular, well-maintained libraries
- Consider performance implications
- Keep dependencies updated

**Student Question:** How would you decide which libraries to include in your React project?

---

### 6. SPA Concept (Single Page Application)

**Definition:** A Single Page Application is a web application that loads a single HTML page and dynamically updates the content as the user interacts with the app, without requiring full page reloads.

**Why we need it:**
- Faster user experience (no page reloads)
- Smoother transitions between views
- Better mobile experience
- Reduced server load
- More app-like feel

**How it works:**
- Initial load downloads HTML, CSS, and JavaScript
- JavaScript handles routing and content updates
- API calls fetch data as needed
- Browser history API manages navigation

**Syntax:** Uses client-side routing and dynamic content rendering.

**Simple Example:**
```tsx
// Conceptual SPA structure
function App() {
  const [currentPage, setCurrentPage] = useState('home');
  
  return (
    <div>
      <nav>
        <button onClick={() => setCurrentPage('home')}>Home</button>
        <button onClick={() => setCurrentPage('about')}>About</button>
      </nav>
      {currentPage === 'home' ? <Home /> : <About />}
    </div>
  );
}
```

**Practical Example:** Simple SPA with React Router
```tsx
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

**Common Errors:**
- Not handling browser back button properly
- Poor initial load performance
- SEO challenges (though solvable)
- Complex state management

**Best Practices:**
- Use proper routing libraries
- Implement loading states
- Consider SEO for public-facing apps
- Optimize bundle size

**Student Question:** What are the main differences between a traditional multi-page website and a SPA?

---

### 7. CSR Concept (Client-Side Rendering)

**Definition:** Client-Side Rendering is a technique where the browser downloads a minimal HTML page and renders the content using JavaScript after the page loads.

**Why we need it:**
- Faster subsequent page navigation
- Rich interactive experiences
- Reduced server load
- Better separation of concerns
- Offline capabilities with service workers

**How it works:**
1. Browser requests initial HTML (mostly empty)
2. Browser downloads JavaScript bundle
3. JavaScript executes and renders content
4. User interactions trigger JavaScript updates
5. Data fetched via API calls as needed

**Syntax:** Standard React rendering with useEffect for data fetching.

**Simple Example:**
```tsx
import { useState, useEffect } from 'react';

function UserList() {
  const [users, setUsers] = useState([]);
  
  useEffect(() => {
    // Data fetched on client side
    fetch('/api/users')
      .then(res => res.json())
      .then(data => setUsers(data));
  }, []);
  
  return (
    <ul>
      {users.map(user => <li key={user.id}>{user.name}</li>)}
    </ul>
  );
}
```

**Practical Example:** CSR with loading states
```tsx
import { useState, useEffect } from 'react';

function ProductList() {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    fetch('/api/products')
      .then(res => {
        if (!res.ok) throw new Error('Failed to fetch');
        return res.json();
      })
      .then(data => {
        setProducts(data);
        setLoading(false);
      })
      .catch(err => {
        setError(err.message);
        setLoading(false);
      });
  }, []);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <div>
      {products.map(product => (
        <div key={product.id}>
          <h3>{product.name}</h3>
          <p>{product.price}</p>
        </div>
      ))}
    </div>
  );
}
```

**Common Errors:**
- Poor initial load performance
- SEO issues
- JavaScript-dependent functionality
- Complex state management

**Best Practices:**
- Implement proper loading states
- Handle errors gracefully
- Consider SEO for public content
- Optimize bundle size
- Use caching strategies

**Student Question:** What are the trade-offs between Client-Side Rendering and Server-Side Rendering?

---

### 8. React vs Vanilla JavaScript

**Definition:** Comparing React's approach to building UIs with traditional vanilla JavaScript DOM manipulation.

**Why we need to understand this:**
- Helps understand React's value proposition
- Shows when React is appropriate
- Demonstrates React's abstractions
- Aids in debugging and optimization

**How it works:**
- **Vanilla JS:** Direct DOM manipulation, imperative
- **React:** Declarative, component-based, virtual DOM

**Syntax Comparison:**

**Vanilla JavaScript:**
```javascript
// Creating elements
const button = document.createElement('button');
button.textContent = 'Click me';
button.addEventListener('click', () => {
  alert('Clicked!');
});
document.body.appendChild(button);

// Updating content
document.getElementById('counter').textContent = count;
```

**React:**
```tsx
// Declarative approach
function Button() {
  const [count, setCount] = useState(0);
  
  return (
    <button onClick={() => setCount(count + 1)}>
      Clicked {count} times
    </button>
  );
}
```

**Practical Example - Counter in both approaches:**

**Vanilla JavaScript:**
```javascript
let count = 0;
const button = document.getElementById('counter-btn');
const display = document.getElementById('counter-display');

button.addEventListener('click', () => {
  count++;
  display.textContent = count;
});
```

**React:**
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

**Common Errors in Vanilla JS:**
- Memory leaks from event listeners
- Inconsistent state management
- Difficult to maintain for complex UIs
- Performance issues with large DOM trees

**Common Errors in React:**
- Over-engineering simple components
- Not understanding React's re-rendering
- Misusing state and props

**Best Practices:**
- Use React for complex, interactive UIs
- Vanilla JS might be sufficient for simple interactions
- Understand both approaches
- Choose the right tool for the job

**Student Question:** In what scenarios would you choose vanilla JavaScript over React?

---

### 9. React vs Next.js

**Definition:** Next.js is a React framework that provides additional features like server-side rendering, static site generation, and file-based routing.

**Why we need to understand this:**
- Helps choose the right tool for the project
- Understanding when to use React vs Next.js
- Knowing the trade-offs between approaches

**How it works:**
- **React:** Library for building UIs, client-side by default
- **Next.js:** Full-featured framework built on React with additional capabilities

**Key Differences:**

| Feature | React | Next.js |
|---------|-------|---------|
| Type | Library | Framework |
| Routing | Manual (React Router) | Built-in file-based |
| Rendering | Client-side default | SSR, SSG, ISR options |
| SEO | Requires setup | Built-in optimization |
| API Routes | Requires separate backend | Built-in API routes |
| Performance | Good | Excellent with optimizations |

**Syntax Comparison:**

**React with Vite:**
```tsx
// App.tsx
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

**Next.js:**
```tsx
// app/page.tsx
export default function Home() {
  return <div>Home Page</div>;
}

// app/about/page.tsx
export default function About() {
  return <div>About Page</div>;
}
```

**Practical Example - Data Fetching:**

**React (Client-side):**
```tsx
import { useState, useEffect } from 'react';

function Products() {
  const [products, setProducts] = useState([]);
  
  useEffect(() => {
    fetch('/api/products')
      .then(res => res.json())
      .then(data => setProducts(data));
  }, []);
  
  return <div>{/* render products */}</div>;
}
```

**Next.js (Server-side):**
```tsx
// app/products/page.tsx
async function getProducts() {
  const res = await fetch('/api/products');
  return res.json();
}

export default async function Products() {
  const products = await getProducts();
  
  return <div>{/* render products */}</div>;
}
```

**Common Errors:**
- Using Next.js features in plain React
- Not understanding SSR vs CSR implications
- Overcomplicating simple projects with Next.js

**Best Practices:**
- Use React for simple SPAs
- Use Next.js for production apps needing SEO
- Consider project requirements when choosing
- Start with React, move to Next.js if needed

**Student Question:** When would you choose Next.js over plain React, and vice versa?

---

### 10. Installing Node.js

**Definition:** Node.js is a JavaScript runtime that allows you to run JavaScript outside the browser. It's required for React development tools.

**Why we need it:**
- React development tools (Vite, Create React App) run on Node.js
- Package manager (npm) comes with Node.js
- Build tools and development servers require Node.js
- TypeScript compilation needs Node.js

**How it works:**
- Download and install Node.js from nodejs.org
- Installation includes npm (Node Package Manager)
- Provides JavaScript runtime environment
- Enables server-side JavaScript development

**Syntax:** Command-line installation and verification.

**Simple Example:**
```bash
# Check if Node.js is installed
node --version

# Check npm version
npm --version
```

**Practical Example - Installation Steps:**

1. **Download Node.js:**
   - Visit https://nodejs.org
   - Download LTS (Long Term Support) version
   - Run installer with default settings

2. **Verify Installation:**
```bash
node --version
# Should output something like: v20.10.0

npm --version
# Should output something like: 10.2.3
```

3. **Update npm (optional but recommended):**
```bash
npm install -g npm@latest
```

**Common Errors:**
- Path issues after installation
- Permission errors on some systems
- Version conflicts with existing installations
- Network issues during installation

**Best Practices:**
- Always use LTS version for stability
- Keep Node.js updated
- Use version managers (nvm) for multiple projects
- Verify installation before starting React development

**Student Question:** Why do we need Node.js for frontend development with React?

---

### 11. Creating React Project using Vite

**Definition:** Vite is a modern build tool that provides a faster development experience for React projects with instant hot module replacement.

**Why we need it:**
- Fast development server startup
- Instant hot module replacement
- Optimized production builds
- Modern build tooling
- Better TypeScript support
- Industry standard for React development

**How it works:**
- Uses native ES modules in development
- Leverages Rollup for production builds
- Provides pre-configured React + TypeScript setup
- Offers plugin system for extensibility

**Syntax:** Command-line project creation.

**Simple Example:**
```bash
npm create vite@latest my-react-app -- --template react-ts
```

**Practical Example - Complete Setup:**

1. **Create new project:**
```bash
npm create vite@latest landing-page -- --template react-ts
```

2. **Navigate to project directory:**
```bash
cd landing-page
```

3. **Install dependencies:**
```bash
npm install
```

4. **Start development server:**
```bash
npm run dev
```

5. **Build for production:**
```bash
npm run build
```

6. **Preview production build:**
```bash
npm run preview
```

**Project Structure After Creation:**
```
landing-page/
├── node_modules/
├── public/
│   └── vite.svg
├── src/
│   ├── App.tsx
│   ├── main.tsx
│   └── vite-env.d.ts
├── index.html
├── package.json
├── tsconfig.json
├── tsconfig.node.json
└── vite.config.ts
```

**Common Errors:**
- Node.js version too old
- Network issues during npm install
- Port already in use
- TypeScript configuration errors

**Best Practices:**
- Always use the TypeScript template
- Keep dependencies updated
- Use meaningful project names
- Read the Vite documentation for advanced features

**Student Exercise:** Create a new React project using Vite with TypeScript and verify it runs successfully.

---

### 12. Project Structure

**Definition:** The project structure in a React + Vite application organizes files and folders in a standard way for maintainability and scalability.

**Why we need it:**
- Organized code is easier to maintain
- Standard structure helps team collaboration
- Makes scaling the project easier
- Follows industry best practices
- Helps with tooling and build processes

**How it works:**
- Separates source code from configuration
- Organizes assets and dependencies
- Provides clear locations for different file types
- Follows convention over configuration

**Syntax:** File and folder organization.

**Standard Vite + React + TypeScript Structure:**
```
my-react-app/
├── node_modules/          # Installed dependencies
├── public/                # Static assets
│   └── vite.svg          # Logo and static files
├── src/                   # Source code
│   ├── assets/           # Images, fonts, etc.
│   ├── components/       # Reusable components
│   ├── App.tsx           # Main app component
│   ├── main.tsx          # Application entry point
│   └── vite-env.d.ts     # Vite TypeScript declarations
├── index.html            # HTML template
├── package.json          # Project configuration
├── tsconfig.json         # TypeScript configuration
├── tsconfig.node.json    # TypeScript config for Node
├── vite.config.ts        # Vite configuration
└── README.md             # Project documentation
```

**Simple Example - Basic Structure:**
```
landing-page/
├── public/
├── src/
│   ├── App.tsx
│   └── main.tsx
├── index.html
└── package.json
```

**Practical Example - Enhanced Structure for Landing Page:**
```
landing-page/
├── public/
│   └── images/
├── src/
│   ├── components/
│   │   ├── Header.tsx
│   │   ├── Hero.tsx
│   │   ├── Features.tsx
│   │   ├── ProductCard.tsx
│   │   └── Footer.tsx
│   ├── App.tsx
│   └── main.tsx
├── index.html
└── package.json
```

**Common Errors:**
- Placing files in wrong directories
- Not following naming conventions
- Cluttering the root directory
- Ignoring the public folder purpose

**Best Practices:**
- Keep source code in src/
- Use components/ for reusable UI parts
- Separate concerns with folders
- Use descriptive file names
- Keep the structure flat initially

**Student Question:** Why do we separate public and src folders in a React project?

---

### 13. package.json

**Definition:** The package.json file is the heart of any Node.js project, containing metadata, dependencies, scripts, and configuration information.

**Why we need it:**
- Defines project dependencies
- Contains scripts for running the project
- Stores project metadata
- Configures project behavior
- Enables reproducible builds

**How it works:**
- JSON format with specific fields
- npm reads this file to manage dependencies
- Build tools use it for configuration
- Contains scripts for development workflow

**Syntax:** JSON format with specific keys.

**Simple Example:**
```json
{
  "name": "my-react-app",
  "version": "1.0.0",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview"
  }
}
```

**Practical Example - Complete package.json:**
```json
{
  "name": "landing-page",
  "private": true,
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@types/react": "^18.2.43",
    "@types/react-dom": "^18.2.17",
    "@vitejs/plugin-react": "^4.2.1",
    "typescript": "^5.2.2",
    "vite": "^5.0.8"
  }
}
```

**Key Sections Explained:**

1. **Metadata:**
```json
{
  "name": "landing-page",
  "version": "1.0.0",
  "private": true,
  "type": "module"
}
```

2. **Scripts:**
```json
{
  "scripts": {
    "dev": "vite",              // Start development server
    "build": "tsc && vite build", // Build for production
    "preview": "vite preview"   // Preview production build
  }
}
```

3. **Dependencies:**
```json
{
  "dependencies": {
    "react": "^18.2.0",        // Runtime dependencies
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@types/react": "^18.2.43", // Development dependencies
    "typescript": "^5.2.2"
  }
}
```

**Common Errors:**
- Invalid JSON syntax
- Missing required fields
- Incorrect version ranges
- Script command errors

**Best Practices:**
- Keep dependencies updated
- Use semantic versioning
- Document custom scripts
- Separate dev and production dependencies
- Use npm scripts instead of direct commands

**Student Exercise:** Add a custom script to package.json that runs both TypeScript checking and development server.

---

### 14. src Folder

**Definition:** The src folder contains all the source code for your React application, including components, styles, utilities, and other application logic.

**Why we need it:**
- Separates source code from configuration
- Organizes application logic
- Makes the project structure clear
- Helps with build processes
- Follows industry conventions

**How it works:**
- Build tools look for source code in src/
- Contains TypeScript/JavaScript files
- Can contain subdirectories for organization
- Entry point (main.tsx) lives here

**Syntax:** Folder organization with TypeScript files.

**Simple Example:**
```
src/
├── App.tsx
└── main.tsx
```

**Practical Example - Organized src Structure:**
```
src/
├── assets/              # Images, fonts, styles
│   ├── images/
│   └── styles/
├── components/          # Reusable components
│   ├── ui/             # Basic UI components
│   └── layout/         # Layout components
├── hooks/              # Custom hooks
├── utils/              # Utility functions
├── types/              # TypeScript type definitions
├── App.tsx             # Main app component
├── main.tsx            # Entry point
└── vite-env.d.ts       # Vite types
```

**File Types in src:**

1. **Components (.tsx):**
```tsx
// src/components/Button.tsx
function Button() {
  return <button>Click me</button>;
}
```

2. **Styles (.css, .module.css):**
```css
/* src/styles/App.css */
.app {
  text-align: center;
}
```

3. **Types (.ts):**
```ts
// src/types/index.ts
export interface User {
  id: number;
  name: string;
  email: string;
}
```

4. **Utilities (.ts):**
```ts
// src/utils/helpers.ts
export function formatDate(date: Date): string {
  return date.toLocaleDateString();
}
```

**Common Errors:**
- Not organizing files properly
- Mixing component types
- Not using subdirectories for larger projects
- Placing non-source files in src/

**Best Practices:**
- Keep related files together
- Use descriptive folder names
- Start simple, add structure as needed
- Follow naming conventions
- Separate concerns (components, utils, types)

**Student Question:** How would you organize the src folder for a large application with many features?

---

### 15. public Folder

**Definition:** The public folder contains static assets that are served directly without being processed by the build tools.

**Why we need it:**
- Serve static files directly
- Place favicon and other metadata
- Store images that don't need processing
- Hold robots.txt and other SEO files
- Reference assets with absolute paths

**How it works:**
- Files are copied to build output as-is
- Can be referenced with absolute paths
- Not processed by TypeScript or bundlers
- Served from the root URL

**Syntax:** Direct file referencing with absolute paths.

**Simple Example:**
```
public/
├── favicon.ico
├── vite.svg
└── robots.txt
```

**Practical Example - Enhanced public Folder:**
```
public/
├── favicon.ico
├── logo.png
├── robots.txt
├── manifest.json
├── images/
│   ├── hero-bg.jpg
│   └── product-1.png
└── fonts/
    └── custom-font.woff2
```

**Usage in React:**

```tsx
// Referencing public folder assets
function App() {
  return (
    <div>
      <img src="/logo.png" alt="Logo" />
      <link rel="icon" href="/favicon.ico" />
    </div>
  );
}
```

**When to Use public vs src/assets:**

**Use public/ for:**
- Favicon and site icons
- robots.txt
- manifest.json (PWA)
- Large images that don't need optimization
- Files that need absolute URLs

**Use src/assets/ for:**
- Images that need processing/optimization
- Component-specific assets
- Files imported in JavaScript/TypeScript
- Assets that should be hashed for caching

**Common Errors:**
- Placing files that should be in src/ in public/
- Using relative paths instead of absolute
- Not understanding the build process
- Overusing public folder for all assets

**Best Practices:**
- Keep public folder minimal
- Use src/assets for most images
- Reference with absolute paths (/filename)
- Only use for truly static files
- Consider file size and optimization

**Student Question:** When would you place an image in the public folder versus src/assets?

---

### 16. main.tsx

**Definition:** main.tsx is the entry point of a React application where the React application is mounted to the DOM.

**Why we need it:**
- Connects React to the HTML
- Initializes the application
- Sets up the root component
- Configures providers and global setup
- Starts the React application

**How it works:**
- Imports the root App component
- Finds the DOM element to mount to
- Creates a React root
- Renders the App component

**Syntax:** Standard React mounting pattern.

**Simple Example:**
```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

**Practical Example - Enhanced main.tsx:**
```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './index.css';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

**Key Parts Explained:**

1. **Imports:**
```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
```

2. **Root Creation:**
```tsx
const root = ReactDOM.createRoot(document.getElementById('root')!);
```

3. **Rendering:**
```tsx
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

**React.StrictMode:**
- Activates additional development checks
- Warns about unsafe practices
- Helps identify potential issues
- Doesn't affect production build

**Common Errors:**
- Missing root element in HTML
- Incorrect import paths
- Not using createRoot (old API)
- TypeScript errors with null checks

**Best Practices:**
- Keep main.tsx simple
- Use StrictMode in development
- Import global styles here
- Don't add complex logic
- Use the non-null assertion (!) carefully

**Student Question:** What is the purpose of React.StrictMode in main.tsx?

---

### 17. App.tsx

**Definition:** App.tsx is the root component of your React application that represents the entire UI hierarchy.

**Why we need it:**
- Serves as the main component
- Contains the overall layout
- Manages global state providers
- Defines the application structure
- Acts as the composition root

**How it works:**
- Imported and rendered in main.tsx
- Can contain child components
- Manages application-level state
- Provides layout and routing structure

**Syntax:** Functional component with JSX return.

**Simple Example:**
```tsx
function App() {
  return (
    <div className="app">
      <h1>Hello, React!</h1>
    </div>
  );
}

export default App;
```

**Practical Example - Structured App Component:**
```tsx
import { useState } from 'react';
import Header from './components/Header';
import Hero from './components/Hero';
import Features from './components/Features';
import Footer from './components/Footer';

function App() {
  const [theme, setTheme] = useState('light');
  
  return (
    <div className={`app ${theme}`}>
      <Header onThemeChange={setTheme} />
      <main>
        <Hero />
        <Features />
      </main>
      <Footer />
    </div>
  );
}

export default App;
```

**App Component Responsibilities:**

1. **Layout Structure:**
```tsx
function App() {
  return (
    <div className="app">
      <Header />
      <main>{/* Page content */}</main>
      <Footer />
    </div>
  );
}
```

2. **Global Providers:**
```tsx
function App() {
  return (
    <ThemeProvider>
      <AuthProvider>
        <Router>
          <Header />
          <Routes>{/* Routes */}</Routes>
          <Footer />
        </Router>
      </AuthProvider>
    </ThemeProvider>
  );
}
```

3. **Application State:**
```tsx
function App() {
  const [user, setUser] = useState(null);
  
  return (
    <div className="app">
      {user ? <Dashboard user={user} /> : <Login onLogin={setUser} />}
    </div>
  );
}
```

**Common Errors:**
- Making App component too complex
- Not organizing child components
- Mixing concerns in App component
- Not using proper composition

**Best Practices:**
- Keep App component focused on structure
- Delegate functionality to child components
- Use App for layout and providers
- Avoid complex business logic in App
- Maintain clear component hierarchy

**Student Question:** What should and shouldn't go into the App component?

---

### 18. JSX

**Definition:** JSX (JavaScript XML) is a syntax extension for JavaScript that allows you to write HTML-like code in your JavaScript files.

**Why we need it:**
- Makes React code more readable
- Provides familiar HTML-like syntax
- Enables component composition
- Integrates JavaScript logic with UI
- Improves developer experience

**How it works:**
- JSX is transpiled to regular JavaScript
- Babel or TypeScript handles the conversion
- JSX elements become React.createElement calls
- Enables writing UI in a declarative way

**Syntax:** HTML-like syntax within JavaScript.

**Simple Example:**
```tsx
const element = <h1>Hello, JSX!</h1>;
```

**Practical Example - JSX with Expressions:**
```tsx
function Greeting({ name }: { name: string }) {
  const greeting = name ? `Hello, ${name}!` : 'Hello, World!';
  
  return (
    <div>
      <h1>{greeting}</h1>
      <p>Welcome to React</p>
    </div>
  );
}
```

**JSX Transpilation:**

**Before (JSX):**
```tsx
const element = <h1>Hello, World!</h1>;
```

**After (JavaScript):**
```javascript
const element = React.createElement('h1', null, 'Hello, World!');
```

**Key JSX Rules:**

1. **Must return a single parent element:**
```tsx
// ✅ Correct
function App() {
  return (
    <div>
      <h1>Title</h1>
      <p>Content</p>
    </div>
  );
}

// ❌ Incorrect - multiple root elements
function App() {
  return (
    <h1>Title</h1>
    <p>Content</p>
  );
}
```

2. **Use camelCase for attributes:**
```tsx
// ✅ Correct
<input className="input" htmlFor="email" />

// ❌ Incorrect
<input class="input" for="email" />
```

3. **Self-closing tags need /:**
```tsx
// ✅ Correct
<img src="logo.png" alt="Logo" />
<input type="text" />

// ❌ Incorrect
<img src="logo.png" alt="Logo">
<input type="text">
```

**Common Errors:**
- Multiple root elements without wrapper
- Using HTML attribute names instead of JSX
- Forgetting self-closing tag syntax
- Improper nesting of elements

**Best Practices:**
- Use fragments to avoid wrapper divs
- Follow JSX naming conventions
- Keep JSX readable with proper formatting
- Use parentheses for multi-line JSX

**Student Question:** Why does JSX need to be transpiled to regular JavaScript?

---

### 19. JSX vs HTML

**Definition:** Understanding the differences between JSX syntax and standard HTML to avoid common mistakes.

**Why we need it:**
- JSX looks like HTML but has important differences
- Prevents syntax errors
- Ensures proper React usage
- Helps with debugging
- Improves code quality

**How it works:**
- JSX is JavaScript, not HTML
- Different attribute naming conventions
- Different syntax rules
- Compiled to React.createElement calls

**Syntax Comparison:**

**HTML:**
```html
<div class="container">
  <label for="email">Email:</label>
  <input type="text" name="email">
  <img src="image.jpg" alt="Image">
</div>
```

**JSX:**
```tsx
<div className="container">
  <label htmlFor="email">Email:</label>
  <input type="text" name="email" />
  <img src="image.jpg" alt="Image" />
</div>
```

**Key Differences:**

1. **Attribute Names:**
```tsx
// HTML → JSX
class → className
for → htmlFor
tabindex → tabIndex
readonly → readOnly
colspan → colSpan
rowspan → rowSpan
```

2. **Self-Closing Tags:**
```tsx
// HTML
<img src="image.jpg">

// JSX
<img src="image.jpg" />
```

3. **Inline Styles:**
```tsx
// HTML
<div style="color: red; font-size: 16px;">

// JSX
<div style={{ color: 'red', fontSize: '16px' }}>
```

4. **Comments:**
```tsx
// HTML
<!-- This is a comment -->

// JSX
{/* This is a comment */}
```

5. **JavaScript Expressions:**
```tsx
// HTML - no expressions
<div>{name}</div> // Shows literal {name}

// JSX - evaluates expressions
<div>{name}</div> // Shows the value of name
```

**Practical Example - Conversion:**

**HTML:**
```html
<div class="card">
  <h2 class="title">Product Name</h2>
  <p class="description">Product description</p>
  <button class="btn" onclick="handleClick()">Buy Now</button>
</div>
```

**JSX:**
```tsx
<div className="card">
  <h2 className="title">Product Name</h2>
  <p className="description">Product description</p>
  <button className="btn" onClick={handleClick}>Buy Now</button>
</div>
```

**Common Errors:**
- Using HTML attribute names in JSX
- Forgetting self-closing tags
- Using HTML comment syntax
- Improper inline style syntax

**Best Practices:**
- Always use className instead of class
- Use htmlFor instead of for
- Self-close all void elements
- Use double curly braces for inline styles
- Use {/* */} for comments

**Student Exercise:** Convert this HTML to JSX:
```html
<div class="user-card">
  <label for="username">Username:</label>
  <input type="text" id="username" name="username">
  <img src="avatar.jpg" alt="User Avatar">
</div>
```

---

### 20. JSX Expressions

**Definition:** JSX expressions allow you to embed JavaScript expressions within JSX using curly braces {}.

**Why we need it:**
- Dynamic content rendering
- Integration of JavaScript logic with UI
- Conditional rendering
- List rendering
- Computed values in UI

**How it works:**
- Curly braces {} contain JavaScript expressions
- Expressions are evaluated and result is rendered
- Can include variables, functions, operations
- Any valid JavaScript expression works

**Syntax:** {expression} within JSX.

**Simple Example:**
```tsx
const name = 'John';
return <h1>Hello, {name}!</h1>;
```

**Practical Examples:**

1. **Variables:**
```tsx
function UserCard({ name, age }: { name: string; age: number }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
    </div>
  );
}
```

2. **Mathematical Operations:**
```tsx
function Calculator({ a, b }: { a: number; b: number }) {
  return (
    <div>
      <p>Sum: {a + b}</p>
      <p>Product: {a * b}</p>
      <p>Division: {a / b}</p>
    </div>
  );
}
```

3. **Function Calls:**
```tsx
function formatDate(date: Date): string {
  return date.toLocaleDateString();
}

function DateDisplay({ date }: { date: Date }) {
  return <p>Date: {formatDate(date)}</p>;
}
```

4. **Template Literals:**
```tsx
function Greeting({ firstName, lastName }: { firstName: string; lastName: string }) {
  const fullName = `${firstName} ${lastName}`;
  return <h1>Welcome, {fullName}!</h1>;
}
```

5. **Object Properties:**
```tsx
function ProductCard({ product }: { product: { name: string; price: number } }) {
  return (
    <div>
      <h3>{product.name}</h3>
      <p>Price: ${product.price}</p>
    </div>
  );
}
```

6. **Ternary Operators:**
```tsx
function Status({ isActive }: { isActive: boolean }) {
  return (
    <span className={isActive ? 'active' : 'inactive'}>
      {isActive ? 'Online' : 'Offline'}
    </span>
  );
}
```

**Common Errors:**
- Putting statements instead of expressions
- Forgetting curly braces
- Complex logic in JSX
- Using undefined/null without handling

**Best Practices:**
- Keep expressions simple
- Extract complex logic to functions
- Handle null/undefined values
- Use ternary for simple conditions
- Use logical AND for optional rendering

**Student Question:** What's the difference between a statement and an expression in JavaScript, and why does JSX only accept expressions?

---

### 21. Variables inside JSX

**Definition:** Using JavaScript variables within JSX to display dynamic content.

**Why we need it:**
- Display data from variables
- Create dynamic interfaces
- Show computed values
- Render user-specific content
- Display API data

**How it works:**
- Define variables in component
- Reference them with curly braces in JSX
- Variables are evaluated when component renders
- Changes trigger re-renders

**Syntax:** {variableName} within JSX.

**Simple Example:**
```tsx
function Welcome() {
  const name = 'John';
  return <h1>Welcome, {name}!</h1>;
}
```

**Practical Examples:**

1. **String Variables:**
```tsx
function UserProfile() {
  const username = 'johndoe';
  const email = 'john@example.com';
  
  return (
    <div>
      <h2>{username}</h2>
      <p>{email}</p>
    </div>
  );
}
```

2. **Number Variables:**
```tsx
function PriceDisplay() {
  const price = 99.99;
  const discount = 0.1;
  const finalPrice = price * (1 - discount);
  
  return (
    <div>
      <p>Original: ${price}</p>
      <p>Discount: {discount * 100}%</p>
      <p>Final: ${finalPrice.toFixed(2)}</p>
    </div>
  );
}
```

3. **Boolean Variables:**
```tsx
function UserStatus() {
  const isLoggedIn = true;
  const isAdmin = false;
  
  return (
    <div>
      <p>Status: {isLoggedIn ? 'Logged In' : 'Guest'}</p>
      <p>Admin: {isAdmin ? 'Yes' : 'No'}</p>
    </div>
  );
}
```

4. **Array Variables:**
```tsx
function TodoList() {
  const todos = ['Learn React', 'Build project', 'Deploy app'];
  
  return (
    <ul>
      {todos.map((todo, index) => (
        <li key={index}>{todo}</li>
      ))}
    </ul>
  );
}
```

5. **Object Variables:**
```tsx
function ProductInfo() {
  const product = {
    name: 'Laptop',
    price: 999,
    inStock: true
  };
  
  return (
    <div>
      <h2>{product.name}</h2>
      <p>Price: ${product.price}</p>
      <p>Status: {product.inStock ? 'In Stock' : 'Out of Stock'}</p>
    </div>
  );
}
```

**Common Errors:**
- Not handling undefined/null variables
- Using objects directly in JSX
- Forgetting curly braces
- Not updating variables that should trigger re-renders

**Best Practices:**
- Initialize variables with default values
- Handle null/undefined cases
- Use meaningful variable names
- Keep variable scope appropriate
- Consider using state for dynamic values

**Student Exercise:** Create a component that displays user information using variables for name, age, and location.

---

### 22. JavaScript inside JSX

**Definition:** Embedding JavaScript logic, expressions, and operations within JSX using curly braces.

**Why we need it:**
- Dynamic content based on logic
- Conditional rendering
- Data transformation before display
- Calculations and formatting
- Integration of business logic with UI

**How it works:**
- JavaScript expressions go inside {}
- Any valid JavaScript expression works
- Evaluated at render time
- Result is converted to string/rendered

**Syntax:** {javascriptExpression} within JSX.

**Simple Example:**
```tsx
function Counter() {
  const count = 5;
  return <p>You clicked {count} times</p>;
}
```

**Practical Examples:**

1. **Mathematical Operations:**
```tsx
function PriceCalculator({ price, quantity }: { price: number; quantity: number }) {
  const total = price * quantity;
  const tax = total * 0.1;
  const grandTotal = total + tax;
  
  return (
    <div>
      <p>Subtotal: ${total}</p>
      <p>Tax: ${tax.toFixed(2)}</p>
      <p>Total: ${grandTotal.toFixed(2)}</p>
    </div>
  );
}
```

2. **String Operations:**
```tsx
function UserGreeting({ firstName, lastName }: { firstName: string; lastName: string }) {
  const fullName = `${firstName} ${lastName}`.toUpperCase();
  const initials = `${firstName[0]}${lastName[0]}`;
  
  return (
    <div>
      <h1>{fullName}</h1>
      <p>Initials: {initials}</p>
    </div>
  );
}
```

3. **Array Methods:**
```tsx
function ShoppingList({ items }: { items: string[] }) {
  const itemCount = items.length;
  const firstItem = items[0];
  const lastItem = items[items.length - 1];
  
  return (
    <div>
      <p>Total items: {itemCount}</p>
      <p>First: {firstItem}</p>
      <p>Last: {lastItem}</p>
    </div>
  );
}
```

4. **Object Operations:**
```tsx
function UserStats({ user }: { user: { name: string; scores: { math: number; english: number } } }) {
  const average = (user.scores.math + user.scores.english) / 2;
  const bestSubject = user.scores.math > user.scores.english ? 'Math' : 'English';
  
  return (
    <div>
      <h2>{user.name}</h2>
      <p>Average: {average.toFixed(1)}</p>
      <p>Best Subject: {bestSubject}</p>
    </div>
  );
}
```

5. **Function Calls:**
```tsx
function formatDate(date: Date): string {
  const options = { year: 'numeric', month: 'long', day: 'numeric' };
  return date.toLocaleDateString('en-US', options);
}

function EventCard({ event }: { event: { title: string; date: Date } }) {
  return (
    <div>
      <h3>{event.title}</h3>
      <p>{formatDate(event.date)}</p>
    </div>
  );
}
```

6. **Conditional Logic:**
```tsx
function DiscountBanner({ hasDiscount, discountAmount }: { hasDiscount: boolean; discountAmount: number }) {
  return (
    <div>
      {hasDiscount && (
        <p className="discount">
          Save {discountAmount}% today!
        </p>
      )}
    </div>
  );
}
```

**Common Errors:**
- Using statements instead of expressions
- Complex logic in JSX
- Not handling edge cases
- Performance issues with complex operations

**Best Practices:**
- Extract complex logic to functions
- Use memoization for expensive operations
- Keep JSX expressions simple
- Handle edge cases gracefully
- Consider readability over brevity

**Student Question:** What types of JavaScript can you use inside JSX expressions, and what can't you use?

---

### 23. className

**Definition:** In JSX, the `className` attribute is used instead of HTML's `class` attribute to apply CSS classes to elements.

**Why we need it:**
- `class` is a reserved keyword in JavaScript
- JSX is JavaScript, so it uses JavaScript naming
- Prevents syntax errors
- Maintains consistency with JavaScript conventions
- Follows React's attribute naming rules

**How it works:**
- Use `className` instead of `class`
- Can accept string values
- Can accept dynamic values with expressions
- Can accept multiple classes

**Syntax:** className="class-name" or className={dynamicClass}.

**Simple Example:**
```tsx
// HTML
<div class="container">Content</div>

// JSX
<div className="container">Content</div>
```

**Practical Examples:**

1. **Static className:**
```tsx
function Button() {
  return <button className="btn-primary">Click me</button>;
}
```

2. **Dynamic className:**
```tsx
function Status({ isActive }: { isActive: boolean }) {
  return (
    <span className={isActive ? 'status-active' : 'status-inactive'}>
      {isActive ? 'Active' : 'Inactive'}
    </span>
  );
}
```

3. **Multiple Classes:**
```tsx
function Card({ isFeatured }: { isFeatured: boolean }) {
  const classes = `card ${isFeatured ? 'card-featured' : ''}`;
  return <div className={classes}>Content</div>;
}
```

4. **Conditional Classes:**
```tsx
function Alert({ type, message }: { type: 'success' | 'error' | 'warning'; message: string }) {
  const typeClasses = {
    success: 'alert-success',
    error: 'alert-error',
    warning: 'alert-warning'
  };
  
  return (
    <div className={`alert ${typeClasses[type]}`}>
      {message}
    </div>
  );
}
```

5. **Template Literals:**
```tsx
function UserProfile({ isOnline, isAdmin }: { isOnline: boolean; isAdmin: boolean }) {
  const statusClass = isOnline ? 'online' : 'offline';
  const roleClass = isAdmin ? 'admin' : 'user';
  
  return (
    <div className={`user-card ${statusClass} ${roleClass}`}>
      User Profile
    </div>
  );
}
```

**Common Errors:**
- Using `class` instead of `className`
- Forgetting to handle undefined/null values
- Complex className logic that's hard to read
- Not using className utilities

**Best Practices:**
- Always use className instead of class
- Extract complex className logic to functions
- Use template literals for multiple classes
- Consider CSS modules for scoped styles
- Use utility class libraries (Tailwind) when appropriate

**Student Exercise:** Create a component that applies different classes based on props (size, variant, disabled state).

---

### 24. htmlFor

**Definition:** In JSX, the `htmlFor` attribute is used instead of HTML's `for` attribute to associate labels with form elements.

**Why we need it:**
- `for` is a reserved keyword in JavaScript (used in loops)
- JSX follows JavaScript naming conventions
- Prevents syntax errors
- Maintains accessibility standards
- Ensures proper form functionality

**How it works:**
- Use `htmlFor` instead of `for` on label elements
- References the id of the associated form element
- Provides accessibility and better UX
- Works identically to HTML's for attribute

**Syntax:** <label htmlFor="input-id">Label</label>.

**Simple Example:**
```tsx
// HTML
<label for="email">Email:</label>
<input type="email" id="email">

// JSX
<label htmlFor="email">Email:</label>
<input type="email" id="email" />
```

**Practical Examples:**

1. **Basic Form:**
```tsx
function EmailForm() {
  return (
    <form>
      <label htmlFor="email">Email:</label>
      <input type="email" id="email" name="email" />
    </form>
  );
}
```

2. **Multiple Form Fields:**
```tsx
function RegistrationForm() {
  return (
    <form>
      <div>
        <label htmlFor="username">Username:</label>
        <input type="text" id="username" name="username" />
      </div>
      <div>
        <label htmlFor="password">Password:</label>
        <input type="password" id="password" name="password" />
      </div>
      <div>
        <label htmlFor="confirm-password">Confirm Password:</label>
        <input type="password" id="confirm-password" name="confirm-password" />
      </div>
    </form>
  );
}
```

3. **Checkbox with Label:**
```tsx
function TermsCheckbox() {
  return (
    <label>
      <input type="checkbox" id="terms" name="terms" />
      <span htmlFor="terms">I agree to the terms and conditions</span>
    </label>
  );
}
```

4. **Radio Buttons:**
```tsx
function GenderSelection() {
  return (
    <fieldset>
      <legend>Gender:</legend>
      <label>
        <input type="radio" name="gender" value="male" id="male" />
        <span htmlFor="male">Male</span>
      </label>
      <label>
        <input type="radio" name="gender" value="female" id="female" />
        <span htmlFor="female">Female</span>
      </label>
    </fieldset>
  );
}
```

5. **Accessible Form Component:**
```tsx
function FormField({ label, id, type = 'text', ...props }: any) {
  return (
    <div className="form-field">
      <label htmlFor={id}>{label}</label>
      <input type={type} id={id} {...props} />
    </div>
  );
}

function ContactForm() {
  return (
    <form>
      <FormField label="Name" id="name" />
      <FormField label="Email" id="email" type="email" />
      <FormField label="Phone" id="phone" type="tel" />
    </form>
  );
}
```

**Common Errors:**
- Using `for` instead of `htmlFor`
- Mismatched htmlFor and id values
- Not using htmlFor for accessibility
- Forgetting id on input elements

**Best Practices:**
- Always use htmlFor instead of for
- Ensure htmlFor matches the input's id
- Use htmlFor for all form labels
- Create reusable form field components
- Test accessibility with screen readers

**Student Question:** Why is it important to use htmlFor with labels from an accessibility perspective?

---

### 25. Self-closing tags

**Definition:** In JSX, elements that don't have children must be self-closed with a forward slash (/) before the closing angle bracket.

**Why we need it:**
- JSX follows XML-like syntax rules
- Prevents parsing errors
- Maintains consistency
- Required by JSX specification
- Helps with code validation

**How it works:**
- Void elements (img, input, br, etc.) must be self-closed
- Custom components without children must be self-closed
- Syntax: <element /> instead of <element>

**Syntax:** <tagName /> or <tagName attribute="value" />.

**Simple Example:**
```tsx
// HTML
<img src="image.jpg" alt="Image">
<input type="text">

// JSX
<img src="image.jpg" alt="Image" />
<input type="text" />
```

**Practical Examples:**

1. **HTML Void Elements:**
```tsx
function ImageGallery() {
  return (
    <div>
      <img src="photo1.jpg" alt="Photo 1" />
      <img src="photo2.jpg" alt="Photo 2" />
      <img src="photo3.jpg" alt="Photo 3" />
    </div>
  );
}
```

2. **Form Elements:**
```tsx
function LoginForm() {
  return (
    <form>
      <input type="email" placeholder="Email" />
      <input type="password" placeholder="Password" />
      <button type="submit">Login</button>
    </form>
  );
}
```

3. **Line Breaks and Horizontal Rules:**
```tsx
function TextContent() {
  return (
    <div>
      <p>First paragraph</p>
      <br />
      <p>Second paragraph</p>
      <hr />
      <p>Third paragraph</p>
    </div>
  );
}
```

4. **Custom Components without Children:**
```tsx
function Spacer({ height }: { height: number }) {
  return <div style={{ height: `${height}px` }} />;
}

function PageContent() {
  return (
    <div>
      <h1>Title</h1>
      <Spacer height={20} />
      <p>Content</p>
      <Spacer height={20} />
      <button>Action</button>
    </div>
  );
}
```

5. **Meta and Link Tags:**
```tsx
function PageHead() {
  return (
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <link rel="icon" href="/favicon.ico" />
    </head>
  );
}
```

**Common Void Elements in JSX:**
- `<img />`
- `<input />`
- `<br />`
- `<hr />`
- `<meta />`
- `<link />`
- `<area />`
- `<base />`
- `<col />`
- `<embed />`
- `<source />`
- `<track />`
- `<wbr />`

**Common Errors:**
- Forgetting self-closing slash
- Using HTML syntax for void elements
- Not self-closing custom components
- Inconsistent self-closing usage

**Best Practices:**
- Always self-close void elements
- Self-close custom components without children
- Be consistent with self-closing style
- Use ESLint rules to enforce this
- Format code to make self-closing obvious

**Student Exercise:** Identify which elements in this HTML need to be self-closed in JSX and correct the syntax:
```html
<div>
  <img src="logo.png" alt="Logo">
  <input type="text" placeholder="Name">
  <button>Submit</button>
</div>
```

---

### 26. Fragments

**Definition:** React Fragments let you group multiple elements without adding extra DOM nodes, solving the limitation of JSX requiring a single parent element.

**Why we need it:**
- JSX requires a single parent element
- Sometimes extra divs break CSS/layout
- Avoid unnecessary DOM nesting
- Improve semantic HTML
- Better performance with fewer DOM nodes

**How it works:**
- Wrap elements in React.Fragment or <>
- Doesn't create actual DOM element
- Children are rendered directly
- Can have keys for lists

**Syntax:** <React.Fragment>children</React.Fragment> or <>children</>.

**Simple Example:**
```tsx
// Without Fragment - needs wrapper div
function App() {
  return (
    <div>
      <h1>Title</h1>
      <p>Content</p>
    </div>
  );
}

// With Fragment - no wrapper
function App() {
  return (
    <>
      <h1>Title</h1>
      <p>Content</p>
    </>
  );
}
```

**Practical Examples:**

1. **Basic Fragment:**
```tsx
function UserCard({ name, email }: { name: string; email: string }) {
  return (
    <>
      <h2>{name}</h2>
      <p>{email}</p>
    </>
  );
}
```

2. **Fragment with Key:**
```tsx
function Table({ data }: { data: { id: number; name: string; value: number }[] }) {
  return (
    <table>
      <tbody>
        {data.map((item) => (
          <React.Fragment key={item.id}>
            <tr>
              <td>{item.name}</td>
              <td>{item.value}</td>
            </tr>
          </React.Fragment>
        ))}
      </tbody>
    </table>
  );
}
```

3. **Fragment with Props:**
```tsx
function Modal({ isOpen, children }: { isOpen: boolean; children: React.ReactNode }) {
  if (!isOpen) return null;
  
  return (
    <React.Fragment>
      <div className="modal-overlay" />
      <div className="modal-content">
        {children}
      </div>
    </React.Fragment>
  );
}
```

4. **Avoiding CSS Issues:**
```tsx
// This would break flexbox layout
function BadComponent() {
  return (
    <div className="flex-container">
      <div>Item 1</div>
      <div>Item 2</div>
    </div>
  );
}

// This maintains flexbox layout
function GoodComponent() {
  return (
    <div className="flex-container">
      <>
        <div>Item 1</div>
        <div>Item 2</div>
      </>
    </div>
  );
}
```

5. **List Rendering:**
```tsx
function DefinitionList({ items }: { items: { term: string; definition: string }[] }) {
  return (
    <dl>
      {items.map((item, index) => (
        <React.Fragment key={index}>
          <dt>{item.term}</dt>
          <dd>{item.definition}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}
```

**Common Errors:**
- Not using fragments when needed
- Adding unnecessary wrapper divs
- Forgetting keys when using fragments in lists
- Using fragments when a wrapper is actually needed

**Best Practices:**
- Use fragments to avoid wrapper divs
- Use short syntax <> when no props needed
- Use React.Fragment when you need keys or other props
- Consider semantic HTML - sometimes a wrapper is appropriate
- Use fragments for better DOM structure

**Student Question:** When would you use a regular div wrapper instead of a Fragment?

---

### 27. Functional Components

**Definition:** Functional components are JavaScript functions that return JSX and are the modern way to write React components.

**Why we need them:**
- Simpler than class components
- Easier to understand and test
- Better performance with React.memo
- Work seamlessly with hooks
- Modern React best practice

**How it works:**
- JavaScript function that returns JSX
- Receives props as parameters
- Can use hooks for state and side effects
- Re-render when props or state change

**Syntax:** function ComponentName(props) { return JSX; }

**Simple Example:**
```tsx
function Welcome() {
  return <h1>Hello, World!</h1>;
}
```

**Practical Examples:**

1. **Basic Functional Component:**
```tsx
function Greeting() {
  const message = 'Welcome to React';
  return <h1>{message}</h1>;
}
```

2. **Component with Props:**
```tsx
interface UserProps {
  name: string;
  age: number;
}

function UserCard({ name, age }: UserProps) {
  return (
    <div className="user-card">
      <h2>{name}</h2>
      <p>Age: {age}</p>
    </div>
  );
}
```

3. **Component with State:**
```tsx
import { useState } from 'react';

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

4. **Component with Hooks:**
```tsx
import { useState, useEffect } from 'react';

function Clock() {
  const [time, setTime] = useState(new Date());
  
  useEffect(() => {
    const timer = setInterval(() => setTime(new Date()), 1000);
    return () => clearInterval(timer);
  }, []);
  
  return <div>Current time: {time.toLocaleTimeString()}</div>;
}
```

5. **Arrow Function Component:**
```tsx
const Button = ({ onClick, children }: { onClick: () => void; children: React.ReactNode }) => {
  return <button onClick={onClick}>{children}</button>;
};
```

6. **Component with Multiple Hooks:**
```tsx
import { useState, useEffect, useContext } from 'react';

function UserProfile({ userId }: { userId: number }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const theme = useContext(ThemeContext);
  
  useEffect(() => {
    fetchUser(userId).then(data => {
      setUser(data);
      setLoading(false);
    });
  }, [userId]);
  
  if (loading) return <div>Loading...</div>;
  
  return (
    <div className={theme}>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </div>
  );
}
```

**Component Lifecycle in Functional Components:**
- Mounting: Component function runs
- Updating: Component function re-runs on prop/state changes
- Unmounting: Cleanup functions in useEffect run

**Common Errors:**
- Not returning JSX
- Missing import statements
- Incorrect prop types
- Not handling edge cases

**Best Practices:**
- Use functional components over class components
- Keep components small and focused
- Use TypeScript interfaces for props
- Follow naming conventions (PascalCase)
- Use hooks for state and side effects

**Student Question:** Why are functional components preferred over class components in modern React?

---

### 28. Component Naming

**Definition:** React components follow specific naming conventions to ensure proper functionality and code readability.

**Why we need it:**
- JSX treats lowercase names as HTML tags
- Uppercase names indicate React components
- Consistent naming improves code readability
- Helps with tooling and IDE support
- Follows industry standards

**How it works:**
- Component names must start with uppercase letter
- Use PascalCase (ComponentName)
- Descriptive names that indicate purpose
- Avoid reserved words

**Syntax:** function ComponentName() { return JSX; }

**Simple Example:**
```tsx
// ✅ Correct
function UserProfile() {
  return <div>User Profile</div>;
}

// ❌ Incorrect - treated as HTML element
function userProfile() {
  return <div>User Profile</div>;
}
```

**Practical Examples:**

1. **Basic Naming:**
```tsx
// ✅ Good
function Header() { return <header>Header</header>; }
function Footer() { return <footer>Footer</footer>; }
function Sidebar() { return <aside>Sidebar</aside>; }

// ❌ Bad
function header() { return <header>Header</header>; }
function footer() { return <footer>Footer</footer>; }
```

2. **Descriptive Naming:**
```tsx
// ✅ Good - descriptive
function UserAuthenticationForm() { return <form>Login</form>; }
function ProductDetailsCard() { return <div>Product</div>; }
function NavigationMenuBar() { return <nav>Menu</nav>; }

// ❌ Bad - not descriptive
function Form() { return <form>Login</form>; }
function Card() { return <div>Product</div>; }
function Menu() { return <nav>Menu</nav>; }
```

3. **Compound Component Names:**
```tsx
// ✅ Good
function UserList() { return <ul>Users</ul>; }
function UserListItem() { return <li>User</li>; }
function UserListHeader() { return <h3>Users</h3>; }

// ❌ Confusing
function List() { return <ul>Users</ul>; }
function Item() { return <li>User</li>; }
function Header() { return <h3>Users</h3>; }
```

4. **Abbreviations:**
```tsx
// ✅ Good - avoid abbreviations
function NavigationBar() { return <nav>Nav</nav>; }
function Authentication() { return <div>Auth</div>; }
function Configuration() { return <div>Config</div>; }

// Acceptable - common abbreviations
function NavBar() { return <nav>Nav</nav>; }
function AuthForm() { return <form>Auth</form>; }
function ConfigPanel() { return <div>Config</div>; }
```

5. **Context and Provider Naming:**
```tsx
// ✅ Good
function ThemeProvider({ children }: { children: React.ReactNode }) {
  return <div>{children}</div>;
}

function AuthContext() {
  return <div>Auth Context</div>;
}
```

**Naming Best Practices:**
- **Use PascalCase:** ComponentName
- **Be descriptive:** UserCard instead of Card
- **Noun-based:** Button, Form, List
- **Avoid abbreviations:** Navigation instead of Nav
- **Related components:** UserCard, UserList, UserForm
- **Context/Providers:** ThemeProvider, AuthContext

**Common Errors:**
- Starting with lowercase letter
- Using non-descriptive names
- Inconsistent naming patterns
- Using reserved words
- Confusing similar names

**Best Practices:**
- Always start with uppercase
- Use descriptive, meaningful names
- Follow naming conventions consistently
- Group related components with similar names
- Consider the component's purpose when naming

**Student Exercise:** Rename these components following React naming conventions:
- button
- user_info
- navBar
- product-card
- auth_context

---

### 29. Component Composition

**Definition:** Component composition is the practice of combining smaller components to build more complex UIs, promoting reusability and maintainability.

**Why we need it:**
- Breaks complex UIs into manageable pieces
- Promotes component reusability
- Makes code more maintainable
- Enables flexible UI construction
- Follows React's composition model

**How it works:**
- Components can contain other components
- Pass data through props
- Use children prop for flexible content
- Build complex UIs from simple building blocks

**Syntax:** Parent component contains child components.

**Simple Example:**
```tsx
function Button({ children }: { children: React.ReactNode }) {
  return <button>{children}</button>;
}

function App() {
  return (
    <div>
      <Button>Click me</Button>
      <Button>Submit</Button>
    </div>
  );
}
```

**Practical Examples:**

1. **Basic Composition:**
```tsx
function Card({ children }: { children: React.ReactNode }) {
  return <div className="card">{children}</div>;
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

2. **Multiple Components:**
```tsx
function Header() {
  return <header>Website Header</header>;
}

function Main() {
  return <main>Main Content</main>;
}

function Footer() {
  return <footer>Website Footer</footer>;
}

function App() {
  return (
    <div className="app">
      <Header />
      <Main />
      <Footer />
    </div>
  );
}
```

3. **Component with Slots:**
```tsx
interface LayoutProps {
  header: React.ReactNode;
  sidebar: React.ReactNode;
  content: React.ReactNode;
  footer: React.ReactNode;
}

function Layout({ header, sidebar, content, footer }: LayoutProps) {
  return (
    <div className="layout">
      <header>{header}</header>
      <div className="body">
        <aside>{sidebar}</aside>
        <main>{content}</main>
      </div>
      <footer>{footer}</footer>
    </div>
  );
}

function App() {
  return (
    <Layout
      header={<Header />}
      sidebar={<Sidebar />}
      content={<MainContent />}
      footer={<Footer />}
    />
  );
}
```

4. **Specialization:**
```tsx
function Button({ children, variant = 'primary' }: { children: React.ReactNode; variant?: 'primary' | 'secondary' }) {
  return <button className={`btn btn-${variant}`}>{children}</button>;
}

function PrimaryButton({ children }: { children: React.ReactNode }) {
  return <Button variant="primary">{children}</Button>;
}

function SecondaryButton({ children }: { children: React.ReactNode }) {
  return <Button variant="secondary">{children}</Button>;
}

function App() {
  return (
    <div>
      <PrimaryButton>Submit</PrimaryButton>
      <SecondaryButton>Cancel</SecondaryButton>
    </div>
  );
}
```

5. **Container Components:**
```tsx
function UserList({ users }: { users: { name: string; email: string }[] }) {
  return (
    <ul>
      {users.map((user, index) => (
        <UserListItem key={index} name={user.name} email={user.email} />
      ))}
    </ul>
  );
}

function UserListItem({ name, email }: { name: string; email: string }) {
  return (
    <li>
      <strong>{name}</strong> - {email}
    </li>
  );
}

function App() {
  const users = [
    { name: 'John', email: 'john@example.com' },
    { name: 'Jane', email: 'jane@example.com' }
  ];
  
  return <UserList users={users} />;
}
```

**Composition Patterns:**

1. **Container/Presenter:**
```tsx
// Container - handles logic
function UserListContainer() {
  const [users, setUsers] = useState([]);
  
  useEffect(() => {
    fetchUsers().then(setUsers);
  }, []);
  
  return <UserList users={users} />;
}

// Presenter - handles UI
function UserList({ users }: { users: any[] }) {
  return <ul>{/* render users */}</ul>;
}
```

2. **Higher-Order Components:**
```tsx
function withLoading<P>(Component: React.ComponentType<P>) {
  return (props: P & { loading?: boolean }) => {
    if (props.loading) return <div>Loading...</div>;
    return <Component {...props} />;
  };
}

const UserListWithLoading = withLoading(UserList);
```

**Common Errors:**
- Over-nesting components
- Not extracting reusable parts
- Tight coupling between components
- Prop drilling too deep

**Best Practices:**
- Keep components small and focused
- Extract reusable UI parts
- Use composition over inheritance
- Pass data through props
- Consider context for deep prop passing

**Student Question:** How does component composition differ from component inheritance?

---

### 30. Reusable Components

**Definition:** Reusable components are designed to be used in multiple places with different data, following the DRY (Don't Repeat Yourself) principle.

**Why we need them:**
- Avoid code duplication
- Consistent UI across the application
- Easier maintenance and updates
- Faster development
- Better testing

**How it works:**
- Design components to be flexible
- Use props for customization
- Make components generic enough for multiple uses
- Provide sensible defaults

**Syntax:** Components that accept props for customization.

**Simple Example:**
```tsx
function Button({ children, onClick }: { children: React.ReactNode; onClick: () => void }) {
  return <button onClick={onClick}>{children}</button>;
}

function App() {
  return (
    <div>
      <Button onClick={() => console.log('Clicked 1')}>Button 1</Button>
      <Button onClick={() => console.log('Clicked 2')}>Button 2</Button>
    </div>
  );
}
```

**Practical Examples:**

1. **Reusable Button:**
```tsx
interface ButtonProps {
  children: React.ReactNode;
  onClick?: () => void;
  variant?: 'primary' | 'secondary' | 'danger';
  size?: 'small' | 'medium' | 'large';
  disabled?: boolean;
}

function Button({ 
  children, 
  onClick, 
  variant = 'primary', 
  size = 'medium',
  disabled = false 
}: ButtonProps) {
  const baseClasses = 'btn';
  const variantClasses = `btn-${variant}`;
  const sizeClasses = `btn-${size}`;
  
  return (
    <button 
      className={`${baseClasses} ${variantClasses} ${sizeClasses}`}
      onClick={onClick}
      disabled={disabled}
    >
      {children}
    </button>
  );
}

// Usage
function App() {
  return (
    <div>
      <Button variant="primary" size="large">Submit</Button>
      <Button variant="secondary" size="medium">Cancel</Button>
      <Button variant="danger" size="small">Delete</Button>
    </div>
  );
}
```

2. **Reusable Card:**
```tsx
interface CardProps {
  title: string;
  children: React.ReactNode;
  footer?: React.ReactNode;
  className?: string;
}

function Card({ title, children, footer, className = '' }: CardProps) {
  return (
    <div className={`card ${className}`}>
      <div className="card-header">
        <h3>{title}</h3>
      </div>
      <div className="card-body">
        {children}
      </div>
      {footer && <div className="card-footer">{footer}</div>}
    </div>
  );
}

// Usage
function App() {
  return (
    <Card title="User Information" footer={<Button>Save</Button>}>
      <p>User details go here</p>
    </Card>
  );
}
```

3. **Reusable Input:**
```tsx
interface InputProps {
  label: string;
  type?: string;
  value: string;
  onChange: (value: string) => void;
  placeholder?: string;
  error?: string;
}

function Input({ 
  label, 
  type = 'text', 
  value, 
  onChange, 
  placeholder, 
  error 
}: InputProps) {
  return (
    <div className="form-group">
      <label htmlFor={label}>{label}</label>
      <input
        type={type}
        id={label}
        value={value}
        onChange={(e) => onChange(e.target.value)}
        placeholder={placeholder}
        className={error ? 'input-error' : ''}
      />
      {error && <span className="error-message">{error}</span>}
    </div>
  );
}

// Usage
function App() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  
  return (
    <form>
      <Input 
        label="Email" 
        type="email" 
        value={email} 
        onChange={setEmail}
        placeholder="Enter your email"
      />
      <Input 
        label="Password" 
        type="password" 
        value={password} 
        onChange={setPassword}
        placeholder="Enter your password"
      />
    </form>
  );
}
```

4. **Reusable Modal:**
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

// Usage
function App() {
  const [isModalOpen, setIsModalOpen] = useState(false);
  
  return (
    <div>
      <button onClick={() => setIsModalOpen(true)}>Open Modal</button>
      <Modal 
        isOpen={isModalOpen} 
        onClose={() => setIsModalOpen(false)}
        title="Example Modal"
      >
        <p>This is a reusable modal component</p>
      </Modal>
    </div>
  );
}
```

5. **Reusable List:**
```tsx
interface ListProps<T> {
  items: T[];
  renderItem: (item: T, index: number) => React.ReactNode;
  keyExtractor: (item: T, index: number) => string;
  emptyMessage?: string;
}

function List<T>({ items, renderItem, keyExtractor, emptyMessage = 'No items' }: ListProps<T>) {
  if (items.length === 0) {
    return <p className="empty-message">{emptyMessage}</p>;
  }
  
  return (
    <ul className="list">
      {items.map((item, index) => (
        <li key={keyExtractor(item, index)}>
          {renderItem(item, index)}
        </li>
      ))}
    </ul>
  );
}

// Usage
function App() {
  const users = [
    { id: 1, name: 'John', email: 'john@example.com' },
    { id: 2, name: 'Jane', email: 'jane@example.com' }
  ];
  
  return (
    <List
      items={users}
      renderItem={(user) => (
        <div>
          <strong>{user.name}</strong>
          <span>{user.email}</span>
        </div>
      )}
      keyExtractor={(user) => user.id.toString()}
    />
  );
}
```

**Reusable Component Best Practices:**
- **Flexible Props:** Accept customization through props
- **Default Values:** Provide sensible defaults
- **TypeScript:** Use interfaces for type safety
- **Documentation:** Document prop usage
- **Composition:** Allow children for flexibility
- **Validation:** Validate prop values when needed

**Common Errors:**
- Making components too specific
- Not providing default values
- Poor prop naming
- Not handling edge cases
- Over-complicating simple components

**Best Practices:**
- Start generic, specialize when needed
- Use TypeScript for prop validation
- Provide good default values
- Document component usage
- Keep components focused
- Test components in isolation

**Student Exercise:** Create a reusable Badge component that can display different colors and sizes based on props.

---

## Practical Project: Landing Page

Now let's build a complete landing page using all the concepts we've learned. We'll create a modern landing page with Header, Hero, Features, Product Cards, and Footer.

### Step 1: Project Setup

```bash
# Create new project
npm create vite@latest landing-page -- --template react-ts

# Navigate to project
cd landing-page

# Install dependencies
npm install

# Start development server
npm run dev
```

### Step 2: Project Structure

Create the following structure:
```
src/
├── components/
│   ├── Header.tsx
│   ├── Hero.tsx
│   ├── Features.tsx
│   ├── ProductCard.tsx
│   └── Footer.tsx
├── App.tsx
├── main.tsx
└── index.css
```

### Step 3: Basic Styling (index.css)

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Arial', sans-serif;
  line-height: 1.6;
  color: #333;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.btn {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.btn-primary {
  background-color: #007bff;
  color: white;
}

.btn-primary:hover {
  background-color: #0056b3;
}

.btn-secondary {
  background-color: #6c757d;
  color: white;
}

.btn-secondary:hover {
  background-color: #545b62;
}
```

### Step 4: Header Component

```tsx
// src/components/Header.tsx
interface HeaderProps {
  onNavigate: (page: string) => void;
}

function Header({ onNavigate }: HeaderProps) {
  return (
    <header className="header">
      <div className="container">
        <nav className="nav">
          <div className="logo">
            <h1>BrandName</h1>
          </div>
          <ul className="nav-menu">
            <li>
              <button onClick={() => onNavigate('home')} className="nav-link">
                Home
              </button>
            </li>
            <li>
              <button onClick={() => onNavigate('features')} className="nav-link">
                Features
              </button>
            </li>
            <li>
              <button onClick={() => onNavigate('products')} className="nav-link">
                Products
              </button>
            </li>
            <li>
              <button onClick={() => onNavigate('contact')} className="nav-link">
                Contact
              </button>
            </li>
          </ul>
        </nav>
      </div>
    </header>
  );
}

export default Header;
```

### Step 5: Hero Component

```tsx
// src/components/Hero.tsx
interface HeroProps {
  onCtaClick: () => void;
}

function Hero({ onCtaClick }: HeroProps) {
  return (
    <section className="hero">
      <div className="container">
        <div className="hero-content">
          <h2 className="hero-title">Build Amazing Products</h2>
          <p className="hero-description">
            Create stunning web applications with modern tools and best practices.
            Start your journey today and transform your ideas into reality.
          </p>
          <div className="hero-buttons">
            <button onClick={onCtaClick} className="btn btn-primary">
              Get Started
            </button>
            <button className="btn btn-secondary">
              Learn More
            </button>
          </div>
        </div>
      </div>
    </section>
  );
}

export default Hero;
```

Add CSS for Hero:
```css
.hero {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 100px 0;
  text-align: center;
}

.hero-title {
  font-size: 48px;
  margin-bottom: 20px;
}

.hero-description {
  font-size: 18px;
  margin-bottom: 30px;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}

.hero-buttons {
  display: flex;
  gap: 15px;
  justify-content: center;
}

.hero-buttons .btn {
  color: white;
}
```

### Step 6: Features Component

```tsx
// src/components/Features.tsx
interface Feature {
  icon: string;
  title: string;
  description: string;
}

function Features() {
  const features: Feature[] = [
    {
      icon: '⚡',
      title: 'Fast Performance',
      description: 'Optimized for speed with modern build tools and efficient rendering.'
    },
    {
      icon: '🎨',
      title: 'Beautiful Design',
      description: 'Modern and responsive design that looks great on all devices.'
    },
    {
      icon: '🔒',
      title: 'Secure',
      description: 'Built with security best practices to protect your data.'
    },
    {
      icon: '📱',
      title: 'Mobile First',
      description: 'Responsive design that works perfectly on mobile devices.'
    },
    {
      icon: '🛠️',
      title: 'Easy to Use',
      description: 'Intuitive interface designed for the best user experience.'
    },
    {
      icon: '🌐',
      title: 'Global Reach',
      description: 'Support for multiple languages and international markets.'
    }
  ];

  return (
    <section className="features">
      <div className="container">
        <h2 className="section-title">Our Features</h2>
        <div className="features-grid">
          {features.map((feature, index) => (
            <div key={index} className="feature-card">
              <div className="feature-icon">{feature.icon}</div>
              <h3 className="feature-title">{feature.title}</h3>
              <p className="feature-description">{feature.description}</p>
            </div>
          ))}
        </div>
      </div>
    </section>
  );
}

export default Features;
```

Add CSS for Features:
```css
.features {
  padding: 80px 0;
  background-color: #f8f9fa;
}

.section-title {
  text-align: center;
  font-size: 36px;
  margin-bottom: 50px;
  color: #333;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 30px;
}

.feature-card {
  background: white;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  text-align: center;
  transition: transform 0.3s;
}

.feature-card:hover {
  transform: translateY(-5px);
}

.feature-icon {
  font-size: 48px;
  margin-bottom: 20px;
}

.feature-title {
  font-size: 24px;
  margin-bottom: 15px;
  color: #333;
}

.feature-description {
  color: #666;
  line-height: 1.6;
}
```

### Step 7: ProductCard Component

```tsx
// src/components/ProductCard.tsx
interface Product {
  id: number;
  name: string;
  price: number;
  description: string;
  image: string;
  rating: number;
}

interface ProductCardProps {
  product: Product;
  onAddToCart: (product: Product) => void;
}

function ProductCard({ product, onAddToCart }: ProductCardProps) {
  return (
    <div className="product-card">
      <div className="product-image">
        <img src={product.image} alt={product.name} />
      </div>
      <div className="product-info">
        <h3 className="product-name">{product.name}</h3>
        <p className="product-description">{product.description}</p>
        <div className="product-meta">
          <span className="product-price">${product.price}</span>
          <span className="product-rating">★ {product.rating}</span>
        </div>
        <button 
          onClick={() => onAddToCart(product)}
          className="btn btn-primary add-to-cart"
        >
          Add to Cart
        </button>
      </div>
    </div>
  );
}

export default ProductCard;
```

Add CSS for ProductCard:
```css
.product-card {
  background: white;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s;
}

.product-card:hover {
  transform: translateY(-5px);
}

.product-image {
  height: 200px;
  overflow: hidden;
}

.product-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.product-info {
  padding: 20px;
}

.product-name {
  font-size: 20px;
  margin-bottom: 10px;
  color: #333;
}

.product-description {
  color: #666;
  margin-bottom: 15px;
  line-height: 1.5;
}

.product-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.product-price {
  font-size: 24px;
  font-weight: bold;
  color: #007bff;
}

.product-rating {
  color: #ffc107;
}

.add-to-cart {
  width: 100%;
}
```

### Step 8: Products Section Component

```tsx
// src/components/Products.tsx
import ProductCard from './ProductCard';

interface Product {
  id: number;
  name: string;
  price: number;
  description: string;
  image: string;
  rating: number;
}

function Products() {
  const products: Product[] = [
    {
      id: 1,
      name: 'Premium Package',
      price: 99,
      description: 'Complete solution for businesses with advanced features.',
      image: 'https://via.placeholder.com/300x200',
      rating: 4.5
    },
    {
      id: 2,
      name: 'Standard Package',
      price: 49,
      description: 'Perfect for small teams and growing businesses.',
      image: 'https://via.placeholder.com/300x200',
      rating: 4.0
    },
    {
      id: 3,
      name: 'Basic Package',
      price: 19,
      description: 'Great for individuals and freelancers.',
      image: 'https://via.placeholder.com/300x200',
      rating: 3.5
    },
    {
      id: 4,
      name: 'Enterprise Package',
      price: 199,
      description: 'Custom solutions for large organizations.',
      image: 'https://via.placeholder.com/300x200',
      rating: 5.0
    }
  ];

  const handleAddToCart = (product: Product) => {
    console.log('Added to cart:', product.name);
    alert(`${product.name} added to cart!`);
  };

  return (
    <section className="products">
      <div className="container">
        <h2 className="section-title">Our Products</h2>
        <div className="products-grid">
          {products.map((product) => (
            <ProductCard 
              key={product.id} 
              product={product} 
              onAddToCart={handleAddToCart}
            />
          ))}
        </div>
      </div>
    </section>
  );
}

export default Products;
```

Add CSS for Products:
```css
.products {
  padding: 80px 0;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 30px;
}
```

### Step 9: Footer Component

```tsx
// src/components/Footer.tsx
function Footer() {
  const currentYear = new Date().getFullYear();

  return (
    <footer className="footer">
      <div className="container">
        <div className="footer-content">
          <div className="footer-section">
            <h3>About Us</h3>
            <p>
              We are dedicated to providing the best solutions for your business needs.
              Our team of experts is here to help you succeed.
            </p>
          </div>
          <div className="footer-section">
            <h3>Quick Links</h3>
            <ul className="footer-links">
              <li><a href="#home">Home</a></li>
              <li><a href="#features">Features</a></li>
              <li><a href="#products">Products</a></li>
              <li><a href="#contact">Contact</a></li>
            </ul>
          </div>
          <div className="footer-section">
            <h3>Contact Us</h3>
            <ul className="contact-info">
              <li>📧 info@example.com</li>
              <li>📞 +1 (555) 123-4567</li>
              <li>📍 123 Business Street, City</li>
            </ul>
          </div>
          <div className="footer-section">
            <h3>Follow Us</h3>
            <div className="social-links">
              <a href="#facebook">Facebook</a>
              <a href="#twitter">Twitter</a>
              <a href="#linkedin">LinkedIn</a>
              <a href="#instagram">Instagram</a>
            </div>
          </div>
        </div>
        <div className="footer-bottom">
          <p>&copy; {currentYear} BrandName. All rights reserved.</p>
        </div>
      </div>
    </footer>
  );
}

export default Footer;
```

Add CSS for Footer:
```css
.footer {
  background-color: #333;
  color: white;
  padding: 60px 0 20px;
}

.footer-content {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 40px;
  margin-bottom: 40px;
}

.footer-section h3 {
  margin-bottom: 20px;
  color: #fff;
}

.footer-links, .contact-info {
  list-style: none;
}

.footer-links li, .contact-info li {
  margin-bottom: 10px;
}

.footer-links a, .contact-info {
  color: #ccc;
  text-decoration: none;
  transition: color 0.3s;
}

.footer-links a:hover {
  color: #fff;
}

.social-links {
  display: flex;
  gap: 15px;
  flex-wrap: wrap;
}

.social-links a {
  color: #ccc;
  text-decoration: none;
  transition: color 0.3s;
}

.social-links a:hover {
  color: #fff;
}

.footer-bottom {
  text-align: center;
  padding-top: 20px;
  border-top: 1px solid #444;
  color: #ccc;
}
```

### Step 10: Main App Component

```tsx
// src/App.tsx
import { useState } from 'react';
import Header from './components/Header';
import Hero from './components/Hero';
import Features from './components/Features';
import Products from './components/Products';
import Footer from './components/Footer';

function App() {
  const [currentPage, setCurrentPage] = useState('home');

  const handleNavigate = (page: string) => {
    setCurrentPage(page);
  };

  const handleCtaClick = () => {
    console.log('CTA button clicked!');
    alert('Welcome to our platform!');
  };

  return (
    <div className="app">
      <Header onNavigate={handleNavigate} />
      <main>
        <Hero onCtaClick={handleCtaClick} />
        <Features />
        <Products />
      </main>
      <Footer />
    </div>
  );
}

export default App;
```

Add CSS for App:
```css
.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

main {
  flex: 1;
}

.header {
  background-color: white;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  position: sticky;
  top: 0;
  z-index: 1000;
}

.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
}

.logo h1 {
  font-size: 24px;
  color: #007bff;
}

.nav-menu {
  display: flex;
  list-style: none;
  gap: 30px;
}

.nav-link {
  background: none;
  border: none;
  font-size: 16px;
  color: #333;
  cursor: pointer;
  transition: color 0.3s;
}

.nav-link:hover {
  color: #007bff;
}
```

### Step 11: Navigation Styling

Add responsive navigation styles:
```css
@media (max-width: 768px) {
  .nav {
    flex-direction: column;
    gap: 20px;
  }

  .nav-menu {
    flex-direction: column;
    gap: 15px;
    text-align: center;
  }

  .hero-title {
    font-size: 36px;
  }

  .hero-description {
    font-size: 16px;
  }

  .hero-buttons {
    flex-direction: column;
  }

  .features-grid,
  .products-grid {
    grid-template-columns: 1fr;
  }

  .footer-content {
    grid-template-columns: 1fr;
  }
}
```

### Step 12: Testing the Application

```bash
# Make sure the dev server is running
npm run dev
```

Open your browser and navigate to `http://localhost:5173` to see your landing page!

### Step 13: Building for Production

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

1. **React Fundamentals:** What React is, why it's popular, and how it differs from traditional JavaScript
2. **Development Setup:** Installing Node.js, creating projects with Vite, understanding project structure
3. **JSX Essentials:** JSX syntax, differences from HTML, expressions, and best practices
4. **Component Basics:** Functional components, naming conventions, and composition
5. **Practical Application:** Built a complete landing page with reusable components

### Key Takeaways

- React is a library, not a framework - you choose the tools you need
- JSX looks like HTML but is actually JavaScript
- Components are the building blocks of React applications
- Functional components with hooks are the modern standard
- Composition is preferred over inheritance
- Reusable components follow the DRY principle

---

## 15 Student Questions

1. What is React and why is it popular?
2. What's the difference between a library and a framework?
3. Explain the concept of Single Page Applications (SPA).
4. What is Client-Side Rendering (CSR)?
5. How does React differ from vanilla JavaScript?
6. When would you choose Next.js over plain React?
7. Why do we need Node.js for React development?
8. What is Vite and why is it used for React projects?
9. Explain the purpose of package.json in a React project.
10. What's the difference between the src and public folders?
11. What is the role of main.tsx in a React application?
12. What is JSX and how does it differ from HTML?
13. Why do we use className instead of class in JSX?
14. What are React Fragments and when would you use them?
15. What are the benefits of using functional components over class components?

---

## 5 Interview Questions

1. **Explain the Virtual DOM and how React uses it for performance optimization.**
   - Answer: The Virtual DOM is a lightweight JavaScript representation of the actual DOM. React creates a virtual DOM tree, compares it with the previous version, and only updates the parts that have changed in the real DOM, minimizing expensive DOM operations.

2. **What is the difference between state and props in React?**
   - Answer: Props are passed from parent to child components and are immutable. State is managed within a component and can be changed using setState. Props are for configuration, state is for internal component data.

3. **Explain the component lifecycle in functional components.**
   - Answer: Functional components use hooks to manage lifecycle. useEffect with empty dependency array runs on mount, return function runs on unmount, and dependency array controls when effects re-run. useState manages component state.

4. **What are the key differences between JSX and HTML?**
   - Answer: JSX uses className instead of class, htmlFor instead of for, self-closing tags need />, inline styles use objects with camelCase properties, and JavaScript expressions are embedded in curly braces.

5. **How do you optimize React application performance?**
   - Answer: Use React.memo for expensive components, implement code splitting with lazy loading, use useMemo and useCallback for expensive computations, avoid unnecessary re-renders, and optimize bundle size.

---

## 3 Practical Exercises

### Exercise 1: Interactive Counter Component
Create a counter component with increment, decrement, and reset buttons. Display the current count and show a message when the count reaches certain milestones (e.g., "Great job!" at 10, "Amazing!" at 20).

### Exercise 2: Dynamic Form Component
Build a form component with validation. Include fields for name, email, and password. Show error messages for invalid inputs and display the submitted data when the form is valid.

### Exercise 3: Theme Switcher Component
Create a theme switcher that allows users to toggle between light and dark themes. Apply the theme to the entire application and persist the user's preference using localStorage.

---

## Homework

1. **Reading Assignment:** Read the official React documentation on "Thinking in React" and "JSX In Depth"
2. **Practice Exercise:** Enhance the landing page by adding:
   - A contact form section
   - Testimonials section
   - Smooth scrolling navigation
   - Mobile-responsive hamburger menu
3. **Component Library:** Create a small library of reusable components (Button, Input, Card, Modal) with proper TypeScript interfaces
4. **Research:** Look into different styling approaches in React (CSS Modules, Styled Components, Tailwind CSS) and write a brief comparison

---

## Key Points to Remember

### JSX Rules
- Always use className instead of class
- Use htmlFor instead of for
- Self-close void elements with />
- Wrap multiple elements in Fragment or a div
- Use camelCase for inline styles

### Component Best Practices
- Use functional components with hooks
- Start component names with uppercase letters
- Keep components small and focused
- Use TypeScript interfaces for props
- Follow the DRY principle with reusable components

### Project Structure
- Source code goes in src/
- Static assets go in public/
- Components in components/ folder
- Main entry point is main.tsx
- Root component is App.tsx

### Development Workflow
- Use npm create vite@latest for new projects
- npm run dev for development
- npm run build for production
- npm run preview to test production build

---

## Next Session Concepts

In the next session, we will cover:

1. **State Management with useState:** Deep dive into React state
2. **Props and Prop Drilling:** Passing data between components
3. **Conditional Rendering:** Rendering based on conditions
4. **List Rendering:** Rendering arrays of data
5. **Event Handling:** User interactions in React
6. **Forms and Controlled Components:** Form handling patterns
7. **useEffect Hook:** Side effects and lifecycle
8. **Custom Hooks:** Creating reusable logic
9. **Context API:** Global state management
10. **React Router:** Client-side routing

**Preparation:** Review the concepts from this session and practice building small components to prepare for the more advanced topics in the next session.

---

## Additional Resources

- [Official React Documentation](https://react.dev)
- [TypeScript Documentation](https://www.typescriptlang.org/docs)
- [Vite Documentation](https://vitejs.dev)
- [MDN Web Docs - JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [CSS Tricks - Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [CSS Tricks - Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)

---

**Congratulations on completing Session 1!** You now have a solid foundation in React and are ready to build more complex applications. Keep practicing and experimenting with the concepts you've learned.