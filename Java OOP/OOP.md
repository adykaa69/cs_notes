# Object-Oriented Programming

### Definition
- **Object-oriented programming** is about creating objects that contain both data and methods, while procedural programming is about writing procedures or methods that perform operations on the data.
- Object-Oriented Programming (OOP) is a programming paradigm based on the concept of **"objects"**, which can contain **data** (fields, often called _attributes_) and **behaviour** (methods).
### Purpose
- Make code **more modular**, **reusable**, and **easier to maintain**.
	- Keep the code [[DRY]] #todo (Don't Repeat Yourself)
- Reflect the **structure and behaviour of real-world systems** in code.
- Promote **encapsulation** of data and functionality within objects, reducing complexity and increasing clarity.
### Benefits
| **Benefit**             | **Description**                                                                                                |
| ----------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Modularity**          | Each class is self-contained. You can build systems by combining smaller, manageable pieces.               |
| **Reusability**         | Once a class is written, it can be reused in other projects or contexts. Inheritance also promotes reuse.  |
| **Maintainability**     | Changes in one part of the system can often be made without affecting others.                              |
| **Testability**         | Classes can be tested independently, and mock objects can replace real ones for unit testing.              |
| **Scalability**         | Easier to scale as projects grow — you can extend existing functionality using inheritance or composition. |
| **Real-world modeling** | Objects naturally represent entities like Customer, Order, Product, etc.                                   |
### Classes and Objects 
- Classes and objects are the two main aspects of object-oriented programming.
	- A class is a template for objects, and an object is an instance of a class.
> [!example]
> - Car is a class
> - Toyota Corolla (a specific car) is an object (instance of the Car class)
> ```java
> public class Car {
>     String color;
>     String brand;
> 
>     public void start() {
>         System.out.println("Car started");
>     }
> }
> 
> Car myCar = new Car();
> myCar.color = "Red";
> myCar.brand = "Toyota";
> myCar.start();
> ```

