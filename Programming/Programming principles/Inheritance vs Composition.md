- [[#Inheritance (IS-A)]]
- [[#Composition (HAS-A)]]

## Inheritance (IS-A)
- **Inheritance** in OOP represents an **"is-a"** **relationship**:
	- If class `B` extends class `A` -> Every object of type `B` is also an object of type `A`
> [!example]
> ```java
> class Animal { }
> class Dog extends Animal { }
> ```
> A `Dog` **"is a(n)"** `Animal`

## Composition (HAS-A)
- **Composition** in OOP represents a "**has-a**" **relationship**
> [!example] 
>```java
> class Tail { }
> class Dog { 
>     private Tail tail;
> }
> ```
> `Dog` "**has a**" `Tail`

