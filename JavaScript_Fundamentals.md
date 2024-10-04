
# JavaScript Fundamentals

### 1. What is JavaScript?
JavaScript is a versatile, high-level programming language that can run both on the client side, within web browsers, and on the server side, typically using environments like Node.js.

---

### 2. Difference between `==` and `===` operator?
- `==` checks for equality of values after performing type conversion. It converts the operands to the same type before making the comparison.
```jsx
5 == '5'  // true
```

- `===` checks for equality of both values and types. It does not perform type conversion and requires both operands to be of the same type and value.
```jsx
5 === '5'  // false 
```

---

### 3. What is an Arrow Function?
Arrow functions offer concise syntax compared to traditional function expressions, particularly for simple functions. They allow implicit return for single-expression bodies and inherit `this` from the enclosing lexical context. However, they lack their own `arguments` object, although you can still access it from the enclosing function.

---

### 4. What is Closure?
A closure is a feature where an inner function has access to outer function’s variables.
```jsx
function outerFunction(outerVariable){
  return function innerFunction(innerVaraible){
    console.log(`${outerVariable}`);
    console.log(`${innerVaraible}`);
  }
}
const newFunction= outerFunction('inside');
newFunction('outside');
```
Another example:
```jsx
function createCounter() {
    let count = 0;
    return function() {
        count += 1;
        return count;
    };
}

const counter = createCounter();

console.log(counter()); // Output: 1
console.log(counter()); // Output: 2
console.log(counter()); // Output: 3
```

---

### 5. What is Hoisting?
Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their containing scope during the compilation phase before the code is executed.

- **Variable hoisting**: Only the declaration is hoisted, not the initialization.
```jsx
console.log(x); // Output: undefined
var x = 5;
console.log(x); // Output: 5
```

- **Function Hoisting**: Function declarations are hoisted entirely, meaning you can call the function before it appears in the code.
```jsx
hoistedFunction(); // Output: "This function is hoisted!"
function hoistedFunction() {
    console.log("This function is hoisted!");
}
```

- Variables declared with `let` and `const` are also hoisted but remain in a "temporal dead zone" from the start of the block until the declaration is encountered.
```jsx
console.log(y); // ReferenceError: Cannot access 'y' before initialization
let y = 10;
console.log(z); // ReferenceError: Cannot access 'z' before initialization
const z = 20;
```

---

### 6. What is the Temporal Dead Zone?
The **Temporal Dead Zone (TDZ)** refers to the period between entering a scope and the actual declaration of a `let` or `const` variable. During this period, the variable cannot be accessed.
```jsx
{
    console.log(a); // ReferenceError: Cannot access 'a' before initialization
    let a = 5;
    console.log(a); // Output: 5
}
```

---

### 7. What is Callback and Callback Hell?
- A callback is a function that is passed as an argument to another function and executed after the completion of some asynchronous operation.
```jsx
function loadUserData(userId, callback) {
    setTimeout(function() {
        const userData = { id: userId, name: 'John Doe' };
        callback(userData);
    }, 1000);
}
```

- Callback Hell occurs when multiple nested callbacks are used, leading to deeply nested, hard-to-read, and maintain code structures.
```jsx
loadUserData(123, function(userData) {
    loadUserPosts(userData.id, function(userPosts) {
        userPosts.forEach(function(post) {
            loadPostComments(post, function(comments) {
                console.log(`Post: ${post}, Comments: ${comments}`);
            });
        });
    });
});
```

---

### 8. What is a Promise?
A promise in JavaScript represents the eventual completion or failure of an asynchronous operation. It has three states: Pending, Fulfilled, and Rejected.
```jsx
function loadUserData(userId) {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            const userData = { id: userId, name: 'John Doe' };
            resolve(userData);
        }, 1000);
    });
}
```

Using Promises to avoid callback hell:
```jsx
loadUserData(123)
    .then(function(userData) {
        return loadUserPosts(userData.id);
    })
    .then(function(userPosts) {
        return Promise.all(userPosts.map(loadPostComments));
    })
    .then(function(allComments) {
        console.log(allComments);
    })
    .catch(function(error) {
        console.error('Error:', error);
    });
```

---

### 9. What is Async / Await?
`async/await` allows handling asynchronous code in a more synchronous-like manner. You define asynchronous functions using the `async` keyword and pause their execution with `await` until promises are settled.
```jsx
async function fetchData() {
    try {
        const userData = await loadUserData(123);
        const userPosts = await loadUserPosts(userData.id);
        const allComments = await Promise.all(userPosts.map(loadPostComments));
        console.log(allComments);
    } catch (error) {
        console.error('Error:', error);
    }
}
fetchData();
```

---

### 10. What is `call`, `apply`, and `bind`?
- `call`: Executes a function with a specific context and individual arguments.
```jsx
const person = {
    fullName: function(city, country) {
        return `${this.firstName} ${this.lastName}, ${city}, ${country}`;
    }
};
const person1 = { firstName: 'John', lastName: 'Doe' };
console.log(person.fullName.call(person1, 'New York', 'USA'));
```

- `apply`: Executes a function with a specific context and an array of arguments.
```jsx
const personInfo = { firstName: 'Alice', lastName: 'Smith' };
console.log(person.fullName.apply(personInfo, ['Paris', 'France']));
```

- `bind`: Creates a new function with a specific context and pre-defined arguments, without invoking it immediately.
```jsx
const boundFullName = person.fullName.bind(personInfo, 'Paris', 'France');
console.log(boundFullName());
```

---

### 11. What is the Spread and Rest Operator?
- **Spread Operator**: Expands elements of an iterable into individual elements.
```jsx
const numbers = [1, 2, 3];
const newArray = [...numbers, 4, 5]; // [1, 2, 3, 4, 5]
```

- **Rest Parameter**: Collects all remaining arguments into a single array variable.
```jsx
function sum(...numbers) {
    return numbers.reduce((acc, num) => acc + num, 0);
}
console.log(sum(1, 2, 3, 4, 5)); // Output: 15
```

---

### 12. What are Higher-Order Functions?
A higher-order function is a function that takes one or more functions as arguments or returns a function.
```jsx
function makeIncrementer(n) {
    return function(x) {
        return x + n;
    };
}
const incrementBy5 = makeIncrementer(5);
console.log(incrementBy5(10)); // Output: 15
```

---

### 13. What is `map`, `filter`, and `reduce`?
- `map`: Creates a new array by applying a function to each element of an existing array.
```jsx
const numbers = [1, 2, 3, 4, 5];
const squares = numbers.map(x => x * x); // [1, 4, 9, 16, 25]
```

- `filter`: Returns a new array with elements that pass a test.

### 14. What is `session storage`, `local storage` , and `cookies` ?

- In `session storage`  data clear when the page or browser tab is closed.
- Example : form data
- In `local storage`  data remains store indefinitely or until it is explicitly deleted.
- Example:  user preferences or settings
- In `cookies`  can be set to either temporary or persistent
- Example authentication token or user session identifiers