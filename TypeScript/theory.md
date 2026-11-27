# TypeScript Interview Questions & Answers

---

## 1. TypeScript Basics

### Q1. What is TypeScript?
**Answer:** TypeScript is a strongly typed, object-oriented programming language developed by Microsoft that builds on JavaScript. It adds static type definitions and compiles (transpiles) down to plain, standard JavaScript that any browser or Node.js runtime can execute.

---

### Q2. Why do we use TypeScript?
**Answer:** We use TypeScript because:
- **Compile-Time Error Detection**: Catches type errors and bugs during development rather than at runtime in production.
- **Enhanced IDE Support**: Provides rich autocompletion, IntelliSense, symbol navigation, and intelligent refactoring.
- **Better Maintainability**: Makes large-scale codebases easier to understand, document, and maintain across teams.
- **Modern JavaScript Features**: Supports latest ECMAScript features and polyfills them for older targets.

---

### Q3. What is the difference between JavaScript and TypeScript?
**Answer:**
| Feature | JavaScript | TypeScript |
| :--- | :--- | :--- |
| **Typing** | Dynamically typed (checked at runtime) | Statically typed (checked at compile time) |
| **Execution** | Runs directly in browsers & Node.js | Must be compiled/transpiled to JS first |
| **Tooling & Refactoring**| Basic autocomplete | Rich IntelliSense, code navigation & refactoring |
| **Advanced Features** | Prototypes, basic ES modules | Interfaces, Generics, Enums, Tuples, Utility Types |

---

### Q4. Is TypeScript executed directly by the browser?
**Answer:** No. Browsers can only interpret and execute JavaScript. TypeScript files (`.ts` or `.tsx`) must be compiled into JavaScript (`.js`) using the TypeScript Compiler (`tsc`) or bundlers like Babel, esbuild, SWC, or Vite before running.

---

### Q5. What is Type Inference?
**Answer:** Type inference is TypeScript's ability to automatically deduce and assign a type to a variable based on its initial value, without requiring explicit type annotations:
```typescript
let age = 25; // TypeScript automatically infers the type as 'number'
// age = "twenty-five"; // Error: Type 'string' is not assignable to type 'number'
```

---

### Q6. What are the basic data types in TypeScript?
**Answer:**
- **Primitive Types**: `string`, `number`, `boolean`, `null`, `undefined`, `symbol`, `bigint`
- **Complex Types**: `object`, `array` (e.g., `number[]`), `tuple` (e.g., `[string, number]`), `enum`
- **Special Types**: `any`, `unknown`, `void`, `never`

---

### Q7. What is Type Annotation?
**Answer:** Type annotation is explicitly specifying the type of a variable, function parameter, or return value using a colon (`:`):
```typescript
let name: string = "Nikhil";
let count: number = 10;
```

---

### Q8. What is the difference between `any` and `unknown`?
**Answer:**
- **`any`**: Completely disables all type checking. You can call any method, access any property, or reassign it to anything without compiler errors. It bypasses type safety and should generally be avoided.
- **`unknown`**: Represents any value type-safely. You cannot perform operations, access properties, or call methods on an `unknown` variable until you explicitly verify/narrow its type using type checks or type assertions.
```typescript
let a: any = "hello";
a.toUpperCase(); // OK

let b: unknown = "hello";
// b.toUpperCase(); // Error: Object is of type 'unknown'
if (typeof b === "string") {
  console.log(b.toUpperCase()); // OK after narrowing
}
```

---

### Q9. What is the `never` type?
**Answer:** The `never` type represents the type of values that **never occur**. It is used for:
1. Functions that throw an error and never return normally.
2. Functions with infinite loops that never terminate.
3. Exhaustive type checking in switch/conditional branches.
```typescript
function throwError(message: string): never {
  throw new Error(message);
}
```

---

### Q10. What is the `void` type?
**Answer:** `void` indicates the absence of any value. It is commonly used as the return type of functions that do not return a value (or implicitly return `undefined`):
```typescript
function logMessage(msg: string): void {
  console.log(msg);
}
```

---

## 2. Interface and Type

### Q11. What is an Interface in TypeScript?
**Answer:** An `interface` defines a contract or blueprint for an object's shape, declaring its property names, types, and method signatures:
```typescript
interface User {
  id: number;
  name: string;
  email?: string; // Optional property
}
```

---

### Q12. What is a Type Alias?
**Answer:** A `type` alias creates a custom name for any type, including primitives, object shapes, unions, intersections, and tuple structures:
```typescript
type UserID = string | number;

type User = {
  id: UserID;
  name: string;
};
```

---

### Q13. What is the difference between `interface` and `type`?
**Answer:**
| Feature | `interface` | `type` Alias |
| :--- | :--- | :--- |
| **Declaration Merging** | Supported (multiple interfaces with same name merge automatically) | Not supported (duplicate identifiers throw an error) |
| **Extending / Combining**| Uses `extends` keyword | Uses intersection operator `&` |
| **Primitives & Unions** | Cannot name a primitive or union directly | Can alias primitives, unions (`A \| B`), tuples |
| **Implementation** | Classes can `implement` interfaces | Classes can `implement` object-shaped types |

---

### Q14. What is Interface Inheritance?
**Answer:** Interface inheritance allows one interface to inherit properties and methods from another interface using the `extends` keyword, promoting code reusability:
```typescript
interface Person {
  name: string;
}

interface Employee extends Person {
  salary: number;
}
```

---

### Q15. Can an interface extend multiple interfaces?
**Answer:** Yes. An interface can inherit from multiple interfaces separated by commas:
```typescript
interface A { name: string; }
interface B { age: number; }

interface C extends A, B {
  role: string;
}
```

---

## 3. Functions and Types

### Q16. How do you define a function in TypeScript?
**Answer:** You define explicit types for parameters and the return value:
```typescript
function add(a: number, b: number): number {
  return a + b;
}

// Arrow function syntax:
const multiply = (x: number, y: number): number => x * y;
```

---

### Q17. What is an Optional Parameter?
**Answer:** An optional parameter is a parameter that is not required to be passed when invoking the function. It is denoted with a question mark (`?`) and defaults to `undefined`. Optional parameters must always appear after required parameters:
```typescript
function greet(name: string, greeting?: string): string {
  return `${greeting || "Hello"}, ${name}`;
}
```

---

### Q18. What is a Default Parameter?
**Answer:** A default parameter provides an initial fallback value if no argument or `undefined` is passed:
```typescript
function greet(name: string = "Guest"): string {
  return `Hello, ${name}`;
}
```

---

### Q19. What is a Union Type?
**Answer:** A union type allows a variable or parameter to hold values of two or more specified types, combined with the vertical bar (`|`):
```typescript
let id: string | number;
id = 101;     // OK
id = "ABC12"; // OK
```

---

### Q20. What is an Intersection Type?
**Answer:** An intersection type combines multiple existing types into a single type using the ampersand (`&`). The resulting type must satisfy all combined types simultaneously:
```typescript
type Timestamps = { createdAt: Date; updatedAt: Date };
type User = { id: number; name: string };

type DatabaseUser = User & Timestamps;
```

---

### Q21. What is a Tuple?
**Answer:** A tuple is a typed array with a fixed length and predefined types for each positional index:
```typescript
let user: [number, string] = [1, "Nikhil"];
// user = ["Nikhil", 1]; // Error: Type order must match exactly
```

---

### Q22. What is an Enum?
**Answer:** An enum (enumeration) defines a collection of named numeric or string constants:
```typescript
enum Status {
  Active = "ACTIVE",
  Inactive = "INACTIVE",
  Pending = "PENDING"
}

let currentStatus: Status = Status.Active;
```

---

## 4. Advanced TypeScript

### Q23. What are Generics in TypeScript?
**Answer:** Generics allow writing flexible, reusable functions, classes, and interfaces that work with different data types while preserving full compile-time type safety:
```typescript
function identity<T>(value: T): T {
  return value;
}

const num = identity<number>(42);
const str = identity<string>("TypeScript");
```

---

### Q24. What is Type Assertion?
**Answer:** Type assertion is a mechanism that tells the compiler: "Trust me, I know the exact type of this value better than you do." It does not perform runtime casting or validation:
```typescript
let value: unknown = "Hello World";
let strLength: number = (value as string).length;
```

---

### Q25. What is Type Narrowing?
**Answer:** Type narrowing is the process of refining a broader type (like a union) into a more specific, concrete type within a conditional code block using:
- `typeof` checks
- `instanceof` checks
- Equality checks (`===`)
- Property existence checks (`"property" in object`)
- Custom type guards (`param is Type`)

---

### Q26. What is Optional Chaining (`?.`)?
**Answer:** Optional chaining (`?.`) allows safely accessing deeply nested object properties or methods without throwing runtime errors (`TypeError: Cannot read property of undefined/null`):
```typescript
const city = user?.address?.city;
```

---

### Q27. What is Nullish Coalescing (`??`)?
**Answer:** The nullish coalescing operator (`??`) returns the right-hand operand only when the left-hand operand is `null` or `undefined`. Unlike the logical OR (`||`) operator, it treats falsy values like `0`, `""`, and `false` as valid values:
```typescript
const count = 0 ?? 10; // Result: 0
const count2 = 0 || 10; // Result: 10
```

---

### Q28. What is the difference between `?` and `!` in TypeScript?
**Answer:**
- **`?` (Optional Operator)**: Marks a property, parameter, or argument as optional, meaning it can be `undefined`.
- **`!` (Non-Null Assertion Operator)**: Postfix operator that asserts to the compiler that an expression is strictly neither `null` nor `undefined`, removing them from the type check.

---

### Q29. What is the `keyof` operator?
**Answer:** The `keyof` operator produces a string or numeric literal union of all known public property keys of an object type:
```typescript
type User = { id: number; name: string };
type UserKeys = keyof User; // "id" | "name"
```

---

### Q30. What is the `typeof` operator in TypeScript?
**Answer:** In a type context, `typeof` captures and extracts the type of an existing variable, constant, or object:
```typescript
const user = { name: "Nikhil", age: 25 };
type UserType = typeof user; // { name: string; age: number; }
```

---

### Q31. What are Utility Types? Name the most important ones.
**Answer:** Utility types are built-in generic types that facilitate common type transformations:
- **`Partial<T>`**: Makes all properties in `T` optional.
- **`Required<T>`**: Makes all properties in `T` required.
- **`Readonly<T>`**: Makes all properties in `T` read-only.
- **`Pick<T, K>`**: Constructs a type by picking specific keys `K` from `T`.
- **`Omit<T, K>`**: Constructs a type by omitting specific keys `K` from `T`.
- **`Record<K, T>`**: Constructs an object type with property keys `K` and value types `T`.

---

### Q32. What is a `readonly` property?
**Answer:** The `readonly` modifier prevents a property from being reassigned or modified after object initialization:
```typescript
interface User {
  readonly id: number;
  name: string;
}

const user: User = { id: 101, name: "Nikhil" };
// user.id = 102; // Error: Cannot assign to 'id' because it is a read-only property
```

---

### Q33. What is a Type Guard?
**Answer:** A type guard is a condition or custom function with a type predicate (`param is Type`) that verifies a variable's runtime type and instructs TypeScript to narrow its type in that scope:
```typescript
function isString(val: unknown): val is string {
  return typeof val === "string";
}

function processValue(val: unknown) {
  if (isString(val)) {
    console.log(val.toUpperCase()); // val is narrowed to string
  }
}
```

---

## 5. TypeScript with Node.js and Backend

### Q34. How do you use TypeScript with Node.js?
**Answer:**
1. Initialize the project: `npm init -y && npm install -D typescript @types/node`.
2. Generate config: `npx tsc --init`.
3. Write server logic in `.ts` files inside `src/`.
4. Compile code: `npx tsc` (outputs compiled `.js` files to `dist/`).
5. Run the compiled application: `node dist/index.js`.
*(In development, tools like `tsx` or `ts-node-dev` are used for instant execution without manual build steps).*

---

### Q35. What is `tsconfig.json`?
**Answer:** `tsconfig.json` is the root configuration file for a TypeScript project. It specifies compiler options, target JS version (`target`), module system (`module`), output directories (`outDir`), and files to include/exclude.

---

### Q36. What is strict mode (`"strict": true`) in TypeScript?
**Answer:** Enabling `"strict": true` in `tsconfig.json` activates a broad suite of rigorous type-checking behaviors, including:
- `noImplicitAny`: Raises errors on expressions implied as `any`.
- `strictNullChecks`: Ensures `null` and `undefined` are handled explicitly.
- `strictFunctionTypes`: Enforces stronger checking of function parameters.
- `strictPropertyInitialization`: Ensures class properties are initialized in constructors.

---

### Q37. What is the difference between `tsc` and `ts-node`?
**Answer:**
- **`tsc` (TypeScript Compiler)**: Statically type-checks and compiles `.ts` files into `.js` files onto the disk for production deployment.
- **`ts-node`**: An in-memory execution engine that transpiles and executes TypeScript directly on-the-fly, commonly used during local development.

---

### Q38. What is a Module in TypeScript?
**Answer:** Any file containing top-level `import` or `export` statements is treated as an isolated module with its own local scope, preventing variable leakage into the global namespace.

---

### Q39. What is a Namespace?
**Answer:** A `namespace` is TypeScript's legacy way of grouping related code under a single global object name (e.g., `namespace Utils { ... }`). Modern TypeScript applications strongly prefer standard **ES Modules (`import`/`export`)**.

---

### Q40. How do you handle errors in TypeScript?
**Answer:** In TypeScript, caught errors in `catch` blocks have the type `unknown` under strict mode. You should narrow them using `instanceof Error`:
```typescript
try {
  // asynchronous API or DB operation
} catch (error: unknown) {
  if (error instanceof Error) {
    console.error(error.message);
  } else {
    console.error("An unexpected error occurred", error);
  }
}
```

---

### Q41. How do you define a Generic API Response Type?
**Answer:** Using a generic interface allows standardizing API responses across endpoints:
```typescript
interface ApiResponse<T> {
  success: boolean;
  statusCode: number;
  message: string;
  data: T;
}

// Usage:
interface UserProfile {
  id: string;
  username: string;
}

const response: ApiResponse<UserProfile> = {
  success: true,
  statusCode: 200,
  message: "User fetched successfully",
  data: { id: "u123", username: "nikhil" }
};
```

---

### Q42. How do you define a Database Model Type in TypeScript?
**Answer:** Define an interface representing the schema fields, combined with optional primary keys or generated timestamp fields:
```typescript
interface UserAttributes {
  id: number;
  email: string;
  name: string;
  isActive: boolean;
  createdAt: Date;
  updatedAt: Date;
}

// Creation attributes omit auto-generated fields
type UserCreationAttributes = Omit<UserAttributes, "id" | "createdAt" | "updatedAt">;
```

---

### Q43. Why is TypeScript particularly valuable in large projects?
**Answer:**
1. **Self-Documenting Code**: Interfaces and types act as living documentation.
2. **Confident Refactoring**: Changing a property or method signature triggers compile-time errors at every impacted call site across the entire project.
3. **Collaboration**: Prevents team members from passing incompatible arguments across services and APIs.
4. **Tooling Efficiency**: Rapid navigation through codebases using IDE definitions and reference lookups.
