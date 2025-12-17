
# 2. Creating Strings
In Java, there are **multiple ways to create Strings**
## 2.1 String literals
> [!example]
> ```java
> String s1 = "hello";
> String s2 = "hello";
> ```
> - `"hello"` is a String literal
> - It is stored in the [[**String Constant Pool** ]] #todo(SCP)
> - s1 and s2 point to the same object in memory
>> [!note]
>> ```java
>> System.out.println(s1 == s2); // true
>> ```

### Compile time constants
> [!example]
> ```java
> final String HELLO = "hello";
> String s3 = HELLO + " world";
> ```
> - The compiler may combine constants at compile-time
> - No new String object is created at runtime if both parts are constants

#### What does "compile-time constant mean?"
- A **compile-time constant** is a value that the **Java compiler can fully determine at compile time**, without needing the program to run.
- For `String` this means:
	- The value is **known**
	- The value is **`final`**
	- The value is made only from **other compile-time constants**
## 2.2 Using constructors
### 2.2.1 Regular `String` constructor
> [!example]
> ```java
> String s1 = "hello";
> String s4 = new String("hello");
> ```
> - `new String ("hello")` creates a **new `String` object on the [[heap]] #todo**, **not in the [[SCP]] #todo**
>> [!note]
>> ```java
>> System.out.println(s4 == s1); // false
>> ```
> - Rarely needed
> - Usually wasteful in modern Java
### 2.2.2 Other constructors
#### 2.2.2.1 From `char[]`
> [!example]
> ```java
> char[] chars = {'h','e','l','l','o'};
> String s5 = new String(chars);
> ```
> - Converts a character array into a `String`
> - Common when reading low-level APIs
#### 2.2.2.2 From `byte[]`
> [!example]

