
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
> ```java
> System.out.println(s1 == s2); // true
> ```
## 2.2 Using constructors