# Session 2: SRP & OCP — The Art of Separation and Extension

**Duration:** 4 hours  
**Instructor(s):** Omar Betawy, Bassel Ahmed

---

### 🎯 **Session Objectives**

1.  **Define "responsibility" in the context of a class.**
2.  **Identify and refactor violations of the Single Responsibility Principle (SRP).**
3.  **Explain how SRP leads to higher cohesion.**
4.  **Articulate the purpose of the Open/Closed Principle (OCP).**
5.  **Use abstraction (interfaces/abstract classes) to make code extensible without modification.**

---

### **Part 1: The Single Responsibility Principle (90 mins)**

#### **Revisiting the "S" in SOLID**
"Last week, we saw the strategic goal of SRP. Today, we dive into the tactics. SRP is the most foundational of the SOLID principles, and getting it right is the first step to clean architecture."

#### **The True Meaning of "Responsibility"**
*   **Common Misconception:** "A class should only do one thing." This is too vague. The `main` method in an application does "one thing" (runs the app), but it coordinates many activities.
*   **The Real Definition:** A class should have **one, and only one, reason to change.**
*   **A "Reason to Change" is an Actor:** An actor is a group of users or stakeholders who need changes in the software.
    *   Example: The **Finance Department** is an actor. The **HR Department** is another actor.
*   **The Core Idea:** You should not have a single class that is changed for reasons requested by two different actors.

#### **Case Study: The `Employee` Class Violation**
*   "Let's look at a classic, dangerous example that you will find in many real-world systems."

*   **Initial Code (Violates SRP):**
    ```java
    // A single class handling concerns for three different actors.
    public class Employee {
        public Money calculatePay() {
            // Logic for calculating salary, overtime, taxes.
            // ACTOR: Finance Department
        }

        public String reportHours() {
            // Logic for formatting a report of hours for HR.
            // ACTOR: HR Department
        }

        public void save() {
            // Logic for serializing the Employee object to the database.
            // ACTOR: Database Administrators / Tech Team
        }
    }
    ```

*   **The Problem (Why This is Dangerous):**
    1.  **Merge Conflicts:** The developers working for Finance and the developers working for HR are now forced to edit the same file (`Employee.java`). This leads to a high probability of merge conflicts.
    2.  **Fragility:** A change in the `calculatePay` method could accidentally break the `reportHours` method if they share some internal state. The two actors' functionalities are not isolated. For example, a helper method used by both might be changed in a way that only works for one.

#### **Refactoring to Adhere to SRP**
*   "The solution is to separate the code that serves different actors into different classes."

*   **Refactored Code (Adheres to SRP):**
    ```java
    // A simple, unadorned data structure. No business logic.
    public class EmployeeData {
        private String name;
        private double hoursWorked;
        // ... getters and setters
    }

    // Class dedicated SOLELY to the Finance actor.
    public class PayCalculator {
        public Money calculatePay(EmployeeData employee) {
            // ... logic for finance
        }
    }

    // Class dedicated SOLELY to the HR actor.
    public class HourReporter {
        public String reportHours(EmployeeData employee) {
            // ... logic for HR
        }
    }

    // Class dedicated SOLELY to the DBA/Tech actor.
    public class EmployeeRepository {
        public void save(EmployeeData employee) {
            // ... logic for database persistence
        }
    }
    ```
*   **Key Takeaway:** We have achieved **high cohesion** and **low coupling**. The `PayCalculator` knows nothing about the database. The `EmployeeRepository` knows nothing about calculating pay. Each class has a single, clear purpose.

---

### **Part 2: The Open/Closed Principle (90 mins)**

#### **The Goal: Stability**
"Once a piece of code is written, tested, and deployed, we should ideally never touch it again. The Open/Closed Principle, or OCP, is the design principle that makes this possible."

*   **The Definition:** Software entities (classes, modules, functions) should be **open for extension**, but **closed for modification.**
*   **In simple terms:** You should be able to add new functionality without changing existing, working code.

#### **Case Study: The `Shape` Calculator**
*   "This is a classic example you'll find in many architecture discussions, including 'Head First Design Patterns'. It perfectly illustrates the problem OCP solves."
*   **Reference:** *Head First Design Patterns* by Eric Freeman & Elisabeth Robson. [Link to book](https://www.oreilly.com/library/view/head-first-design/0596007124/)

*   **Initial Code (Violates OCP):**
    ```java
    public class AreaCalculator {
        public double calculateArea(Object[] shapes) {
            double totalArea = 0;
            for (Object shape : shapes) {
                if (shape instanceof Rectangle) {
                    Rectangle r = (Rectangle) shape;
                    totalArea += r.getWidth() * r.getHeight();
                }
                if (shape instanceof Circle) {
                    Circle c = (Circle) shape;
                    totalArea += c.getRadius() * c.getRadius() * Math.PI;
                }
                // PROBLEM: What happens when we want to add a Triangle?
            }
            return totalArea;
        }
    }
    ```
*   **The Problem:** To add a `Triangle`, we have to come back and **modify** the `AreaCalculator` class. This class is not closed for modification. Every new shape requires a change, increasing the risk of breaking existing logic.

#### **Refactoring to Adhere to OCP**
*   "The key to OCP is **abstraction**. We depend on an abstract interface, not concrete details."

*   **Refactored Code (Adheres to OCP):**
    ```java
    // 1. Create an abstraction (the contract).
    public interface Shape {
        double getArea();
    }

    // 2. Concrete implementations adhere to the contract.
    public class Rectangle implements Shape {
        // ... width, height ...
        public double getArea() { return getWidth() * getHeight(); }
    }

    public class Circle implements Shape {
        // ... radius ...
        public double getArea() { return getRadius() * getRadius() * Math.PI; }
    }

    // NEW REQUIREMENT: Add a Triangle.
    // We create a NEW class. We DON'T modify any old code.
    public class Triangle implements Shape {
        // ... base, height ...
        public double getArea() { return getBase() * getHeight() / 2; }
    }

    // 3. The high-level module depends only on the abstraction.
    // This class is now CLOSED for modification. It will never need to change.
    public class AreaCalculator {
        public double calculateArea(Shape[] shapes) {
            double totalArea = 0;
            for (Shape shape : shapes) {
                totalArea += shape.getArea();
            }
            return totalArea;
        }
    }
    ```
*   **Key Takeaway:** By introducing the `Shape` interface, we have created a system that is **open for extension** (we can add infinite new shapes) but **closed for modification** (the `AreaCalculator` is finished and never needs to be touched). This is the power of OCP.

---

### **Part 3: Project Briefing (30 mins)**

*   **Introduction to the Project:** The Plugin-Powered Word Processor.
*   **The Goal:** This project will force you to combine SRP and OCP. You will design a core application that is so well-decoupled that new features can be "plugged in" without ever modifying the core.
*   **Connection to Real World:** This is how truly extensible systems like VS Code (with its extensions) or WordPress (with its plugins) are designed.
*   **Q&A.**
