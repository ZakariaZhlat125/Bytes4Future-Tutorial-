# React.js Session 5: API Integration

**Duration:** 3 hours  
**Level:** Beginner  
**Prerequisites:** Session 1-4 completed (Components, Props, Events, State, useState, useEffect, Forms)

---

## Session Timeline

### Part 1: API Fundamentals (45 minutes)
- **0:00-0:05:** What is API?, REST API (Topics 1-2)
- **0:05-0:10:** HTTP Methods (Topic 3)
- **0:10-0:15:** GET, POST (Topics 4-5)
- **0:15-0:20:** PUT, PATCH (Topics 6-7)
- **0:20-0:25:** DELETE (Topic 8)
- **0:25-0:30:** HTTP Status Codes (Topic 9)
- **0:30-0:35:** JSON (Topic 10)
- **0:35-0:45:** API Fundamentals Practice

### Part 2: Fetch API & Async/Await (30 minutes)
- **0:45-0:50:** Fetch API (Topic 11)
- **0:50-0:55:** async/await (Topic 12)
- **0:55-1:00:** try/catch (Topic 13)
- **1:00-1:15:** Fetch & Async/Await Practice

### Part 3: API States & useEffect (30 minutes)
- **1:15-1:20:** Loading State (Topic 14)
- **1:20-1:25:** Error State (Topic 15)
- **1:25-1:30:** Success State (Topic 16)
- **1:30-1:35:** Empty State (Topic 17)
- **1:35-1:40:** useEffect + API (Topic 18)
- **1:40-1:45:** API Lifecycle Explanation
- **1:45-1:15:** API States Practice

### Part 4: Axios (30 minutes)
- **1:45-1:50:** Axios Introduction (Topic 19)
- **1:50-1:55:** Axios Instance (Topic 20)
- **1:55-2:00:** Base URL (Topic 21)
- **2:00-2:05:** Headers (Topic 22)
- **2:05-2:10:** Authorization Header (Topic 23)
- **2:10-2:15:** Axios Practice

### Part 5: Custom Hooks & Separation of Concerns (30 minutes)
- **2:15-2:20:** API Services (Topic 24)
- **2:20-2:25:** Custom Hooks (Topic 25)
- **2:25-2:30:** useFetch Hook (Topic 26)
- **2:30-2:35:** Separation of Concerns (Topic 27)
- **2:35-2:45:** Custom Hooks Practice

### Part 6: Products Application Project (30 minutes)
- **2:45-3:00:** Build Products Application with professional structure
- **3:00-3:15:** Implementation of product.service.ts, useFetch.ts, components

---

## Part 1: API Fundamentals

### 1. What is API?

**What is it?**
API (Application Programming Interface) is a set of rules and protocols that allows different software applications to communicate with each other.

**Why we need it?**
- Enable communication between systems
- Access external data and services
- Integrate with third-party services
- Build scalable applications
- Standardize data exchange

**How it works?**
- Client sends request to server
- Server processes request
- Server returns response
- Client handles response
- Data exchanged in standard format

**Simple Example:**
```typescript
// Client requests data
fetch('https://api.example.com/users')
  .then(response => response.json())
  .then(data => console.log(data));

// Server responds with JSON
{
  "users": [
    { "id": 1, "name": "John" },
    { "id": 2, "name": "Jane" }
  ]
}
```

**Real-world Example:**
```typescript
// Weather API example
interface WeatherResponse {
  location: {
    name: string;
    country: string;
  };
  current: {
    temp_c: number;
    condition: {
      text: string;
    };
  };
}

async function getWeather(city: string): Promise<WeatherResponse> {
  const response = await fetch(
    `https://api.weatherapi.com/v1/current.json?key=YOUR_KEY&q=${city}`
  );
  
  if (!response.ok) {
    throw new Error('Failed to fetch weather data');
  }
  
  return response.json();
}

// Usage
getWeather('London')
  .then(weather => {
    console.log(`Weather in ${weather.location.name}:`);
    console.log(`Temperature: ${weather.current.temp_c}°C`);
    console.log(`Condition: ${weather.current.condition.text}`);
  })
  .catch(error => {
    console.error('Error:', error.message);
  });
```

**Types of APIs:**
- **REST API:** Most common, uses HTTP methods
- **GraphQL:** Query language for APIs
- **SOAP:** XML-based protocol
- **WebSocket:** Real-time communication
- **gRPC:** High-performance RPC framework

**Common API Use Cases:**
- Fetching user data
- Submitting forms
- File uploads
- Real-time updates
- Third-party integrations

---

### 2. REST API

**What is it?**
REST (Representational State Transfer) is an architectural style for designing networked applications using HTTP requests to access and manipulate data.

**Why we need it?**
- Standardized approach to API design
- Stateless and scalable
- Uses existing HTTP infrastructure
- Platform-independent
- Easy to understand and implement

**How it works?**
- Resources identified by URLs
- Standard HTTP methods for operations
- Stateless communication
- JSON for data exchange
- Uniform interface

**REST Principles:**
1. **Client-Server:** Separation of concerns
2. **Stateless:** No client context stored on server
3. **Cacheable:** Responses can be cached
4. **Uniform Interface:** Consistent API design
5. **Layered System:** Multiple layers possible
6. **Code on Demand:** Optional downloadable code

**Simple Example:**
```typescript
// REST API endpoints
GET    /api/users          // Get all users
GET    /api/users/1        // Get user with ID 1
POST   /api/users          // Create new user
PUT    /api/users/1        // Update user with ID 1
PATCH  /api/users/1        // Partially update user
DELETE /api/users/1        // Delete user with ID 1
```

**Real-world Example:**
```typescript
// REST API for a blog application
interface BlogPost {
  id: number;
  title: string;
  content: string;
  author: {
    id: number;
    name: string;
  };
  createdAt: string;
  updatedAt: string;
}

// API Service
class BlogService {
  private baseUrl = 'https://api.blog.com/v1';

  async getPosts(): Promise<BlogPost[]> {
    const response = await fetch(`${this.baseUrl}/posts`);
    return response.json();
  }

  async getPost(id: number): Promise<BlogPost> {
    const response = await fetch(`${this.baseUrl}/posts/${id}`);
    return response.json();
  }

  async createPost(post: Omit<BlogPost, 'id' | 'createdAt' | 'updatedAt'>): Promise<BlogPost> {
    const response = await fetch(`${this.baseUrl}/posts`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(post)
    });
    return response.json();
  }

  async updatePost(id: number, updates: Partial<BlogPost>): Promise<BlogPost> {
    const response = await fetch(`${this.baseUrl}/posts/${id}`, {
      method: 'PATCH',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(updates)
    });
    return response.json();
  }

  async deletePost(id: number): Promise<void> {
    await fetch(`${this.baseUrl}/posts/${id}`, {
      method: 'DELETE'
    });
  }
}
```

**REST Best Practices:**
- Use nouns for resource names (/users, not /getUsers)
- Use plural nouns for collections
- Use HTTP methods appropriately
- Use status codes correctly
- Version your API (/v1, /v2)
- Use filtering, sorting, pagination
- Provide consistent error responses

---

### 3. HTTP Methods

**What is it?**
HTTP methods (also called verbs) indicate the desired action to be performed on a resource in REST APIs.

**Why we need it?**
- Standardized operations on resources
- Clear intent of each request
- Enables proper caching
- Idempotent operations
- RESTful API design

**How it works?**
- Specified in request line
- Server responds accordingly
- Different methods have different semantics
- Some methods are idempotent
- Browser handles certain methods specially

**HTTP Methods Overview:**

| Method | Description | Idempotent | Safe | Body |
|--------|-------------|------------|------|------|
| GET | Retrieve resource | Yes | Yes | No |
| POST | Create resource | No | No | Yes |
| PUT | Replace resource | Yes | No | Yes |
| PATCH | Partial update | No | No | Yes |
| DELETE | Remove resource | Yes | No | No |

**Simple Example:**
```typescript
// Different HTTP methods
const api = 'https://api.example.com/users';

// GET - Retrieve data
fetch(api); // GET /users

// POST - Create data
fetch(api, {
  method: 'POST',
  body: JSON.stringify({ name: 'John' })
});

// PUT - Replace data
fetch(`${api}/1`, {
  method: 'PUT',
  body: JSON.stringify({ id: 1, name: 'Jane' })
});

// PATCH - Partial update
fetch(`${api}/1`, {
  method: 'PATCH',
  body: JSON.stringify({ name: 'Jane' })
});

// DELETE - Remove data
fetch(`${api}/1`, { method: 'DELETE' });
```

**Real-world Example:**
```typescript
interface User {
  id: number;
  name: string;
  email: string;
  role: 'admin' | 'user';
}

class UserService {
  private baseUrl = 'https://api.example.com/users';

  // GET - Retrieve all users
  async getAllUsers(): Promise<User[]> {
    const response = await fetch(this.baseUrl);
    if (!response.ok) throw new Error('Failed to fetch users');
    return response.json();
  }

  // GET - Retrieve single user
  async getUserById(id: number): Promise<User> {
    const response = await fetch(`${this.baseUrl}/${id}`);
    if (!response.ok) throw new Error('User not found');
    return response.json();
  }

  // POST - Create new user
  async createUser(user: Omit<User, 'id'>): Promise<User> {
    const response = await fetch(this.baseUrl, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(user)
    });
    if (!response.ok) throw new Error('Failed to create user');
    return response.json();
  }

  // PUT - Replace entire user
  async replaceUser(id: number, user: User): Promise<User> {
    const response = await fetch(`${this.baseUrl}/${id}`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(user)
    });
    if (!response.ok) throw new Error('Failed to replace user');
    return response.json();
  }

  // PATCH - Partially update user
  async updateUser(id: number, updates: Partial<User>): Promise<User> {
    const response = await fetch(`${this.baseUrl}/${id}`, {
      method: 'PATCH',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(updates)
    });
    if (!response.ok) throw new Error('Failed to update user');
    return response.json();
  }

  // DELETE - Remove user
  async deleteUser(id: number): Promise<void> {
    const response = await fetch(`${this.baseUrl}/${id}`, {
      method: 'DELETE'
    });
    if (!response.ok) throw new Error('Failed to delete user');
  }
}
```

**Method Characteristics:**

**GET:**
- Safe: Doesn't modify server state
- Idempotent: Multiple requests same as one
- Cacheable: Can be cached by browsers
- No body: Data in URL parameters

**POST:**
- Not safe: Creates new resource
- Not idempotent: Multiple requests create multiple resources
- Not cacheable: Not cached by default
- Has body: Data in request body

**PUT:**
- Not safe: Replaces resource
- Idempotent: Multiple requests same result
- Not cacheable: Not cached by default
- Has body: Complete resource data

**PATCH:**
- Not safe: Modifies resource
- Not idempotent: Depends on implementation
- Not cacheable: Not cached by default
- Has body: Partial resource data

**DELETE:**
- Not safe: Removes resource
- Idempotent: Multiple deletes same result
- Not cacheable: Not cached by default
- No body: Resource identified by URL

---

### 4. GET

**What is it?**
GET is an HTTP method used to retrieve data from a server. It's the most common HTTP method for fetching resources.

**Why we need it?**
- Retrieve data from server
- Safe operation (no modifications)
- Cacheable by browsers
- Idempotent (safe to retry)
- Standard for read operations

**How it works?**
- Client sends GET request
- Server returns resource data
- Data sent in response body
- Can include query parameters
- Browser caches responses

**Syntax:**
```typescript
// Basic GET request
fetch('https://api.example.com/users')
  .then(response => response.json())
  .then(data => console.log(data));

// GET with query parameters
fetch('https://api.example.com/users?page=1&limit=10')
  .then(response => response.json())
  .then(data => console.log(data));
```

**Simple Example:**
```typescript
async function fetchUsers() {
  const response = await fetch('https://api.example.com/users');
  const users = await response.json();
  return users;
}

// Usage
fetchUsers().then(users => console.log(users));
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

interface ProductListResponse {
  products: Product[];
  total: number;
  page: number;
  limit: number;
}

class ProductService {
  private baseUrl = 'https://api.example.com/products';

  // GET all products
  async getProducts(params?: {
    page?: number;
    limit?: number;
    category?: string;
    search?: string;
  }): Promise<ProductListResponse> {
    const url = new URL(this.baseUrl);
    
    if (params?.page) url.searchParams.append('page', params.page.toString());
    if (params?.limit) url.searchParams.append('limit', params.limit.toString());
    if (params?.category) url.searchParams.append('category', params.category);
    if (params?.search) url.searchParams.append('search', params.search);

    const response = await fetch(url.toString());
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    return response.json();
  }

  // GET single product
  async getProduct(id: number): Promise<Product> {
    const response = await fetch(`${this.baseUrl}/${id}`);
    
    if (!response.ok) {
      throw new Error(`Product not found: ${id}`);
    }
    
    return response.json();
  }

  // GET with filtering
  async getProductsByCategory(category: string): Promise<Product[]> {
    const response = await fetch(`${this.baseUrl}?category=${category}`);
    
    if (!response.ok) {
      throw new Error(`Failed to fetch products in category: ${category}`);
    }
    
    return response.json();
  }

  // GET with search
  async searchProducts(query: string): Promise<Product[]> {
    const response = await fetch(`${this.baseUrl}/search?q=${encodeURIComponent(query)}`);
    
    if (!response.ok) {
      throw new Error(`Search failed for query: ${query}`);
    }
    
    return response.json();
  }
}

// Usage examples
const productService = new ProductService();

// Get all products
productService.getProducts()
  .then(result => console.log(`Found ${result.total} products`));

// Get products with pagination
productService.getProducts({ page: 1, limit: 10 })
  .then(result => console.log(`Page ${result.page}: ${result.products.length} products`));

// Get products by category
productService.getProductsByCategory('electronics')
  .then(products => console.log(`Found ${products.length} electronics`));

// Search products
productService.searchProducts('laptop')
  .then(products => console.log(`Found ${products.length} laptops`));
```

**GET Best Practices:**
- Use for read operations only
- Include query parameters for filtering
- Implement pagination for large datasets
- Cache responses when appropriate
- Handle 404 Not Found gracefully
- Use proper error handling

---

### 5. POST

**What is it?**
POST is an HTTP method used to create new resources on the server. It sends data in the request body.

**Why we need it?**
- Create new resources
- Submit form data
- Upload files
- Trigger server-side processing
- Non-idempotent operations

**How it works?**
- Client sends POST request with data
- Server processes data
- Server creates new resource
- Server returns created resource
- Usually returns 201 Created status

**Syntax:**
```typescript
fetch('https://api.example.com/users', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ name: 'John', email: 'john@example.com' })
})
  .then(response => response.json())
  .then(data => console.log(data));
```

**Simple Example:**
```typescript
async function createUser(userData: { name: string; email: string }) {
  const response = await fetch('https://api.example.com/users', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(userData)
  });
  
  if (!response.ok) {
    throw new Error('Failed to create user');
  }
  
  return response.json();
}

// Usage
createUser({ name: 'John', email: 'john@example.com' })
  .then(user => console.log('Created user:', user));
```

**Real-world Example:**
```typescript
interface CreateProductRequest {
  name: string;
  description: string;
  price: number;
  category: string;
  stock: number;
}

interface Product {
  id: number;
  name: string;
  description: string;
  price: number;
  category: string;
  stock: number;
  createdAt: string;
  updatedAt: string;
}

class ProductService {
  private baseUrl = 'https://api.example.com/products';

  async createProduct(productData: CreateProductRequest): Promise<Product> {
    // Validate input
    if (!productData.name || !productData.name.trim()) {
      throw new Error('Product name is required');
    }
    
    if (productData.price <= 0) {
      throw new Error('Price must be greater than 0');
    }

    const response = await fetch(this.baseUrl, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${this.getAuthToken()}`
      },
      body: JSON.stringify(productData)
    });

    if (response.status === 400) {
      const error = await response.json();
      throw new Error(error.message || 'Validation error');
    }

    if (response.status === 401) {
      throw new Error('Unauthorized: Please login');
    }

    if (!response.ok) {
      throw new Error(`Failed to create product: ${response.statusText}`);
    }

    // 201 Created - return created resource
    const createdProduct: Product = await response.json();
    
    console.log(`Product created with ID: ${createdProduct.id}`);
    return createdProduct;
  }

  async createProductWithImage(
    productData: CreateProductRequest,
    imageFile: File
  ): Promise<Product> {
    const formData = new FormData();
    
    formData.append('name', productData.name);
    formData.append('description', productData.description);
    formData.append('price', productData.price.toString());
    formData.append('category', productData.category);
    formData.append('stock', productData.stock.toString());
    formData.append('image', imageFile);

    const response = await fetch(`${this.baseUrl}/with-image`, {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${this.getAuthToken()}`
        // Don't set Content-Type - browser sets it with boundary
      },
      body: formData
    });

    if (!response.ok) {
      throw new Error('Failed to create product with image');
    }

    return response.json();
  }

  async bulkCreateProducts(products: CreateProductRequest[]): Promise<Product[]> {
    const response = await fetch(`${this.baseUrl}/bulk`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${this.getAuthToken()}`
      },
      body: JSON.stringify({ products })
    });

    if (!response.ok) {
      throw new Error('Failed to create products in bulk');
    }

    return response.json();
  }

  private getAuthToken(): string {
    // Get token from localStorage or cookie
    return localStorage.getItem('authToken') || '';
  }
}

// Usage examples
const productService = new ProductService();

// Create single product
productService.createProduct({
  name: 'Wireless Headphones',
  description: 'High-quality wireless headphones',
  price: 99.99,
  category: 'electronics',
  stock: 50
})
  .then(product => console.log('Created:', product))
  .catch(error => console.error('Error:', error.message));

// Create product with image
const imageFile = new File([''], 'product.jpg', { type: 'image/jpeg' });
productService.createProductWithImage(
  {
    name: 'Smart Watch',
    description: 'Advanced smartwatch',
    price: 199.99,
    category: 'electronics',
    stock: 30
  },
  imageFile
)
  .then(product => console.log('Created with image:', product));

// Bulk create
const productsToCreate = [
  { name: 'Product 1', description: '...', price: 10, category: 'test', stock: 5 },
  { name: 'Product 2', description: '...', price: 20, category: 'test', stock: 10 }
];
productService.bulkCreateProducts(productsToCreate)
  .then(products => console.log(`Created ${products.length} products`));
```

**POST Best Practices:**
- Always set Content-Type header
- Validate data before sending
- Handle 400 Bad Request
- Handle 401 Unauthorized
- Return 201 Created on success
- Include created resource in response
- Use appropriate error messages

---

### 6. PUT

**What is it?**
PUT is an HTTP method used to completely replace an existing resource on the server.

**Why we need it?**
- Replace entire resource
- Idempotent operation
- Safe to retry
- Clear resource replacement
- RESTful update pattern

**How it works?**
- Client sends complete resource data
- Server replaces existing resource
- If resource doesn't exist, may create it
- Returns updated resource
- Idempotent (multiple PUTs same result)

**Syntax:**
```typescript
fetch('https://api.example.com/users/1', {
  method: 'PUT',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ id: 1, name: 'Jane', email: 'jane@example.com' })
})
  .then(response => response.json())
  .then(data => console.log(data));
```

**Simple Example:**
```typescript
async function updateUser(id: number, userData: { name: string; email: string }) {
  const response = await fetch(`https://api.example.com/users/${id}`, {
    method: 'PUT',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({ id, ...userData })
  });
  
  if (!response.ok) {
    throw new Error('Failed to update user');
  }
  
  return response.json();
}

// Usage
updateUser(1, { name: 'Jane', email: 'jane@example.com' })
  .then(user => console.log('Updated user:', user));
```

**Real-world Example:**
```typescript
interface User {
  id: number;
  name: string;
  email: string;
  phone: string;
  address: {
    street: string;
    city: string;
    state: string;
    zipCode: string;
  };
  preferences: {
    theme: 'light' | 'dark';
    notifications: boolean;
  };
}

class UserService {
  private baseUrl = 'https://api.example.com/users';

  async replaceUser(id: number, userData: User): Promise<User> {
    // Validate complete user data
    if (!userData.id || userData.id !== id) {
      throw new Error('User ID must match URL parameter');
    }

    if (!userData.name || !userData.email) {
      throw new Error('Name and email are required');
    }

    const response = await fetch(`${this.baseUrl}/${id}`, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${this.getAuthToken()}`
      },
      body: JSON.stringify(userData)
    });

    if (response.status === 404) {
      throw new Error(`User not found: ${id}`);
    }

    if (response.status === 400) {
      const error = await response.json();
      throw new Error(error.message || 'Validation error');
    }

    if (!response.ok) {
      throw new Error(`Failed to replace user: ${response.statusText}`);
    }

    return response.json();
  }

  async updateUserProfile(id: number, profile: Partial<User>): Promise<User> {
    // First fetch current user
    const currentUser = await this.getUserById(id);
    
    // Merge with existing data
    const updatedUser: User = {
      ...currentUser,
      ...profile
    };

    // Replace entire user
    return this.replaceUser(id, updatedUser);
  }

  async getUserById(id: number): Promise<User> {
    const response = await fetch(`${this.baseUrl}/${id}`);
    
    if (!response.ok) {
      throw new Error(`User not found: ${id}`);
    }
    
    return response.json();
  }

  private getAuthToken(): string {
    return localStorage.getItem('authToken') || '';
  }
}

// Usage examples
const userService = new UserService();

// Replace entire user
userService.replaceUser(1, {
  id: 1,
  name: 'Jane Doe',
  email: 'jane.doe@example.com',
  phone: '555-1234',
  address: {
    street: '123 Main St',
    city: 'Springfield',
    state: 'IL',
    zipCode: '62701'
  },
  preferences: {
    theme: 'dark',
    notifications: true
  }
})
  .then(user => console.log('Replaced user:', user))
  .catch(error => console.error('Error:', error.message));

// Update specific fields (replaces entire user internally)
userService.updateUserProfile(1, {
  name: 'Jane Smith',
  email: 'jane.smith@example.com'
})
  .then(user => console.log('Updated profile:', user));
```

**PUT vs POST:**
- **POST:** Creates new resource at server-generated URL
- **PUT:** Replaces resource at client-specified URL
- **POST:** Not idempotent
- **PUT:** Idempotent

**PUT Best Practices:**
- Send complete resource data
- Include ID in request body
- Validate all required fields
- Handle 404 Not Found
- Handle 400 Bad Request
- Return updated resource
- Use for complete replacements

---

### 7. PATCH

**What is it?**
PATCH is an HTTP method used to partially update an existing resource on the server.

**Why we need it?**
- Update specific fields only
- Reduce data transfer
- More efficient than PUT
- Partial updates
- Common in modern APIs

**How it works?**
- Client sends partial data
- Server updates only provided fields
- Other fields remain unchanged
- Returns updated resource
- Not idempotent (depends on implementation)

**Syntax:**
```typescript
fetch('https://api.example.com/users/1', {
  method: 'PATCH',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ name: 'Jane' })
})
  .then(response => response.json())
  .then(data => console.log(data));
```

**Simple Example:**
```typescript
async function updateUserField(id: number, field: string, value: any) {
  const response = await fetch(`https://api.example.com/users/${id}`, {
    method: 'PATCH',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({ [field]: value })
  });
  
  if (!response.ok) {
    throw new Error('Failed to update user');
  }
  
  return response.json();
}

// Usage
updateUserField(1, 'name', 'Jane')
  .then(user => console.log('Updated user:', user));
```

**Real-world Example:**
```typescript
interface Product {
  id: number;
  name: string;
  description: string;
  price: number;
  category: string;
  stock: number;
  active: boolean;
}

class ProductService {
  private baseUrl = 'https://api.example.com/products';

  async updateProduct(id: number, updates: Partial<Product>): Promise<Product> {
    // Validate updates
    if (updates.price !== undefined && updates.price <= 0) {
      throw new Error('Price must be greater than 0');
    }

    if (updates.stock !== undefined && updates.stock < 0) {
      throw new Error('Stock cannot be negative');
    }

    const response = await fetch(`${this.baseUrl}/${id}`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${this.getAuthToken()}`
      },
      body: JSON.stringify(updates)
    });

    if (response.status === 404) {
      throw new Error(`Product not found: ${id}`);
    }

    if (response.status === 400) {
      const error = await response.json();
      throw new Error(error.message || 'Validation error');
    }

    if (!response.ok) {
      throw new Error(`Failed to update product: ${response.statusText}`);
    }

    return response.json();
  }

  async updatePrice(id: number, newPrice: number): Promise<Product> {
    return this.updateProduct(id, { price: newPrice });
  }

  async updateStock(id: number, quantity: number): Promise<Product> {
    return this.updateProduct(id, { stock: quantity });
  }

  async activateProduct(id: number): Promise<Product> {
    return this.updateProduct(id, { active: true });
  }

  async deactivateProduct(id: number): Promise<Product> {
    return this.updateProduct(id, { active: false });
  }

  async bulkUpdateProducts(updates: Array<{ id: number; updates: Partial<Product> }>): Promise<Product[]> {
    const response = await fetch(`${this.baseUrl}/bulk-update`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${this.getAuthToken()}`
      },
      body: JSON.stringify({ updates })
    });

    if (!response.ok) {
      throw new Error('Failed to bulk update products');
    }

    return response.json();
  }

  private getAuthToken(): string {
    return localStorage.getItem('authToken') || '';
  }
}

// Usage examples
const productService = new ProductService();

// Update multiple fields
productService.updateProduct(1, {
  name: 'Updated Product Name',
  price: 149.99,
  stock: 100
})
  .then(product => console.log('Updated:', product));

// Update single field
productService.updatePrice(1, 199.99)
  .then(product => console.log('Price updated:', product.price));

// Update stock
productService.updateStock(1, 50)
  .then(product => console.log('Stock updated:', product.stock));

// Activate/deactivate
productService.activateProduct(1)
  .then(product => console.log('Product activated'));

productService.deactivateProduct(1)
  .then(product => console.log('Product deactivated'));

// Bulk update
productService.bulkUpdateProducts([
  { id: 1, updates: { price: 99.99 } },
  { id: 2, updates: { stock: 50 } },
  { id: 3, updates: { active: false } }
])
  .then(products => console.log(`Updated ${products.length} products`));
```

**PATCH vs PUT:**
- **PUT:** Replaces entire resource
- **PATCH:** Updates only provided fields
- **PUT:** Idempotent
- **PATCH:** Not necessarily idempotent
- **PUT:** More data transfer
- **PATCH:** Less data transfer

**PATCH Best Practices:**
- Send only fields to update
- Validate partial updates
- Handle 404 Not Found
- Return updated resource
- Use for partial updates
- Document which fields are updatable

---

### 8. DELETE

**What is it?**
DELETE is an HTTP method used to remove a resource from the server.

**Why we need it?**
- Remove resources
- Idempotent operation
- Safe to retry
- Clear deletion intent
- RESTful deletion pattern

**How it works?**
- Client sends DELETE request
- Server removes resource
- Returns status code
- May return deleted resource
- Idempotent (multiple DELETEs same result)

**Syntax:**
```typescript
fetch('https://api.example.com/users/1', {
  method: 'DELETE'
})
  .then(response => {
    if (response.ok) {
      console.log('User deleted');
    }
  });
```

**Simple Example:**
```typescript
async function deleteUser(id: number) {
  const response = await fetch(`https://api.example.com/users/${id}`, {
    method: 'DELETE'
  });
  
  if (!response.ok) {
    throw new Error('Failed to delete user');
  }
  
  return; // No content on successful DELETE
}

// Usage
deleteUser(1)
  .then(() => console.log('User deleted'))
  .catch(error => console.error('Error:', error.message));
```

**Real-world Example:**
```typescript
class ProductService {
  private baseUrl = 'https://api.example.com/products';

  async deleteProduct(id: number): Promise<void> {
    const response = await fetch(`${this.baseUrl}/${id}`, {
      method: 'DELETE',
      headers: {
        'Authorization': `Bearer ${this.getAuthToken()}`
      }
    });

    if (response.status === 404) {
      throw new Error(`Product not found: ${id}`);
    }

    if (response.status === 403) {
      throw new Error('Forbidden: You do not have permission to delete this product');
    }

    if (!response.ok) {
      throw new Error(`Failed to delete product: ${response.statusText}`);
    }

    // 204 No Content - successful deletion
    console.log(`Product ${id} deleted successfully`);
  }

  async softDeleteProduct(id: number): Promise<Product> {
    // Soft delete - mark as inactive instead of removing
    const response = await fetch(`${this.baseUrl}/${id}/soft-delete`, {
      method: 'DELETE',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${this.getAuthToken()}`
      }
    });

    if (!response.ok) {
      throw new Error('Failed to soft delete product');
    }

    return response.json();
  }

  async bulkDeleteProducts(ids: number[]): Promise<void> {
    const response = await fetch(`${this.baseUrl}/bulk-delete`, {
      method: 'DELETE',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${this.getAuthToken()}`
      },
      body: JSON.stringify({ ids })
    });

    if (!response.ok) {
      throw new Error('Failed to bulk delete products');
    }

    console.log(`Deleted ${ids.length} products`);
  }

  async deleteProductWithConfirmation(id: number): Promise<boolean> {
    // Get product details first
    const product = await this.getProductById(id);
    
    // In a real app, show confirmation dialog
    const confirmed = confirm(`Are you sure you want to delete "${product.name}"?`);
    
    if (!confirmed) {
      return false;
    }

    await this.deleteProduct(id);
    return true;
  }

  async getProductById(id: number) {
    const response = await fetch(`${this.baseUrl}/${id}`);
    
    if (!response.ok) {
      throw new Error(`Product not found: ${id}`);
    }
    
    return response.json();
  }

  private getAuthToken(): string {
    return localStorage.getItem('authToken') || '';
  }
}

// Usage examples
const productService = new ProductService();

// Delete single product
productService.deleteProduct(1)
  .then(() => console.log('Product deleted'))
  .catch(error => console.error('Error:', error.message));

// Soft delete
productService.softDeleteProduct(1)
  .then(product => console.log('Product soft deleted:', product));

// Bulk delete
productService.bulkDeleteProducts([1, 2, 3])
  .then(() => console.log('Products deleted'))
  .catch(error => console.error('Error:', error.message));

// Delete with confirmation
productService.deleteProductWithConfirmation(1)
  .then(confirmed => {
    if (confirmed) {
      console.log('Product deleted successfully');
    } else {
      console.log('Deletion cancelled');
    }
  });
```

**DELETE Best Practices:**
- Use 204 No Content on success
- Handle 404 Not Found gracefully
- Handle 403 Forbidden
- Consider soft delete for audit trails
- Require confirmation for destructive actions
- Return deleted resource if needed
- Implement proper authorization

---

### 9. HTTP Status Codes

**What is it?**
HTTP status codes are three-digit numbers returned by servers to indicate the result of a client's request.

**Why we need it?**
- Communicate request outcome
- Standard error handling
- Debug API issues
- Implement retry logic
- Provide user feedback

**How it works?**
- Server sends status code in response
- Client interprets code
- Takes appropriate action
- Different codes for different outcomes
- Standardized across web

**Status Code Categories:**

| Range | Category | Meaning |
|-------|----------|---------|
| 1xx | Informational | Request received, continuing |
| 2xx | Success | Request successfully received |
| 3xx | Redirection | Further action needed |
| 4xx | Client Error | Bad request from client |
| 5xx | Server Error | Server failed to fulfill request |

**Common Status Codes:**

**2xx Success:**
- **200 OK:** Request succeeded
- **201 Created:** Resource created
- **204 No Content:** Success, no content returned

**3xx Redirection:**
- **301 Moved Permanently:** Resource moved
- **302 Found:** Temporary redirect
- **304 Not Modified:** Resource not modified

**4xx Client Error:**
- **400 Bad Request:** Invalid request
- **401 Unauthorized:** Authentication required
- **403 Forbidden:** Access denied
- **404 Not Found:** Resource not found
- **409 Conflict:** Request conflicts
- **422 Unprocessable Entity:** Semantic error
- **429 Too Many Requests:** Rate limited

**5xx Server Error:**
- **500 Internal Server Error:** Server error
- **502 Bad Gateway:** Invalid response
- **503 Service Unavailable:** Server unavailable
- **504 Gateway Timeout:** Timeout

**Simple Example:**
```typescript
async function fetchData() {
  const response = await fetch('https://api.example.com/data');
  
  if (response.status === 200) {
    const data = await response.json();
    console.log('Success:', data);
  } else if (response.status === 404) {
    console.error('Not found');
  } else if (response.status === 500) {
    console.error('Server error');
  }
}
```

**Real-world Example:**
```typescript
class ApiClient {
  private baseUrl = 'https://api.example.com';

  async request(endpoint: string, options?: RequestInit): Promise<any> {
    const response = await fetch(`${this.baseUrl}${endpoint}`, options);
    
    // Handle different status codes
    switch (response.status) {
      case 200:
      case 201:
        return response.json();
      
      case 204:
        return null; // No content
      
      case 400:
        const badRequestError = await response.json();
        throw new Error(`Bad Request: ${badRequestError.message}`);
      
      case 401:
        throw new Error('Unauthorized: Please login');
      
      case 403:
        throw new Error('Forbidden: You do not have permission');
      
      case 404:
        throw new Error('Not Found: Resource does not exist');
      
      case 409:
        const conflictError = await response.json();
        throw new Error(`Conflict: ${conflictError.message}`);
      
      case 422:
        const validationError = await response.json();
        throw new Error(`Validation Error: ${validationError.message}`);
      
      case 429:
        throw new Error('Too Many Requests: Please wait and try again');
      
      case 500:
        throw new Error('Internal Server Error: Please try again later');
      
      case 503:
        throw new Error('Service Unavailable: Server is down for maintenance');
      
      default:
        throw new Error(`Unexpected status: ${response.status}`);
    }
  }

  async getData(): Promise<any> {
    return this.request('/data');
  }

  async createData(data: any): Promise<any> {
    return this.request('/data', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
  }
}

// Usage
const api = new ApiClient();

api.getData()
  .then(data => console.log('Data:', data))
  .catch(error => {
    if (error.message.includes('Unauthorized')) {
      // Redirect to login
      console.log('Redirecting to login...');
    } else if (error.message.includes('Not Found')) {
      // Show 404 page
      console.log('Showing 404 page...');
    } else {
      // Show error message
      console.error('Error:', error.message);
    }
  });
```

**Status Code Best Practices:**
- Use appropriate status codes
- Return error details in response body
- Handle all relevant status codes
- Provide user-friendly error messages
- Implement retry logic for 5xx errors
- Log errors for debugging

---

### 10. JSON

**What is it?**
JSON (JavaScript Object Notation) is a lightweight data interchange format that's easy for humans to read and write, and easy for machines to parse and generate.

**Why we need it?**
- Standard data format for APIs
- Language-independent
- Lightweight and fast
- Easy to parse
- Widely supported

**How it works?**
- Data represented as key-value pairs
- Supports objects and arrays
- Text-based format
- Parsed into native objects
- Stringified for transmission

**JSON vs JavaScript Objects:**
```javascript
// JavaScript Object
const jsObject = {
  name: "John",
  age: 30,
  isAdmin: true
};

// JSON string
const jsonString = '{"name":"John","age":30,"isAdmin":true}';
```

**Simple Example:**
```typescript
// JavaScript object
const user = {
  id: 1,
  name: 'John Doe',
  email: 'john@example.com',
  active: true
};

// Convert to JSON
const jsonString = JSON.stringify(user);
console.log(jsonString);
// Output: {"id":1,"name":"John Doe","email":"john@example.com","active":true}

// Parse JSON back to object
const parsedUser = JSON.parse(jsonString);
console.log(parsedUser.name); // Output: John Doe
```

**Real-world Example:**
```typescript
interface User {
  id: number;
  name: string;
  email: string;
  active: boolean;
  createdAt: string;
  preferences: {
    theme: 'light' | 'dark';
    notifications: boolean;
  };
}

// Convert object to JSON for API request
function prepareUserForApi(user: User): string {
  return JSON.stringify(user, null, 2); // Pretty print with 2 spaces
}

// Parse API response to object
function parseUserFromApi(jsonString: string): User {
  try {
    return JSON.parse(jsonString) as User;
  } catch (error) {
    throw new Error('Invalid JSON response');
  }
}

// Handle complex nested objects
const user: User = {
  id: 1,
  name: 'John Doe',
  email: 'john@example.com',
  active: true,
  createdAt: new Date().toISOString(),
  preferences: {
    theme: 'dark',
    notifications: true
  }
};

// Send to API
const apiPayload = prepareUserForApi(user);
console.log('API Payload:', apiPayload);

// Receive from API
const apiResponse = '{"id":1,"name":"John Doe","email":"john@example.com","active":true,"createdAt":"2024-01-01T00:00:00.000Z","preferences":{"theme":"dark","notifications":true}}';
const receivedUser = parseUserFromApi(apiResponse);
console.log('Received User:', receivedUser);

// Handle arrays
const users: User[] = [user, { ...user, id: 2, name: 'Jane Doe' }];
const usersJson = JSON.stringify(users);
const parsedUsers = JSON.parse(usersJson) as User[];
console.log('Users array:', parsedUsers);

// Handle dates properly
interface ApiUser {
  id: number;
  name: string;
  email: string;
  createdAt: string; // ISO string
}

interface LocalUser {
  id: number;
  name: string;
  email: string;
  createdAt: Date; // Date object
}

function convertApiToLocal(apiUser: ApiUser): LocalUser {
  return {
    ...apiUser,
    createdAt: new Date(apiUser.createdAt)
  };
}

function convertLocalToApi(localUser: LocalUser): ApiUser {
  return {
    ...localUser,
    createdAt: localUser.createdAt.toISOString()
  };
}
```

**JSON Best Practices:**
- Use TypeScript interfaces for type safety
- Handle parse errors with try/catch
- Convert dates to ISO strings
- Don't include circular references
- Use consistent naming conventions
- Validate JSON structure
- Handle null and undefined properly

---

## Part 2: Fetch API & Async/Await

### 11. Fetch API

**What is it?**
The Fetch API is a modern JavaScript interface for making HTTP requests, providing a more powerful and flexible alternative to XMLHttpRequest.

**Why we need it?**
- Make HTTP requests from JavaScript
- Promises-based (easier to use)
- Better error handling
- Supports streaming
- Modern standard

**How it works?**
- Global fetch() function
- Returns a Promise
- Response object with methods
- Chainable with .then()
- Works with async/await

**Syntax:**
```typescript
fetch(url, options?)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

**Simple Example:**
```typescript
// Basic GET request
fetch('https://api.example.com/users')
  .then(response => response.json())
  .then(users => console.log(users))
  .catch(error => console.error('Error:', error));
```

**Real-world Example:**
```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

class ApiClient {
  private baseUrl = 'https://api.example.com';

  async getUsers(): Promise<User[]> {
    const response = await fetch(`${this.baseUrl}/users`);
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    return response.json();
  }

  async getUser(id: number): Promise<User> {
    const response = await fetch(`${this.baseUrl}/users/${id}`);
    
    if (!response.ok) {
      throw new Error(`User not found: ${id}`);
    }
    
    return response.json();
  }

  async createUser(user: Omit<User, 'id'>): Promise<User> {
    const response = await fetch(`${this.baseUrl}/users`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(user)
    });
    
    if (!response.ok) {
      throw new Error('Failed to create user');
    }
    
    return response.json();
  }

  async updateUser(id: number, updates: Partial<User>): Promise<User> {
    const response = await fetch(`${this.baseUrl}/users/${id}`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(updates)
    });
    
    if (!response.ok) {
      throw new Error('Failed to update user');
    }
    
    return response.json();
  }

  async deleteUser(id: number): Promise<void> {
    const response = await fetch(`${this.baseUrl}/users/${id}`, {
      method: 'DELETE'
    });
    
    if (!response.ok) {
      throw new Error('Failed to delete user');
    }
  }

  // Fetch with query parameters
  async searchUsers(query: string, page: number = 1): Promise<User[]> {
    const url = new URL(`${this.baseUrl}/users`);
    url.searchParams.append('q', query);
    url.searchParams.append('page', page.toString());
    
    const response = await fetch(url.toString());
    
    if (!response.ok) {
      throw new Error('Search failed');
    }
    
    return response.json();
  }

  // Fetch with custom headers
  async fetchDataWithAuth(endpoint: string, token: string): Promise<any> {
    const response = await fetch(`${this.baseUrl}${endpoint}`, {
      headers: {
        'Authorization': `Bearer ${token}`,
        'Content-Type': 'application/json'
      }
    });
    
    if (response.status === 401) {
      throw new Error('Unauthorized: Invalid token');
    }
    
    if (!response.ok) {
      throw new Error('Request failed');
    }
    
    return response.json();
  }

  // Fetch with timeout
  async fetchWithTimeout(url: string, timeout: number = 5000): Promise<Response> {
    const controller = new AbortController();
    const timeoutId = setTimeout(() => controller.abort(), timeout);

    try {
      const response = await fetch(url, {
        signal: controller.signal
      });
      clearTimeout(timeoutId);
      return response;
    } catch (error) {
      clearTimeout(timeoutId);
      if (error.name === 'AbortError') {
        throw new Error('Request timeout');
      }
      throw error;
    }
  }
}

// Usage
const api = new ApiClient();

api.getUsers()
  .then(users => console.log(`Found ${users.length} users`))
  .catch(error => console.error('Error:', error.message));

api.searchUsers('John', 1)
  .then(users => console.log(`Found ${users.length} users matching "John"`))
  .catch(error => console.error('Error:', error.message));
```

**Fetch Response Methods:**
- `response.json()`: Parse as JSON
- `response.text()`: Get as text
- `response.blob()`: Get as Blob
- `response.formData()`: Get as FormData
- `response.arrayBuffer()`: Get as ArrayBuffer

**Fetch Best Practices:**
- Always check response.ok
- Handle errors properly
- Use AbortController for cancellation
- Set appropriate headers
- Handle different content types
- Implement retry logic for failures

---

### 12. async/await

**What is it?**
async/await is syntactic sugar built on Promises that makes asynchronous code look and behave more like synchronous code.

**Why we need it?**
- Easier to read and write
- Avoid callback hell
- Better error handling
- Sequential async operations
- More intuitive code flow

**How it works?**
- async function returns Promise
- await pauses execution until Promise resolves
- Can only use await in async functions
- Try/catch for error handling
- Makes async code look synchronous

**Syntax:**
```typescript
async function fetchData() {
  const response = await fetch(url);
  const data = await response.json();
  return data;
}
```

**Simple Example:**
```typescript
// Without async/await (Promise chains)
fetch('https://api.example.com/users')
  .then(response => response.json())
  .then(users => console.log(users))
  .catch(error => console.error(error));

// With async/await
async function getUsers() {
  try {
    const response = await fetch('https://api.example.com/users');
    const users = await response.json();
    console.log(users);
  } catch (error) {
    console.error(error);
  }
}
```

**Real-world Example:**
```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  stock: number;
}

interface Order {
  id: number;
  productId: number;
  quantity: number;
  total: number;
}

class OrderService {
  private baseUrl = 'https://api.example.com';

  async getProductWithStock(productId: number): Promise<Product> {
    const response = await fetch(`${this.baseUrl}/products/${productId}`);
    
    if (!response.ok) {
      throw new Error(`Product not found: ${productId}`);
    }
    
    return response.json();
  }

  async checkAvailability(productId: number, quantity: number): Promise<boolean> {
    const product = await this.getProductWithStock(productId);
    return product.stock >= quantity;
  }

  async createOrder(productId: number, quantity: number): Promise<Order> {
    // Sequential async operations
    const available = await this.checkAvailability(productId, quantity);
    
    if (!available) {
      throw new Error('Product not available in requested quantity');
    }

    const product = await this.getProductWithStock(productId);
    
    const orderData = {
      productId,
      quantity,
      total: product.price * quantity
    };

    const response = await fetch(`${this.baseUrl}/orders`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(orderData)
    });

    if (!response.ok) {
      throw new Error('Failed to create order');
    }

    return response.json();
  }

  async createMultipleOrders(items: Array<{ productId: number; quantity: number }>): Promise<Order[]> {
    // Parallel async operations
    const orders = await Promise.all(
      items.map(item => this.createOrder(item.productId, item.quantity))
    );
    
    return orders;
  }

  async createOrderWithRetry(
    productId: number,
    quantity: number,
    maxRetries: number = 3
  ): Promise<Order> {
    let lastError: Error;

    for (let attempt = 1; attempt <= maxRetries; attempt++) {
      try {
        return await this.createOrder(productId, quantity);
      } catch (error) {
        lastError = error as Error;
        console.log(`Attempt ${attempt} failed, retrying...`);
        
        if (attempt < maxRetries) {
          // Exponential backoff
          await new Promise(resolve => setTimeout(resolve, Math.pow(2, attempt) * 1000));
        }
      }
    }

    throw lastError!;
  }

  async getOrderDetails(orderId: number): Promise<{
    order: Order;
    product: Product;
  }> {
    // Parallel fetches
    const [orderResponse, productResponse] = await Promise.all([
      fetch(`${this.baseUrl}/orders/${orderId}`),
      fetch(`${this.baseUrl}/products`)
    ]);

    if (!orderResponse.ok || !productResponse.ok) {
      throw new Error('Failed to fetch order details');
    }

    const order = await orderResponse.json();
    const allProducts = await productResponse.json();
    const product = allProducts.find((p: Product) => p.id === order.productId);

    if (!product) {
      throw new Error('Product not found');
    }

    return { order, product };
  }
}

// Usage
const orderService = new OrderService();

// Sequential operations
orderService.createOrder(1, 2)
  .then(order => console.log('Order created:', order))
  .catch(error => console.error('Error:', error.message));

// Parallel operations
orderService.createMultipleOrders([
  { productId: 1, quantity: 2 },
  { productId: 2, quantity: 1 }
])
  .then(orders => console.log(`Created ${orders.length} orders`))
  .catch(error => console.error('Error:', error.message));

// With retry
orderService.createOrderWithRetry(1, 2)
  .then(order => console.log('Order created with retry:', order))
  .catch(error => console.error('All retries failed:', error.message));

// Parallel data fetching
orderService.getOrderDetails(1)
  .then(({ order, product }) => {
    console.log('Order:', order);
    console.log('Product:', product);
  })
  .catch(error => console.error('Error:', error.message));
```

**async/await Patterns:**

1. **Sequential operations:**
```typescript
const user = await fetchUser();
const posts = await fetchUserPosts(user.id);
```

2. **Parallel operations:**
```typescript
const [user, posts] = await Promise.all([
  fetchUser(),
  fetchPosts()
]);
```

3. **Error handling:**
```typescript
try {
  const data = await fetchData();
} catch (error) {
  handleError(error);
}
```

**async/await Best Practices:**
- Use try/catch for error handling
- Parallelize independent operations
- Use Promise.all for parallel requests
- Implement retry logic
- Handle timeout scenarios
- Don't forget to mark functions as async

---

### 13. try/catch

**What is it?**
try/catch is JavaScript's error handling mechanism used with async/await to handle errors that occur during asynchronous operations.

**Why we need it?**
- Handle errors gracefully
- Prevent application crashes
- Provide user feedback
- Log errors for debugging
- Implement retry logic

**How it works?**
- Wrap code in try block
- Errors caught in catch block
- Finally block always runs
- Can have multiple catch blocks
- Can re-throw errors

**Syntax:**
```typescript
try {
  const data = await fetchData();
} catch (error) {
  console.error('Error:', error);
} finally {
  // Always runs
}
```

**Simple Example:**
```typescript
async function fetchUser() {
  try {
    const response = await fetch('https://api.example.com/users/1');
    const user = await response.json();
    return user;
  } catch (error) {
    console.error('Failed to fetch user:', error);
    return null;
  }
}
```

**Real-world Example:**
```typescript
class RobustApiClient {
  private baseUrl = 'https://api.example.com';
  private maxRetries = 3;
  private retryDelay = 1000;

  async fetchWithRetry<T>(
    endpoint: string,
    options?: RequestInit
  ): Promise<T> {
    let lastError: Error;

    for (let attempt = 1; attempt <= this.maxRetries; attempt++) {
      try {
        return await this.fetchData<T>(endpoint, options);
      } catch (error) {
        lastError = error as Error;
        
        // Don't retry on client errors (4xx)
        if (error instanceof ApiError && error.isClientError()) {
          throw error;
        }

        // Don't retry on last attempt
        if (attempt === this.maxRetries) {
          break;
        }

        // Exponential backoff
        const delay = this.retryDelay * Math.pow(2, attempt - 1);
        console.log(`Attempt ${attempt} failed, retrying in ${delay}ms...`);
        await this.sleep(delay);
      }
    }

    throw lastError!;
  }

  async fetchData<T>(endpoint: string, options?: RequestInit): Promise<T> {
    const controller = new AbortController();
    const timeoutId = setTimeout(() => controller.abort(), 10000);

    try {
      const response = await fetch(`${this.baseUrl}${endpoint}`, {
        ...options,
        signal: controller.signal
      });

      clearTimeout(timeoutId);

      if (!response.ok) {
        const errorData = await response.json().catch(() => ({}));
        throw new ApiError(
          response.status,
          response.statusText,
          errorData.message || 'Request failed'
        );
      }

      return response.json();
    } catch (error) {
      clearTimeout(timeoutId);

      if (error instanceof ApiError) {
        throw error;
      }

      if (error.name === 'AbortError') {
        throw new ApiError(408, 'Request Timeout', 'Request took too long');
      }

      throw new ApiError(0, 'Network Error', 'Failed to connect to server');
    }
  }

  async getSafeData<T>(endpoint: string): Promise<T | null> {
    try {
      return await this.fetchData<T>(endpoint);
    } catch (error) {
      console.error('Error fetching data:', error);
      return null;
    }
  }

  async getDataWithFallback<T>(
    primaryEndpoint: string,
    fallbackEndpoint: string
  ): Promise<T> {
    try {
      return await this.fetchData<T>(primaryEndpoint);
    } catch (primaryError) {
      console.error('Primary endpoint failed, trying fallback:', primaryError);
      
      try {
        return await this.fetchData<T>(fallbackEndpoint);
      } catch (fallbackError) {
        console.error('Fallback endpoint also failed:', fallbackError);
        throw new Error('Both primary and fallback endpoints failed');
      }
    }
  }

  async getUserProfile(userId: number): Promise<{
    user: any;
    posts: any[];
    comments: any[];
  }> {
    try {
      // Parallel fetches
      const [user, posts, comments] = await Promise.all([
        this.fetchData(`/users/${userId}`),
        this.fetchData(`/users/${userId}/posts`),
        this.fetchData(`/users/${userId}/comments`)
      ]);

      return { user, posts, comments };
    } catch (error) {
      console.error('Failed to fetch user profile:', error);
      
      // Return partial data if possible
      try {
        const user = await this.fetchData(`/users/${userId}`);
        return { user, posts: [], comments: [] };
      } catch (userError) {
        throw new Error('Failed to fetch even basic user data');
      }
    }
  }

  private sleep(ms: number): Promise<void> {
    return new Promise(resolve => setTimeout(resolve, ms));
  }
}

class ApiError extends Error {
  constructor(
    public status: number,
    public statusText: string,
    message: string
  ) {
    super(message);
    this.name = 'ApiError';
  }

  isClientError(): boolean {
    return this.status >= 400 && this.status < 500;
  }

  isServerError(): boolean {
    return this.status >= 500;
  }
}

// Usage
const api = new RobustApiClient();

// With retry
api.fetchWithRetry('/users/1')
  .then(user => console.log('User:', user))
  .catch(error => console.error('All retries failed:', error.message));

// Safe fetch (returns null on error)
api.getSafeData('/users/1')
  .then(user => {
    if (user) {
      console.log('User:', user);
    } else {
      console.log('Failed to fetch user');
    }
  });

// With fallback
api.getDataWithFallback('/users/1', '/backup/users/1')
  .then(user => console.log('User:', user))
  .catch(error => console.error('Error:', error.message));

// Parallel with partial fallback
api.getUserProfile(1)
  .then(profile => {
    console.log('User:', profile.user);
    console.log('Posts:', profile.posts.length);
    console.log('Comments:', profile.comments.length);
  })
  .catch(error => console.error('Error:', error.message));
```

**try/catch Patterns:**

1. **Basic error handling:**
```typescript
try {
  const data = await fetchData();
} catch (error) {
  handleError(error);
}
```

2. **Specific error types:**
```typescript
try {
  const data = await fetchData();
} catch (error) {
  if (error instanceof NetworkError) {
    // Handle network error
  } else if (error instanceof ValidationError) {
    // Handle validation error
  }
}
```

3. **Finally for cleanup:**
```typescript
try {
  const data = await fetchData();
} catch (error) {
  handleError(error);
} finally {
  // Always runs
  cleanup();
}
```

**try/catch Best Practices:**
- Always catch errors in async functions
- Provide meaningful error messages
- Log errors for debugging
- Implement fallback strategies
- Use specific error types
- Clean up resources in finally
- Don't silently swallow errors

---

## Part 3: API States & useEffect

### 14. Loading State

**What is it?**
Loading state indicates that an asynchronous operation is in progress, providing visual feedback to users while data is being fetched.

**Why we need it?**
- Inform users about ongoing operations
- Prevent duplicate requests
- Provide better UX
- Manage user expectations
- Show progress indicators

**How it works?**
- Boolean state (true/false)
- Set to true before request
- Set to false after request
- Display loading indicator
- Disable interactive elements

**Syntax:**
```typescript
const [loading, setLoading] = useState(false);

const fetchData = async () => {
  setLoading(true);
  try {
    const data = await fetch(url);
    setData(data);
  } finally {
    setLoading(false);
  }
};
```

**Simple Example:**
```typescript
function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(false);

  const fetchUsers = async () => {
    setLoading(true);
    try {
      const response = await fetch('/api/users');
      const data = await response.json();
      setUsers(data);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div>
      <button onClick={fetchUsers} disabled={loading}>
        {loading ? 'Loading...' : 'Load Users'}
      </button>
      {loading && <div className="spinner">Loading...</div>}
      <ul>
        {users.map(user => <li key={user.id}>{user.name}</li>)}
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
  image: string;
}

function ProductList() {
  const [products, setProducts] = useState<Product[]>([]);
  const [loading, setLoading] = useState(false);
  const [loadingProgress, setLoadingProgress] = useState(0);

  const fetchProducts = async () => {
    setLoading(true);
    setLoadingProgress(0);

    try {
      // Simulate progress
      for (let i = 0; i <= 100; i += 10) {
        setLoadingProgress(i);
        await new Promise(resolve => setTimeout(resolve, 100));
      }

      const response = await fetch('/api/products');
      const data = await response.json();
      setProducts(data);
    } finally {
      setLoading(false);
      setLoadingProgress(0);
    }
  };

  return (
    <div className="product-list">
      <div className="header">
        <h2>Products</h2>
        <button onClick={fetchProducts} disabled={loading}>
          {loading ? 'Loading...' : 'Refresh'}
        </button>
      </div>

      {loading && (
        <div className="loading-state">
          <div className="spinner"></div>
          <div className="progress-bar">
            <div 
              className="progress-fill" 
              style={{ width: `${loadingProgress}%` }}
            />
          </div>
          <p>Loading products... {loadingProgress}%</p>
        </div>
      )}

      {!loading && products.length === 0 && (
        <div className="empty-state">
          <p>No products loaded. Click "Refresh" to load products.</p>
        </div>
      )}

      {!loading && products.length > 0 && (
        <div className="products-grid">
          {products.map(product => (
            <div key={product.id} className="product-card">
              <img src={product.image} alt={product.name} />
              <h3>{product.name}</h3>
              <p>${product.price}</p>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}
```

**Loading State Patterns:**

1. **Simple boolean:**
```typescript
const [loading, setLoading] = useState(false);
```

2. **With progress:**
```typescript
const [loading, setLoading] = useState(false);
const [progress, setProgress] = useState(0);
```

3. **With message:**
```typescript
const [loading, setLoading] = useState(false);
const [loadingMessage, setLoadingMessage] = useState('');
```

**Loading UI Patterns:**
- Spinner/loader
- Progress bar
- Skeleton screens
- Partial content loading
- Infinite scroll loading

---

### 15. Error State

**What is it?**
Error state captures and displays errors that occur during API requests, providing feedback to users when something goes wrong.

**Why we need it?**
- Inform users of failures
- Provide actionable feedback
- Improve debugging
- Handle edge cases
- Better user experience

**How it works?**
- State for error message
- Set on catch block
- Display error UI
- Clear on retry
- Can include error details

**Syntax:**
```typescript
const [error, setError] = useState<string | null>(null);

const fetchData = async () => {
  setError(null);
  try {
    const data = await fetch(url);
    setData(data);
  } catch (err) {
    setError(err.message);
  }
};
```

**Simple Example:**
```typescript
function UserList() {
  const [users, setUsers] = useState([]);
  const [error, setError] = useState<string | null>(null);

  const fetchUsers = async () => {
    setError(null);
    try {
      const response = await fetch('/api/users');
      if (!response.ok) {
        throw new Error('Failed to fetch users');
      }
      const data = await response.json();
      setUsers(data);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'An error occurred');
    }
  };

  return (
    <div>
      <button onClick={fetchUsers}>Load Users</button>
      {error && <div className="error">{error}</div>}
      <ul>
        {users.map(user => <li key={user.id}>{user.name}</li>)}
      </ul>
    </div>
  );
}
```

**Real-world Example:**
```typescript
interface ApiError {
  message: string;
  status?: number;
  code?: string;
  details?: any;
}

function ProductList() {
  const [products, setProducts] = useState([]);
  const [error, setError] = useState<ApiError | null>(null);
  const [loading, setLoading] = useState(false);

  const fetchProducts = async () => {
    setError(null);
    setLoading(true);

    try {
      const response = await fetch('/api/products');

      if (!response.ok) {
        const errorData = await response.json().catch(() => ({}));
        throw {
          message: errorData.message || 'Failed to fetch products',
          status: response.status,
          code: errorData.code,
          details: errorData.details
        };
      }

      const data = await response.json();
      setProducts(data);
    } catch (err) {
      setError({
        message: err instanceof Error ? err.message : 'An unexpected error occurred',
        status: (err as any).status,
        code: (err as any).code,
        details: (err as any).details
      });
    } finally {
      setLoading(false);
    }
  };

  const getErrorMessage = (): string => {
    if (!error) return '';

    switch (error.status) {
      case 400:
        return 'Invalid request. Please check your input.';
      case 401:
        return 'Unauthorized. Please login to continue.';
      case 403:
        return 'Access denied. You do not have permission.';
      case 404:
        return 'Products not found.';
      case 429:
        return 'Too many requests. Please wait and try again.';
      case 500:
        return 'Server error. Please try again later.';
      default:
        return error.message || 'An error occurred while loading products.';
    }
  };

  const handleRetry = () => {
    fetchProducts();
  };

  return (
    <div className="product-list">
      <div className="header">
        <h2>Products</h2>
        <button onClick={fetchProducts} disabled={loading}>
          {loading ? 'Loading...' : 'Refresh'}
        </button>
      </div>

      {error && (
        <div className="error-state">
          <div className="error-icon">⚠️</div>
          <div className="error-content">
            <h3>Error Loading Products</h3>
            <p>{getErrorMessage()}</p>
            {error.details && (
              <details className="error-details">
                <summary>Technical Details</summary>
                <pre>{JSON.stringify(error.details, null, 2)}</pre>
              </details>
            )}
            <button onClick={handleRetry} className="retry-button">
              Try Again
            </button>
          </div>
        </div>
      )}

      {!error && !loading && products.length === 0 && (
        <div className="empty-state">
          <p>No products available.</p>
        </div>
      )}

      {!error && !loading && products.length > 0 && (
        <div className="products-grid">
          {products.map(product => (
            <div key={product.id} className="product-card">
              <h3>{product.name}</h3>
              <p>${product.price}</p>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}
```

**Error State Patterns:**

1. **Simple string:**
```typescript
const [error, setError] = useState<string | null>(null);
```

2. **Error object:**
```typescript
const [error, setError] = useState<ApiError | null>(null);
```

3. **Error with code:**
```typescript
const [error, setError] = useState<{
  message: string;
  code: string;
} | null>(null);
```

**Error Handling Best Practices:**
- Provide user-friendly messages
- Include retry functionality
- Log errors for debugging
- Show technical details optionally
- Handle different error types
- Clear errors on retry

---

### 16. Success State

**What is it?**
Success state indicates that an operation completed successfully, often used to show confirmation messages or success indicators.

**Why we need it?**
- Confirm successful operations
- Provide positive feedback
- Improve user confidence
- Guide next steps
- Better UX

**How it works?**
- Boolean state for success
- Set after successful operation
- Display success message
- Auto-dismiss after delay
- Clear on new operation

**Syntax:**
```typescript
const [success, setSuccess] = useState(false);

const submitForm = async () => {
  try {
    await api.submit(data);
    setSuccess(true);
    setTimeout(() => setSuccess(false), 3000);
  } catch (error) {
    // Handle error
  }
};
```

**Simple Example:**
```typescript
function ContactForm() {
  const [success, setSuccess] = useState(false);

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    try {
      await fetch('/api/contact', {
        method: 'POST',
        body: JSON.stringify(formData)
      });
      setSuccess(true);
      setTimeout(() => setSuccess(false), 3000);
    } catch (error) {
      console.error(error);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* Form fields */}
      {success && <div className="success">Message sent successfully!</div>}
      <button type="submit">Send</button>
    </form>
  );
}
```

**Real-world Example:**
```typescript
function ProductForm() {
  const [formData, setFormData] = useState({
    name: '',
    price: '',
    category: ''
  });
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);
  const [success, setSuccess] = useState(false);
  const [successMessage, setSuccessMessage] = useState('');

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setError(null);
    setSuccess(false);
    setLoading(true);

    try {
      const response = await fetch('/api/products', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData)
      });

      if (!response.ok) {
        throw new Error('Failed to create product');
      }

      const createdProduct = await response.json();
      
      setSuccess(true);
      setSuccessMessage(`Product "${createdProduct.name}" created successfully!`);
      
      // Reset form
      setFormData({ name: '', price: '', category: '' });

      // Auto-dismiss success message
      setTimeout(() => {
        setSuccess(false);
        setSuccessMessage('');
      }, 5000);

    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to create product');
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="product-form">
      <h2>Create Product</h2>

      {success && (
        <div className="success-state">
          <div className="success-icon">✓</div>
          <div className="success-content">
            <h3>Success!</h3>
            <p>{successMessage}</p>
          </div>
          <button 
            onClick={() => setSuccess(false)} 
            className="dismiss-button"
          >
            Dismiss
          </button>
        </div>
      )}

      {error && (
        <div className="error-state">
          <div className="error-icon">⚠️</div>
          <p>{error}</p>
        </div>
      )}

      <form onSubmit={handleSubmit}>
        <div className="form-group">
          <label>Product Name</label>
          <input
            type="text"
            value={formData.name}
            onChange={(e) => setFormData({ ...formData, name: e.target.value })}
            disabled={loading || success}
          />
        </div>

        <div className="form-group">
          <label>Price</label>
          <input
            type="number"
            value={formData.price}
            onChange={(e) => setFormData({ ...formData, price: e.target.value })}
            disabled={loading || success}
          />
        </div>

        <div className="form-group">
          <label>Category</label>
          <input
            type="text"
            value={formData.category}
            onChange={(e) => setFormData({ ...formData, category: e.target.value })}
            disabled={loading || success}
          />
        </div>

        <button type="submit" disabled={loading || success}>
          {loading ? 'Creating...' : success ? 'Created!' : 'Create Product'}
        </button>
      </form>
    </div>
  );
}
```

**Success State Patterns:**

1. **Simple boolean:**
```typescript
const [success, setSuccess] = useState(false);
```

2. **With message:**
```typescript
const [success, setSuccess] = useState(false);
const [message, setMessage] = useState('');
```

3. **With action link:**
```typescript
const [success, setSuccess] = useState({
  show: false,
  message: '',
  actionUrl: ''
});
```

**Success UI Patterns:**
- Toast notifications
- Success banners
- Modal confirmations
- Inline success messages
- Checkmark indicators

---

### 17. Empty State

**What is it?**
Empty state displays when there's no data to show, providing guidance on what users can do next.

**Why we need it?**
- Inform users of empty data
- Provide next steps
- Better UX
- Clear communication
- Prevent confusion

**How it works?**
- Check if data array is empty
- Display empty state UI
- Provide helpful message
- Suggest actions
- Show empty state icon/illustration

**Syntax:**
```typescript
{data.length === 0 && (
  <div className="empty-state">
    <p>No data available</p>
  </div>
)}
```

**Simple Example:**
```typescript
function UserList() {
  const [users, setUsers] = useState([]);

  return (
    <div>
      {users.length === 0 ? (
        <div className="empty-state">
          <p>No users found</p>
        </div>
      ) : (
        <ul>
          {users.map(user => <li key={user.id}>{user.name}</li>)}
        </ul>
      )}
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
}

function ProductList() {
  const [products, setProducts] = useState<Product[]>([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);
  const [searchQuery, setSearchQuery] = useState('');

  const fetchProducts = async () => {
    setLoading(true);
    setError(null);

    try {
      const url = searchQuery 
        ? `/api/products?search=${encodeURIComponent(searchQuery)}`
        : '/api/products';
      
      const response = await fetch(url);
      
      if (!response.ok) {
        throw new Error('Failed to fetch products');
      }

      const data = await response.json();
      setProducts(data);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'An error occurred');
    } finally {
      setLoading(false);
    }
  };

  const filteredProducts = products.filter(product =>
    product.name.toLowerCase().includes(searchQuery.toLowerCase())
  );

  const hasProducts = filteredProducts.length > 0;
  const hasSearchQuery = searchQuery.trim().length > 0;

  return (
    <div className="product-list">
      <div className="header">
        <h2>Products</h2>
        <input
          type="text"
          placeholder="Search products..."
          value={searchQuery}
          onChange={(e) => setSearchQuery(e.target.value)}
        />
        <button onClick={fetchProducts} disabled={loading}>
          Refresh
        </button>
      </div>

      {loading && (
        <div className="loading-state">
          <div className="spinner"></div>
          <p>Loading products...</p>
        </div>
      )}

      {error && (
        <div className="error-state">
          <p>Error: {error}</p>
          <button onClick={fetchProducts}>Retry</button>
        </div>
      )}

      {!loading && !error && !hasProducts && (
        <div className="empty-state">
          <div className="empty-icon">📦</div>
          <h3>No Products Found</h3>
          
          {hasSearchQuery ? (
            <p>No products match "{searchQuery}". Try a different search term.</p>
          ) : (
            <p>There are no products available yet.</p>
          )}
          
          <div className="empty-actions">
            {hasSearchQuery && (
              <button onClick={() => setSearchQuery('')}>
                Clear Search
              </button>
            )}
            <button onClick={fetchProducts}>
              Refresh Products
            </button>
            <button onClick={() => {/* Navigate to create page */}}>
              Add First Product
            </button>
          </div>
        </div>
      )}

      {!loading && !error && hasProducts && (
        <div className="products-grid">
          <p>Showing {filteredProducts.length} product(s)</p>
          {filteredProducts.map(product => (
            <div key={product.id} className="product-card">
              <h3>{product.name}</h3>
              <p>${product.price}</p>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}
```

**Empty State Patterns:**

1. **No data at all:**
```typescript
{data.length === 0 && <div>No data available</div>}
```

2. **No search results:**
```typescript
{searchQuery && filteredData.length === 0 && (
  <div>No results for "{searchQuery}"</div>
)}
```

3. **With action buttons:**
```typescript
{data.length === 0 && (
  <div>
    <p>No items</p>
    <button>Create first item</button>
  </div>
)}
```

**Empty State Best Practices:**
- Provide clear messaging
- Suggest next actions
- Use appropriate icons/illustrations
- Differentiate between states
- Clear filters easily
- Maintain brand consistency

---

### 18. useEffect + API

**What is it?**
Using useEffect to trigger API requests when components mount or when dependencies change, ensuring data is fetched at the right time.

**Why we need it?**
- Fetch data on component mount
- Refetch when dependencies change
- Clean up requests on unmount
- Prevent memory leaks
- Proper data lifecycle

**How it works?**
- useEffect with fetch
- Empty deps for mount-only
- Dependencies for refetch
- Cleanup function for cancellation
- AbortController for cleanup

**Syntax:**
```typescript
useEffect(() => {
  const fetchData = async () => {
    const response = await fetch(url);
    const data = await response.json();
    setData(data);
  };
  fetchData();
}, [dependencies]);
```

**Simple Example:**
```typescript
function UserProfile({ userId }: { userId: number }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    const fetchUser = async () => {
      const response = await fetch(`/api/users/${userId}`);
      const data = await response.json();
      setUser(data);
    };
    fetchUser();
  }, [userId]);

  return <div>{user ? user.name : 'Loading...'}</div>;
}
```

**Real-world Example:**
```typescript
function ProductList({ category }: { category: string }) {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const controller = new AbortController();

    const fetchProducts = async () => {
      setLoading(true);
      setError(null);

      try {
        const url = category 
          ? `/api/products?category=${encodeURIComponent(category)}`
          : '/api/products';

        const response = await fetch(url, {
          signal: controller.signal
        });

        if (!response.ok) {
          throw new Error('Failed to fetch products');
        }

        const data = await response.json();
        setProducts(data);
      } catch (err) {
        if (err.name !== 'AbortError') {
          setError(err instanceof Error ? err.message : 'An error occurred');
        }
      } finally {
        setLoading(false);
      }
    };

    fetchProducts();

    // Cleanup function
    return () => {
      controller.abort();
    };
  }, [category]); // Re-fetch when category changes

  return (
    <div>
      {loading && <div>Loading...</div>}
      {error && <div>Error: {error}</div>}
      <ul>
        {products.map(product => (
          <li key={product.id}>{product.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

**useEffect + API Patterns:**

1. **Fetch on mount:**
```typescript
useEffect(() => {
  fetchData();
}, []); // Empty deps
```

2. **Fetch when prop changes:**
```typescript
useEffect(() => {
  fetchData(id);
}, [id]); // Re-fetch when id changes
```

3. **With cleanup:**
```typescript
useEffect(() => {
  const controller = new AbortController();
  fetchData(controller.signal);
  return () => controller.abort();
}, []);
```

**Common Mistakes:**
- Not using cleanup function
- Missing dependencies
- Not handling AbortError
- Not setting loading state
- Not handling errors properly

**Best Practices:**
- Always use AbortController
- Cleanup on unmount
- Include proper dependencies
- Handle loading and error states
- Check for aborted requests
- Use functional updates for state

---

## API Lifecycle

### Understanding the API Request Lifecycle

The API lifecycle represents the complete flow of an API request from the component to the server and back to the UI.

```
┌─────────────────────────────────────────────────────────────┐
│                      COMPONENT                                │
│  - User action triggers request                                │
│  - Component sets loading state                              │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                      REQUEST                                  │
│  - HTTP request created                                       │
│  - Headers, method, body set                                │
│  - Request sent to server                                     │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                      LOADING                                  │
│  - Waiting for server response                                │
│  - Show loading indicator to user                            │
│  - Disable interactive elements                               │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                      RESPONSE                                  │
│  - Server processes request                                  │
│  - Returns data or error                                     │
│  - HTTP status code set                                      │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
              ┌──────────┴──────────┐
              │                     │
              ▼                     ▼
┌─────────────────────┐  ┌─────────────────────┐
│      SUCCESS        │  │       ERROR         │
│  - Parse response    │  │  - Error message    │
│  - Update state      │  │  - Set error state  │
│  - Clear loading     │  │  - Clear loading     │
│  - Show data         │  │  - Show error UI     │
└──────────┬──────────┘  └──────────┬──────────┘
           │                        │
           └────────┬───────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────────────────────┐
│                         UI                                  │
│  - Display data or error                                     │
│  - Enable interactive elements                               │
│  - User can interact again                                   │
└─────────────────────────────────────────────────────────────┘
```

### Code Example of API Lifecycle

```typescript
function ProductDetails({ productId }: { productId: number }) {
  // COMPONENT: Initial state
  const [product, setProduct] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    // REQUEST: Start the request
    const controller = new AbortController();
    
    const fetchProduct = async () => {
      // LOADING: Set loading state
      setLoading(true);
      setError(null);

      try {
        // RESPONSE: Wait for server response
        const response = await fetch(`/api/products/${productId}`, {
          signal: controller.signal
        });

        if (!response.ok) {
          // ERROR: Handle HTTP error
          throw new Error(`HTTP error! status: ${response.status}`);
        }

        // SUCCESS: Parse and set data
        const data = await response.json();
        setProduct(data);
        
      } catch (err) {
        // ERROR: Handle fetch error
        if (err.name !== 'AbortError') {
          setError(err instanceof Error ? err.message : 'Failed to fetch product');
        }
      } finally {
        // Clear loading state
        setLoading(false);
      }
    };

    fetchProduct();

    // Cleanup: Cancel request on unmount
    return () => {
      controller.abort();
    };
  }, [productId]);

  // UI: Display based on state
  if (loading) {
    return <div className="loading">Loading product...</div>;
  }

  if (error) {
    return (
      <div className="error">
        <p>Error: {error}</p>
        <button onClick={() => {/* retry */}}>Retry</button>
      </div>
    );
  }

  if (!product) {
    return <div className="empty">Product not found</div>;
  }

  // SUCCESS: Display product data
  return (
    <div className="product-details">
      <h1>{product.name}</h1>
      <p>{product.description}</p>
      <p>${product.price}</p>
    </div>
  );
}
```

---

## Part 4: Axios

### 19. Axios

**What is it?**
Axios is a popular JavaScript library for making HTTP requests, providing a more feature-rich and easier-to-use alternative to the native Fetch API.

**Why we need it?**
- Simpler API than fetch
- Automatic JSON parsing
- Request/response interceptors
- Automatic request transformation
- Better error handling
- Browser compatibility

**How it works?**
- Promise-based HTTP client
- Supports all HTTP methods
- Automatic JSON handling
- Request/response transformation
- Interceptors for middleware
- Cancellation support

**Installation:**
```bash
npm install axios
# or
yarn add axios
```

**Syntax:**
```typescript
import axios from 'axios';

axios.get('/api/users')
  .then(response => console.log(response.data))
  .catch(error => console.error(error));
```

**Simple Example:**
```typescript
import axios from 'axios';

// GET request
axios.get('https://api.example.com/users')
  .then(response => console.log(response.data))
  .catch(error => console.error(error));

// POST request
axios.post('https://api.example.com/users', {
  name: 'John',
  email: 'john@example.com'
})
  .then(response => console.log(response.data))
  .catch(error => console.error(error));
```

**Real-world Example:**
```typescript
import axios, { AxiosInstance, AxiosRequestConfig, AxiosResponse } from 'axios';

class ApiService {
  private client: AxiosInstance;

  constructor(baseURL: string) {
    this.client = axios.create({
      baseURL,
      timeout: 10000,
      headers: {
        'Content-Type': 'application/json'
      }
    });

    // Request interceptor
    this.client.interceptors.request.use(
      (config) => {
        // Add auth token
        const token = localStorage.getItem('authToken');
        if (token) {
          config.headers.Authorization = `Bearer ${token}`;
        }
        return config;
      },
      (error) => {
        return Promise.reject(error);
      }
    );

    // Response interceptor
    this.client.interceptors.response.use(
      (response) => {
        // Handle successful response
        return response;
      },
      (error) => {
        // Handle error response
        if (error.response) {
          switch (error.response.status) {
            case 401:
              // Redirect to login
              window.location.href = '/login';
              break;
            case 403:
              error.message = 'Access denied';
              break;
            case 404:
              error.message = 'Resource not found';
              break;
            case 500:
              error.message = 'Server error';
              break;
            default:
              error.message = error.response.data?.message || 'Request failed';
          }
        }
        return Promise.reject(error);
      }
    );
  }

  async get<T>(url: string, config?: AxiosRequestConfig): Promise<T> {
    const response: AxiosResponse<T> = await this.client.get(url, config);
    return response.data;
  }

  async post<T>(url: string, data?: any, config?: AxiosRequestConfig): Promise<T> {
    const response: AxiosResponse<T> = await this.client.post(url, data, config);
    return response.data;
  }

  async put<T>(url: string, data?: any, config?: AxiosRequestConfig): Promise<T> {
    const response: AxiosResponse<T> = await this.client.put(url, data, config);
    return response.data;
  }

  async patch<T>(url: string, data?: any, config?: AxiosRequestConfig): Promise<T> {
    const response: AxiosResponse<T> = await this.client.patch(url, data, config);
    return response.data;
  }

  async delete<T>(url: string, config?: AxiosRequestConfig): Promise<T> {
    const response: AxiosResponse<T> = await this.client.delete(url, config);
    return response.data;
  }
}

// Usage
const api = new ApiService('https://api.example.com');

api.get<User[]>('/users')
  .then(users => console.log(users))
  .catch(error => console.error(error.message));
```

**Axios vs Fetch:**

| Feature | Axios | Fetch |
|---------|-------|-------|
| JSON parsing | Automatic | Manual |
| Error handling | Better | Basic |
| Interceptors | Yes | No |
| Timeout | Built-in | Manual |
| Cancellation | Yes | AbortController |
| Upload progress | Yes | No |
| Browser support | Older browsers | Modern browsers |

**Axios Best Practices:**
- Use interceptors for auth
- Handle errors globally
- Set reasonable timeouts
- Use TypeScript for type safety
- Create API service layer
- Handle request cancellation

---

### 20. Axios Instance

**What is it?**
An Axios instance is a pre-configured Axios client with specific settings like baseURL, headers, and timeout that can be reused across multiple requests.

**Why we need it?**
- Consistent configuration
- Separate instances for different APIs
- Avoid repetition
- Centralized configuration
- Easier maintenance

**How it works?**
- Create instance with axios.create()
- Set default configuration
- Use instance for requests
- Configuration applies to all requests
- Can override per request

**Syntax:**
```typescript
const api = axios.create({
  baseURL: 'https://api.example.com',
  timeout: 10000,
  headers: { 'Content-Type': 'application/json' }
});
```

**Simple Example:**
```typescript
import axios from 'axios';

const api = axios.create({
  baseURL: 'https://api.example.com',
  timeout: 5000
});

api.get('/users').then(response => console.log(response.data));
```

**Real-world Example:**
```typescript
import axios, { AxiosInstance } from 'axios';

// Multiple API instances
const userApi: AxiosInstance = axios.create({
  baseURL: 'https://api.example.com/users',
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json',
    'X-API-Version': 'v1'
  }
});

const productApi: AxiosInstance = axios.create({
  baseURL: 'https://api.example.com/products',
  timeout: 15000,
  headers: {
    'Content-Type': 'application/json',
    'X-API-Version': 'v2'
  }
});

const analyticsApi: AxiosInstance = axios.create({
  baseURL: 'https://analytics.example.com',
  timeout: 30000,
  headers: {
    'Content-Type': 'application/json'
  }
});

// Add interceptors to specific instances
userApi.interceptors.request.use(config => {
  const token = localStorage.getItem('userToken');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

productApi.interceptors.request.use(config => {
  const token = localStorage.getItem('productToken');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Usage
userApi.get('/profile')
  .then(response => console.log('User profile:', response.data));

productApi.get('/list')
  .then(response => console.log('Products:', response.data));

analyticsApi.post('/track', { event: 'page_view' })
  .then(response => console.log('Event tracked'));
```

**Axios Instance Patterns:**

1. **Single instance:**
```typescript
const api = axios.create({ baseURL: 'https://api.example.com' });
```

2. **Multiple instances:**
```typescript
const userApi = axios.create({ baseURL: 'https://api.example.com/users' });
const productApi = axios.create({ baseURL: 'https://api.example.com/products' });
```

3. **Dynamic instances:**
```typescript
const createApiInstance = (baseURL: string) => 
  axios.create({ baseURL });
```

---

### 21. Base URL

**What is it?**
Base URL is the common part of an API's URL that is shared across all endpoints, allowing you to specify it once and use relative paths for individual requests.

**Why we need it?**
- Avoid repetition
- Easy API URL changes
- Cleaner code
- Centralized configuration
- Environment-specific URLs

**How it works?**
- Set in Axios instance config
- Appended to request URLs
- Removed from individual requests
- Can use environment variables
- Supports different environments

**Syntax:**
```typescript
const api = axios.create({
  baseURL: 'https://api.example.com/api/v1'
});

api.get('/users'); // Full URL: https://api.example.com/api/v1/users
```

**Simple Example:**
```typescript
import axios from 'axios';

const api = axios.create({
  baseURL: 'https://api.example.com/api/v1'
});

// Instead of:
// axios.get('https://api.example.com/api/v1/users')

// Use:
api.get('/users');
```

**Real-world Example:**
```typescript
import axios from 'axios';

// Environment-specific base URL
const BASE_URL = import.meta.env.VITE_API_URL || 'https://api.example.com/api/v1';

const api = axios.create({
  baseURL: BASE_URL,
  timeout: 10000
});

// Environment-specific instances
const developmentApi = axios.create({
  baseURL: 'http://localhost:3000/api/v1'
});

const productionApi = axios.create({
  baseURL: 'https://api.example.com/api/v1'
});

const stagingApi = axios.create({
  baseURL: 'https://staging.example.com/api/v1'
});

// Select based on environment
const currentApi = import.meta.env.MODE === 'production' 
  ? productionApi 
  : developmentApi;

// Usage
currentApi.get('/users')
  .then(response => console.log(response.data));

currentApi.post('/products', { name: 'New Product' })
  .then(response => console.log(response.data));
```

**Base URL Best Practices:**
- Use environment variables
- Include API version
- Remove trailing slashes
- Document the base URL
- Support multiple environments
- Test different environments

---

### 22. Headers

**What is it?**
HTTP headers are additional information sent with requests and responses, providing metadata about the request/response like content type, authentication, and more.

**Why we need it?**
- Specify content type
- Send authentication tokens
- Control caching behavior
- Specify API version
- Custom application headers

**How it works?**
- Key-value pairs
- Sent with request/response
- Set in Axios config
- Can be default or per-request
- Some headers are set automatically

**Syntax:**
```typescript
axios.get('/api/users', {
  headers: {
    'Authorization': 'Bearer token',
    'Content-Type': 'application/json'
  }
});
```

**Simple Example:**
```typescript
import axios from 'axios';

axios.get('/api/users', {
  headers: {
    'Authorization': 'Bearer ' + token,
    'Accept': 'application/json'
  }
});
```

**Real-world Example:**
```typescript
import axios from 'axios';

const api = axios.create({
  baseURL: 'https://api.example.com/api/v1',
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json',
    'X-API-Version': '1.0',
    'X-Client-ID': 'web-app'
  }
});

// Add auth header dynamically
const setAuthHeader = (token: string) => {
  api.defaults.headers.common['Authorization'] = `Bearer ${token}`;
};

const removeAuthHeader = () => {
  delete api.defaults.headers.common['Authorization'];
};

// Request with custom headers
const fetchWithCustomHeaders = async () => {
  try {
    const response = await api.get('/users', {
      headers: {
        'X-Custom-Header': 'custom-value',
        'X-Request-ID': generateRequestId()
      }
    });
    return response.data;
  } catch (error) {
    console.error('Request failed:', error);
  }
};

// Upload with different content type
const uploadFile = async (file: File) => {
  const formData = new FormData();
  formData.append('file', file);

  const response = await api.post('/upload', formData, {
    headers: {
      'Content-Type': 'multipart/form-data'
    }
  });

  return response.data;
};

// Conditional headers
const fetchWithConditionalHeader = async (useCache: boolean) => {
  const headers: Record<string, string> = {};
  
  if (useCache) {
    headers['Cache-Control'] = 'max-age=3600';
  } else {
    headers['Cache-Control'] = 'no-cache';
  }

  const response = await api.get('/data', { headers });
  return response.data;
};

function generateRequestId(): string {
  return Math.random().toString(36).substring(2, 15);
}

// Usage
setAuthHeader('your-jwt-token');
api.get('/users').then(data => console.log(data));
removeAuthHeader();
```

**Common Headers:**
- `Content-Type`: Specifies request body format
- `Accept`: Specifies expected response format
- `Authorization`: Authentication credentials
- `User-Agent`: Client identification
- `Cache-Control`: Caching behavior
- `X-API-Key`: API key for authentication

**Headers Best Practices:**
- Set default headers in instance
- Use environment variables for sensitive data
- Handle auth tokens securely
- Use appropriate content types
- Document custom headers
- Validate headers on server

---

### 23. Authorization Header (Simplified)

**What is it?**
Authorization header is used to send authentication credentials (usually a JWT token) with API requests to identify and authorize the user.

**Why we need it?**
- Authenticate users
- Protect endpoints
- Control access
- Track user sessions
- Implement security

**How it works?**
- Include token in Authorization header
- Server validates token
- Token usually JWT format
- Sent as "Bearer {token}"
- Required for protected endpoints

**Syntax:**
```typescript
headers: {
  'Authorization': `Bearer ${token}`
}
```

**Simple Example:**
```typescript
const token = localStorage.getItem('authToken');

axios.get('/api/protected', {
  headers: {
    'Authorization': `Bearer ${token}`
  }
});
```

**Real-world Example:**
```typescript
import axios from 'axios';

class AuthenticatedApi {
  private client: AxiosInstance;

  constructor() {
    this.client = axios.create({
      baseURL: 'https://api.example.com/api/v1',
      headers: {
        'Content-Type': 'application/json'
      }
    });

    // Add auth header to all requests
    this.client.interceptors.request.use(config => {
      const token = localStorage.getItem('authToken');
      if (token) {
        config.headers.Authorization = `Bearer ${token}`;
      }
      return config;
    });

    // Handle 401 errors
    this.client.interceptors.response.use(
      response => response,
      error => {
        if (error.response?.status === 401) {
          // Token expired or invalid
          localStorage.removeItem('authToken');
          window.location.href = '/login';
        }
        return Promise.reject(error);
      }
    );
  }

  setAuthToken(token: string) {
    localStorage.setItem('authToken', token);
  }

  removeAuthToken() {
    localStorage.removeItem('authToken');
  }

  isAuthenticated(): boolean {
    return !!localStorage.getItem('authToken');
  }

  async getProtectedData() {
    if (!this.isAuthenticated()) {
      throw new Error('Not authenticated');
    }

    const response = await this.client.get('/protected/data');
    return response.data;
  }

  async login(email: string, password: string) {
    const response = await this.client.post('/auth/login', {
      email,
      password
    });

    const { token, user } = response.data;
    this.setAuthToken(token);
    return user;
  }

  async logout() {
    try {
      await this.client.post('/auth/logout');
    } finally {
      this.removeAuthToken();
    }
  }
}

// Usage
const api = new AuthenticatedApi();

// Login
api.login('user@example.com', 'password')
  .then(user => console.log('Logged in as:', user.name));

// Access protected endpoint
api.getProtectedData()
  .then(data => console.log('Protected data:', data))
  .catch(error => console.error('Error:', error.message));

// Logout
api.logout().then(() => console.log('Logged out'));
```

**Authorization Best Practices:**
- Store tokens securely (httpOnly cookies preferred)
- Never log tokens
- Use HTTPS for all requests
- Implement token refresh
- Handle token expiration
- Clear tokens on logout

---

## Part 5: Custom Hooks & Separation of Concerns

### 24. API Services

**What is it?**
API services are dedicated modules/classes that handle all API communication, separating API logic from React components.

**Why we need it?**
- Separation of concerns
- Reusable API logic
- Easier testing
- Centralized error handling
- Better code organization

**How it works?**
- Separate service files
- Functions for each endpoint
- Handle all HTTP operations
- Return typed responses
- Can be used anywhere

**Simple Example:**
```typescript
// services/user.service.ts
import axios from 'axios';

const api = axios.create({
  baseURL: 'https://api.example.com/api/v1'
});

export const userService = {
  getUsers: () => api.get('/users'),
  getUser: (id: number) => api.get(`/users/${id}`),
  createUser: (user: any) => api.post('/users', user),
  updateUser: (id: number, user: any) => api.put(`/users/${id}`, user),
  deleteUser: (id: number) => api.delete(`/users/${id}`)
};
```

**Real-world Example:**
```typescript
// services/product.service.ts
import axios, { AxiosInstance } from 'axios';

interface Product {
  id: number;
  name: string;
  description: string;
  price: number;
  category: string;
  stock: number;
  active: boolean;
  createdAt: string;
  updatedAt: string;
}

interface CreateProductDto {
  name: string;
  description: string;
  price: number;
  category: string;
  stock: number;
}

interface UpdateProductDto {
  name?: string;
  description?: string;
  price?: number;
  category?: string;
  stock?: number;
  active?: boolean;
}

interface ProductListResponse {
  products: Product[];
  total: number;
  page: number;
  limit: number;
}

class ProductService {
  private client: AxiosInstance;

  constructor() {
    this.client = axios.create({
      baseURL: import.meta.env.VITE_API_URL || 'https://api.example.com/api/v1',
      timeout: 10000,
      headers: {
        'Content-Type': 'application/json'
      }
    });

    // Add auth interceptor
    this.client.interceptors.request.use(config => {
      const token = localStorage.getItem('authToken');
      if (token) {
        config.headers.Authorization = `Bearer ${token}`;
      }
      return config;
    });
  }

  async getProducts(params?: {
    page?: number;
    limit?: number;
    category?: string;
    search?: string;
    active?: boolean;
  }): Promise<ProductListResponse> {
    const response = await this.client.get<ProductListResponse>('/products', { params });
    return response.data;
  }

  async getProduct(id: number): Promise<Product> {
    const response = await this.client.get<Product>(`/products/${id}`);
    return response.data;
  }

  async createProduct(data: CreateProductDto): Promise<Product> {
    const response = await this.client.post<Product>('/products', data);
    return response.data;
  }

  async updateProduct(id: number, data: UpdateProductDto): Promise<Product> {
    const response = await this.client.patch<Product>(`/products/${id}`, data);
    return response.data;
  }

  async deleteProduct(id: number): Promise<void> {
    await this.client.delete(`/products/${id}`);
  }

  async getProductsByCategory(category: string): Promise<Product[]> {
    const response = await this.client.get<Product[]>('/products', {
      params: { category }
    });
    return response.data;
  }

  async searchProducts(query: string): Promise<Product[]> {
    const response = await this.client.get<Product[]>('/products/search', {
      params: { q: query }
    });
    return response.data;
  }

  async updateStock(id: number, quantity: number): Promise<Product> {
    const response = await this.client.patch<Product>(`/products/${id}/stock`, {
      stock: quantity
    });
    return response.data;
  }

  async activateProduct(id: number): Promise<Product> {
    const response = await this.client.patch<Product>(`/products/${id}/activate`);
    return response.data;
  }

  async deactivateProduct(id: number): Promise<Product> {
    const response = await this.client.patch<Product>(`/products/${id}/deactivate`);
    return response.data;
  }
}

// Export singleton instance
export const productService = new ProductService();
```

**API Service Best Practices:**
- One service per resource
- TypeScript interfaces for types
- Centralized error handling
- Consistent naming conventions
- Clear method documentation
- Handle authentication

---

### 25. Custom Hooks

**What is it?**
Custom hooks are reusable functions that encapsulate stateful logic, allowing you to share stateful behavior between components without prop drilling.

**Why we need it?**
- Reusable logic
- Separation of concerns
- Cleaner components
- Easier testing
- Better code organization

**How it works?**
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
function useCounter(initialValue: number = 0) {
  const [count, setCount] = useState(initialValue);
  
  const increment = () => setCount(prev => prev + 1);
  const decrement = () => setCount(prev => prev - 1);
  const reset = () => setCount(initialValue);
  
  return { count, increment, decrement, reset };
}
```

**Real-world Example:**
```typescript
import { useState, useEffect, useCallback } from 'react';

interface UseApiResult<T> {
  data: T | null;
  loading: boolean;
  error: string | null;
  refetch: () => Promise<void>;
}

function useApi<T>(
  url: string,
  options?: {
    immediate?: boolean;
    dependencies?: any[];
  }
): UseApiResult<T> {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const fetchData = useCallback(async () => {
    setLoading(true);
    setError(null);

    try {
      const response = await fetch(url);
      
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      
      const result = await response.json();
      setData(result);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'An error occurred');
    } finally {
      setLoading(false);
    }
  }, [url]);

  useEffect(() => {
    if (options?.immediate !== false) {
      fetchData();
    }
  }, [fetchData, options?.immediate]);

  return { data, loading, error, refetch: fetchData };
}

// Usage
function UserProfile({ userId }: { userId: number }) {
  const { data: user, loading, error, refetch } = useApi<User>(
    `https://api.example.com/users/${userId}`
  );

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!user) return <div>User not found</div>;

  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
      <button onClick={refetch}>Refresh</button>
    </div>
  );
}
```

**Custom Hook Patterns:**

1. **Data fetching:**
```typescript
function useFetch(url) { /* ... */ }
```

2. **Form handling:**
```typescript
function useForm(initialValues) { /* ... */ }
```

3. **Local storage:**
```typescript
function useLocalStorage(key, initialValue) { /* ... */ }
```

**Custom Hook Best Practices:**
- Start with "use" prefix
- Return state and functions
- Handle cleanup in useEffect
- Use TypeScript for types
- Keep hooks focused
- Document hook usage

---

### 26. useFetch Hook

**What is it?**
A custom hook specifically designed for data fetching, encapsulating the common pattern of loading state, error handling, and data management.

**Why we need it?**
- Reusable fetching logic
- Consistent error handling
- Loading state management
- Cleanup on unmount
- Type-safe API calls

**How it works?**
- Accepts URL and options
- Manages loading, error, data states
- Supports cancellation
- Can have dependencies
- Returns consistent interface

**Syntax:**
```typescript
const { data, loading, error, refetch } = useFetch(url, options);
```

**Simple Example:**
```typescript
function useFetch(url: string) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    const controller = new AbortController();
    
    const fetchData = async () => {
      setLoading(true);
      try {
        const response = await fetch(url, { signal: controller.signal });
        const result = await response.json();
        setData(result);
      } catch (err) {
        if (err.name !== 'AbortError') {
          setError(err.message);
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

**Real-world Example:**
```typescript
import { useState, useEffect, useCallback } from 'react';

interface UseFetchOptions {
  immediate?: boolean;
  dependencies?: any[];
  onSuccess?: (data: any) => void;
  onError?: (error: Error) => void;
}

interface UseFetchResult<T> {
  data: T | null;
  loading: boolean;
  error: string | null;
  refetch: () => Promise<void>;
  isSuccess: boolean;
}

function useFetch<T = any>(
  url: string,
  options: UseFetchOptions = {}
): UseFetchResult<T> {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);
  const [isSuccess, setIsSuccess] = useState(false);

  const fetchData = useCallback(async () => {
    const controller = new AbortController();
    setLoading(true);
    setError(null);
    setIsSuccess(false);

    try {
      const response = await fetch(url, {
        signal: controller.signal
      });

      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }

      const result = await response.json();
      setData(result);
      setIsSuccess(true);
      options.onSuccess?.(result);
    } catch (err) {
      if (err.name !== 'AbortError') {
        const errorMessage = err instanceof Error ? err.message : 'An error occurred';
        setError(errorMessage);
        options.onError?.(err as Error);
      }
    } finally {
      setLoading(false);
    }

    return () => controller.abort();
  }, [url, options]);

  useEffect(() => {
    if (options.immediate !== false) {
      fetchData();
    }
  }, [fetchData, options.immediate, ...(options.dependencies || [])]);

  return {
    data,
    loading,
    error,
    refetch: fetchData,
    isSuccess
  };
}

// Usage
function ProductList() {
  const { data: products, loading, error, refetch } = useFetch<Product[]>(
    'https://api.example.com/products',
    {
      immediate: true,
      onSuccess: (data) => console.log('Products loaded:', data.length),
      onError: (error) => console.error('Failed to load:', error.message)
    }
  );

  return (
    <div>
      <button onClick={refetch} disabled={loading}>
        {loading ? 'Loading...' : 'Refresh'}
      </button>
      
      {error && <div className="error">{error}</div>}
      
      {loading && <div className="loading">Loading products...</div>}
      
      {products && (
        <ul>
          {products.map(product => (
            <li key={product.id}>{product.name}</li>
          ))}
        </ul>
      )}
    </div>
  );
}
```

**useFetch Best Practices:**
- Support cancellation
- Include refetch function
- Handle loading and error states
- Provide success callback
- Type-safe with generics
- Document options clearly

---

### 27. Separation of Concerns

**What is it?**
Separation of Concerns is a design principle where different parts of an application handle different responsibilities, making the code more maintainable and testable.

**Why we need it?**
- Better code organization
- Easier to maintain
- Easier to test
- Reusable components
- Clear responsibilities
- Better collaboration

**How it works?**
- Components handle UI only
- Services handle API calls
- Hooks handle state logic
- Utils handle helper functions
- Types handle TypeScript interfaces
- Each file has single responsibility

**File Structure:**
```
src/
├── components/       # UI components
├── pages/           # Page components
├── hooks/           # Custom hooks
├── services/        # API services
├── types/           # TypeScript interfaces
└── utils/           # Helper functions
```

**Why Separate API Logic from Components?**

1. **Testability:**
   - Services can be tested independently
   - Can mock services in component tests
   - Easier to write unit tests

2. **Reusability:**
   - Services can be used in multiple components
   - Don't duplicate API logic
   - Consistent API handling

3. **Maintainability:**
   - API changes in one place
   - Components unaffected by API changes
   - Easier to update endpoints

4. **Collaboration:**
   - Frontend dev works on components
   - Backend dev works on services
   - Clear ownership

5. **Performance:**
   - Can optimize API calls in services
   - Implement caching in services
   - Components stay lightweight

**Example:**

❌ **Wrong: API logic in component**
```typescript
function ProductList() {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    setLoading(true);
    fetch('/api/products')
      .then(res => res.json())
      .then(data => setProducts(data))
      .finally(() => setLoading(false));
  }, []);

  // Component is coupled to API
  return <div>{/* UI */}</div>;
}
```

✅ **Correct: Separated concerns**
```typescript
// services/product.service.ts
export const productService = {
  getProducts: () => fetch('/api/products').then(res => res.json())
};

// hooks/useProducts.ts
function useProducts() {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    setLoading(true);
    productService.getProducts()
      .then(setProducts)
      .finally(() => setLoading(false));
  }, []);

  return { products, loading };
}

// components/ProductList.tsx
function ProductList() {
  const { products, loading } = useProducts();
  // Component only handles UI
  return <div>{/* UI */}</div>;
}
```

**Separation of Concerns Best Practices:**
- One responsibility per file
- Services for API calls
- Hooks for state logic
- Components for UI only
- Types for interfaces
- Utils for helpers

---

## Practical Project: Products Application

Let's build a complete Products Application that demonstrates all the concepts we've learned with a professional project structure.

### Project Overview

**Features:**
- Fetch products from API
- Loading, error, empty states
- Product list display
- Product details view
- Search functionality
- Create new product
- Delete product
- Professional file structure

### Professional Structure

```
src/
├── components/
│   ├── ProductCard.tsx
│   ├── ProductList.tsx
│   ├── ProductDetails.tsx
│   ├── ProductForm.tsx
│   ├── SearchBar.tsx
│   └── LoadingSpinner.tsx
├── hooks/
│   └── useProducts.ts
├── services/
│   └── product.service.ts
├── types/
│   └── product.types.ts
├── utils/
│   └── api-client.ts
└── App.tsx
```

### Step 1: Project Setup

```bash
# Create new project
npm create vite@latest products-app -- --template react-ts

# Navigate to project
cd products-app

# Install dependencies
npm install axios

# Start development server
npm run dev
```

### Step 2: Define Types

Create `src/types/product.types.ts`:

```typescript
// src/types/product.types.ts
export interface Product {
  id: number;
  name: string;
  description: string;
  price: number;
  category: string;
  image: string;
  stock: number;
  active: boolean;
  createdAt: string;
  updatedAt: string;
}

export interface CreateProductDto {
  name: string;
  description: string;
  price: number;
  category: string;
  stock: number;
  image: string;
}

export interface UpdateProductDto {
  name?: string;
  description?: string;
  price?: number;
  category?: string;
  stock?: number;
  active?: boolean;
  image?: string;
}

export interface ProductListResponse {
  products: Product[];
  total: number;
  page: number;
  limit: number;
}
```

### Step 3: Create API Client Utility

Create `src/utils/api-client.ts`:

```typescript
// src/utils/api-client.ts
import axios, { AxiosInstance, AxiosRequestConfig, AxiosResponse } from 'axios';

class ApiClient {
  private client: AxiosInstance;

  constructor(baseURL: string) {
    this.client = axios.create({
      baseURL,
      timeout: 10000,
      headers: {
        'Content-Type': 'application/json'
      }
    });

    // Request interceptor
    this.client.interceptors.request.use(
      (config) => {
        console.log(`API Request: ${config.method?.toUpperCase()} ${config.url}`);
        return config;
      },
      (error) => {
        console.error('Request error:', error);
        return Promise.reject(error);
      }
    );

    // Response interceptor
    this.client.interceptors.response.use(
      (response) => {
        console.log(`API Response: ${response.status} ${response.config.url}`);
        return response;
      },
      (error) => {
        console.error('Response error:', error);
        return Promise.reject(error);
      }
    );
  }

  async get<T>(url: string, config?: AxiosRequestConfig): Promise<T> {
    const response: AxiosResponse<T> = await this.client.get(url, config);
    return response.data;
  }

  async post<T>(url: string, data?: any, config?: AxiosRequestConfig): Promise<T> {
    const response: AxiosResponse<T> = await this.client.post(url, data, config);
    return response.data;
  }

  async put<T>(url: string, data?: any, config?: AxiosRequestConfig): Promise<T> {
    const response: AxiosResponse<T> = await this.client.put(url, data, config);
    return response.data;
  }

  async patch<T>(url: string, data?: any, config?: AxiosRequestConfig): Promise<T> {
    const response: AxiosResponse<T> = await this.client.patch(url, data, config);
    return response.data;
  }

  async delete<T>(url: string, config?: AxiosRequestConfig): Promise<T> {
    const response: AxiosResponse<T> = await this.client.delete(url, config);
    return response.data;
  }
}

// Export singleton instance
export const apiClient = new ApiClient('https://dummyjsonapi.com');
```

### Step 4: Create Product Service

Create `src/services/product.service.ts`:

```typescript
// src/services/product.service.ts
import { apiClient } from '../utils/api-client';
import { Product, CreateProductDto, UpdateProductDto } from '../types/product.types';

class ProductService {
  async getProducts(): Promise<Product[]> {
    return apiClient.get<Product[]>('/products');
  }

  async getProduct(id: number): Promise<Product> {
    return apiClient.get<Product>(`/products/${id}`);
  }

  async createProduct(data: CreateProductDto): Promise<Product> {
    return apiClient.post<Product>('/products', data);
  }

  async updateProduct(id: number, data: UpdateProductDto): Promise<Product> {
    return apiClient.patch<Product>(`/products/${id}`, data);
  }

  async deleteProduct(id: number): Promise<void> {
    return apiClient.delete<void>(`/products/${id}`);
  }

  async searchProducts(query: string): Promise<Product[]> {
    return apiClient.get<Product[]>(`/products/search?q=${encodeURIComponent(query)}`);
  }
}

export const productService = new ProductService();
```

### Step 5: Create useFetch Hook

Create `src/hooks/useFetch.ts`:

```typescript
// src/hooks/useFetch.ts
import { useState, useEffect, useCallback } from 'react';

interface UseFetchOptions {
  immediate?: boolean;
  dependencies?: any[];
  onSuccess?: (data: any) => void;
  onError?: (error: Error) => void;
}

interface UseFetchResult<T> {
  data: T | null;
  loading: boolean;
  error: string | null;
  refetch: () => Promise<void>;
  isSuccess: boolean;
}

function useFetch<T = any>(
  fetchFunction: () => Promise<T>,
  options: UseFetchOptions = {}
): UseFetchResult<T> {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);
  const [isSuccess, setIsSuccess] = useState(false);

  const fetchData = useCallback(async () => {
    setLoading(true);
    setError(null);
    setIsSuccess(false);

    try {
      const result = await fetchFunction();
      setData(result);
      setIsSuccess(true);
      options.onSuccess?.(result);
    } catch (err) {
      const errorMessage = err instanceof Error ? err.message : 'An error occurred';
      setError(errorMessage);
      options.onError?.(err as Error);
    } finally {
      setLoading(false);
    }
  }, [fetchFunction, options]);

  useEffect(() => {
    if (options.immediate !== false) {
      fetchData();
    }
  }, [fetchData, options.immediate, ...(options.dependencies || [])]);

  return {
    data,
    loading,
    error,
    refetch: fetchData,
    isSuccess
  };
}

export default useFetch;
```

### Step 6: Create useProducts Hook

Create `src/hooks/useProducts.ts`:

```typescript
// src/hooks/useProducts.ts
import { useCallback } from 'react';
import useFetch from './useFetch';
import { productService } from '../services/product.service';
import { Product, CreateProductDto } from '../types/product.types';

function useProducts() {
  const {
    data: products,
    loading,
    error,
    refetch,
    isSuccess
  } = useFetch<Product[]>(
    () => productService.getProducts(),
    {
      immediate: true,
      onSuccess: (data) => console.log(`Loaded ${data.length} products`),
      onError: (error) => console.error('Failed to load products:', error.message)
    }
  );

  const createProduct = useCallback(async (data: CreateProductDto) => {
    const newProduct = await productService.createProduct(data);
    refetch();
    return newProduct;
  }, [refetch]);

  const deleteProduct = useCallback(async (id: number) => {
    await productService.deleteProduct(id);
    refetch();
  }, [refetch]);

  const searchProducts = useCallback(async (query: string) => {
    const results = await productService.searchProducts(query);
    return results;
  }, []);

  return {
    products,
    loading,
    error,
    isSuccess,
    refetch,
    createProduct,
    deleteProduct,
    searchProducts
  };
}

export default useProducts;
```

### Step 7: Create Loading Spinner Component

Create `src/components/LoadingSpinner.tsx`:

```typescript
// src/components/LoadingSpinner.tsx
function LoadingSpinner() {
  return (
    <div className="loading-spinner">
      <div className="spinner"></div>
      <p>Loading...</p>
    </div>
  );
}

export default LoadingSpinner;
```

### Step 8: Create Product Card Component

Create `src/components/ProductCard.tsx`:

```typescript
// src/components/ProductCard.tsx
import { Product } from '../types/product.types';

interface ProductCardProps {
  product: Product;
  onDelete: (id: number) => void;
  onSelect: (product: Product) => void;
}

function ProductCard({ product, onDelete, onSelect }: ProductCardProps) {
  return (
    <div className="product-card" onClick={() => onSelect(product)}>
      <div className="product-image">
        <img src={product.image} alt={product.name} />
        {!product.active && <div className="inactive-badge">Inactive</div>}
      </div>
      
      <div className="product-info">
        <span className="product-category">{product.category}</span>
        <h3 className="product-name">{product.name}</h3>
        <p className="product-description">{product.description}</p>
        <div className="product-meta">
          <span className="product-price">${product.price.toFixed(2)}</span>
          <span className="product-stock">
            {product.stock} in stock
          </span>
        </div>
      </div>

      <div className="product-actions">
        <button 
          className="delete-button"
          onClick={(e) => {
            e.stopPropagation();
            onDelete(product.id);
          }}
        >
          Delete
        </button>
      </div>
    </div>
  );
}

export default ProductCard;
```

### Step 9: Create Product List Component

Create `src/components/ProductList.tsx`:

```typescript
// src/components/ProductList.tsx
import ProductCard from './ProductCard';
import { Product } from '../types/product.types';

interface ProductListProps {
  products: Product[];
  loading: boolean;
  error: string | null;
  onProductDelete: (id: number) => void;
  onProductSelect: (product: Product) => void;
}

function ProductList({ 
  products, 
  loading, 
  error, 
  onProductDelete, 
  onProductSelect 
}: ProductListProps) {
  if (loading) {
    return (
      <div className="product-list">
        <div className="loading-state">
          <div className="spinner"></div>
          <p>Loading products...</p>
        </div>
      </div>
    );
  }

  if (error) {
    return (
      <div className="product-list">
        <div className="error-state">
          <div className="error-icon">⚠️</div>
          <div className="error-content">
            <h3>Error Loading Products</h3>
            <p>{error}</p>
            <button onClick={() => onProductSelect(products[0])}>
              Retry
            </button>
          </div>
        </div>
      </div>
    );
  }

  if (products.length === 0) {
    return (
      <div className="product-list">
        <div className="empty-state">
          <div className="empty-icon">📦</div>
          <h3>No Products Available</h3>
          <p>There are no products to display.</p>
        </div>
      </div>
    );
  }

  return (
    <div className="product-list">
      <p className="product-count">Showing {products.length} product(s)</p>
      <div className="products-grid">
        {products.map(product => (
          <ProductCard
            key={product.id}
            product={product}
            onDelete={onProductDelete}
            onSelect={onProductSelect}
          />
        ))}
      </div>
    </div>
  );
}

export default ProductList;
```

### Step 10: Create Product Details Component

Create `src/components/ProductDetails.tsx`:

```typescript
// src/components/ProductDetails.tsx
import { Product } from '../types/product.types';

interface ProductDetailsProps {
  product: Product | null;
  loading: boolean;
  error: string | null;
  onBack: () => void;
  onDelete: (id: number) => void;
}

function ProductDetails({ product, loading, error, onBack, onDelete }: ProductDetailsProps) {
  if (loading) {
    return (
      <div className="product-details">
        <div className="loading-state">
          <div className="spinner"></div>
          <p>Loading product details...</p>
        </div>
      </div>
    );
  }

  if (error) {
    return (
      <div className="product-details">
        <div className="error-state">
          <p>Error: {error}</p>
          <button onClick={onBack}>Back</button>
        </div>
      </div>
    );
  }

  if (!product) {
    return (
      <div className="product-details">
        <div className="empty-state">
          <p>Product not found</p>
          <button onClick={onBack}>Back</button>
        </div>
      </div>
    );
  }

  return (
    <div className="product-details">
      <button className="back-button" onClick={onBack}>
        ← Back to Products
      </button>

      <div className="details-content">
        <div className="product-header">
          <img src={product.image} alt={product.name} className="product-image-large" />
          <div className="product-header-info">
            <span className="product-category">{product.category}</span>
            <h1>{product.name}</h1>
            <p className="product-description">{product.description}</p>
            <div className="product-meta">
              <span className="product-price">${product.price.toFixed(2)}</span>
              <span className={`product-stock ${product.stock > 0 ? 'in-stock' : 'out-of-stock'}`}>
                {product.stock > 0 ? `${product.stock} in stock` : 'Out of stock'}
              </span>
              <span className={`product-status ${product.active ? 'active' : 'inactive'}`}>
                {product.active ? 'Active' : 'Inactive'}
              </span>
            </div>
          </div>
        </div>

        <div className="product-actions">
          <button 
            className="delete-button"
            onClick={() => onDelete(product.id)}
          >
            Delete Product
          </button>
        </div>
      </div>
    </div>
  );
}

export default ProductDetails;
```

### Step 11: Create Search Bar Component

Create `src/components/SearchBar.tsx`:

```typescript
// src/components/SearchBar.tsx
import { useState } from 'react';

interface SearchBarProps {
  onSearch: (query: string) => void;
  placeholder?: string;
}

function SearchBar({ onSearch, placeholder = 'Search products...' }: SearchBarProps) {
  const [query, setQuery] = useState('');

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    onSearch(query);
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setQuery(e.target.value);
  };

  const handleClear = () => {
    setQuery('');
    onSearch('');
  };

  return (
    <form className="search-bar" onSubmit={handleSubmit}>
      <input
        type="text"
        value={query}
        onChange={handleChange}
        placeholder={placeholder}
        className="search-input"
      />
      {query && (
        <button type="button" onClick={handleClear} className="clear-button">
          Clear
        </button>
      )}
      <button type="submit" className="search-button">
        Search
      </button>
    </form>
  );
}

export default SearchBar;
```

### Step 12: Create Product Form Component

Create `src/components/ProductForm.tsx`:

```typescript
// src/components/ProductForm.tsx
import { useState } from 'react';
import { CreateProductDto } from '../types/product.types';

interface ProductFormProps {
  onSubmit: (data: CreateProductDto) => void;
  onCancel: () => void;
}

function ProductForm({ onSubmit, onCancel }: ProductFormProps) {
  const [formData, setFormData] = useState<CreateProductDto>({
    name: '',
    description: '',
    price: 0,
    category: '',
    stock: 0,
    image: 'https://via.placeholder.com/150'
  });

  const [errors, setErrors] = useState<Record<string, string>>({});

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    
    // Clear error when user starts typing
    if (errors[name]) {
      setErrors(prev => ({ ...prev, [name]: '' }));
    }
  };

  const validateForm = (): boolean => {
    const newErrors: Record<string, string> = {};
    let isValid = true;

    if (!formData.name.trim()) {
      newErrors.name = 'Product name is required';
      isValid = false;
    }

    if (!formData.description.trim()) {
      newErrors.description = 'Description is required';
      isValid = false;
    }

    if (formData.price <= 0) {
      newErrors.price = 'Price must be greater than 0';
      isValid = false;
    }

    if (!formData.category.trim()) {
      newErrors.category = 'Category is required';
      isValid = false;
    }

    if (formData.stock < 0) {
      newErrors.stock = 'Stock cannot be negative';
      isValid = false;
    }

    setErrors(newErrors);
    return isValid;
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();

    if (!validateForm()) {
      return;
    }

    onSubmit(formData);
  };

  const isFormValid = Object.keys(errors).length === 0 &&
    formData.name.trim() !== '' &&
    formData.description.trim() !== '' &&
    formData.price > 0 &&
    formData.category.trim() !== '' &&
    formData.stock >= 0;

  return (
    <div className="product-form">
      <h2>Create New Product</h2>
      
      <form onSubmit={handleSubmit}>
        <div className="form-group">
          <label>Product Name</label>
          <input
            type="text"
            name="name"
            value={formData.name}
            onChange={handleChange}
            className={errors.name ? 'error' : ''}
          />
          {errors.name && <span className="error-message">{errors.name}</span>}
        </div>

        <div className="form-group">
          <label>Description</label>
          <textarea
            name="description"
            value={formData.description}
            onChange={(e) => setFormData({ ...formData, description: e.target.value })}
            rows={4}
            className={errors.description ? 'error' : ''}
          />
          {errors.description && <span className="error-message">{errors.description}</span>}
        </div>

        <div className="form-group">
          <label>Price</label>
          <input
            type="number"
            name="price"
            value={formData.price}
            onChange={handleChange}
            min="0"
            step="0.01"
            className={errors.price ? 'error' : ''}
          />
          {errors.price && <span className="error-message">{errors.price}</span>}
        </div>

        <div className="form-group">
          <label>Category</label>
          <input
            type="text"
            name="category"
            value={formData.category}
            onChange={handleChange}
            className={errors.category ? 'error' : ''}
          />
          {errors.category && <span className="error-message">{errors.category}</span>}
        </div>

        <div className="form-group">
          <label>Stock</label>
          <input
            type="number"
            name="stock"
            value={formData.stock}
            onChange={handleChange}
            min="0"
            className={errors.stock ? 'error' : ''}
          />
          {errors.stock && <span className="error-message">{errors.stock}</span>
        </div>

        <div className="form-actions">
          <button type="button" onClick={onCancel}>
            Cancel
          </button>
          <button type="submit" disabled={!isFormValid}>
            Create Product
          </button>
        </div>
      </form>
    </div>
  );
}

export default ProductForm;
```

### Step 13: Create Main App Component

Create `src/App.tsx`:

```typescript
// src/App.tsx
import { useState } from 'react';
import useProducts from './hooks/useProducts';
import ProductList from './components/ProductList';
import ProductDetails from './components/ProductDetails';
import ProductForm from './components/ProductForm';
import SearchBar from './components/SearchBar';
import { Product } from './types/product.types';

function App() {
  const [selectedProduct, setSelectedProduct] = useState<Product | null>(null);
  const [showCreateForm, setShowCreateForm] = useState(false);
  const [searchResults, setSearchResults] = useState<Product[] | null>(null);
  const [view, setView] = useState<'list' | 'details'>('list');

  const {
    products,
    loading,
    error,
    isSuccess,
    refetch,
    createProduct,
    deleteProduct,
    searchProducts
  } = useProducts();

  const handleProductSelect = (product: Product) => {
    setSelectedProduct(product);
    setView('details');
  };

  const handleProductDelete = (id: number) => {
    deleteProduct(id);
    if (selectedProduct?.id === id) {
      setSelectedProduct(null);
      setView('list');
    }
  };

  const handleBack = () => {
    setSelectedProduct(null);
    setView('list');
  };

  const handleSearch = async (query: string) => {
    if (query.trim() === '') {
      setSearchResults(null);
    } else {
      const results = await searchProducts(query);
      setSearchResults(results);
    }
  };

  const handleCreateProduct = async (data: any) => {
    await createProduct(data);
    setShowCreateForm(false);
  };

  const displayProducts = searchResults || products;

  return (
    <div className="app">
      <header className="app-header">
        <h1>Products Dashboard</h1>
        <button 
          className="create-button"
          onClick={() => setShowCreateForm(true)}
        >
          + Create Product
        </button>
      </header>

      {showCreateForm && (
        <div className="modal-overlay">
          <div className="modal">
            <ProductForm
              onSubmit={handleCreateProduct}
              onCancel={() => setShowCreateForm(false)}
            />
          </div>
        </div>
      )}

      {view === 'list' && (
        <div className="main-content">
          <SearchBar onSearch={handleSearch} />
          <ProductList
            products={displayProducts}
            loading={loading}
            error={error}
            onProductDelete={handleProductDelete}
            onProductSelect={handleProductSelect}
          />
        </div>
      )}

      {view === 'details' && selectedProduct && (
        <ProductDetails
          product={selectedProduct}
          loading={loading}
          error={error}
          onBack={handleBack}
          onDelete={handleProductDelete}
        />
      )}
    </div>
  );
}

export default App;
```

### Step 14: Add Styling

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
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.app-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
  padding: 20px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.app-header h1 {
  font-size: 24px;
  margin: 0;
}

.create-button {
  padding: 12px 24px;
  background: #667eea;
  color: white;
  border: none;
  border-radius: 6px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.3s;
}

.create-button:hover {
  background: #5568d3;
}

.main-content {
  background: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.search-bar {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.search-input {
  flex: 1;
  padding: 12px 16px;
  border: 2px solid #e0e0e0;
  border-radius: 6px;
  font-size: 16px;
}

.search-input:focus {
  outline: none;
  border-color: #667eea;
}

.search-button {
  padding: 12px 24px;
  background: #667eea;
  color: white;
  border: none;
  border-radius: 6px;
  font-weight: 600;
  cursor: pointer;
}

.clear-button {
  padding: 12px 16px;
  background: #e0e0e0;
  color: #333;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

.product-list {
  min-height: 400px;
}

.product-count {
  margin-bottom: 20px;
  color: #666;
  font-size: 14px;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
}

.product-card {
  background: white;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  overflow: hidden;
  cursor: pointer;
  transition: transform 0.3s, box-shadow 0.3s;
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
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

.inactive-badge {
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
  display: inline-block;
  padding: 4px 8px;
  background: #e0e0e0;
  border-radius: 12px;
  font-size: 12px;
  color: #666;
  margin-bottom: 8px;
}

.product-name {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 8px;
  color: #333;
}

.product-description {
  color: #666;
  font-size: 14px;
  margin-bottom: 12px;
  line-height: 1.4;
}

.product-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 12px;
}

.product-price {
  font-size: 20px;
  font-weight: bold;
  color: #2ecc71;
}

.product-stock {
  font-size: 14px;
  color: #666;
}

.product-stock.in-stock {
  color: #2ecc71;
}

.product-stock.out-of-stock {
  color: #e74c3c;
}

.product-status {
  font-size: 12px;
  padding: 4px 8px;
  border-radius: 12px;
}

.product-status.active {
  background: #d4edda;
  color: #2ecc71;
}

.product-status.inactive {
  background: #fee;
  color: #e74c3c;
}

.product-actions {
  padding: 16px;
  border-top: 1px solid #e0e0e0;
}

.delete-button {
  width: 100%;
  padding: 8px;
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 600;
  transition: background 0.3s;
}

.delete-button:hover {
  background: #c0392b;
}

.loading-state,
.error-state,
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 400px;
  text-align: center;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #667eea;
  border-top-color: transparent;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.error-icon,
.empty-icon {
  font-size: 48px;
  margin-bottom: 16px;
}

.error-content h3 {
  margin-bottom: 8px;
  color: #e74c3c;
}

.error-content p {
  color: #666;
  margin-bottom: 16px;
}

.empty-icon {
  color: #ccc;
}

.product-details {
  background: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.back-button {
  padding: 8px 16px;
  background: #667eea;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  margin-bottom: 20px;
  transition: background 0.3s;
}

.back-button:hover {
  background: #5568d3;
}

.details-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 30px;
}

.product-header {
  display: flex;
  gap: 30px;
}

.product-image-large {
  width: 100%;
  height: 300px;
  object-fit: cover;
  border-radius: 8px;
}

.product-header-info {
  flex: 1;
}

.product-header-info .product-category {
  display: inline-block;
  padding: 6px 12px;
  background: #e0e0e0;
  border-radius: 16px;
  font-size: 14px;
  color: #666;
  margin-bottom: 12px;
}

.product-header-info h1 {
  font-size: 32px;
  margin-bottom: 12px;
  color: #333;
}

.product-header-info .product-description {
  color: #666;
  font-size: 16px;
  line-height: 1.6;
  margin-bottom: 20px;
}

.product-header-info .product-meta {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
}

.product-header-info .product-price {
  font-size: 28px;
  font-weight: bold;
  color: #2ecc71;
}

.product-details .product-actions {
  grid-column: 1 / -1;
  margin-top: 20px;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.modal {
  background: white;
  border-radius: 12px;
  padding: 30px;
  max-width: 500px;
  width: 90%;
  max-height: 90vh;
  overflow-y: auto;
}

.product-form h2 {
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: 500;
  color: #333;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 12px;
  border: 2px solid #e0e0e0;
  border-radius: 6px;
  font-size: 16px;
}

.form-group input.error,
.form-group textarea.error {
  border-color: #e74c3c;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: @667eea;
}

.error-message {
  display: block;
  margin-top: 6px;
  color: #e74c3c;
  font-size: 14px;
}

.form-actions {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

.form-actions button {
  padding: 12px 24px;
  border: none;
  border-radius: 6px;
  font-weight: 600;
  cursor: pointer;
}

.form-actions button:first-child {
  background: #e0e0e0;
  color: #333;
}

.form-actions button:last-child {
  background: #667eea;
  color: white;
}

.form-actions button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

@media (max-width: 768px) {
  .details-content {
    grid-template-columns: 1fr;
  }

  .product-header {
    flex-direction: column;
  }

  .product-image-large {
    height: 200px;
  }

  .products-grid {
    grid-template-columns: 1fr;
  }
}
```

### Step 15: Key Concepts Demonstrated

**API Integration:**
- Fetch API with TypeScript
- Axios vs Fetch comparison
- Custom API service layer
- Request/response handling

**State Management:**
- Loading state management
- Error state handling
- Success state
- Empty state
- State in custom hooks

**useEffect + API:**
- Fetch on component mount
- Cleanup with AbortController
- Refetch functionality
- Dependency management

**Separation of Concerns:**
- Services for API logic
- Hooks for state logic
- Components for UI only
- Utils for helpers
- Types for interfaces

---

## Comprehensive Review

### Session Summary

In this session, we covered:

1. **API Fundamentals:** What is API, REST API, HTTP methods (GET, POST, PUT, PATCH, DELETE), HTTP status codes, and JSON
2. **Fetch API & Async/Await:** How to make HTTP requests, async/await syntax, and try/catch error handling
3. **API States:** Loading, error, success, and empty states with proper UI feedback
4. **useEffect + API:** Using useEffect to fetch data with proper cleanup and dependency management
5. **Axios:** Introduction to Axios, instances, base URL, headers, and authorization
6. **Custom Hooks & Separation:** Creating reusable hooks and separating API logic from components
7. **API Lifecycle:** Understanding the complete flow from component to server and back

### Key Takeaways

- APIs enable communication between client and server
- Use appropriate HTTP methods for different operations
- Always handle loading, error, success, and empty states
- useEffect with proper cleanup is essential for API calls
- Axios provides more features than native fetch
- Separate API logic into services for better organization
- Custom hooks make logic reusable
- Separation of concerns improves maintainability
- TypeScript interfaces ensure type safety

---

## 15 Student Questions

1. What is an API and why do we need it?
2. What is the difference between PUT and PATCH?
3. What are the main HTTP methods and when to use each?
4. What do HTTP status codes indicate?
5. How does async/await improve code readability?
6. Why is try/catch important in async operations?
7. What are the different API states and why do we need them?
8. How does useEffect handle API calls with cleanup?
9. What are the advantages of Axios over fetch?
10. Why should we separate API logic from components?
11. What is a custom hook and when should you create one?
12. How does the API lifecycle work?
13. What is the purpose of a base URL in Axios?
14. How do you handle authentication in API requests?
15. Why is separation of concerns important in React applications?

---

## 5 Interview Questions

### 1. Explain the difference between GET, POST, PUT, and PATCH HTTP methods.

**Answer:**
- **GET:** Retrieves data from server. Safe, idempotent, cacheable. No body.
- **POST:** Creates new resource. Not idempotent. Has body. Returns 201 Created.
- **PUT:** Replaces entire resource. Idempotent. Has body. Updates or creates.
- **PATCH:** Partially updates resource. Not necessarily idempotent. Has body. Updates only provided fields.

Example: Use GET to fetch user data, POST to create new user, PUT to replace entire user profile, PATCH to update just the email.

### 2. How do you handle loading, error, and empty states in React components?

**Answer:**
I use state management with useState:

```typescript
const [data, setData] = useState(null);
const [loading, setLoading] = useState(false);
const [error, setError] = useState(null);

useEffect(() => {
  const fetchData = async () => {
    setLoading(true);
    setError(null);
    try {
      const response = await fetch(url);
      const result = await response.json();
      setData(result);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };
  fetchData();
}, [url]);

// In JSX:
if (loading) return <LoadingSpinner />;
if (error) return <ErrorDisplay error={error} />;
if (!data || data.length === 0) return <EmptyState />;
return <DataDisplay data={data} />;
```

### 3. Why is separation of concerns important in React applications?

**Answer:**
Separation of concerns improves code organization by ensuring each part of the application has a single responsibility:

- **Components:** Handle UI only, no API logic
- **Services:** Handle API calls, no UI logic
- **Hooks:** Handle state logic, reusable across components
- **Types:** Define interfaces, no logic
- **Utils:** Helper functions, pure functions

Benefits:
- Easier to test (each part independently)
- More maintainable (changes isolated)
- More reusable (logic can be shared)
- Better collaboration (clear ownership)
- Better performance (can optimize independently)

### 4. How do you implement cleanup in useEffect for API calls?

**Answer:**
I use AbortController to cancel requests when the component unmounts:

```typescript
useEffect(() => {
  const controller = new AbortController();

  const fetchData = async () => {
    try {
      const response = await fetch(url, {
        signal: controller.signal
      });
      const data = await response.json();
      setData(data);
    } catch (err) {
      if (err.name !== 'AbortError') {
        setError(err.message);
      }
    }
  };

  fetchData();

  // Cleanup function
  return () => {
    controller.abort();
  };
}, [url]);
```

This prevents memory leaks and race conditions when components unmount before requests complete.

### 5. What are the advantages of using Axios over the native Fetch API?

**Answer:**
Axios provides several advantages:

1. **Automatic JSON parsing:** No need to call .json() manually
2. **Better error handling:** Automatic error object with detailed information
3. **Interceptors:** Request/response middleware for auth, logging, etc.
4. **Timeout support:** Built-in timeout configuration
5. **Request/Response transformation:** Automatic data transformation
6. **Browser compatibility:** Works in older browsers
7. **Upload progress:** Built-in upload progress tracking
8. **Automatic retries:** Can be configured with interceptors

Example:
```typescript
// Fetch - manual JSON parsing
const response = await fetch(url);
const data = await response.json();

// Axios - automatic JSON parsing
const { data } = await axios.get(url);
```

---

## 5 Practical Exercises

### Exercise 1: Build a User Management System

Create a user management system with:
- List all users
- Add new user
- Edit existing user
- Delete user
- Search users
- All states (loading, error, empty)

**Requirements:**
- Use professional structure
- Create user service
- Create useUsers hook
- TypeScript interfaces for all types
- Proper error handling

### Exercise 2: Implement Search with Debouncing

Add search functionality with debouncing to the Products application:
- Debounce search input (300ms delay)
- Show loading indicator during debounce
- Cancel previous search if new search starts
- Display search results

**Requirements:**
- Create useDebounce hook
- Use in SearchBar component
- Handle cancellations
- Show loading state

### Exercise 3: Create a Paginated List

Add pagination to the Products application:
- Page size selector (10, 20, 50)
- Page navigation (next/prev)
- Display current page info
- Cache previously loaded pages

**Requirements:**
- Add pagination to service
- Track current page state
- Implement page navigation
- Show loading state per page

### Exercise 4: Implement Error Boundaries

Add error boundaries to handle API errors gracefully:
- Create ErrorBoundary component
- Catch errors in API calls
- Display fallback UI
- Log errors for debugging

**Requirements:**
- Wrap components with ErrorBoundary
- Display error messages
- Add retry functionality
- Log errors to console

### Exercise 5: Create a Data Cache Hook

Create a useDataCache hook that:
- Caches API responses
- Returns cached data if available
- Supports cache invalidation
- Has cache TTL (time-to-live)
- Manual cache clear

**Requirements:**
- Use Map for cache storage
- Implement TTL logic
- Provide cache methods
- Handle cache size limits

---

## Homework

### Reading Assignment
1. Read the official Axios documentation
2. Read MDN documentation on Fetch API
3. Read React documentation on data fetching

### Practice Exercises
1. **API Refactoring:** Take a component with inline API calls and refactor it to use the separation of concerns pattern
2. **Custom Hook:** Create a useLocalStorage hook that persists and retrieves data from localStorage
3. **Error Handling:** Implement a global error handler for all API calls using Axios interceptors
4. **Optimization:** Add request caching to reduce unnecessary API calls
5. **Type Safety:** Add comprehensive TypeScript interfaces for all API responses

### Research Project
Research and write a brief comparison (500 words) of different state management solutions for API data:
- React Query (TanStack Query)
- SWR
- Redux Toolkit
- RTK Query
- Apollo Client

Include pros and cons of each and recommend use cases for each.

---

## API Challenge

### Advanced E-commerce API Integration

Build a complete e-commerce application with advanced API integration patterns.

#### Requirements:

1. **Product Management:**
   - Fetch products with pagination
   - Create, update, delete products
   - Bulk operations
   - Image upload
   - Category management

2. **Shopping Cart:**
   - Add items to cart
   - Update quantities
   - Remove items
   - Calculate totals
   - Persist cart to localStorage
   - Sync with server

3. **Order Processing:**
   - Create order from cart
   - Validate order
   - Process payment
   - Update inventory
   - Order history

4. **User Authentication:**
   - Login/logout
   - Token management
   - Protected routes
   - Token refresh
   - Session persistence

5. **Advanced Features:**
   - Optimistic UI updates
   - Request retry logic
   - Offline support
   - Request queuing
   - Real-time updates (polling or WebSocket)

#### Technical Requirements:

- Use professional file structure
- Create comprehensive API services
- Implement custom hooks for data fetching
- Use TypeScript with strict mode
- Handle all states (loading, error, success, empty)
- Implement proper cleanup
- Add request caching
- Use environment variables for configuration
- Implement error boundaries
- Add comprehensive logging

#### Bonus Features:
- Add unit tests for services and hooks
- Implement request mocking for development
- Add API performance monitoring
- Implement request batching
- Add request deduplication
- Create API mock server for testing

#### Evaluation Criteria:
- Code quality and organization
- Proper separation of concerns
- TypeScript type safety
- Error handling robustness
- State management
- Performance considerations
- User experience
- Documentation

This challenge will test your understanding of all API integration concepts and push you to apply them in a real-world e-commerce scenario.

---

## Common Mistakes to Avoid

### 1. Not cleaning up API requests
```tsx
// ❌ Wrong
useEffect(() => {
  fetch('/api/data').then(setData);
}, []);
```

### 2. Not handling loading states
```tsx
// ❌ Wrong
useEffect(() => {
  fetch('/api/data').then(setData);
}, []);
return <div>{data.map(/*...*/)}</div>; // Shows nothing while loading
```

### 3. Mixing API logic in components
```tsx
// ❌ Wrong
function Component() {
  useEffect(() => {
    fetch('/api/data').then(setData);
  }, []);
}
```

### 4. Not using functional updates in intervals
```tsx
// ❌ Wrong
useEffect(() => {
  setInterval(() => {
    setData(count + 1); // Stale count
  }, 1000);
}, []);
```

### 5. Not implementing error boundaries
```tsx
// ❌ Wrong
// Components crash on API errors
```

### 6. Not using TypeScript interfaces
```tsx
// ❌ Wrong
const [data, setData] = useState<any>();
```

### 7. Not implementing request cancellation
```tsx
// ❌ Wrong
// Requests continue after unmount
```

### 8. Not handling network errors
```tsx
// ❌ Wrong
// No offline handling
```

**Best Practices:**
- Always cleanup with AbortController
- Always show loading states
- Separate API logic into services
- Use functional updates
- Use TypeScript for type safety
- Implement error boundaries
- Cancel requests on unmount
- Handle network errors gracefully

---

## Additional Resources

- [Axios Documentation](https://axios-http.com/docs/intro)
- [Fetch API - MDN](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [React Data Fetching](https://react.dev/learn/learn-fetching)
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)
- [REST API Design Best Practices](https://restfulapi.net/)

---

**Congratulations on completing Session 5!** You now have a solid understanding of API integration in React with Fetch, Axios, custom hooks, and separation of concerns. Continue practicing with the exercises and challenge to reinforce these concepts before moving to the next session.