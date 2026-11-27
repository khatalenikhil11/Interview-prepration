# JavaScript & Node.js Interview Questions & Answers

---

## 1. Node.js Basics

### Q1. What is Node.js?
**Answer:** Node.js is an open-source JavaScript runtime environment that allows us to execute JavaScript outside the browser. It is used to build backend applications and APIs.

---

### Q2. Is Node.js single-threaded or multithreaded?
**Answer:** Node.js executes JavaScript on a single main thread, but it uses background threads through `libuv` for certain operations like file system and cryptographic tasks.

---

### Q3. What is V8 Engine?
**Answer:** V8 is Google's open-source JavaScript engine. It compiles and executes JavaScript code. Node.js uses the V8 engine.

---

### Q4. What is npm?
**Answer:** NPM stands for **Node Package Manager**. It is used to install, manage, and share packages in Node.js applications.

---

### Q5. What is package.json?
**Answer:** `package.json` is a configuration file that contains project information, dependencies, scripts, and version details.

---

### Q6. What is package-lock.json?
**Answer:** `package-lock.json` stores the exact dependency versions and dependency tree to ensure consistent installations across environments.

---

### Q7. What is the difference between Node.js and JavaScript?
**Answer:**
- **JavaScript**: A high-level programming language.
- **Node.js**: A runtime environment that allows JavaScript to run outside the browser on the server side.

---

## 2. Event Loop & Asynchronous Programming

### Q8. What is the Event Loop in Node.js?
**Answer:** The Event Loop handles asynchronous operations and executes their callbacks when they are ready. It allows Node.js to perform non-blocking operations.

---

### Q9. What is synchronous programming?
**Answer:** Synchronous programming executes code one operation at a time. Each operation waits for the previous operation to finish before moving to the next.

---

### Q10. What is asynchronous programming?
**Answer:** Asynchronous programming allows operations to run without blocking the execution of other code.

---

### Q11. What is a callback function?
**Answer:** A callback is a function passed as an argument to another function and executed later.

---

### Q12. What is Callback Hell?
**Answer:** Callback Hell occurs when multiple nested callbacks make code difficult to read and maintain. We can avoid it using **Promises** and **Async/Await**.

---

### Q13. What is a Promise?
**Answer:** A Promise is an object that represents the eventual success or failure of an asynchronous operation and its resulting value.

---

### Q14. What is Async/Await?
**Answer:** Async/Await is a modern way to handle asynchronous operations. It makes Promise-based code easier to read and maintain.

---

### Q15. What are the states of a Promise?
**Answer:**
1. **Pending**: Initial state, neither fulfilled nor rejected.
2. **Fulfilled**: The operation completed successfully.
3. **Rejected**: The operation failed.

---

### Q16. What is the difference between Promise.all() and Promise.allSettled()?
**Answer:**
- **`Promise.all()`**: Rejects as soon as any input Promise rejects.
- **`Promise.allSettled()`**: Waits for all Promises to settle, whether fulfilled or rejected.

---

### Q17. What is the difference between setTimeout() and setImmediate()?
**Answer:**
- **`setTimeout()`**: Executes a callback after a specified minimum delay.
- **`setImmediate()`**: Executes a callback in the check phase of the event loop.

---

### Q18. What is process.nextTick()?
**Answer:** `process.nextTick()` schedules a callback to execute after the current operation, before the Event Loop continues to its next phase.

---

## 3. Node.js Modules

### Q19. What are modules in Node.js?
**Answer:** Modules are reusable blocks of code that help us organize an application into separate files.

---

### Q20. What is the difference between require and import?
**Answer:**
- **`require()`**: Used in CommonJS modules.
- **`import`**: Used in ES Modules (ESM).
- Both are used to load external modules.

---

### Q21. What is the difference between exports and module.exports?
**Answer:** `module.exports` defines the value exported by a CommonJS module, while `exports` is initially a reference to `module.exports`.

---

### Q22. What is the difference between built-in and third-party modules?
**Answer:**
- **Built-in modules**: Come bundled with Node.js, such as `fs`, `path`, and `http`.
- **Third-party modules**: Installed using npm, such as `Fastify` and `Sequelize`.

---

## 4. Node.js Core Concepts

### Q23. What is the Buffer in Node.js?
**Answer:** `Buffer` is used to handle raw binary data directly in memory, such as files, images, and network streams.

---

### Q24. What is a Stream in Node.js?
**Answer:** Streams are used to process data in chunks sequentially instead of loading the entire data into memory at once.

---

### Q25. What are the types of Streams?
**Answer:**
1. **Readable**: Stream from which data can be read.
2. **Writable**: Stream to which data can be written.
3. **Duplex**: Stream that is both Readable and Writable.
4. **Transform**: Duplex stream that can modify or transform data as it is written and read.

---

### Q26. What is EventEmitter?
**Answer:** `EventEmitter` is a Node.js class used to create, emit, and listen to custom events using methods like `.on()` and `.emit()`.

---

### Q27. What is the fs module?
**Answer:** The `fs` module stands for **File System**. It is used to read, write, update, and delete files on the local filesystem.

---

### Q28. What is the difference between readFile and readFileSync?
**Answer:**
- **`readFile()`**: Works asynchronously without blocking the event loop.
- **`readFileSync()`**: Works synchronously and blocks thread execution until completed.

---

### Q29. What is libuv in Node.js?
**Answer:** `libuv` is a multi-platform C library used by Node.js to handle the Event Loop, asynchronous I/O operations, and background thread pool tasks.

---

### Q30. What is process in Node.js?
**Answer:** The `process` object provides information and control over the current Node.js application process, such as environment variables and process ID.

---

## 5. Error Handling & Security

### Q31. How do you handle errors in Node.js?
**Answer:** By using `try-catch` blocks for handling errors in asynchronous functions and implementing centralized error handling middleware to manage application errors.

---

### Q32. What is the difference between throw and return?
**Answer:**
- **`throw`**: Raises an exception and halts execution until handled.
- **`return`**: Exits a function normally and returns a value to the caller.

---

### Q33. What is JWT?
**Answer:** **JWT** stands for **JSON Web Token**. It is used to securely transmit claims/data between two parties and is commonly used for user authentication and authorization.

---

### Q34. What is authentication and authorization?
**Answer:**
- **Authentication**: Verifies who the user is (identity check).
- **Authorization**: Checks what the user is allowed to access (permission check).

---

### Q35. How do you secure Node.js APIs?
**Answer:**
1. Use JWT authentication.
2. Validate and sanitize request data.
3. Use HTTPS.
4. Implement role-based access control (RBAC).
5. Hash passwords using `bcrypt` or `Argon2`.
6. Use rate limiting.

---

### Q36. What is CORS?
**Answer:** **CORS** stands for **Cross-Origin Resource Sharing**. It controls which external origins (domains) are allowed to access our APIs from browsers.

---

## 6. Performance & Architecture

### Q37. What is clustering in Node.js?
**Answer:** Clustering allows us to create multiple Node.js worker processes that can share a server port and utilize multiple CPU cores efficiently.

---

### Q38. What is the difference between cluster and worker_threads?
**Answer:**
- **Cluster**: Creates separate Node.js processes with separate memory spaces.
- **Worker Threads**: Creates separate threads within the same process sharing memory.
- Worker Threads are useful for CPU-intensive JavaScript tasks.

---

### Q39. How do you improve Node.js application performance?
**Answer:**
1. Use asynchronous, non-blocking operations.
2. Optimize database queries and indexing.
3. Implement Redis caching.
4. Use pagination for large datasets.
5. Avoid blocking the Event Loop.
6. Use proper indexing.

---

### Q40. What is memory leak in Node.js?
**Answer:** A memory leak occurs when memory that is no longer needed by the program is not released because references to it are still maintained.

---

### Q41. What is REST API?
**Answer:** REST API is an architectural style that allows applications to communicate using standard HTTP methods such as `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`.

---

### Q42. What is the difference between PUT and PATCH?
**Answer:**
- **`PUT`**: Generally replaces the complete resource.
- **`PATCH`**: Updates specific fields of a resource partially.

---

### Q43. What is environment variable in Node.js?
**Answer:** Environment variables store configuration values such as database credentials, API keys, and application settings. We access them using `process.env`.

---

### Q44. What is dotenv?
**Answer:** `dotenv` is an npm package used to load environment variables from a `.env` file into `process.env`.

---

### Q45. What is Microservices Architecture?
**Answer:** Microservices architecture divides an application into small, independent services. Each service handles specific business functionality and communicates over lightweight protocols.

---

## 7. Practical JavaScript / Node.js Coding Questions

### Q46. Reverse a string.
```javascript
const str = "hello";
console.log(str.split("").reverse().join(""));
// Output: "olleh"
```

---

### Q47. Find the largest number in an array.
```javascript
const arr = [10, 50, 20, 80, 30];
console.log(Math.max(...arr));
// Output: 80
```

---

### Q48. Find duplicate elements in an array.
```javascript
const arr = [1, 2, 3, 2, 4, 1];

const duplicates = [...new Set(
  arr.filter((item, index) => arr.indexOf(item) !== index)
)];

console.log(duplicates);
// Output: [2, 1]
```

---

### Q49. Check palindrome string.
```javascript
const str = "madam";

const result = str === str.split("").reverse().join("");

console.log(result);
// Output: true
```

---

### Q50. Find the second-largest number in an array.
```javascript
const arr = [10, 50, 20, 80, 30];

const unique = [...new Set(arr)];
unique.sort((a, b) => b - a);

console.log(unique[1]);
// Output: 50
```
