# Session 9: Objects — Active Learning Redesign

## Session Plan for the Instructor

- **Total time:** approximately 90 to 120 minutes
- **Pacing rule:** never explain theory continuously for more than 15–20 minutes. After every concept, students must predict, write, or fix code.
- **Pedagogical pattern for every topic:**
  1. **Problem** — a realistic mini-situation
  2. **Guess** — ask: "What do you expect to happen?"
  3. **Explain** — the shortest rule that fixes the problem
  4. **Code** — live code written in front of the students, step by step
  5. **Challenge** — students solve a small task on their own or in groups
  6. **Review** — discuss the answer and the most common mistake

### Competition and Points

- 1 point per correct prediction in the "Guess" phase.
- 1–3 points per completed challenge, depending on difficulty.
- A "Bug Hunter" badge for each student who finds and fixes an intentional error.
- Keep a simple tally on a shared board or in the chat.

### Instructor Questions to Ask During the Session

- "Which notation should we use here?"
- "What does `this` refer to?"
- "Where is the data stored?"
- "Is this a shallow or deep copy?"
- "Does this create a new object or modify the old one?"

---

## Part 0: Warm-Up — The Variable Explosion (5 minutes)

### Problem

A program stores user data in many separate variables.

```javascript
let userName = "John";
let userAge = 30;
let userEmail = "john@example.com";
```

### Guess

Ask: "What if we want to pass all this user data to a function? Is this the best way?"

### Explain

Objects group related data into one value. This makes it easier to organize, pass, and modify data.

### Live Code

```javascript
const user = {
  name: "John",
  age: 30,
  email: "john@example.com"
};

console.log(user.name);
console.log(user.email);
```

### Review

Objects are containers with named properties. `user.name` is dot notation.

---

## Part 1: Object Basics

### 1.1 Creating Objects

#### Problem

Store a product's name, price, and stock.

#### Live Code

```javascript
const product = {
  name: "Laptop",
  price: 999.99,
  inStock: true
};

console.log(product);
```

#### Challenge 1.1 — Make a User (individual, 3 minutes)

- **Requirement:** Create an object for a student with `name`, `age`, and `grade`.
- **Time limit:** 3 minutes

### 1.2 Dot vs Bracket Notation

#### Problem

Some property names have spaces or come from variables.

#### Live Code

```javascript
const user = {
  name: "John",
  "user-id": 123,
  "first name": "John"
};

// Dot notation
console.log(user.name);

// Bracket notation
console.log(user["user-id"]);
console.log(user["first name"]);

// Dynamic access
let prop = "name";
console.log(user[prop]);
```

#### Challenge 1.2 — Bracket Access (individual, 3 minutes)

- **Requirement:** Create an object with a property `"email address"` and access it with bracket notation.
- **Time limit:** 3 minutes

### 1.3 Adding, Modifying, and Deleting Properties

#### Live Code

```javascript
const car = {
  make: "Toyota",
  model: "Camry"
};

car.year = 2020;            // add
car.make = "Honda";         // modify
delete car.model;           // delete

console.log(car);
```

#### Challenge 1.3 — Modify Product (individual, 3 minutes)

- **Requirement:** Add `category` to `product`, change `price` to `899.99`, then delete `inStock`.
- **Time limit:** 3 minutes

---

## Part 2: Nested Objects and Safe Access

### 2.1 Nested Objects

#### Problem

A user has an address and contact info.

#### Live Code

```javascript
const person = {
  name: "John",
  address: {
    street: "123 Main St",
    city: "New York",
    country: "USA"
  },
  contact: {
    email: "john@example.com",
    phone: "555-1234"
  }
};

console.log(person.address.city);
console.log(person.contact.email);
```

#### Challenge 2.1 — Company (individual, 4 minutes)

- **Requirement:** Create a `company` object with nested `departments` (`engineering` and `marketing`).
- **Time limit:** 4 minutes

### 2.2 Optional Chaining

#### Problem

A nested property might not exist. How do we avoid errors?

#### Live Code

```javascript
const user1 = { name: "John", address: { city: "New York" } };
const user2 = { name: "Jane" };

console.log(user1.address?.city);          // "New York"
console.log(user2.address?.city);          // undefined
console.log(user2.address?.city ?? "Unknown"); // "Unknown"
```

#### Challenge 2.2 — Safe Access (individual, 3 minutes)

- **Requirement:** Use optional chaining to print `user.profile.bio` safely.
- **Time limit:** 3 minutes

---

## Part 3: Creating Objects with `new` and Constructors

### 3.1 Constructor Functions

#### Problem

We need many objects with the same shape.

#### Live Code

```javascript
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
  this.isRunning = false;

  this.start = function() {
    this.isRunning = true;
    console.log(`${this.make} started`);
  };
}

const car1 = new Car("Toyota", "Camry", 2020);
const car2 = new Car("Honda", "Civic", 2021);

console.log(car1.model);
car2.start();
```

#### Challenge 3.1 — User Constructor (individual, 4 minutes)

- **Requirement:** Write a `User` constructor with `name`, `email`, and a `greet` method.
- **Time limit:** 4 minutes

### 3.2 ES6 Classes

#### Live Code

```javascript
class Product {
  constructor(name, price, stock) {
    this.name = name;
    this.price = price;
    this.stock = stock;
  }

  applyDiscount(percent) {
    this.price -= this.price * percent / 100;
    return this.price;
  }

  sell(amount) {
    if (amount > this.stock) return "Not enough stock";
    this.stock -= amount;
    return `Sold ${amount}. Remaining: ${this.stock}`;
  }
}

const laptop = new Product("Laptop", 1000, 10);
console.log(laptop.applyDiscount(10));
console.log(laptop.sell(3));
```

#### Challenge 3.2 — Book Class (individual, 5 minutes)

- **Requirement:** Write a `Book` class with `title`, `author`, `isbn`, `isAvailable`, and `borrow()`/`return()` methods.
- **Time limit:** 5 minutes

---

## Part 4: `this` Keyword

### 4.1 `this` in Methods

#### Problem

A method needs to access its own object's data.

#### Live Code

```javascript
const person = {
  name: "John",
  greet: function() {
    console.log(`Hello, I'm ${this.name}`);
  },
  badGreet: () => {
    console.log(`Hello, I'm ${this.name}`);
  }
};

person.greet();    // "John"
person.badGreet(); // undefined
```

#### Challenge 4.1 — this Prediction (individual, 3 minutes)

- **Requirement:** Predict the output, then run:

```javascript
const obj = {
  value: 10,
  show: function() { console.log(this.value); }
};
obj.show();
```

- **Time limit:** 3 minutes

### 4.2 `call`, `apply`, `bind`

#### Live Code

```javascript
function greet(greeting) {
  console.log(`${greeting}, I'm ${this.name}`);
}

const person = { name: "Jane" };

greet.call(person, "Hello");
greet.apply(person, ["Hi"]);
const sayHey = greet.bind(person, "Hey");
sayHey();
```

---

## Part 5: Prototypes and `Object.create`

### 5.1 `Object.create`

#### Problem

Create an object that shares methods with another object.

#### Live Code

```javascript
const animal = {
  eat: function() {
    console.log(`${this.name} is eating`);
  }
};

const dog = Object.create(animal);
dog.name = "Buddy";
dog.bark = function() {
  console.log(`${this.name} is barking`);
};

dog.eat();  // From animal prototype
dog.bark(); // Own method
```

#### Challenge 5.1 — Prototype Chain (individual, 4 minutes)

- **Requirement:** Create a `vehicle` prototype with `drive`, then a `car` object that uses it.
- **Time limit:** 4 minutes

---

## Part 6: `Object.assign` and Copies

### 6.1 Merging Objects

#### Problem

Combine default and user settings.

#### Live Code

```javascript
const defaults = {
  theme: "light",
  language: "en",
  notifications: true
};

const userSettings = {
  theme: "dark",
  notifications: false
};

const final = Object.assign({}, defaults, userSettings);
console.log(final);
```

#### Challenge 6.1 — Merge Config (individual, 3 minutes)

- **Requirement:** Merge `defaultConfig` and `userConfig` into a new object.
- **Time limit:** 3 minutes

### 6.2 Shallow vs Deep Copy

#### Problem

Copy an object, but the nested data still points to the same place.

#### Live Code

```javascript
const original = {
  name: "John",
  address: { city: "New York" }
};

const shallow = Object.assign({}, original);
shallow.address.city = "Boston";
console.log(original.address.city); // "Boston" (same inner object)

const deep = JSON.parse(JSON.stringify(original));
deep.address.city = "Los Angeles";
console.log(original.address.city); // "Boston" (unaffected now)
```

#### Challenge 6.2 — Deep Copy (individual, 4 minutes)

- **Requirement:** Deep copy `original` and change the nested `city` without affecting the original.
- **Time limit:** 4 minutes

---

## Bug Hunt 1

### Problem

Find the bugs in this object code.

```javascript
const user = {
  name: "John",
  address: {
    city: "New York"
  }
};

console.log(user.address.country);

const copy = user;
copy.name = "Jane";
console.log(user.name);

const clone = Object.assign({}, user);
clone.address.city = "Boston";
console.log(user.address.city);
```

### Issues

1. `user.address.country` is `undefined` because `country` does not exist.
2. `copy = user` does not create a new object; both variables point to the same object.
3. `Object.assign` is a shallow copy, so changing `clone.address.city` also changes `user.address.city`.

### Fixed Version

```javascript
const user = {
  name: "John",
  address: {
    city: "New York"
  }
};

console.log(user.address.country); // undefined

const copy = { ...user };
copy.name = "Jane";
console.log(user.name); // "John"

const clone = JSON.parse(JSON.stringify(user));
clone.address.city = "Boston";
console.log(user.address.city); // "New York"
```

### Points

1 point per found issue.

---

## Part 7: Practical Object Models

### 7.1 User Model

#### Live Code

```javascript
class User {
  constructor(name, email, age) {
    this.name = name;
    this.email = email;
    this.age = age;
  }

  isAdult() {
    return this.age >= 18;
  }

  updateEmail(newEmail) {
    this.email = newEmail;
  }
}

const u1 = new User("John", "john@example.com", 30);
console.log(u1.isAdult()); // true
```

#### Challenge 7.1 — User Methods (individual, 4 minutes)

- **Requirement:** Add `getInfo()` to `User` that returns an object with `name`, `email`, and `isAdult`.
- **Time limit:** 4 minutes

### 7.2 Shopping Cart

#### Live Code

```javascript
class ShoppingCart {
  constructor() {
    this.items = [];
  }

  addItem(product, quantity) {
    const existing = this.items.find(item => item.product.id === product.id);
    if (existing) {
      existing.quantity += quantity;
    } else {
      this.items.push({ product, quantity });
    }
  }

  getTotal() {
    return this.items.reduce((sum, item) => sum + item.product.price * item.quantity, 0);
  }
}

const cart = new ShoppingCart();
cart.addItem({ id: 1, name: "Mouse", price: 29.99 }, 2);
cart.addItem({ id: 2, name: "Keyboard", price: 59.99 }, 1);
console.log(cart.getTotal());
```

#### Challenge 7.2 — Remove Item (individual, 5 minutes)

- **Requirement:** Add `removeItem(productId)` to `ShoppingCart`.
- **Time limit:** 5 minutes

### 7.3 Library and Book

#### Live Code

```javascript
class Book {
  constructor(title, author, isbn) {
    this.title = title;
    this.author = author;
    this.isbn = isbn;
    this.isAvailable = true;
  }

  borrow() {
    if (!this.isAvailable) return "Already borrowed";
    this.isAvailable = false;
    return `Borrowed: ${this.title}`;
  }

  returnBook() {
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

  listAvailable() {
    return this.books.filter(book => book.isAvailable);
  }

  findByTitle(title) {
    return this.books.find(book =>
      book.title.toLowerCase() === title.toLowerCase()
    );
  }

  borrowBook(title) {
    const book = this.findByTitle(title);
    if (!book) return "Not found";
    return book.borrow();
  }
}
```

---

## Bug Hunt 2

### Problem

Find the bugs in this class code.

```javascript
class Product {
  constructor(name, price) {
    name = name;
    price = price;
  }

  applyDiscount(percent) {
    price = price - (price * percent / 100);
    return price;
  }
}

const laptop = new Product("Laptop", 1000);
console.log(laptop.name);
console.log(laptop.applyDiscount(10));
```

### Issues

1. In the constructor, `name` and `price` are not assigned to `this`. Should be `this.name = name`.
2. In `applyDiscount`, `price` is not `this.price` and is not updated on the object.

### Fixed Version

```javascript
class Product {
  constructor(name, price) {
    this.name = name;
    this.price = price;
  }

  applyDiscount(percent) {
    this.price = this.price - (this.price * percent / 100);
    return this.price;
  }
}

const laptop = new Product("Laptop", 1000);
console.log(laptop.name);
console.log(laptop.applyDiscount(10));
```

### Points

1 point per found issue.

---

## Group Challenge: Object Builder Race

- **Time:** 12 minutes
- **Teams:** 2 or 3 students per team
- **Task:** Each team creates a class and demonstrates it.
- **Scoring:** 2 points per correct class, 1 point for correct usage.

### Tasks

1. `Rectangle` class with `width`, `height`, `area()`, and `perimeter()`.
2. `BankAccount` class with `deposit(amount)`, `withdraw(amount)`, `getBalance()`.
3. `Student` class with `name`, `scores[]`, and `getAverage()`.
4. `TodoList` class with `add(task)`, `remove(index)`, and `list()`.

### Instructor Answer Key

```javascript
class Rectangle {
  constructor(w, h) {
    this.w = w;
    this.h = h;
  }
  area() { return this.w * this.h; }
  perimeter() { return 2 * (this.w + this.h); }
}

class BankAccount {
  constructor(balance = 0) {
    this.balance = balance;
  }
  deposit(amount) {
    if (amount > 0) this.balance += amount;
    return this.balance;
  }
  withdraw(amount) {
    if (amount > 0 && amount <= this.balance) this.balance -= amount;
    return this.balance;
  }
  getBalance() { return this.balance; }
}

class Student {
  constructor(name) {
    this.name = name;
    this.scores = [];
  }
  addScore(score) { this.scores.push(score); }
  getAverage() {
    if (this.scores.length === 0) return 0;
    return this.scores.reduce((s, n) => s + n, 0) / this.scores.length;
  }
}

class TodoList {
  constructor() {
    this.tasks = [];
  }
  add(task) { this.tasks.push(task); }
  remove(index) { this.tasks.splice(index, 1); }
  list() { return this.tasks; }
}
```

---

## Individual Challenges — Progressive Difficulty

### Level 1: Make an Object (3 minutes)

- **Requirement:** Create an object representing a `book` with `title`, `author`, and `year`.

### Level 2: Dot and Bracket (3 minutes)

- **Requirement:** Create an object with `"book title"` and `"author-name"`. Access both using dot and bracket notation.

### Level 3: Nested (4 minutes)

- **Requirement:** Create a `store` object with nested `products` array. Print the first product name.

### Level 4: Constructor (4 minutes)

- **Requirement:** Write a `Movie(title, year)` constructor and create two instances.

### Level 5: Class (5 minutes)

- **Requirement:** Write a `Rectangle` class with `area()` and `perimeter()` methods.

### Level 6: Copy (5 minutes)

- **Requirement:** Create a deep copy of an object with a nested object.

---

## Mini Project: Library Management System

### Time

25 minutes

### Goal

Combine objects, classes, nested data, and methods in one HTML page.

### Requirements for the Students

1. Create an HTML page with inputs for:
   - Book title
   - Author
   - ISBN

2. Add buttons:
   - Add Book
   - Show Available Books
   - Borrow Book (by title)
   - Return Book (by title)

3. Use a `Library` class and `Book` class.

4. Display results in a `<ul>` or `<pre>`.

### Starter HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Library Manager</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 600px; margin: 20px auto; }
    .section { background: #f9f9f9; padding: 15px; margin-bottom: 20px; border-radius: 4px; }
    input, button { padding: 8px; margin: 5px; }
    .output { background: #f0f0f0; padding: 15px; border-radius: 4px; margin-top: 10px; }
  </style>
</head>
<body>
  <h1>Library Manager</h1>

  <div class="section">
    <input type="text" id="title" placeholder="Title" value="JavaScript Guide">
    <input type="text" id="author" placeholder="Author" value="John Doe">
    <input type="text" id="isbn" placeholder="ISBN" value="123">
    <button onclick="addBook()">Add Book</button>
  </div>

  <div class="section">
    <input type="text" id="actionTitle" placeholder="Book title" value="JavaScript Guide">
    <button onclick="borrowBook()">Borrow</button>
    <button onclick="returnBook()">Return</button>
    <button onclick="showAvailable()">Show Available</button>
    <div id="output" class="output"></div>
  </div>

  <script>
    class Book {
      constructor(title, author, isbn) {
        this.title = title;
        this.author = author;
        this.isbn = isbn;
        this.isAvailable = true;
      }

      borrow() {
        if (!this.isAvailable) return "Already borrowed";
        this.isAvailable = false;
        return `Borrowed: ${this.title}`;
      }

      returnBook() {
        this.isAvailable = true;
        return `Returned: ${this.title}`;
      }
    }

    class Library {
      constructor() {
        this.books = [];
      }

      addBook(book) {
        this.books.push(book);
      }

      listAvailable() {
        return this.books.filter(book => book.isAvailable);
      }

      findByTitle(title) {
        return this.books.find(book => book.title.toLowerCase() === title.toLowerCase());
      }

      borrowBook(title) {
        const book = this.findByTitle(title);
        if (!book) return "Not found";
        return book.borrow();
      }

      returnBook(title) {
        const book = this.findByTitle(title);
        if (!book) return "Not found";
        return book.returnBook();
      }
    }

    const library = new Library();

    function addBook() {
      const title = document.getElementById("title").value;
      const author = document.getElementById("author").value;
      const isbn = document.getElementById("isbn").value;
      library.addBook(new Book(title, author, isbn));
      showAvailable();
    }

    function borrowBook() {
      const title = document.getElementById("actionTitle").value;
      document.getElementById("output").textContent = library.borrowBook(title);
    }

    function returnBook() {
      const title = document.getElementById("actionTitle").value;
      document.getElementById("output").textContent = library.returnBook(title);
    }

    function showAvailable() {
      const books = library.listAvailable().map(book => `${book.title} by ${book.author}`).join("\n");
      document.getElementById("output").textContent = books || "No books available";
    }
  </script>
</body>
</html>
```

### Review Questions for the Mini Project

- "Why do we need `this` in the `Book` class?"
- "What is the difference between `find` and `filter`?"
- "What happens if a book is already borrowed?"

---

<details>
<summary>Trainer Solutions — Do Not Show Until Students Try</summary>

## Trainer Solutions — Do Not Show Until Students Try

### Challenge 1.2

```javascript
const user = {
  "email address": "john@example.com"
};
console.log(user["email address"]);
```

### Challenge 1.3

```javascript
product.category = "Electronics";
product.price = 899.99;
delete product.inStock;
console.log(product);
```

### Challenge 2.1

```javascript
const company = {
  name: "Tech Corp",
  departments: {
    engineering: { lead: "Alice" },
    marketing: { lead: "Bob" }
  }
};
console.log(company.departments.engineering.lead);
```

### Challenge 2.2

```javascript
console.log(user?.profile?.bio);
```

### Challenge 3.1

```javascript
function User(name, email) {
  this.name = name;
  this.email = email;
  this.greet = function() {
    return `Hello, I'm ${this.name}`;
  };
}

const u = new User("John", "john@example.com");
console.log(u.greet());
```

### Challenge 3.2

```javascript
class Book {
  constructor(title, author, isbn) {
    this.title = title;
    this.author = author;
    this.isbn = isbn;
    this.isAvailable = true;
  }

  borrow() {
    if (!this.isAvailable) return "Already borrowed";
    this.isAvailable = false;
    return `Borrowed: ${this.title}`;
  }

  returnBook() {
    this.isAvailable = true;
    return `Returned: ${this.title}`;
  }
}
```

### Challenge 4.1

Output: `10`. `this` refers to `obj` because `show` is a regular method.

### Challenge 5.1

```javascript
const vehicle = {
  drive: function() {
    console.log(`${this.name} is driving`);
  }
};

const car = Object.create(vehicle);
car.name = "Toyota";
car.drive();
```

### Challenge 6.1

```javascript
const defaultConfig = { theme: "light", notifications: true };
const userConfig = { theme: "dark" };
const final = Object.assign({}, defaultConfig, userConfig);
console.log(final);
```

### Challenge 6.2

```javascript
const deepCopy = JSON.parse(JSON.stringify(original));
deepCopy.address.city = "Los Angeles";
console.log(original.address.city); // original value
```

### Challenge 7.1

```javascript
getInfo() {
  return {
    name: this.name,
    email: this.email,
    isAdult: this.isAdult()
  };
}
```

### Challenge 7.2

```javascript
removeItem(productId) {
  this.items = this.items.filter(item => item.product.id !== productId);
}
```

### Individual Challenges Solutions

```javascript
// Level 1
const book = {
  title: "JavaScript Guide",
  author: "John Doe",
  year: 2024
};

// Level 2
const obj = {
  "book title": "JS",
  "author-name": "JD"
};
console.log(obj["book title"]); // bracket
console.log(obj["author-name"]); // bracket

// Level 3
const store = {
  products: [
    { name: "Laptop", price: 1000 },
    { name: "Mouse", price: 30 }
  ]
};
console.log(store.products[0].name);

// Level 4
function Movie(title, year) {
  this.title = title;
  this.year = year;
}
const m1 = new Movie("Inception", 2010);
const m2 = new Movie("Interstellar", 2014);

// Level 5
class Rectangle {
  constructor(w, h) {
    this.w = w;
    this.h = h;
  }
  area() { return this.w * this.h; }
  perimeter() { return 2 * (this.w + this.h); }
}

// Level 6
const original = { person: { name: "John" } };
const copy = JSON.parse(JSON.stringify(original));
copy.person.name = "Jane";
console.log(original.person.name); // "John"
```

</details>

---

## Review Questions

1. Which notation is preferred for simple property names?
   - [ ] Bracket notation
   - [x] Dot notation
   - [ ] Both are equal
   - [ ] Neither

2. What does `new` do?
   - [ ] Creates a new variable
   - [x] Creates a new object instance
   - [ ] Deletes an object
   - [ ] Copies an object

3. What is `this` in a method?
   - [ ] Always the global object
   - [x] The object the method belongs to
   - [ ] Always undefined
   - [ ] The function itself

4. What does `Object.create()` do?
   - [x] Creates a new object with a prototype
   - [ ] Creates a deep copy
   - [ ] Merges objects
   - [ ] Deletes properties

5. What does `Object.assign()` do?
   - [ ] Creates a new object
   - [x] Copies properties from source to target
   - [ ] Deletes properties
   - [ ] Creates a deep copy

6. When is bracket notation required?
   - [ ] Always
   - [ ] Never
   - [x] For property names with spaces/special characters
   - [ ] Only for numbers

7. What is a nested object?
   - [ ] An object with many properties
   - [x] An object containing other objects
   - [ ] An object with methods
   - [ ] An object with arrays

8. What is optional chaining used for?
   - [ ] Chaining methods
   - [x] Safe property access
   - [ ] Creating objects
   - [ ] Deleting properties

9. How do you create a deep copy?
   - [ ] Object.assign()
   - [ ] Spread operator
   - [x] JSON.parse(JSON.stringify())
   - [ ] Direct assignment

10. What is a constructor function?
    - [ ] A function that constructs strings
    - [x] A function used with `new` to create objects
    - [ ] A function that deletes objects
    - [ ] A function that copies objects

---

## Additional Resources

- [MDN: Working with Objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects)
- [MDN: Object.create()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
- [MDN: Object.assign()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
- [MDN: this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
- [JavaScript.info: Objects](https://javascript.info/object)
- [JavaScript.info: Prototypes](https://javascript.info/prototype-inheritance)
