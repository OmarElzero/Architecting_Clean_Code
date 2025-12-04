# Project 3: The Universal Payment Gateway

### **Scenario**

You are designing the core payment processing library for a new global e-commerce platform. The business requirements are clear: you must support payments via **Credit Card** and **PayPal** immediately, and you must be prepared to add new providers like **Cryptocurrency** in the future.

However, these providers have different capabilities. Credit Cards can be charged and refunded. PayPal can be charged, refunded, and can also set up recurring subscription payments. Crypto payments, due to their nature, can *only* be charged; they cannot be refunded through your system.

Your task is to design a set of interfaces and classes that models this reality in a way that is both **correct (LSP)** and **flexible (ISP)**.

### **Real-World Inspiration**

*   **Stripe API:** Stripe provides a clean API that abstracts away the complexity of dealing with thousands of different banks and payment methods. Their interface design is a masterclass in ISP and LSP.
*   **OmniPay (PHP Library):** A popular open-source library in the PHP world that provides a unified API for dozens of different payment gateways. Its success depends entirely on the quality of its abstractions. [Link to OmniPay](https://github.com/thephpleague/omnipay)

### **The Challenge**

You will design a `PaymentService` and the interfaces it depends on. The service should be able to process payments from any provider without knowing the concrete details of that provider, and it must do so safely, without causing runtime errors for unsupported operations.

### **Core Requirements**

1.  **Interface Design (ISP Focus):**
    *   Design a set of small, role-based interfaces for payment processing.
    *   Do **not** create a single, "fat" `IPaymentProcessor` interface.
    *   Instead, create interfaces like `IChargeable` (for handling charges), `IRefundable` (for handling refunds), and `ISubscribable` (for handling subscriptions).

2.  **Concrete Implementations:**
    *   Create three classes that implement the appropriate interfaces:
        *   `CreditCardProcessor` (should implement `IChargeable` and `IRefundable`).
        *   `PayPalProcessor` (should implement `IChargeable`, `IRefundable`, and `ISubscribable`).
        *   `CryptoProcessor` (should implement *only* `IChargeable`).

3.  **The Payment Service (LSP Focus):**
    *   Create a `PaymentService` class.
    *   This service will have methods that operate on the interfaces, for example:
        ```java
        // This method can accept ANY processor that is chargeable.
        public void processCharge(IChargeable processor, Money amount) {
            processor.charge(amount);
        }

        // This method can ONLY accept processors that are refundable.
        public void processRefund(IRefundable processor, Money amount) {
            processor.refund(amount);
        }
        ```
    *   The key is that your design should prevent a developer from accidentally trying to refund a `CryptoProcessor` at **compile time**, not at runtime. You should not have any `if (processor instanceof IRefundable)` checks in your `PaymentService`. The type system should enforce correctness.

### **The Litmus Test**

Your design will be successful if a developer using your `PaymentService` finds it impossible to even *attempt* to call `processRefund` with a `CryptoProcessor` instance. The code simply should not compile if they try. This is the ultimate proof of a design that adheres to LSP and ISP.

### **Evaluation Criteria**

*   **ISP Adherence (40%):** Are your interfaces segregated by role? Are they lean and focused?
*   **LSP Adherence (40%):** Does your design use the type system to enforce correctness? Is it impossible for a client to use a subtype in a way that breaks the program?
*   **Code Clarity & Design (20%):** The overall elegance and safety of your API design.

This project is a deep dive into the theory of object-oriented design. A great solution will feel safe, intuitive, and robust, preventing errors before they can happen.
