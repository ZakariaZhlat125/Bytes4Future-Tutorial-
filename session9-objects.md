# Session 9: Objects

## 📚 Theory (1h)

### Object Introduction

Objects are fundamental data structures in JavaScript that store key-value pairs and represent complex entities.

#### What is an Object?

An object is a collection of related data and functionality. It stores properties (data) and methods (functions).

#### Creating Objects

**1. Object Literal (Most Common)**
```javascript
const person = {
    name: "John",
    age: 30,
    greet: function() {
        console.log("Hello!");
    }
};
```

**2. Using `new Object()`**
```javascript
const person = new Object();
person.name = "John";
person.age = 30;
person.greet = function() {
    console.log("Hello!");
};
```

**3. Constructor Function**
```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function() {
        console.log("Hello!");
    };
}

const person = new Person("John", 30);
```

**4. ES6 Classes**
```javascript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    greet() {
        console.log("Hello!");
    }
}

const person = new Person("John", 30);
```

#### Object Properties

Properties are the values associated with an object.

```javascript
const person = {
    name: "John",           // String property
    age: 30,               // Number property
    isStudent: true,       // Boolean property
    address: {             // Object property
        street: "123 Main St",
        city: "New York"
    },
    hobbies: ["reading", "coding"],  // Array property
    greet: function() {    // Method property
        console.log("Hello!");
    }
};
```

#### Accessing Properties

**Dot Notation**
```javascript
const person = {
    name: "John",
    age: 30
};

console.log(person.name);  // "John"
console.log(person.age);   // 30
```

**Bracket Notation**
```javascript
console.log(person["name"]);  // "John"
console.log(person["age"]);   // 30
```

### Dot vs Bracket Notation

#### Dot Notation

```javascript
const person = {
    name: "John",
    age: 30
};

// Dot notation
person.name = "Jane";
console.log(person.name);

// Dot notation is cleaner and more readable
// Preferred when property name is known
```

**Advantages:**
- More readable and concise
- Easier to write
- Better for static property names

**Limitations:**
- Cannot use property names with spaces or special characters
- Cannot use property names that are reserved words
- Cannot use variable names as properties

#### Bracket Notation

```javascript
const person = {
    name: "John",
    "first name": "John",  // Property with space
    "user-name": "john_doe"  // Property with hyphen
};

// Bracket notation
console.log(person["name"]);        // "John"
console.log(person["first name"]);  // "John"
console.log(person["user-name"]);   // "john_doe"

// Using variables
const propertyName = "name";
console.log(person[propertyName]);  // "John"
```

**Advantages:**
- Can use any string as property name
- Can use variables to access properties
- Can use property names with spaces/special characters

**When to Use:**
- Property name contains spaces or special characters
- Property name is a variable
- Property name is a reserved word

#### Comparison

```javascript
const person = {
    name: "John",
    age: 30,
    "first name": "John"
};

// Dot notation (preferred when possible)
console.log(person.name);  // "John"

// Bracket notation (required for special cases)
console.log(person["first name"]);  // "John"

// Dynamic property access
const prop = "age";
console.log(person[prop]);  // 30
```

### Nested Objects

Objects can contain other objects as properties, creating complex data structures.

#### Basic Nested Objects

```javascript
const person = {
    name: "John",
    age: 30,
    address: {
        street: "123 Main St",
        city: "New York",
        country: "USA",
        zipCode: "10001"
    },
    contact: {
        email: "john@example.com",
        phone: "555-1234"
    }
};

// Accessing nested properties
console.log(person.address.city);        // "New York"
console.log(person.contact.email);       // "john@example.com"
console.log(person["address"]["city"]);  // "New York"
```

#### Deeply Nested Objects

```javascript
const company = {
    name: "Tech Corp",
    departments: {
        engineering: {
            frontend: {
                teamLead: "Alice",
                developers: ["Bob", "Charlie"]
            },
            backend: {
                teamLead: "David",
                developers: ["Eve", "Frank"]
            }
        },
        marketing: {
            manager: "Grace",
            team: ["Henry", "Ivy"]
        }
    }
};

// Accessing deeply nested properties
console.log(company.departments.engineering.frontend.teamLead);  // "Alice"
console.log(company.departments.marketing.manager);               // "Grace"
```

#### Safe Property Access

```javascript
const person = {
    name: "John"
    // address property might not exist
};

// Unsafe - will throw error if address doesn't exist
// console.log(person.address.city);  // Error

// Safe - using optional chaining
console.log(person.address?.city);  // undefined

// Safe - using logical AND
console.log(person.address && person.address.city);  // undefined
```

### Creating Objects with `new`

The `new` keyword is used with constructor functions to create object instances.

#### Constructor Functions

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function() {
        console.log(`Hello, my name is ${this.name}`);
    };
}

const person1 = new Person("John", 30);
const person2 = new Person("Jane", 25);

console.log(person1.name);  // "John"
console.log(person2.name);  // "Jane"
person1.greet();           // "Hello, my name is John"
```

#### Built-in Object Constructors

```javascript
// Object
const obj = new Object();
obj.name = "John";

// Array
const arr = new Array(1, 2, 3);

// Date
const date = new Date();

// RegExp
const regex = new RegExp("pattern");

// Map
const map = new Map();

// Set
const set = new Set();
```

#### `new` with Built-in Types

```javascript
// String
const str1 = "Hello";
const str2 = new String("Hello");

console.log(typeof str1);  // "string"
console.log(typeof str2);  // "object"

// Number
const num1 = 42;
const num2 = new Number(42);

console.log(typeof num1);  // "number"
console.log(typeof num2);  // "object"

// Boolean
const bool1 = true;
const bool2 = new Boolean(true);

console.log(typeof bool1);  // "boolean"
console.log(typeof bool2);  // "object"
```

#### Object.create()

`Object.create()` creates a new object with a specified prototype.

```javascript
const personPrototype = {
    greet: function() {
        console.log(`Hello, my name is ${this.name}`);
    }
};

const person1 = Object.create(personPrototype);
person1.name = "John";
person1.greet();  // "Hello, my name is John"

const person2 = Object.create(personPrototype);
person2.name = "Jane";
person2.greet();  // "Hello, my name is Jane"
```

---

## 💻 Practical (1.5h)

### Exercise 1: Object Basics

```javascript
// Exercise 1.1: Creating objects
console.log("=== Creating Objects ===");

// Object literal
const person1 = {
    name: "John",
    age: 30,
    city: "New York"
};

console.log("Person 1:", person1);

// Using new Object()
const person2 = new Object();
person2.name = "Jane";
person2.age = 25;
person2.city = "Los Angeles";

console.log("Person 2:", person2);

// Exercise 1.2: Accessing properties
console.log("\n=== Accessing Properties ===");

const person = {
    name: "John",
    age: 30,
    "first name": "John",
    "last name": "Doe"
};

console.log("Dot notation:", person.name);
console.log("Bracket notation:", person["name"]);
console.log("Space property:", person["first name"]);
console.log("Hyphen property:", person["last name"]);

// Exercise 1.3: Adding and modifying properties
console.log("\n=== Adding and Modifying ===");

person.email = "john@example.com";
person.age = 31;
console.log("After changes:", person);

// Exercise 1.4: Deleting properties
console.log("\n=== Deleting Properties ===");

delete person.email;
console.log("After delete:", person);
```

### Exercise 2: Dot vs Bracket Notation

```javascript
// Exercise 2.1: When to use each
console.log("=== Dot vs Bracket Notation ===");

const user = {
    name: "John",
    age: 30,
    "user-id": 123,
    "first name": "John",
    "last name": "Doe"
};

// Dot notation (preferred)
console.log("Name:", user.name);
console.log("Age:", user.age);

// Bracket notation (required for special cases)
console.log("User ID:", user["user-id"]);
console.log("First name:", user["first name"]);

// Exercise 2.2: Dynamic property access
console.log("\n=== Dynamic Access ===");

const propertyName = "name";
console.log("Dynamic name:", user[propertyName]);

const properties = ["name", "age", "user-id"];
properties.forEach(prop => {
    console.log(`${prop}:`, user[prop]);
});

// Exercise 2.3: Computed property names
console.log("\n=== Computed Property Names ===");

const key = "dynamic";
const value = "I'm dynamic";

const obj = {
    [key]: value,
    [`computed_${key}`]: "Computed value"
};

console.log("Computed properties:", obj);
```

### Exercise 3: Nested Objects

```javascript
// Exercise 3.1: Basic nested objects
console.log("=== Nested Objects ===");

const person = {
    name: "John",
    age: 30,
    address: {
        street: "123 Main St",
        city: "New York",
        country: "USA",
        coordinates: {
            lat: 40.7128,
            lng: -74.0060
        }
    },
    contact: {
        email: "john@example.com",
        phone: "555-1234",
        social: {
            twitter: "@john",
            linkedin: "john-doe"
        }
    }
};

console.log("City:", person.address.city);
console.log("Latitude:", person.address.coordinates.lat);
console.log("Twitter:", person.contact.social.twitter);

// Exercise 3.2: Modifying nested properties
console.log("\n=== Modifying Nested ===");

person.address.city = "Boston";
person.contact.social.twitter = "@john_updated";
console.log("Modified person:", person);

// Exercise 3.3: Safe property access
console.log("\n=== Safe Property Access ===");

const user1 = {
    name: "John",
    address: {
        city: "New York"
    }
};

const user2 = {
    name: "Jane"
    // No address property
};

console.log("User1 city:", user1.address?.city);     // "New York"
console.log("User2 city:", user2.address?.city);     // undefined
console.log("User2 city (safe):", user2.address?.city ?? "Unknown");  // "Unknown"

// Exercise 3.4: Nested object operations
console.log("\n=== Nested Operations ===");

const company = {
    name: "Tech Corp",
    employees: [
        { name: "John", department: "Engineering", salary: 80000 },
        { name: "Jane", department: "Marketing", salary: 60000 },
        { name: "Bob", department: "Engineering", salary: 90000 }
    ],
    departments: {
        engineering: { budget: 500000 },
        marketing: { budget: 200000 }
    }
};

// Calculate total engineering salary
const engSalary = company.employees
    .filter(emp => emp.department === "Engineering")
    .reduce((sum, emp) => sum + emp.salary, 0);

console.log("Engineering total salary:", engSalary);
console.log("Engineering budget:", company.departments.engineering.budget);
```

### Exercise 4: Creating Objects with `new`

```javascript
// Exercise 4.1: Constructor function
console.log("=== Constructor Function ===");

function Car(make, model, year) {
    this.make = make;
    this.model = model;
    this.year = year;
    this.isRunning = false;
    
    this.start = function() {
        this.isRunning = true;
        console.log(`${this.make} ${this.model} started`);
    };
    
    this.stop = function() {
        this.isRunning = false;
        console.log(`${this.make} ${this.model} stopped`);
    };
    
    this.getInfo = function() {
        return `${this.year} ${this.make} ${this.model}`;
    };
}

const car1 = new Car("Toyota", "Camry", 2020);
const car2 = new Car("Honda", "Civic", 2021);

console.log("Car 1:", car1.getInfo());
console.log("Car 2:", car2.getInfo());
car1.start();
car2.start();

// Exercise 4.2: Constructor with methods in prototype
console.log("\n=== Prototype Methods ===");

function Animal(name, species) {
    this.name = name;
    this.species = species;
}

Animal.prototype.speak = function() {
    console.log(`${this.name} the ${this.species} makes a sound`);
};

Animal.prototype.eat = function(food) {
    console.log(`${this.name} eats ${food}`);
};

const dog = new Animal("Buddy", "Dog");
const cat = new Animal("Whiskers", "Cat");

dog.speak();
dog.eat("dog food");
cat.speak();
cat.eat("cat food");

// Exercise 4.3: Built-in constructors
console.log("\n=== Built-in Constructors ===");

const date = new Date();
console.log("Current date:", date.toLocaleDateString());

const map = new Map([
    ["name", "John"],
    ["age", 30]
]);
console.log("Map:", map.get("name"));

const set = new Set([1, 2, 3, 2, 1]);
console.log("Set:", [...set]);

// Exercise 4.4: Object.create()
console.log("\n=== Object.create() ===");

const personPrototype = {
    greet: function() {
        console.log(`Hello, I'm ${this.name}`);
    },
    introduce: function() {
        console.log(`I'm ${this.name}, ${this.age} years old`);
    }
};

const person1 = Object.create(personPrototype);
person1.name = "John";
person1.age = 30;

const person2 = Object.create(personPrototype);
person2.name = "Jane";
person2.age = 25;

person1.greet();
person1.introduce();
person2.greet();
person2.introduce();
```

### Exercise 5: this Keyword

```javascript
// Exercise 5.1: this in methods
console.log("=== this in Methods ===");

const person = {
    name: "John",
    age: 30,
    
    greet: function() {
        console.log(`Hello, I'm ${this.name}`);
    },
    
    getAge: () => {
        console.log(`Age: ${this.age}`); // this is not person
    }
};

person.greet();  // "Hello, I'm John"
person.getAge(); // "Age: undefined"

// Exercise 5.2: this in constructor functions
console.log("\n=== this in Constructor ===");

function Person(name, age) {
    this.name = name;
    this.age = age;
    
    this.greet = function() {
        console.log(`Hello, I'm ${this.name}`);
    };
}

const person1 = new Person("John", 30);
person1.greet();  // "Hello, I'm John"

// Exercise 5.3: this with call, apply, bind
console.log("\n=== call, apply, bind ===");

const person2 = {
    name: "Jane",
    age: 25
};

function greet(greeting) {
    console.log(`${greeting}, I'm ${this.name}`);
}

greet.call(person2, "Hello");           // "Hello, I'm Jane"
greet.apply(person2, ["Hi"]);            // "Hi, I'm Jane"

const boundGreet = greet.bind(person2, "Hey");
boundGreet();                             // "Hey, I'm Jane"

// Exercise 5.4: this in nested functions
console.log("\n=== this in Nested Functions ===");

const obj = {
    name: "John",
    
    outer: function() {
        console.log("Outer this:", this.name);
        
        const inner = () => {
            console.log("Inner this:", this.name); // Inherits from outer
        };
        
        inner();
    }
};

obj.outer();
```

### Exercise 6: Object.create()

```javascript
// Exercise 6.1: Basic Object.create()
console.log("=== Object.create() ===");

const prototype = {
    greet: function() {
        console.log(`Hello, I'm ${this.name}`);
    },
    farewell: function() {
        console.log(`Goodbye from ${this.name}`);
    }
};

const person1 = Object.create(prototype);
person1.name = "John";

const person2 = Object.create(prototype);
person2.name = "Jane";

person1.greet();
person2.farewell();

// Exercise 6.2: Object.create() with properties
console.log("\n=== Object.create() with Properties ===");

const person3 = Object.create(prototype, {
    name: {
        value: "Bob",
        writable: true,
        enumerable: true,
        configurable: true
    },
    age: {
        value: 35,
        writable: true,
        enumerable: true,
        configurable: true
    }
});

console.log("Person 3:", person3);
person3.greet();

// Exercise 6.3: Prototype chain
console.log("\n=== Prototype Chain ===");

const animal = {
    eat: function() {
        console.log(`${this.name} is eating`);
    }
};

const mammal = Object.create(animal);
mammal.giveBirth = function() {
    console.log(`${this.name} gave birth`);
};

const dog = Object.create(mammal);
dog.name = "Buddy";
dog.bark = function() {
    console.log(`${this.name} is barking`);
};

dog.eat();       // From animal prototype
dog.giveBirth();  // From mammal prototype
dog.bark();       // Own method

// Exercise 6.4: Object.create() vs new
console.log("\n=== Object.create() vs new ===");

// Using constructor
function Person(name) {
    this.name = name;
}
Person.prototype.greet = function() {
    console.log(`Hello, ${this.name}`);
};

const personA = new Person("John");
personA.greet();

// Using Object.create()
const personB = Object.create(Person.prototype);
Person.call(personB, "Jane");
personB.greet();
```

### Exercise 7: Object.assign()

```javascript
// Exercise 7.1: Basic Object.assign()
console.log("=== Object.assign() ===");

const target = { a: 1, b: 2 };
const source = { b: 3, c: 4 };

const result = Object.assign(target, source);
console.log("Result:", result);  // { a: 1, b: 3, c: 4 }
console.log("Target modified:", target);  // { a: 1, b: 3, c: 4 }

// Exercise 7.2: Cloning objects
console.log("\n=== Cloning Objects ===");

const original = { name: "John", age: 30 };
const clone = Object.assign({}, original);

console.log("Clone:", clone);
console.log("Same reference:", original === clone);  // false

clone.name = "Jane";
console.log("Original unchanged:", original);  // { name: "John", age: 30 }

// Exercise 7.3: Merging multiple objects
console.log("\n=== Merging Multiple Objects ===");

const obj1 = { a: 1 };
const obj2 = { b: 2 };
const obj3 = { c: 3 };

const merged = Object.assign({}, obj1, obj2, obj3);
console.log("Merged:", merged);  // { a: 1, b: 2, c: 3 }

// Exercise 7.4: Deep vs shallow copy
console.log("\n=== Shallow Copy Issue ===");

const original2 = {
    name: "John",
    address: {
        city: "New York"
    }
};

const shallowCopy = Object.assign({}, original2);
shallowCopy.address.city = "Boston";

console.log("Original city changed:", original2.address.city);  // "Boston"

// Deep copy solution
const deepCopy = JSON.parse(JSON.stringify(original2));
deepCopy.address.city = "Los Angeles";

console.log("Original city unchanged:", original2.address.city);  // "Boston"

// Exercise 7.5: Practical example - default configuration
console.log("\n=== Default Configuration ===");

const defaultConfig = {
    theme: "light",
    language: "en",
    notifications: true,
    autosave: true
};

const userConfig = {
    theme: "dark",
    notifications: false
};

const finalConfig = Object.assign({}, defaultConfig, userConfig);
console.log("Final config:", finalConfig);
// { theme: "dark", language: "en", notifications: false, autosave: true }
```

### Exercise 8: Practical Object Modeling

```javascript
// Exercise 8.1: User model
console.log("=== User Model ===");

class User {
    constructor(name, email, age) {
        this.name = name;
        this.email = email;
        this.age = age;
        this.createdAt = new Date();
    }
    
    getInfo() {
        return {
            name: this.name,
            email: this.email,
            age: this.age,
            memberSince: this.createdAt.toLocaleDateString()
        };
    }
    
    updateEmail(newEmail) {
        this.email = newEmail;
    }
    
    isAdult() {
        return this.age >= 18;
    }
}

const user1 = new User("John Doe", "john@example.com", 30);
const user2 = new User("Jane Smith", "jane@example.com", 17);

console.log("User 1 info:", user1.getInfo());
console.log("User 1 is adult:", user1.isAdult());
console.log("User 2 is adult:", user2.isAdult());

// Exercise 8.2: Product model
console.log("\n=== Product Model ===");

class Product {
    constructor(name, price, category, stock) {
        this.name = name;
        this.price = price;
        this.category = category;
        this.stock = stock;
        this.id = Date.now() + Math.random();
    }
    
    applyDiscount(discountPercentage) {
        const discount = this.price * (discountPercentage / 100);
        this.price -= discount;
        return this.price;
    }
    
    sell(quantity) {
        if (quantity > this.stock) {
            return "Insufficient stock";
        }
        this.stock -= quantity;
        return `Sold ${quantity} items. Remaining: ${this.stock}`;
    }
    
    restock(quantity) {
        this.stock += quantity;
        return `Restocked. New stock: ${this.stock}`;
    }
}

const product = new Product("Laptop", 999.99, "Electronics", 10);
console.log("Product:", product.name);
console.log("After 10% discount:", product.applyDiscount(10));
console.log("Sell 3:", product.sell(3));
console.log("Restock 5:", product.restock(5));

// Exercise 8.3: ShoppingCart model
console.log("\n=== Shopping Cart Model ===");

class ShoppingCart {
    constructor() {
        this.items = [];
        this.createdAt = new Date();
    }
    
    addItem(product, quantity) {
        const existingItem = this.items.find(item => item.product.id === product.id);
        
        if (existingItem) {
            existingItem.quantity += quantity;
        } else {
            this.items.push({ product, quantity });
        }
    }
    
    removeItem(productId) {
        this.items = this.items.filter(item => item.product.id !== productId);
    }
    
    getTotal() {
        return this.items.reduce((total, item) => 
            total + (item.product.price * item.quantity), 0
        );
    }
    
    getItemCount() {
        return this.items.reduce((count, item) => count + item.quantity, 0);
    }
    
    clear() {
        this.items = [];
    }
}

const cart = new ShoppingCart();
const laptop = new Product("Laptop", 999.99, "Electronics", 10);
const mouse = new Product("Mouse", 29.99, "Electronics", 50);

cart.addItem(laptop, 1);
cart.addItem(mouse, 2);
cart.addItem(mouse, 1);  // Add more of existing item

console.log("Cart total:", cart.getTotal());
console.log("Item count:", cart.getItemCount());
cart.removeItem(mouse.id);
console.log("After removal:", cart.getTotal());

// Exercise 8.4: Library model
console.log("\n=== Library Model ===");

class Book {
    constructor(title, author, isbn) {
        this.title = title;
        this.author = author;
        this.isbn = isbn;
        this.isAvailable = true;
    }
    
    borrow() {
        if (!this.isAvailable) {
            return "Book is already borrowed";
        }
        this.isAvailable = false;
        return `Borrowed: ${this.title}`;
    }
    
    return() {
        this.isAvailable = true;
        return `Returned: ${this.title}`;
    }
}

class Library {
    constructor(name) {
        this.name = name;
        this.books = [];
    }
    
    addBook(book) {
        this.books.push(book);
    }
    
    findBookByTitle(title) {
        return this.books.find(book => 
            book.title.toLowerCase() === title.toLowerCase()
        );
    }
    
    listAvailableBooks() {
        return this.books.filter(book => book.isAvailable);
    }
    
    borrowBook(title) {
        const book = this.findBookByTitle(title);
        if (!book) return "Book not found";
        return book.borrow();
    }
}

const library = new Library("City Library");
const book1 = new Book("JavaScript Guide", "John Doe", "123-456");
const book2 = new Book("Python Basics", "Jane Smith", "789-012");

library.addBook(book1);
library.addBook(book2);

console.log("Available books:", library.listAvailableBooks().length);
console.log("Borrow result:", library.borrowBook("JavaScript Guide"));
console.log("Available after borrow:", library.listAvailableBooks().length);
```

### Exercise 9: Complete Working Example

**Complete script.js:**
```javascript
// Session 9: Objects
// This script demonstrates object creation, manipulation, and methods

console.log("=== Session 9: Objects ===");

// 1. Object creation
console.log("\n--- Object Creation ---");
const person = {
    name: "John",
    age: 30,
    city: "New York",
    greet: function() {
        console.log(`Hello, I'm ${this.name}`);
    }
};

console.log("Person:", person);
person.greet();

// 2. Dot vs bracket notation
console.log("\n--- Dot vs Bracket Notation ---");
const user = {
    name: "John",
    "user-id": 123
};

console.log("Dot notation:", user.name);
console.log("Bracket notation:", user["user-id"]);

// 3. Nested objects
console.log("\n--- Nested Objects ---");
const company = {
    name: "Tech Corp",
    address: {
        city: "New York",
        country: "USA"
    }
};

console.log("Company city:", company.address.city);

// 4. this keyword
console.log("\n--- this Keyword ---");
const obj = {
    name: "John",
    showName: function() {
        console.log("Name:", this.name);
    }
};

obj.showName();

// 5. Object.create()
console.log("\n--- Object.create() ---");
const prototype = {
    greet: function() {
        console.log(`Hello, ${this.name}`);
    }
};

const person1 = Object.create(prototype);
person1.name = "Jane";
person1.greet();

// 6. Object.assign()
console.log("\n--- Object.assign() ---");
const target = { a: 1 };
const source = { b: 2 };
const merged = Object.assign(target, source);
console.log("Merged:", merged);

console.log("\n=== Session 9 Complete ===");
```

**Complete index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 9 - Objects</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 1000px;
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
        .demo input, .demo button {
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
        .output {
            background: #f0f0f0;
            padding: 15px;
            border-radius: 4px;
            font-family: monospace;
            white-space: pre-wrap;
            margin-top: 10px;
            max-height: 300px;
            overflow-y: auto;
        }
        .object-display {
            background: white;
            padding: 15px;
            border-radius: 4px;
            border: 1px solid #e8e8e8;
            margin-top: 10px;
        }
        .object-display pre {
            margin: 0;
            overflow-x: auto;
        }
    </style>
</head>
<body>
    <h1>Session 9: Objects</h1>
    
    <div class="section">
        <h2>Topics Covered</h2>
        <ul>
            <li>Object introduction and creation</li>
            <li>Dot vs bracket notation</li>
            <li>Nested objects</li>
            <li>Creating objects with new</li>
            <li>this keyword</li>
            <li>Object.create() and Object.assign()</li>
        </ul>
    </div>

    <div class="section">
        <h2>Object Creation Demo</h2>
        <div class="demo">
            <input type="text" id="objName" placeholder="Name">
            <input type="number" id="objAge" placeholder="Age">
            <input type="text" id="objCity" placeholder="City">
            <button onclick="createObject()">Create Object</button>
            <button onclick="createWithNew()">Create with new</button>
            <div id="objectOutput" class="object-display">
                <pre>Object will be displayed here...</pre>
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Dot vs Bracket Notation</h2>
        <div class="demo">
            <input type="text" id="propName" placeholder="Property name">
            <input type="text" id="propValue" placeholder="Property value">
            <button onclick="demoDotNotation()">Dot Notation</button>
            <button onclick="demoBracketNotation()">Bracket Notation</button>
            <div id="notationOutput" class="output">
                Click to see notation demonstration...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Nested Objects Demo</h2>
        <div class="demo">
            <button onclick="demoNestedObjects()">Create Nested Object</button>
            <button onclick="accessNested()">Access Nested Properties</button>
            <div id="nestedOutput" class="object-display">
                <pre>Nested object will be displayed here...</pre>
            </div>
        </div>
    </div>

    <div class="section">
        <h2>this Keyword Demo</h2>
        <div class="demo">
            <button onclick="demoThisMethod()">this in Method</button>
            <button onclick="demoThisConstructor()">this in Constructor</button>
            <button onclick="demoThisArrow()">this in Arrow Function</button>
            <div id="thisOutput" class="output">
                Click to see this demonstration...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Object.create() Demo</h2>
        <div class="demo">
            <button onclick="demoObjectCreate()">Create with Prototype</button>
            <button onclick="demoPrototypeChain()">Prototype Chain</button>
            <div id="createOutput" class="output">
                Click to see Object.create() demonstration...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Object.assign() Demo</h2>
        <div class="demo">
            <button onclick="demoObjectAssign()">Merge Objects</button>
            <button onclick="demoClone()">Clone Object</button>
            <button onclick="demoDeepCopy()">Deep Copy</button>
            <div id="assignOutput" class="output">
                Click to see Object.assign() demonstration...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Console Output</h2>
        <p>Open the browser console (F12) to see all JavaScript examples.</p>
    </div>

    <script src="script.js" defer></script>
    <script>
        // Object creation
        function createObject() {
            const name = document.getElementById('objName').value || 'John';
            const age = parseInt(document.getElementById('objAge').value) || 30;
            const city = document.getElementById('objCity').value || 'New York';
            
            const person = {
                name: name,
                age: age,
                city: city,
                createdAt: new Date()
            };
            
            document.getElementById('objectOutput').innerHTML = 
                '<pre>' + JSON.stringify(person, null, 2) + '</pre>';
        }

        function createWithNew() {
            function Person(name, age, city) {
                this.name = name;
                this.age = age;
                this.city = city;
                this.createdAt = new Date();
            }
            
            const name = document.getElementById('objName').value || 'John';
            const age = parseInt(document.getElementById('objAge').value) || 30;
            const city = document.getElementById('objCity').value || 'New York';
            
            const person = new Person(name, age, city);
            
            document.getElementById('objectOutput').innerHTML = 
                '<pre>' + JSON.stringify(person, null, 2) + '</pre>';
        }

        // Dot vs bracket notation
        function demoDotNotation() {
            const propName = document.getElementById('propName').value || 'name';
            const propValue = document.getElementById('propValue').value || 'John';
            
            const obj = {};
            obj[propName] = propValue;
            
            let output = "=== Dot Notation ===\n";
            output += `Created object: ${JSON.stringify(obj)}\n`;
            
            if (propName.indexOf(' ') === -1 && propName.indexOf('-') === -1) {
                output += `Access with dot: obj.${propName} = ${obj[propName]}\n`;
            } else {
                output += `Cannot use dot notation (property has special characters)\n`;
            }
            
            document.getElementById('notationOutput').textContent = output;
        }

        function demoBracketNotation() {
            const propName = document.getElementById('propName').value || 'name';
            const propValue = document.getElementById('propValue').value || 'John';
            
            const obj = {};
            obj[propName] = propValue;
            
            let output = "=== Bracket Notation ===\n";
            output += `Created object: ${JSON.stringify(obj)}\n`;
            output += `Access with brackets: obj["${propName}"] = ${obj[propName]}\n`;
            output += `Access with variable: obj[propName] = ${obj[propName]}\n`;
            
            document.getElementById('notationOutput').textContent = output;
        }

        // Nested objects
        function demoNestedObjects() {
            const company = {
                name: "Tech Corp",
                address: {
                    street: "123 Main St",
                    city: "New York",
                    country: "USA"
                },
                employees: {
                    engineering: {
                        count: 10,
                        lead: "Alice"
                    },
                    marketing: {
                        count: 5,
                        lead: "Bob"
                    }
                }
            };
            
            document.getElementById('nestedOutput').innerHTML = 
                '<pre>' + JSON.stringify(company, null, 2) + '</pre>';
        }

        function accessNested() {
            const company = {
                name: "Tech Corp",
                address: {
                    city: "New York",
                    country: "USA"
                }
            };
            
            let output = "=== Accessing Nested Properties ===\n";
            output += `Company name: ${company.name}\n`;
            output += `City: ${company.address.city}\n`;
            output += `Country: ${company.address.country}\n`;
            output += `Safe access: ${company.address?.zipCode ?? "Not provided"}\n`;
            
            document.getElementById('nestedOutput').innerHTML = 
                '<pre>' + output + '</pre>';
        }

        // this keyword
        function demoThisMethod() {
            const person = {
                name: "John",
                showName: function() {
                    return `Name: ${this.name}`;
                }
            };
            
            document.getElementById('thisOutput').textContent = 
                `this in method: ${person.showName()}`;
        }

        function demoThisConstructor() {
            function Person(name) {
                this.name = name;
                this.showName = function() {
                    return `Name: ${this.name}`;
                };
            }
            
            const person = new Person("Jane");
            document.getElementById('thisOutput').textContent = 
                `this in constructor: ${person.showName()}`;
        }

        function demoThisArrow() {
            const person = {
                name: "John",
                regular: function() {
                    return `Regular: ${this.name}`;
                },
                arrow: () => {
                    return `Arrow: ${this.name}`; // this is not person
                }
            };
            
            document.getElementById('thisOutput').textContent = 
                `Regular function: ${person.regular()}\nArrow function: ${person.arrow()}`;
        }

        // Object.create()
        function demoObjectCreate() {
            const prototype = {
                greet: function() {
                    return `Hello, ${this.name}`;
                },
                farewell: function() {
                    return `Goodbye, ${this.name}`;
                }
            };
            
            const person1 = Object.create(prototype);
            person1.name = "John";
            
            const person2 = Object.create(prototype);
            person2.name = "Jane";
            
            let output = "=== Object.create() ===\n";
            output += `Person 1: ${person1.greet()}\n`;
            output += `Person 2: ${person2.farewell()}\n`;
            output += `Same prototype: ${Object.getPrototypeOf(person1) === Object.getPrototypeOf(person2)}`;
            
            document.getElementById('createOutput').textContent = output;
        }

        function demoPrototypeChain() {
            const animal = {
                eat: function() {
                    return `${this.name} is eating`;
                }
            };
            
            const mammal = Object.create(animal);
            mammal.giveBirth = function() {
                return `${this.name} gave birth`;
            };
            
            const dog = Object.create(mammal);
            dog.name = "Buddy";
            dog.bark = function() {
                return `${this.name} is barking`;
            };
            
            let output = "=== Prototype Chain ===\n";
            output += `Eat (from animal): ${dog.eat()}\n`;
            output += `Give birth (from mammal): ${dog.giveBirth()}\n`;
            output += `Bark (own method): ${dog.bark()}`;
            
            document.getElementById('createOutput').textContent = output;
        }

        // Object.assign()
        function demoObjectAssign() {
            const target = { a: 1, b: 2 };
            const source = { b: 3, c: 4 };
            
            const result = Object.assign(target, source);
            
            let output = "=== Object.assign() ===\n";
            output += `Target: { a: 1, b: 2 }\n`;
            output += `Source: { b: 3, c: 4 }\n`;
            output += `Result: ${JSON.stringify(result)}\n`;
            output += `Target modified: ${JSON.stringify(target)}`;
            
            document.getElementById('assignOutput').textContent = output;
        }

        function demoClone() {
            const original = { name: "John", age: 30 };
            const clone = Object.assign({}, original);
            
            clone.name = "Jane";
            
            let output = "=== Shallow Clone ===\n";
            output += `Original: ${JSON.stringify(original)}\n`;
            output += `Clone: ${JSON.stringify(clone)}\n`;
            output += `Same reference: ${original === clone}`;
            
            document.getElementById('assignOutput').textContent = output;
        }

        function demoDeepCopy() {
            const original = {
                name: "John",
                address: {
                    city: "New York"
                }
            };
            
            const shallow = Object.assign({}, original);
            const deep = JSON.parse(JSON.stringify(original));
            
            shallow.address.city = "Boston";
            deep.address.city = "Los Angeles";
            
            let output = "=== Deep Copy ===\n";
            output += `Original city: ${original.address.city}\n`;
            output += `Shallow copy changed original: true\n`;
            output += `Deep copy did not change original: true`;
            
            document.getElementById('assignOutput').textContent = output;
        }
    </script>
</body>
</html>
```

---

## 📝 Review (0.5h)

### Q&A

**Q1: What is the difference between dot notation and bracket notation?**
A: Dot notation is cleaner and preferred for simple property names. Bracket notation is required for property names with spaces, special characters, or when using variables as property names.

**Q2: What is a nested object?**
A: A nested object is an object that contains other objects as properties, allowing you to represent complex, hierarchical data structures.

**Q3: What does the `new` keyword do?**
A: The `new` keyword creates a new instance of an object, calling the constructor function and setting `this` to the new object.

**Q4: What is `this` in JavaScript?**
A: `this` refers to the context in which a function is called. In methods, it refers to the object the method belongs to. Its value depends on how a function is called.

**Q5: What does `Object.create()` do?**
A: `Object.create()` creates a new object with a specified prototype, allowing for inheritance and shared methods between objects.

**Q6: What does `Object.assign()` do?**
A: `Object.assign()` copies all enumerable own properties from one or more source objects to a target object, returning the modified target object.

**Q7: What is the difference between shallow copy and deep copy?**
A: Shallow copy copies references to nested objects, so changes to nested objects affect both copies. Deep copy creates entirely new copies of nested objects.

**Q8: When should you use arrow functions vs regular functions for object methods?**
A: Use regular functions for object methods when you need access to `this` referring to the object. Arrow functions inherit `this` from the surrounding scope and don't have their own `this`.

**Q9: What is a constructor function?**
A: A constructor function is a special function used to create and initialize objects. It's called with the `new` keyword and typically capitalizes the first letter.

**Q10: How do you safely access nested object properties?**
A: Use optional chaining (`?.`) to safely access nested properties that might not exist, avoiding errors: `obj.address?.city`.

### Review Questions

1. **Which notation is preferred for simple property names?**
   - [ ] Bracket notation
   - [ ] Dot notation
   - [ ] Both are equal
   - [ ] Neither

2. **What does `new` do?**
   - [ ] Creates a new variable
   - [ ] Creates a new object instance
   - [ ] Deletes an object
   - [ ] Copies an object

3. **What is `this` in a method?**
   - [ ] Always the global object
   - [ ] The object the method belongs to
   - [ ] Always undefined
   - [ ] The function itself

4. **What does `Object.create()` do?**
   - [ ] Creates a new object with a prototype
   - [ ] Creates a deep copy
   - [ ] Merges objects
   - [ ] Deletes properties

5. **What does `Object.assign()` do?**
   - [ ] Creates a new object
   - [ ] Copies properties from source to target
   - [ ] Deletes properties
   - [ ] Creates a deep copy

6. **When is bracket notation required?**
   - [ ] Always
   - [ ] Never
   - [ ] For property names with spaces/special characters
   - [ ] Only for numbers

7. **What is a nested object?**
   - [ ] An object with many properties
   - [ ] An object containing other objects
   - [ ] An object with methods
   - [ ] An object with arrays

8. **What is optional chaining used for?**
   - [ ] Chaining methods
   - [ ] Safe property access
   - [ ] Creating objects
   - [ ] Deleting properties

9. **How do you create a deep copy?**
   - [ ] Object.assign()
   - [ ] Spread operator
   - [ ] JSON.parse(JSON.stringify())
   - [ ] Direct assignment

10. **What is a constructor function?**
    - [ ] A function that constructs strings
    - [ ] A function used with `new` to create objects
    - [ ] A function that deletes objects
    - [ ] A function that copies objects

### Correct Answers

1. ✅ Dot notation
2. ✅ Creates a new object instance
3. ✅ The object the method belongs to
4. ✅ Creates a new object with a prototype
5. ✅ Copies properties from source to target
6. ✅ For property names with spaces/special characters
7. ✅ An object containing other objects
8. ✅ Safe property access
9. ✅ JSON.parse(JSON.stringify())
10. ✅ A function used with `new` to create objects

---

## 🎯 Next Steps

1. ✅ Practice object creation with different methods
2. ✅ Master dot vs bracket notation
3. ✅ Work with nested objects
4. ✅ Understand `this` keyword in different contexts
5. ✅ Practice prototype-based inheritance
6. ✅ Learn about object methods (keys, values, entries)
7. ✅ Explore ES6 classes in depth

---

## 📚 Additional Resources

- [MDN: Working with Objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects)
- [MDN: Object.create()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
- [MDN: Object.assign()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
- [MDN: this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
- [JavaScript.info: Objects](https://javascript.info/object)
- [JavaScript.info: Prototypes](https://javascript.info/prototype-inheritance)

**Remember:** Objects are the foundation of JavaScript programming. Master them to build complex, maintainable applications! 💪