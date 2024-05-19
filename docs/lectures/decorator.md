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

# Decorator Pattern

## Introduction

The Decorator Pattern is a structural design pattern that allows you to dynamically add behavior or responsibilities to objects without modifying their code. This pattern is useful for adhering to the Open/Closed Principle, which states that classes should be open for extension but closed for modification.

## Motivation

In software development, there are scenarios where you need to add responsibilities to individual objects dynamically and transparently. The Decorator Pattern addresses this by providing a flexible alternative to subclassing for extending functionality.

## Implementation

Let's dive into the implementation of the Decorator Pattern. In this pattern, we define a component interface that declares common operations, concrete components that implement the interface, and decorator classes that wrap concrete components to extend their behavior.

We define a `Beverage` interface with `cost` and `description` methods.

```{code-cell} ipython3
from abc import ABC, abstractmethod

# Component
class Beverage(ABC):
    @abstractmethod
    def cost(self):
        pass

    @abstractmethod
    def description(self):
        pass
```

We define an `Espresso` class as a concrete component that implements the `Beverage` interface.

```{code-cell} ipython3
# Concrete Component
class Espresso(Beverage):
    def cost(self):
        return 1.99

    def description(self):
        return "Espresso"
```

We define an `AddOnDecorator` abstract class that also implements the `Beverage` interface and wraps a `Beverage` object.

```{code-cell} ipython3
# Decorator
class AddOnDecorator(Beverage, ABC):
    def __init__(self, beverage):
        self._beverage = beverage

    @abstractmethod
    def cost(self):
        pass

    @abstractmethod
    def description(self):
        pass
```

We define `MilkDecorator` and `MochaDecorator` classes as concrete decorators that extend the behavior of the wrapped `Beverage` object.

```{code-cell} ipython3
# Concrete Decorators
class MilkDecorator(AddOnDecorator):
    def cost(self):
        return self._beverage.cost() + 0.50

    def description(self):
        return self._beverage.description() + ", Milk"

class MochaDecorator(AddOnDecorator):
    def cost(self):
        return self._beverage.cost() + 0.75

    def description(self):
        return self._beverage.description() + ", Mocha"
```

Now, let's see how we can use the Decorator Pattern in a real-life example of a coffee shop where customers can add various ingredients to their beverages.

```{code-cell} ipython3
# Client code
espresso = Espresso()
print(f"{espresso.description()} - ${espresso.cost()}")

# Add milk to the espresso
espresso_with_milk = MilkDecorator(espresso)
print(f"{espresso_with_milk.description()} - ${espresso_with_milk.cost()}")

# Add mocha to the espresso with milk
espresso_with_milk_and_mocha = MochaDecorator(espresso_with_milk)
print(f"{espresso_with_milk_and_mocha.description()} - ${espresso_with_milk_and_mocha.cost()}")
```

In this example:
- We create an `Espresso` object representing a simple coffee.
- We decorate the `Espresso` object with `MilkDecorator` to add milk.
- We further decorate the `Espresso` object with `MochaDecorator` to add mocha.
- We print the description and cost of each beverage to see how the decorators dynamically add behavior to the `Espresso`.

+++

## Benefits

- Allows for the dynamic addition of behavior to objects without modifying their code.
- Promotes adherence to the Open/Closed Principle.
- Provides flexibility and composability in extending object behavior.

## Drawbacks

- Can lead to a large number of small classes, increasing code complexity.
- May introduce performance overhead due to the additional layers of abstraction.
