---
layout: post
title: "SOLID Design Principles in Python"
author: "Mohidul Islam"
tags: Tale
---

The SOLID principles are a set of design principles that were introduced by Robert C. Martin, also known as Uncle Bob, in the early 2000s. These principles provide guidelines for writing maintainable, scalable, and flexible code. In this blog post, we will explore the five SOLID principles and how they can be applied to Python code with practical examples.

#### S - Single Responsibility Principle (SRP)
The Single Responsibility Principle (SRP) is a design principle that states that a class or module should have only one reason to change, which means it should have only one responsibility. This principle aims to improve the maintainability and testability of software by making it easier to understand and modify.

To better understand the SRP, let's consider an example of a `User` class that is responsible for both authenticating and updating user information:
```python
class User:
    def __init__(self, username, password, email):
        self.username = username
        self.password = password
        self.email = email

    def authenticate(self):
        # authenticate the user

    def update_info(self, username, password, email):
        # update user information

```
In this example, the `User` class has two responsibilities: authentication and updating user information. This violates the SRP because a change in one responsibility (e.g., updating user information) could impact the other responsibility (e.g., authentication).

To apply the SRP, we can split the `User` class into two separate classes, one responsible for authentication and another responsible for updating user information:
```python
class Authenticator:
    def __init__(self, username, password):
        self.username = username
        self.password = password

    def authenticate(self):
        # authenticate the user


class User:
    def __init__(self, username, password, email):
        self.username = username
        self.password = password
        self.email = email

    def update_info(self, username, password, email):
        # update user information
```
Now, the `Authenticator` class is responsible for authentication, while the `User` class is responsible for updating user information. This separation of concerns makes the code more maintainable and easier to understand.

In summary, the Single Responsibility Principle is an essential design principle that encourages developers to write code that is focused and has a clear purpose. By separating responsibilities into smaller, more cohesive modules, developers can improve the maintainability and testability of their software.

#### O - Open/Closed Principle (OCP)
The Open/Closed Principle (OCP) is a design principle that states that software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. This means that we should be able to add new functionality to the software without changing its existing code.

To understand the OCP, let's consider an example of a `Shape` class hierarchy that has two subclasses: `Rectangle` and `Circle`.
```python
class Shape:
    def draw(self):
        pass

class Rectangle(Shape):
    def draw(self):
        print("Drawing a rectangle")

class Circle(Shape):
    def draw(self):
        print("Drawing a circle")
```
In this example, the `Shape` class is the base class for the `Rectangle` and `Circle` classes. Each subclass overrides the draw method to draw a different shape.

Now, let's say we want to add a new shape, `Triangle`, to our `Shape` hierarchy. We could modify the existing code to add the new subclass:
```python
class Triangle(Shape):
    def draw(self):
        print("Drawing a triangle")

class Rectangle(Shape):
    def draw(self):
        print("Drawing a rectangle")

class Circle(Shape):
    def draw(self):
        print("Drawing a circle")
```
This violates the OCP because we had to modify the existing code to add the new functionality.

To follow the OCP, we can use the concept of polymorphism to make our code open for extension but closed for modification. Instead of modifying the existing `Shape` class hierarchy, we can create a new `Triangle` class that inherits from `Shape`.
```python
class Shape:
    def draw(self):
        pass

class Rectangle(Shape):
    def draw(self):
        print("Drawing a rectangle")

class Circle(Shape):
    def draw(self):
        print("Drawing a circle")

class Triangle(Shape):
    def draw(self):
        print("Drawing a triangle")
```
Now, we have extended our `Shape` hierarchy without modifying its existing code. This makes our code more flexible and easier to maintain.

In summary, the Open/Closed Principle is a fundamental design principle that encourages developers to write code that is open for extension but closed for modification. By using polymorphism and inheritance, we can extend our software without changing its existing code, making it more flexible and easier to maintain.

#### L - The Liskov Substitution Principle (LSP)

The Liskov Substitution Principle (LSP) is a fundamental design principle that states that objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program. This means that a subclass should be able to substitute its parent class without changing the behavior of the program.

To understand the LSP, let's consider an example of a `Rectangle` class hierarchy that has two subclasses: `Square` and `Rectangle`.
```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def set_width(self, width):
        self.width = width

    def set_height(self, height):
        self.height = height

    def get_area(self):
        return self.width * self.height

class Square(Rectangle):
    def __init__(self, size):
        super().__init__(size, size)

    def set_width(self, width):
        self.width = width
        self.height = width

    def set_height(self, height):
        self.width = height
        self.height = height
```
In this example, `Square` is a subclass of `Rectangle` that represents a square shape. It overrides the `set_width` and `set_height` methods to ensure that both width and height are set to the same value.

Now, let's say we have a function that calculates the area of a rectangle:
```python
def calculate_area(rectangle):
    rectangle.set_width(5)
    rectangle.set_height(4)
    return rectangle.get_area()
```
If we pass a `Rectangle` object to the `calculate_area` function, it will return the correct area of 20. However, if we pass a `Square` object to the function, it will also return 20, which is incorrect because the Square object should have an area of 25.

This violates the LSP because the `Square` object is not substitutable for the `Rectangle` object, as its behavior differs.

To follow the LSP, we can modify the `Square` class to ensure that it behaves as a `Rectangle`:
```python
class Square(Rectangle):
    def __init__(self, size):
        super().__init__(size, size)

    def set_width(self, width):
        self.set_size(width)

    def set_height(self, height):
        self.set_size(height)

    def set_size(self, size):
        self.width = size
        self.height = size
```
Now, the `Square` object is substitutable for the `Rectangle` object, as its behavior is consistent with the Rectangle object. By following the Liskov Substitution Principle, we can ensure that our software is robust, flexible, and easier to maintain.


#### I - Interface Segregation Principle (ISP)
The Interface Segregation Principle (ISP) is a design principle that states that no client should be forced to depend on methods it does not use. This means that interfaces should be designed to be cohesive and focused, with a specific set of methods that are only relevant to the clients that use them.

To understand the ISP, let's consider an example of a `Printer` class that has several methods, including `print`, `scan`, `copy`, and `fax`.
```python
class Printer:
    def print(self, document):
        pass

    def scan(self, document):
        pass

    def copy(self, document):
        pass

    def fax(self, document):
        pass
```
This class represents a typical printer that can perform multiple tasks. However, not all clients that use a printer need all these methods. For example, a client that only needs to print documents should not be forced to depend on the `scan`, `copy`, or `fax` methods.

To follow the ISP, we can split the `Printer` class into several smaller interfaces, each with a specific set of methods that are relevant to a specific client.
```python
class Printer:
    def print(self, document):
        pass

class Scanner:
    def scan(self, document):
        pass

class Copier:
    def copy(self, document):
        pass

class Fax:
    def fax(self, document):
        pass
```
Now, each client can depend on the specific interface that it needs, without being forced to depend on methods it does not use. For example, a client that only needs to print documents can depend on the `Printer` interface, while a client that needs to scan documents can depend on the `Scanner` interface.
```python
class SimplePrinter:
    def __init__(self, printer: Printer):
        self.printer = printer

    def print_document(self, document):
        self.printer.print(document)

class SimpleScanner:
    def __init__(self, scanner: Scanner):
        self.scanner = scanner

    def scan_document(self):
        return self.scanner.scan()

class SimpleCopier:
    def __init__(self, copier: Copier):
        self.copier = copier

    def copy_document(self, document):
        return self.copier.copy(document)
```
In this example, we have three simple client classes that depend on a single interface. By following the ISP, we can ensure that our software is more maintainable and flexible, with interfaces that are focused and cohesive, and clients that are only dependent on the methods they use.


#### D - Dependency Inversion Principle (DIP)

The Dependency Inversion Principle (DIP) is a design principle that states that high-level modules should not depend on low-level modules. Instead, both should depend on abstractions. Additionally, abstractions should not depend on details; details should depend on abstractions. This principle encourages us to use interfaces or abstract classes to decouple modules and improve the flexibility of our software.

To understand the DIP, let's consider an example of a `UserService` class that depends on a low-level `UserRepository` class.
```python
class UserRepository:
    def get_user(self, user_id):
        pass

class UserService:
    def __init__(self, repository: UserRepository):
        self.repository = repository

    def get_user(self, user_id):
        return self.repository.get_user(user_id)

```
In this example, the `UserService` depends on the `UserRepository` class, which represents a low-level module that interacts with the database. This creates a tight coupling between the two modules, making it difficult to change the implementation of the `UserRepository` or replace it with a different data source.

To follow the DIP, we can introduce an interface or an abstract class that represents the functionality of the `UserRepository`, and make both the `UserService` and `UserRepository` depend on it.
```python
from abc import ABC, abstractmethod

class UserRepositoryInterface(ABC):
    @abstractmethod
    def get_user(self, user_id):
        pass

class UserRepository(UserRepositoryInterface):
    def get_user(self, user_id):
        pass

class UserService:
    def __init__(self, repository: UserRepositoryInterface):
        self.repository = repository

    def get_user(self, user_id):
        return self.repository.get_user(user_id)
```
In this updated example, we've introduced an `UserRepositoryInterface` interface that defines the `get_user` method. Both the `UserRepository` and `UserService` classes now depend on this interface, rather than on the implementation of the `UserRepository`. This means that we can easily replace the implementation of the `UserRepository` with a different data source, as long as it conforms to the `UserRepositoryInterface`.

By following the DIP, we can make our software more flexible and easier to maintain, with decoupled modules that depend on abstractions rather than on concrete implementations.


By following these principles, developers can create software systems that are easier to maintain, more flexible, and more robust. These principles can be applied to any programming language or system, including Python, and can help developers create code that is of higher quality, easier to understand, and more maintainable over time.
