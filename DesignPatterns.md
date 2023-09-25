# Design pattern

## Definition

- it is not code
- it is a general concept for solving commonly occurring problems in software design

## Why

## Types

### Creational

#### Singleton

- ensure that a class has only one instance, while providing a global access point to this instance.
- java.util.Calendar

#### Factory Method

- provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created
- does not have to create new instances, can return cached objects
- creation methods that construct objects from concrete classes
- but the return type is usually an abstract class or interface

#### Abstract Factory

#### Builder

- lets the user build complex objects in small steps
- by recombining the steps the user is able to create different representations or types of an object

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

### Structural

#### Decorator

- allows a user to add new functionality to an existing object without altering its structure
- wraps the original class and provides additional functionality keeping class methods signature intact

#### Facade

- hides the complexities of the system and provides an interface for the client to access
- involves a single class which provides simplified methods required by client and delegates calls to methods of existing system classes

#### Adapter

#### Bridge

#### Proxy

#### Flyweight

#### Composite

### Behavioral

#### Iterator

#### Command

#### Observer

#### Chain of Responsibility

#### Mediator

#### Memento

#### State

#### Strategy

#### Template method

#### Visitor
