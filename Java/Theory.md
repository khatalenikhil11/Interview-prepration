# Java Interview Questions & Answers

---

### Q1. What is Java?
Java is a high-level, object-oriented programming language. It is platform-independent because Java code runs on the JVM.

---

### Q2. What are the main features of Java?
- **Portable**
- **Multithreaded**
- **Robust**
- **Secure**
- **Platform-independent**
- **Object-oriented**

---

### Q3. What is JVM?
**JVM** stands for **Java Virtual Machine**. It executes Java bytecode.

---

### Q4. What is JDK?
**JDK** stands for **Java Development Kit**. It provides tools to develop, compile, and run Java applications.

---

### Q5. Why is Java platform-independent?
Java is platform-independent because its compiler converts source code into bytecode, which can run on any operating system that has a compatible JVM.

---

### Q6. Difference between JDK, JRE, and JVM?
- **JVM**: Executes Java bytecode.
- **JRE**: Used to run Java applications.
- **JDK**: Used to develop Java applications.

---

### Q7. What is OOP?
**OOP** stands for **Object-Oriented Programming**. It is based on objects and classes.

---

### Q8. What are the four pillars of OOP?
1. **Abstraction**
2. **Polymorphism**
3. **Inheritance**
4. **Encapsulation**

---

### Q9. What is a Class?
A class is a blueprint for creating objects.

---

### Q10. What is an Object?
An object is an instance of a class.

---

### Q11. What is Inheritance?
Inheritance allows one class to acquire the properties and methods of another class.

---

### Q12. What is Polymorphism?
Polymorphism means one thing can have multiple forms.

**Types:**
- **Runtime polymorphism**: Method Overriding
- **Compile-time polymorphism**: Method Overloading

---

### Q13. What is Encapsulation?
Encapsulation means wrapping data and methods into a single unit and controlling access using access modifiers.

---

### Q14. What is Abstraction?
Abstraction means hiding implementation details and showing only essential functionality.

---

### Q15. Difference between C++ and Java?
C++ and Java are both programming languages:
- **Platform Independence**: C++ is platform-dependent, whereas Java is platform-independent.
- **Pointers**: C++ supports direct pointers, while Java does not support direct pointer manipulation.
- **Memory Management**: Java provides automatic memory management using Garbage Collection, while C++ requires manual memory management.

---

### Q16. What is a Constructor?
A constructor is a special method used to initialize an object.

---

### Q17. What is Method Overloading?
Defining multiple methods with the same name but different parameters.

---

### Q18. What is Method Overriding?
When a child class provides its own specific implementation of a method that is already defined in its parent class.

---

### Q19. What is an Interface?
An interface defines a contract that implementing classes must follow.

---

### Q20. Difference between Abstract Class and Interface?
- An abstract class can have constructors, instance variables, abstract methods, and concrete methods.
- An interface defines a contract that classes implement and supports multiple inheritance of type.
- A class can extend only one class but implement multiple interfaces.

---

### Q21. What is Exception Handling?
It is a mechanism to handle runtime errors using `try`, `catch`, `finally`, `throw`, and `throws`.

---

### Q22. Difference between Checked and Unchecked Exceptions?
- **Unchecked Exceptions**: Occur at runtime (e.g., `NullPointerException`, `ArithmeticException`).
- **Checked Exceptions**: Checked at compile time (e.g., `IOException`, `SQLException`).

---

### Q23. What is Finally Block?
The `finally` block generally executes whether an exception occurs or not.

---

### Q24. Difference between throw and throws?
- **throw**: Used to explicitly throw an exception.
- **throws**: Used in a method declaration to specify exceptions that may be thrown by the method.

---

### Q25. Difference between final, finally, and finalize()?
- **final**: A keyword used to prevent variable reassignment, method overriding, or class inheritance.
- **finally**: A block used in exception handling that generally executes whether an exception occurs or not.
- **finalize()**: An old method called by the garbage collector before object reclamation (now deprecated and should not be used).

---

### Q26. What is Collection Framework?
It provides classes and interfaces to store and manage groups of objects.

---

### Q27. Difference between Array and ArrayList?
- **Array**: Has a fixed size and can store both primitive types and objects.
- **ArrayList**: Has a dynamic size and stores objects (not primitive types directly).

---

### Q28. Difference between ArrayList and LinkedList?
- **ArrayList**: Generally faster for random access; uses a dynamic array.
- **LinkedList**: Better for insertions and deletions; uses a doubly linked list.

---

### Q29. Difference between HashMap and HashSet?
- **HashSet**: Stores unique values.
- **HashMap**: Stores key-value pairs.

---

### Q30. Can HashMap contain duplicate keys?
No, `HashMap` does not allow duplicate keys. If you insert a key that already exists, the new value replaces the old value.

---

### Q31. Difference between String, StringBuilder, and StringBuffer?
- **String**: Immutable, meaning its value cannot be changed once created.
- **StringBuilder**: Mutable and not thread-safe (faster for single-threaded operations).
- **StringBuffer**: Mutable and thread-safe because its methods are synchronized.

---

### Q32. Difference between Static and Non-Static Methods?
- **Static method**: Belongs to the class itself and can be called directly without creating an object.
- **Non-static method**: Belongs to an instance (object) and requires an object to be invoked.

---

### Q33. What is Multithreading in Java?
Multithreading is a feature that allows multiple threads to execute concurrently within a single program, improving responsiveness and CPU resource utilization.

---

### Q34. What is Garbage Collection in Java?
Garbage collection is an automatic memory management process in Java that removes objects that are no longer reachable or referenced by the application.

---

### Q35. What is Spring Boot?
Spring Boot is a Java-based open-source framework used to develop backend applications, REST APIs, and microservices quickly and easily with minimal configuration.
