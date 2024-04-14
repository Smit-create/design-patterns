---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.15.2
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

# Factory Method Pattern

## Introduction

The Factory Method Pattern is a creational design pattern that provides an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created. It encapsulates the object creation logic, making it easier to manage and extend.

## Motivation

In software development, there are scenarios where the exact type of object to be created may not be known until runtime or may vary based on certain conditions. The Factory Method Pattern addresses this by delegating the responsibility of object creation to subclasses, thereby promoting loose coupling between the creator and the product.

## Implementation

Let's dive into the implementation of the Factory Method Pattern. In this pattern, we define an interface or abstract class for creating objects, and subclasses implement the factory method to create specific types of objects.

Let's consider a real-life example of a Factory Method Pattern in a scenario where we have different types of employees being hired by a company.

### Example: Employee Hiring System

Suppose you're developing a system for a company to hire different types of employees: full-time employees, part-time employees, and contractors. Each type of employee has different attributes and benefits.

We can use the Factory Method Pattern to create a flexible system for hiring different types of employees.

We start by defining an `Employee` interface or abstract class with a method `calculate_bonus`, which represents the bonus calculation for different types of employees.

```{code-cell} ipython3
from abc import ABC, abstractmethod

class Employee(ABC):
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

    @abstractmethod
    def calculate_bonus(self):
        pass

    @abstractmethod
    def type_name(self):
        pass
```

 Now, we define concrete implementations of the Employee interface: `FullTimeEmployee`, `PartTimeEmployee`, and `Contractor`, each implementing the `calculate_bonus` method based on their specific rules.

```{code-cell} ipython3
class FullTimeEmployee(Employee):
    def calculate_bonus(self):
        # Full-time employees get 10% bonus
        return self.salary * 0.1

    def type_name(self):
        return "FullTimeEmployee"

class PartTimeEmployee(Employee):
    def calculate_bonus(self):
        # Part-time employees get 5% bonus
        return self.salary * 0.05

    def type_name(self):
        return "PartTimeEmployee"

class Contractor(Employee):
    def calculate_bonus(self):
        # Contractors don't receive bonus
        return 0

    def type_name(self):
        return "Contractor"
```

Now, let's define a `HiringManager` interface or abstract class with a factory method `make_employee`, which is responsible for creating instances of employees.

```{code-cell} ipython3
class HiringManager(ABC):
    @abstractmethod
    def make_employee(self, name, salary):
        pass
```

We define concrete implementations of the `HiringManager` interface: `HRManager` and `Recruiter`, each implementing the `make_employee` method to create specific types of employees based on certain conditions.

```{code-cell} ipython3
class HRManager(HiringManager):
    def make_employee(self, name, salary):
        return FullTimeEmployee(name, salary)

class Recruiter(HiringManager):
    def make_employee(self, name, salary):
        if salary < 50000:
            return PartTimeEmployee(name, salary)
        else:
            return Contractor(name, salary)
```

Now, let's see how we can use the Employee Hiring System:

```{code-cell} ipython3
# Create hiring managers
hr_manager = HRManager()
recruiter = Recruiter()
```

```{code-cell} ipython3
# Hire employees using factory methods
employee1 = hr_manager.make_employee("John", 60000)
employee2 = recruiter.make_employee("Alice", 40000)
employee3 = recruiter.make_employee("Bob", 80000)
```

```{code-cell} ipython3
# Calculate bonuses for employees
bonus1 = employee1.calculate_bonus()
bonus2 = employee2.calculate_bonus()
bonus3 = employee3.calculate_bonus()
```

```{code-cell} ipython3
print(f"{employee1.name}: Bonus = ${bonus1}, Type: {employee1.type_name()}")
print(f"{employee2.name}: Bonus = ${bonus2}, Type: {employee2.type_name()}")
print(f"{employee3.name}: Bonus = ${bonus3}, Type: {employee3.type_name()}")
```

In this example, based on the salary provided, the hiring manager (`HRManager` or `Recruiter`) decides which type of employee to create using the factory method `make_employee`.

Each employee's bonus is then calculated using the `calculate_bonus` method specific to their type.


## Benefits & Drawbacks

```{admonition} Benefits
- Promotes loose coupling between creator and product classes.
- Allows for easy extension by adding new creator and product subclasses.
- Encapsulates object creation logic, making it easier to manage and maintain.
```

```{admonition} Drawbacks
- May result in a proliferation of subclasses if there are many product variations.
- Requires the creation of additional classes, which can increase complexity.
```
