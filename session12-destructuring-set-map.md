# Session 12: Destructuring, Set & Map — Student Active-Learning Lab

Welcome to this session. The goal today is to unpack data with **destructuring**, remove duplicates with **Set**, store keyed data with **Map**, and combine arrays with modern helpers. Do not just read — predict, type, run, and fix.

---

## 🧭 How to move through this session

1. **Read the problem first.**
2. **Stop.** Do not look at the code yet.
3. **Write your prediction** in a comment or notebook.
4. **Type the code and run it.**
5. **Compare, ask why, then change one thing.**
6. **Do the challenge before you look at the answer key.**

---

## Part 0: Warm-Up — Many Variables

### The problem

A function returns an array with three values. We want to use them in separate variables.

```javascript
const result = ["John", 30, "New York"];
```

### 🤔 Think

How can we get `name`, `age`, and `city` without writing three separate index accesses?

```text
My idea: _______________________________________________________________
```

### 🔮 Predict

Look at this code and write what you think will print:

```javascript
const [name, age, city] = ["John", 30, "New York"];
console.log(name, age, city);
// ?
```

### ✅ Result

Run the code.

### 🧠 Discover

Destructuring lets us unpack values from arrays and objects into variables in one line.

### 🧪 Experiment

Destructure your own array of three favorite things.

```javascript
// your code here
```

---

## Part 1: Array Destructuring

### 1.1 Basic Array Destructuring

### The problem

Extract values from an array.

### 🔮 Predict

```javascript
const colors = ["red", "green", "blue"];
const [first, second, third] = colors;

console.log(first);   // ?
console.log(second);  // ?
console.log(third);   // ?
```

What will each line print?

### ✅ Result

Run it.

### 🧠 Why?

Array destructuring matches each variable to the value in the same position.

### Challenge 1.1 — Destructure Coordinates

Extract `x` and `y` from `const point = [10, 20]`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const [x, y] = [10, 20];
```

</details>

---

### 1.2 Skipping and Rest

### The problem

Sometimes you only want some values, or you want to gather the rest.

### 🔮 Predict

```javascript
const numbers = [1, 2, 3, 4, 5];

const [one, , three] = numbers;
console.log(one, three);           // ?

const [first, second, ...rest] = numbers;
console.log(first);                // ?
console.log(second);               // ?
console.log(rest);                 // ?
```

What is `rest`?

### ✅ Result

Run it.

### 🧠 Why?

Use empty commas to skip positions. `...rest` collects the remaining values into a new array.

### Challenge 1.2 — Skip and Rest

From `const values = [10, 20, 30, 40, 50]`, get the first, skip the second, and collect the rest.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const [first, , ...rest] = values;
```

</details>

---

### 1.3 Swapping and Defaults

### The problem

Swap two variables in one line, or give a variable a fallback value when the array is short.

### 🔮 Predict

```javascript
let a = 1;
let b = 2;
[a, b] = [b, a];
console.log(a, b);                 // ?

const pair = [1];
const [x, y = 10] = pair;
console.log(x, y);                 // ?
```

What will each `console.log` print?

### ✅ Result

Run it.

### 🧠 Why?

Array destructuring can swap values without a temporary variable. A default value fills a missing position.

### Challenge 1.3 — Swap and Default

Swap `x` and `y`. Then destructure `const nums = [5]` and use a default of `0` for the second value.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
let x = 1, y = 2;
[x, y] = [y, x];

const [x2, y2 = 0] = [5];
```

</details>

---

## Part 2: Object Destructuring

### 2.1 Basic Object Destructuring

### The problem

Extract properties from an object without using dot notation repeatedly.

### 🔮 Predict

```javascript
const user = { name: "John", age: 30, city: "New York" };
const { name, age, city } = user;

console.log(name, age, city);      // ?
```

### ✅ Result

Run it.

### 🧠 Why?

Object destructuring uses property names. `const { name, age } = user` creates variables that match the keys.

### Challenge 2.1 — Destructure User

Extract `email` and `role` from `const account = { email: "john@example.com", role: "admin" }`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const { email, role } = account;
```

</details>

---

### 2.2 Renaming and Nested

### The problem

An object can hold other objects. Sometimes the key name in the object is not the variable name you want.

### 🔮 Predict

```javascript
const person = {
  name: "John",
  profile: {
    age: 30,
    address: { city: "New York" }
  }
};

const { name: fullName, profile: { age, address: { city } } } = person;
console.log(fullName, age, city);  // ?
```

What will it print?

### ✅ Result

Run it.

### 🧠 Why?

`name: fullName` renames the property. Nested destructuring reaches into objects inside objects.

### Challenge 2.2 — Nested Destructuring

From `const data = { product: { name: "Laptop", price: 1000 } }`, extract `name` and `price`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const { product: { name, price } } = data;
```

</details>

---

### 2.3 Defaults and Function Parameters

### The problem

Missing properties can have defaults. Functions can also destructure their parameters.

### 🔮 Predict

```javascript
const config = { host: "localhost" };
const { host, port = 8080, ssl = false } = config;
console.log(host, port, ssl);      // ?

function createUser({ name = "Guest", age = 18 } = {}) {
  return { name, age };
}

console.log(createUser({ name: "Jane" })); // ?
```

### ✅ Result

Run it.

### 🧠 Why?

Defaults in object destructuring work the same as with arrays. `= {}` as a default parameter means the function can be called with no arguments.

### Challenge 2.3 — Defaults in Function

Write `connect({ url = "localhost", port = 3000 } = {})` and call it with and without arguments.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
function connect({ url = "localhost", port = 3000 } = {}) {
  return { url, port };
}

connect();
connect({ url: "example.com" });
```

</details>

---

## Bug Hunt 1

### The mission

Find the bugs in this destructuring code. Do not run it yet. Read and write what you think is wrong.

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

### 🐛 What I think is wrong

1. _______________________________________________________________
2. _______________________________________________________________

### ✅ Fixed version

Write your fixed version, then test it.

```javascript
// your fixed version here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const { name, address: { city } } = user;
console.log(city); // "New York"
```

</details>

---

## Part 3: Set

### 3.1 Creating and Using Sets

### The problem

Remove duplicate values from a list and check membership.

### 🔮 Predict

```javascript
const numbers = [1, 2, 2, 3, 3, 3];
const unique = new Set(numbers);

console.log(unique);               // ?
console.log(unique.size);          // ?
console.log(unique.has(2));        // ?

unique.add(4);
unique.delete(1);
console.log(unique);               // ?
```

What will each `console.log` show?

### ✅ Result

Run it.

### 🧠 Why?

A `Set` stores each value only once. `size`, `has`, `add`, and `delete` are useful methods.

### Challenge 3.1 — Unique Names

Create a `Set` from `const names = ["Alice", "Bob", "Alice", "Carol"]` and log its size.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const unique = new Set(names);
console.log(unique.size);
```

</details>

---

### 3.2 Set Operations

### The problem

Combine sets, find common values, or find values that are in one set but not another.

### 🔮 Predict

```javascript
const a = new Set([1, 2, 3]);
const b = new Set([3, 4, 5]);

const union = new Set([...a, ...b]);
const intersection = new Set([...a].filter(x => b.has(x)));
const difference = new Set([...a].filter(x => !b.has(x)));

console.log(union);                // ?
console.log(intersection);         // ?
console.log(difference);           // ?
```

### ✅ Result

Run it.

### 🧠 Why?

Spread `...a` turns the `Set` into an array. Array methods like `filter` work on arrays, so we convert first.

### Challenge 3.2 — Common Tags

Find the intersection of `const tags1 = ["js", "dom", "api"]` and `const tags2 = ["api", "react", "js"]`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const set1 = new Set(tags1);
const set2 = new Set(tags2);
const common = new Set([...set1].filter(tag => set2.has(tag)));
```

</details>

---

## Part 4: Map

### 4.1 Map Basics

### The problem

Store key-value pairs where keys can be of any type and order matters.

### 🔮 Predict

```javascript
const userMap = new Map();
userMap.set("name", "John");
userMap.set(1, "admin");
userMap.set(true, "verified");

console.log(userMap.get("name"));  // ?
console.log(userMap.has(1));       // ?
console.log(userMap.size);         // ?

userMap.delete(true);
console.log(userMap.has(true));    // ?
```

### ✅ Result

Run it.

### 🧠 Why?

A `Map` keeps insertion order, any key type, and has `get`, `set`, `has`, `delete`, and `size`.

### Challenge 4.1 — Map of Users

Create a `Map` with keys `1`, `2`, `3` and user objects as values. Use `get` to read user `2`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const users = new Map();
users.set(1, { name: "John" });
users.set(2, { name: "Jane" });
users.set(3, { name: "Bob" });
console.log(users.get(2));
```

</details>

---

### 4.2 Map Iteration and Conversion

### The problem

Loop through a `Map` or convert it to and from an object.

### 🔮 Predict

```javascript
const map = new Map([["a", 1], ["b", 2]]);

for (const [key, value] of map) {
  console.log(key, value);         // ?
}

const keys = [...map.keys()];      // ?
const values = [...map.values()];  // ?

const obj = Object.fromEntries(map);
const map2 = new Map(Object.entries(obj));

console.log(obj);                  // ?
console.log(map2);                 // ?
```

### ✅ Result

Run it.

### 🧠 Why?

`for...of` works directly on a `Map`. `Object.fromEntries` and `Object.entries` let you switch between `Map` and plain object.

### Challenge 4.2 — Map to Object

Convert `const map = new Map([["name", "John"], ["age", 30]])` to an object, then back to a Map.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const obj = Object.fromEntries(map);
const map2 = new Map(Object.entries(obj));
```

</details>

---

## Bug Hunt 2

### The mission

Find the bugs in this Map/Set code. Do not run it yet. Read and write what you think is wrong.

```javascript
const users = new Map();
users.set({ id: 1 }, "John");
users.set({ id: 2 }, "Jane");

const john = { id: 1 };
console.log(users.get(john));
```

### 🐛 What I think is wrong

1. _______________________________________________________________
2. _______________________________________________________________

### ✅ Fixed version

Write your fixed version, then test it.

```javascript
// your fixed version here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const users = new Map();
const johnKey = { id: 1 };
users.set(johnKey, "John");
console.log(users.get(johnKey));

// Or use a number/string key
users.set(1, "John");
console.log(users.get(1));
```

</details>

---

## Part 5: Modern Array Helpers

### 5.1 Array.from

### The problem

Turn something that is not an array into an array, or build an array from a rule.

### 🔮 Predict

```javascript
const str = "hello";
console.log(Array.from(str));      // ?

const numbers = [1, 2, 3];
console.log(Array.from(numbers, n => n * 2)); // ?

const range = Array.from({ length: 5 }, (_, i) => i + 1);
console.log(range);                // ?
```

### ✅ Result

Run it.

### 🧠 Why?

`Array.from` builds a real array from an iterable or array-like object. The second argument maps each new item.

### 🧪 Experiment

Use `Array.from` to build `[2, 4, 6, 8, 10]`.

```javascript
// your code here
```

### Challenge 5.1 — Create Range

Use `Array.from` to create `[10, 20, 30, 40, 50]`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const range = Array.from({ length: 5 }, (_, i) => (i + 1) * 10);
```

</details>

---

### 5.2 Array.some and Array.every

### The problem

Check whether any item passes a test, or all items pass a test.

### 🔮 Predict

```javascript
const numbers = [1, 2, 3, 4, 5];

console.log(numbers.some(n => n > 4));   // ?
console.log(numbers.every(n => n > 0));  // ?
console.log(numbers.every(n => n > 2));  // ?
```

### ✅ Result

Run it.

### 🧠 Why?

`some` is `true` if at least one item passes. `every` is `true` only if all items pass.

### Challenge 5.2 — Validate Users

Check if any user in `const users = [{ active: true }, { active: false }]` is active. Check if all are active.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
console.log(users.some(u => u.active));
console.log(users.every(u => u.active));
```

</details>

---

### 5.3 Spread Syntax

### The problem

Copy an array or object, or combine two into one.

### 🔮 Predict

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log(combined);             // ?

const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3 };
const merged = { ...obj1, ...obj2 };
console.log(merged);               // ?

const copy = [...arr1];
console.log(copy);                 // ?
```

### ✅ Result

Run it.

### 🧠 Why?

The spread operator `...` expands an iterable into individual items. It makes copying and merging easy.

### 🧪 Experiment

Merge `const more = [0, ...arr1, 10]` and log the result.

```javascript
// your code here
```

### Challenge 5.3 — Merge and Copy

Merge `const defaults = { theme: "light" }` and `const user = { theme: "dark" }` with spread, then copy `const nums = [1, 2, 3]`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const merged = { ...defaults, ...user };
const copy = [...nums];
```

</details>

---

## Part 6: WeakSet and WeakMap (Brief)

### The problem

Store private references to objects without preventing them from being garbage collected.

### 🔮 Predict

```javascript
const weakSet = new WeakSet();
let obj = { id: 1 };
weakSet.add(obj);
console.log(weakSet.has(obj));     // ?

obj = null;
// The object can be garbage collected

const weakMap = new WeakMap();
const key = { id: 1 };
weakMap.set(key, "value");
console.log(weakMap.get(key));     // ?
```

### ✅ Result

Run it.

### 🧠 Why?

`WeakSet` and `WeakMap` only accept objects as keys. They do not prevent garbage collection, and you cannot iterate over them.

### Challenge 6.1 — When to Use WeakMap

Explain one use case for `WeakMap` (e.g., private data for objects). Then try the example above.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
// Use `WeakMap` to attach private data to objects without preventing garbage collection.
const privateData = new WeakMap();
const user = { id: 1 };
privateData.set(user, "secret");
console.log(privateData.get(user));
```

</details>

---

## Group Challenge: Data Transformations

Work in a team of 2 or 3. Each team completes two of the tasks below.

### Tasks

1. Destructure `const data = { user: { name: "John", age: 30 }, posts: [{ title: "A" }, { title: "B" }] }` to get `name`, `age`, and the first `title`.
2. Create a `Set` of unique words from a sentence.
3. Build a `Map` that counts the frequency of each character in a string.
4. Use `some` and `every` to check if any or all numbers in `[2, 4, 6, 8]` are even.

---

## Individual Challenges — Progressive Difficulty

Do these in order. Do not look at the answer key until you have tried.

### Level 1: Basic Array Destructuring

From `const rgb = [255, 128, 0]`, get `red`, `green`, and `blue`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const [red, green, blue] = [255, 128, 0];
```

</details>

### Level 2: Object Destructuring

From `const product = { name: "Laptop", price: 999 }`, get `name` and `price`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const { name, price } = product;
```

</details>

### Level 3: Rest and Defaults

From `const nums = [10, 20]`, get `first`, `second`, and `third = 0`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const [first, second, third = 0] = [10, 20];
```

</details>

### Level 4: Nested Object

Extract `city` from `const user = { address: { city: "Paris" } }`.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const { address: { city } } = user;
```

</details>

### Level 5: Set

Create a `Set` from `[1, 2, 2, 3]` and log its size.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const unique = new Set([1, 2, 2, 3]);
console.log(unique.size);
```

</details>

### Level 6: Map

Create a `Map` with two entries, iterate, and log each key-value pair.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const map = new Map([["a", 1], ["b", 2]]);
for (const [key, value] of map) {
  console.log(key, value);
}
```

</details>

### Level 7: Spread

Copy `const arr = [1, 2, 3]` with spread, then add `4` to the end.

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
const copy = [...arr, 4];
```

</details>

### Level 8: some/every

Use `some` and `every` on `const scores = [85, 90, 78]` to check for passing (> 70) and excellent (> 80).

```javascript
// your code here
```

<details>
<summary>Answer — try first!</summary>

```javascript
console.log(scores.some(s => s > 70));   // true
console.log(scores.every(s => s > 80));  // false
```

</details>

---

## Mini Project: Shopping Cart with Map and Set

### Goal

Build a small product catalog and cart using `Map`, `Set`, and destructuring.

### Requirements

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

### Questions to think about

1. Why did we use a `Map` for the cart?
2. Why did we use a `Set` for discount codes?
3. How does destructuring help in the `for...of` loop?

---

## Review Questions

Answer these before you finish. Do not look at the answer key.

1. What does array destructuring do?
   - [ ] Creates a new array
   - [ ] Unpacks array values into variables
   - [ ] Deletes array elements
   - [ ] Sorts array elements

2. What is the rest operator in destructuring?
   - [ ] Removes elements
   - [ ] Collects remaining elements
   - [ ] Duplicates elements
   - [ ] Reverses elements

3. What is the main difference between Set and WeakSet?
   - [ ] No difference
   - [ ] Set can store any value, WeakSet only objects
   - [ ] WeakSet is iterable, Set is not
   - [ ] Set prevents garbage collection, WeakSet allows it

4. What is the main difference between Map and Object?
   - [ ] No difference
   - [ ] Map can have any key type, Object keys are strings
   - [ ] Object is iterable, Map is not
   - [ ] Map maintains insertion order, Object may not

5. What does Array.from() do?
   - [ ] Creates an array from an array-like object
   - [ ] Converts array to string
   - [ ] Sorts array elements
   - [ ] Deletes array elements

6. What does Array.some() return?
   - [ ] true if all elements pass condition
   - [ ] true if any element passes condition
   - [ ] The first element
   - [ ] All elements

7. What does Array.every() return?
   - [ ] true if all elements pass condition
   - [ ] true if any element passes condition
   - [ ] The first element
   - [ ] All elements

8. What does the spread operator do?
   - [ ] Removes elements
   - [ ] Expands an iterable into individual elements
   - [ ] Sorts elements
   - [ ] Filters elements

9. What does copyWithin() do?
   - [ ] Copies array from another array
   - [ ] Copies array elements within the same array
   - [ ] Removes array elements
   - [ ] Reverses array elements

10. Can you destructure objects in function parameters?
    - [ ] No
    - [ ] Yes
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

---

<details>
<summary>Answer Key — Try everything first!</summary>

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

### Bug Hunt 1 Fixed Version

```javascript
const { name, address: { city } } = user;
console.log(city); // "New York"
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

### Bug Hunt 2 Fixed Version

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

### Group Challenge Answer Key

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
