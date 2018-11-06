# Java oops Concepts


### Encapsulation

Encapsulation is one of the four fundamental OOP concepts. The other three are inheritance, polymorphism, and abstraction.
Encapsulation in Java is a mechanism of wrapping the data (variables) and code acting on the data (methods) together as as single unit. In encapsulation the variables of a class will be hidden from other classes, and can be accessed only through the methods of their current class, therefore it is also known as data hiding.

To achieve encapsulation in Java :

* Declare the variables of a class as private.
* Provide public setter and getter methods to modify and view the variables values.

### Inheritance

Inheritance can be defined as the process where one class acquires the properties (methods and fields) of another. With the use of inheritance the information is made manageable in a hierarchical order.

The class which inherits the properties of other is known as subclass (derived class, child class) and the class whose properties are inherited is known as superclass (base class, parent class).

**extends** is the keyword used to inherit the properties of a class. Below given is the syntax of extends keyword.

### Polymorphism

Polymorphism is the concept where an object behaves differently in different situations.
**There are two types of polymorphism – compile time polymorphism and runtime polymorphism.**

**Compile time polymorphism** is achieved by method overloading. For example, we can have a class as below.

	public class Circle {

	public void draw(){
	System.out.println("Drwaing circle with default color Black and diameter 1 cm.");
	}

	public void draw(int diameter){
	System.out.println("Drwaing circle with default color Black and diameter"+diameter+" cm.");
	}

	public void draw(int diameter, String color){
	System.out.println("Drwaing circle with color"+color+" and diameter"+diameter+" cm.");
	}
	}

Here we have multiple draw methods but they have different behavior. This is a case of method overloading because all the methods name is same and arguments are different. Here compiler will be able to identify the method to invoke at compile time, hence it’s called compile time polymorphism.

**Runtime polymorphism** is implemented when we have “IS-A” relationship between objects. This is also called as method overriding because subclass has to override the superclass method for runtime polymorphism. If we are working in terms of superclass, the actual implementation class is decided at runtime. Compiler is not able to decide which class method will be invoked. This decision is done at runtime, hence the name as runtime polymorphism or dynamic method dispatch.

	package com.test;

	public interface Shape {
	public void draw();
	}

	package com.test;

	public class Circle implements Shape{

	@Override
	public void draw(){
	System.out.println("Drwaing circle");
	}

	}

	package com.test;
	public class Square implements Shape {

	@Override
	public void draw() {
	System.out.println("Drawing Square");
	}

	}
Shape is the superclass and there are two subclasses Circle and Square. Below is an example of runtime polymorphism.

	Shape sh = new Circle();
	sh.draw();

	Shape sh1 = getShape(); //some third party logic to determine shape
	sh1.draw();

In above examples, java compiler don’t know the actual implementation class of Shape that will be used at runtime, hence runtime polymorphism.

### Abstraction

In Object oriented programming Abstraction is a process process of hiding the implementation details from the user, only the functionality will be provided to the user. In other words user will have the information on what the object does instead of how it does it.

**In Java Abstraction is achieved using Abstract classes and Interfaces.**

A class which contains the abstract keyword in its declaration is known as abstract class.


An interface is a reference type in Java, it is similar to class, it is a collection of abstract methods. A class implements an interface, thereby inheriting the abstract methods of the interface.
Along with abstract methods an interface may also contain constants, default methods, static methods, and nested types. Method bodies exist only for default methods and static methods.

Writing an interface is similar to writing a class. But a class describes the attributes and behaviours of an object. And an interface contains behaviours that a class implements.

Unless the class that implements the interface is abstract, all the methods of the interface need to be defined in the class.

An interface is similar to a class in the following ways:

* An interface can contain any number of methods.
* An interface is written in a file with a .java extension, with the name of the interface matching the name of the file.
* The byte code of an interface appears in a .class file.
* Interfaces appear in packages, and their corresponding bytecode file must be in a directory structure that matches the package name.
* However, an interface is different from a class in several ways, including:
* You cannot instantiate an interface.
* An interface does not contain any constructors.
* All of the methods in an interface are abstract.
* An interface cannot contain instance fields. The only fields that can appear in an interface must be declared both static and final.
* An interface is not extended by a class; it is implemented by a class.
* An interface can extend multiple interfaces.

### Refrences

https://www.google.com
https://www.tutorialspoint.com