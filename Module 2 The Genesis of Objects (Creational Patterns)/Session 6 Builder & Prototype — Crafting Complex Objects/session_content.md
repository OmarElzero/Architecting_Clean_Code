# Session 6: Builder & Prototype — Crafting Complex Objects

**Duration:** 4 hours
**Instructor(s):** Omar Betawy, Bassel Ahmed
**Reference:** *Head First Design Patterns* by Eric Freeman & Elisabeth Robson; *Effective Java* by Joshua Bloch.

---

### 🎯 **Session Objectives**

1.  **Identify the problem of the "telescoping constructor" anti-pattern.**
2.  **Implement the Builder pattern to construct complex objects step-by-step.**
3.  **Create a fluent API using the Builder pattern.**
4.  **Explain the use case for the Prototype pattern: creating new objects by cloning.**
5.  **Implement the Prototype pattern and understand the difference between shallow and deep copies.**

---

### **Part 1: The Builder Pattern — Taming the Complex Constructor (90 mins)**

#### **The Problem: The Telescoping Constructor**
"Last session, we used factories to decouple the client from *which* concrete class to create. Today, we address a different problem: what if the creation of a single object is itself a complex process? What if an object has many optional parameters?"

*   **The Anti-Pattern:** Consider a `Pizza` class where the crust, cheese, and toppings are all optional. To handle all combinations, developers often resort to the "telescoping constructor" anti-pattern.

*   **Code Example (Anti-Pattern):**
    ```java
    public class Pizza {
        private String crust;    // required
        private String sauce;    // required
        private String cheese;   // optional
        private String topping1; // optional
        private String topping2; // optional

        public Pizza(String crust, String sauce) {
            this(crust, sauce, "mozzarella", null, null);
        }
        public Pizza(String crust, String sauce, String cheese) {
            this(crust, sauce, cheese, null, null);
        }
        public Pizza(String crust, String sauce, String cheese, String topping1) {
            this(crust, sauce, cheese, topping1, null);
        }
        public Pizza(String crust, String sauce, String cheese, String topping1, String topping2) {
            this.crust = crust;
            // ... etc.
        }
    }
    ```
*   **The Problems:**
    *   **Hard to Read:** `new Pizza("thin", "tomato", "mozzarella", "pepperoni")`. What do all these parameters mean? The client code is unreadable.
    *   **Error-Prone:** It's very easy to mix up the order of the parameters.
    *   **Inflexible:** What if you want a pizza with no cheese but with a topping? You can't do it with this constructor set.

#### **The Solution: The Builder Pattern**
*   **Intent:** Separate the construction of a complex object from its representation, so that the same construction process can create different representations.
*   **Reference:** The Builder pattern is famously championed by Joshua Bloch in his book *Effective Java*. [Item 2: Consider a builder when faced with many constructor parameters](https://www.informit.com/articles/article.aspx?p=1216151&seqNum=2)
*   **The Structure:**
    1.  A static nested `Builder` class inside the object you want to create (e.g., `Pizza.Builder`).
    2.  The `Builder` has methods for setting each optional parameter. These methods return the `Builder` itself, allowing for a **fluent API**.
    3.  The `Builder` has a `build()` method that creates and returns the final `Pizza` object.
    4.  The main `Pizza` class has a private constructor that can only be called by the `Builder`.

*   **Code Example (Using Builder):**
    ```java
    public class Pizza {
        private final String crust;
        private final String sauce;
        private final String cheese;
        private final String topping;

        // Private constructor, only the Builder can call it.
        private Pizza(Builder builder) {
            this.crust = builder.crust;
            this.sauce = builder.sauce;
            this.cheese = builder.cheese;
            this.topping = builder.topping;
        }

        // Static nested Builder class
        public static class Builder {
            // Required parameters
            private final String crust;
            private final String sauce;

            // Optional parameters with default values
            private String cheese = "mozzarella";
            private String topping = null;

            public Builder(String crust, String sauce) {
                this.crust = crust;
                this.sauce = sauce;
            }

            // Methods for optional parameters, returning "this" for fluency
            public Builder cheese(String cheese) {
                this.cheese = cheese;
                return this;
            }
            public Builder topping(String topping) {
                this.topping = topping;
                return this;
            }

            // The final build step
            public Pizza build() {
                return new Pizza(this);
            }
        }
    }
    ```

#### **Using the Builder**
*   "The client code is now incredibly readable and flexible."
    ```java
    // Client code
    Pizza hawaiianPizza = new Pizza.Builder("pan", "tomato")
                                    .cheese("mozzarella")
                                    .topping("ham and pineapple")
                                    .build();

    Pizza basicPizza = new Pizza.Builder("thin", "garlic")
                                 .build(); // Uses default cheese, no topping
    ```

---

### **Part 2: The Prototype Pattern — Cloning for Efficiency (90 mins)**

#### **The Problem: Expensive Object Creation**
"The Builder pattern is great for complex objects, but what if creating an object is not just complex, but also *expensive*? What if it requires a database call, a network request, or a heavy computation?"

"If you need to create many objects that are very similar to each other, creating each one from scratch can be a performance bottleneck."

#### **The Solution: The Prototype Pattern**
*   **Intent:** Specify the kinds of objects to create using a **prototypical instance**, and create new objects by copying this prototype.
*   **The Core Idea:** Instead of building a new object from scratch, you take an existing, fully-formed object and clone it.

#### **The Structure**
1.  A `Prototype` interface that declares a `clone()` method.
2.  Concrete `Prototype` classes that implement the `clone()` method.
3.  A `PrototypeRegistry` or `Cache` that stores pre-built, ready-to-clone prototype objects.

*   **Code Example (Using Prototype):**
    ```java
    // 1. The Prototype interface (in Java, we often use the built-in Cloneable)
    public interface Shape extends Cloneable {
        void draw();
        Shape clone();
    }

    // 2. Concrete Prototypes
    public class Circle implements Shape {
        // ... properties ...
        @Override
        public Shape clone() {
            // ... logic to create a copy of this circle ...
        }
    }
    public class Rectangle implements Shape { // ... similar ... }

    // 3. The Registry
    public class ShapeCache {
        private static Map<String, Shape> shapeMap = new HashMap<>();

        public static void loadCache() {
            // Create and store the initial, expensive-to-create prototypes
            Circle circle = new Circle();
            shapeMap.put("circle", circle);

            Rectangle rectangle = new Rectangle();
            shapeMap.put("rectangle", rectangle);
        }

        public static Shape getShape(String shapeId) {
            Shape cachedShape = shapeMap.get(shapeId);
            return cachedShape.clone(); // We return a clone, not the original!
        }
    }
    ```

#### **Shallow vs. Deep Copy**
*   "The most critical part of the Prototype pattern is the `clone()` method. You must decide if you are doing a shallow or a deep copy."
*   **Shallow Copy:** Copies the object's fields directly. If a field is a reference to another object (e.g., an `Address` object inside a `User` object), only the *reference* is copied, not the `Address` object itself. Both the original and the clone will now point to the **same** `Address` object. A change in one will affect the other.
*   **Deep Copy:** Copies not only the object's fields, but also recursively clones any objects that are referenced. This ensures the clone is completely independent of the original.
*   **Discussion:** When would you want a shallow copy? When would a deep copy be necessary? (Shallow is faster; deep is safer).

---

### **Part 3: Project Briefing (30 mins)**

*   **Introduction to the Project:** The Query Builder API.
*   **The Goal:** You will use the **Builder** pattern to create a fluent, readable API for constructing complex database queries. This is a very common and powerful application of the pattern. You will then touch on the **Prototype** pattern in a conceptual way.
*   **Connection to Real World:** Almost every modern database access library and Object-Relational Mapper (ORM) — like jOOQ (Java), LINQ (C#), and SQLAlchemy (Python) — uses the Builder pattern extensively to create a fluent interface for writing queries in code.
*   **Q&A.**
