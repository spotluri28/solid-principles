# Solid-Principles

The SOLID principles are a valuable set of guidelines for writing better code. By following these principles, you can write code that is easier to understand, maintain, and extend.
In Python programming, "solid principles" typically refer to the SOLID principles, which are a set of five design principles that help you create maintainable and scalable software. These

principles were introduced by Robert C. Martin and are commonly used in object-oriented programming. The SOLID acronym stands for:

**Single Responsibility Principle (SRP)**: This principle states that a class should have only one reason to change. In other words, a class should have only one responsibility or job. This promotes modular and maintainable code.
A class should have only one responsibility. This means that each class should only do one thing and do it well.

So, what does that mean actually? While you design your logic in either class or method, _**you should not be writing all kinds of responsibilities in one place**_. This will make your code quite complex and unmanageable. It will also be difficult to adjust new changes later as there are high chances it will affect the other functionality and you will end up testing all the functionalities even though it is a smaller change.

# Single Responsibility Principle (SRP)

```class FileManager:
    def __init__(self, file_path):
        self.file_path = file_path

    def read_file(self):
        pass

    def write_file(self, data):
        pass

    def encrypt_data(self, data):
        pass

    def decrypt_data(self, data):
        pass
```

In this example, the `FileManager` class violates SRP because it has multiple responsibilities. It is responsible for file management operations such as reading and writing files, as well as performing encryption and decryption of data.
To apply SRP, we can separate the responsibilities into different classes:

```class FileManager
    def __init__(self, file_path):
        self.file_path = file_path

    def read_file(self):
        pass

    def write_file(self, data):
        pass

class DataEncryptor:
    def encrypt_data(self, data):
        pass

    def decrypt_data(self, data):
        pass
```

In this refactored version, the `FileManager` class now focuses solely on file management operations. It handles reading and writing data to/from a file. On the other hand, the `DataEncryptor` class is responsible for encrypting and decrypting data.

**Open-Closed Principle (OCP)**: According to the OCP, software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. You should be able to add new functionality without changing existing code.
It means that we should be able to add new functionality to the code without having to modify existing classes. By including new features without the need to change existing code, we can reduce bugs.

![image](https://github.com/developer-Akhil/Solid-Principles/assets/64408106/34543303-34c2-4f51-a50c-18f850bc3c94)

```class Person:
    def __init__(self, name):
        self.name = name
    def __repr__(self):
        return f'Person(name={self.name})'

class PersonStorage:
    def save_to_database(self, person):
        print(f'Save the {person} to database')

    def save_to_json(self, person):
        print(f'Save the {person} to a JSON file')

if __name__ == '__main__':
    person = Person('John Doe')
    storage = PersonStorage()
    storage.save_to_database(person)
```
    
The open-closed principle example

![image](https://github.com/developer-Akhil/Solid-Principles/assets/64408106/a150062b-9e4e-4c29-abaf-ca66d0830637)

```from abc import ABC, abstractmethod

class Person:
    def __init__(self, name):
        self.name = name

    def __repr__(self):
        return f'Person(name={self.name})'

class PersonStorage(ABC):
    @abstractmethod
    def save(self, person):
        pass

class PersonDB(PersonStorage):
    def save(self, person):
        print(f'Save the {person} to database')

class PersonJSON(PersonStorage):
    def save(self, person):
        print(f'Save the {person} to a JSON file')
```

**Liskov Substitution Principle (LSP):** This principle states that objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program. Subclasses should be able to extend but not violate the behaviour of the parent class.
The Liskov Substitution Principle, introduced by Barbara Liskov in 1987, is a crucial principle that governs the behaviour of objects in an object-oriented system. Simply put, _**it states that objects of a superclass should be seamlessly replaceable with objects of its subclass, without causing any issues or compromising the correctness of the program**_. This means that a subclass should be able to effortlessly substitute its parent class wherever it is expected.

```
class KitchenAppliance:
    def on(self):
        pass

    def off(self):
        pass

    def set_temperature(self):
        pass

class Toaster(KitchenAppliance):

    def on(self):
        pass

    def off(self):
        pass

    def set_temperature(self):
        pass

class Juicer(KitchenAppliance):

    def on(self):
        pass

    def off(self):
        pass
```
**Resolving LSP**
```
class KitchenAppliance:
    def on(self):
        pass

    def off(self):
        pass

class KitchenApplicaneWithTemp(KitchenAppliance):

    def set_temperature(self):
        pass

class Toaster(KitchenApplicaneWithTemp):

    def on(self):
        pass

    def off(self):
        pass

    def set_temperature(self):
        pass

class Juicer(KitchenAppliance):

    def on(self):
        pass

    def off(self):
        pass
```
**Interface Segregation Principle (ISP)**: ISP suggests that clients should not be forced to depend on interfaces they do not use. In Python, this often translates to having smaller, focused interfaces rather than large, monolithic ones.
_Uncle Bob, who is the originator of the SOLID term, explains the interface segregation principle by advising that “Make fine-grained interfaces that are client-specific. Clients should not be forced to implement interfaces they do not use.”_

**The Problem**: Violation of ISP in Poor Design
Consider the following code snippet:	

```
from abc import ABC, abstractmethod

class Printer(ABC):
    @abstractmethod
    def print(self, document):
        pass

    @abstractmethod
    def fax(self, document):
        pass

    @abstractmethod
    def scan(self, document):
        pass

class OldPrinter(Printer):
    def print(self, document):
        print(f"Printing {document} in black and white...")

    def fax(self, document):
        raise NotImplementedError("Fax functionality not supported")

    def scan(self, document):
        raise NotImplementedError("Scan functionality not supported")

class ModernPrinter(Printer):
    def print(self, document):
        print(f"Printing {document} in color...")

    def fax(self, document):
        print(f"Faxing {document}...")

    def scan(self, document):
        print(f"Scanning {document}...")
```
In this example, the base class **Printer** defines an interface that its subclasses are required to implement. However, the **OldPrinter** subclass doesn't utilize the **fax()** and **scan()** methods because it lacks support for these functionalities.
Unfortunately, this design violates the ISP as it forces **OldPrinter** to expose an interface that it neither implements nor requires.
**The Solution**: Applying ISP for Better Design
To address the violation of ISP, it is advisable to separate the interfaces into smaller, more specialized classes. Let's take a look at an improved design:

```
from abc import ABC, abstractmethod

class Printer(ABC):
    @abstractmethod
    def print(self, document):
        pass

class Fax(ABC):
    @abstractmethod
    def fax(self, document):
        pass

class Scanner(ABC):
    @abstractmethod
    def scan(self, document):
        pass

class OldPrinter(Printer):
    def print(self, document):
        print(f"Printing {document} in black and white...")

class NewPrinter(Printer, Fax, Scanner):
    def print(self, document):
        print(f"Printing {document} in color...")

    def fax(self, document):
        print(f"Faxing {document}...")

    def scan(self, document):
        print(f"Scanning {document}...")
```
In this revised design, the base classes—Printer, Fax, and Scanner—provide distinct interfaces, each responsible for a single functionality. The OldPrinter class only inherits the Printer interface, ensuring that it doesn't have any unused methods. On the other hand, the NewPrinter class inherits from all the interfaces, incorporating the complete set of functionalities. This segregation of the Printer interface enables the creation of various machines with different combinations of functionalities, enhancing flexibility and extensibility.

**Dependency Inversion(flip) Principle (DIP):** 
	High level modules or low-level modules in your code should not depend on the actual implementation. They should depend on abstractions.
	Abstractions should not depend on implementation. Implementation should depend on abstractions.
DIP encourages high-level modules to depend on abstractions (interfaces or abstract classes) rather than concrete implementations. This promotes loose coupling between components.
High-level modules should not depend on low-level modules. Both should depend on abstractions. This means that you should design your code so that high-level modules do not depend on specific low-level modules. Instead, they should depend on abstract interfaces.

![image](https://github.com/developer-Akhil/Solid-Principles/assets/64408106/bed4809b-38cc-4b23-9eed-be903599a72e)

```
class Apple:
    def eat(self):
        print(f"Eating Apple. Transferring {5} units of energy to brain...")

class Chocolate:
    def eat(self):
        print(f"Eating Chocolate. Transferring {10} units of energy to brain...")

class Robot:
    def get_energy(self, eatable: str):
        if eatable == "Apple":
            apple = Apple()
            apple.eat()
        elif eatable == "Chocolate":
            chocolate = Chocolate()
            chocolate.eat()

if __name__ == '__main__':
    robot = Robot()
    robot.get_energy("Apple")
```

![image](https://github.com/developer-Akhil/Solid-Principles/assets/64408106/02c8f688-2551-48a1-baab-1442cf3ce473)


```
from abc import ABC, abstractmethod

class Eatable(ABC):
    @abstractmethod
    def eat(self):
        return NotImplemented

class Apple(Eatable):
    def eat(self):
        print(f"Eating Apple. Transferring {5} units of energy to brain...")

class Chocolate(Eatable):
    def eat(self):
        print(f"Eating Chocolate. Transferring {10} units of energy to brain...")

class Robot:
    def get_energy(self, eatable: Eatable):
        eatable.eat()
if __name__ == '__main__':
    robot = Robot()
    robot.get_energy(Apple())
```
