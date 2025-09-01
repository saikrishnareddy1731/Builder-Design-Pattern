# Builder Design Pattern

## Overview
The **Builder Pattern** is a **creational design pattern** used to construct complex objects step-by-step. It separates the construction process from the representation of the object, allowing the same process to create different variations.

For example, a class may start with a few mandatory fields but over time, optional fields are added. Using multiple constructors for all combinations becomes complex and hard to maintain. This is called the **Telescoping Constructor Pattern** and is difficult to use correctly as the number of parameters grows.

Using setters is another approach, but it can lead to inconsistent objects if mandatory fields are missed.

The Builder Pattern solves these problems by taking responsibility for object creation. The client provides inputs and calls `build()` to get the final object. Until `build()` is called, the object is not created, ensuring consistency and correctness.

It is also useful when a class can have different representations. For example, a **Burger** can be Veg or Non-Veg, with optional toppings like Cheese, Egg, Mayonnaise, Onion, Lettuce, and sizes like Medium or Large. Instead of creating multiple constructors or using setters, a builder allows the client to specify exactly what they want:

> "I want a large veg burger with extra cheese"

---

## Why Use Builder Pattern?

- **Avoid Confusing Constructors**: No need to manage multiple constructors with different arguments.
- **Prevent Inconsistent Objects**: Mandatory fields are enforced before object creation.
- **Flexible Object Creation**: Supports multiple object representations (Veg/Non-Veg, optional toppings, etc.).
- **Maintainable and Readable Code**: The client code is clean and easy to understand.

---

## How It Works

1. **Product**: The object being built (e.g., Burger, Meal)
2. **Builder**: Class (or inner class) that collects required input and configurations
3. **Concrete Builder**: Implements specific steps to build a particular representation (e.g., Veg or Non-Veg Burger)
4. **Director (Optional)**: Orchestrates the building process step-by-step
5. **Build**: Client calls `build()` to get the final constructed object

---

## Example Scenario

- **Burger Builder**:
  - Can be Veg or Non-Veg
  - Optional toppings: Cheese, Egg, Mayonnaise, Onion, Lettuce
  - Bread size: Medium or Large

- **Meal Builder**:
  - Can be Veg or Non-Veg meals
  - Step-by-step addition of bread, curry, briyani, and drink

---

## Benefits

- Simplifies construction of complex objects
- Guarantees consistent and valid objects
- Improves code readability and maintainability
- Supports different object representations using the same construction process
- Enables step-by-step object creation

---

## Summary

The Builder Pattern is ideal for creating complex objects with multiple optional parameters or variations. It allows:

- Step-by-step construction of objects
- Consistent and valid object creation
- Clean, maintainable client code
- Different representations from the same construction process

- ## Visual Diagram

```text
Client
   |
   v
Builder Interface (defines steps)
   |
   v
Concrete Builder (implements steps for specific object)
   |
   v
Director (optional, controls order of steps)
   |
   v
Product (final object created)
---

> Widely used in Java, including Joshua Bloch’s Builder for immutable classes
