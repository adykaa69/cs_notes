# 1. What is a `String` in Java?
## 1.1 What `String` represents
- In Java, a `String` represents **text** — a sequence of **characters**.
- Examples of a **text** (`String`)
	- "Hello"
	- "Java"
	- "user@email.com"
	- "12345"
>[!note]
> - A `String` is **not a single** character.
> - A `String` is a **sequence** of characters
## 1.2 String is a class, not a primitive
- In Java, primitives are:
	- int
	- double
	- boolean
	- char
- `String` is not a primitive, it is a **class**
	> [!example]
	> ```java
	> String name = "Adi"
	> ```
	> - `String` is a class
	> - `name` is a reference
	> - The actual text lives somewhere in [[**memory**]] #todo
	> 	- **name** only points to it
- This explains many things later topics:
	- [[`==` vs `.equals()`]] #todo 
	- [[immutability]] #todo 
	- [[String Constant Pool]] #todo (SCP)
	- performance issues
## 1.3 String is a reference type
> [!example]
> ```java
> int a = 10;
> int b = a;
> ```
> - `a` and `b` are completely independent

> [!example]
> ```java
> String s1 = "Hello";
> String s2 = s1;
> ```
> - `s1` and `s2` **point** to the same `String` object
> - There is one `"Hello"` object in memory