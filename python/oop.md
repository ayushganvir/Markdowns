# Object Oriented Programming In Python 3
## Main concepts of OOP in Python
* Class
* Objects
* Polymorphism
* Encapsulation
* Inheritance
* Data Abstraction


> Part of Inheritance

__Checking if Classes are Inherited or an Instance of a Class__
```python

print(issubclass(Derived, Base))
print(issubclass(Base, Derived))
  
d = Derived()
b = Base()
  
# b is not an instance of Derived
print(isinstance(b, Derived))
  
# But d is an instance of Base
print(isinstance(d, Base))

```
## Class
Collection of Objects. Contains the blueprint/prototype of that type objects.

* Classes are created by keyword class.
* Attributes are the variables that belong to a class.
* Attributes are always public and can be accessed using the dot (.) operator. Eg.: Myclass.Myattribute

## Objects
An object consists of :

* State: It is represented by the attributes of an object. It also reflects the properties of an object.
* Behavior: It is represented by the methods of an object. It also reflects the response of an object to other objects.
* Identity: It gives a unique name to an object and enables one object to interact with other objects.

## The ____init\_\___ method 
It is the constructor, run as soon as an object of a class is instantiated. The method is useful to do any initialization you want to do with your object. 

```python
class Dog:
 
    # class attribute
    attr1 = "mammal"
 
    # Instance attribute
    def __init__(self, name):
        self.name = name
 
# Driver code
# Object instantiation
Rodger = Dog("Rodger")
Tommy = Dog("Tommy")
```

## Inheritance
Inheritance is the capability of one class to derive or inherit the properties from another class. The class that derives properties is called the derived class or child class and the class from which the properties are being derived is called the base class or parent class.

## Polymorphism
Polymorphism simply means having many forms. 

>if the parent class has a method named ABC then the child class also can have a method with the same name ABC having its own parameters and variables.
```python

class Bird:
   
    def intro(self):
        print("There are many types of birds.")
 
    def flight(self):
        print("Most of the birds can fly but some cannot.")
 
class sparrow(Bird):
   
    def flight(self):
        print("Sparrows can fly.")
 
class ostrich(Bird):
 
    def flight(self):
        print("Ostriches cannot fly.")

```
## Encapsulation
It describes the idea of wrapping data and the methods that work on data within one unit.

 This puts restrictions on accessing variables and methods directly and can prevent the accidental modification of data.
 
  To prevent accidental change, an object’s variable can only be changed by an object’s method. Those types of variables are known as __private variables__.

## Data Abstraction

