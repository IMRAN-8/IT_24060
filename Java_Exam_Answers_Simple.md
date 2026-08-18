# ☕ Java Final Exam — Simple Answers (L1–L12, All 36 Questions)

> Written in plain, easy language. Practical answers include short, complete Java code.

---

## 📘 L1 — Encapsulation and Polymorphism

**1.1 — What is encapsulation?**
Encapsulation means locking your data (fields) inside a class and only allowing access through controlled methods (getters/setters). You mark fields `private` so no one outside the class can touch them directly. If they want to read or change the value, they must go through your `public` methods, where you can add checks.
**Real-life analogy:** A medicine capsule. The medicine (data) is sealed inside a shell. You can't touch the medicine directly — you only interact with the capsule as a whole (the public interface).

**1.2 — What is polymorphism?**
Polymorphism means "one name, many forms" — the same method name behaves differently depending on the situation.
- **Compile-time (static) polymorphism** = method **overloading**. The compiler decides which method to run based on the parameters, before the program even runs.
  Example: `add(int a, int b)` vs `add(double a, double b)`.
- **Run-time (dynamic) polymorphism** = method **overriding**. The JVM decides which method to run while the program is actually running, based on the real object type.
  Example: `Animal a = new Dog(); a.sound();` — calls Dog's version even though `a` is declared as `Animal`.

**1.3 — Practical: BankAccount class**
```java
class BankAccount {
    private double balance;

    public double getBalance() {
        return balance;
    }

    // Overload 1
    public void deposit(double amount) {
        balance += amount;
        System.out.println("Deposited: " + amount);
    }

    // Overload 2 (polymorphism - compile-time)
    public void deposit(double amount, String remarks) {
        balance += amount;
        System.out.println("Deposited: " + amount + " | Remarks: " + remarks);
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount acc = new BankAccount();
        acc.deposit(500);                       // calls deposit(double)
        acc.deposit(1000, "Salary credit");     // calls deposit(double, String)
        System.out.println("Balance: " + acc.getBalance());
    }
}
```

---

## 📘 L2 — Method Overloading vs Overriding (Early vs Late Binding)

**2.1 — Overloading vs Overriding**

| Point | Overloading | Overriding |
|---|---|---|
| Definition | Same method name, different parameters | Same method name & parameters, redefined in a subclass |
| Class involved | Same class | Parent and child class |
| Parameters | Must be different | Must be the same |
| Return type | Can differ | Must be same or a subtype (covariant) |
| Binding time | Compile time (early binding) | Run time (late binding) |

**2.2 — Early vs Late binding**
- **Early binding**: The compiler already knows exactly which method to call, just by looking at the method signature. This happens with overloading, `static`, `private`, and `final` methods.
- **Late binding**: The compiler doesn't know the final method to call in advance — the JVM checks the actual object at run time and calls the right version. This happens with overriding.
Overriding needs late binding because the same reference type (e.g. `Shape`) can point to different objects (`Circle`, `Rectangle`) — Java can't know which one until the program is actually running.

**2.3 — Practical: Shape hierarchy**
```java
class Shape {
    public double area() {
        return 0;
    }

    // Overloading (early binding)
    public void describe(String name) {
        System.out.println("Shape: " + name);
    }

    public void describe(String name, int sides) {
        System.out.println("Shape: " + name + ", Sides: " + sides);
    }
}

class Circle extends Shape {
    private double radius;
    public Circle(double radius) { this.radius = radius; }
    @Override
    public double area() { return Math.PI * radius * radius; }
}

class Rectangle extends Shape {
    private double length, width;
    public Rectangle(double length, double width) {
        this.length = length; this.width = width;
    }
    @Override
    public double area() { return length * width; }
}

public class Main {
    public static void main(String[] args) {
        Shape s1 = new Circle(5);       // late binding
        Shape s2 = new Rectangle(4, 6); // late binding

        System.out.println("Circle Area: " + s1.area());
        System.out.println("Rectangle Area: " + s2.area());

        s1.describe("Circle");             // early binding
        s2.describe("Rectangle", 4);       // early binding
    }
}
```
**Sample output:**
```
Circle Area: 78.53981633974483
Rectangle Area: 24.0
Shape: Circle
Shape: Rectangle, Sides: 4
```

---

## 📘 L3 — Abstract Class vs Interface

**3.1 — Definitions & differences**
- **Abstract class**: A class that cannot be instantiated directly. It can mix finished (concrete) methods with unfinished (abstract) methods that subclasses must complete.
- **Interface**: A contract listing method signatures with no body (mostly). Any class that implements it must fill in those methods.

| Aspect | Abstract Class | Interface |
|---|---|---|
| Fields | Can have normal instance variables | Only `public static final` constants |
| Constructors | Yes, can have one | No |
| Method bodies | Mix of implemented + abstract methods | Only default/static methods have bodies |
| Multiple inheritance | A class can extend only ONE abstract class | A class can implement MANY interfaces |

**3.2 — When to choose which**
- Use an **abstract class** for an "is-a" relationship where subclasses share common code. Example: `Vehicle` is a base for `Car`, `Bike` — they all share `startEngine()` logic.
- Use an **interface** for a "can-do" capability that unrelated classes might share. Example: `Insurable` — a `Car`, a `House`, or even a `Person` could all be insurable, even though they're not related by inheritance.

**3.3 — Practical: Vehicle / Insurable / Car**
```java
abstract class Vehicle {
    public void startEngine() {
        System.out.println("Engine started...");
    }
    public abstract String fuelType(); // must be implemented by subclass
}

interface Insurable {
    double calculatePremium();
}

class Car extends Vehicle implements Insurable {
    @Override
    public String fuelType() {
        return "Petrol";
    }

    @Override
    public double calculatePremium() {
        return 15000.0;
    }
}

public class Main {
    public static void main(String[] args) {
        Car car = new Car();
        car.startEngine();                          // inherited concrete method
        System.out.println("Fuel: " + car.fuelType());
        System.out.println("Premium: " + car.calculatePremium());
    }
}
```
*Why abstract class for `Vehicle`:* it's clearly an "is-a" relationship, and `startEngine()` is shared code we don't want to rewrite in every subclass.
*Why interface for `Insurable`:* being insurable is a capability, not a type of vehicle — other unrelated classes could implement it too.

---

## 📘 L4 — Collection Framework

**4.1 — ArrayList vs Vector vs LinkedList**

| Feature | ArrayList | Vector | LinkedList |
|---|---|---|---|
| Structure | Resizable array | Resizable array | Doubly linked list |
| Synchronized (thread-safe) | No | Yes | No |
| Random access (get by index) | Fast | Fast | Slow (must walk the chain) |
| Insert/delete (middle) | Slow (shifts elements) | Slow | Fast (just relinks pointers) |

**4.2 — Set and its implementations**
A `Set` is a collection that does **not allow duplicate** elements.
- `HashSet`: no guaranteed order, fastest.
- `LinkedHashSet`: keeps insertion order.
- `TreeSet`: keeps elements sorted automatically.
`TreeSet` maintains order using a **Red-Black Tree** internally — every time you add an element, it's placed in the correct sorted position (using natural ordering via `Comparable`, or a custom `Comparator`).

**4.3 — Practical: ArrayList vs TreeSet**
```java
import java.util.ArrayList;
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("David");
        list.add("Alice");
        list.add("Ethan");
        list.add("Bob");
        list.add("Carol");

        System.out.println("ArrayList (insertion order):");
        for (String name : list) {
            System.out.println(name);
        }

        TreeSet<String> set = new TreeSet<>(list);
        System.out.println("\nTreeSet (sorted order):");
        for (String name : set) {
            System.out.println(name);
        }
    }
}
```
**Comment:** The `ArrayList` prints names in the exact order they were added. The `TreeSet` automatically sorts them alphabetically and would also remove any duplicates.

---

## 📘 L5 — Multithreading & Custom Exception Handling

**5.1 — Ways to implement multithreading**
1. **Extending `Thread`** and overriding `run()`.
2. **Implementing `Runnable`** and passing it to a `Thread` object.
3. **Using `ExecutorService`/`Callable`** — a thread pool manages threads for you, and `Callable` can return a result.
**Preferred:** `Runnable` (or `ExecutorService`) is generally preferred, because Java only allows single inheritance — if your class extends `Thread`, it can't extend anything else. `Runnable` keeps your class free to extend other classes, and separates "the task" from "the thread that runs it."

**5.2 — Practical: two threads printing 1–5**
```java
class MyThread extends Thread {
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Thread(extends): " + i);
        }
    }
}

class MyRunnable implements Runnable {
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Thread(runnable): " + i);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        Thread t2 = new Thread(new MyRunnable());

        t1.start();
        t2.start();
    }
}
```

**5.3 — Practical: custom checked exception**
```java
class InvalidRadiusException extends Exception {
    public InvalidRadiusException(String msg) {
        super(msg);
    }
}

class Circle {
    private double radius;

    public Circle(double radius) throws InvalidRadiusException {
        if (radius < 0) {
            throw new InvalidRadiusException("Radius cannot be negative: " + radius);
        }
        this.radius = radius;
    }

    public double area() {
        return Math.PI * radius * radius;
    }
}

import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter radius: ");
        double r = sc.nextDouble();

        try {
            Circle c = new Circle(r);
            System.out.println("Area: " + c.area());
        } catch (InvalidRadiusException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```
*Note: in real Java, the `import` line must be at the very top of the file, above the classes — it's shown here next to `Main` just for grouping.*

---

## 📘 L6 — JDBC with MySQL/Oracle (MVC Pattern)

**6.1 — Steps to connect Java to a database via JDBC**
1. Load/register the JDBC driver (usually automatic since JDBC 4.0).
2. Open a connection: `Connection con = DriverManager.getConnection(url, user, password);`
3. Create a statement: `PreparedStatement ps = con.prepareStatement(sql);`
4. Run the query/update: `ps.executeQuery()` or `ps.executeUpdate()`.
5. Process results using `ResultSet` (for SELECT queries).
6. Close the connection.
**Key classes:** `DriverManager` (creates connections), `Connection` (represents the DB session), `PreparedStatement` (runs safe, parameterized SQL), `ResultSet` (holds query results).

**6.2 — MVC pattern in a JDBC app**
MVC splits an app into three parts:
- **Model** — the plain data class (e.g. `Student`) representing one record.
- **View** — whatever collects input and shows output to the user (e.g. `Main`, or a JSP page).
- **Controller** — the class that handles logic and talks to the database (e.g. `StudentDAO`), sitting between Model and View.

**6.3 — Practical: MVC-style Student insert**
```java
import java.sql.*;
import java.util.*;

// MODEL
class Student {
    private int id;
    private String name;
    private double cgpa;

    public Student(int id, String name, double cgpa) {
        this.id = id; this.name = name; this.cgpa = cgpa;
    }
    public int getId() { return id; }
    public String getName() { return name; }
    public double getCgpa() { return cgpa; }
}

// CONTROLLER / DAO
class StudentDAO {
    public void insert(Student s) throws SQLException {
        String url = "jdbc:mysql://localhost:3306/student_db";
        try (Connection con = DriverManager.getConnection(url, "root", "password");
             PreparedStatement ps = con.prepareStatement(
                 "INSERT INTO Students (id, name, cgpa) VALUES (?, ?, ?)")) {
            ps.setInt(1, s.getId());
            ps.setString(2, s.getName());
            ps.setDouble(3, s.getCgpa());
            ps.executeUpdate();
        }
    }
}

// VIEW
public class Main {
    public static void main(String[] args) throws SQLException {
        Scanner sc = new Scanner(System.in);
        System.out.print("ID: ");
        int id = sc.nextInt();
        System.out.print("Name: ");
        String name = sc.next();
        System.out.print("CGPA: ");
        double cgpa = sc.nextDouble();

        Student s = new Student(id, name, cgpa);
        new StudentDAO().insert(s);
        System.out.println("Student saved!");
    }
}
```

---

## 📘 L7 — JavaFX — House Loan Calculator

**7.1 — JavaFX application structure**
- `Application` — the base class every JavaFX app extends; JavaFX calls its `start()` method to launch the GUI.
- `Stage` — the actual window on screen.
- `Scene` — the content that fills the window (like a canvas holding all your UI elements).
- `GridPane`/`VBox` — layout containers that arrange buttons, labels, and text fields (grid-style vs vertical-stack style).
**Role of `start()`:** It's the entry point JavaFX calls automatically after launch — this is where you build the Scene, set up the layout, add event handlers, and show the Stage.

**7.2 — Practical: GridPane layout**
```java
import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.GridPane;
import javafx.stage.Stage;

public class LoanCalculator extends Application {
    TextField loanField = new TextField();
    TextField rateField = new TextField();
    TextField yearsField = new TextField();
    Label resultLabel = new Label();

    @Override
    public void start(Stage stage) {
        GridPane grid = new GridPane();
        grid.setPadding(new Insets(10));
        grid.setHgap(10);
        grid.setVgap(10);

        grid.add(new Label("Loan Amount:"), 0, 0);
        grid.add(loanField, 1, 0);
        grid.add(new Label("Annual Rate (%):"), 0, 1);
        grid.add(rateField, 1, 1);
        grid.add(new Label("Number of Years:"), 0, 2);
        grid.add(yearsField, 1, 2);

        Button calcBtn = new Button("Calculate");
        grid.add(calcBtn, 1, 3);
        grid.add(resultLabel, 0, 4, 2, 1);

        calcBtn.setOnAction(e -> calculate());

        stage.setScene(new Scene(grid, 350, 250));
        stage.setTitle("House Loan Calculator");
        stage.show();
    }

    private void calculate() {
        double P = Double.parseDouble(loanField.getText());
        double annualRate = Double.parseDouble(rateField.getText());
        int n = Integer.parseInt(yearsField.getText()) * 12;

        double r = annualRate / 12 / 100;
        double M = P * r * Math.pow(1 + r, n) / (Math.pow(1 + r, n) - 1);
        double T = M * n;
        double D = T - P;

        resultLabel.setText(String.format(
            "Monthly: %.2f | Total: %.2f | Difference: %.2f", M, T, D));
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

**7.3 — Event handler (already included above)**
The `calcBtn.setOnAction(e -> calculate())` line runs the `calculate()` method when the button is clicked. It uses the standard loan amortization formula:
```
r = AnnualRate / 12 / 100
M = P × r × (1 + r)^n / [(1 + r)^n − 1]
T = M × n
D = T − P
```
Then it updates `resultLabel` with the Monthly Installment, Total Payment, and Difference.

---

## 📘 L8 — Socket Programming & Java RMI (Chat System)

**8.1 — Socket programming vs Java RMI**
- **Sockets**: low-level, you send and receive raw text/bytes yourself over a `Socket`/`ServerSocket` connection. You control the exact message format.
- **RMI (Remote Method Invocation)**: high-level, you call a method on a remote object almost like it's local — Java handles the network communication for you.
**When to prefer each:** Use **sockets** when you need full control over the protocol, cross-language communication, or lightweight custom messaging (like a chat app). Use **RMI** when you're working purely in Java and want to call remote methods directly without manually building a message protocol.

**8.2 — Practical: Server code**
```java
import java.io.*;
import java.net.*;

public class ChatServer {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(5000);
        System.out.println("Server waiting for connection...");
        Socket socket = serverSocket.accept();
        System.out.println("Client connected!");

        BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
        PrintWriter out = new PrintWriter(socket.getOutputStream(), true);

        String message = in.readLine();
        System.out.println("Client says: " + message);
        out.println("Server received: " + message);

        socket.close();
        serverSocket.close();
    }
}
```

**8.3 — Practical: Client code**
```java
import java.io.*;
import java.net.*;

public class ChatClient {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("localhost", 5000);

        PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
        BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));

        out.println("Hello Server!");
        String reply = in.readLine();
        System.out.println("Server reply: " + reply);

        socket.close();
    }
}
```

---

## 📘 L9 — Servlet + JSP + JDBC CRUD (Student Records)

**9.1 — Steps to set up the project**
1. Create the MySQL database `student_db` and a `Students` table with columns `id`, `name`, `cgpa`.
2. Set up a Dynamic Web Project (or Maven web app) in your IDE, add the MySQL JDBC connector `.jar` to the classpath.
3. Map the Servlet to a URL pattern (e.g. `/addStudent`) using `@WebServlet` or `web.xml`.
4. Build the JDBC connection string, e.g. `jdbc:mysql://localhost:3306/student_db`.
5. Write the Servlet (handles form submissions), a DAO class (does the DB work), and a JSP page (shows results).

**9.2 — Practical: Servlet `doPost()`**
```java
import java.io.*;
import java.sql.*;
import jakarta.servlet.*;
import jakarta.servlet.http.*;

public class AddStudentServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        int id = Integer.parseInt(request.getParameter("id"));
        String name = request.getParameter("name");
        double cgpa = Double.parseDouble(request.getParameter("cgpa"));

        String url = "jdbc:mysql://localhost:3306/student_db";
        try (Connection con = DriverManager.getConnection(url, "root", "password");
             PreparedStatement ps = con.prepareStatement(
                 "INSERT INTO Students (id, name, cgpa) VALUES (?, ?, ?)")) {
            ps.setInt(1, id);
            ps.setString(2, name);
            ps.setDouble(3, cgpa);
            ps.executeUpdate();

            response.getWriter().println("Student added successfully!");
        } catch (SQLException e) {
            response.getWriter().println("Error: " + e.getMessage());
        }
    }
}
```

**9.3 — Practical: JSP page**
```jsp
<%@ page import="java.sql.*" %>
<html>
<body>
<h2>Student List</h2>
<table border="1">
<tr><th>ID</th><th>Name</th><th>CGPA</th></tr>
<%
    Connection con = DriverManager.getConnection(
        "jdbc:mysql://localhost:3306/student_db", "root", "password");
    Statement st = con.createStatement();
    ResultSet rs = st.executeQuery("SELECT * FROM Students");
    while (rs.next()) {
%>
<tr>
    <td><%= rs.getInt("id") %></td>
    <td><%= rs.getString("name") %></td>
    <td><%= rs.getDouble("cgpa") %></td>
</tr>
<%
    }
    con.close();
%>
</table>
</body>
</html>
```

---

## 📘 L10 — Spring Boot REST API with JPA/ORM

**10.1 — Setting up Spring Boot + REST + JPA**
1. Create a Spring Boot project (via Spring Initializr) with dependencies: `spring-boot-starter-web` (for REST), `spring-boot-starter-data-jpa` (for ORM), and the MySQL connector.
2. Configure the database connection in `application.properties` (URL, username, password).
3. Write an `@Entity` class (Model), a `Repository` interface, and a `@RestController` (endpoints).
**Role of embedded Tomcat:** Spring Boot bundles a Tomcat server inside the app itself, so you don't need to install or configure a separate server — running the app starts a working web server automatically.

**10.2 — Practical: JPA Entity + Repository**
```java
import jakarta.persistence.*;

@Entity
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private double cgpa;

    // getters and setters
    public Long getId() { return id; }
    public String getName() { return name; }
    public double getCgpa() { return cgpa; }
    public void setName(String name) { this.name = name; }
    public void setCgpa(double cgpa) { this.cgpa = cgpa; }
}
```
```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentRepository extends JpaRepository<Student, Long> {
    // no code needed — JpaRepository already gives save(), findAll(), findById(), etc.
}
```

**10.3 — Practical: REST Controller**
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/students")
public class StudentController {

    @Autowired
    private StudentRepository repo;

    @GetMapping
    public List<Student> getAll() {
        return repo.findAll();
    }

    @PostMapping
    public Student add(@RequestBody Student student) {
        return repo.save(student);
    }
}
```

---

## 📘 L11 — Servlet CRUD — District Quiz Game

**11.1 — DB schema design**
A quiz needs to track categories, questions, players, and scores:
- `CATEGORY(category_id PK, category_name)` — e.g. Crops, Geography, Institutions.
- `QUESTION(question_id PK, category_id FK, question_text, option_a, option_b, option_c, option_d, correct_option)` — each question belongs to one category.
- `PLAYER(player_id PK, name)` — the person playing.
- `PLAYER_SCORE(score_id PK, player_id FK, total_score, played_on)` — one row per game session, linking back to the player.
**Justification:** Separating `QUESTION` from `CATEGORY` lets you filter/add questions per topic easily. Separating `PLAYER` from `PLAYER_SCORE` lets one player have many play sessions (score history) instead of just one score.

**11.2 — Practical: Save player score**
```java
import java.sql.*;
import jakarta.servlet.*;
import jakarta.servlet.http.*;

public class SaveScoreServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, java.io.IOException {
        String name = request.getParameter("name");
        int score = Integer.parseInt(request.getParameter("score"));

        String url = "jdbc:mysql://localhost:3306/quiz_db";
        try (Connection con = DriverManager.getConnection(url, "root", "password");
             PreparedStatement ps = con.prepareStatement(
                 "INSERT INTO PLAYER_SCORE (player_id, total_score, played_on) " +
                 "VALUES ((SELECT player_id FROM PLAYER WHERE name = ?), ?, NOW())")) {
            ps.setString(1, name);
            ps.setInt(2, score);
            ps.executeUpdate();
            response.getWriter().println("Score saved for " + name);
        } catch (SQLException e) {
            response.getWriter().println("Error: " + e.getMessage());
        }
    }
}
```

**11.3 — Practical: Quiz logic**
```java
import java.util.Scanner;

class Question {
    String text, optA, optB, optC, optD, correct;
    Question(String text, String a, String b, String c, String d, String correct) {
        this.text = text; this.optA = a; this.optB = b;
        this.optC = c; this.optD = d; this.correct = correct;
    }
}

public class QuizGame {
    public static void main(String[] args) {
        Question[] questions = {
            new Question("Main crop of the district?", "A) Rice", "B) Wheat", "C) Tea", "D) Cotton", "A"),
            new Question("Which river flows through the district?", "A) Padma", "B) Meghna", "C) Jamuna", "D) Surma", "B"),
            new Question("Oldest academic institution?", "A) High School", "B) College", "C) University", "D) Madrasa", "C")
        };

        Scanner sc = new Scanner(System.in);
        int score = 0;

        for (Question q : questions) {
            System.out.println(q.text);
            System.out.println(q.optA + "  " + q.optB + "  " + q.optC + "  " + q.optD);
            System.out.print("Your answer (A/B/C/D): ");
            String ans = sc.next().toUpperCase();
            if (ans.equals(q.correct)) {
                score++;
                System.out.println("Correct!\n");
            } else {
                System.out.println("Wrong! Correct answer: " + q.correct + "\n");
            }
        }

        System.out.println("Final Score: " + score + "/" + questions.length);
    }
}
```

---

## 📘 L12 — GoF Design Patterns

**12.1 — Creational patterns (5)**

| Pattern | Purpose |
|---|---|
| Singleton | Ensure only one instance of a class exists, with one global access point |
| Factory Method | Let subclasses decide which exact object to create |
| Abstract Factory | Create families of related objects without specifying their exact classes |
| Builder | Build a complex object step-by-step, separately from its final representation |
| Prototype | Create new objects by cloning an existing one |

**12.2 — Structural patterns (7)**

| Pattern | Purpose |
|---|---|
| Adapter | Convert one interface into another one the client expects |
| Bridge | Separate an abstraction from its implementation so both can change independently |
| Composite | Treat single objects and groups of objects the same way (tree structures) |
| Decorator | Add new behavior to an object dynamically, without changing its class |
| Facade | Give a simple front-door interface to a complicated subsystem |
| Flyweight | Share common data between many small objects to save memory |
| Proxy | Provide a stand-in object that controls access to the real object |

**12.3 — Practical: Singleton (thread-safe) + Adapter**
```java
// Thread-safe Singleton
class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

// Adapter pattern
interface Target {
    void request();
}

class Adaptee {
    public void specificRequest() {
        System.out.println("Adaptee's specific behavior");
    }
}

class Adapter implements Target {
    private Adaptee adaptee;

    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    @Override
    public void request() {
        adaptee.specificRequest(); // delegates the call
    }
}

public class Main {
    public static void main(String[] args) {
        Singleton s1 = Singleton.getInstance();
        Singleton s2 = Singleton.getInstance();
        System.out.println("Same instance? " + (s1 == s2)); // true

        Target t = new Adapter(new Adaptee());
        t.request(); // "Adaptee's specific behavior"
    }
}
```

---

### 📌 Quick tips for the exam
- For theory questions, always give **one clear definition + one example** — that's usually enough for full marks.
- For practical questions, make sure your code actually **compiles in your head**: check class names, method signatures, and that `Main` matches what's asked.
- Practice writing the UML diagrams from the original README by hand — professors often ask you to redraw them.
