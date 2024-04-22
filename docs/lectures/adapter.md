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

# Adapter Pattern

## Introduction

The Adapter Pattern is a structural design pattern that allows incompatible interfaces to work together by providing a wrapper or adapter that translates the interface of one class to another. This pattern is useful when you want to use an existing class in a system with a different interface.

## Motivation

In software development, there are scenarios where a system expects a certain interface, but you want to use an existing class with a different interface. The Adapter Pattern addresses this by providing a way to adapt the existing class to the expected interface, enabling interoperability between the two.

## Implementation

Let's dive into the implementation of the Adapter Pattern. In this pattern, we define the target interface that the client expects, and an adapter class that implements the target interface and delegates calls to the existing class.

We define a `TargetInterface` class with a method `request`, representing the expected interface.

```{code-cell} ipython3
class TargetInterface:
    def request(self):
        pass
```

We define an `Adaptee` class with a method `specific_request`, which has a different interface from what the client expects.

```{code-cell} ipython3
class Adaptee:
    def specific_request(self):
        return "Response from Adaptee"
```

And, finall, we define an `Adapter` class that implements the `TargetInterface` and delegates calls to the `Adaptee` class, translating the interface.

```{code-cell} ipython3
class Adapter(TargetInterface):
    def __init__(self, adaptee):
        self.adaptee = adaptee

    def request(self):
        return self.adaptee.specific_request()
```

### Example

Now, let's see how we can use the Adapter Pattern in a real-life example of adapting a European electrical socket (Adaptee) to work with a US plug (TargetInterface).

We define a `EuropeanSocket` class representing a European electrical socket that provides European power.

```{code-cell} ipython3
class EuropeanSocket:
    def provide_european_power(self):
        return "European power"
```

We define a `USPlug` class representing the interface expected by a US plug.

```{code-cell} ipython3
class USPlug:
    def request_power(self):
        pass
```

And, now, we define a `SocketAdapter` class that implements the `USPlug` interface and adapts the `EuropeanSocket` to provide US power by converting the European power.

```{code-cell} ipython3
class SocketAdapter(USPlug):
    def __init__(self, european_socket):
        self.european_socket = european_socket

    def request_power(self):
        # Adapting European power to US power
        european_power = self.european_socket.provide_european_power()
        return f"Converted {european_power} to US power"
```

```{code-cell} ipython3
european_socket = EuropeanSocket()
socket_adapter = SocketAdapter(european_socket)

# Using the adapter to request power for a US plug
power = socket_adapter.request_power()
print(power)
```

## Benefits & Drawbacks

```{admonition} Benefits
- Allows incompatible interfaces to work together, enabling interoperability.
- Promotes code reuse by adapting existing classes to new interfaces.
- Provides a flexible way to integrate new classes into existing systems.
```

```{admonition} Drawbacks
- Can add complexity to the code, especially when there are multiple levels of adaptation.
- May introduce performance overhead due to the extra layer of abstraction.
```
