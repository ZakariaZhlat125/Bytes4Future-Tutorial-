# Session 12: Destructuring, Set & Map

## 📚 Theory (1h)

### Array Destructuring

Array destructuring allows you to unpack values from arrays into distinct variables using a concise syntax.

#### Part 1: Basic Array Destructuring

```javascript
// Basic destructuring
const numbers = [1, 2, 3, 4, 5];
const [first, second, third] = numbers;

console.log(first);  // 1
console.log(second); // 2
console.log(third);  // 3
```

#### Part 2: Skipping and Rest Elements

```javascript
const numbers = [1, 2, 3, 4, 5];

// Skip elements (use commas)
const [first, , third] = numbers;
console.log(first);  // 1
console.log(third);  // 3

// Rest operator for remaining elements
const [first, second, ...rest] = numbers;
console.log(first);  // 1
console.log(second); // 2
console.log(rest);   // [3, 4, 5]
```

#### Part 3: Variable Swapping

```javascript
// Traditional swapping
let a = 1;
let b = 2;
let temp = a;
a = b;
b = temp;

// Destructuring swapping
let x = 1;
let y = 2;
[x, y] = [y, x];
console.log(x); // 2
console.log(y); // 1
```

#### Default Values

```javascript
const numbers = [1, 2];
const [first, second, third = 3] = numbers;

console.log(first);  // 1
console.log(second); // 2
console.log(third);  // 3 (default value)
```

### Object Destructuring

Object destructuring allows you to extract properties from objects into variables.

#### Part 1: Basic Object Destructuring

```javascript
const person = {
    name: "John",
    age: 30,
    city: "New York"
};

const { name, age, city } = person;

console.log(name); // "John"
console.log(age);  // 30
console.log(city); // "New York"
```

#### Part 2: Renaming and Nested Destructuring

```javascript
const person = {
    name: "John",
    age: 30,
    address: {
        city: "New York",
        country: "USA"
    }
};

// Renaming
const { name: userName, age: userAge } = person;
console.log(userName); // "John"
console.log(userAge); // 30

// Nested destructuring
const { address: { city, country } } = person;
console.log(city);    // "New York"
console.log(country); // "USA"
```

#### Default Values

```javascript
const person = {
    name: "John",
    age: 30
};

const { name, age, country = "USA" } = person;
console.log(country); // "USA" (default value)
```

### Destructuring Function Parameters

Destructuring can be used directly in function parameters for cleaner code.

#### Array Destructuring in Parameters

```javascript
function calculateSum([a, b, c]) {
    return a + b + c;
}

console.log(calculateSum([1, 2, 3])); // 6

// With rest parameter
function processArray([first, ...rest]) {
    return { first, rest };
}

console.log(processArray([1, 2, 3, 4])); // { first: 1, rest: [2, 3, 4] }
```

#### Object Destructuring in Parameters

```javascript
function greetUser({ name, age }) {
    console.log(`Hello ${name}, you are ${age} years old`);
}

greetUser({ name: "John", age: 30 }); // "Hello John, you are 30 years old"

// With default values
function createUser({ name = "Anonymous", age = 18 } = {}) {
    return { name, age };
}

console.log(createUser()); // { name: "Anonymous", age: 18 }
console.log(createUser({ name: "Jane" })); // { name: "Jane", age: 18 }
```

### Mixed Content Destructuring

You can destructure arrays and objects together in various ways.

#### Destructuring Nested Structures

```javascript
const data = {
    users: [
        { name: "John", age: 30 },
        { name: "Jane", age: 25 }
    ],
    metadata: {
        count: 2,
        lastUpdated: "2024-01-01"
    }
};

const { users: [{ name: firstName }, { name: secondName }], metadata: { count } } = data;
console.log(firstName);  // "John"
console.log(secondName); // "Jane"
console.log(count);     // 2
```

#### Destructuring in Loops

```javascript
const users = [
    { id: 1, name: "John" },
    { id: 2, name: "Jane" },
    { id: 3, name: "Bob" }
];

for (const { id, name } of users) {
    console.log(`User ${id}: ${name}`);
}
```

#### Destructuring Function Returns

```javascript
function getUser() {
    return {
        name: "John",
        age: 30,
        address: {
            city: "New York",
            country: "USA"
        }
    };
}

const { name, address: { city } } = getUser();
console.log(name); // "John"
console.log(city); // "New York"
```

---

## 💻 Practical (1.5h)

### Exercise 1: Array Destructuring

```javascript
// Exercise 1.1: Basic array destructuring
console.log("=== Basic Array Destructuring ===");

const colors = ["red", "green", "blue", "yellow", "purple"];
const [color1, color2, color3] = colors;

console.log("Color 1:", color1);
console.log("Color 2:", color2);
console.log("Color 3:", color3);

// Exercise 1.2: Skipping elements
console.log("\n=== Skipping Elements ===");

const numbers = [1, 2, 3, 4, 5];
const [first, , third, , fifth] = numbers;

console.log("First:", first);
console.log("Third:", third);
console.log("Fifth:", fifth);

// Exercise 1.3: Rest operator
console.log("\n=== Rest Operator ===");

const values = [10, 20, 30, 40, 50];
const [first, second, ...remaining] = values;

console.log("First:", first);
console.log("Second:", second);
console.log("Remaining:", remaining);

// Exercise 1.4: Variable swapping
console.log("\n=== Variable Swapping ===");

let a = "apple";
let b = "banana";
console.log("Before swap:", a, b);

[a, b] = [b, a];
console.log("After swap:", a, b);

// Exercise 1.5: Default values
console.log("\n=== Default Values ===");

const shortArray = [1, 2];
const [x, y, z = 3] = shortArray;

console.log("x:", x);
console.log("y:", y);
console.log("z:", z);
```

### Exercise 2: Object Destructuring

```javascript
// Exercise 2.1: Basic object destructuring
console.log("=== Basic Object Destructuring ===");

const person = {
    name: "John",
    age: 30,
    email: "john@example.com",
    city: "New York"
};

const { name, age, email } = person;

console.log("Name:", name);
console.log("Age:", age);
console.log("Email:", email);

// Exercise 2.2: Renaming properties
console.log("\n=== Renaming Properties ===");

const { name: userName, age: userAge } = person;
console.log("User name:", userName);
console.log("User age:", userAge);

// Exercise 2.3: Nested destructuring
console.log("\n=== Nested Destructuring ===");

const user = {
    name: "John",
    profile: {
        age: 30,
        address: {
            city: "New York",
            country: "USA"
        }
    }
};

const { profile: { age, address: { city } } } = user;
console.log("Age:", age);
console.log("City:", city);

// Exercise 2.4: Default values
console.log("\n=== Default Values ===");

const incompleteUser = {
    name: "Jane"
};

const { name, age = 25, city = "Unknown" } = incompleteUser;
console.log("Name:", name);
console.log("Age:", age);
console.log("City:", city);
```

### Exercise 3: Destructuring Function Parameters

```javascript
// Exercise 3.1: Array parameters
console.log("=== Array Parameters ===");

function calculateStats([a, b, c, d, e]) {
    const sum = a + b + c + d + e;
    const avg = sum / 5;
    const max = Math.max(a, b, c, d, e);
    const min = Math.min(a, b, c, d, e);
    
    return { sum, avg, max, min };
}

const numbers = [10, 20, 30, 40, 50];
console.log("Stats:", calculateStats(numbers));

// Exercise 3.2: Object parameters
console.log("\n=== Object Parameters ===");

function createUserProfile({ name, age, city = "Unknown" }) {
    return {
        name,
        age,
        city,
        isAdult: age >= 18
    };
}

const person1 = { name: "John", age: 30, city: "New York" };
const person2 = { name: "Jane", age: 25 };

console.log("Profile 1:", createUserProfile(person1));
console.log("Mixed 2:", createUserProfile(person2));

// Exercise 3.3: Mixed destructuring
console.log("\n=== Mixed Destructuring ===");

function processUser({ name, profile: { age }, hobbies = [] }) {
    return {
        name,
        age,
        hobbies,
        hobbyCount: hobbies.length
    };
}

const user = {
    name: "John",
    profile: { age: 30 },
    hobbies: ["reading", "coding"]
};

console.log("Processed user:", processUser(user));

// Exercise 3.4: Rest parameters with destructuring
console.log("\n=== Rest Parameters ===");

function logNumbers([first, ...rest]) {
    console.log("First:", first);
    console.log("Rest:", rest);
}

logNumbers([1, 2, 3, 4, 5]);
```

### Exercise 4: Mixed Content Destructuring

```javascript
// Exercise 4.1: Destructuring arrays of objects
console.log("=== Arrays of Objects ===");

const users = [
    { id: 1, name: "John", details: { age: 30, city: "NY" } },
    { id: 2, name: "Jane", details: { age: 25, city: "LA" } }
];

const [{ name: name1 }, { name: name2 }] = users;
console.log("Name 1:", name1);
console.log("Name 2:", name2);

// Exercise 4.2: Object with arrays
console.log("\n=== Object with Arrays ===");

const data = {
    numbers: [1, 2, 3],
    names: ["John", "Jane", "Bob"],
    metadata: { count: 3 }
};

const { numbers: [n1, n2, n3], names: [name1, name2, name3] } = data;
console.log("Numbers:", n1, n2, n3);
console.log("Names:", name1, name2, name3);

// Exercise 4.3: Complex nested destructuring
console.log("\n=== Complex Nested ===");

const response = {
    data: {
        users: [
            { name: "John", posts: [{ title: "Post 1" }, { title: "Post 2" }] },
            { name: "Jane", posts: [{ title: "Post 3" }] }
        ]
    }
};

const { data: { users: [{ posts: [{ title: title1 }] }] } } = response;
console.log("First post title:", title1);

// Exercise 4.4: Destructuring in forEach
console.log("\n=== Destructuring in forEach ===");

const products = [
    { id: 1, name: "Laptop", price: 999 },
    { id: 2, name: "Phone", price: 699 },
    { id: 3, name: "Tablet", price: 299 }
];

products.forEach(({ name, price }) => {
    console.log(`${name}: $${price}`);
});
```

### Exercise 5: Set Data Type & Methods

```javascript
// Exercise 5.1: Creating Sets
console.log("=== Creating Sets ===");

const set1 = new Set();
set1.add(1);
set1.add(2);
set1.add(3);
set1.add(2); // Duplicate, won't be added

console.log("Set:", set1); // Set {1, 2, 3}
console.log("Size:", set1.size); // 3

// From array
const set2 = new Set([1, 2, 3, 4, 5]);
console.log("Set from array:", set2);

// Exercise 5.2: Set methods
console.log("\n=== Set Methods ===");

const fruits = new Set(["apple", "banana", "orange"]);

// Add
fruits.add("grape");
console.log("After add:", fruits);

// Has
console.log("Has apple:", fruits.has("apple"));
console.log("Has pear:", fruits.has("pear"));

// Delete
fruits.delete("banana");
console.log("After delete:", fruits);

// Clear
fruits.clear();
console.log("After clear:", fruits);

// Exercise 5.3: Set iteration
console.log("\n=== Set Iteration ===");

const numbers = new Set([1, 2, 3, 4, 5]);

// forEach
numbers.forEach(num => console.log("forEach:", num));

// for...of
console.log("for...of:");
for (const num of numbers) {
    console.log(num);
}

// Convert to array
const numberArray = [...numbers];
console.log("Array:", numberArray);

// Exercise 5.4: Set operations
console.log("\n=== Set Operations ===");

const setA = new Set([1, 2, 3]);
const setB = new Set([3, 4, 5]);

// Union
const union = new Set([...setA, ...setB]);
console.log("Union:", union); // {1, 2, 3, 4, 5}

// Intersection
const intersection = new Set([...setA].filter(x => setB.has(x)));
console.log("Intersection:", intersection); // {3}

// Difference
const difference = new Set([...setA].filter(x => !setB.has(x)));
console.log("Difference:", difference); // {1, 2}
```

### Exercise 6: Set vs WeakSet

```javascript
// Exercise 6.1: Set vs WeakSet
console.log("=== Set vs WeakSet ===");

const set = new Set();
const weakSet = new WeakSet();

let obj = { id: 1 };

set.add(obj);
weakSet.add(obj);

console.log("Set size:", set.size);     // 1
console.log("WeakSet size:", weakSet.size); // undefined (not enumerable)

// WeakSet methods
console.log("Has in WeakSet:", weakSet.has(obj)); // true
weakSet.delete(obj);
console.log("Has after delete:", weakSet.has(obj)); // false

// Exercise 6.2: Garbage collection demonstration
console.log("\n=== Garbage Collection ===");

const regularSet = new Set();
const weakSet2 = new WeakSet();

let obj1 = { id: 1 };
let obj2 = { id: 2 };

regularSet.add(obj1);
weakSet2.add(obj2);

console.log("Regular set has obj1:", regularSet.has(obj1));
console.log("WeakSet has obj2:", weakSet2.has(obj2));

// Remove references
obj1 = null;
obj2 = null;

// Regular set keeps the object (memory leak risk)
// WeakSet allows garbage collection
console.log("Regular set after null:", regularSet.has(obj1)); // true (if not forced GC)

// Exercise 6.3: WeakSet use cases
console.log("\n=== WeakSet Use Cases ===");

function createWeakCache() {
    const cache = new WeakSet();
    
    return {
        add(item) {
            cache.add(item);
        },
        has(item) {
            return cache.has(item);
        },
        remove(item) {
            cache.delete(item);
        }
    };
}

const weakCache = createWeakCache();
const tempObj = { data: "temporary" };

weakCache.add(tempObj);
console.log("Has tempObj:", weakCache.has(tempObj));

// When tempObj is no longer needed, it can be garbage collected
tempObj = null;

// Exercise 6.4: Performance comparison
console.log("\n=== Performance Comparison ===");

// Set - can store any value, iterates in insertion order
const numericSet = new Set([3, 1, 2]);
console.log("Set iteration order:", [...numericSet]); // [3, 1, 2]

// WeakSet - only objects, cannot be iterated
const objectSet = new WeakSet();
objectSet.add({ id: 1 });
objectSet.add({ id: 2 });
// Cannot use [...objectSet] - not iterable
```

### Exercise 7: Map vs Object

```javascript
// Exercise 7.1: Creating Maps
console.log("=== Creating Maps ===");

const map = new Map();
map.set("name", "John");
map.set("age", 30);
map.set("city", "New York");

console.log("Map:", map);
console.log("Size:", map.size);

// From array
const map2 = new Map([
    ["name", "Jane"],
    ["age", 25],
    ["city", "Los Angeles"]
]);
console.log("Map from array:", map2);

// Exercise 7.2: Map vs Object comparison
console.log("\n=== Map vs Object ===");

const obj = {
    name: "John",
    age: 30,
    city: "New York"
};

const map3 = new Map([
    ["name", "John"],
    ["age", 30],
    ["city", "New York"]
]);

// Keys
console.log("Object keys:", Object.keys(obj));
console.log("Map keys:", [...map3.keys()]);

// Values
console.log("Object values:", Object.values(obj));
console.log("Map values:", [...map3.values()]);

// Size
console.log("Object size:", Object.keys(obj).length);
console.log("Map size:", map3.size);

// Exercise 7.3: Key types
console.log("\n=== Key Types ===");

const map4 = new Map();
map4.set("string", "value");
map4.set(1, "number key");
map4.set(true, "boolean key");
map4.set({ id: 1 }, "object key");

console.log("Map with various keys:", map4);

// Object keys are always strings
const obj2 = {};
obj2["string"] = "value";
obj2[1] = "number key";
obj2[true] = "boolean key";
obj2[{ id: 1 }] = "object key";

console.log("Object keys:", Object.keys(obj2)); // All converted to strings

// Exercise 7.4: Key ordering
console.log("\n=== Key Ordering ===");

const orderedMap = new Map([
    ["a", 1],
    ["b", 2],
    ["c", 3]
]);

console.log("Map maintains insertion order:", [...orderedMap.keys()]); // ["a", "b", "c"]

const obj3 = { c: 3, a: 1, b: 2 };
console.log("Object key order:", Object.keys(obj3)); // May vary
```

### Exercise 8: Map Methods

```javascript
// Exercise 8.1: Basic Map methods
console.log("=== Basic Map Methods ===");

const map = new Map([
    ["name", "John"],
    ["age", 30],
    ["city", "New York"]
]);

// Get
console.log("Get name:", map.get("name"));
console.log("Get missing:", map.get("country")); // undefined

// Has
console.log("Has name:", map.has("name"));
console.log("Has country:", map.has("country"));

// Set
map.set("email", "john@example.com");
console.log("After set:", map.get("email"));

// Delete
map.delete("age");
console.log("After delete:", map.has("age"));

// Clear
// map.clear();

// Exercise 8.2: Map iteration
console.log("\n=== Map Iteration ===");

const map2 = new Map([
    ["name", "John"],
    ["age", 30],
    ["city", "New York"]
]);

// forEach
map2.forEach((value, key) => {
    console.log(`${key}: ${value}`);
});

// for...of
console.log("for...of entries:");
for (const [key, value] of map2.entries()) {
    console.log(`${key}: ${value}`);
}

// Exercise 8.3: Map to array conversion
console.log("\n=== Map to Array ===");

const map3 = new Map([
    ["name", "John"],
    ["age", 30],
    ["city", "New York"]
]);

const keys = [...map3.keys()];
const values = [...map3.values()];
const entries = [...map3.entries()];

console.log("Keys:", keys);
console.log("Values:", values);
console.log("Entries:", entries);

// Exercise 8.4: Object to Map and back
console.log("\n=== Object to Map ===");

const obj = { name: "John", age: 30, city: "New York" };
const map4 = new Map(Object.entries(obj));
console.log("Object to Map:", map4);

const obj2 = Object.fromEntries(map4);
console.log("Map to Object:", obj2);

// Exercise 8.5: Advanced Map operations
console.log("\n=== Advanced Map Operations ===");

const userMap = new Map([
    [1, { name: "John", age: 30 }],
    [2, { name: "Jane", age: 25 }],
    [3, { name: "Bob", age: 35 }]
]);

// Map over values
const updatedUsers = new Map(
    [...userMap].map(([id, user]) => [id, { ...user, processed: true }])
);
console.log("Updated users:", updatedUsers);

// Filter entries
const filteredUsers = new Map(
    [...userMap].filter(([id, user]) => user.age >= 30)
);
console.log("Filtered users (age >= 30):", filteredUsers);
```

### Exercise 9: Map vs WeakMap

```javascript
// Exercise 9.1: WeakMap basics
console.log("=== WeakMap Basics ===");

const weakMap = new WeakMap();
let key = { id: 1 };
let value = { data: "important" };

weakMap.set(key, value);
console.log("Has key:", weakMap.has(key));
console.log("Get value:", weakMap.get(key));

weakMap.delete(key);
console.log("After delete:", weakMap.has(key));

// Exercise 9.2: Map vs WeakMap comparison
console.log("\n=== Map vs WeakMap ===");

const map = new Map();
const weakMap2 = new WeakMap();

let obj = { id: 1 };

map.set(obj, "data");
weakMap2.set(obj, "data");

console.log("Map size:", map.size);
console.log("WeakMap size:", weakMap2.size); // undefined

// Exercise 9.3: Garbage collection in WeakMap
console.log("\n=== WeakMap Garbage Collection ===");

const cache = new WeakMap();
let data1 = { id: 1, info: "data 1" };
let data2 = { id: 2, info: "data 2" };

cache.set(data1, "value1");
cache.set(data2, "value2");

console.log("Has data1:", cache.has(data1));
console.log("Has data2:", cache.has(data2));

// Remove reference
data1 = null;

// data1 can be garbage collected, its entry removed from WeakMap
console.log("After removing reference:", cache.has(data1)); // false

// Exercise 9.4: WeakMap use cases
console.log("\n=== WeakMap Use Cases ===");

function createMetadataCache() {
    const metadata = new WeakMap();
    
    return {
        setMetadata(object, data) {
            metadata.set(object, data);
        },
        getMetadata(object) {
            return metadata.get(object);
        },
        hasMetadata(object) {
            return metadata.has(object);
        }
    };
}

const metaCache = createMetadataCache();
const myObject = { name: "My Object" };

metaCache.setMetadata(myObject, { created: Date.now(), type: "custom" });
console.log("Metadata:", metaCache.getMetadata(myObject));
```

### Exercise 10: Array.from

```javascript
// Exercise 10.1: Array.from basics
console.log("=== Array.from Basics ===");

// From array-like objects
const arrayLike = { 0: "a", 1: "b", 2: "c", length: 3 };
const array1 = Array.from(arrayLike);
console.log("From array-like:", array1); // ["a", "b", "c"]

// From Set
const set = new Set([1, 2, 3, 4, 5]);
const array2 = Array.from(set);
console.log("From Set:", array2); // [1, 2, 3, 4, 5]

// From Map
const map = new Map([["a", 1], ["b", 2]]);
const array3 = Array.from(map.values());
console.log("From Map values:", array3); // [1, 2]

// Exercise 10.2: Array.from with mapping function
console.log("\n=== Array.from with Mapping ===");

const numbers = [1, 2, 3, 4, 5];
const doubled = Array.from(numbers, num => num * 2);
console.log("Doubled:", doubled); // [2, 4, 6, 8, 10]

// Exercise 10.3: Array.from from string
console.log("\n=== Array.from from String ===");

const str = "hello";
const chars = Array.from(str);
console.log("Characters:", chars); // ["h", "e", "l", "l", "o"]

// Exercise 10.4: Array.from with length
console.log("\n=== Array.from with Length ===");

const array4 = Array.from({ length: 5 }, (_, i) => i + 1);
console.log("Generated array:", array4); // [1, 2, 3, 4, 5]

// Exercise 10.5: Practical example - Range generation
console.log("\n=== Range Generation ===");

function range(start, end, step = 1) {
    return Array.from(
        { length: Math.ceil((end - start) / step) },
        (_, i) => start + i * step
    );
}

console.log("Range 1-10:", range(1, 10));
console.log("Range 1-10 step 2:", range(1, 10, 2));
console.log("Range 10-1:", range(10, 1, -1));
```

### Exercise 11: Array.copyWithin

```javascript
// Exercise 11.1: Basic copyWithin
console.log("=== Basic copyWithin ===");

const array1 = [1, 2, 3, 4, 5, 6, 7, 8, 9];

// copyWithin(target, start, end)
// Copies elements from start to end to target position
array1.copyWithin(3, 0, 3);
console.log("After copyWithin(3, 0, 3):", array1); // [1, 2, 3, 1, 2, 3, 7, 8, 9]

// Exercise 11.2: copyWithin variations
console.log("\n=== copyWithin Variations ===");

const array2 = [1, 2, 3, 4, 5, 6, 7, 8, 9];

// Only target and start (end defaults to length)
array2.copyWithin(2, 0);
console.log("copyWithin(2, 0):", array2); // [1, 2, 1, 2, 3, 4, 5, 6, 7, 8]

// Exercise 11.3: Practical example - array rotation
console.log("\n=== Array Rotation ===");

function rotateLeft(array, positions) {
    const copy = [...array];
    copy.copyWithin(0, positions);
    return copy;
}

function rotateRight(array, positions) {
    const copy = [...array];
    copy.copyWithin(array.length - positions, 0, array.length - positions);
    return copy;
}

const numbers = [1, 2, 3, 4, 5];
console.log("Original:", numbers);
console.log("Rotate left 2:", rotateLeft(numbers, 2));
console.log("Rotate right 2:", rotateRight(numbers, 2));

// Exercise 11.4: Overlapping copy
console.log("\n=== Overlapping Copy ===");

const array3 = [1, 2, 3, 4, 5];
array3.copyWithin(1, 0, 3);
console.log("Overlapping copyWithin(1, 0, 3):", array3); // [1, 1, 2, 3, 5]
```

### Exercise 12: Array.some

```javascript
// Exercise 12.1: Basic some
console.log("=== Basic some ===");

const numbers = [1, 2, 3, 4, 5];

const hasEven = numbers.some(num => num % 2 === 0);
console.log("Has even number:", hasEven); // true

const hasNegative = numbers.some(num => num < 0);
console.log("Has negative:", hasNegative); // false

// Exercise 12.2: Practical examples
console.log("\n=== Practical Examples ===");

const users = [
    { name: "John", age: 30, active: true },
    { name: "Jane", age: 25, active: false },
    { name: "Bob", age: 35, active: true }
];

const hasActiveUser = users.some(user => user.active);
console.log("Has active user:", hasActiveUser); // true

const hasSenior = users.some(user => user.age >= 65);
console.log("Has senior:", hasSenior); // false

// Exercise 12.3: some with objects
console.log("\n=== some with Objects ===");

const products = [
    { name: "Laptop", price: 999, inStock: true },
    { name: "Phone", price: 699, inStock: false },
    { name: "Tablet", price: 299, inStock: true }
];

const hasOutOfStock = products.some(product => !product.inStock);
console.log("Has out of stock:", hasOutOfStock); // true

const hasExpensive = products.some(product => product.price > 500);
console.log("Has expensive item:", hasExpensive); // true

// Exercise 12.4: some with strings
console.log("\n=== some with Strings ===");

const emails = ["john@example.com", "jane@example.com", "invalid-email"];
const hasInvalidEmail = emails.some(email => !email.includes("@"));
console.log("Has invalid email:", hasInvalidEmail); // true
```

### Exercise 13: Array.every

```javascript
// Exercise 13.1: Basic every
console.log("=== Basic every ===");

const numbers = [2, 4, 6, 8, 10];

const allEven = numbers.every(num => num % 2 === 0);
console.log("All even:", allEven); // true

const allPositive = numbers.every(num => num > 0);
console.log("All positive:", allPositive); // true

// Exercise 13.2: Practical examples
console.log("\n=== Practical Examples ===");

const users = [
    { name: "John", age: 30, active: true },
    { name: "Jane", age: 25, active: true },
    { name: "Bob", age: 35, active: true }
];

const allActive = users.every(user => user.active);
console.log("All active:", allActive); // true

const allAdults = users.every(user => user.age >= 18);
console.log("All adults:", allAdults); // true

// Exercise 13.3: every with validation
console.log("\n=== Validation with every ===");

const passwords = ["Password123", "Secret456", "Secure789"];
const allLongEnough = passwords.every(pwd => pwd.length >= 8);
console.log("All long enough:", allLongEnough); // true

const allHaveNumber = passwords.every(pwd => /\d/.test(pwd));
console.log("All have numbers:", allHaveNumber); // true

// Exercise 13.4: every vs some comparison
console.log("\n=== every vs some ===");

const mixedNumbers = [1, 2, 3, 4, 5, 6];

console.log("Some even:", mixedNumbers.some(n => n % 2 === 0)); // true
console.log("All even:", mixedNumbers.every(n => n % 2 === 0)); // false

console.log("Some positive:", mixedNumbers.some(n => n > 0)); // true
console.log("All positive:", mixedNumbers.every(n => n > 0)); // true
```

### Exercise 14: Spread Syntax

```javascript
// Exercise 14.1: Spread with arrays
console.log("=== Spread with Arrays ===");

const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

const combined = [...arr1, ...arr2];
console.log("Combined:", combined); // [1, 2, 3, 4, 5, 6]

// Exercise 14.2: Spread with objects
console.log("\n=== Spread with Objects ===");

const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };

const merged = { ...obj1, ...obj2 };
console.log("Merged:", merged); // { a: 1, b: 2, c: 3, d: 4 }

// Exercise 14.3: Spread with function calls
console.log("\n=== Spread with Function Calls ===");

function sum(a, b, c) {
    return a + b + c;
}

const numbers = [1, 2, 3];
console.log("Sum:", sum(...numbers));

// Exercise 14.4: Spread with strings
console.log("\n=== Spread with Strings ===");

const str = "hello";
const chars = [...str];
console.log("Characters:", chars); // ["h", "e", "l", "l", "o"]

// Exercise 14.5: Spread in array literals
console.log("\n=== Spread in Array Literals ===");

const baseArray = [1, 2, 3];
const newArray = [...baseArray, 4, 5];
console.log("New array:", newArray); // [1, 2, 3, 4, 5]

// Exercise 14.6: Spread for cloning
console.log("\n=== Spread for Cloning ===");

const original = [1, 2, 3];
const clone = [...original];
clone[0] = 99;

console.log("Original:", original); // [1, 2, 3]
console.log("Clone:", clone);     // [99, 2, 3]
```

### Exercise 15: Complete Working Example

**Complete script.js:**
```javascript
// Session 12: Destructuring, Set & Map
// This script demonstrates modern JavaScript features

console.log("=== Session 12: Destructuring, Set & Map ===");

// 1. Array destructuring
console.log("\n--- Array Destructuring ---");
const numbers = [1, 2, 3, 4, 5];
const [first, second, ...rest] = numbers;
console.log("First:", first, "Second:", second, "Rest:", rest);

// 2. Object destructuring
console.log("\n--- Object Destructuring ---");
const person = { name: "John", age: 30, city: "New York" };
const { name, age, city } = person;
console.log("Name:", name, "Age:", age, "City:", city);

// 3. Set
console.log("\n--- Set ---");
const set = new Set([1, 2, 3, 4, 5]);
set.add(6);
console.log("Set:", [...set]);
console.log("Size:", set.size);

// 4. Map
console.log("\n--- Map ---");
const map = new Map([["name", "John"], ["age", 30]]);
map.set("city", "New York");
console.log("Map:", map);
console.log("Get name:", map.get("name"));

// 5. Array methods
console.log("\n--- Array Methods ---");
const arr = [1, 2, 3, 4, 5];
console.log("Some even:", arr.some(n => n % 2 === 0));
console.log("All positive:", arr.every(n => n > 0));
console.log("Array.from range:", Array.from({ length: 5 }, (_, i) => i + 1));

// 6. Spread syntax
console.log("\n--- Spread Syntax ---");
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log("Combined:", combined);

console.log("\n=== Session 12 Complete ===");
```

**Complete index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 12 - Destructuring, Set & Map</title>
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
        .demo input, .demo button {
            padding: 8px;
            margin: 5px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        .demo button {
            background-color: # #1890ff;
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
        .interactive-box {
            background: white;
            padding: 15px;
            border-radius: 4px;
            border: 1px solid #e8e8e8;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h1>Session 12: Destructuring, Set & Map</h1>
    
    <div class="section">
        <h2>Topics Covered</h2>
        <ul>
            <li>Array destructuring (basic, skipping, rest, swapping)</li>
            <li>Object destructuring (basic, renaming, nested, defaults)</li>
            <li>Destructuring function parameters</li>
            <li>Mixed content destructuring</li>
            <li>Set data type & methods</li>
            <li>Set vs WeakSet</li>
            <li>Map vs Object</li>
            <li>Map methods</li>
            <li>Map vs WeakMap</li>
            <li>Array.from, Array.copyWithin</li>
            <li>Array.some, Array.every</li>
            <Spread syntax</li>
        </ul>
    </div>

    <div class="section">
        <h2>Destructuring Demo</h2>
        <div class="demo">
            <button onclick="demoArrayDestructuring()">Array Destructuring</button>
            <button onclick="demoObjectDestructuring()">Object Destructuring</button>
            <button onclick="demoDestructuringParams()">Destructuring Parameters</button>
            <div id="destructuringOutput" class="output">
                Click to see destructuring examples...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Set Demo</h2>
        <div class="demo">
            <input type="text" id="setItem" placeholder="Add item to Set">
            <button onclick="addToSet()">Add to Set</button>
            <button onclick="clearSet()">Clear Set</button>
            <div id="setOutput" class="output">
                Set contents will appear here...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Map Demo</h2>
        <div class="demo">
            <input type="text" id="mapKey" placeholder="Key">
            <input type="text" id="mapValue" placeholder="Value">
            <button onclick="addToMap()">Add to Map</button>
            <button onclick="getFromMap()">Get from Map</button>
            <button onclick="clearMap()">Clear Map</button>
            <div id="mapOutput" class="output">
                Map contents will appear here...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Array Methods Demo</h2>
        <div class="demo">
            <button onclick="demoArrayFrom()">Array.from</button>
            <button onclick="demoSomeEvery()">some & every</button>
            <button onclick="demoSpread()">Spread Syntax</button>
            <div id="arrayOutput" class="output">
                Click to see array method examples...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Interactive Demo</h2>
        <div class="demo">
            <div class="interactive-box">
                <h3>Shopping Cart (Map)</h3>
                <input type="text" id="productName" placeholder="Product name">
                <input type="number" id="productPrice" placeholder="Price">
                <button onclick="addToCart()">Add to Cart</button>
                <button onclick="showCart()">Show Cart</button>
                <button onclick="clearCart()">Clear Cart</button>
                <div id="cartOutput" class="output">
                    Cart will appear here...
                </div>
            </div>
            
            <div class="interactive-box">
                <h3>Todo List (Set)</h3>
                <input type="text" id="todoItem" placeholder="Todo item">
                <button onclick="addTodo()">Add Todo</button>
                <button onclick="showTodos()">Show Todos</button>
                <button onclick="removeCompleted()">Remove Completed</button>
                <div id="todoOutput" class="output">
                    Todos will appear here...
                </div>
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Console Output</h2>
        <p>Open the browser console (F12) to see all JavaScript examples.</p>
    </div>

    <script src="script.js" defer></script>
    <script>
        // Destructuring demos
        function demoArrayDestructuring() {
            const colors = ["red", "green", "blue", "yellow", "purple"];
            const [color1, color2, ...rest] = colors;
            
            let output = "=== Array Destructuring ===\n";
            output += `Color 1: ${color1}\n`;
            output += `Color 2: ${color2}\n`;
            output += `Rest: ${rest.join(", ")}\n`;
            
            // Variable swap
            let a = "apple", b = "banana";
            [a, b] = [b, a];
            output += `\nSwap result: ${a}, ${b}`;
            
            document.getElementById("destructuringOutput").textContent = output;
        }

        function demoObjectDestructuring() {
            const person = {
                name: "John",
                age: 30,
                address: { city: "New York", country: "USA" }
            };
            
            const { name, address: { city } } = person;
            
            let output = "=== Object Destructuring ===\n";
            output += `Name: ${name}\n`;
            output += `City: ${city}\n`;
            
            document.getElementById("destructuringOutput").textContent = output;
        }

        function demoDestructuringParams() {
            function processUser({ name, age, city = "Unknown" }) {
                return { name, age, city };
            }
            
            const user = { name: "John", age: 30 };
            const result = processUser(user);
            
            document.getElementById("destructuringOutput").textContent = 
                `Processed user: ${JSON.stringify(result)}`;
        }

        // Set demo
        let mySet = new Set();

        function addToSet() {
            const item = document.getElementById("setItem").value;
            if (item) {
                mySet.add(item);
                updateSetOutput();
                document.getElementById("setItem").value = "";
            }
        }

        function clearSet() {
            mySet.clear();
            updateSetOutput();
        }

        function updateSetOutput() {
            document.getElementById("setOutput").textContent = 
                `Set contents: ${[...mySet].join(", ")}\nSize: ${mySet.size}`;
        }

        // Map demo
        let myMap = new Map();

        function addToMap() {
            const key = document.getElementById("mapKey").value;
            const value = document.getElementById("mapValue").value;
            
            if (key && value) {
                myMap.set(key, value);
                updateMapOutput();
                document.getElementById("mapKey").value = "";
                document.getElementById("mapValue").value = "";
            }
        }

        function getFromMap() {
            const key = document.getElementById("mapKey").value;
            const value = myMap.get(key);
            document.getElementById("mapOutput").textContent = 
                value ? `Value for "${key}": ${value}` : `Key "${key}" not found`;
        }

        function clearMap() {
            myMap.clear();
            updateMapOutput();
        }

        function updateMapOutput() {
            let output = "Map contents:\n";
            for (const [key, value] of myMap) {
                output += `${key}: ${value}\n`;
            }
            output += `Size: ${myMap.size}`;
            document.getElementById("mapOutput").textContent = output;
        }

        // Array methods demo
        function demoArrayFrom() {
            const numbers = [1, 2, 3, 4, 5];
            const doubled = Array.from(numbers, n => n * 2);
            const range = Array.from({ length: 10 }, (_, i) => i + 1);
            
            let output = "=== Array.from ===\n";
            output += `Original: ${numbers.join(", ")}\n`;
            output += `Doubled: ${doubled.join(", ")}\n`;
            output += `Range 1-10: ${range.join(", ")}\n`;
            
            document.getElementById("arrayOutput").textContent = output;
        }

        function demoSomeEvery() {
            const numbers = [1, 2, 3, 4, 5, 6];
            
            let output = "=== some & every ===\n";
            output += `Numbers: ${numbers.join(", ")}\n`;
            output += `Some even: ${numbers.some(n => n % 2 === 0)}\n`;
            output += `All even: ${numbers.every(n => n % 2 === 0)}\n`;
            output += `Some positive: ${numbers.some(n => n > 0)}\n`;
            output += `All positive: ${numbers.every(n => n > 0)}\n`;
            
            document.getElementById("arrayOutput")..textContent = output;
        }

        function demoSpread() {
            const arr1 = [1, 2, 3];
            const arr2 = [4, 5, 6];
            const obj1 = { a: 1, b: 2 };
            const obj2 = { c: 3, d: 4 };
            
            let output = "=== Spread Syntax ===\n";
            output += `Combined arrays: ${[...arr1, ...arr2].join(", ")}\n`;
            output += `Merged objects: ${JSON.stringify({...obj1, ...obj2})}\n`;
            output += `Spread in function: ${Math.max(...arr1)}`;
            
            document.getElementById("arrayOutput").textContent = output;
        }

        // Interactive demos
        let cart = new Map();
        let todos = new Set();

        function addToCart() {
            const name = document.getElementById("productName").value;
            const price = parseFloat(document.getElementById("productPrice").value);
            
            if (name && !isNaN(price)) {
                cart.set(name, price);
                updateCartOutput();
                document.getElementById("productName").value = "";
                document.getElementById("productPrice").value = "";
            }
        }

        function showCart() {
            let total = 0;
            let output = "Cart:\n";
            
            for (const [name, price] of cart) {
                total += price;
                output += `${name}: $${price}\n`;
            }
            
            output += `\nTotal: $${total.toFixed(2)}`;
            document.getElementById("cartOutput").textContent = output;
        }

        function clearCart() {
            cart.clear();
            document.getElementById("cartOutput").textContent = "Cart cleared";
        }

        function updateCartOutput() {
            let output = `Cart (${cart.size} items):\n`;
            for (const [name, price] of cart) {
                output += `${name}: $${price}\n`;
            }
            document.getElementById("cartOutput").textContent = output;
        }

        function addTodo() {
            const item = document.getElementById("todoItem").value;
            if (item) {
                todos.add({ item, completed: false, createdAt: Date.now() });
                updateTodoOutput();
                document.getElementById("todoItem").value = "";
            }
        }

        function showTodos() {
            let output = "Todos:\n";
            for (const todo of todos) {
                const status = todo.completed ? "✓" : "○";
                output += `${status} ${todo.item}\n`;
            }
            document.getElementById("todoOutput").textContent = output;
        }

        function removeCompleted() {
            const completed = [...todos].filter(todo => todo.completed);
            completed.forEach(todo => todos.delete(todo));
            updateTodoOutput();
        }

        function updateTodoOutput() {
            let output = `Todos (${todos.size}):\n`;
            for (const todo of todos) {
                const status = todo.completed ? "✓" : "○";
                output += `${status} ${todo.item}\n`;
            }
            document.getElementById("todoOutput").textContent = output;
        }
    </script>
</body>
</html>
```

---

## 📝 Review (0.5h)

### Destructuring Challenge

```javascript
// Challenge 1: Destructure nested data structure
const data = {
    user: {
        name: "John",
        profile: {
            age: 30,
            address: {
                city: "New York",
                country: "USA"
            }
        }
    },
    posts: [
        { title: "Post 1", likes: 10 },
        { title: "Post 2", likes: 20 }
    ]
};

// Extract: name, age, city, first post title, first post likes

// Challenge 2: Destructure function parameters
function processData({ users: [{ name: firstUser }], metadata: { count } }) {
    // Extract: firstUser, count
}

// Challenge 3: Swap multiple variables
let a = 1, b = 2, c = 3;
// Swap a, b, c to become c, a, b

// Challenge 4: Default values in destructuring
const config = { host: "localhost" };
// Destructure with defaults for port (8080) and ssl (false)

// Challenge 5: Mixed destructuring
const response = {
    data: {
        items: [
            { id: 1, name: "Item 1", price: 10 },
            { id: 2, name: "Item 2", price: 20 }
        ]
    }
};
// Extract first item name and price
```

### Map/Set Challenge

```javascript
// Challenge 1: Create a word frequency counter using Map
const text = "hello world hello world world";
// Count word occurrences

// Challenge 2: Create a cache using Map with expiration
// Add items with timestamps
// Remove items older than 5 seconds

// Challenge 3: Implement a simple in-memory database using Map
// Table: users
// CRUD operations: create, read, update, delete

// Challenge 4: Create a unique ID generator using Set
// Generate unique IDs and track used IDs

// Challenge 5: Build a shopping cart with Map
// Add items, update quantities, remove items
// Calculate total
// Persist to localStorage
```

### Challenge Solutions

**Destructuring Challenge Solutions:**
```javascript
// Challenge 1
const data = {
    user: {
        name: "John",
        profile: {
            age: 30,
            address: {
                city: "New York",
                country: "USA"
            }
        }
    },
    posts: [
        { title: "Post 1", likes: 10 },
        { title: "Post 2", likes: 20 }
    ]
};

const { 
    user: { 
        profile: { 
            age, 
            address: { city } 
        } 
    }, 
    posts: [{ 
        title: firstPostTitle, 
        likes: firstPostLikes 
    }] 
} = data;

console.log("Name:", data.user.name);
console.log("Age:", age);
console.log("City:", city);
console.log("First post title:", firstPostTitle);
console.log("First post likes:", firstPostLikes);

// Challenge 2
function processData({ users: [{ name: firstUser }], metadata: { count } }) {
    return { firstUser, count };
}

// Challenge 3
let a = 1, b = 2, c = 3;
[a, b, c] = [c, a, b];

// Challenge 4
const config = { host: "localhost" };
const { host, port = 8080, ssl = false } = config;

// Challenge 5
const response = {
    data: {
        items: [
            { id: 1, name: "Item 1", price: 10 },
            { id: 2, name: "Item 2", price: 20 }
        ]
    }
};

const { 
    data: { 
        items: [{ 
            name: itemName, 
            price: itemPrice 
        }] 
    } 
} = response;
```

**Map/Set Challenge Solutions:**
```javascript
// Challenge 1: Word frequency counter
const text = "hello world hello world world";
const words = text.split(" ");
const wordCount = new Map();

words.forEach(word => {
    wordCount.set(word, (wordCount.get(word) || 0) + 1);
});

console.log("Word frequency:", wordCount);

// Challenge 2: Cache with expiration
class ExpiringCache {
    constructor(ttl = 5000) {
        this.cache = new Map();
        this.ttl = ttl;
    }
    
    set(key, value) {
        this.cache.set(key, {
            value,
            expires: Date.now() + this.ttl
        });
    }
    
    get(key) {
        const item = this.cache.get(key);
        if (!item) return undefined;
        
        if (Date.now() > item.expires) {
            this.cache.delete(key);
            return undefined;
        }
        
        return item.value;
    }
    
    cleanup() {
        const now = Date.now();
        for (const [key, item] of this.cache) {
            if (now > item.expires) {
                this.cache.delete(key);
            }
        }
    }
}

// Challenge 3: In-memory database
class SimpleDB {
    constructor() {
        this.tables = new Map();
    }
    
    createTable(tableName) {
        this.tables.set(tableName, new Map());
    }
    
    insert(tableName, id, data) {
        const table = this.tables.get(tableName);
        if (!table) return false;
        table.set(id, data);
        return true;
    }
    
    select(tableName, id) {
        const table = this.tables.get(tableName);
        if (!table) return undefined;
        return table.get(id);
    }
    
    update(tableName, id, data) {
        const table = this.tables.get(tableName);
        if (!table || !table.has(id)) return false;
        table.set(id, { ...table.get(id), ...data });
        return true;
    }
    
    delete(tableName, id) {
        const table = this.tables.get(tableName);
        if (!table) return false;
        return table.delete(id);
    }
}

// Challenge 4: Unique ID generator
class UniqueIDGenerator {
    constructor() {
        this.usedIds = new Set();
        this.counter = 0;
    }
    
    generate() {
        let id;
        do {
            id = ++this.counter;
        } while (this.usedIds.has(id));
        
        this.usedIds.add(id);
        return id;
    }
    
    release(id) {
        this.usedIds.delete(id);
    }
}

// Challenge 5: Shopping cart with Map
class ShoppingCart {
    constructor() {
        this.cart = new Map();
    }
    
    addItem(name, price, quantity = 1) {
        const existing = this.cart.get(name);
        if (existing) {
            this.cart.set(name, {
                ...existing,
                quantity: existing.quantity + quantity
            });
        } else {
            this.cart.set(name, { name, price, quantity });
        }
    }
    
    removeItem(name) {
        this.cart.delete(name);
    }
    
    updateQuantity(name, quantity) {
        const item = this.cart.get(name);
        if (item) {
            item.quantity = quantity;
            this.cart.set(name, item);
        }
    }
    
    getTotal() {
        let total = 0;
        for (const { price, quantity } of this.cart.values()) {
            total += price * quantity;
        }
        return total;
    }
    
    getItems() {
        return [...this.cart.entries()];
    }
    
    clear() {
        this.cart.clear();
    }
}
```

### Review Questions

1. **What does array destructuring do?**
   - [ ] Creates a new array
   - [ ] Unpacks array values into variables
   - [ ] Deletes array elements
   - [ ] Sorts array elements

2. **What is the rest operator in destructuring?**
   - [ ] Removes elements
   - [ ] Collects remaining elements
   - [ ] Duplicates elements
   - [ ] Reverses elements

3. **What is the main difference between Set and WeakSet?**
   - [ ] No difference
   - [ ] Set can store any value, WeakSet only objects
   - [ ] WeakSet is iterable, Set is not
   - [ ] Set prevents garbage collection, WeakSet allows it

4. **What is the main difference between Map and Object?**
   - [ ] No difference
   - [ ] Map can have any key type, Object keys are strings
   - [ ] Object is iterable, Map is not
   - [ ] Map maintains insertion order, Object may not

5. **What does Array.from() do?**
   - [ ] Creates an array from an array-like object
   - [ ] Converts array to string
   - [ ] Sorts array elements
   - [ ] Deletes array elements

6. **What does Array.some() return?**
   - [ ] true if all elements pass condition
   - [ ] true if any element passes condition
   - [ ] The first element
   - [ ] All elements

7. **What does Array.every() return?**
   - [ ] true if all elements pass condition
   - [ ] true if any element passes condition
   - [ ] The first element
   - [ ] All elements

8. **What does the spread operator do?**
   - [ ] Removes elements
   - [ ] Expands an iterable into individual elements
   - [ ] Sorts elements
   - [ ] Filters elements

9. **What does copyWithin() do?**
   - [ ] Copies array from another array
   - [ ] Copies array elements within the same array
   - [ ] Removes array elements
   - [ ] Reverses array elements

10. **Can you destructure objects in function parameters?**
    - [ ] No
    - [ ] Yes
    - [ ] Only with arrays
    - [ ] Only with primitives

### Correct Answers

1. ✅ Unpacks array values into variables
2. ✅ Collects remaining elements
3. ✅ Set can store any value, WeakSet only objects
4. ✅ Map can have any key type, Object keys are strings
5. ✅ Creates an array from an array-like object
6. ✅ true if any element passes condition
7. ✅ true if all elements pass condition
8. ✅ Expands an iterable into individual elements
9. ✅ Copies array elements within the same array
10. ✅ Yes

---

## 🎯 Next Steps

1. ✅ Practice all destructuring patterns
2. ✅ Master Set and Map operations
3. ✅ Understand when to use WeakSet and WeakMap
4. ✅ Practice array methods (from, copyWithin, some, every)
5. ✅ Master spread operator for cloning and merging
6. ✅ Build practical applications with these features
7. �   Learn about other modern JavaScript features (optional chaining, nullish coalescing)

---

## 📚 Additional Resources

- [MDN: Destructuring Assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
- [MDN: Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)
- [MDN: Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)
- [MDN: WeakSet](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet)
- [MDN: WeakMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)
- [MDN: Array.from](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)
- [MDN: Spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)
- [JavaScript.info: Destructuring](https://javascript.info/destructuring)
- [JavaScript.info: Sets and Maps](https://javascript.info/data-structures/set-map)

**Remember:** Destructuring, Set, and Map are powerful ES6 features that make code more concise and efficient. Master them to write modern, clean JavaScript! 💪