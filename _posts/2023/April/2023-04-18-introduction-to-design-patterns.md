---
title: Introduction to Design Patterns
tags: [Computer Science, Software Design]
description: Introduction to various design patterns that are commonly used in software designs and engineering.
comments: true
mathjax: false
style: fill
color: danger
---

## Design Patterns

Design Patterns are general, reusable solutions to common software problems that have been tested and proven over time. They provide solutions to common problems and help developers write more maintainable, flexible, and scalable code. There are three main types of Design Patterns: Creational, Structural, and Behavioral. In this blog, I'll try listing some of the important design patterns and their broader classification and in later blogs, we'll try to explain them in detail.

## Creational Design Patterns

Creational design patterns are a category of design patterns that deal with object creation mechanisms, trying to create objects in a manner suitable to the situation. These patterns provide various ways to create objects while hiding the creation logic, making the code more flexible and less coupled to specific classes or types.

There are five commonly recognized creational design patterns:

- **Singleton:** This pattern ensures that only one instance of a class is created, and provides a global point of access to it.

- **Factory:** This pattern provides an interface for creating objects, but allows subclasses to decide which class to instantiate.

- **Abstract Factory:** This pattern provides an interface for creating families of related or dependent objects, without specifying their concrete classes.

- **Builder:** This pattern separates the construction of a complex object from its representation, allowing the same construction process to create different representations.

- **Prototype:** This pattern creates new objects by cloning an existing object, thus avoiding the overhead of creating a new object from scratch.

We'll cover **Singleton** and **Factory** patterns in detail in upcoming posts as these are most commonly opted ones.

## Structural Design Patterns

Structural design patterns are a category of design patterns that deal with object composition, providing a way to form larger, more complex structures from simple objects and classes. These patterns focus on the relationship between objects and how they can be used to form larger structures, while keeping the system flexible and easy to modify.

There are seven commonly recognized structural design patterns:

- **Adapter:** This pattern allows objects with incompatible interfaces to work together by creating a wrapper that translates one interface into another.

- **Bridge:** This pattern decouples an abstraction from its implementation, allowing them to vary independently.

- **Composite:** This pattern allows you to compose objects into tree structures to represent part-whole hierarchies. It lets clients treat individual objects and compositions of objects uniformly.

- **Decorator:** This pattern attaches additional responsibilities to an object dynamically, providing a flexible alternative to subclassing for extending functionality.

- **Facade:** This pattern provides a simple interface to a complex system, hiding its complexity and making it easier to use.

- **Flyweight:** This pattern minimizes memory usage by sharing as much data as possible with similar objects.

- **Proxy:** This pattern provides a placeholder for an object to control access to it, providing additional functionality such as lazy initialization or access control.

We'll cover **Facade**, **MVVM(Model-View-ViewModel)** and **Decorator** patterns in detail in upcoming posts as these are most commonly opted ones.

## Behavioral Design Patterns

Behavioral patterns are a category of design patterns that focus on communication between objects and how they interact to achieve a particular behavior or functionality. These patterns deal with the responsibilities of objects and the way they communicate and collaborate with each other.

There are 11 commonly recognized behavioral design patterns:

- **Chain of Responsibility:** This pattern allows a chain of objects to handle a request and pass it along the chain until it is handled.

- **Command:** This pattern encapsulates a request as an object, allowing you to parameterize clients with different requests and queue or log requests.

- **Interpreter:** This pattern provides a way to evaluate language grammar or expressions.

- **Iterator:** This pattern provides a way to traverse a collection of objects without exposing the underlying implementation.

- **Mediator:** This pattern defines an object that encapsulates how a set of objects interact, promoting loose coupling between them.

- **Memento:** This pattern provides the ability to capture and restore an object's internal state.

- **Observer:** This pattern defines a one-to-many dependency between objects, so that when one object changes state, all its dependents are notified and updated automatically.

- **State:** This pattern allows an object to alter its behavior when its internal state changes, with the object appearing to change its class.

- **Strategy:** This pattern defines a family of algorithms, encapsulating each one and making them interchangeable.

- **Template Method:** This pattern defines the skeleton of an algorithm in a superclass, with subclasses providing specific implementations.

- **Visitor:** This pattern separates an algorithm from an object structure, allowing new operations to be added to an object structure without changing the objects themselves.

We'll cover **Observer** and **Pub-Sub** patterns in detail in upcoming posts as these are most commonly opted ones.

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}
