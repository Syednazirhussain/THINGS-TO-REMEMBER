S: Single Responsibility Principle
O: Open close principle
L: Liskov Subsitution
I: Interface Segregation
D: Dependency Inversion

1. SINGLE RESPONSIBILITY PRINCIPLE
    - A class should have only one responsibility.
        - This means that a class should only have one reason to change. When a class is responsible for multiple unrelated tasks, it becomes harder to modify or extend in the future without affecting other parts of the system.
    - Focus on a single purpose.
        - The class should be designed to handle one specific functionality or domain. If a class takes on multiple roles or responsibilities, it becomes harder to maintain, test, and update over time.

2. OPEN-CLOSE PRINCIPLE
    - Open for extension, closed for modification.
        - The core idea is that your software entities (classes, modules, functions) should be designed in a way that allows you to add new functionality without changing the existing code. This reduces the risk of introducing bugs when new features are added.
    - Allow new behavior without altering existing code.
        - Instead of modifying the existing code directly, you should extend it to add new features. For example, you could create new subclasses, add new methods, or implement new interfaces to extend the behavior of existing classes.

3. LISKVO SUBSITUTION PRINCIPLE
    - A child class should be able to replace its parent class without affecting the correctness of the program.
        - This means that objects of a child class should be able to be substituted wherever a parent class object is expected, without causing errors or unexpected behavior.
    - The child class should not override parent class functionality in a way that would break the intended use of the parent class.
        - If a parent class has a method, and the child class overrides it, the child class must ensure that it still adheres to the same contract or behavior. For instance, if the parent class method is designed to return a non-null value, the child class should not return null or behave unpredictably.

4. INTERFACE SEGREGATION
    - An interface is like a contract that tells your class what it needs to do.
    - Instead of having one big interface with many methods, break it into smaller ones. For example, create separate interfaces for reading, writing, and deleting.
    - When you need to use all the methods, you can make a main interface that includes all the smaller ones.
    - This way, your class only has to deal with the methods it actually needs, avoiding the problem of dealing with methods you don't use or haven't implemented yet.

5. DEPENDENCY INVERSION
    - High-level modules should not depend on low-level modules. Both should depend on abstractions.
    - Decouple your system to make it more flexible and easier to maintain.
        - By depending on abstractions rather than concrete implementations, your system becomes more modular. This decoupling allows you to easily replace or modify components without affecting other parts of the system.
    - Inversion of control improves testability.
        - When high-level modules depend on abstractions, it’s much easier to substitute real implementations with mock or stub classes, which makes testing more straightforward and isolated.
    - Avoid tight coupling between components.
        - Tight coupling makes your system rigid and difficult to modify. By following Dependency Inversion, high-level modules are insulated from changes in low-level modules, reducing the risk of breaking other parts of the system when changes are made.
    - Promotes reusability and scalability.
        - Since high-level modules only rely on abstractions, they can be reused in different contexts with various low-level implementations, making the system more adaptable to future changes or new requirements.