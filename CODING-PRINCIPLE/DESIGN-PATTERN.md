- Design Patterns
    - There are 23 design patterns in general that are categorize into three groups.
        1. Creational: creational patterns help you manage the instantiation of objects in a way that suits your needs, especially in complex systems.
            - Factory Method.
            - Abstract Method.
            - Factory Method.
            - Builder.
            - Prototype.
            - Singleton.

        2. Structural: Structural patterns help you design and manage the relationships between objects and classes in a way that makes your system more scalable and easier to maintain..
            - Adapter.
            - Bridge.
            - Composite.
            - Decorator.
            - Facade.
            - Flyweight.
            - Proxcy.

        3. Behavioural: Behavioral patterns are concerned with the way objects collaborate and communicate to complete tasks, making the system more flexible and easier to maintain.
            - Chain of Responsibility
            - Command
            - Interpreter
            - Iterator
            - Mediator
            - Memento
            - Observer
            - State
            - Strategy Patterns
            - Template Method
            - Visitor

1. Strategy Pattern (Behaviour Pattern):
    - What it is: The Strategy Pattern is used when you want to choose an algorithm or behavior at runtime. It allows you to define a family of algorithms and make them interchangeable, without modifying the code that uses them.
        - Baking Cake Example:
            Imagine you have different types of cakes: chocolate cake, vanilla cake, etc. Each type of cake has its own way of getting ingredients and methods to make it.
            You create a CakeStrategy interface with two methods: getIngredients() and getMethods().
            Then, each specific cake class (like ChocolateCake, VanillaCake) implements this interface to provide its own specific details on how to bake that type of cake.
        - Benefit: 
            This avoids messy code because each cake type knows how to bake itself, and you can easily add new cakes in the future without changing the existing code.

2. Decorator Pattern (Structural Pattern):
    - What it is: The Decorator Pattern allows you to add new functionality to an object dynamically, without altering its structure. It's like putting additional layers or decorations on an object to extend its behavior.
        - Example in SOLID principle (Open/Closed Principle):
            Imagine you have a basic object like a Coffee class. You can use the Decorator Pattern to add more features like milk or sugar to your coffee without changing the original Coffee class.
            This is in line with the Open/Closed Principle, where the class is open for extension (you can add new features like milk or sugar) but closed for modification (you don’t change the original Coffee class).

3. Observer Pattern (Behaviour Pattern):
    - What it is: The Observer Pattern is used to allow one object (the subject) to notify multiple other objects (the observers) about changes in its state. It’s useful when you want to implement event-driven systems where one change triggers updates in other parts of the system.
        - Example: 
            Imagine a weather station that measures temperature. You have multiple devices (observers), like a phone app or a display board, that want to be updated every time the temperature changes.
            When the weather station (subject) changes the temperature, it notifies all the devices (observers) to update their displayed data accordingly.

4. Facade Pattern (Behaviour Pattern):
    - What it is: The Facade Pattern provides a simplified interface to a complex subsystem. It hides the complexity of the system and exposes only the essential parts, making it easier for the client code to interact with.
        - Example:
            Imagine you have a home theater system with many components like a projector, sound system, and lights. Instead of the user having to deal with each individual component, you create a Facade (e.g., HomeTheaterFacade) that simplifies the process. Now, the user just interacts with the facade to turn everything on, instead of dealing with each system separately.
        - Benefit: The client only needs to interact with the facade, making things easier and cleaner.

5. Singleton Pattern (Creational Pattern):
    - What it is: The Singleton Pattern ensures that a class has only one instance and provides a global point of access to that instance. It’s useful when you want to limit the number of instances of a class to just one (like a configuration manager or a connection pool).
        - Example:
            Imagine a logging class that logs messages to a file. You only want one instance of this class to manage all logging tasks to avoid conflicts or wasted resources. Using the Singleton pattern, you make sure only one instance of the logger is created and used across your entire application.


## DESIGN PATTERN BOOK
-   Elements of Reusable Object-Oriented Software. by GoF

