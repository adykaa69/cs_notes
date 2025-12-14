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
	> - **name** only points to it