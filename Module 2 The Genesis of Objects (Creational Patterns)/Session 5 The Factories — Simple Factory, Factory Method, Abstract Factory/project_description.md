# Project 5: The Cross-Platform UI Toolkit

### **Scenario**

You are a team of framework architects at a major tech company. Your task is to build the core of a new cross-platform UI toolkit. This toolkit will allow developers to write their application's UI code once, and it will render with the native look and feel of the operating system it's running on (Windows, macOS, etc.).

Your challenge is to use the **Factory patterns** to decouple the client code (the application developer's code) from the concrete UI widgets of each operating system.

### **Real-World Inspiration**

*   **Java's Abstract Window Toolkit (AWT):** One of the earliest examples of this architecture. It provided a common API for UI elements, with different "peer" factories for different operating systems.
*   **React Native / Flutter:** Modern frameworks that use a similar principle. Developers write code against a common component API, and the framework's rendering engine (the "factory") is responsible for creating the actual native iOS or Android UI views.

### **The Challenge**

You will implement a simplified version of this toolkit. Your solution must demonstrate a clear separation between the client code and the concrete widget families.

### **Core Requirements**

This project is divided into two parts, building on the patterns from the session.

#### **Part 1: The Factory Method**

1.  **The `Dialog` and the `Button`:**
    *   Create an abstract `Button` class with a `render()` method.
    *   Create concrete implementations: `WindowsButton` and `MacButton`. Their `render()` methods should just print a message like "Rendering a button in Windows style."
    *   Create an abstract `Dialog` class. This is your **Creator**.
    *   The `Dialog` class should have some core logic, like a `renderWindow()` method that prints "Rendering a dialog box." and then calls a `createButton()` method.
    *   `createButton()` is your **factory method**. It should be abstract.

2.  **Concrete `Dialog` Subclasses:**
    *   Create `WindowsDialog` and `MacDialog` that extend `Dialog`.
    *   Each of these subclasses will implement the `createButton()` factory method to return the appropriate button type (`WindowsButton` or `MacButton`).

3.  **Client Code (Part 1):**
    *   Your main application should be able to create a `Dialog` of a specific type (e.g., `new WindowsDialog()`) and call its `renderWindow()` method. The output should show the correct type of button being rendered.

#### **Part 2: The Abstract Factory**

The first approach is good, but what if a dialog needs to create *multiple* kinds of widgets (buttons, checkboxes, text fields)? A single factory method isn't enough.

1.  **The `GUIFactory` Interface:**
    *   Create an `IGUIFactory` interface. This is your **Abstract Factory**.
    *   It should declare methods for creating a family of related widgets: `createButton()` and `createCheckbox()`.

2.  **Concrete Widget and Factory Families:**
    *   Create abstract `Button` and `Checkbox` interfaces.
    *   Create concrete families of widgets: `WindowsButton`, `WindowsCheckbox`, `MacButton`, `MacCheckbox`.
    *   Create two concrete factory implementations: `WindowsGUIFactory` and `MacGUIFactory`. Each will return the appropriate family of widgets.

3.  **The `Application` (The Client):**
    *   Create an `Application` class that is configured with a specific `IGUIFactory`. It receives the factory in its constructor.
    *   The `Application` class will have a `createUI()` method. Inside this method, it will use the factory to create a button and a checkbox and then call their `render()` methods.
    *   The `Application` class should know **nothing** about `WindowsButton` or `MacCheckbox`. It should only know about the abstract `IGUIFactory`, `Button`, and `Checkbox` types.

4.  **Client Code (Part 2):**
    *   Your main method will be responsible for creating the correct factory based on some configuration (e.g., a string saying "Windows" or "macOS") and then passing that factory to the `Application`.

### **Evaluation Criteria**

*   **Factory Method Implementation (30%):** Is your Part 1 solution a correct and clear implementation of the Factory Method pattern?
*   **Abstract Factory Implementation (50%):** Does your Part 2 solution correctly decouple the `Application` client from the concrete widget families? Can you add a new OS (e.g., "Linux") by only creating a new factory and a new set of widgets, without changing the `Application` class at all?
*   **Code Clarity & Design (20%):** The overall quality and readability of your code.

This project is a deep dive into the patterns that enable some of the most powerful and flexible software frameworks in the world.
