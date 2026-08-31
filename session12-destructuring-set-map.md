# Session 12: Destructuring, Set & Map — Active Learning Redesign

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

- "Which variable will get which value?"
- "What does the rest operator collect?"
- "Does a Set keep duplicates?"
- "What can a Map key be?"
- "When should we use spread vs destructuring?"

---

## Part 0: Warm-Up — Many Variables (5 minutes)

### Problem

A function returns an array with three values. We want to use them in separate variables.

```javascript
const result = ["John", 30, "New York"];
```

### Guess

Ask: "How can we get `name`, `age`, and `city` without writing three separate index accesses?"

### Explain

Destructuring lets us unpack values from arrays and objects into variables in one line.

### Live Code

```javascript
const [name, age, city] = ["John", 30, "New York"];
console.log(name, age, city);
```

### Review

One line created three variables from one array.

---

## Part 1: Array Destructuring

### 1.1 Basic Array Destructuring

#### Problem

Extract values from an array.

#### Live Code

```javascript
const colors = ["red", "green", "blue"];
const [first, second, third] = colors;

console.log(first);
console.log(second);
console.log(third);
```

#### Challenge 1.1 — Destructure Coordinates (individual, 2 minutes)

- **Requirement:** Extract `x` and `y` from `const point = [10, 20]`.
- **Time limit:** 2 minutes

### 1.2 Skipping and Rest

#### Live Code

```javascript
const numbers = [1, 2, 3, 4, 5];

const [one, , three] = numbers;
console.log(one, three);

const [first, second, ...rest] = numbers;
console.log(first);
console.log(second);
console.log(rest);
```

#### Challenge 1.2 — Skip and Rest (individual, 3 minutes)

- **Requirement:** From `const values = [10, 20, 30, 40, 50]`, get the first, skip the second, and collect the rest.
- **Time limit:** 3 minutes

### 1.3 Swapping and Defaults

#### Live Code

```javascript
let a = 1;
let b = 2;
[a, b] = [b, a];
console.log(a, b);

const pair = [1];
const [x, y = 10] = pair;
console.log(x, y);
```

#### Challenge 1.3 — Swap and Default (individual, 3 minutes)

- **Requirement:** Swap `x` and `y`. Then destructure `const nums = [5]` and use a default of `0` for the second value.
- **Time limit:** 3 minutes

---

## Part 2: Object Destructuring

### 2.1 Basic Object Destructuring

#### Problem

Extract properties from an object without using dot notation repeatedly.

#### Live Code

```javascript
const user = { name: "John", age: 30, city: "New York" };
const { name, age, city } = user;

console.log(name, age, city);
```

#### Challenge 2.1 — Destructure User (individual, 2 minutes)

- **Requirement:** Extract `email` and `role` from `const account = { email: "john@example.com", role: "admin" }`.
- **Time limit:** 2 minutes

### 2.2 Renaming and Nested

#### Live Code

```javascript
const person = {
  name: "John",
  profile: {
    age: 30,
    address: { city: "New York" }
  }
};

const { name: fullName, profile: { age, address: { city } } } = person;
console.log(fullName, age, city);
```

#### Challenge 2.2 — Nested Destructuring (individual, 4 minutes)

- **Requirement:** From `const data = { product: { name: "Laptop", price: 1000 } }`, extract `name` and `price`.
- **Time limit:** 4 minutes

### 2.3 Defaults and Function Parameters

#### Live Code

```javascript
const config = { host: "localhost" };
const { host, port = 8080, ssl = false } = config;
console.log(host, port, ssl);

function createUser({ name = "Guest", age = 18 } = {}) {
  return { name, age };
}

console.log(createUser({ name: "Jane" }));
```

#### Challenge 2.3 — Defaults in Function (individual, 4 minutes)

- **Requirement:** Write `connect({ url = "localhost", port = 3000 } = {})` and call it with and without arguments.
- **Time limit:** 4 minutes

---

## Bug Hunt 1

### Problem

Find the bugs in this destructuring code.

```javascript
const user = {
  name: "John",
  address: {
    city: "New York"
  }
};

const { name, address: city } = user;
console.log(city);
```

### Issues

1. `address: city` renames `address` to `city`, so `city` is the whole `address` object, not the city string.

### Fixed Version

```javascript
const { name, address: { city } } = user;
console.log(city); // "New York"
```

### Points

1 point for finding the bug.

---

## Part 3: Set

### 3.1 Creating and Using Sets

#### Problem

Remove duplicate values from a list.

#### Live Code

```javascript
const numbers = [1, 2, 2, 3, 3, 3];
const unique = new Set(numbers);

console.log(unique);
console.log(unique.size);
console.log(unique.has(2));

unique.add(4);
unique.delete(1);
console.log(unique);
```

#### Challenge 3.1 — Unique Names (individual, 3 minutes)

- **Requirement:** Create a `Set` from `const names = ["Alice", "Bob", "Alice", "Carol"]` and log its size.
- **Time limit:** 3 minutes

### 3.2 Set Operations

#### Live Code

```javascript
const a = new Set([1, 2, 3]);
const b = new Set([3, 4, 5]);

const union = new Set([...a, ...b]);
const intersection = new Set([...a].filter(x => b.has(x)));
const difference = new Set([...a].filter(x => !b.has(x)));

console.log(union);
console.log(intersection);
console.log(difference);
```

#### Challenge 3.2 — Common Tags (individual, 4 minutes)

- **Requirement:** Find the intersection of `const tags1 = ["js", "dom", "api"]` and `const tags2 = ["api", "react", "js"]`.
- **Time limit:** 4 minutes

---

## Part 4: Map

### 4.1 Map Basics

#### Problem

Store key-value pairs where keys can be of any type and order matters.

#### Live Code

```javascript
const userMap = new Map();
userMap.set("name", "John");
userMap.set(1, "admin");
userMap.set(true, "verified");

console.log(userMap.get("name"));
console.log(userMap.has(1));
console.log(userMap.size);

userMap.delete(true);
```

#### Challenge 4.1 — Map of Users (individual, 3 minutes)

- **Requirement:** Create a `Map` with keys `1`, `2`, `3` and user objects as values. Use `get` to read user `2`.
- **Time limit:** 3 minutes

### 4.2 Map Iteration and Conversion

#### Live Code

```javascript
const map = new Map([["a", 1], ["b", 2]]);

for (const [key, value] of map) {
  console.log(key, value);
}

const keys = [...map.keys()];
const values = [...map.values()];

const obj = Object.fromEntries(map);
const map2 = new Map(Object.entries(obj));
```

#### Challenge 4.2 — Map to Object (individual, 4 minutes)

- **Requirement:** Convert `const map = new Map([["name", "John"], ["age", 30]])` to an object, then back to a Map.
- **Time limit:** 4 minutes

---

## Bug Hunt 2

### Problem

Find the bugs in this Map/Set code.

```javascript
const users = new Map();
users.set({ id: 1 }, "John");
users.set({ id: 2 }, "Jane");

const john = { id: 1 };
console.log(users.get(john));
```

### Issues

1. `{ id: 1 }` used as a key is a different object each time. `users.get(john)` uses a new object and returns `undefined` because object equality is by reference, not value.

### Fixed Version

Use the same object reference or use a primitive key.

```javascript
const users = new Map();
const johnKey = { id: 1 };
users.set(johnKey, "John");
console.log(users.get(johnKey));

// Or use a number/string key
users.set(1, "John");
console.log(users.get(1));
```

### Points

1 point for finding the issue.

---

## Part 5: Modern Array Helpers

### 5.1 Array.from

#### Problem

Turn something that is not an array into an array.

#### Live Code

```javascript
const str = "hello";
console.log(Array.from(str));

const numbers = [1, 2, 3];
console.log(Array.from(numbers, n => n * 2));

const range = Array.from({ length: 5 }, (_, i) => i + 1);
console.log(range);
```

#### Challenge 5.1 — Create Range (individual, 3 minutes)

- **Requirement:** Use `Array.from` to create `[10, 20, 30, 40, 50]`.
- **Time limit:** 3 minutes

### 5.2 Array.some and Array.every

#### Live Code

```javascript
const numbers = [1, 2, 3, 4, 5];

console.log(numbers.some(n => n > 4));   // true
console.log(numbers.every(n => n > 0));  // true
console.log(numbers.every(n => n > 2));  // false
```

#### Challenge 5.2 — Validate Users (individual, 4 minutes)

- **Requirement:** Check if any user in `const users = [{ active: true }, { active: false }]` is active. Check if all are active.
- **Time limit:** 4 minutes

### 5.3 Spread Syntax

#### Live Code

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log(combined);

const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3 };
const merged = { ...obj1, ...obj2 };
console.log(merged);

const copy = [...arr1];
console.log(copy);
```

#### Challenge 5.3 — Merge and Copy (individual, 4 minutes)

- **Requirement:** Merge `const defaults = { theme: "light" }` and `const user = { theme: "dark" }` with spread, then copy `const nums = [1, 2, 3]`.
- **Time limit:** 4 minutes

---

## Part 6: WeakSet and WeakMap (Brief)

### Live Code

```javascript
const weakSet = new WeakSet();
let obj = { id: 1 };
weakSet.add(obj);
console.log(weakSet.has(obj));

obj = null;
// The object can be garbage collected

const weakMap = new WeakMap();
const key = { id: 1 };
weakMap.set(key, "value");
console.log(weakMap.get(key));
```

### Explain

WeakSet and WeakMap only accept objects as keys. They do not prevent garbage collection. You cannot iterate over them.

#### Challenge 6.1 — When to Use WeakMap (individual, 3 minutes)

- **Requirement:** Explain one use case for `WeakMap` (e.g., private data for objects).
- **Time limit:** 3 minutes

---

## Group Challenge: Data Transformations

- **Time:** 12 minutes
- **Teams:** 2 or 3 students per team
- **Task:** Each team completes two of the tasks.
- **Scoring:** 2 points per correct solution.

### Tasks

1. Destructure `const data = { user: { name: "John", age: 30 }, posts: [{ title: "A" }, { title: "B" }] }` to get `name`, `age`, and the first `title`.
2. Create a `Set` of unique words from a sentence.
3. Build a `Map` that counts the frequency of each character in a string.
4. Use `some` and `every` to check if any or all numbers in `[2, 4, 6, 8]` are even.

### Instructor Answer Key

```javascript
// 1
const { user: { name, age }, posts: [{ title: firstTitle }] } = data;

// 2
const words = "hello world hello".split(" ");
const unique = new Set(words);

// 3
const counts = new Map();
for (const char of "abracadabra") {
  counts.set(char, (counts.get(char) || 0) + 1);
}

// 4
const nums = [2, 4, 6, 8];
console.log(nums.some(n => n % 2 === 0));  // true
console.log(nums.every(n => n % 2 === 0)); // true
```

---

## Individual Challenges — Progressive Difficulty

### Level 1: Basic Array Destructuring (2 minutes)

- **Requirement:** From `const rgb = [255, 128, 0]`, get `red`, `green`, and `blue`.

### Level 2: Object Destructuring (3 minutes)

- **Requirement:** From `const product = { name: "Laptop", price: 999 }`, get `name` and `price`.

### Level 3: Rest and Defaults (3 minutes)

- **Requirement:** From `const nums = [10, 20]`, get `first`, `second`, and `third = 0`.

### Level 4: Nested Object (4 minutes)

- **Requirement:** Extract `city` from `const user = { address: { city: "Paris" } }`.

### Level 5: Set (4 minutes)

- **Requirement:** Create a `Set` from `[1, 2, 2, 3]` and log its size.

### Level 6: Map (4 minutes)

- **Requirement:** Create a `Map` with two entries, iterate, and log each key-value pair.

### Level 7: Spread (4 minutes)

- **Requirement:** Copy `const arr = [1, 2, 3]` with spread, then add `4` to the end.

### Level 8: some/every (5 minutes)

- **Requirement:** Use `some` and `every` on `const scores = [85, 90, 78]` to check for passing (> 70) and excellent (> 80).

---

## Mini Project: Shopping Cart with Map and Set

### Time

25 minutes

### Goal

Build a small product catalog and cart using Map, Set, and destructuring.

### Requirements for the Students

1. Create an HTML page with:
   - Product name, price, quantity inputs
   - Add to Cart button
   - Cart display
   - Total display
   - Discount code input

2. Use a `Map` to store cart items with product name as the key.

3. Use a `Set` to track applied discount codes.

4. Display the cart and update the total when items change.

5. Allow removing items.

### Starter HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Shopping Cart</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 600px; margin: 20px auto; }
    .cart-item { display: flex; justify-content: space-between; padding: 8px; background: #f9f9f9; margin: 5px 0; }
    .output { margin-top: 20px; background: #f0f0f0; padding: 15px; }
    input, button { padding: 8px; margin: 5px; }
  </style>
</head>
<body>
  <h1>Shopping Cart</h1>

  <input type="text" id="name" placeholder="Product name" value="Laptop" />
  <input type="number" id="price" placeholder="Price" value="1000" />
  <input type="number" id="quantity" placeholder="Quantity" value="1" />
  <button id="addBtn">Add to Cart</button>

  <div id="cart" class="output"></div>
  <p id="total">Total: $0</p>

  <input type="text" id="discount" placeholder="Discount code" value="SAVE10" />
  <button id="applyDiscount">Apply Discount</button>

  <script>
    const cart = new Map();
    const appliedDiscounts = new Set();

    function renderCart() {
      const container = document.getElementById("cart");
      container.innerHTML = "";
      let total = 0;

      for (const [name, { price, quantity }] of cart) {
        const itemTotal = price * quantity;
        total += itemTotal;

        const div = document.createElement("div");
        div.className = "cart-item";
        div.innerHTML = `
          <span>${name} x ${quantity} @ $${price}</span>
          <button data-name="${name}">Remove</button>
        `;
        container.appendChild(div);
      }

      if (appliedDiscounts.has("SAVE10")) {
        total *= 0.9;
      }

      document.getElementById("total").textContent = `Total: $${total.toFixed(2)}`;
    }

    document.getElementById("addBtn").addEventListener("click", () => {
      const name = document.getElementById("name").value.trim();
      const price = parseFloat(document.getElementById("price").value);
      const quantity = parseInt(document.getElementById("quantity").value);

      if (!name || isNaN(price) || isNaN(quantity)) return;

      const existing = cart.get(name);
      if (existing) {
        existing.quantity += quantity;
      } else {
        cart.set(name, { price, quantity });
      }

      renderCart();
    });

    document.getElementById("cart").addEventListener("click", (event) => {
      if (event.target.tagName === "BUTTON") {
        const name = event.target.dataset.name;
        cart.delete(name);
        renderCart();
      }
    });

    document.getElementById("applyDiscount").addEventListener("click", () => {
      const code = document.getElementById("discount").value.trim();
      if (appliedDiscounts.has(code)) {
        alert("Discount already applied");
      } else {
        appliedDiscounts.add(code);
        renderCart();
      }
    });

    renderCart();
  </script>
</body>
</html>
```

### Review Questions for the Mini Project

- "Why did we use a `Map` for the cart?"
- "Why did we use a `Set` for discount codes?"
- "How does destructuring help in the `for...of` loop?"

---

<details>
<summary>Trainer Solutions — Do Not Show Until Students Try</summary>

## Trainer Solutions — Do Not Show Until Students Try

### Challenge 1.1

```javascript
const [x, y] = [10, 20];
```

### Challenge 1.2

```javascript
const [first, , ...rest] = values;
```

### Challenge 1.3

```javascript
let x = 1, y = 2;
[x, y] = [y, x];

const [x2, y2 = 0] = [5];
```

### Challenge 2.1

```javascript
const { email, role } = account;
```

### Challenge 2.2

```javascript
const { product: { name, price } } = data;
```

### Challenge 2.3

```javascript
function connect({ url = "localhost", port = 3000 } = {}) {
  return { url, port };
}

connect();
connect({ url: "example.com" });
```

### Challenge 3.1

```javascript
const unique = new Set(names);
console.log(unique.size);
```

### Challenge 3.2

```javascript
const set1 = new Set(tags1);
const set2 = new Set(tags2);
const common = new Set([...set1].filter(tag => set2.has(tag)));
```

### Challenge 4.1

```javascript
const users = new Map();
users.set(1, { name: "John" });
users.set(2, { name: "Jane" });
users.set(3, { name: "Bob" });
console.log(users.get(2));
```

### Challenge 4.2

```javascript
const obj = Object.fromEntries(map);
const map2 = new Map(Object.entries(obj));
```

### Challenge 5.1

```javascript
const range = Array.from({ length: 5 }, (_, i) => (i + 1) * 10);
```

### Challenge 5.2

```javascript
console.log(users.some(u => u.active));
console.log(users.every(u => u.active));
```

### Challenge 5.3

```javascript
const merged = { ...defaults, ...user };
const copy = [...nums];
```

### Challenge 6.1

Use `WeakMap` to attach private data to objects without preventing garbage collection.

### Individual Challenges Solutions

```javascript
// Level 1
const [red, green, blue] = [255, 128, 0];

// Level 2
const { name, price } = product;

// Level 3
const [first, second, third = 0] = [10, 20];

// Level 4
const { address: { city } } = user;

// Level 5
const unique = new Set([1, 2, 2, 3]);
console.log(unique.size);

// Level 6
const map = new Map([["a", 1], ["b", 2]]);
for (const [key, value] of map) {
  console.log(key, value);
}

// Level 7
const copy = [...arr, 4];

// Level 8
console.log(scores.some(s => s > 70));   // true
console.log(scores.every(s => s > 80));  // false
```

</details>

---

## Review Questions

1. What does array destructuring do?
   - [ ] Creates a new array
   - [x] Unpacks array values into variables
   - [ ] Deletes array elements
   - [ ] Sorts array elements

2. What is the rest operator in destructuring?
   - [ ] Removes elements
   - [x] Collects remaining elements
   - [ ] Duplicates elements
   - [ ] Reverses elements

3. What is the main difference between Set and WeakSet?
   - [ ] No difference
   - [x] Set can store any value, WeakSet only objects
   - [ ] WeakSet is iterable, Set is not
   - [ ] Set prevents garbage collection, WeakSet allows it

4. What is the main difference between Map and Object?
   - [ ] No difference
   - [x] Map can have any key type, Object keys are strings
   - [ ] Object is iterable, Map is not
   - [ ] Map maintains insertion order, Object may not

5. What does Array.from() do?
   - [x] Creates an array from an array-like object
   - [ ] Converts array to string
   - [ ] Sorts array elements
   - [ ] Deletes array elements

6. What does Array.some() return?
   - [ ] true if all elements pass condition
   - [x] true if any element passes condition
   - [ ] The first element
   - [ ] All elements

7. What does Array.every() return?
   - [x] true if all elements pass condition
   - [ ] true if any element passes condition
   - [ ] The first element
   - [ ] All elements

8. What does the spread operator do?
   - [ ] Removes elements
   - [x] Expands an iterable into individual elements
   - [ ] Sorts elements
   - [ ] Filters elements

9. What does copyWithin() do?
   - [ ] Copies array from another array
   - [x] Copies array elements within the same array
   - [ ] Removes array elements
   - [ ] Reverses array elements

10. Can you destructure objects in function parameters?
    - [ ] No
    - [x] Yes
    - [ ] Only with arrays
    - [ ] Only with primitives

---

## Additional Resources

- [MDN: Destructuring Assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
- [MDN: Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)
- [MDN: Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)
- [MDN: WeakSet](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet)
- [MDN: WeakMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)
- [MDN: Array.from](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)
- [MDN: Spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)
