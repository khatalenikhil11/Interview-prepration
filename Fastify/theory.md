# Fastify Interview Questions & Answers

---

## 1. Fastify Basics

### Q1. What is Fastify?
**Answer:** Fastify is a fast, highly efficient, and lightweight web framework for Node.js, specifically designed to provide high performance and low overhead when building REST APIs and backend microservices.

---

### Q2. Why do we use Fastify?
**Answer:** We use Fastify because it provides:
- Extremely high throughput and low latency (one of the fastest Node.js frameworks).
- Built-in schema-based validation and serialization (powered by Ajv and fast-json-stringify).
- A powerful, encapsulated plugin architecture.
- Full lifecycle request-response hooks.
- Built-in, high-speed asynchronous logging via Pino.
- TypeScript support out of the box.

---

### Q3. What are the main features of Fastify?
**Answer:**
- **High performance**: Benchmarks consistently show significantly higher requests per second compared to Express.
- **Schema-based validation**: Uses JSON Schema to compile validators and serializers ahead-of-time.
- **Plugin architecture**: Modular, scoped, and reusable components.
- **Hooks**: Lifecycle interception points for requests and responses.
- **Error handling**: Centralized, customizable error handling.
- **Logging support**: Deeply integrated zero-cost logger using Pino.

---

### Q4. What is the difference between Express and Fastify?
**Answer:**
| Feature | Express | Fastify |
| :--- | :--- | :--- |
| **Performance** | Moderate (middleware overhead) | High (optimized routing & Ahead-Of-Time compilation) |
| **Schema Validation** | Requires third-party middleware (e.g., Joi, Yup, express-validator) | Built-in using JSON Schema & Ajv |
| **Architecture** | Middleware-centric pipeline | Scoped Plugin architecture with encapsulation |
| **JSON Serialization** | Standard `JSON.stringify()` | High-speed serialization via `fast-json-stringify` |
| **Logging** | Needs external tools (e.g., Morgan, Winston) | Built-in high-performance logging via Pino |
| **Async/Await** | Needs wrappers in Express 4 for uncaught errors | Native first-class support for `async/await` |

---

### Q5. How do you create a Fastify server?
**Answer:**
```javascript
const fastify = require('fastify')({ logger: true });

fastify.get('/', async (request, reply) => {
  return { message: 'Hello World' };
});

const start = async () => {
  try {
    await fastify.listen({ port: 3000 });
  } catch (err) {
    fastify.log.error(err);
    process.exit(1);
  }
};

start();
```

---

### Q6. What is a route in Fastify?
**Answer:** A route defines an HTTP method and URL endpoint path that processes incoming client requests.
```javascript
fastify.get('/users', async (request, reply) => {
  return { message: 'Users list' };
});
```

---

### Q7. What are `request` and `reply` in Fastify?
**Answer:**
- **`request`**: An object containing all incoming request metadata and payloads, such as `request.body`, `request.params`, `request.query`, `request.headers`, and `request.ip`.
- **`reply`**: An object used to control and send the HTTP response, such as setting status codes (`reply.code(200)`), headers (`reply.header()`), and sending payloads (`reply.send()`).

---

### Q8. What is a Fastify plugin?
**Answer:** A plugin is a modular, self-contained, and reusable function used to add routes, decorators, hooks, or configurations to a Fastify application.

---

### Q9. How do you register a plugin?
**Answer:** We register plugins using the `fastify.register()` method:
```javascript
fastify.register(require('./routes/userRoutes'));
```

---

### Q10. What is encapsulation in Fastify?
**Answer:** Encapsulation means plugins create their own independent execution scope. Any hooks, decorators, or schemas defined inside a registered plugin are only available within that plugin and its children; they do not leak into sibling or parent scopes.

---

## 2. Fastify Hooks

### Q11. What are hooks in Fastify?
**Answer:** Hooks are lifecycle functions that allow you to intercept and interact with requests and responses at specific execution stages during the Fastify request-response lifecycle.

---

### Q12. Name important Fastify lifecycle hooks in order of execution.
**Answer:**
1. **`onRequest`**: Fires immediately when a request arrives, before payload parsing.
2. **`preParsing`**: Executes before parsing the request payload (allows raw stream modification).
3. **`preValidation`**: Executes before incoming data is validated against the JSON schema.
4. **`preHandler`**: Executes after validation and right before the route handler.
5. **`preSerialization`**: Executes right before the response payload is serialized into JSON.
6. **`onError`**: Executes when an error is thrown at any point.
7. **`onSend`**: Executes before sending the finalized response payload to the client.
8. **`onResponse`**: Executes after the response has been sent to the client (for logging/metrics).

---

### Q13. What is the use of `preHandler`?
**Answer:** `preHandler` executes right before the route handler. It is the standard hook used for authentication (e.g., verifying JWT tokens) and authorization/role checks.

---

### Q14. What is the difference between `preValidation` and `preHandler`?
**Answer:**
- **`preValidation`**: Runs **before** Fastify validates the request with Ajv schema validation (useful for payload sanitization or normalizing query parameters).
- **`preHandler`**: Runs **after** schema validation passes and before reaching the route handler (ideal for authentication and business logic guards).

---

### Q15. What is `onRequest`?
**Answer:** `onRequest` runs immediately when a new HTTP request is received by the server, before any body/payload parsing begins. It is typically used for early routing checks, rate-limiting, or setting correlation IDs.

---

### Q16. What is `onResponse`?
**Answer:** `onResponse` is an asynchronous hook executed after the complete response has been dispatched and delivered to the client. It is primarily used for logging request duration, emitting performance metrics, and cleaning up resources.

---

### Q17. How do you add a hook in Fastify?
**Answer:** Using `fastify.addHook()`:
```javascript
fastify.addHook('preHandler', async (request, reply) => {
  console.log('Running hook before route handler');
});
```

---

## 3. Validation and Error Handling

### Q18. What is schema validation in Fastify?
**Answer:** Schema validation checks incoming request components (`body`, `querystring`, `params`, `headers`) against a JSON Schema using Ajv (Another JSON Schema Validator).

---

### Q19. Why do we use schema validation?
**Answer:**
1. **Input Security**: Prevents SQL/NoSQL injection, unexpected fields, and malicious inputs.
2. **Data Consistency**: Ensures strictly typed parameters (numbers, booleans, strings).
3. **Performance**: Compiles JSON validation functions ahead-of-time (AOT) for maximum throughput.
4. **Auto-generated Documentation**: Integrates with OpenAPI / Swagger.

---

### Q20. How do you validate a request body in Fastify?
**Answer:**
```javascript
fastify.post('/users', {
  schema: {
    body: {
      type: 'object',
      required: ['name', 'email'],
      properties: {
        name: { type: 'string' },
        email: { type: 'string', format: 'email' }
      }
    }
  }
}, async (request, reply) => {
  return request.body;
});
```

---

### Q21. What is `setErrorHandler`?
**Answer:** `setErrorHandler()` defines a centralized error-handling function for a given Fastify instance or plugin scope to catch uncaught errors and format custom error responses:
```javascript
fastify.setErrorHandler((error, request, reply) => {
  fastify.log.error(error);
  reply.code(error.statusCode || 500).send({
    success: false,
    message: error.message || 'Internal Server Error'
  });
});
```

---

### Q22. How do you send a custom HTTP status code?
**Answer:** We use `reply.code()` or `reply.status()`:
```javascript
reply.code(201).send({ message: 'User created' });
```

---

### Q23. What is the difference between `reply.send()` and `return`?
**Answer:**
- **`reply.send()`**: Explicitly instructs Fastify to finalize and transmit the response payload.
- **`return`**: In an `async` route handler, returning any value automatically serializes and sends it as the response body without needing `reply.send()`.
- *Note:* Do not mix `return reply.send()` and plain `return` ambiguously in the same branch.

---

## 4. Authentication and Authorization

### Q24. How do you implement authentication in Fastify?
**Answer:** Authentication is commonly implemented using official plugins like `@fastify/jwt` or `@fastify/auth` combined with a `preHandler` hook that extracts and verifies bearer tokens before protected route execution.

---

### Q25. What is JWT?
**Answer:** JWT (JSON Web Token) is a compact, URL-safe open standard (RFC 7519) used to securely transmit claims between two parties. In Fastify, it is used for stateless authentication and authorization.

---

### Q26. What is the difference between authentication and authorization?
**Answer:**
- **Authentication (AuthN)**: Verifies the identity of the user (e.g., validating username/password or JWT signature).
- **Authorization (AuthZ)**: Verifies what permissions and resources the authenticated user has rights to access (e.g., Admin vs User roles).

---

### Q27. What is a decorator in Fastify?
**Answer:** A decorator allows you to customize and extend core Fastify objects (`fastify`, `request`, or `reply`) by attaching reusable utility methods, database connections, or global configurations:
```javascript
// Decorating the fastify instance
fastify.decorate('appName', 'My Fastify App');

// Decorating the request object
fastify.decorateRequest('user', null);
```

---

### Q28. What is `fastify-plugin`?
**Answer:** `fastify-plugin` (often imported as `fp`) is a utility package that **bypasses/disables encapsulation**. It allows decorators, hooks, and schemas defined inside a plugin to be exposed globally across the entire Fastify application and parent scopes.

---

## 5. Database and API Development

### Q29. How do you connect PostgreSQL with Fastify?
**Answer:** You can connect using:
1. Native PostgreSQL driver plugin: `@fastify/postgres` (built on top of `pg`).
2. ORMs / Query Builders: Prisma, Sequelize, Drizzle, or TypeORM registered inside a Fastify plugin using decorators:
```javascript
const fp = require('fastify-plugin');
const { Pool } = require('pg');

async function dbConnector(fastify, options) {
  const pool = new Pool({ connectionString: process.env.DATABASE_URL });
  fastify.decorate('db', pool);
}

module.exports = fp(dbConnector);
```

---

### Q30. How do you organize a Fastify project structure?
**Answer:** A clean, production-ready Fastify architecture is structured into:
- **`routes/`**: Endpoint routing definitions.
- **`controllers/`**: Request handling and response formatting logic.
- **`services/`**: Core business logic and external integrations.
- **`models/`**: Database schemas and entities.
- **`plugins/`**: Encapsulated reusable features (DB connections, authentication, CORS).
- **`schemas/`**: Reusable JSON schemas for request validation and response serialization.
- **`hooks/`**: Global or scoped lifecycle hooks.
- **`config/`**: Environment variable validation and app configurations.

---

### Q31. What is the difference between PUT and PATCH?
**Answer:**
- **`PUT`**: Idempotent HTTP method intended to replace the entire target resource with the new representation.
- **`PATCH`**: Applies partial updates to modify only specific fields of an existing resource.

---

### Q32. How do you handle asynchronous operations in Fastify?
**Answer:** Fastify has native first-class support for `async/await` and Promises. Route handlers and hooks can be defined as `async` functions without needing third-party wrappers:
```javascript
fastify.get('/data', async (request, reply) => {
  const result = await fetchRemoteData();
  return result;
});
```

---

### Q33. How do you handle database errors in Fastify?
**Answer:**
1. Wrap database calls in `try...catch` blocks within services/controllers.
2. Translate raw database error codes (e.g., PostgreSQL `23505` unique violation) into clean HTTP status codes (e.g., `409 Conflict`).
3. Leverage Fastify's centralized `setErrorHandler` to catch unhandled database exceptions and return standard error payloads without leaking database internals.

---

### Q34. How do you improve Fastify API performance?
**Answer:**
1. Define strict `response` JSON schemas (enables `fast-json-stringify` for up to 2x faster JSON serialization).
2. Utilize database connection pooling (`pg.Pool`).
3. Apply Redis caching for expensive or repetitive queries.
4. Keep plugins small, modular, and leverage encapsulation.
5. Avoid blocking the Node.js event loop with CPU-heavy tasks.
6. Enable clustering or multi-instance deployment behind a reverse proxy (e.g., NGINX).

---

### Q35. What is logging in Fastify?
**Answer:** Fastify comes with built-in, low-overhead asynchronous logging powered by **Pino**. It is enabled by passing `{ logger: true }` when creating the Fastify server instance, and loggers are automatically contextualized on requests (`request.log.info(...)`).

---

## 6. Scenario-Based Questions

### Q36. How do you protect an API in Fastify?
**Answer:**
1. **Authentication**: Verify signed JWTs on protected routes via a `preHandler` hook.
2. **Authorization**: Implement Role-Based Access Control (RBAC) to inspect user permissions.
3. **Input Validation**: Use JSON schemas with Ajv to reject malformed or malicious payloads.
4. **Security Plugins**: Use `@fastify/helmet` for security headers, `@fastify/cors` for origin controls, and `@fastify/rate-limit` for DDoS / brute-force protection.
5. **Secrets Management**: Store secrets in environment variables using `@fastify/env`.

---

### Q37. How do you create reusable authentication logic in Fastify?
**Answer:** Create an authentication decorator using `fastify-plugin` and invoke it as a `preHandler` hook on specific routes:
```javascript
// authPlugin.js
const fp = require('fastify-plugin');

module.exports = fp(async (fastify, opts) => {
  fastify.decorate('authenticate', async (request, reply) => {
    try {
      await request.jwtVerify();
    } catch (err) {
      reply.code(401).send({ error: 'Unauthorized access' });
    }
  });
});

// route.js
fastify.get('/profile', { preHandler: [fastify.authenticate] }, async (request, reply) => {
  return request.user;
});
```

---

### Q38. How do you handle duplicate records in Fastify?
**Answer:**
1. Enforce **Unique Constraints** at the database level (e.g., `UNIQUE(email)`).
2. In the service/controller, handle the database unique violation error code (e.g., PostgreSQL `23505` or MySQL `1062`) and return a `409 Conflict` HTTP response.
3. Perform an existence check prior to inserting when custom business rules apply.

---

### Q39. How do you handle large API responses in Fastify?
**Answer:**
1. **Pagination**: Implement cursor-based or limit/offset pagination in queries.
2. **Field Selection**: Return only requested projection fields instead of entire records.
3. **HTTP Streaming**: Use Node.js Streams or `@fastify/compress` (Gzip/Brotli) to stream large datasets in chunks without high memory consumption.
4. **Caching**: Store pre-computed responses in Redis.
