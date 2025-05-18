## Notebook 12

### 1. Introduction to OOP

Object-Oriented Programming (OOP) is a programming paradigm based on the concept of "objects" which can contain data and code. Data comes in the form of attributes (fields), and code in the form of methods (procedures).

OOP was developed to handle the complexity of large software systems by organizing code around objects rather than actions and logic.

### 2. What is OOP?

**Definition**: OOP is a programming paradigm based on the concept of "objects" that contain data and procedures. It aims to implement real-world entities like inheritance, polymorphism, encapsulation, etc., in programming.

**Benefits**:
- **Modularity**: Code is divided into separate objects that can be modified without affecting other parts
- **Reusability**: Objects can be reused across programs
- **Maintainability**: Easier to maintain, modify, and debug
- **Scalability**: OOP code is easier to scale for larger applications
- **Real-world modeling**: Maps naturally to real-world objects and relationships

### 3. Key Principles of OOP

#### Encapsulation
Encapsulation is the bundling of data and methods that operate on that data within a single unit (class). It restricts direct access to some of an object's components, hiding the internal state and requiring interaction through defined interfaces.

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.__owner = owner     # private attribute
        self.__balance = balance # private attribute
    
    def deposit(self, amount):
        self.__balance += amount
        return self.__balance
    
    def withdraw(self, amount):
        if amount <= self.__balance:
            self.__balance -= amount
            return True
        return False
```

#### Abstraction
Abstraction is the concept of hiding complex implementation details and showing only the necessary features of an object. It reduces complexity by hiding unnecessary details from the user.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
```

#### Inheritance
Inheritance is the mechanism where a new class inherits properties and behaviors from an existing class. This promotes code reuse and establishes a relationship between classes.

```python
class Animal:
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return "Woof!"
```

#### Polymorphism
Polymorphism allows methods to do different things based on the object it is acting upon. It enables one interface to be used for a general class of actions.

```python
class Cat(Animal):
    def speak(self):
        return "Meow!"

def animal_sound(animal):
    print(animal.speak())

animal_sound(Dog())  # Output: Woof!
animal_sound(Cat())  # Output: Meow!
```

### 4. Basics of Classes & Objects

**Class**: A blueprint for creating objects, defining attributes and behavior.

**Object/Instance**: A specific realization of a class, with actual values for attributes.

**Self**: A reference to the instance of the class. It's the first parameter in any instance method.

**Instantiation**: The process of creating an object from a class.

```python
class Person:
    def __init__(self, name, age):
        self.name = name  # instance attribute
        self.age = age    # instance attribute
    
    def greet(self):
        return f"Hello, my name is {self.name}"

# Instantiation
person1 = Person("Alice", 30)  # Creating an object
print(person1.greet())  # Output: Hello, my name is Alice
```

### 5. Constructors & Destructors

**Constructor**: A special method called when an object is created. In Python, it's `__init__`.

**Destructor**: A special method called when an object is destroyed. In Python, it's `__del__`.

```python
class Example:
    def __new__(cls, *args, **kwargs):
        # Called before __init__, creates and returns the instance
        print("Creating a new instance")
        return super().__new__(cls)
    
    def __init__(self, value):
        # Called when an instance is created
        print("Initializing the instance")
        self.value = value
    
    def __del__(self):
        # Called when instance is garbage collected
        print(f"Destroying instance with value: {self.value}")
```

### 6. Class Attributes vs. Instance Attributes

**Class Attributes**: Attributes shared by all instances of a class. Defined outside any method.

**Instance Attributes**: Attributes specific to each instance. Defined in the `__init__` method.

```python
class Dog:
    # Class attribute
    species = "Canis familiaris"
    
    def __init__(self, name, breed):
        # Instance attributes
        self.name = name
        self.breed = breed

dog1 = Dog("Rex", "German Shepherd")
dog2 = Dog("Buddy", "Golden Retriever")

print(dog1.species)  # Output: Canis familiaris
print(dog2.species)  # Output: Canis familiaris
print(dog1.name)     # Output: Rex
print(dog2.name)     # Output: Buddy
```

### 7. Methods in Python Classes

#### Instance Methods
Methods that operate on instance data. They take `self` as their first parameter.

```python
class Circle:
    def __init__(self, radius):
        self.radius = radius
    
    # Instance method
    def area(self):
        return 3.14 * self.radius ** 2
```

#### Class Methods
Methods that operate on class data. They take `cls` as their first parameter and are decorated with `@classmethod`.

```python
class Circle:
    all_circles = []
    
    def __init__(self, radius):
        self.radius = radius
        Circle.all_circles.append(self)
    
    # Class method
    @classmethod
    def total_circles(cls):
        return len(cls.all_circles)
```

#### Static Methods
Methods that don't operate on instance or class data. They don't take `self` or `cls` parameters and are decorated with `@staticmethod`.

```python
class MathOperations:
    # Static method
    @staticmethod
    def add(x, y):
        return x + y
```

### 8. Encapsulation

#### Access Modifiers in Python:
- **Public**: No underscore prefix, accessible from anywhere
- **Protected**: Single underscore prefix (`_name`), a convention indicating it shouldn't be accessed outside the class/subclasses
- **Private**: Double underscore prefix (`__name`), name-mangled to prevent accidental access

#### Getters/Setters
Methods that control access to an attribute.

```python
class Person:
    def __init__(self, name, age):
        self.__name = name  # private
        self.__age = age    # private
    
    # Getter
    def get_age(self):
        return self.__age
    
    # Setter
    def set_age(self, age):
        if age > 0:
            self.__age = age
        else:
            print("Age must be positive")
```

### 9. Inheritance

#### Single Inheritance
A class inherits from only one parent class.

```python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        pass

class Dog(Animal):  # Dog inherits from Animal
    def speak(self):
        return "Woof!"
```

#### Multiple Inheritance
A class inherits from multiple parent classes.

```python
class Flyable:
    def fly(self):
        return "I can fly"

class Swimmable:
    def swim(self):
        return "I can swim"

class Duck(Flyable, Swimmable):
    pass

duck = Duck()
print(duck.fly())  # Output: I can fly
print(duck.swim())  # Output: I can swim
```

#### Super() and Method Overriding
`super()` calls the parent class's method. Method overriding is redefining a method in a child class.

```python
class Vehicle:
    def __init__(self, make, model):
        self.make = make
        self.model = model

class Car(Vehicle):
    def __init__(self, make, model, year):
        super().__init__(make, model)  # Call parent's __init__
        self.year = year  # Add Car-specific attribute
```

### 10. Polymorphism

#### Method Overriding
Redefining a method in a child class.

```python
class Animal:
    def sound(self):
        return "Some generic sound"

class Dog(Animal):
    # Override parent method
    def sound(self):
        return "Woof!"
```

#### Operator Overloading
Defining how operators behave with user-defined classes.

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    # Overload the + operator
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
```

#### Duck Typing
"If it walks like a duck and quacks like a duck, then it's a duck." Python focuses on behavior rather than type.

```python
def make_sound(animal):
    # No type checking, just call the method
    print(animal.sound())

# Any object with a sound() method will work
make_sound(Dog())
```

### 11. Abstraction

#### Abstract Base Classes & Methods (abc module)
A way to define interfaces or contracts for classes. Child classes must implement the abstract methods.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
    
    @abstractmethod
    def perimeter(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)
```

### 12. The Object Class

Every class in Python implicitly inherits from `object`. It provides default implementations for several special methods:

```python
class MyClass:  # Implicitly inherits from object
    pass

# Some dunder methods from object:
# __new__, __init__, __str__, __repr__, __eq__, __hash__, etc.
```

### 13. Special (Magic/Dunder) Methods

These methods, with double underscores, allow you to define how Python operators and built-in functions interact with your classes.

```python
class Book:
    def __init__(self, title, author, pages):
        self.title = title
        self.author = author
        self.pages = pages
    
    def __str__(self):
        return f"{self.title} by {self.author}"
    
    def __len__(self):
        return self.pages
    
    def __eq__(self, other):
        return self.title == other.title and self.author == other.author

book = Book("Python 101", "John Doe", 200)
print(str(book))    # Output: Python 101 by John Doe
print(len(book))    # Output: 200
```

## Notebook 13

### 1. Class & Static Variables

**Class variables** are shared among all instances of a class, while static variables in Python are essentially the same as class variables.

```python
class Employee:
    # Class variable
    company = "TechCorp"
    employee_count = 0
    
    def __init__(self, name):
        self.name = name  # Instance variable
        Employee.employee_count += 1
```

### 2. Composition vs. Aggregation

**Composition** is a "has-a" relationship where one object is composed of one or more other objects. If the parent object is destroyed, all of its composed objects are also destroyed.

**Aggregation** is a weaker form of composition where the child objects can exist independently of the parent.

```python
# Composition example
class Engine:
    def start(self):
        return "Engine started"

class Car:
    def __init__(self):
        self.engine = Engine()  # Car has-an Engine

# Aggregation example
class School:
    def __init__(self, students=None):
        self.students = students or []  # School aggregates Students

class Student:
    def __init__(self, name):
        self.name = name
        
# Students can exist without a School
student1 = Student("Alice")
student2 = Student("Bob")
school = School([student1, student2])
```

### 3. Method Resolution Order (MRO)

MRO defines the order in which Python looks for methods and attributes in a class hierarchy, especially important in multiple inheritance.

```python
class A:
    def method(self):
        return "A"

class B(A):
    def method(self):
        return "B"

class C(A):
    def method(self):
        return "C"

class D(B, C):
    pass

print(D.mro())  # Shows the MRO for class D
# Output: [<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>]

d = D()
print(d.method())  # Output: B (from class B)
```

### 4. Decorators in Classes

#### Class Decorators
Functions that modify or enhance classes.

```python
def add_greeting(cls):
    cls.greet = lambda self: f"Hello, I'm {self.name}"
    return cls

@add_greeting
class Person:
    def __init__(self, name):
        self.name = name

person = Person("Alice")
print(person.greet())  # Output: Hello, I'm Alice
```

#### Property Decorators
Control access to attributes using `@property`, `@setter`, and `@deleter`.

```python
class Person:
    def __init__(self, name):
        self._name = name
    
    @property
    def name(self):
        return self._name
    
    @name.setter
    def name(self, value):
        if not value:
            raise ValueError("Name cannot be empty")
        self._name = value
    
    @name.deleter
    def name(self):
        del self._name

person = Person("Alice")
print(person.name)  # Output: Alice
person.name = "Bob"  # Uses setter
print(person.name)  # Output: Bob
del person.name  # Uses deleter
```

### 5. Callable Objects

Objects that can be called like functions by implementing the `__call__` method.

```python
class Counter:
    def __init__(self):
        self.count = 0
    
    def __call__(self):
        self.count += 1
        return self.count

counter = Counter()
print(counter())  # Output: 1
print(counter())  # Output: 2
print(callable(counter))  # Output: True
```

### 6. Modules & Packages in OOP

Modules are Python files, and packages are directories containing modules. They help organize code into reusable components.

```
my_package/
├── __init__.py
├── module1.py
└── subpackage/
    ├── __init__.py
    └── module2.py
```

```python
# In module1.py
class MyClass:
    pass

# In another file
from my_package.module1 import MyClass
from my_package.subpackage.module2 import AnotherClass
```

### 7. Advanced OOP Concepts

#### Metaclasses
Classes of classes - they define how classes behave.

```python
class Meta(type):
    def __new__(cls, name, bases, attrs):
        # Add a method to the class
        attrs['custom_method'] = lambda self: "This is a custom method"
        return super().__new__(cls, name, bases, attrs)

class MyClass(metaclass=Meta):
    pass

obj = MyClass()
print(obj.custom_method())  # Output: This is a custom method
```

#### Singleton Pattern
Ensures a class has only one instance.

```python
class Singleton:
    _instance = None
    
    def __new__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

s1 = Singleton()
s2 = Singleton()
print(s1 is s2)  # Output: True
```

#### Factory Pattern
Creates objects without specifying the exact class.

```python
class Dog:
    def speak(self):
        return "Woof!"

class Cat:
    def speak(self):
        return "Meow!"

class AnimalFactory:
    def create_animal(self, animal_type):
        if animal_type == "dog":
            return Dog()
        elif animal_type == "cat":
            return Cat()
        else:
            raise ValueError("Unknown animal type")

factory = AnimalFactory()
animal = factory.create_animal("dog")
print(animal.speak())  # Output: Woof!
```

### 8. Error Handling in OOP

Creating custom exceptions and using try-except blocks for error handling.

```python
class InsufficientFundsError(Exception):
    """Raised when there are insufficient funds for a withdrawal"""
    pass

class BankAccount:
    def __init__(self, balance=0):
        self.balance = balance
    
    def withdraw(self, amount):
        if amount > self.balance:
            raise InsufficientFundsError("Not enough funds")
        self.balance -= amount
        return amount

# Using the custom exception
account = BankAccount(100)
try:
    account.withdraw(200)
except InsufficientFundsError as e:
    print(f"Error: {e}")
```

### 9. Testing OOP Code with pytest

Writing tests for OOP code using the pytest framework.

```python
# file: test_bank_account.py
import pytest
from bank_account import BankAccount, InsufficientFundsError

def test_bank_account_init():
    account = BankAccount(100)
    assert account.balance == 100

def test_bank_account_deposit():
    account = BankAccount()
    account.deposit(100)
    assert account.balance == 100

def test_bank_account_withdraw():
    account = BankAccount(100)
    amount = account.withdraw(50)
    assert amount == 50
    assert account.balance == 50

def test_bank_account_insufficient_funds():
    account = BankAccount(100)
    with pytest.raises(InsufficientFundsError):
        account.withdraw(200)
```

### 10. Best Practices in OOP: SOLID Principles

SOLID is an acronym for five design principles intended to make software designs more maintainable and flexible:

1. **Single Responsibility Principle**: A class should have only one reason to change
2. **Open/Closed Principle**: Classes should be open for extension but closed for modification
3. **Liskov Substitution Principle**: Objects of a superclass should be replaceable with objects of its subclasses
4. **Interface Segregation Principle**: Many specific interfaces are better than one general interface
5. **Dependency Inversion Principle**: Depend on abstractions, not concretions

```python
# Single Responsibility Principle example
class FileManager:
    def read_file(self, file_path):
        # Read file content
        pass

class DataProcessor:
    def process_data(self, data):
        # Process the data
        pass

# Open/Closed Principle example
class Shape:
    def area(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14 * self.radius ** 2
```

### 11. Iterable Protocol & Iterators

Making classes that can be used in `for` loops by implementing `__iter__` and `__next__` methods.

```python
class Countdown:
    def __init__(self, start):
        self.start = start
    
    def __iter__(self):
        # Return an iterator object
        return CountdownIterator(self.start)

class CountdownIterator:
    def __init__(self, start):
        self.current = start
    
    def __next__(self):
        if self.current <= 0:
            raise StopIteration
        
        self.current -= 1
        return self.current + 1

# Using the iterable
for num in Countdown(5):
    print(num)  # Outputs: 5, 4, 3, 2, 1
```

### 12. Object-Based vs. Object-Oriented Languages

**Object-based languages** support objects but don't support inheritance and polymorphism. Examples include JavaScript (pre-ES6).

**Object-oriented languages** fully support objects, classes, inheritance, and polymorphism. Examples include Python, Java, and C++.

Python is a fully object-oriented language that also supports procedural and functional programming paradigms.

### 13. Python's Unified Type System

In Python, "everything is an object," meaning all entities (numbers, strings, functions, classes, etc.) are objects with identities, types, and values.

```python
# An integer is an object
num = 42
print(type(num))  # Output: <class 'int'>
print(num.__class__)  # Output: <class 'int'>

# A function is an object
def my_function():
    pass

print(type(my_function))  # Output: <class 'function'>

# Even a class is an object
class MyClass:
    pass

print(type(MyClass))  # Output: <class 'type'>
```
