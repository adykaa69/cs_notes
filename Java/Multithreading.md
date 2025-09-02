## Definition of Multithreading
- **_Multithreading_** is a feature that enables the concurrent execution of two or more parts of a program, maximizing **CPU** utilization.
- Each part of such a program is called a thread. So, threads are lightweight processes within a process.

## Multithreading in Java
In Java, threads can be created by using **two** mechanisms: 
### Extending the `Thread` Class
Steps:
1. Create a class that extends the `java.lang.Thread` class.
2. This class overrides the `run()` method available in the `Thread` class.
	- A thread begins its life inside the `run()` method.
3. Create an object of the new class and call the `start()` method to begin execution in a separate thread.
4. The `start()` method internally arranges for the `run()` method to be called on that new thread.
> [!example]
> ```java
> class MyThread extends Thread {
>     public void run() {
>         System.out.println("Running in thread: " + Thread.currentThread().getName());
>     }
> }
> 
> public class Main {
>     public static void main(String[] args) {
>         MyThread t = new MyThread();
>         t.start(); // Never call run() directly
>     }
> }
> ```
### Implementing the `Runnable` Interface
Steps:
1. Create a new class which implements `java.lang.Runnable` interface.
2. Define the `run()` method in the new class.
3. Instantiate a `Thread` object of the new class and call the `start()` method to begin execution in a separate thread.
4. The `start()` method internally arranges for the `run()` method to be called on that new thread.
> [!example]
> ```java
> class MyRunnable implements Runnable {
>     public void run() {
>         System.out.println("Running in thread: " + Thread.currentThread().getName());
>     }
> }
> 
> public class Main {
>     public static void main(String[] args) {
>         Thread t = new Thread(new MyRunnable());
>         t.start(); // Never call run() directly
>     }
> }
> ```
## Thread Management API
- `start()`: The `start()` method internally arranges for the `run()` method to be called on that new thread.
	- "internally arranges" = causes the JVM to execute the thread's `run()`
> [!fail] Wrong way to create new thread
>```java
> public class Main {
>     public static void main(String[] args) {
>         MyThread t = new MyThread();
> 
>         // Correct: starts a new thread
>         t.start();
> 
>         // Wrong: just calls run() in the main thread
>         t.run();
>     }
> }
>    ```
> `start()`: tells the JVM to create a new OS-level thread, then calls `run()` in that new thread.
> `run()`: just a normal method, no new thread is created.

### Thread vs Runnable
#### [[Inheritance vs Composition]]
- Extending `Thread`: 
	- We are saying: "My class _[[is a]]_ `Thread`" - but `MyThread` is not a thread itself, it just contains a task that should run in a thread (violating [[IS-A]] principle)
	- --> **Inheritance**
- Implementing `Runnable`:
	- We are saying: "My class has a work that can be executed in a thread." - then we pass that work (the `Runnable`) into a `Thread` object ([[HAS-A]]);
		```java
		Runnable task = () -> System.out.println("Running in a thread!");
		Thread thread = new Thread(task);
		thread.start();
		```
	- --> **Composition** (the thread object _contains_ the task instead of _being_ the task)
- --> **Composition** is generally more flexible and loosely coupled than **Inheritance**
#### Inheritance limitation
- In Java, a class can only extend **one** class.
	- Extending `Thread` - losing the ability to extend any other class
	- Implementing `Runnable` - free to extend another class (more flexible)
#### Lambda expressions (Java 8+)
- `Runnable` is a **[[functional interface]]** (it has exactly one abstract method: `run()`)
	- Lambda expressions can be used
		```java
		new Thread(() -> System.out.println("Lambda thread")).start();
		```
#### Conclusion
- Generally implementing `Runnable` is **preferred** over `Thread`
- Use `Thread`:
	- only when you actually need to customize how the thread itself behaves (for example, overriding `start()` or setting up some thread-specific logic)
- In short: **Use `Runnable` for the task, `Thread` for execution.**




#todo
Thread vs Runnable
Thread Management API
Synchronization
Inter-Thread Communication
Java Concurrency Utilities
Volatile vs Synchronized
Common Problems
Best Practices