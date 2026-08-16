# Session 14: JSON, APIs & Asynchronous JavaScript

---

## 📚 Theory (1h)

### What is JSON

JSON (JavaScript Object Notation) is a lightweight data interchange format that is easy for humans to read and write, and easy for machines to parse and generate.

#### JSON vs JavaScript Objects

```javascript
// JavaScript Object (can have functions, undefined, etc.)
const jsObject = {
    name: "John",
    age: 30,
    greet: function() { return "Hello"; },
    active: true,
    address: undefined
};

// JSON (must be valid JSON)
const jsonString = {
    "name": "John",
    "age": 30,
    "active": true,
    "address": null  // No undefined, no functions
};

// JSON string
const jsonStringified = '{"name":"John","age":30,"active":true,"address":null}';
```

#### Key Differences

| Feature | JavaScript Object | JSON |
|---------|------------------|------|
| Functions | ✅ Allowed | ❌ Not allowed |
| undefined | ✅ Allowed | ❌ Use null |
| Comments | ✅ Allowed | ❌ Not allowed |
| Quotes | Optional on keys | Required on keys and strings |
| Trailing commas | ✅ Allowed | ❌ Not allowed |

### What is an API

API (Application Programming Interface) is a set of rules and protocols that allows different software applications to communicate with each other.

#### Web APIs

Web APIs allow your JavaScript code to interact with external services over HTTP.

```javascript
// Example: Fetching data from an API
fetch('https://api.example.com/users')
    .then(response => response.json())
    .then(data => console.log(data));
```

#### Common API Concepts

- **Endpoint**: The URL where the API is accessed
- **Request**: Sending data to the API
- **Response**: Receiving data from the API
- **HTTP Methods**: GET, POST, PUT, DELETE, etc.
- **Status Codes**: 200 (OK), 404 (Not Found), 500 (Server Error), etc.

### JSON.parse/stringify

#### JSON.parse()

Converts a JSON string into a JavaScript object.

```javascript
const jsonString = '{"name":"John","age":30}';
const jsObject = JSON.parse(jsonString);

console.log(jsObject.name); // "John"
console.log(jsObject.age);  // 30
```

#### JSON.stringify()

Converts a JavaScript object into a JSON string.

```javascript
const jsObject = { name: "John", age: 30 };
const jsonString = JSON.stringify(jsObject);

console.log(jsonString); // '{"name":"John","age":30}'
```

#### With Formatting

```javascript
const data = { name: "John", age: 30 };
const formatted = JSON.stringify(data, null, 2);

console.log(formatted);
/*
{
  "name": "John",
  "age": 30
}
*/
```

### Sync vs Async Programming

#### Synchronous Programming

Code executes line by line, blocking until each operation completes.

```javascript
console.log("Start");
console.log("Middle");
console.log("End");
// Output: Start, Middle, End (in order)
```

#### Asynchronous Programming

Code can execute without blocking, allowing other operations to run while waiting for long tasks.

```javascript
console.log("Start");
setTimeout(() => console.log("Middle"), 1000);
console.log("End");
// Output: Start, End, Middle (after 1 second)
```

### Call Stack & Web API

#### Call Stack

The call stack is a mechanism for the JavaScript interpreter to keep track of function calls.

```javascript
function first() {
    second();
    console.log("First");
}

function second() {
    third();
    console.log("Second");
}

function third() {
    console.log("Third");
}

first();
// Stack: first -> second -> third -> third completes -> second completes -> first completes
```

#### Web API

Web APIs are provided by the browser (setTimeout, fetch, DOM events, etc.) and handle asynchronous operations.

```javascript
console.log("Start");
setTimeout(() => console.log("Timeout"), 0);
console.log("End");
// Web API handles setTimeout, then puts callback in queue
```

### Event Loop & Callback Queue

#### Event Loop

The event loop is the mechanism that coordinates the call stack, callback queue, and Web APIs.

```
┌─────────────────────┐
│   Call Stack        │
├─────────────────────┤
│   Web APIs          │
├─────────────────────┤
│   Callback Queue    │
└─────────────────────┘
```

#### How It Works

1. Synchronous code runs on the call stack
2. Async operations are handled by Web APIs
3. When async operations complete, callbacks go to the callback queue
4. Event loop moves callbacks from queue to stack when stack is empty

```javascript
console.log("1"); // Stack: log("1")
setTimeout(() => console.log("2"), 0); // Web API: setTimeout
console.log("3"); // Stack: log("3")
// Event loop moves callback to stack when empty
// Output: 1, 3, 2
```

---

## 💻 Practical (1.5h)

### Exercise 1: JSON Operations

```javascript
// Exercise 1.1: JSON.parse
console.log("=== JSON.parse ===");

const jsonString = '{"name":"John","age":30,"city":"New York"}';
const user = JSON.parse(jsonString);

console.log("User:", user);
console.log("Name:", user.name);
console.log("Age:", user.age);

// Exercise 1.2: JSON.stringify
console.log("\n=== JSON.stringify ===");

const data = {
    name: "Jane",
    age: 25,
    active: true,
    skills: ["JavaScript", "Python"]
};

const jsonStringified = JSON.stringify(data);
console.log("Stringified:", jsonStringified);

// Exercise 1.3: JSON.stringify with formatting
console.log("\n=== JSON.stringify Formatted ===");

const formatted = JSON.stringify(data, null, 2);
console.log("Formatted:");
console.log(formatted);

// Exercise 1.4: JSON.stringify with replacer
console.log("\n=== JSON.stringify with Replacer ===");

const data2 = {
    name: "John",
    age: 30,
    password: "secret123",
    email: "john@example.com"
};

// Exclude password
const sanitized = JSON.stringify(data2, (key, value) => {
    if (key === "password") return undefined;
    return value;
}, 2);

console.log("Sanitized:", sanitized);

// Exercise 1.5: Error handling
console.log("\n=== JSON Error Handling ===");

const invalidJson = '{"name":"John",}'; // Invalid JSON

try {
    const parsed = JSON.parse(invalidJson);
} catch (error) {
    console.log("Parse error:", error.message);
}
```

### Exercise 2: What is AJAX

```javascript
// Exercise 2.1: Traditional AJAX with XMLHttpRequest
console.log("=== Traditional AJAX ===");

function makeAJAXRequest(url) {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        
        xhr.open("GET", url);
        xhr.onload = function() {
            if (xhr.status === 200) {
                resolve(JSON.parse(xhr.responseText));
            } else {
                reject(new Error(`HTTP error! status: ${xhr.status}`));
            }
        };
        xhr.onerror = function() {
            reject(new Error("Network error"));
        };
        xhr.send();
    });
}

// Usage (commented out - requires actual API)
// makeAJAXRequest("https://jsonplaceholder.typicode.com/users/1")
//     .then(data => console.log("AJAX data:", data))
//     .catch(error => console.log("AJAX error:", error));

// Exercise 2.2: AJAX with POST
console.log("\n=== AJAX POST ===");

function makeAJAXPost(url, data) {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        
        xhr.open("POST", url);
        xhr.setRequestHeader("Content-Type", "application/json");
        
        xhr.onload = function() {
            if (xhr.status === 201) {
                resolve(JSON.parse(xhr.responseText));
            } else {
                reject(new Error(`HTTP error! status: ${xhr.status}`));
            }
        };
        
        xhr.onerror = function() {
            reject(new Error("Network error"));
        };
        
        xhr.send(JSON.stringify(data));
    });
}

// Usage (commented out)
// makeAJAXPost("https://jsonplaceholder.typicode.com/posts", {
//     title: "Test Post",
//     body: "This is a test",
//     userId: 1
// })
//     .then(data => console.log("POST response:", data))
//     .catch(error => console.log("POST error:", error));

// Exercise 2.3: AJAX with query parameters
console.log("\n=== AJAX with Query Params ===");

function buildURL(baseURL, params) {
    const url = new URL(baseURL);
    Object.keys(params).forEach(key => {
        url.searchParams.append(key, params[key]);
    });
    return url.toString();
}

const url = buildURL("https://jsonplaceholder.typicode.com/posts", {
    userId: 1,
    _limit: 5
});

console.log("URL with params:", url);
```

### Exercise 3: Real API Request/Response

```javascript
// Exercise 3.1: Fetch API - GET request
console.log("=== Fetch API GET ===");

async function fetchUser(userId) {
    try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const data = await response.json();
        console.log("User data:", data);
        return data;
    } catch (error) {
        console.log("Fetch error:", error.message);
    }
}

// fetchUser(1);

// Exercise 3.2: Fetch API - POST request
console.log("\n=== Fetch API POST ===");

async function createPost(postData) {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(postData)
        });
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const data = await response.json();
        console.log("Created post:", data);
        return data;
    } catch (error) {
        console.log("POST error:", error.message);
    }
}

// createPost({
//     title: "My Post",
//     body: "This is my post content",
//     userId: 1
// });

// Exercise 3.3: Fetch API - PUT request
console.log("\n=== Fetch API PUT ===");

async function updatePost(postId, postData) {
    try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${postId}`, {
            method: "PUT",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(postData)
        });
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const data = await response.json();
        console.log("Updated post:", data);
        return data;
    } catch (error) {
        console.log("PUT error:", error.message);
    }
}

// updatePost(1, { title: "Updated Title", body: "Updated Body", userId: 1 });

// Exercise 3.4: Fetch API - DELETE request
console.log("\n=== Fetch API DELETE ===");

async function deletePost(postId) {
    try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${postId}`, {
            method: "DELETE"
        });
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        console.log("Post deleted successfully");
        return true;
    } catch (error) {
        console.log("DELETE error:", error.message);
    }
}

// deletePost(1);
```

### Exercise 4: Looping on Fetched Data

```javascript
// Exercise 4.1: Fetch and loop through array
console.log("=== Fetch and Loop ===");

async function fetchAndLoopUsers() {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users");
        const users = await response.json();
        
        console.log("Total users:", users.length);
        
        users.forEach(user => {
            console.log(`User: ${user.name} (${user.email})`);
        });
        
        return users;
    } catch (error) {
        console.log("Error:", error.message);
    }
}

// fetchAndLoopUsers();

// Exercise 4.2: Filter fetched data
console.log("\n=== Filter Fetched Data ===");

async function fetchActiveUsers() {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users");
        const users = await response.json();
        
        const activeUsers = users.filter(user => 
            user.company.name.includes("Group")
        );
        
        console.log("Active users:", activeUsers.length);
        activeUsers.forEach(user => {
            console.log(`- ${user.name} from ${user.company.name}`);
        });
        
        return activeUsers;
    } catch (error) {
        console.log("Error:", error.message);
    }
}

// fetchActiveUsers();

// Exercise 4.3: Transform fetched data
console.log("\n=== Transform Fetched Data ===");

async function fetchUserSummaries() {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users");
        const users = await response.json();
        
        const summaries = users.map(user => ({
            id: user.id,
            name: user.name,
            email: user.email,
            company: user.company.name
        }));
        
        console.log("User summaries:", summaries);
        return summaries;
    } catch (error) {
        console.log("Error:", error.message);
    }
}

// fetchUserSummaries();

// Exercise 4.4: Reduce fetched data
console.log("\n=== Reduce Fetched Data ===");

async function fetchUsersByCompany() {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users");
        const users = await response.json();
        
        const byCompany = users.reduce((acc, user) => {
            const company = user.company.name;
            if (!acc[company]) {
                acc[company] = [];
            }
            acc[company].push(user.name);
            return acc;
        }, {});
        
        console.log("Users by company:", byCompany);
        return byCompany;
    } catch (error) {
        console.log("Error:", error.message);
    }
}

// fetchUsersByCompany();
```

### Exercise 5: Callback Hell

```javascript
// Exercise 5.1: What is callback hell
console.log("=== Callback Hell Example ===");

// Simulating nested callbacks
function step1(callback) {
    console.log("Step 1");
    setTimeout(() => callback("Step 1 done"), 1000);
}

function step2(data, callback) {
    console.log("Step 2:", data);
    setTimeout(() => callback("Step 2 done"), 1000);
}

function step3(data, callback) {
    console.log("Step 3:", data);
    setTimeout(() => callback("Step 3 done"), 1000);
}

// Callback hell
step1((result1) => {
    step2(result1, (result2) => {
        step3(result2, (result3) => {
            console.log("Final:", result3);
        });
    });
});

// Exercise 5.2: Promises solution
console.log("\n=== Promises Solution ===");

function step1Promise() {
    return new Promise(resolve => {
        console.log("Step 1");
        setTimeout(() => resolve("Step 1 done"), 1000);
    });
}

function step2Promise(data) {
    return new Promise(resolve => {
        console.log("Step 2:", data);
        setTimeout(() => resolve("Step 2 done"), 1000);
    });
}

function step3Promise(data) {
    return new Promise(resolve => {
        console.log("Step 3:", data);
        setTimeout(() => resolve("Step 3 done"), 1000);
    });
}

// Chained promises
step1Promise()
    .then(result1 => step2Promise(result1))
    .then(result2 => step3Promise(result2))
    .then(result3 => console.log("Final:", result3));

// Exercise 5.3: async/await solution
console.log("\n=== async/await Solution ===");

async function runSteps() {
    const result1 = await step1Promise();
    const result2 = await step2Promise(result1);
    const result3 = await step3Promise(result2);
    console.log("Final:", result3);
}

// runSteps();
```

### Exercise 6: Promises (Intro, then/catch/finally)

```javascript
// Exercise 6.1: Creating Promises
console.log("=== Creating Promises ===");

function fetchUserPromise(userId) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (userId > 0) {
                resolve({ id: userId, name: "John Doe" });
            } else {
                reject(new Error("Invalid user ID"));
            }
        }, 1000);
    });
}

// Exercise 6.2: then/catch
console.log("\n=== then/catch ===");

fetchUserPromise(1)
    .then(user => {
        console.log("User found:", user);
        return user.name;
    })
    .then(name => {
        console.log("User name:", name);
    })
    .catch(error => {
        console.log("Error:", error.message);
    });

fetchUserPromise(-1)
    .then(user => console.log("User:", user))
    .catch(error => console.log("Error:", error.message));

// Exercise 6.3: finally
console.log("\n=== finally ===");

fetchUserPromise(1)
    .then(user => console.log("Success:", user.name))
    .catch(error => console.log("Error:", error.message))
    .finally(() => console.log("Operation complete"));

// Exercise 6.4: Promise chaining
console.log("\n=== Promise Chaining ===");

function fetchPosts() {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve([
                { id: 1, title: "Post 1" },
                { id: 2, title: "Post 2" }
            ]);
        }, 1000);
    });
}

function fetchComments(postId) {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve([
                { id: 1, postId: postId, text: "Comment 1" },
                { id: 2, postId: postId, text: "Comment 2" }
            ]);
        }, 1000);
    });
}

fetchPosts()
    .then(posts => {
        console.log("Posts:", posts);
        return fetchComments(posts[0].id);
    })
    .then(comments => {
        console.log("Comments:", comments);
    })
    .catch(error => {
        console.log("Error:", error.message);
    });
```

### Exercise 7: Fetch API

```javascript
// Exercise 7.1: Basic fetch
console.log("=== Basic Fetch ===");

async function basicFetch() {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
        const data = await response.json();
        console.log("Fetched data:", data);
    } catch (error) {
        console.log("Error:", error.message);
    }
}

// basicFetch();

// Exercise 7.2: Fetch with headers
console.log("\n=== Fetch with Headers ===");

async function fetchWithHeaders() {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/posts/1", {
            headers: {
                "Accept": "application/json",
                "Authorization": "Bearer token123"
            }
        });
        const data = await response.json();
        console.log("Data with headers:", data);
    } catch (error) {
        console.log("Error:", error.message);
    }
}

// fetchWithHeaders();

// Exercise 7.3: Fetch with query parameters
console.log("\n=== Fetch with Query Params ===");

async function fetchWithParams() {
    try {
        const url = new URL("https://jsonplaceholder.typicode.com/posts");
        url.searchParams.append("userId", "1");
        url.searchParams.append("_limit", "5");
        
        const response = await fetch(url);
        const data = await response.json();
        console.log("Data with params:", data);
    } catch (error) {
        console.log("Error:", error.message);
    }
}

// fetchWithParams();

// Exercise 7.4: Error handling with fetch
console.log("\n=== Fetch Error Handling ===");

async function fetchWithErrorHandling() {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/invalid");
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const data = await response.json();
        console.log("Data:", data);
    } catch (error) {
        console.log("Fetch error:", error.message);
    }
}

// fetchWithErrorHandling();
```

### Exercise 8: Promise.all/allSettled/race

```javascript
// Exercise 8.1: Promise.all
console.log("=== Promise.all ===");

async function fetchMultipleUsers() {
    try {
        const promises = [
            fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json()),
            fetch("https://jsonplaceholder.typicode.com/users/2").then(r => r.json()),
            fetch("https://jsonplaceholder.typicode.com/users/3").then(r => r.json())
        ];
        
        const users = await Promise.all(promises);
        console.log("All users:", users);
        return users;
    } catch (error) {
        console.log("Error:", error.message);
    }
}

// fetchMultipleUsers();

// Exercise 8.2: Promise.allSettled
console.log("\n=== Promise.allSettled ===");

async function fetchWithAllSettled() {
    try {
        const promises = [
            fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json()),
            fetch("https://jsonplaceholder.typicode.com/invalid").then(r => r.json()),
            fetch("https://jsonplaceholder.typicode.com/users/3").then(r => r.json())
        ];
        
        const results = await Promise.allSettled(promises);
        
        results.forEach((result, index) => {
            if (result.status === "fulfilled") {
                console.log(`Request ${index + 1} succeeded:`, result.value.name);
            } else {
                console.log(`Request ${index + 1} failed:`, result.reason.message);
            }
        });
        
        return results;
    } catch (error) {
        console.log("Error:", error.message);
    }
}

// fetchWithAllSettled();

// Exercise 8.3: Promise.race
console.log("\n=== Promise.race ===");

async function fetchWithRace() {
    try {
        const slowRequest = fetch("https://jsonplaceholder.typicode.com/users/1")
            .then(r => r.json());
        
        const fastRequest = new Promise(resolve => 
            setTimeout(() => resolve({ source: "timeout" }), 100)
        );
        
        const result = await Promise.race([slowRequest, fastRequest]);
        console.log("Race result:", result);
        return result;
    } catch (error) {
        console.log("Error:", error.message);
    }
}

// fetchWithRace();

// Exercise 8.4: Promise.any
console.log("\n=== Promise.any ===");

async function fetchWithAny() {
    try {
        const promises = [
            fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json()),
            fetch("https://jsonplaceholder.typicode.com/users/2").then(r => r.json()),
            fetch("https://jsonplaceholder.typicode.com/users/3").then(r => r.json())
        ];
        
        const firstSuccess = await Promise.any(promises);
        console.log("First successful:", firstSuccess);
        return firstSuccess;
    } catch (error) {
        console.log("All promises failed:", error.message);
    }
}

// fetchWithAny();
```

### Exercise 9: Complete Working Example

**Complete script.js:**
```javascript
// Session 14: JSON, APIs & Asynchronous JavaScript
// This script demonstrates JSON, API requests, and async patterns

console.log("=== Session 14: JSON, APIs & Async ===");

// 1. JSON operations
console.log("\n--- JSON Operations ---");
const data = { name: "John", age: 30 };
const jsonString = JSON.stringify(data);
const parsed = JSON.parse(jsonString);

console.log("Original:", data);
console.log("Stringified:", jsonString);
console.log("Parsed:", parsed);

// 2. Async demonstration
console.log("\n--- Async Demonstration ---");
console.log("Start");
setTimeout(() => console.log("Async operation"), 1000);
console.log("End");

// 3. Promise example
console.log("\n--- Promise Example ---");

function simulateFetch() {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve({ id: 1, name: "John" });
        }, 1000);
    });
}

simulateFetch()
    .then(data => console.log("Fetched:", data))
    .catch(error => console.log("Error:", error));

// 4. async/await example
console.log("\n--- async/await Example ===");

async function fetchData() {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
        const data = await response.json();
        console.log("User:", data.name);
    } catch (error) {
        console.log("Error:", error.message);
    }
}

fetchData();

console.log("\n=== Session 14 Complete ===");
```

**Complete index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 14 - JSON, APIs & Async</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        h1 {
            color: #333;
            text-align: center;
        }
        .section {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }
        .section h2 {
            color: #1890ff;
            margin-top: 0;
        }
        .demo {
            background: #f9f9f9;
            padding: 15px;
            border-radius: 4px;
            margin-top: 10px;
        }
        .demo input, .demo button, .demo select {
            padding: 8px;
            margin: 5px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        .demo button {
            background-color: #1890ff;
            color: white;
            border: none;
            cursor: pointer;
        }
        .demo button:hover {
            background-color: #0c7cd5;
        }
        .demo button:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }
        .output {
            background: #f0f0f0;
            padding: 15px;
            border-radius: 4px;
            font-family: monospace;
            white-space: pre-wrap;
            margin-top: 10px;
            max-height: 400px;
            overflow-y: auto;
        }
        .loading {
            color: #1890ff;
            font-style: italic;
        }
        .error {
            color: #ff4d4f;
        }
        .success {
            color: #52c41a;
        }
        .user-card {
            background: white;
            padding: 15px;
            border-radius: 4px;
            border: 1px solid #e8e8e8;
            margin: 10px 0;
        }
        .user-card h3 {
            margin: 0 0 10px 0;
            color: #333;
        }
    </style>
</head>
<body>
    <h1>Session 14: JSON, APIs & Asynchronous JavaScript</h1>
    
    <div class="section">
        <h2>Topics Covered</h2>
        <ul>
            <li>JSON (parse, stringify, syntax)</li>
            <li>APIs (REST, HTTP methods, status codes)</li>
            <li>Asynchronous programming (sync vs async)</li>
            <li>Call stack, Web API, Event loop</li>
            <li>AJAX & Fetch API</li>
            <li>Callback hell, Promises</li>
            <li>async/await</li>
            <li>Promise methods (all, allSettled, race)</li>
        </ul>
    </div>

    <div class="section">
        <h2>JSON Operations</h2>
        <div class="demo">
            <input type="text" id="jsonInput" placeholder='{"name":"John","age":30}'>
            <button onclick="parseJSON()">Parse JSON</button>
            <button onclick="stringifyJSON()">Stringify Object</button>
            <div id="jsonOutput" class="output">
                JSON operations result...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>API Request Demo</h2>
        <div class="demo">
            <select id="apiEndpoint">
                <option value="users">Users</option>
                <option value="posts">Posts</option>
                <option value="comments">Comments</option>
                <option value="albums">Albums</option>
                <option value="todos">Todos</option>
            </select>
            <input type="number" id="resourceId" placeholder="ID (optional)" min="1">
            <button onclick="fetchData()">Fetch Data</button>
            <button onclick="fetchMultiple()">Fetch Multiple</button>
            <div id="apiOutput" class="output">
                API response will appear here...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Promise Demo</h2>
        <div class="demo">
            <button onclick="demoPromise()">Demo Promise</button>
            <button onclick="demoPromiseAll()">Promise.all</button>
            <button onclick="demoPromiseRace()">Promise.race</button>
            <div id="promiseOutput" class="output">
                Promise demonstrations...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>async/await Demo</h2>
        <div class="demo">
            <button onclick="demoAsyncAwait()">async/await Example</button>
            <button onclick="demoErrorHandling()">Error Handling</button>
            <button onclick="demoParallelRequests()">Parallel Requests</button>
            <div id="asyncOutput" class="output">
                async/await demonstrations...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Users Display</h2>
        <div class="demo">
            <button onclick="displayUsers()">Load Users</button>
            <button onclick="displayUserDetails()">Load User Details</button>
            <div id="usersContainer">
                <!-- Users will be displayed here -->
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Console Output</h2>
        <p>Open the browser console (F12) to see all JavaScript examples.</p>
    </div>

    <script src="script.js" defer></script>
    <script>
        // JSON operations
        function parseJSON() {
            const input = document.getElementById("jsonInput").value;
            try {
                const parsed = JSON.parse(input);
                document.getElementById("jsonOutput").textContent = 
                    "Parsed:\n" + JSON.stringify(parsed, null, 2);
            } catch (error) {
                document.getElementById("jsonOutput").textContent = 
                    "Error: " + error.message;
            }
        }

        function stringifyJSON() {
            const obj = { name: "John", age: 30, active: true };
            const stringified = JSON.stringify(obj, null, 2);
            document.getElementById("jsonOutput").textContent = 
                "Stringified:\n" + stringified;
        }

        // API requests
        async function fetchData() {
            const endpoint = document.getElementById("apiEndpoint").value;
            const id = document.getElementById("resourceId").value;
            
            let url = `https://jsonplaceholder.typicode.com/${endpoint}`;
            if (id) {
                url += `/${id}`;
            }
            
            const output = document.getElementById("apiOutput");
            output.textContent = "Loading...";
            output.className = "output loading";
            
            try {
                const response = await fetch(url);
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                const data = await response.json();
                output.textContent = JSON.stringify(data, null, 2);
                output.className = "output success";
            } catch (error) {
                output.textContent = "Error: " + error.message;
                output.className = "output error";
            }
        }

        async function fetchMultiple() {
            const output = document.getElementById("apiOutput");
            output.textContent = "Loading multiple requests...";
            output.className = "output loading";
            
            try {
                const promises = [
                    fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json()),
                    fetch("https://jsonplaceholder.typicode.com/users/2").then(r => r.json()),
                    fetch("https://jsonplaceholder.typicode.com/users/3").then(r => r.json())
                ];
                
                const results = await Promise.all(promises);
                output.textContent = JSON.stringify(results, null, 2);
                output.className = "output success";
            } catch (error) {
                output.textContent = "Error: " + error.message;
                output.className = "output error";
            }
        }

        // Promise demos
        function demoPromise() {
            const output = document.getElementById("promiseOutput");
            output.textContent = "Loading...";
            
            const promise = new Promise((resolve, reject) => {
                setTimeout(() => {
                    resolve("Promise resolved!");
                }, 1000);
            });
            
            promise
                .then(result => {
                    output.textContent = "Result: " + result;
                })
                .catch(error => {
                    output.textContent = "Error: " + error.message;
                });
        }

        function demoPromiseAll() {
            const output = document.getElementById("promiseOutput");
            output.textContent = "Loading Promise.all...";
            
            const promises = [
                new Promise(resolve => setTimeout(() => resolve("Task 1"), 1000)),
                new Promise(resolve => setTimeout(() => resolve("Task 2"), 1500)),
                new Promise(resolve => setTimeout(() => resolve("Task 3"), 2000))
            ];
            
            Promise.all(promises)
                .then(results => {
                    output.textContent = "All completed:\n" + results.join("\n");
                })
                .catch(error => {
                    output.textContent = "Error: " + error.message;
                });
        }

        function demoPromiseRace() {
            const output = document.getElementById("promiseOutput");
            output.textContent = "Running race...";
            
            const promises = [
                new Promise(resolve => setTimeout(() => resolve("Fast - 500ms"), 500)),
                new Promise(resolve => setTimeout(() => resolve("Slow - 2000ms"), 2000)),
                new Promise(resolve => setTimeout(() => resolve("Medium - 1000ms"), 1000))
            ];
            
            Promise.race(promises)
                .then(result => {
                    output.textContent = "Winner: " + result;
                })
                .catch(error => {
                    output.textContent = "Error: " + error.message;
                });
        }

        // async/await demos
        async function demoAsyncAwait() {
            const output = document.getElementById("asyncOutput");
            output.textContent = "Loading...";
            
            try {
                const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
                const data = await response.json();
                output.textContent = "User: " + data.name + "\n" + JSON.stringify(data, null, 2);
            } catch (error) {
                output.textContent = "Error: " + error.message;
            }
        }

        async function demoErrorHandling() {
            const output = document.getElementById("asyncOutput");
            output.textContent = "Testing error handling...";
            
            try {
                const response = await fetch("https://jsonplaceholder.typicode.com/invalid");
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                const data = await response.json();
                output.textContent = JSON.stringify(data, null, 2);
            } catch (error) {
                output.textContent = "Caught error: " + error.message;
            }
        }

        async function demoParallelRequests() {
            const output = document.getElementById("asyncOutput");
            output.textContent = "Loading parallel requests...";
            
            try {
                const [users, posts] = await Promise.all([
                    fetch("https://jsonplaceholder.typicode.com/users").then(r => r.json()),
                    fetch("https://jsonplaceholder.typicode.com/posts").then(r => r.json())
                ]);
                
                output.textContent = 
                    `Users: ${users.length}\n` +
                    `Posts: ${posts.length}\n` +
                    `Total data points: ${users.length + posts.length}`;
            } catch (error) {
                output.textContent = "Error: " + error.message;
            }
        }

        // Display users
        async function displayUsers() {
            const container = document.getElementById("usersContainer");
            container.innerHTML = "Loading...";
            
            try {
                const response = await fetch("https://jsonplaceholder.typicode.com/users");
                const users = await response.json();
                
                container.innerHTML = "";
                users.forEach(user => {
                    const card = document.createElement("div");
                    card.className = "user-card";
                    card.innerHTML = `
                        <h3>${user.name}</h3>
                        <p><strong>Email:</strong> ${user.email}</p>
                        <p><strong>Company:</strong> ${user.company.name}</p>
                    `;
                    container.appendChild(card);
                });
            } catch (error) {
                container.innerHTML = "Error: " + error.message;
            }
        }

        async function displayUserDetails() {
            const container = document.getElementById("usersContainer");
            container.innerHTML = "Loading...";
            
            try {
                const [user, posts] = await Promise.all([
                    fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json()),
                    fetch("https://jsonplaceholder.typicode.com/posts?userId=1").then(r => r.json())
                ]);
                
                container.innerHTML = `
                    <div class="user-card">
                        <h3>${user.name}</h3>
                        <p><strong>Email:</strong> ${user.email}</p>
                        <p><strong>Phone:</strong> ${user.phone}</p>
                        <p><strong>Website:</strong> ${user.website}</p>
                        <p><strong>Posts:</strong> ${posts.length}</p>
                    </div>
                `;
            } catch (error) {
                container.innerHTML = "Error: " + error.message;
            }
        }
    </script>
</body>
</html>
```

---

## 📝 Review (0.5h)

### async/await Training

```javascript
// Exercise 1: Basic async/await
async function basicAsync() {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
        const user = await response.json();
        console.log("User:", user.name);
    } catch (error) {
        console.log("Error:", error.message);
    }
}

// Exercise 2: Sequential async operations
async function sequential() {
    const user1 = await fetch("https://jsonplaceholder.typicode.com/users/1")
        .then(r => r.json());
    const user2 = await fetch("https://jsonplaceholder.typicode.com/users/2")
        .then(r => r.json());
    console.log("Users:", user1.name, user2.name);
}

// Exercise 3: Parallel async operations
async function parallel() {
    const [user1, user2] = await Promise.all([
        fetch("https://jsonplaceholder.typicode.com/users/1").then(r => r.json()),
        fetch("https://jsonplaceholder.typicode.com/users/2").then(r => r.json())
    ]);
    console.log("Users:", user1.name, user2.name);
}
```

### try/catch/finally with Fetch

```javascript
async function fetchWithErrorHandling(url) {
    try {
        const response = await fetch(url);
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const data = await response.json();
        return data;
        
    } catch (error) {
        console.log("Fetch error:", error.message);
        throw error; // Re-throw if needed
        
    } finally {
        console.log("Fetch attempt complete");
    }
}

// Usage
fetchWithErrorHandling("https://jsonplaceholder.typicode.com/users/1")
    .then(data => console.log("Success:", data))
    .catch(error => console.log("Final error:", error.message));
```

### Wrap-up: JavaScript Phase Complete

Congratulations! You've completed the JavaScript phase of your training. Here's what you've mastered:

#### Core Fundamentals
- ✅ Variables, Data Types, Strings
- ✅ Operators, Numbers
- ✅ Conditionals
- ✅ Arrays
- ✅ Loops
- ✅ Functions

#### Advanced Concepts
- ✅ Scope, Arrow Functions, Higher-Order Functions
- ✅ Objects (OOP, Classes, Inheritance)
- ✅ DOM Manipulation
- ✅ BOM (Browser Object Model)
- ✅ Destructuring, Set, Map
- ✅ Regex, Date, Generators, Modules

#### Asynchronous Programming
- ✅ JSON
- ✅ APIs & AJAX
- ✅ Promises
- ✅ async/await
- ✅ Fetch API

#### Next Steps in Your Journey
1. 🚀 Practice with real-world projects
2. 🌐 Learn about frameworks (React, Vue, Angular)
3. 📦 Explore Node.js for backend development
4. 🧪 Practice with more complex APIs
5. 🎯 Build full-stack applications

---

## 🎯 Review Questions

1. **What is JSON?**
   - [ ] A programming language
   - [ ] A data interchange format
   - [ ] A database
   - [ ] A framework

2. **What does JSON.parse() do?**
   - [ ] Converts object to string
   - [ ] Converts string to object
   - [ ] Validates JSON
   - [ ] Formats JSON

3. **What is an API?**
   - [ ] A programming interface for software communication
   - [ ] A database
   - [ ] A programming language
   - [ ] A UI framework

4. **What is the difference between sync and async?**
   - [ ] No difference
   - [ ] Sync blocks, async doesn't
   - [ ] Async blocks, sync doesn't
   - [ ] Both block

5. **What is the event loop?**
   - [ ] A looping construct
   - [ ] Mechanism that coordinates call stack and callback queue
   - [ ] A database query
   - [ ] A UI component

6. **What is callback hell?**
   - [ ] A place where callbacks go
   - [ ] Deeply nested callbacks that are hard to read
   - [ ] A callback that errors
   - [ ] A callback that never executes

7. **What does Promise.all() do?**
   - [ ] Resolves when all promises resolve
   - [ ] Resolves when first promise resolves
   - [ ] Resolves when all promises settle
   - [ ] Rejects immediately

8. **What does async/await do?**
   - [ ] Makes code synchronous
   - [ ] Allows writing async code that looks synchronous
   - [ ] Makes code faster
   - [ ] Prevents errors

9. **What does try/catch do?**
   - [ ] Tries to catch errors
   - [ ] Handles errors gracefully
   - [ ] Creates errors
   - [ ] Ignores errors

10. **What does finally do?**
    - [ ] Runs only on success
    - [ ] Runs only on error
    - [ ] Runs regardless of success or error
    - [ ] Never runs

### Correct Answers

1. ✅ A data interchange format
2. ✅ Converts string to object
3. ✅ A programming interface for software communication
4. ✅ Sync blocks, async doesn't
5. ✅ Mechanism that coordinates call stack and callback queue
6. ✅ Deeply nested callbacks that are hard to read
7. ✅ Resolves when all promises resolve
8. ✅ Allows writing async code that looks synchronous
9. ✅ Handles errors gracefully
10. ✅ Runs regardless of success or error

---

## 📚 Additional Resources

- [MDN: JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- [MDN: Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [MDN: Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN: async/await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [JSONPlaceholder](https://jsonplaceholder.typicode.com/) - Free fake API for testing
- [REST API Tutorial](https://restfulapi.net/)
- [JavaScript.info: Promises](https://javascript.info/promise-basics)
- [JavaScript.info: async/await](https://javascript.info/async)

**🎉 Congratulations on completing the JavaScript phase! You now have a solid foundation to build modern web applications. The skills you've learned are essential for frontend development and will serve you well in your journey as a developer.** 💪🚀