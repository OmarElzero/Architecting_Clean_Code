# The Software Architect's Gauntlet: From Clean Code to System Design

## Course Overview

*   **Course Title:** The Software Architect's Gauntlet: From Clean Code to System Design
*   **Duration:** 18 Sessions (72 hours total)
*   **Format:** Intensive, project-based, and competitive team challenges
*   **Target Audience:** Highly motivated developers aiming for senior/FAANG-level software engineering roles
*   **Organized By:** DSC Cairo University Chapter
*   **Goal:** To forge elite software architects with a mastery of design principles, patterns, and system design, capable of building scalable, resilient, and maintainable software for the most demanding environments.

---

## The Gauntlet: A Series of Architectural Challenges

Each session is a 4-hour deep-dive workshop, culminating in a competitive team project. Teams will be evaluated on design quality, implementation, and presentation.

### **Module 1: The Foundations of Steel (SOLID & Advanced Principles)**

*   **Session 1: Beyond Clean Code — The Philosophy of Maintainability**
    *   **Topics:** The cost of bad code, advanced code smells, coupling vs. cohesion, and the SOLID principles at a strategic level.
    *   **Project:** *The Legacy Code Rescue.* Teams are given a large, tangled codebase. Their mission: refactor it towards SOLID principles and present a report on the improvements.
*   **Session 2: SRP & OCP — The Art of Separation and Extension**
    *   **Topics:** Deep dive into the Single Responsibility and Open/Closed Principles.
    *   **Project:** *The Plugin-Powered Word Processor.* Design a core text editor that can be extended with new features (e.g., spell check, image insertion) without modifying the core code.
*   **Session 3: LSP & ISP — The Contracts of Code**
    *   **Topics:** Deep dive into the Liskov Substitution and Interface Segregation Principles.
    *   **Project:** *The Universal Payment Gateway.* Build a system that can process payments from various providers (Stripe, PayPal, etc.) through a clean, unified interface.
*   **Session 4: DIP — The Ultimate Decoupling**
    *   **Topics:** Deep dive into the Dependency Inversion Principle, Dependency Injection, and Inversion of Control containers.
    *   **Project:** *The Mocking Framework Challenge.* Build a miniature testing framework that uses DI to mock dependencies in a given application.

### **Module 2: The Genesis of Objects (Creational Patterns)**

*   **Session 5: The Factories — Simple Factory, Factory Method, Abstract Factory**
    *   **Topics:** Mastering the factory patterns to decouple object creation.
    *   **Project:** *The Cross-Platform UI Toolkit.* Design a factory that can create UI elements (buttons, windows) for different operating systems (Windows, macOS, Linux).
*   **Session 6: Builder & Prototype — Crafting Complex Objects**
    *   **Topics:** Using the Builder for step-by-step object construction and Prototype for cloning.
    *   **Project:** *The Query Builder API.* Create a fluent API for building complex SQL or NoSQL database queries.
*   **Session 7: Singleton — The Double-Edged Sword**
    *   **Topics:** The Singleton pattern, its use cases, and its dangers (anti-pattern).
    *   **Project:** *The Configuration Manager.* Build a thread-safe, application-wide configuration manager.

### **Module 3: Weaving the Code (Structural Patterns)**

*   **Session 8: Adapter & Facade — Unifying and Simplifying**
    *   **Topics:** Using Adapter to make incompatible interfaces work together and Facade to simplify complex subsystems.
    *   **Project:** *The Third-Party API Integrator.* Integrate several disparate, poorly documented third-party APIs into a single, clean Facade.
*   **Session 9: Decorator & Proxy — Dynamic Behavior and Control**
    *   **Topics:** Using Decorator to add responsibilities to objects dynamically and Proxy to control access.
    *   **Project:** *The Caching Proxy Server.* Build a proxy that intercepts requests to a slow service and caches the results.
*   **Session 10: Composite & Flyweight — Trees and Efficiency**
    *   **Topics:** Using Composite to treat individual objects and compositions of objects uniformly, and Flyweight to save memory.
    *   **Project:** *The Vector Graphics Editor.* Design a graphics editor that can handle complex, nested shapes efficiently.
*   **Session 11: Bridge — The Great Decoupler**
    *   **Topics:** Decoupling an abstraction from its implementation with the Bridge pattern.
    *   **Project:** *The Multi-Device Media Player.* Build a media player that can play different formats on different devices (e.g., audio on a speaker, video on a screen).

### **Module 4: Orchestrating Behavior (Behavioral Patterns)**

*   **Session 12: Strategy & Template Method — Encapsulating Algorithms**
    *   **Topics:** Using Strategy to make algorithms interchangeable and Template Method to define the skeleton of an algorithm.
    *   **Project:** *The Sorting Algorithm Visualizer.* Build a tool that can visualize different sorting algorithms (Bubble Sort, Quick Sort, etc.) applied to the same dataset.
*   **Session 13: Command & Chain of Responsibility — Decoupling Requests**
    *   **Topics:** Using Command to turn a request into a stand-alone object and Chain of Responsibility to pass requests along a chain of handlers.
    *   **Project:** *The Undo/Redo Engine.* Implement a robust undo/redo system for a text or graphics editor.
*   **Session 14: Observer & Mediator — Communication Hubs**
    *   **Topics:** Using Observer to notify objects of state changes and Mediator to centralize complex communications.
    *   **Project:** *The Stock Market Ticker.* Build a real-time stock ticker where multiple displays (Observers) react to price changes from a central feed (the Subject).
*   **Session 15: State, Memento & Iterator — Managing State and Traversal**
    *   **Topics:** Using State to alter an object's behavior when its state changes, Memento to capture and restore an object's internal state, and Iterator to traverse containers.
    *   **Project:** *The Turn-Based Game Engine.* Build the core engine for a turn-based game, managing player turns (State) and game saves (Memento).
*   **Session 16: Visitor & Interpreter — The Language of Your Code**
    *   **Topics:** Deep dive into the Visitor pattern for adding external operations and the Interpreter pattern for processing languages.
    *   **Project:** *The AST Calculator.* Build an Abstract Syntax Tree for mathematical expressions and use the Visitor or Interpreter pattern to calculate the results.

### **Module 5: The Grand Design (System Design Fundamentals)**

*   **Session 17: Principles of Scalable Systems**
    *   **Topics:** Vertical vs. Horizontal Scaling, Load Balancing, Caching Strategies (Client-side, CDN, Server-side), Database Replication and Sharding.
    *   **Project:** *The URL Shortener.* Design a high-availability URL shortening service like Bitly, focusing on the data model and caching layers.
*   **Session 18: The System Design Interview — The Final Gauntlet**
    *   **Topics:** The complete system design interview lifecycle: clarifying requirements, estimating scale, designing the API, high-level design, and deep-diving into components. CAP Theorem, Message Queues, and final Q&A.
    *   **Project:** *The Capstone Project — Design a FAANG-Scale System.* Teams will be given a classic system design prompt (e.g., "Design a Social Media Feed," "Design a Ride-Sharing App") and will have the entire session to design a solution, present it, and defend their architectural choices before a panel.
