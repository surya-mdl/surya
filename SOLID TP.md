#      SOLID Principles
- - -
**SOLID principles** are an object-oriented approach that are applied to software structure design. It  ensures that the software is modular, easy to understand, debug, and refactor
The word *SOLID* acronym stands for
* **S**ingle Responsibility Principle (SRP)
* **O**pen-Closed Principle (OCP)
* **L**iskov Substitution Principle(LCP)
* **I**nterface Segregation Principle(ISP)
* **D**ependency Inversion Principle(DIP)
## Single Responsibility Principle
The single responsibility principle states that **every Java class must perform a single functionality**.
Implementation of multiple functionalities in a single class mashup the code and if any modification is required may affect the whole class. 
It precise the code and the code can be easily maintained.
<br/>Let us take a example for clear understanding<br/>
Suppose, **Student** is a class having three methods namely **printDetails(), calculatePercentage(), and addStudent()**. Hence, the Student class has three responsibilities to print the details of students,
calculate percentages, and add student to database.<br/>
This is not following Single Repository Principle
    
    public class Student  
    {  
    public void printDetails();  
    {  
    //functionality of the method  
    }  
    pubic void calculatePercentage();  
    {  
    //functionality of the method  
    }  
    public void addStudent();  
    {  
    //functionality of the method  
    }
    }
This is not following SRP,to achieve that principle to be fulfilled we should divide that code into three classes performing 3 different functionalities.<br/>
**Student.java**

    public class Student  
    {  
    public void addStudent();  
    {  
    //functionality of the method  
    }  
    }  
**PrintStudentDetails.java**

    public class PrintStudentDetails  
    {  
    public void printDetails();  
    {  
    //functionality of the method  
    }  
    }  
**Percentage.java**

    public class Percentage  
    {  
    public void calculatePercentage();  
    {  
    //functionality of the method  
    }  
    }  
The goal is achieved by dividing code into classes by their functionality

## Open-Closed Principle
Software entities (e.g., classes, modules, functions)should be **open for an extension, but closed for modification**.The extension allows us to implement new functionality to the module.<br/>
Consider the below method of the class **VehicleCalculations** :<br/>
````
public class VehicleCalculations {
public double calculateValue(Vehicle v) {
if (v instanceof Car) {
return v.getValue() * 0.8;
if (v instanceof Bike) {
return v.getValue() * 0.5;
  }
}
````
Suppose we now want to add another subclass called **Truck**. We would have to modify the above class by adding another if statement, which goes against the Open-Closed Principle.
A better approach would be for the subclasses**Car** and **Truck** to override the **calculateValue**method:
````
public class Vehicle {
    public double calculateValue() {...}
}
public class Car extends Vehicle {
    public double calculateValue() {
        return this.getValue() * 0.8;
}
public class Truck extends Vehicle {
    public double calculateValue() {
        return this.getValue() * 0.9;
}
  }
````
Here we are calculating value of truck by not modifying class **Vehicle** but by extending that class.This is Open-Closed Principle.
## Liskov substitution principle
The **Liskov Substitution Principle** (LSP) applies to inheritance hierarchies such that derived classes must be completely substitutable for their base classes.<br/>
Consider a typical example of a Square derived class and Rectangle base class:
````
public class Rectangle {
    private double height;
    private double width;
    public void setHeight(double h) { height = h; }
    public void setWidht(double w) { width = w; }
    ...
}
public class Square extends Rectangle {
    public void setHeight(double h) {
        super.setHeight(h);
        super.setWidth(h);
    }
    public void setWidth(double w) {
        super.setHeight(w);
        super.setWidth(w);
    }
}
````
The above classes do not obey LSP because you cannot replace the **Rectangle** base class with its derived class **Square**. The **Square** class has extra constraints, i.e., the height and width 
must be the same. Therefore, substituting**Rectangle** with **Square** class may result in unexpected behavior.<br/>
## Interface Segregation principle 
The **Interface Segregation Principle (ISP)** states that clients should not be forced to depend upon interface members they do not use. In other 
words, do not force any client to implement an interface that is irrelevant to them.<br/>
Suppose there’s an interface for vehicle and a Bike class:
````
public interface Vehicle {
    public void drive();
    public void stop();
    public void refuel();
    public void openDoors();
}
public class Bike implements Vehicle {

    // Can be implemented
    public void drive() {...}
    public void stop() {...}
    public void refuel() {...}
    
    // Can not be implemented
    public void openDoors() {...}
}
````
As you can see, it does not make sense for a **Bike** class to implement the **openDoors()** method
as a bike does not have any doors! To fix this, ISP proposes that the interfaces be broken down 
into multiple, small cohesive interfaces so that no class is forced to implement any interface, and 
therefore methods, that it does not need.
##Dependency inversion principle
The Dependency Inversion Principle (DIP) states that we should depend on abstractions
(interfaces and abstract classes) instead of concrete implementations (classes). The abstractions should
not depend on details; instead, the details should depend on abstractions.<br/>
Consider the example below. We have a **Car** class that depends on the concrete **Engine** class; therefore, it
is not obeying DIP.
````
public class Car {
    private Engine engine;
    public Car(Engine e) {
        engine = e;
    }
    public void start() {
        engine.start();
    }
}
public class Engine {
   public void start() {...}
}
````
The code will work, for now, but what if we wanted to add another engine type, let’s say a diesel engine? This will require refactoring the **Car** class.
However, we can solve this by introducing a layer of abstraction. Instead of **Car** depending directly on **Engine**, let’s add an interface:
````
public interface EngineInterface {
    public void start();
}
````
Now we can connect any type of **Engine** that implements the Engine interface to the **Car** class:
````
public class Car {
    private EngineInterface engine;
    public Car(EngineInterface e) {
        engine = e;
    }
    public void start() {
        engine.start();
    }
}
public class PetrolEngine implements EngineInterface {
   public void start() {...}
}
public class DieselEngine implements EngineInterface {
   public void start() {...}
}
````