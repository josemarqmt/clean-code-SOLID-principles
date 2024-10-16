# Notes on the book: “Clean Code - A Handbook of Agile Software Craftsmanship” by Robert C. Martin and explanation of SOLID principles.




<img src="https://github.com/user-attachments/assets/951126f3-cc0e-4df8-bc5b-dce9a2d59bb2" alt="clean_code_book" width="200" style="margin-right: 200px;"/>

<img src="https://github.com/user-attachments/assets/2863564a-4d80-4ced-aaeb-95adb66824b2" alt="SOLID" width="500"/>
<br><br>

1. [Main rules to be followed in software development](#id1)<br>
2. [What is clean code?](#id2)<br>
3. [Meaningful names](#id3)<br>
4. [Git: Use of commits and branches correctly and using meaningful names (Not included in the book but I think it’s necessary)](#id4)<br>
  4.1 [Commits](#id4.1)<br>
  4.2 [Branches](#id4.2)<br>
  4.3 [When to use merge and when to rebase](#id4.3)<br>
5. [Functions](#id5)<br>
  5.1 [Order of the code based on the level of abstraction within the functions](#id5.1)<br>
  5.2 [Use of switch instructions in functions](#id5.2)<br>
  5.3 [Exception handling in functions](#id5.3)<br>
  5.4 [Use of arguments in functions](#id5.4)<br>
6. [Comments](#id6)<br>
  6.1 [Necessary comments](#id6.1)<br>
7. [Format of the code in the files](#id7)<br>
  7.1 [File size](#id7.1)<br>
  7.2 [Order of the code lines in the files](#id7.2)<br>
  7.3 [Order of functions in the files](#id7.3)<br>
  7.4 [Variable declaration in the files](#id7.4)<br>
  7.5 [Length of the list of statements in a file](#id7.5)<br>
8. [Objects and data structures](#id8)<br>
  8.1 [Difference](#id8.1)<br>
9. [Process errors](#id9)<br>
   9.1 [Process errors from external api](#id9.1)<br>
   9.2 [Do not return null or pass null to methods](#id9.2)<br>
10. [Use third party codes or APIs](#id10)<br>
11. [Unit tests](#id11)<br>
12. [Classes](#id12)<br>
13. [SOLID Principles of Object Oriented Design](#id11)<br>


<a name="id1"> </a>
## Main rules to be followed in software development

1. Use of correct names.
2. A code fragment should be where we expect to find it, otherwise refactor until you get it.
3. Do not flood the code with comments and lines that capture future stories or desires.
4. Consistent code style and a set of practices within the group.
5. Be disciplined in applying the practices and reflect them in the work.

<a name="id2"> </a>
## What is clean code?

- Clean code is concrete, each function, each class and each module shows a single attitude that remains unchanged and untainted by surrounding details.
- Code, without tests, is not clean.
- Clean code always looks like it was written by someone who cares.
- When something is repeated over and over again, it's a sign that we have an idea that we don't quite represent correctly.
- If it is easier to read, it will be easier to write.

<a name="id3"> </a>
## Meaningful names

- For a name to be meaningful, it must indicate why it exists, what it does and how it is used.

Bad:

```java
public List<int[]> getThem() {
   List<int[]> list1 = new ArrayList<int[]>();

   for (int[] x : theList)
    if (x[0] == 4)
	    list1.add(x);
   return list1;
 }
```

Good:

```java
public List<int[]> getFlaggedCells() {
	List<int[]> flaggedCells = new ArrayList<int[]>();

  for (int[] cell : gameBoard)
    if (cell[STATUS_VALUE] == FLAGGED)
	    flaggedCells.add(cell);

  return flaggedCells;
}
```

- Make names easy to distinguish.
    
    the variable `moneyAmount` is not distinguished from `money`, `customerInfo` is not distinguished from `customer`, `accountData` is not distinguished from `account` and `theMessage` is not distinguished from `message`.
    
- Create names that are easy to pronounce.
- Avoid name encoding.
- The name of a class must not be a verb. It should be a noun.
    
    Examples: `Customer`, `WikiPage`, `Account` and `AddressParser`.
    

<a name="id4"> </a>
## Git: Use of commits and branches correctly and using meaningful names (Not included in the book but I think it’s necessary)

<a name="id4.1"> </a>
### Commits

- We must commit every small change.
- The title must start with a verb in the present singular that describes the action performed in the code such as `Add`, `Change`/`Update`, `Fix`, `Remove`.
    
    Examples:
    
    ```bash
    add a new search feature
    
    fix a problem with the top bar
    
    change the dafault system color
    
    remove a random notification
    
    ```
    
- The title should be as short as possible and in case of needing a longer explanation we should add a message to the commit.

<a name="id4.2"> </a>
### Branches

- Use them according to the workflow used in the equipment.
- The name should be divided by a description of the reason for the branch and to the functionality of the application to be modified.
    
    Examples:
    
    ```bash
    bug/welcoming-homepage
    feature/user-login
    experiment/automatic-carousel-scrolling
    hotfix/fix-registration-form
    
    ```
    
<a name="id4.3"> </a>
### When to use merge and when to rebase

- Always use merge to bring changes to the main branch, as this leaves a clear record of how and from where the changes were made in the history.
- Use rebase to bring changes to functionality branches, this way the merge commit history does not get full.

<a name="id5"> </a>
## Functions

- They must be small in size.
- Functions should only do one thing and they should do it well.
- When adding blocks in if, else, while and similar statements they must call other functions.
    
    Bad:
    
    ```java
    public static String renderPageWithSetupsAndTeardowns(
        PageData pageData, boolean isSuite
        ) throws Exception {
          boolean isTestPage = pageData.hasAttribute(“Test”);
          if (isTestPage) {
    	      WikiPage testPage = pageData.getWikiPage();
    	      StringBuffer newPageContent = new StringBuffer();
    	      includeSetupPages (testPage, newPageContent, isSuite);
    	      newPageContent.append(pageData.getContent());
    	      includeTeardownPages(testPage, newPageContent, isSuite);
    	      pageData.setContent(newPageContent.toString());
        }
    
       return pageData.getHtml();
    
    }
    ```
    
    Good:
    
    ```java
    	public static String renderPageWithSetupsAndTeardowns(
        PageData pageData, boolean isSuite) throws Exception {
        if (isTestPage(pageData))
    	    includeSetupAndTeardownPages(pageData, isSuite);
    
        return pageData.getHtml();
     }
    ```
    
<a name="id5.1"> </a>
### Order of the code based on the level of abstraction within the functions

- The instructions of a function must be at the same level of abstraction.
    
    Do not mix for example instructions such as getHtml(), PathParser.render or .append(“append”) in a function as these functions are at different levels of abstraction.
    
- The functions must be ordered in the code following a descending rule in the abstraction level, in which each function describes the current level and refers the subsequent ones to its level.

<a name="id5.2"> </a>
### Use of switch instructions in functions

- Avoid using switch instructions and in the case of including them in the function, try to hide them in an abstract factory.
    
    Example:
    
    ```java
    public abstract class Employee {
    
        public abstract boolean isPayday();
        public abstract Money calculatePay();
        public abstract void deliverPay(Money pay);
    
    }
    
    public interface EmployeeFactory {
        public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType;
    }
    
    public class EmployeeFactoryImpl implements EmployeeFactory {
    
        public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
            switch (r.type) {
                case COMMISSIONED:
                    return new CommissionedEmployee(r);
                case HOURLY:
                    return new HourlyEmployee(r);
                case SALARIED:
                    return new SalariedEmployee(r);
                default:
                    throw new InvalidEmployeeType(r.type);
            }
        }
    }
    ```
    
<a name="id5.3"> </a>
### Exception handling in functions

- Extracts the body of try and catch blocks in individual functions.
    
    Example:
    
    ```java
    public void delete(Page page) {
        try {
            deletePageAndAllReferences(page);
        } catch (Exception e) {
            logError(e);
        }
    }
    
    private void deletePageAndAllReferences(Page page) throws Exception {
        deletePage(page);
        registry.deleteReference(page.name);
        configKeys.deleteKey(page.name.makeKey());
    }
    
    private void logError(Exception e) {
        logger.log(e.getMessage());
    ```
    
<a name="id5.4"> </a>
### Use of arguments in functions

- The ideal number of arguments for a function is zero and at most three.
- Reasons for making a function with only one argument:
    - Ask a question about the argument.
        
        ```java
        InputStream fileOpen(“MyFile”)
        ```
        
    - Process argument.
        
        ```java
        fileOpen(“MyFile”)
        ```
        
- Do not pass boolean arguments.
- Always opt for argument reduction using classes.
    
    Example:
    
    ```java
    Circle makeCircle(double x, double y, double radius); // three arguments
    
    Circle makeCircle(Point center, double radius); // reduction to two arguments using class
    ```
    
- The function and argument should form a verb and noun pair and try to give as much information as possible about the type of argument it will receive and its order.
    
    Example:
    
    ```java
    writeField(name) # indicates that name is a field
    assertExpectedEqualsActual # indicates order of arguments
    ```
    
<a name="id6"> </a>
## Comments

- Use them only when the programming language does not allow us to express in detail.
- The code is the only source of accurate information and they are not compensated by comments.
    
    Example of incorrect code using comments:
    
    ```java
    // Check if the employee is entitled to all the benefits
    if ((employee.flags & HOURLY_FLAG) && (employee.age > 65)) {
        // code
    }
    
    ```
    
    Example of expressive code that does not require the use of comments:
    
    ```java
    if (employee.isEligibleForFullBenefits()) 
    ```
    
<a name="id6.1"> </a>
### Necessary comments

- Copyright comments.
- Comments warning of serious consequences or use of certain practices.
- Comments TODO in case the task is to be performed in a short time frame.

<a name="id7"> </a>
## Format of the code in the files

<a name="id7.1"> </a>
### File size

- Small file sizes are better understood than large files.
- A file should not exceed 500 lines of code vertically and should not exceed 120 characters horizontally.

<a name="id7.2"> </a>
### Order of the code lines in the files

- Each blank line should be a visual clue to help us identify and differentiate independent concepts.

<a name="id7.3"> </a>
### Order of functions in the files

- If one function invokes another, they should be vertically next to each other, and the invoking function should be above the invoked one whenever possible.

<a name="id7.4"> </a>
### Variable declaration in the files

- Variables must appear at the top of each function.
- Loop control variables must be placed in the same instruction as the loop.
    
    Example:
    
    ```java
    public int countTestCases() {
        int count = 0;
    
        for (Test each : tests) {
            count += each.countTestCases();
        }
    
        return count;
    }
    ```
    
- Instance variables, on the other hand, must be declared at the top of the class.

<a name="id7.5"> </a>
### Length of the list of statements in a file

- The length of the list of statements cannot be long, if it is, it suggests that this class should be split.

<a name="id8"> </a>
## Objects and data structures

<a name="id8.1"> </a>
### Difference

- Objects
    
    Objects hide their data behind abstractions and show functions that operate on that data.
    
    They should be used when we want to include several types of objects without needing to change existing behaviors.
    
    Example:
    
    ```java
    public class Square implements Shape {
    
        private Point topLeft;
        private double side;
    
        public double area() {
            return side * side;
        }
    
        public class Rectangle implements Shape {
    
            private Point topLeft;
            private double height;
            private double width;
    
            public double area() {
                return height * width;
            }
        }
    
        public class Circle implements Shape {
    
            private Point center;
            private double radius;
            public final double PI = 3.141592653589793;
    
            public double area() {
                return PI * radius * radius;
            }
        }
    }
    ```
    

- Data structure
    
    Structures display their data and lack significant behaviors.
    
    They should be used when we want to include several functions without the need to change the classes or data structures created.
    
    Example:
    
    ```java
    public class Square {
        public Point topLeft;
        public double side;
    }
    
    public class Rectangle {
        public Point topLeft;
        public double height;
        public double width;
    }
    
    public class Circle {
        public Point center;
        public double radius;
    }
    
    public class Geometry {
    
        public final double PI = 3.141592653589793;
    
        public double area(Object shape) throws NoSuchShapeException {
            if (shape instanceof Square) {
                Square s = (Square) shape;
                return s.side * s.side;
            } else if (shape instanceof Rectangle) {
                Rectangle r = (Rectangle) shape;
                return r.height * r.width;
            } else if (shape instanceof Circle) {
                Circle c = (Circle) shape;
                return PI * c.radius * c.radius;
            }
    
            throw new NoSuchShapeException();
        }
    }
    ```
    
<a name="id9"> </a>
## Process errors

- Using exceptions instead of code returns
- Separate error control logic from code.
- Exceptions should provide adequate context to determine the source and location of an error through informative error messages mentioning the failed operation and type of failure.

<a name="id9.1"> </a>
### Process errors from external api

We must encapsulate the exception handling of an external library into classes in order to reduce all exception handling into a single custom exception.

Examples:

- Calling custom exception in LocalPort class:
    
    ```java
    LocalPort port = new LocalPort(12);
    
    try {
        port.open();
    } catch (PortDeviceFailure e) {
        reportError(e);
        logger.log(e.getMessage(), e);
    } finally {
        // ...
    }
    
    ```
    

- Clase LocalPort:
    
    ```java
    public class LocalPort {
    
        private ACMEPort innerPort;
    
        public LocalPort(int portNumber) {
            innerPort = new ACMEPort(portNumber);
        }
    
        public void open() {
            try {
                innerPort.open();
            } catch (DeviceResponseException e) {
                throw new PortDeviceFailure(e);
            } catch (ATM1212UnlockedException e) {
                throw new PortDeviceFailure(e);
            } catch (GMXError e) {
                throw new PortDeviceFailure(e);
            }
        }
    
        // ...
    }
    ```
    
<a name="id9.2"> </a>
### Do not return null or pass null to methods.

- Do not return null or pass null to methods, as it is one of the main causes of common errors such as the famous **`NullPointerException`** and forces the **** developer to add additional checks or conditionals to handle the possibility of `null` .
- We have to make sure that the code never generates null instead of handling null exceptions, for that the code returns exceptions, a special case object, or an empty object like a list instead of generating a variable with null value.

<a name="id10"> </a>
## Use third party codes or APIs

- Abstracting logic from an API:
    
    We must use own classes to encapsulate the functionality we want to incorporate from the third party code, this makes that we can perform an abstraction of the logic and be able to change public API only by modifying the class, we also get to get access only to the desired functionality of the API without revealing all its functionalities.
    
    Useful example of abstracting map functionality using a class:
    
    ```java
    public class Sensors {
    
        private Map sensors = new HashMap();
    
        public Sensor getById(String id) {
            return (Sensor) sensors.get(id);
        }
    
        // corte
    }
    ```
    

- Create tests for each functionality that we want to obtain from an API:
    
    This way we can update in the future the API to a new version and see if the procedure for the use we made of the API has been modified.
    
    Example:
    
    ```java
    public class LogTest {
    
        private Logger logger;
    
        @Before
        public void initialize() {
            logger = Logger.getLogger("logger");
            logger.removeAllAppenders();
            Logger.getRootLogger().removeAllAppenders();
        }
    
        @Test
        public void basicLogger() {
            BasicConfigurator.configure();
            logger.info("basicLogger");
        }
    
        @Test
        public void addAppenderWithStream() {
            logger.addAppender(new ConsoleAppender(
                new PatternLayout("%p %t %m%n"),
                ConsoleAppender.SYSTEM_OUT
            ));
            logger.info("addAppenderWithStream");
        }
    
        @Test
        public void addAppenderWithoutStream() {
            logger.addAppender(new ConsoleAppender(
                new PatternLayout("%p %t %m%n")
            ));
            logger.info("addAppenderWithoutStream");
        }
    }
    ```
    
<a name="id11"> </a>
## Unit tests

- Tests must meet the same code standards as production code.
- They should be created before production code.
- Each test must be able to run independently and test only one concept per test.

<a name="id12"> </a>
## Classes

- They should follow the following order:
    1. List of variables
    2. Private static variables
    3. Private instance variable
    4. Public functions
- They must be small in size
- They must have a single responsibility, therefore it is necessary to avoid classes with names such as Processor, Manager or Super that encompass several responsibilities.
- They must have a reduced number of instance variables.

<a name="id13"> </a>
## SOLID Principles of Object Oriented Design

SOLID principles are a set of guidelines to help software developers design maintanable, scalable, and robust systems.

- (S) Single Responsability Principle
    
    A class should have only one job or responsability, to make the class easier to understand, mantain and test.
    
    ```python
    class Report:
        def __init__(self, data):
            self.data = data
    
        def generate_report(self):
            return f"Report Data: {self.data}"
    
    class ReportPrinter:
        def print_report(self, report):
            print(report.generate_report())
    
    # One class is responsible for the report content, and another class is responsible for printing it.
    report = Report("Sales Data")
    printer = ReportPrinter()
    printer.print_report(report)
    ```
    

- (O) Open/Closed Principle
    
    Classes should be able to extend it’s behavior without modifying its source code, to prevent bugs in existing functionality and encourages code reuse.
    
    ```python
    class Shape:
        def area(self):
            raise NotImplementedError()
    
    class Circle(Shape):
        def __init__(self, radius):
            self.radius = radius
    
        def area(self):
            return 3.14 * self.radius * self.radius
    
    class Rectangle(Shape):
        def __init__(self, width, height):
            self.width = width
            self.height = height
    
        def area(self):
            return self.width * self.height
    
    # New shapes can be added without modifying existing code.
    shapes = [Circle(10), Rectangle(5, 6)]
    for shape in shapes:
        print(f"Area: {shape.area()}")
    ```
    

- (L) Liskov Substitution Principle
    
    Subtypes must be substitutable for their base types without altering the correctness of the program, to ensure that a derived class can stand in for its base class without introducing bugs.
    
    ```python
    class Bird:
        def fly(self):
            raise NotImplementedError()
    
    class Sparrow(Bird):
        def fly(self):
            return "Sparrow is flying"
    
    class Penguin(Bird):  # Violating LSP (penguins can't fly)
        def fly(self):
            raise Exception("Penguins can't fly")
    
    def make_bird_fly(bird):
        print(bird.fly())
    
    # Following LSP: All bird subclasses should behave like Bird.
    sparrow = Sparrow()
    make_bird_fly(sparrow)  # Works fine
    
    # Penguin would violate LSP since it doesn't behave like a Bird.
    # penguin = Penguin()
    # make_bird_fly(penguin)  # This would raise an exception (LSP violation)
    ```
    

- (I) Interface Segregation Principle
    
    No client should be forced to depend on methods it does not use. Instead of one large interface, it’s better to have many small interfaces, that helps prevent bloated interfaces and keeps code more focused and clean.
    
    ```python
    class Printer:
        def print(self, document):
            raise NotImplementedError()
    
    class Scanner:
        def scan(self, document):
            raise NotImplementedError()
    
    class MultiFunctionPrinter(Printer, Scanner):
        def print(self, document):
            return f"Printing: {document}"
    
        def scan(self, document):
            return f"Scanning: {document}"
    
    class SimplePrinter(Printer):
        def print(self, document):
            return f"Printing: {document}"
    
    # Clients can choose the specific interface they need, without implementing unused methods.
    printer = SimplePrinter()
    print(printer.print("My Document"))
    
    ```
    

### (D) Dependency Inversion Principle

- High-level modules should not depend on low-level modules, should depend on abstractions, to make the code more flexible, reusable, and testable.

  ```python
  from abc import ABC, abstractmethod
  
  class NotificationSender(ABC):
      @abstractmethod
      def send(self, message):
          pass
  
  class EmailSender(NotificationSender):
      def send(self, message):
          print(f"Sending email: {message}")
  
  class SMSSender(NotificationSender):
      def send(self, message):
          print(f"Sending SMS: {message}")
  
  class NotificationService:
      def __init__(self, sender: NotificationSender):
          self.sender = sender
  
      def notify(self, message):
          self.sender.send(message)
  
  # High-level class depends on the abstraction (NotificationSender), not on the concrete implementation.
  email_service = NotificationService(EmailSender())
  email_service.notify("Hello via Email!")
  
  sms_service = NotificationService(SMSSender())
  sms_service.notify("Hello via SMS!")
  
  ```
