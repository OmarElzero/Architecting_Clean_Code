# Session 3: LSP & ISP — The Contracts of Code

**Duration:** 4 hours  
**Instructor(s):** Omar Betawy, Bassel Ahmed

---

### 🎯 **Session Objectives**

1.  **Define "substitutability" in an object-oriented context.**
2.  **Identify and fix violations of the Liskov Substitution Principle (LSP).**
3.  **Explain how LSP ensures the integrity of your abstractions.**
4.  **Recognize "fat" interfaces and the problems they cause.**
5.  **Refactor fat interfaces into smaller, role-based interfaces using the Interface Segregation Principle (ISP).**

---

### **Part 1: The Liskov Substitution Principle (90 mins)**

#### **The Foundation of Correct Abstractions**
"Welcome back. In our last session, we created abstractions to make our code extensible. Today, we learn how to make sure those abstractions are *correct*. LSP, named after Barbara Liskov, is the principle that governs the correctness of inheritance and polymorphism."

#### **The Definition**
*   **Formal Definition:** "Let Φ(x) be a property provable about objects x of type T. Then Φ(y) should be true for objects y of type S where S is a subtype of T."
*   **Simple Translation:** Subtypes must be **substitutable** for their base types without altering the correctness of the program.
*   **The Core Idea:** A subclass should behave in all the ways that its parent class is expected to behave. It should not break the implicit "contract" of the parent class.

#### **Case Study: The Infamous Rectangle/Square Problem**
*   "This is the most famous, and clearest, example of an LSP violation. It demonstrates how a relationship that makes sense in the real world (`a square is a rectangle`) can be a dangerous trap in code."

*   **The Flawed Logic:**
    ```java
    // The "contract" of this class implies that width and height can be changed independently.
    public class Rectangle {
        protected int width;
        protected int height;

        public void setWidth(int width) { this.width = width; }
        public void setHeight(int height) { this.height = height; }
        public int getArea() { return this.width * this.height; }
    }

    // The Square subclass breaks this contract.
    public class Square extends Rectangle {
        @Override
        public void setWidth(int width) {
            this.width = width;
            this.height = width; // Side effect: changes height
        }

        @Override
        public void setHeight(int height) {
            this.width = height; // Side effect: changes width
            this.height = height;
        }
    }
    ```

*   **Why This Breaks Everything:**
    *   Consider a piece of client code that operates on a `Rectangle`. It has a reasonable expectation or assertion.
    ```java
    public void clientCode(Rectangle r) {
        r.setWidth(5);
        r.setHeight(4);
        // Any reasonable programmer would assert that the area is now 20.
        assert(r.getArea() == 20);
    }
    ```
    *   If we call `clientCode(new Rectangle())`, the assertion passes.
    *   If we call `clientCode(new Square())`, the assertion **fails**. The `setHeight(4)` call also changed the width to 4, making the area 16.
    *   **Conclusion:** The `Square` is **not substitutable** for the `Rectangle`. The client code, which should not need to know about subtypes, breaks. This is a classic LSP violation.

*   **The Fix:** Don't use inheritance. Model the world as it is in your code, not as it is in language. `Square` and `Rectangle` are distinct entities that might share a common, more abstract interface like `Shape`.

---

### **Part 2: The Interface Segregation Principle (90 mins)**

#### **The Problem with "Fat" Interfaces**
"ISP is about keeping your interfaces lean and focused. It's the SRP for interfaces."

*   **The Definition:** Clients should not be forced to depend on methods they do not use.
*   **The "Fat" Interface Problem:** A large interface with many methods forces implementing classes to provide implementations for methods they don't need or can't support. This leads to bloated classes, confusing APIs, and often, throwing `UnsupportedOperationException`.

#### **Case Study: The "Smart" Printer**
*   "Let's design an interface for a multi-function office device."

*   **Initial Design (Violates ISP):**
    ```java
    // This is a "fat" interface. It has multiple responsibilities.
    public interface MultiFunctionDevice {
        void print(Document d);
        void scan(Document d);
        void fax(Document d);
        void staple(Document d);
    }
    ```

*   **The Problem:**
    ```java
    public class SimplePrinter implements MultiFunctionDevice {
        public void print(Document d) {
            // This is fine.
        }
        public void scan(Document d) {
            // This class can't scan. What do we do?
            throw new UnsupportedOperationException("I am just a simple printer.");
        }
        public void fax(Document d) {
            // This class can't fax.
            throw new UnsupportedOperationException("I am just a simple printer.");
        }
        public void staple(Document d) {
            // This class can't staple.
            throw new UnsupportedOperationException("I am just a simple printer.");
        }
    }
    ```
    *   The client is forced to implement methods it doesn't support. This is a fragile design. What if a client calls `scan` on a `SimplePrinter`? The program crashes at runtime.

#### **Refactoring to Adhere to ISP**
*   "The solution is to break down the fat interface into smaller, more cohesive, role-based interfaces."

*   **Refactored Code (Adheres to ISP):**
    ```java
    // Each interface has a single, clear responsibility.
    public interface IPrinter { void print(Document d); }
    public interface IScanner { void scan(Document d); }
    public interface IFaxer { void fax(Document d); }
    public interface IStapler { void staple(Document d); }

    // Now, classes can implement only the capabilities they actually have.
    public class SimplePrinter implements IPrinter {
        public void print(Document d) { /* ... */ }
    }

    public class Photocopier implements IPrinter, IScanner {
        public void print(Document d) { /* ... */ }
        public void scan(Document d) { /* ... */ }
    }

    public class AllInOneMachine implements IPrinter, IScanner, IFaxer, IStapler {
        // ... implements all methods
    }
    ```
*   **Key Takeaway:** The design is now flexible and robust. A client that only needs to print can depend *only* on the `IPrinter` interface. This prevents accidental calls to unsupported methods and makes the system's capabilities much clearer.

---

### **Part 3: Project Briefing (30 mins)**

*   **Introduction to the Project:** The Universal Payment Gateway.
*   **The Goal:** This project will challenge you to design a set of abstractions that are both correct (LSP) and lean (ISP). You will model a system that processes payments from multiple, varied sources.
*   **Connection to Real World:** This is a highly realistic scenario. Companies like Stripe, PayPal, and Adyen build their entire businesses on providing clean, reliable abstractions for a messy world of different payment methods.
*   **Q&A.**
