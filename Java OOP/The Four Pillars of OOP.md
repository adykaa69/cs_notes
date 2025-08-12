- [[#Encapsulation]]
- [[#Inheritance]]
- [[#Abstraction]]
- [[#Polymorphism]]
<br/>

## Encapsulation
[GFG - Encapsulation in Java](https://www.geeksforgeeks.org/java/encapsulation-in-java/)
#### Definition of Encapsulation: 
- Process of wrapping code and data together into a single unit.
- Practice of **hiding internal details** of a class and only exposing a **[[Java Access Modifiers#Public|public]] [[interface]] #todo** to interact with it.
- Protecting the internal state of an object from unintended or harmful changes.
#### How Encapsulation works in Java
- Fields are marked `private`
- Access is provided through `public` getters and setters

> [!example]
> ```java
> public class BankAccount {
>     private double balance;  // hidden from direct access
> 
>     public double getBalance() { 
>         return balance;
>     }
> 
>     public void deposit(double amount) {
>         if (amount > 0) {
>             balance += amount;
>         }
>     }
> }
> ```
> 
> You cannot directly access or change `balance`. You must go through controlled methods.
#### Benefits of Encapsulation
- Data protection
- Validation before changing internal state (`if (amount > 0) {`)
- Simplifies usage by exposing only what is necessary

## Inheritance
[GFG - Inheritance in Java](https://www.geeksforgeeks.org/java/inheritance-in-java/)
#### Definition of Inheritance
- Allowing a class (subclass/child) to **inherit properties and behaviour** (fields and methods) from another class (superclass/parent) 
#### How Inheritance works in Java
- `extends` keyword
- Subclass gets access to all non-private fields and methods of superclass

>[!example] 
>```java
> public class Animal {
>     public void eat() {
>         System.out.println("Eating...");
>     }
> }
> 
> public class Dog extends Animal {
>     public void bark() {
>         System.out.println("Bark!");
>     }
> }
> 
> Dog d = new Dog();
> d.eat();   // Inherited method
> d.bark();  // Own method
> ```
##### Benefits of Inheritance
- Code reusability
- [[Method Overriding]] #todo
- Hierarchical structure
- [[#Abstraction]]

## Abstraction
[GFG - Abstraction in Java](https://www.geeksforgeeks.org/java/abstraction-in-java-2/)
#### Definition of Abstraction
- Hiding **complex implementation details** and showing only the **essential features**
- Focusing on **what an object does**, instead of **how it does it**.
	- exposing only the functionality to the user
#### How Abstraction works in Java
- [[Abstract class]] #todo (0% - 100%)
- [[Interface]] #todo (100%)

> [!example]
> Interface-based abstraction
> ```java
> public interface Animal {
>     void makeSound();
> }
> 
> public class Dog implements Animal {
>     public void makeSound() {
>         System.out.println("Bark");
>     }
> }
> 
> public class Cat implements Animal {
>     public void makeSound() {
>         System.out.println("Meow");
>     }
> }
> ```
> A method using `Animal` doesn't care if it's a `Dog` or a `Cat`. It just calls `makeSound()`.


> [!example]
> Abstract class based abstraction
> ```java
> // Abstract base class
> public abstract class Animal {
>     protected String name;
> 
>     // Constructor
>     public Animal(String name) {
>         this.name = name;
>     }
> 
>     // Abstract method — must be implemented by subclasses
>     public abstract void makeSound();
> 
>     // Optional: common behavior for all animals
>     public void sleep() {
>         System.out.println(name + " is sleeping...");
>     }
> }
> 
> // Concrete subclass - Dog
> public class Dog extends Animal {
>     public Dog(String name) {
>         super(name);
>     }
> 
>     @Override
>     public void makeSound() {
>         System.out.println("Woof!");
>     }
> }
> 
> // Concrete subclass - Cat
> public class Cat extends Animal {
>     public Cat(String name) {
>         super(name);
>     }
> 
>     @Override
>     public void makeSound() {
>         System.out.println("Meow!");
>     }
> }
> 
> // Usage example
> public class Main {
>     public static void main(String[] args) {
>         Animal dog = new Dog("Buddy");
>         Animal cat = new Cat("Whiskers");
> 
>         dog.makeSound(); // Woof!
>         cat.makeSound(); // Meow!
> 
>         dog.sleep(); // Buddy is sleeping...
>     }
> }
> ```
> A method using `Animal` doesn't care if it's a `Dog` or a `Cat`. It just calls `makeSound()`.
#### Benefits of Abstraction
- Focus on high-level logic
- Easier to swap implementations
- Supports interface-based design and testing

# Polymorphism
[GFG - Polymorphism in Java](https://www.geeksforgeeks.org/java/polymorphism-in-java/)
#### Definition of Polymorphism
- Meaning: "many forms"
- Allowing objects to behave differently based on their specific class type
Two types:
1. [[Compile-time]] #todo polymorphism (Static) 
	- [[Method Overloading]] #todo
	- Multiple methods can be defined with the same name but different parameters
2. [[Runtime]] #todo polymorphism (Dynamic)
	-  [[Method Overriding]] #todo 
	- Child class can redefine a method of its parent class

#### How Polymorphism works in Java
> [!example]
> Runtime polymorphism 
> ```java
> public class Animal {
>     public void makeSound() {
>         System.out.println("Some sound");
>     }
> }
> 
> public class Dog extends Animal {
>     @Override
>     public void makeSound() {
>         System.out.println("Bark");
>     }
> }
> 
> public class Cat extends Animal {
>     @Override
>     public void makeSound() {
>         System.out.println("Meow");
>     }
> }
> 
> Animal a1 = new Dog();  // Reference type: Animal; Actual object: Dog
> Animal a2 = new Cat();
> 
> a1.makeSound(); // Bark
> a2.makeSound(); // Meow
> ```
> The method `makeSound()` is overridden.
> The method called depends on the actual object (Dog), not the reference type (Animal).

>[!example]
> Compile-time Polymorphism
> ```java
> public class Calculator {
> 
>     // Method 1
>     public int add(int a, int b) {
>         return a + b;
>     }
> 
>     // Method 2 (overloaded)
>     public double add(double a, double b) {
>         return a + b;
>     }
> 
>     // Method 3 (overloaded)
>     public int add(int a, int b, int c) {
>         return a + b + c;
>     }
> }
> 
> // Usage
> Calculator calc = new Calculator();
> 
> System.out.println(calc.add(2, 3));         // int version → Output: 5
> System.out.println(calc.add(2.5, 3.1));     // double version → Output: 5.6
> System.out.println(calc.add(1, 2, 3));      // three-argument version → Output: 6
> ```
> The **compiler decides** which `add()` method to call based on the **method signature** (parameter types and count). 
> There's no dynamic dispatch here.
#### Benefits of Polymorphism
- Clean code
- Easier to extend functionality
- [[#Abstraction]]
- Dynamic behaviour
	-  Java can select the appropriate method to call at [[runtime]] #todo
