# C# Reference Types Deep Dive

This repository provides a comprehensive guide to understanding reference types in C#. It covers fundamental concepts, advanced topics, and best practices for working with reference types efficiently.

## Table of Contents

1. [Introduction to Reference Types](01_Introduction.md)
2. [Classes](02_Classes.md)
3. [Interfaces](03_Interfaces.md)
4. [Delegates and Events](04_DelegatesAndEvents.md)
5. [Memory Management for Reference Types](05_MemoryManagement.md)
6. [Inheritance and Polymorphism](06_InheritanceAndPolymorphism.md)
7. [Reference Types vs Value Types](07_ReferenceVsValueTypes.md)
8. [Nullable Reference Types](08_NullableReferenceTypes.md)


## Overview

Reference types are fundamental to C# programming and understanding them is crucial for writing efficient and correct code. This guide covers:

- The nature and behavior of reference types
- Different categories of reference types (classes, interfaces, delegates)
- How reference types are managed in memory
- Object-oriented programming concepts related to reference types
- Performance implications of using reference types
- Advanced concepts like nullable reference types
- Best practices for working with reference types

Each topic is explained with code examples, memory diagrams, and practical tips.

# Introduction to Reference Types in C#

Reference types are one of the two main categories of types in C#, the other being value types. Understanding reference types is crucial for effective C# programming and memory management.

## What are Reference Types?

Reference types are types that store a reference (memory address) to their data, rather than containing the data directly. The actual data is stored on the managed heap.

## Key Characteristics of Reference Types

1. **Heap Allocation**: The data is stored on the managed heap.
2. **Reference on Stack**: A reference (pointer) to the heap data is stored on the stack.
3. **Nullable**: Can be assigned a null value.
4. **Inheritance**: Support inheritance (except for sealed classes).
5. **Pass by Reference**: When passed to methods, only the reference is copied, not the data.

## Categories of Reference Types

1. **Classes**: User-defined types that encapsulate data and behavior.
2. **Interfaces**: Contracts that define a set of methods and properties.
3. **Delegates**: Type-safe function pointers.
4. **Arrays**: Even if they contain value types, arrays themselves are reference types.
5. **Object**: The base type for all other types in C#.
6. **String**: Although immutable, string is a reference type.

## Example

```csharp
class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

Person person1 = new Person { Name = "Alice", Age = 30 };
Person person2 = person1;  // Both variables reference the same object

person2.Age = 31;  // This changes the age for both person1 and person2
```

## Memory Allocation

```
Stack              Heap
+---------------+   +------------------+
| person1       |   |    Person        |
| [Reference]---|-->| Name: "Alice"    |
+---------------+   | Age: 31          |
| person2       |   +------------------+
| [Reference]---|--^
+---------------+
```

## Key Points

1. Multiple variables can reference the same object.
2. Changes to the object are visible through all references.
3. Assignment of reference types copies the reference, not the object.
4. Reference types support polymorphism through inheritance.
5. Garbage collection automatically manages memory for unused objects.

## Next Steps

In the following sections, we'll explore each category of reference types in detail, starting with [Classes](02_Classes.md).


# Classes in C#

Classes are the fundamental building blocks of object-oriented programming in C#. They are reference types that encapsulate data and behavior into a single unit.

## Defining a Class

```csharp
public class Car
{
    // Fields
    private string _make;
    private string _model;

    // Properties
    public int Year { get; set; }

    // Constructor
    public Car(string make, string model, int year)
    {
        _make = make;
        _model = model;
        Year = year;
    }

    // Methods
    public string GetInfo()
    {
        return $"{_make} {_model} ({Year})";
    }
}
```

## Key Components of a Class

1. **Fields**: Variables that store the data.
2. **Properties**: Provide a flexible mechanism to read, write, or compute the values of private fields.
3. **Constructors**: Special methods for initializing objects.
4. **Methods**: Define the behavior of the class.
5. **Events**: Enable a class to notify other objects of occurrences.
6. **Nested Types**: Types declared within the scope of the class.

## Using Classes

```csharp
Car myCar = new Car("Toyota", "Corolla", 2022);
Console.WriteLine(myCar.GetInfo());  // Outputs: Toyota Corolla (2022)

myCar.Year = 2023;  // Modifying a property
Console.WriteLine(myCar.GetInfo());  // Outputs: Toyota Corolla (2023)
```

## Inheritance

Classes in C# support single inheritance:

```csharp
public class ElectricCar : Car
{
    public int BatteryCapacity { get; set; }

    public ElectricCar(string make, string model, int year, int batteryCapacity)
        : base(make, model, year)
    {
        BatteryCapacity = batteryCapacity;
    }
}
```

## Access Modifiers

- `public`: Accessible from anywhere.
- `private`: Accessible only within the same class.
- `protected`: Accessible within the same class and derived classes.
- `internal`: Accessible within the same assembly.
- `protected internal`: Combination of protected and internal.

## Static Members

Static members belong to the class itself, not to any specific instance:

```csharp
public class MathOperations
{
    public static int Add(int a, int b) => a + b;
}

int result = MathOperations.Add(5, 3);  // Calling a static method
```

## Memory Allocation

When you create an instance of a class:
1. Memory is allocated on the heap for the object.
2. The constructor is called to initialize the object.
3. A reference to the object is returned and typically stored in a variable.

## Best Practices

1. Follow the Single Responsibility Principle: A class should have only one reason to change.
2. Use properties instead of public fields for better encapsulation.
3. Initialize all fields in the constructor.
4. Override `ToString()` for meaningful string representations.
5. Implement `IDisposable` for classes that use unmanaged resources.

## Advanced Concepts

- Partial classes
- Abstract classes
- Sealed classes
- Extension methods

These will be covered in more detail in the [Advanced Topics](09_AdvancedTopics.md) section.

## Next Steps

Explore [Interfaces](03_Interfaces.md) to learn about defining contracts for classes and enabling multiple inheritance of behavior.

# Interfaces in C#

Interfaces in C# define a contract that classes can implement. They provide a way to achieve abstraction and enable multiple inheritance of behavior.

## Defining an Interface

```csharp
public interface IVehicle
{
    void Start();
    void Stop();
    bool IsRunning { get; }
}
```

## Key Characteristics of Interfaces

1. Can contain method signatures, properties, events, and indexers.
2. Cannot contain implementation code (prior to C# 8.0).
3. A class can implement multiple interfaces.
4. Interfaces can inherit from other interfaces.

## Implementing an Interface

```csharp
public class Car : IVehicle
{
    private bool _isRunning;

    public void Start()
    {
        _isRunning = true;
        Console.WriteLine("Car started");
    }

    public void Stop()
    {
        _isRunning = false;
        Console.WriteLine("Car stopped");
    }

    public bool IsRunning => _isRunning;
}
```

## Interface Inheritance

Interfaces can inherit from other interfaces:

```csharp
public interface IElectricVehicle : IVehicle
{
    int BatteryPercentage { get; }
    void Charge();
}
```

## Explicit Interface Implementation

When a class implements multiple interfaces with the same method name:

```csharp
public interface IA { void Method(); }
public interface IB { void Method(); }

public class MyClass : IA, IB
{
    void IA.Method() { Console.WriteLine("IA Method"); }
    void IB.Method() { Console.WriteLine("IB Method"); }
}

// Usage
MyClass obj = new MyClass();
((IA)obj).Method();  // Calls IA's Method
((IB)obj).Method();  // Calls IB's Method
```

## Default Interface Methods (C# 8.0+)

C# 8.0 introduced the ability to have default implementations in interfaces:

```csharp
public interface ILogger
{
    void LogError(string message);
    void LogWarning(string message) => Console.WriteLine($"Warning: {message}");
}
```

## Benefits of Interfaces

1. **Abstraction**: Define contracts without specifying implementation.
2. **Multiple Inheritance**: A class can implement multiple interfaces.
3. **Loose Coupling**: Code to interfaces, not implementations.
4. **Testability**: Easily mock interfaces for unit testing.

## Interface vs Abstract Class

- Use an interface when:
  - You want to define a contract without any implementation.
  - You need multiple inheritance.
  - You want to specify behavior for unrelated classes.

- Use an abstract class when:
  - You want to provide a base implementation for related classes.
  - You need to define non-public members.
  - You want to take advantage of constructor chaining.

## Best Practices

1. Name interfaces with an "I" prefix (e.g., `IDisposable`).
2. Keep interfaces focused and small (Interface Segregation Principle).
3. Use interfaces to define behavior, not data.
4. Consider using interfaces for dependency injection.

## Advanced Concepts

- Covariance and contravariance with interfaces
- Interface reimplementation in derived classes
- Generic interfaces

These will be covered in more detail in the [Advanced Topics](09_AdvancedTopics.md) section.

## Next Steps

Explore [Delegates and Events](04_DelegatesAndEvents.md) to learn about creating and using function pointers and event-driven programming in C#.


# Delegates and Events in C#

Delegates and events are powerful features in C# that enable flexible and loosely coupled designs, particularly useful for implementing callbacks and the observer pattern.

## Delegates

A delegate is a type that represents references to methods with a particular parameter list and return type.

### Defining a Delegate

```csharp
public delegate int MathOperation(int x, int y);
```

### Using a Delegate

```csharp
public class Calculator
{
    public static int Add(int x, int y) => x + y;
    public static int Subtract(int x, int y) => x - y;
}

MathOperation operation = Calculator.Add;
int result = operation(5, 3);  // result is 8

operation = Calculator.Subtract;
result = operation(5, 3);  // result is 2
```

### Multicast Delegates

Delegates can be combined to call multiple methods:

```csharp
public delegate void Logger(string message);

Logger log = Console.WriteLine;
log += (msg) => File.AppendAllText("log.txt", msg + Environment.NewLine);

log("This message is logged to console and file");
```

## Events

Events provide a way for a class to notify other objects when something of interest happens.

### Defining an Event

```csharp
public class Button
{
    public event EventHandler Click;

    public void OnClick()
    {
        Click?.Invoke(this, EventArgs.Empty);
    }
}
```

### Subscribing to an Event

```csharp
Button button = new Button();
button.Click += (sender, e) => Console.WriteLine("Button was clicked!");
button.OnClick();  // Triggers the event
```

## Func and Action Delegates

C# provides built-in generic delegates for common scenarios:

```csharp
Func<int, int, int> add = (x, y) => x + y;
int sum = add(3, 5);  // sum is 8

Action<string> print = Console.WriteLine;
print("Hello, World!");
```

## Event Patterns

### Standard Event Pattern

```csharp
public class DataProcessor
{
    public event EventHandler<DataProcessedEventArgs> DataProcessed;

    protected virtual void OnDataProcessed(DataProcessedEventArgs e)
    {
        DataProcessed?.Invoke(this, e);
    }

    public void ProcessData(string data)
    {
        // Process the data...
        OnDataProcessed(new DataProcessedEventArgs(data));
    }
}

public class DataProcessedEventArgs : EventArgs
{
    public string ProcessedData { get; }
    public DataProcessedEventArgs(string data) => ProcessedData = data;
}
```

### Custom Event Accessors

You can control how events are accessed:

```csharp
private EventHandler _myEvent;
public event EventHandler MyEvent
{
    add { _myEvent += value; }
    remove { _myEvent -= value; }
}
```

## Best Practices

1. Use the `EventHandler` and `EventHandler<TEventArgs>` delegates for most events.
2. Always check for null before invoking an event.
3. Consider using weak events to prevent memory leaks.
4. Use delegates for callback functionality.
5. Prefer events over public delegates to encapsulate invocation.

## Advanced Concepts

- Asynchronous delegates and events
- Custom delegate creation vs. using built-in delegates
- Event aggregator pattern

These will be covered in more detail in the [Advanced Topics](09_AdvancedTopics.md) section.

## Next Steps

Explore [Memory Management for Reference Types](05_MemoryManagement.md) to understand how C# manages memory for objects and handles garbage collection.


# Memory Management for Reference Types in C#

Understanding how C# manages memory for reference types is crucial for writing efficient and reliable applications. This section covers the basics of memory allocation, garbage collection, and best practices for managing resources.

## Stack vs Heap

- **Stack**: Stores value types and references to objects on the heap.
- **Heap**: Stores the actual objects (instances of reference types).

## Object Lifecycle

1. **Allocation**: When you create a new object, memory is allocated on the heap.
2. **Usage**: The object is used in your program.
3. **Deallocation**: When the object is no longer needed, it becomes eligible for garbage collection.

```csharp
public void ObjectLifecycle()
{
    MyClass obj = new MyClass();  // Allocation
    obj.DoSomething();            // Usage
}  // obj goes out of scope, eligible for garbage collection
```

## Garbage Collection

The .NET Garbage Collector (GC) automatically manages memory for your application.

### How GC Works

1. **Mark**: The GC identifies which objects are still in use.
2. **Sweep**: Unused objects are removed.
3. **Compact**: Remaining objects are compacted to reduce fragmentation.

### Generations

Objects are allocated in generations:
- **Generation 0**: New objects
- **Generation 1**: Objects that survive one garbage collection
- **Generation 2**: Long-lived objects

```csharp
GC.Collect();  // Force garbage collection (usually not recommended)
```

## Disposable Pattern

For objects that use unmanaged resources, implement `IDisposable`:

```csharp
public class ResourceHandler : IDisposable
{
    private bool _disposed = false;
    private IntPtr _handle;  // Unmanaged resource

    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    protected virtual void Dispose(bool disposing)
    {
        if (!_disposed)
        {
            if (disposing)
            {
                // Dispose managed resources
            }

            // Free unmanaged resources
            if (_handle != IntPtr.Zero)
            {
                FreeHandle(_handle);
                _handle = IntPtr.Zero;
            }

            _disposed = true;
        }
    }

    ~ResourceHandler()
    {
        Dispose(false);
    }
}
```

## Memory Leaks

Common causes of memory leaks in C#:
1. **Unsubscribed events**
2. **Static references to objects**
3. **Unclosed database connections or file handles**

### Preventing Memory Leaks

```csharp
public class Subscriber
{
    public void Subscribe(Publisher publisher)
    {
        publisher.SomeEvent += HandleEvent;
    }

    public void Unsubscribe(Publisher publisher)
    {
        publisher.SomeEvent -= HandleEvent;
    }

    private void HandleEvent(object sender, EventArgs e) { }
}
```

## Weak References

Weak references allow you to reference an object while still allowing that object to be collected by the GC:

```csharp
WeakReference<BigObject> weakRef = new WeakReference<BigObject>(new BigObject());

BigObject bigObject;
if (weakRef.TryGetTarget(out bigObject))
{
    // Use bigObject
}
```

## Best Practices

1. Implement `IDisposable` for classes that use unmanaged resources.
2. Use `using` statements or `using` declarations for `IDisposable` objects.
3. Avoid finalizers unless absolutely necessary.
4. Be cautious with large object allocations.
5. Unsubscribe from events when they're no longer needed.
6. Use memory profiling tools to identify leaks and inefficiencies.

## Advanced Concepts

- Large Object Heap (LOH)
- Garbage collection modes (workstation vs. server)
- Memory-mapped files
- Unsafe code and pointers

These will be covered in more detail in the [Advanced Topics](09_AdvancedTopics.md) section.

## Next Steps

Explore [Inheritance and Polymorphism](06_InheritanceAndPolymorphism.md) to understand how C# implements these core object-oriented programming concepts with reference types.


# Inheritance and Polymorphism in C#

Inheritance and polymorphism are fundamental concepts in object-oriented programming that C# supports through its reference types. These concepts allow for code reuse, extensibility, and flexible designs.

## Inheritance

Inheritance allows a class to inherit properties and methods from another class.

### Basic Inheritance

```csharp
public class Animal
{
    public string Name { get; set; }
    public virtual void MakeSound() { Console.WriteLine("The animal makes a sound"); }
}

public class Dog : Animal
{
    public override void MakeSound() { Console.WriteLine("The dog barks"); }
}
```

### Types of Inheritance

1. **Single Inheritance**: A class inherits from one base class.
2. **Multi-level Inheritance**: A class inherits from a class, which in turn inherits from another class.
3. **Interface Inheritance**: A class can implement multiple interfaces.

### Access Modifiers in Inheritance

- `public`: Accessible from anywhere.
- `protected`: Accessible within the class and its derived classes.
- `internal`: Accessible within the same assembly.
- `protected internal`: Accessible within the same assembly or derived classes.
- `private`: Not inherited.

### Sealed Classes

Prevent further inheritance:

```csharp
public sealed class FinalClass
{
    // This class cannot be inherited
}
```

## Polymorphism

Polymorphism allows objects of different types to be treated as objects of a common base type.

### Method Overriding

```csharp
public class Shape
{
    public virtual double GetArea() { return 0; }
}

public class Circle : Shape
{
    public double Radius { get; set; }
    public override double GetArea() { return Math.PI * Radius * Radius; }
}

public class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }
    public override double GetArea() { return Width * Height; }
}
```

### Using Polymorphism

```csharp
Shape[] shapes = new Shape[]
{
    new Circle { Radius = 5 },
    new Rectangle { Width = 4, Height = 5 }
};

foreach (var shape in shapes)
{
    Console.WriteLine($"Area: {shape.GetArea()}");
}
```

## Abstract Classes and Methods

Abstract classes provide a base for other classes but cannot be instantiated themselves.

```csharp
public abstract class Animal
{
    public abstract void MakeSound();
}

public class Dog : Animal
{
    public override void MakeSound() { Console.WriteLine("Woof!"); }
}
```

## Interfaces and Polymorphism

Interfaces allow for implementation of multiple inheritance and polymorphism.

```csharp
public interface IDrawable
{
    void Draw();
}

public class Circle : IDrawable
{
    public void Draw() { Console.WriteLine("Drawing a circle"); }
}

public class Square : IDrawable
{
    public void Draw() { Console.WriteLine("Drawing a square"); }
}

// Usage
IDrawable[] shapes = new IDrawable[] { new Circle(), new Square() };
foreach (var shape in shapes)
{
    shape.Draw();
}
```

## Method Hiding

Use the `new` keyword to hide a base class method:

```csharp
public class BaseClass
{
    public void Method() { Console.WriteLine("BaseClass Method"); }
}

public class DerivedClass : BaseClass
{
    public new void Method() { Console.WriteLine("DerivedClass Method"); }
}
```

## Best Practices

1. Use inheritance to model "is-a" relationships.
2. Prefer composition over inheritance for "has-a" relationships.
3. Use interfaces for defining contracts that can be implemented by unrelated classes.
4. Keep inheritance hierarchies shallow (typically no more than 3 levels deep).
5. Use the `override` keyword when intending to extend base class behavior.
6. Use `abstract` classes to define common behavior for related classes.

## Advanced Concepts

- Covariance and contravariance
- Explicit interface implementation
- Generic constraints in inheritance

These will be covered in more detail in the [Advanced Topics](09_AdvancedTopics.md) section.

## Next Steps

Explore [Reference Types vs Value Types](07_ReferenceVsValueTypes.md) to understand the key differences and when to use each in your C# programs.



# Reference Types vs Value Types in C#

Understanding the differences between reference types and value types is crucial for effective C# programming. These two categories of types have different behaviors in terms of memory allocation, assignment, and passing as method parameters.

## Key Differences

| Aspect | Reference Types | Value Types |
|--------|-----------------|-------------|
| Memory allocation | Heap | Stack (usually) |
| Default value | null | Zero/null equivalent |
| Assignment behavior | Copies the reference | Creates a copy of the data |
| Passing to methods | By reference | By value (copy) |
| Garbage collection | Subject to GC | Not subject to GC |
| Inheritance | Can inherit | Cannot inherit (except from object) |
| Examples | class, interface, delegate, array | struct, enum, primitive types (int, float, etc.) |

## Memory Allocation

### Reference Types
```csharp
class Person { public string Name; }
Person p = new Person();
```
```
Stack              Heap
+-------+          +-------------+
|   p   |--------->|   Person    |
|  ref  |          | Name: null  |
+-------+          +-------------+
```

### Value Types
```csharp
struct Point { public int X, Y; }
Point p = new Point { X = 10, Y = 20 };
```
```
Stack
+-------+
|   p   |
| X: 10 |
| Y: 20 |
+-------+
```

## Assignment Behavior

### Reference Types
```csharp
Person p1 = new Person { Name = "Alice" };
Person p2 = p1;  // Both p1 and p2 refer to the same object
p2.Name = "Bob"; // Changes are visible through both p1 and p2
```

### Value Types
```csharp
Point p1 = new Point { X = 10, Y = 20 };
Point p2 = p1;  // Creates a copy of p1
p2.X = 30;      // Doesn't affect p1
```

## Method Parameters

### Reference Types
```csharp
void ChangeName(Person p)
{
    p.Name = "Charlie";  // Affects the original object
}

Person person = new Person { Name = "Alice" };
ChangeName(person);  // person.Name is now "Charlie"
```

### Value Types
```csharp
void Increment(int x)
{
    x++;  // Only affects the local copy
}

int number = 5;
Increment(number);  // number is still 5
```

## Performance Considerations

- Value types are generally more efficient for small data structures.
- Reference types can be more efficient for large data structures or when sharing is needed.
- Frequent boxing/unboxing of value types can impact performance.

## When to Use Each

### Use Reference Types When:
- You need to support inheritance
- The data structure is large
- You need reference semantics (equality based on identity)
- You need to share instances across multiple parts of your code

### Use Value Types When:
- The data structure is small (generally < 16 bytes)
- The type has value semantics (equality based on value, not identity)
- Instances are short-lived or frequently created/destroyed

## Nullable Types

- Value types can be made nullable using `?` (e.g., `int?`)
- Reference types are inherently nullable, but can be made non-nullable in C# 8.0+ using nullable reference types feature

## Best Practices

1. Choose between value and reference types based on the semantics of your data, not just size.
2. Use structs for small, immutable value types.
3. Use classes for larger data structures or when inheritance is needed.
4. Be aware of performance implications, especially with large structs.
5. Use `ref` or `in` parameters for large value types to avoid copying.
6. Consider immutability for both value and reference types for safer code.

## Advanced Concepts

- Boxing and unboxing
- Ref locals and ref returns
- Span<T> and memory management optimizations

These will be covered in more detail in the [Advanced Topics](09_AdvancedTopics.md) section.

## Next Steps

Explore [Nullable Reference Types](08_NullableReferenceTypes.md) to learn about C# 8.0's feature for dealing with potential null values in reference types.

# Nullable Reference Types in C#

Introduced in C# 8.0, Nullable Reference Types is a feature that helps developers write more robust code by explicitly expressing intent about the nullability of reference types.

## The Problem Nullable Reference Types Solve

Before C# 8.0, all reference types were implicitly nullable, leading to potential null reference exceptions and unclear intent in the code.

## Enabling Nullable Reference Types

You can enable this feature in two ways:

1. Project-wide in the .csproj file:
   ```xml
   <PropertyGroup>
     <Nullable>enable</Nullable>
   </PropertyGroup>
   ```

2. File-specific using a pragma:
   ```csharp
   #nullable enable
   ```

## Nullability Annotations

- `string` - Non-nullable reference type
- `string?` - Nullable reference type

## Examples

```csharp
#nullable enable

string nonNullableString = "Hello";  // OK
string nullableString = null;        // Warning
string? explicitNullable = null;     // OK

void ProcessString(string s)  // 's' is non-nullable
{
    Console.WriteLine(s.Length);  // No warning
}

void ProcessNullableString(string? s)  // 's' is nullable
{
    if (s != null)
    {
        Console.WriteLine(s.Length);  // OK
    }
}
```

## Null-Forgiving Operator

Use the `!` operator to suppress warnings about possible null values:

```csharp
string? possiblyNull = GetString();
string definitelyNotNull = possiblyNull!;  // Suppresses the warning
```

## Null-Conditional Operator

The `?.` operator provides a concise way to access members of a potentially null object:

```csharp
string? name = person?.Name;
```

## Null-Coalescing Operator

The `??` operator provides a default value when an expression is null:

```csharp
string displayName = person?.Name ?? "Unknown";
```

## Attributes for Null Analysis

- `[NotNull]`: Indicates that a parameter, return value, or field should never be null.
- `[MaybeNull]`: Indicates that a parameter, return value, or field might be null.

```csharp
public class Example
{
    [return: NotNull]
    public string GetValue([MaybeNull] string input)
    {
        return input ?? "Default";
    }
}
```

## Best Practices

1. Enable nullable reference types for new projects.
2. Use nullable reference types to express intent clearly.
3. Avoid using the null-forgiving operator (`!`) unless absolutely necessary.
4. Use null-conditional (`?.`) and null-coalescing (`??`) operators to handle potential null values.
5. Be consistent with nullability annotations across your codebase.

## Challenges and Considerations

- Interoperability with older code or libraries not using nullable reference types.
- Potential for increased verbosity in some scenarios.
- Learning curve for developers used to implicit nullability.

