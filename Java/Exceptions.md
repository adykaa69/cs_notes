- [[#Exception Hierarchy]]
- [[#Types of Exceptions]]
- [[#Exception Handling]]
- [[#Custom (User-Defined) Exceptions]]
- [[#Methods to print exception info]]
- [[#Errors]]
- [[#GlobalExceptionHandler - @ControllerAdvice]] #todo
<br/>

- [GFG - Java Exception Handling](https://www.geeksforgeeks.org/java/exceptions-in-java/)
- [Baeldung - Exception Handling in Java](https://www.baeldung.com/java-exceptions)
<br/>
## Definition of Exception
- An `Exception` is an unwanted or unexpected event that occurs during the execution of a program (i.e., at [[**runtime**]] #todo) and disrupts the normal flow of the program's instructions.

## Exception Hierarchy
- In Java, all exceptions are objects that inherit from the `Throwable` class.

```java
Throwable
├── Error                         // Serious JVM/system errors (unrecoverable)
│   ├── OutOfMemoryError
│   ├── StackOverflowError
│   ├── VirtualMachineError
│   └── AssertionError
│
└── Exception                     // Exceptions applications might want to catch
    ├── IOException              // Input/output failures
    │   ├── FileNotFoundException
    │   ├── EOFException
    │   └── SocketException
    │
    ├── SQLException             // Database access errors (JDBC)
    ├── ParseException           // Parsing issues (e.g., SimpleDateFormat)
    ├── ClassNotFoundException   // Class loading failure
    ├── InvocationTargetException// Reflection-related issue
    ├── InterruptedException     // Thread interruption
    │
    └── RuntimeException         // Unchecked exceptions (programming bugs)
        ├── NullPointerException
        ├── IllegalArgumentException
        │   ├── NumberFormatException
        ├── ArrayIndexOutOfBoundsException
        ├── ArithmeticException
        ├── IllegalStateException
        ├── ClassCastException
        └── UnsupportedOperationException
```

              ---> Throwable <--- 
              |    (checked)     |
              |                  |
              |                  |
      ---> Exception           Error
      |    (checked)        (unchecked)
      |
	RuntimeException
	(unchecked)

## Types of Exceptions
### Checked Exceptions
- **Checked** at **[[compile-time]]** #todo 
- Must be either caught or **declared** in the **method** signature
- Extend `Exception` (but not `RuntimeException`)
> [!info] Examples
> `IOException`, `SQLException`, `ParseException`

> [!example] 
> ```java
> public void readFile() throws IOException {
>     BufferedReader br = new BufferedReader(new FileReader("file.txt"));
> }
> ```

### Unchecked Exceptions
- **Occur** at **[[runtime]]** #todo
- **Not required** to be **caught** or **declared**
- Extend `RuntimeException`
> [!info] Examples
> `NullPointerException`, `ArrayIndexOutOfBoundsException`, `IllegalArgumentException`, `ArithmeticException`

> [!example]
> ```java
> public int divide(int a, int b) {
>     return a / b; // Might throw ArithmeticException
> }
> ```

## Exception Handling
### Catching exceptions
#### `try-catch` block
- The `try-catch` block is a mechanism to handle exceptions.
	- It catches exceptions so the program does not crash unexpectedly.
- The `try` block contains code that might thrown an exception and the `catch` block is used to handle the exceptions if it occurs.

> [!info] Syntax
>```java
> try {
>     // Code that may throw an exception
> } catch (ExceptionType e) {
>     // Code to handle the exception
> }
> ```

> [!Example] Examples
>> [!example]- Standard try-catch block
>> ```java
>> try {
>>    int result = 10 / 0;
>> } catch (ArithmeticException e) {
>>   System.out.println("You can't divide by zero!");
>> }
>> ```
>
>> [!example]- Multiple try-catch blocks
>> ```java
>> try {
>>     String text = null;
>>     System.out.println(text.length());
>> } catch (NullPointerException e) {
>>     System.out.println("Null pointer caught");
>> } catch (Exception e) {
>>     System.out.println("Other exception");
>> }
>> ``` 
>
>> [!example]- Union catch blocks (Java 7+)
>> Use `|` to catch multiple exceptions with the same handling logic.
>> ```java
>> try {
>>     // some code
>> } catch (IOException | SQLException e) {
>>     e.printStackTrace();
>> }
>> ```
#### `finally` block
- The finally block is used to execute important code regardless of whether an exception occurs or not.
- `finally` block is **always** executes **after** the` try-catch` block. 
- Also used for resource cleanup.

> [!info] Syntax
> ```java
> try {
>     // Code that may throw an exception
> } catch (ExceptionType e) {
>     // Code to handle the exception
> } finally {
>     // cleanup code
> }
> ```

> [!example]
> Use `finally` for **resource cleanup** (closing files, DB connections, etc)..
> ```java
> FileReader reader = null;
> try {
>     reader = new FileReader("data.txt");
>     // read file
> } catch (IOException e) {
>     System.out.println("File error: " + e.getMessage());
> } finally {
>     if (reader != null) {
>         try {
>             reader.close();
>         } catch (IOException e) {
>             e.printStackTrace();
>         }
>     }
> }
> ```
>> [!note] 
>> When working with resources that implement the `AutoCloseable` interface, the resource will be **auto-closed**. They are called ***[[try-with-resources]]*** #todo.
>> [Baeldung - Try with Resources](https://www.baeldung.com/java-try-with-resources)

### Throwing and Declaring exceptions
#### TL;DR
- `throw` = You **throw** a single exception from a method.
- `throws` = You **declare** possible exceptions that a method might throw.

```java
// throw: actually throwing the exception
throw new IllegalArgumentException("Bad input");

// throws: declaring that this method might throw an exception
public void readFile() throws IOException
```
#### `throw`
- Use `throw` to **explicitly create and throw an exception**.
- You can only `throw` objects that are [[#Exception Hierarchy|instances of]] `Throwable` or its subclasses.

> [!info] Syntax
> ```java
> throw new ExceptionType("Error message");
> ```

> [!example]
> ```java
> public void validateAge(int age) {
>     if (age < 18) {
>         throw new IllegalArgumentException("Age must be 18 or older");
>     }
> }
> ```

#### `throws`
- Used in a method signature to **declare** that the method might throw a checked exception.
- The simplest way to "handle" an exception is to **rethrow** it.
	- This way you can **propagate** the responsibility to handle the exception to the calling method (**[Exception Propagation in Java](https://www.geeksforgeeks.org/java/exception-propagation-java/)**).

> [!info] Syntax
> ```java
> public void method() throws ExceptionType {
>     // code that may throw ExceptionType
> }
> ```

> [!example]
> ```java
> public void readFile(String filename) throws FileNotFoundException {
>     FileReader reader = new FileReader(filename);
> }
> ```
> - The compiler **requires** you to declare `throws` or [[#Catching exceptions|catch]] it.
> - Since `FileNotFoundException` is a **[[#Checked Exceptions|checked]]** exception, this is the simplest way to satisfy the compiler.
>	- But it does mean that **anyone that calls this method now needs to handle it too**.
> > [!note]
> >```java
> > public void divide(int a, int b) {
>>     int result = a / b; // Might throw ArithmeticException - Unchecked Exception
>> }
>> ```
>> Even though `ArithmeticException` ([[#Unchecked Exceptions|unchecked]]) may be thrown at runtime, the **compiler doesn’t force you to** propagate the exception.

## Custom (User-Defined) Exceptions
### Benefit of custom exceptions
- Java exceptions cover almost all general exceptions that are bound to happen in programming.
- Create custom exceptions to add semantic meaning to your application-specific exceptions.
	- Specific business logic exceptions (domain exceptions)
	- To catch and provide specific treatment to a subset of existing Java exceptions
	- To add clear, descriptive error messages for better debugging.
	- To group related exceptions together.
### Checked custom exception
> [!example]
> ```java
> public class InvalidAgeException extends Exception {
>     public InvalidAgeException(String message) {
>         super(message);
>     }
> }
> ```
> **[[#Checked Exceptions|Checked]]** custom exception `extends` `Exception`
> ```java
> public void register(int age) throws InvalidAgeException {
>     if (age < 18) {
>         throw new InvalidAgeException("Must be 18+ to register");
>     }
> }
> ```
### Unchecked custom exception
> [!example]
> ```java
> public class BusinessRuleException extends RuntimeException {
>     public BusinessRuleException(String message) {
>         super(message);
>     }
> }
> ```
> **[[#Unchecked Exceptions|Unchecked]]** custom exception `extends` `RuntimeException`
> ```
> public void setPrice(double price) {
>     if (price < 0) {
>         throw new BusinessRuleException("Price must be non-negative");
>     }
> }
> ```

## Methods to print exception info
### `getMessage()`
`java.lang.Throwable.getMessage()`
Returns the exception's **detail message** (the one passed to the constructor).
```java
try {
    throw new IllegalArgumentException("Invalid input");
} catch (IllegalArgumentException e) {
    System.out.println(e.getMessage()); 
    // → "Invalid input"
}
```
### `toString()`
Returns a string that includes the **exception class name** and the message.
```java
try {
    throw new NullPointerException("value was null");
} catch (NullPointerException e) {
    System.out.println(e.toString());
    // OR
    // System.out.println(e);
    // → java.lang.NullPointerException: value was null
}
```
### `printStackTrace()`
`java.lang.Throwable.printStackTrace()`
Prints the **full call stack** to `System.err`. 
- stack trace: wherein the code, that exception has occurred
	- which methods were called, and in which line the error occurred
```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    e.printStackTrace();
}
```
## Errors
- An `Error` in Java is a **serious, unrecoverable issue** that occurs **at runtime**, typically related to the [[JVM]] #todo or system resources.
	- Unlike `Exception`, **you’re not supposed to handle `Error` objects** in your code.
	- They extend `Error`, not `Exception`
```java
Throwable
├── Error                         
│   ├── OutOfMemoryError
│   ├── StackOverflowError
│   ├── VirtualMachineError
│   └── AssertionError
```

### `OutOfMemoryError`
Occurs when the [[JVM]] #todo runs out of heap memory.
```java
List<int[]> memoryEater = new ArrayList<>();
while (true) {
    memoryEater.add(new int[10_000_000]);
}
```
This stimulates a memory leak. JVM will crash with:
```java
java.lang.OutOfMemoryError: Java heap space
```
### `StackOverflowError`
Caused by **infinite recursion** – the call stack overflows.
Stack is a memory region that stores **method call frames**. Too many → `StackOverflowError`.
```java
public void recurse() {
    recurse(); // Infinite recursion
}
```

## GlobalExceptionHandler - @ControllerAdvice
**TODO**
- [ ] GlobalExceptionHandler - @ControllerAdvice #todo
