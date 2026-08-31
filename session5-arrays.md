# Session 5: Arrays — Active Learning Redesign

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

- "What do you expect the array to look like after this line?"
- "Which index is the first element?"
- "Does this method change the original array?"
- "Which method gives you one value back, and which gives you a new array?"
- "What would you use if you wanted all matching items?"

---

## Part 0: Warm-Up — The Shopping List Is Broken (5 minutes)

### Problem

A shopping list app stores items in separate variables and cannot count them.

```javascript
let item1 = "Milk";
let item2 = "Bread";
let item3 = "Eggs";

console.log(item1, item2, item3);
console.log("Total items: ???");
```

### Guess

Ask: "How would you store 100 items? What if you want to add more?"

### Explain

An array is a single variable that can hold many values in order. Each value has an index starting at `0`.

### Live Code

```javascript
let shoppingList = ["Milk", "Bread", "Eggs"];

console.log(shoppingList[0]);          // "Milk"
console.log(shoppingList.length);      // 3
console.log(shoppingList[shoppingList.length - 1]); // "Eggs"

shoppingList.push("Butter");
console.log(shoppingList);
```

### Review

Arrays solve the problem of storing many values. The first item is at index `0`. `length` gives the count.

---

## Part 1: Creating and Accessing Arrays

### 1.1 Array Literal and Constructor

#### Problem

Create a list of colors and a list of scores.

#### Explain

The easiest way to create an array is with square brackets `[]`. You can also use `new Array()` or `Array.from()`.

#### Live Code

```javascript
// Array literal
let fruits = ["apple", "banana", "orange"];
let numbers = [1, 2, 3, 4, 5];
let mixed = [1, "hello", true, null, { name: "John" }];
let empty = [];

console.log(fruits, numbers, mixed, empty);

// Array constructor
let moreFruits = new Array("grape", "mango");
let fiveSlots = new Array(5); // 5 empty slots
console.log(moreFruits);
console.log(fiveSlots);

// Array.from
let letters = Array.from("hello");
console.log(letters); // ["h", "e", "l", "l", "o"]

let range = Array.from({ length: 5 }, (_, i) => i + 1);
console.log(range); // [1, 2, 3, 4, 5]
```

#### Challenge 1.1 — Create Your Own (individual, 3 minutes)

- **Requirement:** Create three arrays: one with your top 3 movies, one with 3 numbers, and one mixed. Log each first and last item.
- **Time limit:** 3 minutes

#### Review

Check that students use `array[0]` and `array[array.length - 1]`.

### 1.2 Length and Index Access

#### Problem

A student tries to get the last item like this:

```javascript
let colors = ["red", "green", "blue"];
console.log(colors[colors.length]); // undefined
```

#### Guess

Ask: "Why is the last item `undefined`?"

#### Explain

- `length` is the number of items. The last index is `length - 1`.
- `length` can also be set to shrink or clear an array.

#### Live Code

```javascript
let colors = ["red", "green", "blue", "yellow"];

console.log(colors.length);              // 4
console.log(colors[0]);                  // "red"
console.log(colors[colors.length - 1]);  // "yellow"
console.log(colors[10]);                 // undefined

// Shrink
let fruits = ["apple", "banana", "orange"];
fruits.length = 2;
console.log(fruits); // ["apple", "banana"]

// Clear
fruits.length = 0;
console.log(fruits); // []
```

#### Challenge 1.2 — Safe Last Item (individual, 3 minutes)

- **Requirement:** Write a line that always gets the last item of any array `arr`.
- **Time limit:** 3 minutes
- **Hint:** `arr[arr.length - 1]`.

---

## Part 2: Adding and Removing Elements

### 2.1 Adding to the End and Beginning

#### Problem

A queue of customers. New customers can arrive at the front (VIP) or the back.

#### Live Code

```javascript
let queue = ["John", "Jane"];

// End
queue.push("Bob");
console.log(queue); // ["John", "Jane", "Bob"]

queue.push("Alice", "Tom");
console.log(queue); // ["John", "Jane", "Bob", "Alice", "Tom"]

// Beginning
queue.unshift("VIP");
console.log(queue); // ["VIP", "John", ...]

queue.unshift("VIP2", "VIP3");
console.log(queue);
```

#### Challenge 2.1 — Add and Remove (individual, 4 minutes)

- **Requirement:** Start with `let tasks = ["Email"];`. Add `"Code"` to the end and `"Plan"` to the beginning. Log the result.
- **Time limit:** 4 minutes

### 2.2 Removing from the End and Beginning

#### Live Code

```javascript
let fruits = ["apple", "banana", "orange", "grape"];

let last = fruits.pop();
console.log(last);       // "grape"
console.log(fruits);     // ["apple", "banana", "orange"]

let first = fruits.shift();
console.log(first);      // "apple"
console.log(fruits);     // ["banana", "orange"]
```

#### Challenge 2.2 — Stack (individual, 4 minutes)

- **Requirement:** Simulate a stack. Start with `let stack = ["A", "B", "C"];`. Pop the last item, then push `"D"`, then pop again. Log the stack after each step.
- **Time limit:** 4 minutes

### 2.3 splice — Add or Remove at Any Position

#### Problem

A playlist needs a song inserted in the middle, or removed from the middle.

#### Explain

`splice(start, deleteCount, ...items)` modifies the original array.

#### Live Code

```javascript
let songs = ["Song A", "Song B", "Song D"];

songs.splice(2, 0, "Song C");
console.log(songs); // ["Song A", "Song B", "Song C", "Song D"]

songs.splice(1, 1);
console.log(songs); // ["Song A", "Song C", "Song D"]

songs.splice(1, 1, "New Song", "Another Song");
console.log(songs);
```

#### Challenge 2.3 — Edit in Place (individual, 5 minutes)

- **Requirement:** Start with `let items = ["pen", "pencil", "eraser", "ruler"]`. Remove `"pencil"` and insert `"marker"` in its place.
- **Time limit:** 5 minutes
- **Hint:** Find the index first, then `splice(index, 1, "marker")`.

---

## Part 3: Searching in Arrays

### 3.1 indexOf, lastIndexOf, includes

#### Problem

A student wants to know if `"banana"` is in the list and where it is.

#### Live Code

```javascript
let fruits = ["apple", "banana", "orange", "grape", "banana"];

console.log(fruits.indexOf("banana"));      // 1
console.log(fruits.lastIndexOf("banana"));  // 4
console.log(fruits.indexOf("pear"));        // -1
console.log(fruits.includes("orange"));     // true
console.log(fruits.includes("Banana"));     // false (case-sensitive)
```

#### Challenge 3.1 — Find the Position (individual, 4 minutes)

- **Requirement:** Given `let names = ["Ada", "Grace", "Alan", "Grace"]`, find and log the first and last index of `"Grace"`.
- **Time limit:** 4 minutes

### 3.2 find and findIndex

#### Problem

Find the first adult in a list of people.

#### Live Code

```javascript
let users = [
  { id: 1, name: "John", age: 30 },
  { id: 2, name: "Jane", age: 25 },
  { id: 3, name: "Bob", age: 35 }
];

let firstAdult = users.find(u => u.age >= 30);
console.log(firstAdult); // John

let janeIndex = users.findIndex(u => u.name === "Jane");
console.log(janeIndex);  // 1

let notFound = users.find(u => u.age > 50);
console.log(notFound);   // undefined
```

#### Challenge 3.2 — Find One (individual, 4 minutes)

- **Requirement:** Given an array of products, find the first product with a price greater than `100`.
- **Time limit:** 4 minutes
- **Hint:** Use `products.find(p => p.price > 100)`.

---

## Bug Hunt 1

### Problem

The following code has three deliberate bugs. Ask students to find them.

```javascript
let cart = ["Milk", "Bread", "Eggs"];

cart.push("Cheese");
cart.pop();
console.log(cart[3]);

let index = cart.indexOf("bread");
if (index > 0) {
  cart.splice(index, 1);
}

console.log(cart);
```

### Issues

1. `cart[3]` is `undefined` because the array only has 3 items after pop.
2. `indexOf("bread")` is case-sensitive, so it returns `-1`.
3. The `if` condition uses `> 0` instead of `>= 0` or `> -1`, so it will not remove `"Bread"` even if found.

### Fixed Version (for the instructor)

```javascript
let cart = ["Milk", "Bread", "Eggs"];

cart.push("Cheese");
cart.pop();
console.log(cart[cart.length - 1]);

let index = cart.indexOf("Bread");
if (index !== -1) {
  cart.splice(index, 1);
}

console.log(cart);
```

### Points

1 point for each found bug.

---

## Part 4: Sorting Arrays

### 4.1 String Sorting and Reverse

#### Problem

A list of words should be in alphabetical order.

#### Live Code

```javascript
let fruits = ["banana", "apple", "orange", "grape"];

fruits.sort();
console.log(fruits); // ["apple", "banana", "grape", "orange"]

fruits.reverse();
console.log(fruits); // ["orange", "grape", "banana", "apple"]
```

#### Challenge 4.1 — Sort Names (individual, 3 minutes)

- **Requirement:** Sort `let names = ["Zara", "Alice", "Mike", "Bob"]` alphabetically and log the result.
- **Time limit:** 3 minutes

### 4.2 Numeric Sorting

#### Problem

The same `sort()` gives wrong results for numbers.

#### Guess

Ask: "What will `[10, 5, 100, 1, 50].sort()` print?"

#### Explain

`sort()` converts numbers to strings and compares them as strings. Use a compare function for numbers.

#### Live Code

```javascript
let numbers = [10, 5, 100, 1, 50];

numbers.sort();
console.log(numbers); // [1, 10, 100, 5, 50] — wrong!

numbers.sort((a, b) => a - b);
console.log(numbers); // [1, 5, 10, 50, 100]

numbers.sort((a, b) => b - a);
console.log(numbers); // [100, 50, 10, 5, 1]
```

#### Challenge 4.2 — Sort Prices (individual, 4 minutes)

- **Requirement:** Sort `let prices = [9.99, 4.50, 12.00, 1.99]` from lowest to highest.
- **Time limit:** 4 minutes
- **Hint:** `prices.sort((a, b) => a - b)`.

### 4.3 Sorting Objects

#### Live Code

```javascript
let users = [
  { name: "John", age: 30 },
  { name: "Jane", age: 25 },
  { name: "Bob", age: 35 }
];

users.sort((a, b) => a.age - b.age);
console.log(users); // Jane, John, Bob

users.sort((a, b) => a.name.localeCompare(b.name));
console.log(users); // Bob, Jane, John
```

#### Challenge 4.3 — Sort Products (individual, 5 minutes)

- **Requirement:** Sort `let products = [{name: "Laptop", price: 999}, {name: "Mouse", price: 25}, {name: "Book", price: 15}]` by price ascending.
- **Time limit:** 5 minutes

---

## Part 5: Slicing, Joining, and Combining

### 5.1 slice

#### Problem

Extract a subset of an array without changing the original.

#### Live Code

```javascript
let fruits = ["apple", "banana", "orange", "grape", "mango"];

console.log(fruits.slice(1, 3));  // ["banana", "orange"]
console.log(fruits.slice(2));     // ["orange", "grape", "mango"]
console.log(fruits.slice(-2));    // ["grape", "mango"]
console.log(fruits.slice(1, -1)); // ["banana", "orange", "grape"]

// Clone
let clone = fruits.slice();
clone.push("kiwi");
console.log(fruits);  // unchanged
console.log(clone);   // has kiwi
```

#### Challenge 5.1 — First Three (individual, 3 minutes)

- **Requirement:** Use `slice` to get the first three items of any array.
- **Time limit:** 3 minutes
- **Hint:** `arr.slice(0, 3)`.

### 5.2 join and concat

#### Live Code

```javascript
let fruits = ["apple", "banana", "orange"];

console.log(fruits.join());          // "apple,banana,orange"
console.log(fruits.join(", "));    // "apple, banana, orange"
console.log(fruits.join(" - "));    // "apple - banana - orange"

let group1 = ["apple", "banana"];
let group2 = ["orange", "grape"];
let all = group1.concat(group2);
console.log(all);

// Modern spread
let combined = [...group1, ...group2];
console.log(combined);
```

#### Challenge 5.2 — Sentence Builder (individual, 4 minutes)

- **Requirement:** Convert `let words = ["I", "love", "JavaScript"]` into the sentence `"I love JavaScript."` using `join`.
- **Time limit:** 4 minutes
- **Hint:** `words.join(" ") + "."`.

---

## Part 6: Array Iteration Methods

### 6.1 forEach

#### Problem

Print every item in a list with its position.

#### Live Code

```javascript
let fruits = ["apple", "banana", "orange"];

fruits.forEach((fruit, index) => {
  console.log(`${index}: ${fruit}`);
});
```

#### Challenge 6.1 — Log Prices (individual, 3 minutes)

- **Requirement:** Use `forEach` to print each price in `let prices = [10, 20, 30]` with the format `"Price: $10"`.
- **Time limit:** 3 minutes

### 6.2 map

#### Problem

Apply the same transformation to every item and make a new array.

#### Live Code

```javascript
let numbers = [1, 2, 3, 4, 5];

let doubled = numbers.map(num => num * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

let users = [
  { name: "John", age: 30 },
  { name: "Jane", age: 25 }
];
let names = users.map(user => user.name);
console.log(names); // ["John", "Jane"]
```

#### Challenge 6.2 — Double and Uppercase (individual, 4 minutes)

- **Requirement:** Map `[1, 2, 3]` to `[2, 4, 6]` and `["a", "b", "c"]` to `["A", "B", "C"]`.
- **Time limit:** 4 minutes

### 6.3 filter

#### Live Code

```javascript
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

let even = numbers.filter(n => n % 2 === 0);
console.log(even); // [2, 4, 6, 8, 10]

let users = [
  { name: "John", age: 30 },
  { name: "Jane", age: 25 },
  { name: "Bob", age: 35 }
];
let adults = users.filter(u => u.age >= 30);
console.log(adults); // John and Bob
```

#### Challenge 6.3 — Filter by Category (individual, 5 minutes)

- **Requirement:** Filter `let products = [{name: "Book", category: "Books"}, {name: "Phone", category: "Electronics"}, {name: "Pen", category: "Stationery"}]` to only `"Electronics"`.
- **Time limit:** 5 minutes

### 6.4 reduce

#### Problem

Calculate the total price of a shopping cart.

#### Live Code

```javascript
let numbers = [1, 2, 3, 4, 5];
let sum = numbers.reduce((acc, n) => acc + n, 0);
console.log(sum); // 15

let cart = [
  { item: "Book", price: 10 },
  { item: "Pen", price: 2 },
  { item: "Notebook", price: 5 }
];
let total = cart.reduce((acc, item) => acc + item.price, 0);
console.log(total); // 17
```

#### Challenge 6.4 — Total Price (individual, 5 minutes)

- **Requirement:** Use `reduce` to get the total of `let prices = [10, 20, 30, 40]`.
- **Time limit:** 5 minutes
- **Hint:** Start the accumulator at `0`.

---

## Part 7: Other Useful Array Methods

### 7.1 every and some

#### Problem

Check if all students passed, or if any student scored above 90.

#### Live Code

```javascript
let scores = [80, 85, 90, 95];

let allPassed = scores.every(s => s >= 60);
console.log(allPassed); // true

let hasTopScore = scores.some(s => s >= 90);
console.log(hasTopScore); // true
```

#### Challenge 7.1 — Check Conditions (individual, 4 minutes)

- **Requirement:** Use `every` to check if all numbers in `[2, 4, 6, 8]` are even, and `some` to check if any are greater than `5`.
- **Time limit:** 4 minutes

### 7.2 flat and flatMap

#### Live Code

```javascript
let nested = [1, [2, [3, [4, 5]]]];

console.log(nested.flat());           // [1, 2, [3, [4, 5]]]
console.log(nested.flat(2));          // [1, 2, 3, [4, 5]]
console.log(nested.flat(Infinity));   // [1, 2, 3, 4, 5]

let numbers = [1, 2, 3];
let doubled = numbers.flatMap(n => [n, n * 2]);
console.log(doubled); // [1, 2, 2, 4, 3, 6]
```

#### Challenge 7.2 — Flatten (individual, 4 minutes)

- **Requirement:** Flatten `let groups = [["A", "B"], ["C"], ["D", "E"]]` to a single array.
- **Time limit:** 4 minutes
- **Hint:** Use `flat()` or `flat(Infinity)`.

---

## Part 8: Practical Array Projects

### 8.1 Product Manager

#### Live Code

```javascript
class ProductManager {
  constructor() {
    this.products = [];
  }

  addProduct(name, price, category) {
    const product = {
      id: Date.now(),
      name,
      price,
      category
    };
    this.products.push(product);
    console.log(`Added: ${name} ($${price})`);
    return product;
  }

  removeProduct(id) {
    const index = this.products.findIndex(p => p.id === id);
    if (index > -1) {
      const removed = this.products.splice(index, 1)[0];
      console.log(`Removed: ${removed.name}`);
      return removed;
    }
    console.log("Product not found");
    return null;
  }

  findProduct(name) {
    return this.products.find(p =>
      p.name.toLowerCase() === name.toLowerCase()
    ) || null;
  }

  getByCategory(category) {
    return this.products.filter(p =>
      p.category.toLowerCase() === category.toLowerCase()
    );
  }

  sortByPrice(ascending = true) {
    return [...this.products].sort((a, b) =>
      ascending ? a.price - b.price : b.price - a.price
    );
  }

  getTotalValue() {
    return this.products.reduce((total, p) => total + p.price, 0);
  }

  display() {
    console.log("\n--- Products ---");
    this.products.forEach((p, i) => {
      console.log(`${i + 1}. ${p.name} - $${p.price} (${p.category})`);
    });
  }
}

const manager = new ProductManager();
manager.addProduct("Laptop", 999.99, "Electronics");
manager.addProduct("Book", 19.99, "Books");
manager.addProduct("Headphones", 149.99, "Electronics");
manager.display();
```

### 8.2 Task Manager

#### Live Code

```javascript
class TaskManager {
  constructor() {
    this.tasks = [];
  }

  addTask(title, priority = "medium") {
    const task = {
      id: Date.now(),
      title,
      priority,
      completed: false
    };
    this.tasks.push(task);
    return task;
  }

  completeTask(id) {
    const task = this.tasks.find(t => t.id === id);
    if (task) {
      task.completed = true;
      return task;
    }
    return null;
  }

  deleteTask(id) {
    const index = this.tasks.findIndex(t => t.id === id);
    if (index > -1) {
      return this.tasks.splice(index, 1)[0];
    }
    return null;
  }

  getPending() {
    return this.tasks.filter(t => !t.completed);
  }

  getCompleted() {
    return this.tasks.filter(t => t.completed);
  }

  sortByPriority() {
    const order = { high: 0, medium: 1, low: 2 };
    return [...this.tasks].sort((a, b) =>
      order[a.priority] - order[b.priority]
    );
  }

  display() {
    console.log("\n--- Tasks ---");
    this.tasks.forEach((t, i) => {
      const status = t.completed ? "done" : "pending";
      console.log(`${i + 1}. [${status}] ${t.title} (${t.priority})`);
    });
  }
}
```

---

## Bug Hunt 2

### Problem

The task manager has bugs. Ask students to find them.

```javascript
let tasks = [
  { id: 1, title: "Read", completed: false },
  { id: 2, title: "Write", completed: false }
];

function completeTask(id) {
  let task = tasks.find(t => t.id === id);
  task.completed = true;
}

function deleteTask(id) {
  let task = tasks.find(t => t.id === id);
  tasks = tasks.filter(t => t !== task);
}

completeTask(2);
deleteTask(2);
console.log(tasks.length);

tasks.sort((a, b) => a.completed > b.completed);
console.log(tasks);
```

### Issues

1. `completeTask(2)` changes `completed` but then `deleteTask(2)` removes the same task. The order matters, but the logic is not obviously wrong.
2. `tasks.sort((a, b) => a.completed > b.completed)` uses `>` which returns a boolean, not a number. This can cause inconsistent sort results.
3. `completeTask` does not check if `task` exists. If `id` is not found, `task` is `undefined` and `task.completed` throws an error.

### Fixed Version (for the instructor)

```javascript
let tasks = [
  { id: 1, title: "Read", completed: false },
  { id: 2, title: "Write", completed: false }
];

function completeTask(id) {
  let task = tasks.find(t => t.id === id);
  if (task) {
    task.completed = true;
  }
}

function deleteTask(id) {
  tasks = tasks.filter(t => t.id !== id);
}

completeTask(2);
console.log(tasks.find(t => t.id === 2));

tasks.sort((a, b) => a.completed - b.completed);
console.log(tasks);
```

### Points

1 point for each found issue.

---

## Group Challenge: Array Olympics

- **Time:** 10 minutes
- **Teams:** 2 or 3 students per team
- **Task:** Each team receives the same data. They must solve as many of the following as possible.
- **Scoring:** 2 points per correct solution. The team with the most points wins.

### Data

```javascript
let students = [
  { name: "Alice", score: 85 },
  { name: "Bob", score: 55 },
  { name: "Carol", score: 92 },
  { name: "Dave", score: 70 },
  { name: "Eve", score: 45 }
];
```

### Tasks

1. Find the first student who passed (`score >= 60`).
2. Make a new array of names only.
3. Filter students who passed.
4. Calculate the average score.
5. Sort the students by score, highest first.

### Instructor Answer Key

```javascript
// 1
students.find(s => s.score >= 60);

// 2
students.map(s => s.name);

// 3
students.filter(s => s.score >= 60);

// 4
let average = students.reduce((sum, s) => sum + s.score, 0) / students.length;

// 5
students.sort((a, b) => b.score - a.score);
```

---

## Individual Challenges — Progressive Difficulty

### Level 1: Push and Pop (3 minutes)

- **Requirement:** Start with `let stack = []`. Push `"A"`, `"B"`, `"C"`, then pop the last item and log the stack.
- **Expected:** `["A", "B"]`

### Level 2: Find and Remove (4 minutes)

- **Requirement:** From `let colors = ["red", "blue", "green", "blue"]`, remove the first `"blue"` and log the result.
- **Hint:** Use `indexOf` and `splice`.

### Level 3: Map and Filter (5 minutes)

- **Requirement:** From `let numbers = [1, 2, 3, 4, 5, 6]`, create a new array of the doubled values of only the odd numbers.
- **Hint:** Filter odd, then map to double.

### Level 4: Total by Category (5 minutes)

- **Requirement:** Calculate the total price of all `"Electronics"` items in the following array.

```javascript
let cart = [
  { name: "Phone", price: 500, category: "Electronics" },
  { name: "Book", price: 20, category: "Books" },
  { name: "Cable", price: 10, category: "Electronics" }
];
```

- **Hint:** Filter by category, then reduce.

### Level 5: Sort and Slice (5 minutes)

- **Requirement:** Sort `let scores = [88, 92, 55, 70, 100]` from highest to lowest, then get the top 3.
- **Hint:** `scores.sort((a, b) => b - a).slice(0, 3)`.

---

## Mini Project: Interactive Task and Product Dashboard

### Time

25 minutes

### Goal

Combine array creation, search, sort, filter, map, and reduce in one HTML page with two mini-apps.

### Requirements for the Students

1. Create an HTML page with two sections:
   - **Products:** inputs for name, price, category; buttons to add, remove, sort, and filter by category.
   - **Tasks:** input for task title and priority; buttons to add, complete, delete, filter, and sort.

2. Use one array for `products` and one for `tasks`.

3. Use these methods:
   - `push` to add
   - `find` or `findIndex` to locate
   - `splice` or `filter` to remove
   - `sort` to order
   - `filter` to show subsets
   - `map` to render HTML
   - `reduce` to calculate totals

4. Display the list, total value, and counts.

### Time Limit

25 minutes

### Hints (optional)

- Use `innerHTML = array.map(item => `<div>...</div>`).join("")` to render.
- For `Date.now()`, use it as a simple unique ID.
- Use `parseFloat` for prices.

### Starter HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Array Dashboard</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 800px; margin: 20px auto; }
    .section { background: #f9f9f9; padding: 15px; margin-bottom: 20px; border-radius: 4px; }
    input, select { padding: 5px; margin: 5px; }
    button { padding: 8px 15px; margin: 5px; }
    .list { background: white; padding: 10px; border: 1px solid #ddd; margin-top: 10px; }
    .item { padding: 8px; border-bottom: 1px solid #eee; display: flex; justify-content: space-between; }
    .item:last-child { border-bottom: none; }
  </style>
</head>
<body>
  <h1>Array Dashboard</h1>

  <div class="section">
    <h2>Products</h2>
    <input type="text" id="pName" placeholder="Name">
    <input type="number" id="pPrice" placeholder="Price" step="0.01">
    <select id="pCategory">
      <option>Electronics</option>
      <option>Books</option>
      <option>Food</option>
    </select>
    <button onclick="addProduct()">Add</button>
    <div id="productList" class="list"></div>
    <button onclick="sortProducts('asc')">Sort Price Low-High</button>
    <button onclick="sortProducts('desc')">Sort Price High-Low</button>
    <button onclick="filterProducts('Electronics')">Filter Electronics</button>
    <button onclick="showAllProducts()">Show All</button>
    <p><strong>Total:</strong> $<span id="productTotal">0.00</span> | <strong>Count:</strong> <span id="productCount">0</span></p>
  </div>

  <div class="section">
    <h2>Tasks</h2>
    <input type="text" id="tTitle" placeholder="Task">
    <select id="tPriority">
      <option value="high">High</option>
      <option value="medium">Medium</option>
      <option value="low">Low</option>
    </select>
    <button onclick="addTask()">Add</button>
    <div id="taskList" class="list"></div>
    <button onclick="showTasks('all')">All</button>
    <button onclick="showTasks('pending')">Pending</button>
    <button onclick="showTasks('completed')">Completed</button>
    <p><strong>Pending:</strong> <span id="pendingCount">0</span> | <strong>Completed:</strong> <span id="completedCount">0</span></p>
  </div>

  <script>
    let products = [];
    let tasks = [];

    function addProduct() {
      let name = document.getElementById("pName").value.trim();
      let price = parseFloat(document.getElementById("pPrice").value);
      let category = document.getElementById("pCategory").value;
      if (!name || isNaN(price)) return;
      products.push({ id: Date.now(), name, price, category });
      renderProducts(products);
    }

    function renderProducts(list) {
      let html = list.map(p => `
        <div class="item">
          <span>${p.name} - $${p.price.toFixed(2)} (${p.category})</span>
          <button onclick="removeProduct(${p.id})">Remove</button>
        </div>
      `).join("");
      document.getElementById("productList").innerHTML = html || "<p>No products</p>";
      let total = products.reduce((sum, p) => sum + p.price, 0);
      document.getElementById("productTotal").textContent = total.toFixed(2);
      document.getElementById("productCount").textContent = products.length;
    }

    function removeProduct(id) {
      products = products.filter(p => p.id !== id);
      renderProducts(products);
    }

    function sortProducts(order) {
      let sorted = [...products].sort((a, b) =>
        order === "asc" ? a.price - b.price : b.price - a.price
      );
      renderProducts(sorted);
    }

    function filterProducts(category) {
      let filtered = products.filter(p => p.category === category);
      renderProducts(filtered);
    }

    function showAllProducts() {
      renderProducts(products);
    }

    function addTask() {
      let title = document.getElementById("tTitle").value.trim();
      let priority = document.getElementById("tPriority").value;
      if (!title) return;
      tasks.push({ id: Date.now(), title, priority, completed: false });
      renderTasks(tasks);
    }

    function renderTasks(list) {
      let html = list.map(t => `
        <div class="item">
          <span>${t.completed ? "[x]" : "[ ]"} ${t.title} (${t.priority})</span>
          <span>
            <button onclick="toggleTask(${t.id})">${t.completed ? "Undo" : "Done"}</button>
            <button onclick="deleteTask(${t.id})">Delete</button>
          </span>
        </div>
      `).join("");
      document.getElementById("taskList").innerHTML = html || "<p>No tasks</p>";
      document.getElementById("pendingCount").textContent = tasks.filter(t => !t.completed).length;
      document.getElementById("completedCount").textContent = tasks.filter(t => t.completed).length;
    }

    function toggleTask(id) {
      let task = tasks.find(t => t.id === id);
      if (task) {
        task.completed = !task.completed;
        renderTasks(tasks);
      }
    }

    function deleteTask(id) {
      tasks = tasks.filter(t => t.id !== id);
      renderTasks(tasks);
    }

    function showTasks(type) {
      let list;
      if (type === "pending") list = tasks.filter(t => !t.completed);
      else if (type === "completed") list = tasks.filter(t => t.completed);
      else list = tasks;
      renderTasks(list);
    }
  </script>
</body>
</html>
```

### Review Questions for the Mini Project

- "Why did we use `[...products].sort()` instead of `products.sort()`?"
- "What happens if `renderProducts` receives an empty array?"
- "Which method would you use to find one task by its title?"

---

<details>
<summary>Trainer Solutions — Do Not Show Until Students Try</summary>

## Trainer Solutions — Do Not Show Until Students Try

### Challenge 1.1

```javascript
let movies = ["Inception", "Matrix", "Interstellar"];
let numbers = [7, 14, 21];
let mixed = ["hello", 42, true];
console.log(movies[0], movies[movies.length - 1]);
```

### Challenge 1.2

```javascript
arr[arr.length - 1];
```

### Challenge 2.1

```javascript
let tasks = ["Email"];
tasks.push("Code");
tasks.unshift("Plan");
console.log(tasks); // ["Plan", "Email", "Code"]
```

### Challenge 2.2

```javascript
let stack = ["A", "B", "C"];
stack.pop(); // ["A", "B"]
stack.push("D"); // ["A", "B", "D"]
stack.pop(); // ["A", "B"]
console.log(stack);
```

### Challenge 2.3

```javascript
let items = ["pen", "pencil", "eraser", "ruler"];
let index = items.indexOf("pencil");
if (index !== -1) {
  items.splice(index, 1, "marker");
}
console.log(items);
```

### Challenge 3.1

```javascript
let names = ["Ada", "Grace", "Alan", "Grace"];
console.log(names.indexOf("Grace"));     // 1
console.log(names.lastIndexOf("Grace")); // 3
```

### Challenge 3.2

```javascript
let products = [
  { name: "Book", price: 15 },
  { name: "Laptop", price: 999 },
  { name: "Mouse", price: 25 }
];
let expensive = products.find(p => p.price > 100);
console.log(expensive);
```

### Challenge 4.1

```javascript
let names = ["Zara", "Alice", "Mike", "Bob"];
names.sort();
console.log(names);
```

### Challenge 4.2

```javascript
let prices = [9.99, 4.50, 12.00, 1.99];
prices.sort((a, b) => a - b);
console.log(prices);
```

### Challenge 4.3

```javascript
let products = [
  { name: "Laptop", price: 999 },
  { name: "Mouse", price: 25 },
  { name: "Book", price: 15 }
];
products.sort((a, b) => a.price - b.price);
console.log(products);
```

### Challenge 5.1

```javascript
arr.slice(0, 3);
```

### Challenge 5.2

```javascript
let words = ["I", "love", "JavaScript"];
let sentence = words.join(" ") + ".";
console.log(sentence);
```

### Challenge 6.1

```javascript
let prices = [10, 20, 30];
prices.forEach(price => console.log(`Price: $${price}`));
```

### Challenge 6.2

```javascript
let nums = [1, 2, 3];
let doubled = nums.map(n => n * 2);

let letters = ["a", "b", "c"];
let upper = letters.map(l => l.toUpperCase());
```

### Challenge 6.3

```javascript
let products = [
  { name: "Book", category: "Books" },
  { name: "Phone", category: "Electronics" },
  { name: "Pen", category: "Stationery" }
];
let electronics = products.filter(p => p.category === "Electronics");
console.log(electronics);
```

### Challenge 6.4

```javascript
let prices = [10, 20, 30, 40];
let total = prices.reduce((sum, p) => sum + p, 0);
console.log(total);
```

### Challenge 7.1

```javascript
let nums = [2, 4, 6, 8];
console.log(nums.every(n => n % 2 === 0)); // true
console.log(nums.some(n => n > 5));        // true
```

### Challenge 7.2

```javascript
let groups = [["A", "B"], ["C"], ["D", "E"]];
console.log(groups.flat());
```

### Individual Challenges Solutions

```javascript
// Level 1
let stack = [];
stack.push("A");
stack.push("B");
stack.push("C");
stack.pop();
console.log(stack); // ["A", "B"]

// Level 2
let colors = ["red", "blue", "green", "blue"];
let idx = colors.indexOf("blue");
if (idx !== -1) colors.splice(idx, 1);
console.log(colors);

// Level 3
let numbers = [1, 2, 3, 4, 5, 6];
let result = numbers.filter(n => n % 2 !== 0).map(n => n * 2);
console.log(result); // [2, 6, 10]

// Level 4
let cart = [
  { name: "Phone", price: 500, category: "Electronics" },
  { name: "Book", price: 20, category: "Books" },
  { name: "Cable", price: 10, category: "Electronics" }
];
let electronicsTotal = cart
  .filter(item => item.category === "Electronics")
  .reduce((sum, item) => sum + item.price, 0);
console.log(electronicsTotal); // 510

// Level 5
let scores = [88, 92, 55, 70, 100];
let top3 = scores.sort((a, b) => b - a).slice(0, 3);
console.log(top3); // [100, 92, 88]
```

</details>

---

## Review Questions

1. Which method adds an element to the end of an array?
   - [ ] unshift()
   - [x] push()
   - [ ] pop()
   - [ ] shift()

2. What does `pop()` do?
   - [ ] Adds element to end
   - [ ] Removes first element
   - [x] Removes last element
   - [ ] Adds element to beginning

3. Which method is used to search for an element in an array?
   - [ ] search()
   - [x] find()
   - [ ] locate()
   - [ ] get()

4. What does `map()` return?
   - [ ] The original array
   - [x] A new array with transformed elements
   - [ ] A single value
   - [ ] Nothing

5. How do you sort numbers correctly?
   - [ ] array.sort()
   - [x] array.sort((a, b) => a - b)
   - [ ] array.sort((a, b) => b - a)
   - [ ] array.order()

6. What does `filter()` do?
   - [ ] Modifies original array
   - [x] Returns elements that pass condition
   - [ ] Returns first matching element
   - [ ] Removes all elements

7. Which method concatenates arrays?
   - [ ] join()
   - [x] concat()
   - [ ] merge()
   - [ ] combine()

8. What does `slice()` do to the original array?
   - [ ] Modifies it
   - [x] Does not modify it
   - [ ] Deletes elements
   - [ ] Adds elements

9. How do you get the length of an array?
   - [ ] array.size
   - [x] array.length
   - [ ] array.count
   - [ ] array.total

10. What does `reduce()` return?
    - [ ] A new array
    - [x] A single value
    - [ ] The original array
    - [ ] Nothing

---

## Additional Resources

- [MDN: Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [MDN: Array Methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#instance_methods)
- [JavaScript.info: Arrays](https://javascript.info/array)
- [JavaScript.info: Array Methods](https://javascript.info/array-methods)
- [Array Explorer](https://arrayexplorer.com/)
