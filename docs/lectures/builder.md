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

# Builder Pattern

## Introduction

The Builder Pattern is a creational design pattern that separates the construction of a complex object from its representation, allowing the same construction process to create different representations. It is particularly useful when dealing with objects that have multiple attributes or configurations.


## Motivation

In software development, there are scenarios where objects need to be created with numerous configuration options or attributes. Directly instantiating such objects with a constructor can lead to a complex and error-prone code. The Builder Pattern addresses this by encapsulating the construction logic and providing a fluent interface to set different attributes.


## Implementation

Let's dive into the implementation of the Builder Pattern. In this pattern, we define a builder class responsible for constructing the complex object step by step, and a director class that orchestrates the construction process.

We define a `Product` class representing the complex object to be constructed.

```{code-cell} ipython3
class Product:
    def __init__(self):
        self.part1 = None
        self.part2 = None
        self.part3 = None
```

Now, we define a `Builder` class with methods to construct different parts of the product and assemble them together.

```{code-cell} ipython3
class Builder:
    def __init__(self):
        self.product = Product()

    def build_part1(self, part1):
        self.product.part1 = part1

    def build_part2(self, part2):
        self.product.part2 = part2

    def build_part3(self, part3):
        self.product.part3 = part3

    def get_product(self):
        return self.product
```

We define a `Director` class that takes a builder object and orchestrates the construction process by invoking the builder's methods.

```{code-cell} ipython3
class Director:
    def __init__(self, builder):
        self.builder = builder

    def construct(self, part1, part2, part3):
        self.builder.build_part1(part1)
        self.builder.build_part2(part2)
        self.builder.build_part3(part3)
```

### Example

Now, let's see how we can use the Builder Pattern in a real-life example of building a custom meal at a restaurant.

```{code-cell} ipython3
class MealBuilder:
    def __init__(self):
        self.builder = Builder()

    def prepare_veg_meal(self):
        self.builder.build_part1("Salad")
        self.builder.build_part2("Vegetable Curry")
        self.builder.build_part3("Rice")
        return self.builder.get_product()

    def prepare_non_veg_meal(self):
        self.builder.build_part1("Chicken Soup")
        self.builder.build_part2("Grilled Chicken")
        self.builder.build_part3("Chicken Tikka")
        return self.builder.get_product()
```

`MealBuilder` class acts as the director, while the `Builder` class constructs different parts of the meal (`Product`). The `prepare_veg_meal` and `prepare_non_veg_meal` methods demonstrate the flexibility of the Builder Pattern in creating different representations of the product.

```{code-cell} ipython3
meal_builder = MealBuilder()
veg_meal = meal_builder.prepare_veg_meal()
non_veg_meal = meal_builder.prepare_non_veg_meal()

print("Veg Meal:")
print("Part 1:", veg_meal.part1)
print("Part 2:", veg_meal.part2)
print("Part 3:", veg_meal.part3)

print("\nNon-Veg Meal:")
print("Part 1:", non_veg_meal.part1)
print("Part 2:", non_veg_meal.part2)
print("Part 3:", non_veg_meal.part3)
```

## Benefits & Drawbacks

```{admonition} Benefits
- Separates the construction of a complex object from its representation.
- Provides a fluent interface for configuring the object's attributes.
- Allows for the construction of different representations using the same construction process.
```

```{admonition} Drawbacks
- Can introduce complexity, especially for objects with a large number of attributes.
- Requires the creation of additional classes, which can increase code overhead.
```
