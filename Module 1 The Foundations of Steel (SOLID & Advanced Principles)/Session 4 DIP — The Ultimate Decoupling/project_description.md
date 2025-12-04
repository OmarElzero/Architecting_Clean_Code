# Project 4: The Mocking Framework Challenge

### **Scenario**

In professional software development, unit tests are sacred. A good unit test verifies a single piece of business logic in **complete isolation**. To achieve this isolation, we must replace real, low-level dependencies (like databases or web services) with "test doubles" or "mocks."

Your mission is to build a miniature mocking framework. This framework will leverage the **Dependency Inversion Principle (DIP)** to allow a developer to test a high-level service without ever touching the real, low-level dependency.

### **Real-World Inspiration**

*   **Mockito (Java):** A hugely popular open-source mocking framework. It allows you to create mock objects and define their behavior with a clean, fluent API. [Link to Mockito](https://site.mockito.org/)
*   **Moq (C# / .NET):** The most popular mocking library for the .NET ecosystem, heavily inspired by Mockito.
*   **Jest (JavaScript):** The leading testing framework in the JavaScript world, which has powerful, built-in mocking capabilities.

### **The Challenge**

You will build a small, but complete, system that demonstrates the testability that DIP provides.

### **Core Requirements**

1.  **The Abstraction and its Dependencies:**
    *   Create an interface for a low-level dependency: `IUserRepository`. It should have one method: `String findUserById(int id)`.
    *   Create a concrete implementation: `RealUserRepository`. This class will simulate a database call (e.g., `return "Real User From Database";`).
    *   Create a high-level service that depends on the abstraction: `UserService`. This service will have a method, `getFormattedUserData(int id)`, which calls the repository and formats the result (e.g., `return "Data for: Real User From Database";`).
    *   The `UserService` **must** use **Constructor Injection** to receive its `IUserRepository` dependency.

2.  **The Mocking Framework:**
    *   Create a class called `MockUserRepository` that also implements `IUserRepository`.
    *   This mock class should have a public method, `when(int id, String thenReturn)`, that allows a test to configure its behavior. It should store these configurations in a map or dictionary.
    *   When its `findUserById` method is called, it should check its internal map and return the configured string. If no configuration is found, it can return null or throw an exception.

3.  **The Unit Test:**
    *   Write a test class for `UserService`.
    *   This test **must not**, under any circumstances, create an instance of `RealUserRepository`.
    *   The test should:
        1.  Create an instance of your `MockUserRepository`.
        2.  Configure it: `mockRepo.when(101, "Mocked User Name");`
        3.  Create an instance of `UserService`, **injecting the mock repository** into its constructor.
        4.  Call `userService.getFormattedUserData(101)`.
        5.  Assert that the result is `"Data for: Mocked User Name"`.

### **The Litmus Test**

The success of your project is determined by this: can your unit test for `UserService` run correctly even if the `RealUserRepository` class is completely deleted? If the answer is yes, you have achieved true isolation through Dependency Inversion.

### **Evaluation Criteria**

*   **DIP/DI Adherence (50%):** Does the `UserService` correctly depend on the `IUserRepository` abstraction and use constructor injection? Is it fully decoupled from the `RealUserRepository`?
*   **Mock Framework Design (30%):** How clean and effective is your `MockUserRepository`? Is its API intuitive for a developer writing a test?
*   **Test Quality (20%):** Does your unit test successfully verify the `UserService`'s logic in complete isolation?

This project is the capstone of this module because testability is the ultimate proof of a well-designed, decoupled architecture. A system that is easy to test is a system that is easy to maintain.
