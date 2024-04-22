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

# Bridge Pattern

## Introduction

The Bridge Pattern is a structural design pattern that decouples an abstraction from its implementation, allowing the two to vary independently. It achieves this by separating an abstraction from the specific details of how it is implemented.

## Motivation

In software development, there are scenarios where an abstraction needs to be decoupled from its implementation to improve flexibility, maintainability, and scalability. For example, an application may need to support multiple platforms, devices, or interfaces without altering the higher-level abstraction.

## Implementation

Let's dive into the implementation of the Bridge Pattern. In this pattern, we define an abstraction class with a reference to an implementer, and a hierarchy of concrete implementers. The abstraction class delegates the actual work to the implementer.

We define a `Device` interface or abstract class representing the implementer, with methods to turn on and off a device.

```{code-cell} ipython3
from abc import ABC, abstractmethod

# Implementer interface
class Device(ABC):
    @abstractmethod
    def turn_on(self):
        pass

    @abstractmethod
    def turn_off(self):
        pass
```

Now, we define concrete implementations of the `Device` interface: `TV` and `Radio`, each implementing the methods to control the respective device.

```{code-cell} ipython3
class TV(Device):
    def turn_on(self):
        print("Turning on the TV")

    def turn_off(self):
        print("Turning off the TV")

class Radio(Device):
    def turn_on(self):
        print("Turning on the radio")

    def turn_off(self):
        print("Turning off the radio")
```

Finally, we define a `RemoteControl` class as the abstraction that holds a reference to a `Device` instance. It delegates the actual work of controlling the device to the `Device` instance.

```{code-cell} ipython3
# Abstraction
class RemoteControl:
    def __init__(self, device):
        self.device = device

    def power_on(self):
        self.device.turn_on()

    def power_off(self):
        self.device.turn_off()
```

Now, let's see how we can use the Bridge Pattern in the example of using a remote control to control different devices.

We create `TV` and `Radio` objects, representing concrete implementations of the `Device` interface.

```{code-cell} ipython3
tv = TV()
radio = Radio()
```

The remote controls use the `power_on` and `power_off` methods to control the respective devices, demonstrating the decoupling of the abstraction (`RemoteControl`) from the implementation (`TV` and `Radio`).

```{code-cell} ipython3
# Create remote control for TV
tv_remote = RemoteControl(tv)
tv_remote.power_on()
tv_remote.power_off()
```

```{code-cell} ipython3
# Create remote control for radio
radio_remote = RemoteControl(radio)
radio_remote.power_on()
radio_remote.power_off()
```

## Benefits & Drawbacks

```{admonition} Benefits
- Decouples an abstraction from its implementation, allowing the two to vary independently.
- Promotes flexibility and scalability in the system.
- Simplifies the addition of new devices or abstractions.
```

```{admonition} Drawbacks
- Can add complexity to the code due to the additional layers of abstraction.
- Requires careful handling of abstraction and implementation hierarchies.
```
