# Design pattern

## Origin

- Design Patterns: Elements of Reusable Object-Oriented Software (1995)
  - book written by the Gang of Four
  - Erich Gamma, Richard Helm, Ralph Johnson and John Vlissides

## Definition and properties

- it is a general concept for solving commonly occurring problems in object oriented software design
- it is a solution template for a particular problem
- it is not code
- independent of language

## Why

- when used in the correct situation it makes development easier
- makes code more maintainable and robust
- as they are widely known (even without acknowledging that a particular code is a design pattern) it makes code more readable

## Types

### Creational

#### Abstract Factory

- provides an interface to create families of related or dependent objects without specifying their concrete classes

```java
interface Button {
void render();
}

interface Checkbox {
    void render();
}

class WindowsButton implements Button {
    @Override
    public void render() {
        System.out.println("Rendering Windows Button");
    }
}

class WindowsCheckbox implements Checkbox {
    @Override
    public void render() {
        System.out.println("Rendering Windows Checkbox");
    }
}

class MacOSButton implements Button {
    @Override
    public void render() {
        System.out.println("Rendering MacOS Button");
    }
}

class MacOSCheckbox implements Checkbox {
    @Override
    public void render() {
        System.out.println("Rendering MacOS Checkbox");
    }
}

interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

class WindowsFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new WindowsButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new WindowsCheckbox();
    }
}

class MacOSFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new MacOSButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new MacOSCheckbox();
    }
}
```

#### Builder

- lets the user build complex objects in small steps
- by recombining the steps the user is able to create different representations or types of an object

#### Factory Method

- provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created
- does not have to create new instances, can return cached objects
- creation methods that construct objects from concrete classes
- but the return type is usually an abstract class or interface

```java
interface Product {
    void create();
}

class ConcreteProductA implements Product {

    @Override
    public void create() {
        System.out.println("Creating Product A");
    }
}

class ConcreteProductB implements Product {

    @Override
    public void create() {
        System.out.println("Creating Product B");
    }
}

abstract class Creator {
    public abstract Product factoryMethod();
}

class ConcreteCreatorA extends Creator {
    @Override
    public Product factoryMethod() {
        return new ConcreteProductA();
    }
}
```

#### Prototype

- the Prototype pattern is useful in scenarios where object creation is resource-intensive, and you want to efficiently create new objects by copying existing ones. It promotes flexibility and reusability in your code by allowing you to create objects with varying states without the need for extensive subclassing or complex object initialization logic.

```java
import java.util.Objects;

class Person implements Cloneable {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Person clone() throws CloneNotSupportedException {
        return (Person) super.clone();
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Name: " + name + ", Age: " + age;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Person person = (Person) obj;
        return age == person.age && Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}

public class PrototypeExample {
    public static void main(String[] args) {
        // Create a prototype person
        Person prototypePerson = new Person("John", 30);

        try {
            // Clone the prototype to create a new person with a different name
            Person person1 = prototypePerson.clone();
            person1.setName("Alice");

            // Clone the prototype again to create another new person with a different name
            Person person2 = prototypePerson.clone();
            person2.setName("Bob");

            // Output the details of the cloned persons
            System.out.println(person1);  // Output: Name: Alice, Age: 30
            System.out.println(person2);  // Output: Name: Bob, Age: 30
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
    }
}
```

```python
import copy

class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def clone(self):
        # Use the 'copy' module to create a shallow copy of the object
        return copy.copy(self)

    def __str__(self):
        return f"Name: {self.name}, Age: {self.age}"

# Create a prototype person
prototype_person = Person("John", 30)

# Create a new person by cloning the prototype
person1 = prototype_person.clone()
person1.name = "Alice"

# Create another new person by cloning the prototype
person2 = prototype_person.clone()
person2.name = "Bob"

# Output the details of the cloned persons
print(person1)  # Output: Name: Alice, Age: 30
print(person2)  # Output: Name: Bob, Age: 30
```

#### Singleton

- ensure that a class has only one instance, while providing a global access point to this instance.

```java
public class Singleton {

    private static Singleton instance;

    private Singleton() {};

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
    return instance;
    }
}
```

### Structural

#### Adapter

#### Bridge

#### Composite

#### Decorator

- allows a user to add new functionality to an existing object without altering its structure
- wraps the original class and provides additional functionality keeping class methods signature intact

#### Facade

- hides the complexities of the system and provides an interface for the client to access
- involves a single class which provides simplified methods required by client and delegates calls to methods of existing system classes

#### Flyweight

#### Proxy

### Behavioral

#### Chain of Responsibility

#### Command

#### Interpreter

#### Iterator

#### Mediator

#### Memento

#### Observer

#### State

#### Strategy

#### Template method

#### Visitor
