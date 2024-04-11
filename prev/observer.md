# Observer Pattern

## Problem

You are given a problem to manage the news publishing software that
deals with the following work:
- Anyone can subscribe to the news software and start seeing the news.
- Any subscriber can unsubscribe at anytime and stop seeing the news.

This is the problem of Publishers + Subscribers, and the solution
is to use a Observer Pattern.

```java
class NewsPublisher {
    List<NewsSubscriber> subscribers;

    public void freshNewsAvailable() {
        for (NewsSubscriber subscriber: subscribers) {
            subscriber.updateNewsData();
        }
    }
    // Other methods
}
```

## Solution

The Observer Pattern defines a one-to-many dependency between objects
so that when one object changes state, all of its dependents are
notified and updated automatically.

### Principles

1. Strive for loosely coupled designs between objects that interact.
2. Program to an interface, not an implementation.
3. Favor composition over inheritance.

### Technique

Let's recall the design principle from the startegy pattern to
program to an interface. We define the interfaces of the Observer and the
Observable. In our example above, Observer is `NewsSubscriber` and
Observable is `NewsPublisher`. Also, using the first principle here

```java
public interface Subject {
    public void registerObserver(Observer obs);
    public void removeObserver(Observer obs);
    public void notifyObserver();
}

public interface Observer {
    public void update();
}
```

Rewrite our `NewsPublisher` and `NewsSubscriber` to use this interfaces.

```java
class NewsPublisher implements Subject {
    List<NewsSubscriber> subscribers;

    public void registerObserver(Observer obs) {
        subscribers.add(obs);
    }

    public void removeObserver(Observer obs) {
        subscribers.remove(obs);
    }

    public void freshNewsAvailable() {
        for (Observer subscriber: subscribers) {
            subscriber.update();
        }
    }
    // Other methods
}

class NewsSubscriber implements Observer {
    List<NewsSubscriber> subscribers;

    public NewsSubscriber(Subject subject) {
        subject.registerObserver(this);
    }

    public void update() {
        System.out.println("Fresh news here!");
    }
    // Other methods
}
```

This way we can easily manage given problem statement along with
providing easy flexibility of registering and removing a Subscriber.

Notice here, that the this is kind of a *push* model from Publisher end.
In some cases when you want the *pull* model from Subscriber end that
provides flexibility to the subscriber to pull the updates from
publisher only when it requires. The above design will also work for the
same if you just add the method `getCurrentState` in `Publisher` and
use it from `Subscriber` to get the latest state of Publisher.

### JAVA's built-in Observer Pattern

With Java's built-in support, all you have to do is extend Observable
and tell it when to notify the Observers. The API does the rest for you.

```java
import java.util.Observable;
import java.util.Observer;

class NewsPublisher extends Observable {

    public void freshNewsAvailable() {
        // The following two are implemented in Observable
        setChanged();
        notifyObservers(someData);
    }
    // Other methods
}

class NewsSubscriber extends Observer {
    List<NewsSubscriber> subscribers;

    public void update(Observable obs, Object arg) {
        if (obs instanceof NewsPublisher) {
             System.out.println("Fresh news here! " + arg);
        }
    }
    // Other methods
}
```

Internally, the following pseudo-code does the work for you:
```java
setChanged() {
    changed = true;
}

notifyObservers(Object someData) {
    if (changed) {
        for every observer: call update(this, arg);
        chaged = false;
    }
}

notifyObservers() {
    notifyObservers(null);
}
```

#### Some issues with JAVA's built-in Observable

Observable is a class and not an interface. That means you can't add
on the Observable behaviour to an existing class that already extends
another subclass.
