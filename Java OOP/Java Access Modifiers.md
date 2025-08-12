- [[#Default]]
- [[#Public]]
- [[#Private]]
- [[#Protected]]
<br/>

## Access Modifiers
- Tools that define how the members of a class, like variables, methods and even the class itself can be accessed from the other parts of the program

### Access Modifier Compatibility Table
|Element|`public`|`protected`|_default_|`private`|
|---|---|---|---|---|
|**Top-level classes**|✅ Yes|❌ No|✅ Yes|❌ No|
|**Nested classes**|✅ Yes|✅ Yes|✅ Yes|✅ Yes|
|**Fields**|✅ Yes|✅ Yes|✅ Yes|✅ Yes|
|**Methods**|✅ Yes|✅ Yes|✅ Yes|✅ Yes|
|**Constructors**|✅ Yes|✅ Yes|✅ Yes|✅ Yes|

### Default
- No keyword required - when no access modifier is specified it is said to have the **default** access modifier by default
- Only classes **within the same package** can access it
	- Also called **package-private** modifier

> [!example]
> `Animal` has `default` access modifier. 
> It is in the `com.zoo` package.
> ```java
> // File: com/zoo/Animal.java
> package com.zoo;
> 
> class Animal { // No modifier = default access
>     void speak() {
>         System.out.println("Animal sound");
>     }
> }
> ```
> `Zoo` is also in the `com.zoo` package so it can access `Animal`.
> ```java
> // File: com/zoo/Zoo.java
> package com.zoo;
> 
> public class Zoo {
>     public static void main(String[] args) {
>         Animal animal = new Animal();  // Allowed — same package
>         animal.speak();                // Allowed — same package
>     }
> }
> ```
> `Visitor` in a different package - `other`.
> It **cannot** access `Animal`. 
> ```java
> // File: other/Visitor.java
> package other;
> 
> import com.zoo.Animal;
> 
> public class Visitor {
>     public static void main(String[] args) {
>         Animal animal = new Animal(); // Error: Animal is not public
>     }
> }
> ```
#### Use cases of default
- Internal utility classes
- Package-scoped logic 
- Tests or mocks inside the same package

### Public
- **Visible everywhere** - in all packages and all classes

> [!example]
> `Animal` and `Visitor` are in different packages.
> ```java
> // File: com/zoo/Animal.java
> package com.zoo;
> 
> public class Animal {
>     public void makeSound() {
>         System.out.println("Generic animal sound");
>     }
> }
> ```
> 
> ```java
> // File: other/Visitor.java
> package other;
> 
> import com.zoo.Animal;
> 
> public class Visitor {
>     public static void main(String[] args) {
>         Animal animal = new Animal(); // OK - class is public
>         animal.makeSound();           // OK — method is public
>     }
> }
> ```
> The class and method are `public`, so they’re visible **across packages**.
#### Use cases of public
- Full access to a method/class/field from outside
- API to be externally usable
- Exposing stable, safe functionality

### Private
- Visible only in the same class
- Cannot be accessed from subclasses or other classes (even in the same package)

> [!example]
> ```java
> // File: com/zoo/Animal.java
> package com.zoo;
> 
> public class Animal {
>     private void breathe() {
>         System.out.println("Breathing...");
>     }
> 
>     public void test() {
>         breathe(); // OK: called inside the same class
>     }
> }
> ```
> 
> ```java
> // File: com/zoo/Zoo.java
> package com.zoo;
> 
> public class Zoo {
>     public static void main(String[] args) {
>         Animal animal = new Animal();
>         animal.breathe();  // ERROR: breathe() has private access
>         animal.test();     // OK: public method can call private one internally
>     }
> }
> ```
> Only `test()` (`public`) is accessible from the `Animal` class, `breathe()` (`private`) cannot be accessed (even though `Animal` and `Zoo` are in the same package)
#### Use case of private
- [[The Four Pillars of OOP#Encapsulation|Encapsulation]]
- Hiding implementation details
- Preventing direct access to critical variables

### Protected
- Visible in the same [[package]] #todo
- Visible in [[subclasses]] #todo (even in other packages)

> [!example]
> Same package access
> ```java
> // File: com/zoo/Animal.java
> package com.zoo;
> 
> public class Animal {
>     protected void eat() {
>         System.out.println("Animal is eating...");
>     }
> }
> ```
> 
> ```java
> // File: com/zoo/Zoo.java
> package com.zoo;
> 
> public class Zoo {
>     public static void main(String[] args) {
>         Animal animal = new Animal();
>         animal.eat();  // OK: protected is accessible within the same package
>     }
> }
> ```
> `eat()` has `protected` access - accessible from the same package

> [!example]
> Subclass in another package
>```java
> // File: com/zoo/Animal.java
> package com.zoo;
> 
> public class Animal {
>     protected void eat() {
>         System.out.println("Animal is eating...");
>     }
> }
>```
> ```java
> // File: wild/WildAnimal.java
> package wild;
> 
> import com.zoo.Animal;
> 
> public class WildAnimal extends Animal {
>     public void hunt() {
>         eat();  // OK: subclass can access protected method, even from a different package
>     }
> }
> ```
> `eat()` has `protected` access - subclass can access `protected` method of superclass
> 
> ```java
> // File: wild/Safari.java
> package wild;
> 
> import com.zoo.Animal;
> 
> public class Safari extends Animal {
>     public static void main(String[] args) {
>         Animal animal = new Animal();
>         animal.eat();  //  ERROR: 'eat()' has protected access in 'Animal'
>     }
> }
> ```
> Even though `Safari` is a subclass of `Animal`, the method `eat()` is being accessed **through a superclass reference** (`animal`), **not via inheritance**.

- In Java `protected` access **from another package** is allowed only:
	- **within subclass**
	- **only via `this` or inherited members**, not via **any arbitrary instance** of the superclass
- You can only access protected members **through `this`** or **through inherited behavior**, **not via superclass instances**, even inside a subclass.

This works:
```java
this.eat();
super.eat();
```

This does not work:
```java
Animal a = new Animal();
a.eat(); // ERROR
```

#### Use cases of protected
- Allow access to subclasses (even across packages)
- Prefer `private` + `getter`/`setter` unless subclass needs access 