# Session 5: The Factories — Simple Factory, Factory Method, Abstract Factory

**Duration:** 4 hours
**Instructor(s):** Omar Betawy, Bassel Ahmed
**Reference:** *Head First Design Patterns* by Eric Freeman & Elisabeth Robson, Chapters 3 & 4.

---

### 🎯 **Session Objectives**

1.  **Identify the problem that Creational Patterns solve: decoupling a client from concrete object creation.**
2.  **Implement the Simple Factory idiom.**
3.  **Implement the Factory Method pattern to let subclasses decide which class to instantiate.**
4.  **Implement the Abstract Factory pattern to create families of related objects.**
5.  **Analyze the tradeoffs between these three factory patterns.**

---

### **Part 1: The Problem — Your Code is Glued to Concrete Classes (45 mins)**

#### **Introduction: Why Creational Patterns?**
"Welcome to Module 2. In our last module, we mastered the SOLID principles, which guide the structure of our classes. Now, we move on to **Design Patterns**. These are proven, reusable solutions to common problems. We begin with the **Creational Patterns**, which all deal with one fundamental problem: **object creation**."

"The `new` keyword is the most common feature in object-oriented programming. It's also one of the most dangerous. Every time you write `new SomeConcreteClass()`, you are tying your code *directly* to that concrete implementation. This creates tight coupling and violates the Open/Closed Principle."

#### **Case Study: The Pizza Shop**
*   "Let's imagine we're building software for a pizza shop. The code to create a pizza object might look like this."

*   **Initial Code (Tightly Coupled):**
    ```java
    public class PizzaShop {
        public Pizza orderPizza(String type) {
            Pizza pizza;
            if (type.equals("cheese")) {
                pizza = new CheesePizza();
            } else if (type.equals("pepperoni")) {
                pizza = new PepperoniPizza();
            } else if (type.equals("veggie")) {
                pizza = new VeggiePizza();
            } else {
                return null; // Or throw exception
            }

            pizza.prepare();
            pizza.bake();
            pizza.cut();
            pizza.box();
            return pizza;
        }
    }
    ```
*   **The Problem:** The `PizzaShop` is directly dependent on `CheesePizza`, `PepperoniPizza`, and `VeggiePizza`. If the pizza shop decides to add a `ClamPizza`, we have to go back and **modify** the `PizzaShop` class. This violates OCP. The part of our code that *changes* (the types of pizzas) is mixed in with the part that *stays the same* (the process of ordering a pizza).

---

### **Part 2: Simple Factory — A First Step to Decoupling (60 mins)**

"Our first step is to encapsulate what changes. The creation of pizzas is the part that varies, so we'll move it into its own object."

#### **The Simple Factory Idiom**
*   "A Simple Factory is not technically a full Design Pattern, but it's a very common and useful starting point. It's a class whose sole job is to create objects for a client."

*   **Refactored Code (Using Simple Factory):**
    ```java
    // 1. The factory class that encapsulates creation logic.
    public class SimplePizzaFactory {
        public Pizza createPizza(String type) {
            if (type.equals("cheese")) {
                return new CheesePizza();
            } // ... other types
            return null;
        }
    }

    // 2. The client now uses the factory.
    public class PizzaShop {
        private SimplePizzaFactory factory;

        public PizzaShop(SimplePizzaFactory factory) {
            this.factory = factory;
        }

        public Pizza orderPizza(String type) {
            Pizza pizza = factory.createPizza(type); // The client is now decoupled!

            pizza.prepare();
            // ... etc.
            return pizza;
        }
    }
    ```
*   **The Improvement:** The `PizzaShop` is no longer responsible for knowing how to create all the different pizza types. If we add a new pizza, we only have to modify the `SimplePizzaFactory`. The `PizzaShop` remains unchanged. We have improved cohesion and reduced coupling.

---

### **Part 3: The Factory Method Pattern — Letting Subclasses Decide (75 mins)**

"Simple Factory is good, but what if different pizza shops have different ways of making pizzas? For example, a New York pizza shop uses different ingredients than a Chicago pizza shop."

#### **The Factory Method Pattern**
*   **Intent:** Defines an interface for creating an object, but lets **subclasses** decide which class to instantiate.
*   **The Structure:**
    *   An abstract `Creator` class declares the `factoryMethod()` that returns an object of type `Product`.
    *   The `Creator` also contains the core business logic that operates on the `Product`.
    *   Concrete `Creator` subclasses override the `factoryMethod()` to return a specific `ConcreteProduct`.

*   **Refactored Code (Using Factory Method):**
    ```java
    // The abstract "Creator"
    public abstract class PizzaShop {
        // The core business logic that doesn't change.
        public Pizza orderPizza(String type) {
            Pizza pizza = createPizza(type); // Call the factory method!
            pizza.prepare();
            // ...
            return pizza;
        }

        // The Factory Method: abstract, forces subclasses to implement it.
        protected abstract Pizza createPizza(String type);
    }

    // A concrete "Creator"
    public class NYPizzaShop extends PizzaShop {
        @Override
        protected Pizza createPizza(String type) {
            if (type.equals("cheese")) {
                return new NYStyleCheesePizza(); // New York specific implementation
            } // ...
            return null;
        }
    }

    // Another concrete "Creator"
    public class ChicagoPizzaShop extends PizzaShop {
        @Override
        protected Pizza createPizza(String type) {
            if (type.equals("cheese")) {
                return new ChicagoStyleCheesePizza(); // Chicago specific
            } // ...
            return null;
        }
    }
    ```
*   **The Improvement:** The decision of which *kind* of pizza to make is now deferred to the subclasses. The high-level `PizzaShop` class knows *that* a pizza will be created, but it doesn't know *how*. This is another example of the Dependency Inversion Principle at work.

---

### **Part 4: The Abstract Factory Pattern — Creating Families of Objects (75 mins)**

"Now for the final evolution. What if creating a pizza requires a family of related ingredients? A New York pizza needs thin crust dough, marinara sauce, and mozzarella cheese. A Chicago pizza needs deep dish dough, plum tomato sauce, and parmesan cheese."

#### **The Abstract Factory Pattern**
*   **Intent:** Provides an interface for creating **families of related or dependent objects** without specifying their concrete classes.
*   **The Structure:**
    *   An `AbstractFactory` interface declares methods for creating each product in the family (e.g., `createDough()`, `createSauce()`).
    *   Concrete `Factory` classes implement this interface to create a specific family of products.
    *   The client code depends only on the `AbstractFactory` interface.

*   **Refactored Code (Using Abstract Factory):**
    ```java
    // The Abstract Factory interface
    public interface PizzaIngredientFactory {
        Dough createDough();
        Sauce createSauce();
        Cheese createCheese();
    }

    // A Concrete Factory for the "New York" family of ingredients
    public class NYPizzaIngredientFactory implements PizzaIngredientFactory {
        public Dough createDough() { return new ThinCrustDough(); }
        public Sauce createSauce() { return new MarinaraSauce(); }
        public Cheese createCheese() { return new MozzarellaCheese(); }
    }
     // ... another factory for Chicago ingredients ...

    // The client (Pizza class) now uses the factory to get its ingredients
    public abstract class Pizza {
        protected PizzaIngredientFactory ingredientFactory;
        // ...
        public abstract void prepare(); // This method will use the factory
    }

    public class CheesePizza extends Pizza {
        public CheesePizza(PizzaIngredientFactory factory) {
            this.ingredientFactory = factory;
        }
        public void prepare() {
            // We don't know what kind of dough we're getting,
            // we just know it's the right kind for this pizza.
            Dough dough = ingredientFactory.createDough();
            Sauce sauce = ingredientFactory.createSauce();
        }
    }
    ```
*   **The Improvement:** We have completely decoupled the pizza creation from the creation of the ingredient families. We can now introduce a new family of ingredients (e.g., California-style with pesto sauce and goat cheese) by simply creating a new `PizzaIngredientFactory`. The `Pizza` classes remain unchanged.

---

### **Part 5: Project Briefing (30 mins)**

*   **Introduction to the Project:** The Cross-Platform UI Toolkit.
*   **The Goal:** You will use these factory patterns to build a UI toolkit that can create different UI elements (buttons, checkboxes) for different operating systems (Windows, macOS). This is a classic and highly practical application of these patterns.
*   **Connection to Real World:** This is how many cross-platform frameworks are designed, from Java's AWT/Swing to modern frameworks like React Native. They provide an abstract API for UI elements, and a concrete factory handles creating the native-looking widgets for the specific platform.
*   **Q&A.**
