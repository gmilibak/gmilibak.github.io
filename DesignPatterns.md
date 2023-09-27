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
- the related objects share an interface too
- components
  - abstract factory: interface or abstract class that declares methods where each method is responsible for creating a specific type of object
  - concrete factory: class that implements the **abstract factory**. Each factory is responsible for creating products that are consistent with each other
  - abstract product: interface or abstract class that declares the commonalities for a type of object
  - concrete product: class that implements the **abstract product**. Represent the actual product instances created by concrete factories. Different factories create different concrete products
  - client code uses the abstract factory and defines the concrete factory to be used through a parameter

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

```python
# Abstract Product
class AbstractChair:
    def sit_on(self):
        pass

# Concrete Products
class ModernChair(AbstractChair):
    def sit_on(self):
        return "Sitting on a modern chair."

class VictorianChair(AbstractChair):
    def sit_on(self):
        return "Sitting on a Victorian chair."

# Abstract Factory
class FurnitureFactory:
    def create_chair(self):
        pass

# Concrete Factories
class ModernFurnitureFactory(FurnitureFactory):
    def create_chair(self):
        return ModernChair()

class VictorianFurnitureFactory(FurnitureFactory):
    def create_chair(self):
        return VictorianChair()

# Client
def client(furniture_factory):
    chair = furniture_factory.create_chair()
    print(chair.sit_on())

# Usage
modern_furniture = ModernFurnitureFactory()
victorian_furniture = VictorianFurnitureFactory()

client(modern_furniture)      # Output: Sitting on a modern chair.
client(victorian_furniture)   # Output: Sitting on a Victorian chair.
```

#### Builder

- lets the user build complex objects in small steps
- by recombining the steps the user is able to create different representations or types of an object
- useful when the 'other solution' is a bunch of constructors with different number of arguments
- the methods building part of an object can return the builder object allowing to chain building methods
- in java it is much like using the setters but more fluent
- components
  - director (optional): a class that holds methods to create the commonly built variants
  - builder: interface with the different build steps declared
  - concrete builder: class that implements the **builder** interface
  - product: the object that is built by the concrete builder class
 
```java
interface HousePlan
{
    public void setBasement(String basement);
  
    public void setStructure(String structure);
  
    public void setRoof(String roof);
  
    public void setInterior(String interior);
}
  
class House implements HousePlan
{
  
    private String basement;
    private String structure;
    private String roof;
    private String interior;
  
    public void setBasement(String basement) 
    {
        this.basement = basement;
    }
  
    public void setStructure(String structure) 
    {
        this.structure = structure;
    }
  
    public void setRoof(String roof) 
    {
        this.roof = roof;
    }
  
    public void setInterior(String interior) 
    {
        this.interior = interior;
    }
  
}
  
  
interface HouseBuilder
{
  
    public void buildBasement();
  
    public void buildStructure();
  
    public void buildRoof();
  
    public void buildInterior();
  
    public House getHouse();
}
  
class IglooHouseBuilder implements HouseBuilder
{
    private House house;
  
    public IglooHouseBuilder() 
    {
        this.house = new House();
    }
  
    public void buildBasement() 
    {
        house.setBasement("Ice Bars");
    }
  
    public void buildStructure() 
    {
        house.setStructure("Ice Blocks");
    }
  
    public void buildInterior() 
    {
        house.setInterior("Ice Carvings");
    }
  
    public void buildRoof() 
    {
        house.setRoof("Ice Dome");
    }
  
    public House getHouse() 
    {
        return this.house;
    }
}
  
class TipiHouseBuilder implements HouseBuilder
{
    private House house;
  
    public TipiHouseBuilder() 
    {
        this.house = new House();
    }
  
    public void buildBasement() 
    {
        house.setBasement("Wooden Poles");
    }
  
    public void buildStructure() 
    {
        house.setStructure("Wood and Ice");
    }
  
    public void buildInterior() 
    {
        house.setInterior("Fire Wood");
    }
  
    public void buildRoof() 
    {
        house.setRoof("Wood, caribou and seal skins");
    }
  
    public House getHouse() 
    {
        return this.house;
    }
  
}
  
class CivilEngineer 
{
  
    private HouseBuilder houseBuilder;
  
    public CivilEngineer(HouseBuilder houseBuilder)
    {
        this.houseBuilder = houseBuilder;
    }
  
    public House getHouse()
    {
        return this.houseBuilder.getHouse();
    }
  
    public void constructHouse()
    {
        this.houseBuilder.buildBasement();
        this.houseBuilder.buildStructure();
        this.houseBuilder.buildRoof();
        this.houseBuilder.buildInterior();
    }
}
  
class Builder
{
    public static void main(String[] args)
    {
        HouseBuilder iglooBuilder = new IglooHouseBuilder();
        CivilEngineer engineer = new CivilEngineer(iglooBuilder);
  
        engineer.constructHouse();
  
        House house = engineer.getHouse();
  
        System.out.println("Builder constructed: "+ house);
    }
}
```

#### Factory Method

- provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created
- does not necessarily create new instances, can return cached objects
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

- the Prototype pattern is useful in scenarios where object creation is resource-intensive
- creates new objects by copying existing ones
- it promotes flexibility and reusability in your code by allowing you to create objects with varying states without the need for extensive subclassing or complex object initialization logic

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

- allows two incompatible interfaces to collaborate
- useful when the developer want to use a class that does not fit with existing code and cannot be easily modified
- components
  - target interface: this is expected to stay the same (client side code)
  - adaptee: the interface that has to be made compatible with the target interface
  - adapter: implements the target interface and translate the code into calls the **adaptee** can process

```java
// Target Interface
interface MediaPlayer {
    void play(String mediaType, String fileName);
}

// Adaptee
class AdvancedMediaPlayer {
    void playVlc(String fileName) {
        System.out.println("Playing VLC file: " + fileName);
    }

    void playMp4(String fileName) {
        System.out.println("Playing MP4 file: " + fileName);
    }
}

// Adapter
class MediaAdapter implements MediaPlayer {
    private AdvancedMediaPlayer advancedPlayer;

    MediaAdapter(String mediaType) {
        if (mediaType.equalsIgnoreCase("vlc")) {
            advancedPlayer = new AdvancedMediaPlayer();
        } else if (mediaType.equalsIgnoreCase("mp4")) {
            advancedPlayer = new AdvancedMediaPlayer();
        }
    }

    @Override
    public void play(String mediaType, String fileName) {
        if (mediaType.equalsIgnoreCase("vlc")) {
            advancedPlayer.playVlc(fileName);
        } else if (mediaType.equalsIgnoreCase("mp4")) {
            advancedPlayer.playMp4(fileName);
        }
    }
}

// Client Code
class AudioPlayer implements MediaPlayer {
    @Override
    public void play(String mediaType, String fileName) {
        if (mediaType.equalsIgnoreCase("mp3")) {
            System.out.println("Playing MP3 file: " + fileName);
        } else if (mediaType.equalsIgnoreCase("vlc") || mediaType.equalsIgnoreCase("mp4")) {
            MediaAdapter mediaAdapter = new MediaAdapter(mediaType);
            mediaAdapter.play(mediaType, fileName);
        } else {
            System.out.println("Invalid media type: " + mediaType);
        }
    }
}

public class AdapterPatternDemo {
    public static void main(String[] args) {
        MediaPlayer audioPlayer = new AudioPlayer();

        audioPlayer.play("mp3", "song.mp3");
        audioPlayer.play("vlc", "movie.vlc");
        audioPlayer.play("mp4", "video.mp4");
    }
}
```

#### Bridge

- used to separate an object abstraction from its implementation
- designed to handle variations in the class hierarchy
- instead of extending inheritance in multiple dimensions the object will reference an object (composition) from a separate hierarchy leaving the original intact
- components
  - abstraction: this is a high level interface for the client code to interact with it
  - implementor: an interface to define methods implemented in the low level **concrete implementors**
  - refined abstraction: class that implements or extend the abstraction class. These implementations are using the **implementor** interface
  - concrete implementor: class that implements the implementor interface

```java
// Implementor interface
interface DrawingAPI {
    void drawCircle(double x, double y, double radius);
    void drawRectangle(double x1, double y1, double x2, double y2);
}

// Concrete Implementor 1
class DrawingAPI1 implements DrawingAPI {
    @Override
    public void drawCircle(double x, double y, double radius) {
        System.out.printf("API1 - Drawing a circle at (%.2f, %.2f) with radius %.2f%n", x, y, radius);
    }

    @Override
    public void drawRectangle(double x1, double y1, double x2, double y2) {
        System.out.printf("API1 - Drawing a rectangle from (%.2f, %.2f) to (%.2f, %.2f)%n", x1, y1, x2, y2);
    }
}

// Concrete Implementor 2
class DrawingAPI2 implements DrawingAPI {
    @Override
    public void drawCircle(double x, double y, double radius) {
        System.out.printf("API2 - Drawing a circle at (%.2f, %.2f) with radius %.2f%n", x, y, radius);
    }

    @Override
    public void drawRectangle(double x1, double y1, double x2, double y2) {
        System.out.printf("API2 - Drawing a rectangle from (%.2f, %.2f) to (%.2f, %.2f)%n", x1, y1, x2, y2);
    }
}

// Abstraction
abstract class Shape {
    protected DrawingAPI drawingAPI;

    protected Shape(DrawingAPI drawingAPI) {
        this.drawingAPI = drawingAPI;
    }

    abstract void draw();
}

// Refined Abstraction 1
class Circle extends Shape {
    private double x, y, radius;

    public Circle(double x, double y, double radius, DrawingAPI drawingAPI) {
        super(drawingAPI);
        this.x = x;
        this.y = y;
        this.radius = radius;
    }

    @Override
    void draw() {
        drawingAPI.drawCircle(x, y, radius);
    }
}

// Refined Abstraction 2
class Rectangle extends Shape {
    private double x1, y1, x2, y2;

    public Rectangle(double x1, double y1, double x2, double y2, DrawingAPI drawingAPI) {
        super(drawingAPI);
        this.x1 = x1;
        this.y1 = y1;
        this.x2 = x2;
        this.y2 = y2;
    }

    @Override
    void draw() {
        drawingAPI.drawRectangle(x1, y1, x2, y2);
    }
}

public class BridgePatternDemo {
    public static void main(String[] args) {
        DrawingAPI api1 = new DrawingAPI1();
        DrawingAPI api2 = new DrawingAPI2();

        Shape circle1 = new Circle(1, 2, 3, api1);
        Shape circle2 = new Circle(4, 5, 6, api2);
        Shape rectangle1 = new Rectangle(1, 2, 3, 4, api1);
        Shape rectangle2 = new Rectangle(5, 6, 7, 8, api2);

        circle1.draw();
        circle2.draw();
        rectangle1.draw();
        rectangle2.draw();
    }
}
```

#### Composite

- used to compose objects into tree structures and handle these structures as individual objects
- it allows the developer to work with individual objects and their compositions in a consistent manner
- components:
  - component: base interface that can represent both leaf and composite objects
  - leaf: class that implements the component and represents individual objects that cannot have any children in the hierarchy. the smallest unit of the composition
  - composite: class that implements the component and represents a collection of child components (both composites and leaves)
  - client: treats the leaf and composites as components so does not have to know when is a leaf reached

```java
// Component interface
interface Component {
    void operation();
}

// Leaf class
class Leaf implements Component {
    private String name;

    public Leaf(String name) {
        this.name = name;
    }

    @Override
    public void operation() {
        System.out.println("Leaf " + name + " is performing an operation.");
    }
}

// Composite class
class Composite implements Component {
    private String name;
    private List<Component> children = new ArrayList<>();

    public Composite(String name) {
        this.name = name;
    }

    public void add(Component component) {
        children.add(component);
    }

    public void remove(Component component) {
        children.remove(component);
    }

    @Override
    public void operation() {
        System.out.println("Composite " + name + " is performing an operation.");
        for (Component child : children) {
            child.operation();
        }
    }
}

// Client code
public class CompositePatternDemo {
    public static void main(String[] args) {
        Component leaf1 = new Leaf("Leaf 1");
        Component leaf2 = new Leaf("Leaf 2");
        Component leaf3 = new Leaf("Leaf 3");

        Component composite1 = new Composite("Composite 1");
        composite1.add(leaf1);
        composite1.add(leaf2);

        Component composite2 = new Composite("Composite 2");
        composite2.add(leaf3);
        composite2.add(composite1);

        composite2.operation();
    }
}
```

#### Decorator

- allows a user to add new functionality to an existing object without altering its structure (either statically or dynamically)
- wraps the original class and provides additional functionality keeping class methods signature intact
- components:
  - component: interface or abstract class that defines the core functionalities for objects that can be decorated
  - concrete component: class that implements the component interface
  - decorator: abstract class that implements the component interface. Contains a reference to a **component** object and can add further functionalities to the wrapped object
  - concrete decorator: class that implements the decorator interface. Adds to or override the behavior of the contained **concrete component**. Multiple decorators can be stacked together for complex combinations of behavior

```java
// Component interface
interface Coffee {
    String getDescription();
    double cost();
}

// Concrete Component
class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple Coffee";
    }

    @Override
    public double cost() {
        return 2.0;
    }
}

// Decorator abstract class
abstract class CoffeeDecorator implements Coffee {
    private Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee decoratedCoffee) {
        this.decoratedCoffee = decoratedCoffee;
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription();
    }

    @Override
    public double cost() {
        return decoratedCoffee.cost();
    }
}

// Concrete Decorators
class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee decoratedCoffee) {
        super(decoratedCoffee);
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", Milk";
    }

    @Override
    public double cost() {
        return super.cost() + 0.5;
    }
}

class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee decoratedCoffee) {
        super(decoratedCoffee);
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", Sugar";
    }

    @Override
    public double cost() {
        return super.cost() + 0.2;
    }
}

public class DecoratorPatternDemo {
    public static void main(String[] args) {
        Coffee coffee = new SimpleCoffee();
        System.out.println("Cost: $" + coffee.cost());
        System.out.println("Ingredients: " + coffee.getDescription());

        Coffee milkCoffee = new MilkDecorator(coffee);
        System.out.println("Cost: $" + milkCoffee.cost());
        System.out.println("Ingredients: " + milkCoffee.getDescription());

        Coffee sugarMilkCoffee = new SugarDecorator(milkCoffee);
        System.out.println("Cost: $" + sugarMilkCoffee.cost());
        System.out.println("Ingredients: " + sugarMilkCoffee.getDescription());
    }
}
```

#### Facade

- hides the complexities of the system and provides an interface for the client to access
- involves a single class which provides simplified methods required by client and delegates calls to methods of existing system classes
- components
  - facade: central class that provided a simplified, high level interface for the client to use
  - subsystems: set of classes and interfaces that perform specific tasks in the system
  - client: client code that interacts with the **facade** instead of interacting the **subsystems** or knowing about the low level complexities
- promotes loose coupling between the **client code** and the **subsystems**

```java
// Subsystem 1
class CPU {
    public void start() {
        System.out.println("CPU: Starting...");
    }

    public void shutdown() {
        System.out.println("CPU: Shutting down...");
    }
}

// Subsystem 2
class Memory {
    public void load() {
        System.out.println("Memory: Loading data...");
    }

    public void unload() {
        System.out.println("Memory: Unloading data...");
    }
}

// Subsystem 3
class HardDrive {
    public void read() {
        System.out.println("Hard Drive: Reading data...");
    }

    public void write() {
        System.out.println("Hard Drive: Writing data...");
    }
}

// Facade
class ComputerFacade {
    private CPU cpu;
    private Memory memory;
    private HardDrive hardDrive;

    public ComputerFacade() {
        this.cpu = new CPU();
        this.memory = new Memory();
        this.hardDrive = new HardDrive();
    }

    public void start() {
        System.out.println("ComputerFacade: Starting computer...");
        cpu.start();
        memory.load();
        hardDrive.read();
        System.out.println("ComputerFacade: Computer started.");
    }

    public void shutdown() {
        System.out.println("ComputerFacade: Shutting down computer...");
        cpu.shutdown();
        memory.unload();
        hardDrive.write();
        System.out.println("ComputerFacade: Computer shut down.");
    }
}

// Client code
public class FacadePatternDemo {
    public static void main(String[] args) {
        ComputerFacade computer = new ComputerFacade();

        // Start and shut down the computer using the Facade
        computer.start();
        System.out.println("Working on the computer...");
        computer.shutdown();
    }
}
```

#### Flyweight

- focuses on optimizing memory usage by sharing as much properties among multiple similar objects as can
- useful for handling a huge number of objects which have a significant amount of shared state but also contain unique information
- components
  - flyweight: interface or abstract class that defines the common functionalities and properties for both the shared and unique states
  - concrete flyweight: class that implements the **flyweight** and references states that are shared between objects (and preferably immutable)
  - flyweight factory: class that manages and creates **flyweight** objects. Ensures that shared **flyweight** objects are reused and new created only when necessary
  - client: client code that uses the **flyweight** objects

```java
// Flyweight Interface - Character
public interface Character {
    void display(String font);
}
// Concrete Flyweight - ConcreteCharacter
public class ConcreteCharacter implements Character {
    private char character;
    public ConcreteCharacter(char character) {
        this.character = character;
    }
    @Override
    public void display(String font) {
        System.out.println("Character: " + character + ", Font: " + font);
    }
}
// Flyweight Factory - CharacterFactory
public class CharacterFactory {
    private Map<java.lang.Character, Character> characterCache;
    public CharacterFactory() {
        characterCache = new HashMap<>();
    }
    public Character getCharacter(char c) {
        Character character = characterCache.get(c);
        if (character == null) {
            character = new ConcreteCharacter(c);
            characterCache.put(c, character);
        }
        return character;
    }
}

public class Application {
    public static void main(String[] args) {
        CharacterFactory characterFactory = new CharacterFactory();
        Character character1 = characterFactory.getCharacter('A');
        character1.display("Arial");
        Character character2 = characterFactory.getCharacter('B');
        character2.display("Times New Roman");
        Character character3 = characterFactory.getCharacter('A');
        character3.display("Calibri");
    }
}
```

```java
import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Container;
import java.awt.Graphics;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;

import java.util.HashMap;

public interface Shape {

	public void draw(Graphics g, int x, int y, int width, int height,
			Color color);
}

public class Line implements Shape {

	public Line(){
		System.out.println("Creating Line object");
		//adding time delay
		try {
			Thread.sleep(2000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
	@Override
	public void draw(Graphics line, int x1, int y1, int x2, int y2,
			Color color) {
		line.setColor(color);
		line.drawLine(x1, y1, x2, y2);
	}

}

public class Oval implements Shape {
	
	//intrinsic property
	private boolean fill;
	
	public Oval(boolean f){
		this.fill=f;
		System.out.println("Creating Oval object with fill="+f);
		//adding time delay
		try {
			Thread.sleep(2000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
	@Override
	public void draw(Graphics circle, int x, int y, int width, int height,
			Color color) {
		circle.setColor(color);
		circle.drawOval(x, y, width, height);
		if(fill){
			circle.fillOval(x, y, width, height);
		}
	}

}

public class ShapeFactory {

	private static final HashMap<ShapeType,Shape> shapes = new HashMap<ShapeType,Shape>();

	public static Shape getShape(ShapeType type) {
		Shape shapeImpl = shapes.get(type);

		if (shapeImpl == null) {
			if (type.equals(ShapeType.OVAL_FILL)) {
				shapeImpl = new Oval(true);
			} else if (type.equals(ShapeType.OVAL_NOFILL)) {
				shapeImpl = new Oval(false);
			} else if (type.equals(ShapeType.LINE)) {
				shapeImpl = new Line();
			}
			shapes.put(type, shapeImpl);
		}
		return shapeImpl;
	}
	
	public static enum ShapeType{
		OVAL_FILL,OVAL_NOFILL,LINE;
	}
}

public class DrawingClient extends JFrame{

	private static final long serialVersionUID = -1350200437285282550L;
	private final int WIDTH;
	private final int HEIGHT;

	private static final ShapeType shapes[] = { ShapeType.LINE, ShapeType.OVAL_FILL,ShapeType.OVAL_NOFILL };
	private static final Color colors[] = { Color.RED, Color.GREEN, Color.YELLOW };
	
	public DrawingClient(int width, int height){
		this.WIDTH=width;
		this.HEIGHT=height;
		Container contentPane = getContentPane();

		JButton startButton = new JButton("Draw");
		final JPanel panel = new JPanel();

		contentPane.add(panel, BorderLayout.CENTER);
		contentPane.add(startButton, BorderLayout.SOUTH);
		setSize(WIDTH, HEIGHT);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);

		startButton.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent event) {
				Graphics g = panel.getGraphics();
				for (int i = 0; i < 20; ++i) {
					Shape shape = ShapeFactory.getShape(getRandomShape());
					shape.draw(g, getRandomX(), getRandomY(), getRandomWidth(),
							getRandomHeight(), getRandomColor());
				}
			}
		});
	}
	
	private ShapeType getRandomShape() {
		return shapes[(int) (Math.random() * shapes.length)];
	}

	private int getRandomX() {
		return (int) (Math.random() * WIDTH);
	}

	private int getRandomY() {
		return (int) (Math.random() * HEIGHT);
	}

	private int getRandomWidth() {
		return (int) (Math.random() * (WIDTH / 10));
	}

	private int getRandomHeight() {
		return (int) (Math.random() * (HEIGHT / 10));
	}

	private Color getRandomColor() {
		return colors[(int) (Math.random() * colors.length)];
	}

	public static void main(String[] args) {
		DrawingClient drawing = new DrawingClient(500,600);
	}
}
```

#### Proxy

- provides a placeholder for another object to control access to it
- allows developer to add additional layer of control over the interaction with the real object
- components
  - subject: interface or abstract class defining the common interface for both the **real subject** and the **proxy**. Defines functionality that the **proxy** will delegate to the **real subject**
  - real subject: actual object that implements the **subject** and will be represented by the **proxy**
  - proxy: class that implements the **subject** and holds a reference to the **real subject**. Then it intercepts the requests targeted to the **real subject** and perform additional actions before or after delegating
- types
  - virtual proxy: lazy-loads the **real subject** only at the point it is first needed
  - remote proxy: provides local representation of object from remote location
  - protection proxy: adds access control and authorization checks before allowing the client to access the **real subject**
  - cache proxy: store results of extensive operations and returns them when the same operation is requested again

```java
// Subject interface
interface Image {
    void display();
}

// Real Subject
class RealImage implements Image {
    private final String filename;

    public RealImage(String filename) {
        this.filename = filename;
        loadFromDisk();
    }

    private void loadFromDisk() {
        System.out.println("Loading image: " + filename);
    }

    @Override
    public void display() {
        System.out.println("Displaying image: " + filename);
    }
}

// Proxy
class ProxyImage implements Image {
    private RealImage realImage;
    private final String filename;

    public ProxyImage(String filename) {
        this.filename = filename;
    }

    @Override
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(filename);
        }
        realImage.display();
    }
}

// Client code
public class ProxyPatternDemo {
    public static void main(String[] args) {
        Image image1 = new ProxyImage("image1.jpg");
        Image image2 = new ProxyImage("image2.jpg");

        // The real image is loaded and displayed only when requested
        image1.display();
        image2.display();
    }
}
```

### Behavioral

#### Chain of Responsibility

- allows the developer to pass requests along a chain of handlers
- each handler decides whether to process the request or pass it along
- promotes loose coupling between clients and handlers
- components
  - handler: interface or abtract class that defines the method for handling requests and contains a reference to the next handler in the chain
  - concrete handler: class that implements the handler interface. makes a decision to handle or pass the request to the next **concrete handler**
  - client: client code or object that initiates the request

```java
// Handler interface
interface Handler {
    void handleRequest(Request request);
    void setNextHandler(Handler nextHandler);
}

// ConcreteHandler 1
class ConcreteHandler1 implements Handler {
    private Handler nextHandler;

    @Override
    public void handleRequest(Request request) {
        if (request.getType() == RequestType.TYPE1) {
            System.out.println("ConcreteHandler1 is handling the request.");
        } else if (nextHandler != null) {
            nextHandler.handleRequest(request);
        } else {
            System.out.println("No handler is able to process the request.");
        }
    }

    @Override
    public void setNextHandler(Handler nextHandler) {
        this.nextHandler = nextHandler;
    }
}

// ConcreteHandler 2
class ConcreteHandler2 implements Handler {
    private Handler nextHandler;

    @Override
    public void handleRequest(Request request) {
        if (request.getType() == RequestType.TYPE2) {
            System.out.println("ConcreteHandler2 is handling the request.");
        } else if (nextHandler != null) {
            nextHandler.handleRequest(request);
        } else {
            System.out.println("No handler is able to process the request.");
        }
    }

    @Override
    public void setNextHandler(Handler nextHandler) {
        this.nextHandler = nextHandler;
    }
}

// Request class
class Request {
    private final RequestType type;

    public Request(RequestType type) {
        this.type = type;
    }

    public RequestType getType() {
        return type;
    }
}

// Enum for request types
enum RequestType {
    TYPE1, TYPE2
}

// Client code
public class ChainOfResponsibilityPatternDemo {
    public static void main(String[] args) {
        Handler handler1 = new ConcreteHandler1();
        Handler handler2 = new ConcreteHandler2();

        handler1.setNextHandler(handler2);

        Request request1 = new Request(RequestType.TYPE1);
        handler1.handleRequest(request1);

        Request request2 = new Request(RequestType.TYPE2);
        handler1.handleRequest(request2);
    }
}
```

#### Command

- encapsulates a request as an object
- turns the request into a standalone object that can be parameterized and holds all necessary informations
- these can be used in queues as it decouples the sender (client) and receiver
- components
  - command: interface or abstract class that declares the 'execute()' method
  - concrete command: class that implements the **command** interface and encapsulates the details of the request (receiver reference, arguments). Invokes the receiver operation when 'execute()' called
  - receiver: the object that performs the actual operation
  - invoker: responsible for triggering the **command** execution
  - client: client code is responsible for creating **command** objects and setting the necessary information. Also configures the invoker with the **command** objects

```java
// Command interface
interface Command {
    void execute();
}

// Concrete Command
class LightOnCommand implements Command {
    private Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOn();
    }
}

// Receiver
class Light {
    public void turnOn() {
        System.out.println("Light is on");
    }

    public void turnOff() {
        System.out.println("Light is off");
    }
}

// Invoker
class RemoteControl {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        command.execute();
    }
}

// Client code
public class CommandPatternDemo {
    public static void main(String[] args) {
        // Create receiver
        Light light = new Light();

        // Create concrete command with receiver
        Command lightOnCommand = new LightOnCommand(light);

        // Create invoker and set the command
        RemoteControl remoteControl = new RemoteControl();
        remoteControl.setCommand(lightOnCommand);

        // Press the button to execute the command
        remoteControl.pressButton();
    }
}
```

#### Interpreter

- used to define a grammar for interpreting a language
- evaluate and execute expressions or statements in a language
- components
  - abstract expression: interface or abstract class that declares an 'interpret()' method.
  - terminal expression: represents basic building blocks of the language and does not have sub-expressions. Implements **abstract expression** (with the 'interpret()' method)
  - non-terminal expression: represents complex expressions composed of one or more sub-expressions. Implements **abstract expression** (with the 'interpret()' method) but delegates the actual interpretation to the sub-expressions (and can perform additional operations) 
  - context: contains information that shared among **expressions** and necessary for the interpretation
  - client: responsible for creating expressions in order to execute the desired operations in the interpreted language. Also sets up the context and invokes 'interpret()'

```java
import java.util.Map;

// Abstract Expression
interface Expression {
    int interpret(Map<String, Integer> context);
}

// Terminal Expression
class NumberExpression implements Expression {
    private int number;

    public NumberExpression(int number) {
        this.number = number;
    }

    @Override
    public int interpret(Map<String, Integer> context) {
        return number;
    }
}

// Non-terminal Expression
class AddExpression implements Expression {
    private Expression leftOperand;
    private Expression rightOperand;

    public AddExpression(Expression left, Expression right) {
        this.leftOperand = left;
        this.rightOperand = right;
    }

    @Override
    public int interpret(Map<String, Integer> context) {
        int leftValue = leftOperand.interpret(context);
        int rightValue = rightOperand.interpret(context);
        return leftValue + rightValue;
    }
}

// Context
class Context {
    private Map<String, Integer> variables;

    public Context(Map<String, Integer> variables) {
        this.variables = variables;
    }

    public int getVariableValue(String variableName) {
        return variables.get(variableName);
    }

    public void setVariableValue(String variableName, int value) {
        variables.put(variableName, value);
    }
}

// Client code
public class InterpreterPatternDemo {
    public static void main(String[] args) {
        // Define the variables and their values
        Context context = new Context(Map.of("x", 5, "y", 7));

        // Create expressions
        Expression expression1 = new NumberExpression(10);
        Expression expression2 = new NumberExpression(2);
        Expression expression3 = new NumberExpression(3);

        Expression addExpression = new AddExpression(expression1, expression2);
        Expression complexExpression = new AddExpression(addExpression, expression3);

        // Interpret the expressions
        int result = complexExpression.interpret(context);
        System.out.println("Result: " + result);
    }
}
```

#### Iterator

- provides a way to access elements of a collection sequentially withouth exposing the underlying representation
- with it the developer can iterate through elements in a consistent manner
- components
  - iterator: interface or abstract class that defines the common methods for iterating through elements in a collection (like 'next()', 'hasNext()', 'remove()')
  - concrete iterator: class that implements the **iterator** interface. Holds actual implementation of the iteration logic for a type of collection
  - aggregate: interface or abstract class that defines methods for creating iterators
  - concrete aggregate: class that implements **aggregate** interface and represents a collection of objects. Creates **concrete iterator** instance that can operate on the represented collection
  - client: client code that uses the **iterator** to traverse the elements of the collection

```java
import java.util.Iterator;

// Aggregate interface
interface Menu {
    Iterator<String> createIterator();
}

// Concrete Aggregate
class BreakfastMenu implements Menu {
    private String[] items;

    public BreakfastMenu() {
        items = new String[]{"Scrambled Eggs", "Pancakes", "Bacon"};
    }

    @Override
    public Iterator<String> createIterator() {
        return new BreakfastMenuIterator(items);
    }
}

// Concrete Iterator
class BreakfastMenuIterator implements Iterator<String> {
    private String[] items;
    private int position = 0;

    public BreakfastMenuIterator(String[] items) {
        this.items = items;
    }

    @Override
    public boolean hasNext() {
        return position < items.length;
    }

    @Override
    public String next() {
        if (!hasNext()) {
            throw new IllegalStateException("No more items to iterate.");
        }
        return items[position++];
    }
}

// Client code
public class IteratorPatternDemo {
    public static void main(String[] args) {
        Menu breakfastMenu = new BreakfastMenu();
        Iterator<String> iterator = breakfastMenu.createIterator();

        System.out.println("Breakfast Menu:");
        while (iterator.hasNext()) {
            String item = iterator.next();
            System.out.println(item);
        }
    }
}
```

#### Mediator

- centralizes interactions between objects through a mediator object
- allows objects to communicate with each other without having direct references to each other
- the mediator coordinates and manages the interactions
- components
  - mediator: interface or abstract class that defines the communication interface between **colleague** objects. Usually includes methods for registering, notifying and handling **colleague** interactions
  - concrete mediator: class that implements the **mediator** interface. Holds references to **colleague** objects
  - colleague: represents individual objects that need to communicate with each other but should avoid direct references. Aware of the **mediator** interface
- benefits
  - reduce dependencies between objects
  - centralize control, communication logic in one place
  - flexible, allows for dynamic and runtime communication between objects

```java
import java.util.ArrayList;
import java.util.List;

// Mediator interface
interface ChatMediator {
    void sendMessage(String message, Colleague colleague);
}

// Concrete Mediator
class ChatRoom implements ChatMediator {
    private List<Colleague> colleagues = new ArrayList<>();

    @Override
    public void sendMessage(String message, Colleague colleague) {
        for (Colleague c : colleagues) {
            if (c != colleague) {
                c.receive(message);
            }
        }
    }

    public void addColleague(Colleague colleague) {
        colleagues.add(colleague);
    }
}

// Colleague interface
abstract class Colleague {
    private ChatMediator mediator;

    public Colleague(ChatMediator mediator) {
        this.mediator = mediator;
    }

    public void send(String message) {
        mediator.sendMessage(message, this);
    }

    public abstract void receive(String message);
}

// Concrete Colleague
class User extends Colleague {
    private String name;

    public User(String name, ChatMediator mediator) {
        super(mediator);
        this.name = name;
    }

    @Override
    public void receive(String message) {
        System.out.println(name + " received: " + message);
    }
}

// Client code
public class MediatorPatternDemo {
    public static void main(String[] args) {
        ChatMediator chatMediator = new ChatRoom();

        User user1 = new User("Alice", chatMediator);
        User user2 = new User("Bob", chatMediator);
        User user3 = new User("Charlie", chatMediator);

        chatMediator.addColleague(user1);
        chatMediator.addColleague(user2);
        chatMediator.addColleague(user3);

        user1.send("Hello, everyone!");
    }
}
```

#### Memento

- allows the developer to capture and externalize an object's internal state without exposing its internal structure
- provides a way to save and restore the state of an object
- with it possible to implement features like undo/redo or the ability to revert an object to a previous state
- components
  - originator: the object whose state needs to be saved. Can create a **memento** object that represents its state
  - memento: an object that stores the internal state of the **originator**. May include metadata or timestamps and typically immutable and only accessed by the **originator**
  - caretaker: responsible for managing **mementos**. It can requests **mementos** from the **originator**

```java
// Memento class
class Memento {
    private String state;

    public Memento(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }
}

// Originator class
class Originator {
    private String state;

    public void setState(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }

    public Memento saveStateToMemento() {
        return new Memento(state);
    }

    public void restoreStateFromMemento(Memento memento) {
        state = memento.getState();
    }
}

// Caretaker class
class Caretaker {
    private Memento memento;

    public void saveState(Memento memento) {
        this.memento = memento;
    }

    public Memento getSavedState() {
        return memento;
    }
}

// Client code
public class MementoPatternDemo {
    public static void main(String[] args) {
        Originator originator = new Originator();
        Caretaker caretaker = new Caretaker();

        // Set and save the state
        originator.setState("State 1");
        caretaker.saveState(originator.saveStateToMemento());

        // Change the state
        originator.setState("State 2");

        // Restore the state
        originator.restoreStateFromMemento(caretaker.getSavedState());

        System.out.println("Current State: " + originator.getState());
    }
}
```

#### Observer

- defines a one-to-many dependency between objects so that when the one changes state all its dependents are notified automatically
- implements a mechanism for objects to communicate and stay synchronized without being tightly coupled
- components
  - subject (publisher): the object that is of interest. Provides methods for attaching, detaching, and notifying observers
  - concrete subject: class that implements **subject** interface and maintains the state. It notifies its **observers** when the state changes.
  - observer (subscriber): interface or abstract class that defines updating mechanism
  - concrete observer: class that implements **observer** interface. Register with the **subject** and receive updates when **subject's** state is changed
- benefits
  - decouples subject and observers
  - flexibility: can add observers without any change

```java
import java.util.ArrayList;
import java.util.List;

// Observer interface
interface Observer {
    void update(String message);
}

// Subject interface
interface Subject {
    void attach(Observer observer);
    void detach(Observer observer);
    void notifyObservers(String message);
}

// Concrete Subject
class NewsAgency implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String news;

    @Override
    public void attach(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void detach(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }

    public void setNews(String news) {
        this.news = news;
        notifyObservers(news);
    }
}

// Concrete Observer
class NewsChannel implements Observer {
    private String news;

    @Override
    public void update(String message) {
        news = message;
        displayNews();
    }

    public void displayNews() {
        System.out.println("News Channel received news: " + news);
    }
}

// Client code
public class ObserverPatternDemo {
    public static void main(String[] args) {
        NewsAgency newsAgency = new NewsAgency();

        NewsChannel channel1 = new NewsChannel();
        NewsChannel channel2 = new NewsChannel();

        newsAgency.attach(channel1);
        newsAgency.attach(channel2);

        newsAgency.setNews("Breaking news: Observer pattern works!");
    }
}
```

#### State

#### Strategy

#### Template method

#### Visitor
