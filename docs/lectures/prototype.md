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

# Prototype Pattern


## Introduction

The Prototype Pattern is a creational design pattern that allows for the creation of new objects by copying an existing object, known as the prototype. It is particularly useful when the cost of creating a new object by conventional means is high or when the initialization process is complex.

## Motivation

In software development, there are scenarios where creating new objects involves significant overhead, such as reading from a database or performing complex initialization. The Prototype Pattern addresses this by providing a mechanism to clone existing objects, reducing the cost and complexity of object creation.

## Implementation

Let's dive into the implementation of the Prototype Pattern. In this pattern, we define a prototype interface or abstract class with a method to clone itself, and concrete prototype classes that implement the cloning method.

We define a `Prototype` interface or abstract class with a `clone` method.

```{code-cell} ipython3
from copy import deepcopy

class Prototype:
    def clone(self):
        pass
```

We define a concrete implementation of the `Prototype` interface: `ProductPrototype`, which implements the `clone` method to create a deep copy of itself using the `deepcopy` function from the `copy` module.

```{code-cell} ipython3
class ProductPrototype(Prototype):
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def clone(self):
        return deepcopy(self)
```

```{code-cell} ipython3
original_product = ProductPrototype("Smartphone", 500)
cloned_product = original_product.clone()

print("Original Product:", original_product.name, original_product.price)
print("Cloned Product:", cloned_product.name, cloned_product.price)
```

`ProductPrototype` class acts as the concrete prototype, representing a product in some application. We create an original product object and then clone it to create a new product with the same attributes.


## Benefits & Drawbacks

```{admonition} Benefits
- Reduces the cost of creating new objects by avoiding complex initialization.
- Allows for the creation of new objects by copying existing ones, reducing overhead.
- Provides a flexible and scalable approach to object creation.
```

```{admonition} Drawbacks
- Deep copying can be resource-intensive, especially for complex objects with nested structures.
- Requires careful handling of object references and mutable state.
```
