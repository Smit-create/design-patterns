# Strategy Pattern

## Problem

Let's consider that you are designing a Car application. Some of
the observations are
- There are thousands of cars having some common functionalities like
accelerator, breaks, etc.
- Each of the cars have some funtionalities which might look common but
are actually varying like fuel type (petrol, diesel, electricity,
hydrogen, etc.), number of seats (2, 3, 4, 5, ...).

We have a super class `Car` which is *inherited* by many classes.
Now, the following scenario arises. If we dump all these *varying* methods
in the super class and over-ride them for each new `CarType`, then
we hit the problem of maintainability and a problem of extensibility when
we want to add 1000 new CarTypes. We have to sit and over-ride all these
methods in their implementation.

```java
public abstract class Car {
    public Car() {}

    public void accelerate() {};
    public void applyBreak() {};
    public String fuelType() {};
    public int numberOfSeats() {};
}
```

Now, how can we make this design more maintainable and also control
the behaviour of a Car dynamically? Let's say a Car can have two `fuelType` and
you want to change the `fuelType` of the Car if the Car has traveled 100
kms for some reason.

The solution is to use a Strategy Pattern.

## Solution

The Strategy Pattern defines a family of algorithms, encapsulates each one,
and makes them interchangable. It lets the algorithms vary independently
from clients that use it.

### Principles

1. Identify the aspects if your application that vary and separate them
from what stays the same.
2. Program to an interface, not an implementation.
3. Favor composition over inheritance.

### Technique

The point is to exploit *polymorphism* by programming to a supertype so that
the actual runtime objects isn't locked into some code. Let's use
the principles and re-design step by step:

1. Identify the aspects if your application that vary and separate them
from what stays the same.
- To these we identify that `fuelType` and `numberOfSeats` are the varying
components.


2. Program to an interface:

```java
public interface FuelType {
    public String fuelType();
}

public interface SeatNumber {
    public int numberOfSeats();
}
```

Implementations:

```java
public class Diesel implements FuelType {
    public String fuelType() {
        return "Diesel";
    }
}

public class Petrol implements FuelType {
    public String fuelType() {
        return "Petrol";
    }
}

public class FourSeater implements SeatNumber {
    public int numberOfSeats() {
        return 4;
    }
}

public class TwoSeater implements SeatNumber {
    public int numberOfSeats() {
        return 2;
    }
}
```

3. Favor composition over inheritance:

```java
public abstract class Car {

    FuelType fuelType;
    SeatNumber seatNumber;
    public Car() {
        fuelType = new Petrol();
        seatNumber = new FourSeater();
    }

    public void accelerate() {};
    public void applyBreak() {};
    public String getFuelType() {
        return fuelType.fuelType();
    }
    public int numberOfSeats() {
        return seatNumber.numberOfSeats();
    }
    // Let's add some runtime polymorphism flavors
    public void setFuelType(FuelType ft) {
        fuelType = ft;
    }
    public void setSeatNumber(SeatNumber sn) {
        seatNumber = sn;
    }
}
```

This how when we put two classes together, we are using composition.
Instead of inheriting their behaviour, the Cars get their behavior by
being composed with the right behaviour object. With the methods
`setFuelType` and `setSeatNumber` we get the ability to change the
number of seats and fuel type at the runtime.
