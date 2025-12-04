# Project 1: The Legacy Code Rescue

### **Scenario**

You are a team of elite software architects hired by a promising startup. Their core product, a command-line inventory management system, was built quickly by the founders and has become a "Big Ball of Mud." It's critical to their business, but it's nearly impossible to maintain. Adding a new feature takes weeks, and every change breaks something new.

Your mission is to go in, stabilize the system, refactor it towards clean architecture, and provide a clear path for future development.

### **The Codebase**

You will be given a Java or Python application with the following characteristics:

*   **No Tests:** There is zero test coverage.
*   **Single "God" Class:** Most of the business logic is crammed into one or two large, unmanageable classes.
*   **High Coupling:** The application directly reads from the console, contains business logic, and writes directly to a file-based database, all in the same methods.
*   **Low Cohesion:** Methods are long and serve multiple, unrelated purposes.
*   **Primitive Obsession:** Data like product IDs, prices, and quantities are passed around as simple strings and doubles.

### **Your Mission**

Your task is divided into three phases:

1.  **Phase 1: Get it Under Test (The Golden Rule of Refactoring)**
    *   **Goal:** Before you change a single line of business logic, you must get the existing code under a test harness.
    *   **Challenge:** The code is not testable. You will need to use techniques like **characterization tests** (tests that characterize the actual behavior of the system) and carefully break dependencies to introduce seams where you can inject test doubles.
    *   **Reference:** Michael Feathers' book, "Working Effectively with Legacy Code," is the bible for this kind of work. [Link to book summary/review](https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code)

2.  **Phase 2: Refactor Towards SOLID**
    *   **Goal:** Systematically refactor the code to improve its structure.
    *   **Actions:**
        *   Identify responsibilities and apply the **Single Responsibility Principle** to break up the "god" classes.
        *   Introduce interfaces for external dependencies (like the file system and console) to apply the **Dependency Inversion Principle**.
        *   Create Value Objects to combat **Primitive Obsession**.
        *   Improve naming and reduce method complexity.

3.  **Phase 3: Document and Present**
    *   **Goal:** Prepare a professional report and presentation for the "startup's CTO" (your instructors).
    *   **Content:**
        *   An analysis of the original codebase's key problems.
        *   A summary of your refactoring strategy.
        *   "Before and After" code examples to demonstrate the improvements.
        *   A final UML diagram of the new, cleaner architecture.

### **Evaluation Criteria**

*   **Test Quality (40%):** The comprehensiveness and quality of your test suite. Did you successfully get the legacy code under test *before* refactoring?
*   **Architectural Improvement (40%):** How well you applied the SOLID principles. Is the final code demonstrably more maintainable, cohesive, and decoupled?
*   **The Report & Presentation (20%):** The clarity, professionalism, and persuasiveness of your final report.

This project is a realistic simulation of a common and highly valuable software engineering task. Good luck, architects.
