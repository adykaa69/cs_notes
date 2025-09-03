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
	- We are saying: "My class _[[Inheritance vs Composition#Inheritance (IS-A)|is a]]_ `Thread`" - but `MyThread` is not a thread itself, it just contains a task that should run in a thread (violating [[Inheritance vs Composition#Inheritance (IS-A)|IS-A]] principle)
	- --> **Inheritance**
- Implementing `Runnable`:
	- We are saying: "My class _[[Inheritance vs Composition#Composition (HAS-A)|has a]]_ work that can be executed in a thread." - then we pass that work (the `Runnable`) into a `Thread` object ([[Inheritance vs Composition#Composition (HAS-A)|HAS-A]] principle);
		```java
		Runnable task = () -> System.out.println("Running in a thread!");
		Thread thread = new Thread(task);
		thread.start();
		```
	- --> **Composition** (the thread object _contains_ the task instead of _being_ the task)
- --> 
	- **Composition** is generally more flexible and loosely coupled than **Inheritance**.
	- A `Thread` "has a" `Runnable` is the correct model, not `task` "is a" `Thread`.
#### Inheritance limitation
- In Java, a class can only extend **one** class.
	- Extending `Thread` - losing the ability to extend any other class
	- Implementing `Runnable` - free to extend another class (more flexible)
#### Lambda expressions (Java 8+)
- `Runnable` is a **[[functional interface]]** #todo (it has exactly one abstract method: `run()`)
	- Lambda expressions can be used
		```java
		new Thread(() -> System.out.println("Lambda thread")).start();
		```
#### Conclusion
- Generally implementing `Runnable` is **preferred** over `Thread`
- Use `Thread`:
	- only when you actually need to customize how the thread itself behaves (for example, overriding `start()` or setting up some thread-specific logic)
- In short: **Use `Runnable` for the task, `Thread` for execution.**

### Thread Management API
#### `sleep(milliseconds)`
- Temporarily pauses the current thread
- Throws `InterruptedException` if another thread interrupts it while sleeping
- Useful for simulating delays, scheduling, or waiting without busy-waiting
>[!example] 
>```java
> public class Main {
>     public static void main(String[] args) {
>         Thread t = new Thread(() -> {
>             try {
>                 System.out.println("Working...");
>                 Thread.sleep(2000); // Sleep for 2 seconds
>                 System.out.println("Resumed after sleep.");
>             } catch (InterruptedException e) {
>                 System.out.println("Thread was interrupted!");
>             }
>         });
>         t.start();
>     }
> }
> ```

#### `join()`
- Tells the **calling thread** to wait until another thread finishes execution
- Optional timeout parameter (`join(long millis)`)
> [!example] Example 1 - main + single worker thread
>```java
> public class Main {
>     public static void main(String[] args) throws InterruptedException {
>         Thread worker = new Thread(() -> {
>             try {
>                 Thread.sleep(1000);
>                 System.out.println("Worker finished work");
>             } catch (InterruptedException e) {
>                 e.printStackTrace();
>             }
>         });
> 
>         worker.start();
>         System.out.println("Waiting for worker to finish...");
>         worker.join(); // main thread waits here
>         System.out.println("Main thread continues after worker is done");
>     }
> }
> ```
> ```
> Waiting for worker to finish...
> Worker finished work
> Main thread continues after worker is done
> ```
> `main` is the calling thread, `main` waits until `worker` finishes execution
- `join()` only affects the **thread that calls it** and the **specific thread it is called on**
- In case of multiple threads, they can be joined one by one selectively
> [!example] Example 2 - main + multiple worker threads
>```java
> public class Main {
>     public static void main(String[] args) throws InterruptedException {
>         Thread t1 = new Thread(() -> {
>             try { Thread.sleep(1000); } catch (InterruptedException ignored) {}
>             System.out.println("Worker 1 done");
>         });
> 
>         Thread t2 = new Thread(() -> {
>             try { Thread.sleep(2000); } catch (InterruptedException ignored) {}
>             System.out.println("Worker 2 done");
>         });
> 
>         Thread t3 = new Thread(() -> {
>             try { Thread.sleep(1500); } catch (InterruptedException ignored) {}
>             System.out.println("Worker 3 done");
>         });
> 
>         t1.start();
>         t2.start();
>         t3.start();
> 
>         // Main waits for all workers
>         t1.join();
>         t2.join();
>         t3.join();
> 
>         System.out.println("All workers finished. Main continues.");
>     }
> }
> ```
> ```
> Worker 1 done
> Worker 3 done
> Worker 2 done
> All workers finished. Main continues.
> ```
>  `main` is the calling thread, `main` waits until `t1`, `t2`, `t3` worker threads finish execution
> - The order of workers finishing depends on their **sleep times**
> 	- But `System.out.println("All workers finished. Main continues.")` only happens after all 3 joins complete
- Threads can also wait for each other
> [!example] Example 3 - Joining inside a worker thread 
>```java
> Thread t1 = new Thread(() -> {
>     try {
>         t2.join(); // waits for t2
>         System.out.println("t1 continues after t2");
>     } catch (InterruptedException ignored) {}
> });
> 
> Thread t2 = new Thread(() -> {
>     try {
>         Thread.sleep(1000);
>         System.out.println("t2 finished work");
>     } catch (InterruptedException ignored) {}
> });
> ```
> ```
> t2 finished work
> t1 continues after t2
> ```
> `t1` is the calling thread, `t1` waits until `t2` finishes execution
> > [!warning]
> > It can cause **deadlocks** if two threads wait on each other.

#todo
Thread Management API
Synchronization
Inter-Thread Communication
Java Concurrency Utilities
Volatile vs Synchronized
Common Problems
Best Practices

