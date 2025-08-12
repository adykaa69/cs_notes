[Baeldung - SOLID Principles](https://www.baeldung.com/solid-principles)
- Introduced by Robert C. Martin (Uncle Bob), later built upon by Michael Feathers
- Encourage to build a software that is more
	- Maintainable
	- Understandable
	- Flexible 

- [[#Single Responsibility Principle]]
- [[#Open/Closed Principle]]
- [[#Liskov Substitution Principle]]
- [[#Interface Segregation Principle]]
- [[#Dependency Inversion]] 

## Single Responsibility Principle

### Definition of SRP
- A class should only have one reason to change - only one responsibility/function
- If a class is doing multiple things, changes in one responsibility could affect or break the others
### Benefits of SRP
- Easier to test
- Easier to maintain
- More readable code, clearer organization
- Reusable components
### SRP in Java
> [!fail] Bad Example
> ```java
> public class UserManager {
> 
>     public void addUser(String username) {
>         // logic to add user
>         System.out.println("User added: " + username);
>     }
> 
>     public void sendEmail(String email, String message) {
>         // logic to send email
>         System.out.println("Email sent to: " + email + ", message: " + message);
>     }
> 
>     public void generateUserReport() {
>         // logic to generate report
>         System.out.println("User report generated");
>     }
> }
> ```
> Problem:
> - `UserManager` has **three responsibilites**:
> 	1. Managing user data (`addUser()`)
> 	2. Sending emails (`sendEmail()`)
> 	3. Generating reports (`generateUserReport()`)
> --> **Violates SRP** 

> [!success] Good Example
> UserService.java
> ```java
> public class UserService {
>     public void addUser(String username) {
>         System.out.println("User added: " + username);
>     }
> }
> ```
> EmailService.java
> ```java
> public class EmailService {
>     public void sendEmail(String email, String message) {
>         System.out.println("Email sent to: " + email + ",  message: " + message);
>     }
> }
> ```
>  ReportService.java
> ```java
> public class ReportService {
>     public void generateUserReport() {
>         System.out.println("User report generated");
>     }
> }
> ```
> Optional: UserController.java - Coordinating class (facade)
> ``` java
> public class UserController {
>     private final UserService userService = new UserService();
>     private final EmailService emailService = new EmailService();
>     private final ReportService reportService = new ReportService();
> 
>     public void onboardUser(String username, String email) {
>         userService.addUser(username);
>         emailService.sendEmail(email, "Welcome " + username + "!");
>         reportService.generateUserReport();
>     }
> }
> ```
> Splitting the responsibilites into focused classes

## Open/Closed Principle
### Definition of OCP
- Software entities should be **open for extension** but **closed for modification**.
	- You should be able to extend a class behaviour without modifying it.
	- Add new functionality by adding new code, not by changing existing code.
### Benefits of OCP
- Avoid breaking existing functionality
- Make the system more **robust** and **scalable**

### OCP in Java
> [!fail] Bad Example
> ```java
> public class NotificationService {
>     public void send(String type, String message) {
>         if (type.equals("email")) {
>             // send email
>         } else if (type.equals("sms")) {
>             // send SMS
>         }
>     }
> }
> ```
> You need to **modify the method** to add new notification types.

> [!success] Good Example
> ```java
> public interface NotificationSender {
>     void send(String message);
> }
> 
> public class EmailSender implements NotificationSender {
>     public void send(String message) { /* send email */ }
> }
> 
> public class SmsSender implements NotificationSender {
>     public void send(String message) { /* send SMS */ }
> }
> 
> public class NotificationService {
>     private List<NotificationSender​> senders;
> 
>     public NotificationService(List<NotificationSender​​> senders) {
>         this.senders = senders;
>     }
> 
>     public void sendAll(String message) {
>         for (NotificationSender sender : senders) {
>             sender.send(message);
>         }
>     }
> }
> ```
> Added new functionality by **extending existing code** (implementing the `NotificationSender` interface)

## Liskov Substitution Principle
### Definition of LSP
- Subtypes must be substitutable for their base types without altering the correctness of the program.
	- Simply put, if class _A_ is a subtype of class _B_, we should be able to replace _B_ with _A_ without disrupting the behaviour of our program.
### Benefits of LSP
- Prevents unexpected behaviour
- Supports robust programming
### LSP in Java
> [!fail] Bad Example
> ```java
> class Bird {
>     void fly() { /*...*/ }
> }
> 
> class Ostrich extends Bird {
>     void fly() { throw new UnsupportedOperationException(); }
> }
> ```
> `Ostrich` cannot fly - substituting it for a `Bird` breaks expectations.

> [!success] Good Example
> ```java
> interface Bird { }
> interface FlyingBird extends Bird {
>     void fly();
> }
> 
> class Sparrow implements FlyingBird {
>     public void fly() { /*...*/ }
> }
> 
> class Ostrich implements Bird {
>     // no fly()
> }
> ```

## Interface Segregation Principle
### Definition of ISP
- Clients should not be forced to depend on interfaces they do not use.
- Larger interfaces should be split into smaller ones.
	-  --> By doing so, we can ensure that implementing classes only need to be concerned about the methods that are of interest to them.
- Small, focused interfaces should be designed, instead of large, general-purpose ones.
### Benefits of ISP
- Avoid unnecessary implementation burden
- Ensure cohesion and readability
### ISP in Java
> [!fail] Bad Example
> ```java
> public interface Machine {
>     void print();
>     void scan();
>     void fax();
> }
> 
> public class OldPrinter implements Machine {
>     public void print() { /*...*/ }
>     public void scan() { throw new UnsupportedOperationException(); }
>     public void fax() { throw new UnsupportedOperationException(); }
> }
> ```
> `OldPrinter` implements `Machine`, so `scan()` and `fax()` are unnecessary but required methods to be implemented.

> [!success] Good Example
> ```java
> public interface Printer {
>     void print();
> }
> 
> public interface Scanner {
>     void scan();
> }
> 
> public interface Faxer {
> 	void fax();
> }
> 
> public class OldPrinter implements Printer {
>     public void print() { /*...*/ }
> }
> ```

## Dependency Inversion
### Definition of DI
- This principle of **[[Dependency Inversion]]** #todo refers to the **decoupling** of software modules:
	- High-level modules should not depend on low-level modules.
		- Both should depend on **abstractions**.
	- Depend on interfaces (abstractions), not on concrete implementations.
>[!note] 
>- *Dependency Inversion* is often confused *Dependency Injection*, but they are closely related.
> - In Java, frameworks like **[[Spring]]** #todo help achieve dependency inversion via **dependency injection**.
### Benefits of DI
- [[Loose coupling]] #todo
- Enable easier **testing**, **flexibility** and **extensibility**
### DI in Java
> [!fail] Bad Example
> ```java
> public class ReportService {
>     private final MySQLReportRepository repository = new MySQLReportRepository();
> 
>     public void generateReport() {
>         repository.saveReport("Generated report");
>     }
> }
> ```
> **Tightly coupled**
> - By declaring the  `ReportRepository` with the `new` keyword, we’ve tightly coupled these two classes together.
> 	- `ReportService` **depends on** a **specific implementation** (`MySQLReportRepository`).
> - This make `ReportService` hard to replace, mock or test.
> - Lost the ability to switch out `ReportRepository` class with a different one if needed

> [!success] Good Example
> ```java
> public interface ReportRepository {
>     void saveReport(String report);
> }
> 
> public class MySQLReportRepository implements ReportRepository {
>     public void saveReport(String report) {
>         // save to MySQL DB
>     }
> }
> 
> public class ReportService {
>     private final ReportRepository repository;
> 
>     public ReportService(ReportRepository repository) {
>         this.repository = repository;
>     }
> 
>     public void generateReport() {
>         repository.saveReport("Generated report");
>     }
> }
> ```
> **Loosely coupled**
> - `ReportService` depends only on the **interface** `ReportRepository`.
> - Easily **switchable** to other implementations (e.g., `InMemoryReportRepository` for testing).

> [!example]
> Dependency Inversion in Spring Boot (with **Dependency Injection**)
> ```java
> @Service
> public class ReportService {
>     private final ReportRepository repository;
> 
>     @Autowired
>     public ReportService(ReportRepository repository) {
>         this.repository = repository;
>     }
> }
> ```
> Spring looks for a bean that implements `ReportRepository` (e.g., `MySQLReportRepository`) and **injects it**.