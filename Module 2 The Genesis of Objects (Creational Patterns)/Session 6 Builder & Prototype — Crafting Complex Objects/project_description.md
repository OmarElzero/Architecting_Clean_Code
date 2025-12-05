# Project 6: The Fluent Query Builder API

### **Scenario**

You are building a new data access layer for your company. To make it easy and intuitive for other developers to interact with the database, you want to provide a "fluent API" for building SQL queries. Instead of forcing developers to manually concatenate strings (which is error-prone and vulnerable to SQL injection), you will provide a **Builder** that allows them to construct complex queries step-by-step in a readable, chainable way.

### **Real-World Inspiration**

*   **jOOQ (Java):** A very popular library in the Java ecosystem that provides a type-safe, fluent API for writing SQL in Java. Its core is a powerful and complex query builder. [Link to jOOQ](https://www.jooq.org/)
*   **SQLAlchemy (Python):** A major ORM and SQL toolkit for Python. Its Expression Language allows for the programmatic construction of SQL queries using a builder-like pattern.
*   **LINQ (C#):** Language-Integrated Query in C# allows developers to write query expressions directly in the language, which are then translated into SQL or other query languages. This is a very advanced form of the builder concept.

### **The Challenge**

You will implement a `QueryBuilder` for a simplified SQL `SELECT` statement. The builder should allow a client to construct a query by specifying the `SELECT` columns, the `FROM` table, `WHERE` clauses, and an `ORDER BY` clause.

### **Core Requirements**

1.  **The `Query` Class:**
    *   This is the complex object you are building.
    *   It should be **immutable**. Its fields should be final.
    *   It should have private fields to store the parts of the query: `select`, `from`, `where`, `orderBy`.
    *   Its constructor should be **private**. Only the `QueryBuilder` can create a `Query` object.
    *   It should have a `toString()` method that assembles the final, valid SQL string. For example: `SELECT name, age FROM users WHERE age > 21 ORDER BY name;`

2.  **The `QueryBuilder` Class:**
    *   This will be the static nested class inside `Query`.
    *   It must have methods for each part of the query construction. These methods must return the `Builder` instance (`this`) to allow for a fluent, chainable API.
        *   `select(String... columns)`: Specifies the columns to select.
        *   `from(String table)`: Specifies the table to query.
        *   `where(String condition)`: Specifies a condition. This can be called multiple times to add `AND` conditions.
        *   `orderBy(String column)`: Specifies the column to sort by.
    *   It must have a final `build()` method that creates and returns the `Query` object.

3.  **Client Code:**
    *   Write a main application that demonstrates how to use your `QueryBuilder` to construct several different queries.
    *   Example usage should look like this:
        ```java
        Query query1 = new Query.QueryBuilder()
                            .select("name", "email")
                            .from("customers")
                            .where("city = 'New York'")
                            .build();
        System.out.println(query1);

        Query query2 = new Query.QueryBuilder()
                            .select("*")
                            .from("products")
                            .where("price > 100")
                            .where("stock > 0")
                            .orderBy("price")
                            .build();
        System.out.println(query2);
        ```

### **Bonus Challenge (Conceptual Prototype)**

You do not need to implement this part, but you should answer the following questions in a comment block in your main application file:

*   Imagine that you have a very common, expensive-to-construct query that is used as a template in many parts of your application (e.g., a complex report query with many default `WHERE` clauses).
*   How could you use the **Prototype pattern** to allow developers to get a copy of this "template query" and then add their own specific `ORDER BY` or additional `WHERE` clauses without modifying the original template?
*   What would be the main challenge in implementing the `clone()` method for your `Query` object? (Hint: Think about mutable vs. immutable fields).

### **Evaluation Criteria**

*   **Builder Implementation (60%):** Does your `QueryBuilder` correctly construct the `Query` object? Is its API fluent and intuitive? Is the final `Query` object immutable?
*   **Generated SQL (20%):** Does the `toString()` method of your `Query` class produce a syntactically correct SQL query string?
*   **Code Clarity & Design (20%):** The overall quality, readability, and design of your solution.
*   **Bonus Challenge:** The quality and correctness of your conceptual explanation of how the Prototype pattern could be applied.

This project is a direct application of a pattern used in countless professional software libraries. A well-designed builder can make a complex system a pleasure to use.
