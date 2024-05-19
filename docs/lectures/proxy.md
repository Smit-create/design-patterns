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

# Proxy Pattern

## Introduction

The Proxy Pattern is a structural design pattern that provides a surrogate or placeholder for another object to control access to it. This pattern is useful for implementing lazy initialization, access control, logging, and more.

## Motivation

In software development, there are scenarios where you need to control access to an object, delay its creation, or provide additional functionality before or after its operations. The Proxy Pattern addresses these needs by introducing an intermediary that manages access to the real object.

## Implementation

Let's dive into the implementation of the Proxy Pattern. In this pattern, we define an interface for the subject, a real subject that implements this interface, and a proxy that also implements the interface and controls access to the real subject.

We define a `Subject` interface with a `request` method.

```{code-cell} ipython3
from abc import ABC, abstractmethod

# Subject interface
class Subject(ABC):
    @abstractmethod
    def request(self):
        pass
```

We define a `RealSubject` class that implements the `Subject` interface and handles the request.

```{code-cell} ipython3
# Real Subject
class RealSubject(Subject):
    def request(self):
        print("RealSubject: Handling request")
```

We define a `Proxy` class that also implements the `Subject` interface and controls access to the `RealSubject` by performing additional operations such as access control and logging.

```{code-cell} ipython3
# Proxy
class Proxy(Subject):
    def __init__(self, real_subject):
        self._real_subject = real_subject

    def request(self):
        if self._check_access():
            self._real_subject.request()
            self._log_access()

    def _check_access(self):
        # Simulate access control check
        print("Proxy: Checking access prior to firing a real request")
        return True

    def _log_access(self):
        # Simulate logging the access
        print("Proxy: Logging the time of request")
```

Now, let's see how we can use the Proxy Pattern in a real-life example of a document access system where access to sensitive documents is controlled and logged.

```{code-cell} ipython3
# Client code
def client_code(subject: Subject):
    # The client code works with subjects via the Subject interface
    subject.request()

# Real Subject
real_subject = RealSubject()

# Proxy controlling access to the Real Subject
proxy = Proxy(real_subject)

print("Client: Executing the client code with a real subject:")
client_code(real_subject)

print("\nClient: Executing the same client code with a proxy:")
client_code(proxy)
```

In this example:
- We define a `client_code` function that works with `Subject` objects via the `Subject` interface.
- We create a `RealSubject` object representing the actual document handler.
- We create a `Proxy` object that controls access to the `RealSubject`.
- We execute the client code with both the `RealSubject` and the `Proxy` to demonstrate how the proxy controls access and adds additional behavior.


## Real-Life Example

Let's consider a real-life example of a video streaming service where videos are only loaded and played when requested. The proxy controls access to the video loading process.

We define a `Video` interface with a `play` method.

```{code-cell} ipython3
# Subject interface
class Video(ABC):
    @abstractmethod
    def play(self):
        pass
```

We define a `RealVideo` class that loads and plays a video file.

```{code-cell} ipython3
# Real Subject
class RealVideo(Video):
    def __init__(self, filename):
        self.filename = filename
        self._load_video()

    def _load_video(self):
        print(f"RealVideo: Loading video {self.filename}")

    def play(self):
        print(f"RealVideo: Playing video {self.filename}")
```

We define a `VideoProxy` class that controls access to the `RealVideo`, loading it only when the `play` method is called for the first time.

```{code-cell} ipython3
# Proxy
class VideoProxy(Video):
    def __init__(self, filename):
        self.filename = filename
        self._real_video = None

    def play(self):
        if self._real_video is None:
            self._real_video = RealVideo(self.filename)
        self._real_video.play()
```

The client code requests to play the video through the proxy, demonstrating lazy initialization and access control.

```{code-cell} ipython3
# Client code
def client_code(video: Video):
    video.play()

# Using the proxy
video_proxy = VideoProxy("example.mp4")

print("Client: Requesting to play video through proxy:")
client_code(video_proxy)
```

## Benefits & Drawbacks

```{admonition} Benefits
- Provides control over the access to an object.
- Can introduce additional behavior (e.g., logging, access control) without modifying the real subject.
- Supports lazy initialization, reducing resource usage until the object is needed.
```

```{admonition} Drawbacks
- Can add complexity to the code due to the additional layer of abstraction.
- May introduce performance overhead due to the intermediary.
```
