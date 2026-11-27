# Node.js Interview Questions & Answers

---

## 1. Node.js Basics

### Q1. What is Node.js?
**Answer:** Node.js is an open-source JavaScript runtime environment that allows us to execute JavaScript outside the browser. It is used to build backend applications and APIs.

---

### Q2. Is Node.js single-threaded or multithreaded?
**Answer:** Node.js executes JavaScript on a single main thread, but it uses background threads through `libuv` for certain operations like file system and cryptographic tasks.

---

### Q3. What is V8 Engine?
**Answer:** V8 is Google's open-source JavaScript engine. It compiles and executes JavaScript code into native machine code. Node.js is built on top of the V8 engine.

---

### Q4. What is npm?
**Answer:** NPM stands for **Node Package Manager**. It is the default package manager used to install, manage, and share packages/dependencies in Node.js applications.

---

### Q5. What is package.json?
**Answer:** `package.json` is a configuration manifest file that contains metadata about the project, including its dependencies, scripts, version details, and project configurations.

---

### Q6. What is package-lock.json?
**Answer:** `package-lock.json` stores the exact dependency versions and the full dependency tree that was installed, ensuring consistent and reproducible installations across different environments.

---

### Q7. What is the difference between Node.js and JavaScript?
**Answer:**
- **JavaScript**: A high-level scripting programming language.
- **Node.js**: A runtime environment that allows JavaScript code to run outside web browsers on the server side.

---

## 2. Event Loop & Asynchronous Programming

### Q8. What is the Event Loop in Node.js?
**Answer:** The Event Loop handles asynchronous operations and executes their callbacks when they are ready. It allows Node.js to perform non-blocking I/O operations despite JavaScript being single-threaded.

---

### Q9. What is synchronous programming?
**Answer:** Synchronous programming executes code sequentially, one operation at a time. Each operation blocks and waits for the previous operation to finish before continuing.

---

### Q10. What is asynchronous programming?
**Answer:** Asynchronous programming allows long-running operations (like I/O, database queries, network requests) to run in the background without blocking the execution of subsequent code.

---

### Q11. What is a callback function?
**Answer:** A callback is a function passed as an argument to another function, which is then invoked or executed inside the outer function to complete some routine or action.

---

### Q12. What is Callback Hell?
**Answer:** Callback Hell occurs when multiple nested callbacks make code deeply indented, pyramid-shaped, difficult to read, and hard to maintain. We can avoid it using **Promises** and **Async/Await**.

---

### Q13. What is a Promise?
**Answer:** A Promise is an object representing the eventual completion (fulfillment) or failure (rejection) of an asynchronous operation and its resulting value.

---

### Q14. What is Async/Await?
**Answer:** `async/await` is syntactic sugar built on top of Promises. It allows writing asynchronous code in a clean, synchronous-looking manner, making it easier to read and maintain with standard `try/catch` error handling.

---

### Q15. What are the states of a Promise?
**Answer:**
1. **Pending**: Initial state, neither fulfilled nor rejected.
2. **Fulfilled**: The operation completed successfully.
3. **Rejected**: The operation failed with an error.

---

### Q16. What is the difference between Promise.all() and Promise.allSettled()?
**Answer:**
- **`Promise.all()`**: Resolves when all promises resolve; rejects immediately as soon as any one input promise rejects (fail-fast).
- **`Promise.allSettled()`**: Waits until all input promises have completed (settled), whether they are fulfilled or rejected, returning an array of objects describing the outcome of each.

---

### Q17. What is the difference between setTimeout() and setImmediate()?
**Answer:**
- **`setTimeout()`**: Executes a callback after a specified minimum threshold of time (delay in ms) has passed (Timers phase).
- **`setImmediate()`**: Executes a callback immediately after I/O events in the **Check phase** of the event loop.

---

### Q18. What is process.nextTick()?
**Answer:** `process.nextTick()` schedules a callback to execute immediately after the current operation finishes, before the Event Loop proceeds to its next phase (microtask priority).

---

## 3. Node.js Modules

### Q19. What are modules in Node.js?
**Answer:** Modules are discrete, reusable blocks of code that encapsulate related functionality and help organize an application into maintainable separate files.

---

### Q20. What is the difference between require and import?
**Answer:**
- **`require()`**: CommonJS module syntax, loaded synchronously at runtime.
- **`import`**: ES Modules (ESM) syntax, loaded statically and resolved asynchronously at compile/parse time.
- Both are mechanisms used to include and load external modules.

---

### Q21. What is the difference between exports and module.exports?
**Answer:**
- **`module.exports`**: The actual object reference that gets exported by the CommonJS module system.
- **`exports`**: A shorthand helper variable initially pointing to `module.exports`. Reassigning `exports = ...` breaks the reference to `module.exports`.

---

### Q22. What is the difference between built-in and third-party modules?
**Answer:**
- **Built-in modules**: Bundled natively with the Node.js core installation (e.g., `fs`, `path`, `http`, `crypto`).
- **Third-party modules**: External packages created by the community and installed via npm/yarn (e.g., `fastify`, `express`, `sequelize`, `mongoose`).

---

## 4. Node.js Core Concepts

### Q23. What is the Buffer in Node.js?
**Answer:** `Buffer` is a global class used to handle raw binary data directly in memory, commonly used when dealing with files, images, cryptography, and network TCP streams.

---

### Q24. What is a Stream in Node.js?
**Answer:** Streams are objects used to read or write data sequentially in small chunks instead of loading the entire dataset into memory all at once, optimizing memory usage and performance.

---

### Q25. What are the types of Streams?
**Answer:**
1. **Readable**: Stream from which data can be read (e.g., `fs.createReadStream()`).
2. **Writable**: Stream to which data can be written (e.g., `fs.createWriteStream()`).
3. **Duplex**: Stream that is both Readable and Writable (e.g., `net.Socket`).
4. **Transform**: Duplex stream where the output is computed based on the input (e.g., `zlib.createGzip()`).

---

### Q26. What is EventEmitter?
**Answer:** `EventEmitter` is a core Node.js class from the `events` module used to handle events. It allows objects to emit named events and register listener callbacks using methods like `.on()` and `.emit()`.

---

### Q27. What is the fs module?
**Answer:** The `fs` (File System) module provides an API for interacting with the file system on your computer, enabling file operations like reading, writing, renaming, appending, and deleting files.

---

### Q28. What is the difference between readFile and readFileSync?
**Answer:**
- **`readFile()`**: Executes asynchronously without blocking the main event loop, calling a callback or returning a promise upon completion.
- **`readFileSync()`**: Executes synchronously, halting/blocking all JavaScript execution until the entire file read operation finishes.

---

### Q29. What is libuv in Node.js?
**Answer:** `libuv` is a multi-platform C library that provides Node.js with support for asynchronous I/O based on an event loop, non-blocking network/file system operations, and a background thread pool (default 4 threads).

---

### Q30. What is process in Node.js?
**Answer:** The `process` object is a global object providing information and control over the current running Node.js process (e.g., `process.env`, `process.argv`, `process.pid`, `process.exit()`).

---

## 5. Error Handling & Security

### Q31. How do you handle errors in Node.js?
**Answer:**
- Using `try...catch` blocks inside synchronous code and `async` functions.
- Listening to `'error'` events on EventEmitters and Streams.
- Implementing centralized error-handling middleware in web frameworks.
- Catching uncaught exceptions via `process.on('uncaughtException')` and unhandled rejections via `process.on('unhandledRejection')`.

---

### Q32. What is the difference between throw and return?
**Answer:**
- **`throw`**: Interrupts the normal execution flow and raises an exception/error that must be caught by a caller.
- **`return`**: Normal termination of a function that passes a specified value back to the caller.

---

### Q33. What is JWT?
**Answer:** **JWT** (JSON Web Token) is a compact, URL-safe open standard (RFC 7519) used to securely transmit data/claims between parties as a JSON object, heavily utilized for stateless user authentication and authorization.

---

### Q34. What is authentication and authorization?
**Answer:**
- **Authentication (AuthN)**: Verifies the identity of a user (e.g., logging in with email and password).
- **Authorization (AuthZ)**: Determines what resources or actions the authenticated user has permission to access.

---

### Q35. How do you secure Node.js APIs?
**Answer:**
1. Implement secure authentication (e.g., JWT, OAuth).
2. Validate and sanitize all incoming request data (against SQL/NoSQL injection, XSS).
3. Enforce HTTPS/TLS encryption.
4. Apply Role-Based Access Control (RBAC).
5. Hash passwords securely using modern algorithms like `bcrypt` or `Argon2`.
6. Implement rate limiting and DDoS protection.
7. Use security headers (e.g., `helmet`).

---

### Q36. What is CORS?
**Answer:** **CORS** (Cross-Origin Resource Sharing) is a browser security mechanism that uses HTTP headers to allow or restrict resources requested from a domain outside the origin domain.

---

## 6. Performance & Architecture

### Q37. What is clustering in Node.js?
**Answer:** Clustering allows spawning multiple Node.js worker child processes across CPU cores that share the same server port, effectively utilizing multi-core systems to handle higher concurrent traffic.

---

### Q38. What is the difference between cluster and worker_threads?
**Answer:**
- **Cluster**: Creates separate operating system processes with separate memory spaces and V8 instances.
- **Worker Threads**: Creates lightweight threads sharing the same process memory space, ideal for CPU-intensive computational tasks in JavaScript.

---

### Q39. How do you improve Node.js application performance?
**Answer:**
1. Use non-blocking asynchronous operations.
2. Optimize database queries and apply proper indexing.
3. Cache frequently accessed data using Redis or Memcached.
4. Implement API pagination for large datasets.
5. Avoid blocking the Event Loop with heavy computations.
6. Use Gzip/Brotli compression and reverse proxies (e.g., NGINX).

---

### Q40. What is memory leak in Node.js?
**Answer:** A memory leak occurs when allocated memory is no longer required by the application but cannot be collected by the Garbage Collector because references to it are still retained (e.g., lingering global variables, detached event listeners, uncleared timers).

---

### Q41. What is REST API?
**Answer:** REST (Representational State Transfer) is an architectural style for networked applications that utilizes standard stateless HTTP methods (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`) to perform operations on resources identified by URIs.

---

### Q42. What is the difference between PUT and PATCH?
**Answer:**
- **`PUT`**: Idempotent method intended to replace the entire target resource with the uploaded representation.
- **`PATCH`**: Applies partial modifications/updates to specific fields of an existing resource.

---

### Q43. What is environment variable in Node.js?
**Answer:** Environment variables are external variables configured outside the code that hold sensitive or environment-specific data (e.g., database credentials, port numbers, API secrets), accessed through `process.env`.

---

### Q44. What is dotenv?
**Answer:** `dotenv` is a zero-dependency npm module that loads environment variables from a local `.env` file into `process.env` during application startup.

---

### Q45. What is Microservices Architecture?
**Answer:** Microservices architecture is a design pattern where an application is divided into a collection of small, independently deployable, loosely coupled services, each dedicated to a specific business capability and communicating over lightweight protocols (e.g., HTTP/REST, gRPC, message queues).
