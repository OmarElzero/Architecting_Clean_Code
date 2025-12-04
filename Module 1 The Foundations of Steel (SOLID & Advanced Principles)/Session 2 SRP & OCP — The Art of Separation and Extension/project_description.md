# Project 2: The Plugin-Powered Word Processor

### **Scenario**

You are the lead architects designing a new, cutting-edge word processor. A key selling point is its "plugin marketplace," where third-party developers can create and sell new features. To make this possible, the core application must be incredibly stable and extensible. You must be able to add major new features (like spell checking, new export formats, etc.) without ever modifying the core editor code.

Your task is to design this plugin architecture, proving that the core is **closed for modification** while being **open for extension**.

### **Real-World Inspiration**

*   **Visual Studio Code:** One of the most successful examples of a plugin-based architecture. The core editor is small and stable, but it can be extended to support any language or feature through its extension marketplace.
*   **WordPress:** The famous content management system. The core is stable, but its functionality is extended by thousands of plugins for e-commerce, SEO, and more.

### **The Challenge**

You will build the core of the word processor and a set of plugins, focusing on a clean separation of concerns (**SRP**) and a robust, extensible design (**OCP**).

### **Core Requirements**

1.  **The Core Editor (SRP Focus):**
    *   Create a central `Editor` class.
    *   This class's **only responsibility** should be managing the text content (e.g., `getText()`, `setText()`, `insertText()`).
    *   It should know nothing about spell checking, exporting, or any other feature. Its only actor is the user typing text.

2.  **The Plugin Architecture (OCP Focus):**
    *   Design a `Plugin` interface (or abstract class). This is the contract that all plugins must adhere to. It might have a method like `execute(Editor editor)`.
    *   Create a `PluginManager` that can "register" and "run" plugins. The main application will use this manager to execute all active features.

3.  **Implement at Least Three Concrete Plugins:**
    *   `SpellCheckPlugin`: A plugin that iterates through the editor's text and prints any "misspelled" words (you can use a simple hardcoded dictionary).
    *   `ExportToHtmlPlugin`: A plugin that takes the editor's text and saves it to a simple `output.html` file.
    *   `WordCountPlugin`: A plugin that prints the total number of words in the editor's text to the console.

4.  **The Main Application:**
    *   The main entry point of your application should:
        1.  Create an `Editor` instance.
        2.  Create a `PluginManager` instance.
        3.  Register the three plugins.
        4.  Simulate a user typing some text into the editor.
        5.  Run all the registered plugins.

### **The Litmus Test**

After you have built all of the above, your final test is to **create a fourth plugin** (e.g., an `ExportToMarkdownPlugin`) and integrate it into the application **without modifying a single line of code in the `Editor`, `PluginManager`, or the existing three plugin classes.** You should only have to add the new plugin and register it in the main application file.

### **Evaluation Criteria**

*   **SRP Adherence (30%):** How well did you isolate the core text-editing responsibility? Is the `Editor` class free of any feature-specific logic?
*   **OCP Adherence (50%):** Does your design pass the "litmus test"? Can a new plugin be added without any modification to existing code? The elegance and flexibility of your `Plugin` interface will be heavily scrutinized.
*   **Code Clarity & Design (20%):** The overall readability and quality of your solution.

This project will demonstrate the commercial value of good architecture. A system designed with OCP is a system that can adapt and grow with business needs without requiring costly and risky rewrites.
