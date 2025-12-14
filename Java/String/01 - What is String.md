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
> Primitive type
> ```java
> int a = 10;
> int b = a;
> ```
> - `a` and `b` are completely independent

> [!example] 
> Reference type
> ```java
> String s1 = "Hello";
> String s2 = s1;
> ```
> - `s1` and `s2` **point** to the same `String` object
> - There is one `"Hello"` object in memory

## 1.4 Immutability
- One of the most important properties of `String`:
	- Once a `String` object is created, it can never change.
	- --> `String` is **immutable**
[!example]
> ```java
> String s = "Hello";
> s = s + "World"; 
> ```
> - What happens?
> 	- `"Hello"` is not modified
> 	- A new `String`: `"Hello World"` is created
> 	- `s` now **points** to the new object
> - Why is this important?
> 	- [[Thread safety]] #todo 
> 	- Security (passwords, URLs, class names)
> 	- String pool optimization

More on [[immutability]] #todo

## 1.6 String integration in Java
- `String` is not just “text”. As everything in Java, it is a Class and it’s deeply integrated into Java

### 1.6.1 `String` implements important interfaces
> [!note]
> ```java
> public final class String
>     implements Serializable, Comparable<String>, CharSequence
> ```

#### 1.6.1.1 `CharSequence`
- `String` implements `CharSequence`
	- It means it behaves like a sequence of characters
- Methods:
	- length()
	- charAt()
	- subSequence()
- Other classes also implement `CharSequence`:
	- [[StringBuilder]] #todo 
	- [[StringBuffer]] #todo 
	
#### 1.6.1.2 `Comparable<String>`
- `String` implements `Comparable<String>`
	- It means Strings can be compared and sorted
> [!example]
> ```java
> "apple".compareTo("banana"); // negative  
> ```
> - negative -> `"apple"` comes before `"banana"`

- Used in:
	- Sorting
	- TreeMap / TreeSet
	- Ordering Logic

#### 1.6.1.3 `Serializable`
- `String` implements `Serializable`
	- It means String can be written to:
		- Files
		- Network
		- JSON
		- Database
	- Without special handling
- Crucial for:
	- Rest APIs
	- Logging
	- Persistence
### 1.6.2 `String` is `final`
- continue: https://chatgpt.com/c/693e8a6c-5dcc-8327-8eec-0d6a62a679ec