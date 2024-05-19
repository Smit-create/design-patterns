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

# Composite Pattern

## Introduction

The Composite Pattern is a structural design pattern that allows you to compose objects into tree-like structures to represent part-whole hierarchies. This pattern enables clients to treat individual objects and compositions of objects uniformly.

## Motivation

In software development, there are scenarios where you need to work with tree structures or hierarchies, such as organizational charts, file systems, or graphical user interfaces. The Composite Pattern simplifies these scenarios by providing a unified interface for both individual objects and composites.

## Implementation

Let's dive into the implementation of the Composite Pattern. In this pattern, we define a component interface or abstract class that declares common operations for both simple and complex objects. We then create concrete implementations for leaf objects and composite objects.

We define a `FileSystemComponent` interface or abstract class with a method `show_details`, representing the common operation.

```{code-cell} ipython3
from abc import ABC, abstractmethod

# Component
class FileSystemComponent(ABC):
    @abstractmethod
    def show_details(self):
        pass
```

Now, we define a `File` class as the leaf, which implements the `FileSystemComponent` interface.

```{code-cell} ipython3
# Leaf
class File(FileSystemComponent):
    def __init__(self, name):
        self.name = name

    def show_details(self):
        print(f"File: {self.name}")
```

Finally, we define a `Directory` class as the composite, which also implements the `FileSystemComponent` interface and contains a collection of `FileSystemComponent` objects. The composite class implements methods to add, remove, and show details of its child components.

```{code-cell} ipython3
# Composite
class Directory(FileSystemComponent):
    def __init__(self, name):
        self.name = name
        self.components = []

    def add(self, component):
        self.components.append(component)

    def remove(self, component):
        self.components.remove(component)

    def show_details(self):
        print(f"Directory: {self.name}")
        for component in self.components:
            component.show_details()
```

Now, let's see how we can use the Composite Pattern in a real-life example of a file system with directories and files.

```{code-cell} ipython3
# Client code
root_directory = Directory("root")
home_directory = Directory("home")
user_directory = Directory("user")

file1 = File("file1.txt")
file2 = File("file2.txt")
file3 = File("file3.txt")
```

In this example:

- We create `Directory` objects representing the root, home, and user directories.
- We create `File` objects representing individual files.
- We build a tree structure by adding directories and files to their respective parents.
- We call the `show_details` method on the root directory to display the details of the entire file system hierarchy.

```{code-cell} ipython3
# Build the tree structure
root_directory.add(home_directory)
home_directory.add(user_directory)
user_directory.add(file1)
user_directory.add(file2)
root_directory.add(file3)

# Show details of the file system
root_directory.show_details()
```

## Benefits & Drawbacks

```{admonition} Benefits
- Simplifies client code by treating individual objects and compositions uniformly.
- Makes it easier to work with tree structures and hierarchies.
- Promotes flexibility and extensibility in the system.
```

```{admonition} Drawbacks
- Can add complexity to the design, especially when dealing with large hierarchies.
- Requires careful management of the relationships between components and composites.
```
