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

# Singleton Pattern


## Introduction

The **Singleton Pattern** is one of the creational design patterns that ensures a class has only one instance and provides a global point of access to that instance. This pattern is commonly used when only one instance of a class is needed throughout the lifetime of an application, such as managing a shared resource or maintaining global state.


## Motivation

Imagine a scenario where you need to ensure that only one instance of a class is created and shared across multiple parts of your application. This could be a logger, a database connection, or a configuration manager. Using the Singleton Pattern, you can guarantee that there is only one instance of the class, and all requests for that instance return the same object.

## Implementation

Let's dive into the implementation of the Singleton Pattern. There are several ways to implement the Singleton Pattern in Python, but one of the most common approaches is to use a class variable to store the instance and a class method to provide access to that instance.

Suppose you're developing a Python application that requires logging messages to a file. You want to ensure that there is only one logger instance throughout the application to maintain consistency and prevent multiple log files from being created.

Let's implement the `Logger` class as a Singleton.

```{code-cell} ipython3
class Logger:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            # Open the log file
            cls._instance._log_file = open("application.log", "a")
        return cls._instance

    def log(self, message):
        """Write a message to the log file"""
        self._log_file.write(message + '\n')

    def close(self):
        # Close log file when application terminates
        self._log_file.close()
```

In this example:

- We define a Logger class with a `_log_file` attribute to store the file object for writing log messages.
- The `_instance` class variable is used to store the single instance of the Logger class.
- In the `__new__` method, we check if the `_instance` variable is `None`. If it is, we create a new instance of the Logger class and open the log file in append mode. Otherwise, we return the existing instance.

```{code-cell} ipython3
# Create two logger instances
logger1 = Logger()
logger2 = Logger()
```

```{code-cell} ipython3
# Log messages using both of them
logger1.log("Info: Application started from logger 1")
logger2.log("Info: Application started from logger 2")
```

```{code-cell} ipython3
# Both instances refer to the same object
print(logger1 is logger2)
```

As you can see, both `logger1` and `logger2` refer to the same object, demonstrating that only one instance of the `Logger` class is created.

```{code-cell} ipython3
logger1.close()
```

In the above example, we ensure that there is only one instance of the `Logger` class throughout the application. All log messages are written to the same log file, maintaining consistency and preventing multiple log files from being created.


## Benefits & Drawbacks

```{admonition} Benefits
- Ensures that a class has only one instance.
- Provides a global point of access to that instance.
- Lazy initialization: The instance is created only when it is first requested.
```

```{admonition} Drawbacks
- Can introduce tight coupling between classes.
- Global state: Changes to the singleton object affect the entire application.
```
