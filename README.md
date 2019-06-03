# Java Notes

## Singleton Class
Controls object creation, limiting # of objects to only one. Since there is only one instance, instance fields will occur once per class, similar to static fields.  
Singletons often control access to resources like database connections or sockets.  
```java
public class Singleton {
    private static Singleton singleton = new Singleton();
    private Singleton () {} // private constructor prevents any other class from instantiating
    
    public static Singleton getInstance() { return singleton; }
    protected static void demoMethod() { System.out.println("demo"); }
}
```

Example usage:
```java
Singleton tmp = Singleton.getInstance();
tmp.demoMethod();
```
    
## Scope of Variables
| **Local**| **Instance** | **Class/Static**  |
| --- |---|---|
| Declared in methods, constructors, or blocks| Declared in a class, but outside a method/block | Same as instance, but with `static` keyword |
| Created when block is entered, destroyed upon block exit | Created when an object is created (`new`). When space is allocated for a block on the heap, a slot for each instance var is created. | Created when program starts, destroyed when program ends. |
| No access modifiers | Access modifiers OK. Visible to all methods & constructors in class. | Access modifiers OK. Visible to all methods & constructors in class. |
| No default values. | Have default values. | Have default values. |

## Access Modifiers and Visibility
|  | **Public**| **Protected** | **Default**  | **Private**  |
|:---:|:---:|:---:|:---:|:---:|
| Same Class                     | Y | Y | Y | Y |
| Same Package Subclass          | Y | Y | Y | N |
| Same Package Non-subclass      | Y | Y | Y | N |
| Different Package Subclass     | Y | Y | N | N |
| Different Package Non-subclass | Y | N | N | N |

- Classes & interfaces cannot be `private` or `protected`
- Methods declared `public` in superclass must be `public` in all subclasses
- Methods declared `protected` in superclass must be `public` or `protected` in subclasses
- Private methods are not inherited


## Keywords
### `final` Keyword
#### Variables
- Can be initialized only once
- A reference variable declared `final` can never be reassigned to refer to a different object. However, the data within the object can be changed (unless it is also `final`). In other words, the state of the object can be changed, but not the reference.

#### Methods
- Cannot be overriden by subclasses

#### Classes
- Cannot be subclassed. Thus, no features can be inherited.

### `abstract` Keyword
#### Methods
- Have no implementation. Implementation is provided by subclass.
- Can never be `final` or `strict`
- Any class that extends an `abstract` class must implement **all** of its `abstract` methods, unless the subclass is also `abstract`

#### Classes
- Can never be instantiated
- Cannot be both `abstract` and `final` (there is an obvious conflict of purpose between those two keywords)
- If a class contains `abstract` methods, the class **must** be declared as `abstract`
- An `abstract` class may have `abstract` as well as other methods
- An `abstract` class doesn't have to have `abstract` methods.

### `synchronized` Keyword
Indicates a method can only be accessed by one thread at a time

### `transient` Keyword
An instance variable marked as `transient` tells the JVM to skip that variable when serializing the object containing it

### `throws` Keyword
Used to postpone the handling of a checked (compile-time) exception.
E.g.
```java
import java.io.*;
public class className {
    public void deposit(double amount) throws RemoteException, InsufficientFundsException {
        // Method implementation ...
        throw new RemoteException();
    }
    ...
}
```

### `volatile` Keyword
Tells the JVM that a thread accessing the variable must merge its own private copy of the variable with the master copy in memory. `volatile` can only be used on instance variables.

## Bitwise Operations
### Width vs. Possible Values
The number of bits used (width) determines the numbers that can be encoded: 2^n total.
- Unsigned: 0 through 2^n - 1
- 2's Complement: -2^(n-1) through 2^(n-1) - 1

### Numerical primitives
- **byte:** 8 bits, e.g. from -128 to 127
- **short:** 16 bits
- **char:** *unsigned* 16 bits
- **int:** 32 bits
- **long:** 64 bits

### Operators

For the following examples, assume a = 60, b = 13, c= -2. Their binary 2's complement representations are below.  
a = 0011 1100  
b = 0000 1101  
c = 1111 1110  

| Operation | Function | Example |
|:---:|:---:|---|
| &   |AND                            | a&b = 0000 1100 (12) |
| \|  |OR                             | a\|b = 0011 1101 (61) |
| ^   |XOR                            | a^b = 0011 0001 (49) |
| ~   |Complement (bitwise inversion) | ~a = 1100 0011 (-61 in 2's complement) |
| <<  |Left shift                     | a << 2 = 1111 0000 (-16 in 2's complement) |
| >>  |Arithmetic shift right         | c >> 2 = 1111 1111 (-1) |
| >>> |Logical shift right (zero-fill)| c >>> 2 = 0111 1111 (127) |

### Useful Tricks
- **XOR**
  - x ^ 0 = x
  - x ^ 1 = ~x
  - x ^ x = 0
- **AND**
  - x & 0 = 0
  - x & 1 = x
  - x & x = x
- **OR**
  - x \| 0 = x
  - x \| 1 = 1
  - x \| x = x
- Swapping two values without a temporary variable:
  - a = a ^ b
  - b = a ^ b
  - a = a ^ b
  - E.g. a = 2, b = 3; a = 0010, b = 0011

## `Number` Wrapper Classes
```
                   Number
                     |
  ___________________|_________________
 |       |       |       |      |      |
Byte  Integer  Double  Short  Float  Long
```

Converting primitive data types into objects is called **boxing**.  
Similarly, the reverse operation is called **unboxing**.

## Java Collections Framework
![alt text](https://media.geeksforgeeks.org/wp-content/uploads/java-collection.jpg "Java Collections Framework")

## Exceptions
Three types:
1. **Checked Exceptions:** Notified by the compiler at compile-time
2. **Unchecked Exceptions:** Runtime Exceptions
3. **Errors:** Problems that arise beyond the control of the user and programmer, e.g. stack overflow

### Exception Hierarchy
![alt text](http://cdncontribute.geeksforgeeks.org/wp-content/uploads/Exception-in-java1.png "Java Exceptions Hierarchy")

![alt text](http://www.benchresources.net/wp-content/uploads/2017/02/exception-hierarchy-in-java.png "Java Exceptions Hierarchy")

### try-with-resources
Automatically closes the resources used, e.g.
```java
try (FileReader fr = new FileReader(filepath)) {
    // use the resource
} catch () {
    // handle the exception
}
```

### User-defined Exceptions
- Must be a child of `Throwable`
- If checked exception, must extend `Exception`
- If unchecked exception, must extend `RuntimeException`

## Nested Classes
Types of nested classes:
```
                                Nested classes
                                      |
                   ___________________|__________________
                  |                                      |
            Inner classes                      Static nested classes
    ______________|_________________
   |              |                 |
 Inner      Method-local        Anonymous
classes     inner classes     inner classes
```

**Java does NOT support multiplhe inheritance.**  
This means a class cannot inherit multiple classes.  
However, a class **can** implement multiple interfaces.

## Method Overriding
Rules for overriding methods (NOT overloading!)
- The argument list must be the same
- The return type must be the same or a subtype of the return type declared in the overriden method
- The access level cannot be more restrictive than the overidden method's
- Instance methods can only be overridden if they are inherited by the subclass
- `final` methods cannot be overriden
- A `static` method can be redeclared, but not overridden
- If a method cannot be inherited, it cannot be overriden
- Constructors cannot be overriden

## Polymorphism
- Any object that can pass an IS-A test is polymorphic
  - All objects are polymorphic to the `Object` class
- Objects can **only** be accessed through reference variables
  - A reference variable can only be of one type, and that type cannot be changed once declared. However, the reference variable can be reassigned to other objects (given that it is not `final`)
    - The type of a reference variable determines the methods it can invoke on an object
    - A reference variable can refer to any object of its declared type or any subtype
    - A reference variable can be declared as a class or interface type
    
Example:
Given the following:  
```java
public interface Vegetarian { ... }
public class Animal { ... }
public class Deer extends Animal implements Vegetarian { ... }
```
then a Deer IS-A Animal, Vegetarian, Deer, and Object

Thus the following are all legal:
```java
Deer d = new Deer();
Animal a = d;
Vegetarian v = d;
Object o = d;
```
All four of these references refer to the same Deer object on the heap.

## Interfaces
An interface may have abstract methods, default methods, static methods, constants, and nested types. Method bodies exist only for default and static methods.

An interface contains behaviors that a class implements. All methods of an interface must be defined in the implementing class, unless the class itself is abstract.

Interfaces are **similar** to classes in that:
- They can contain any number of methods
- Saved as InterfaceName.java

Interfaces are **different** from classes in that:
- Interfaces cannot be instantiated
- Interfaces don't have constructors
- All interface methods are *implicitly* abstract
- An interface cannot have instance fields; only `static final` fields (constants)
- An interface can extend multiple interfaces (classes can implement multiple interfaces - but they *cannot* extend multiple classes)

Interfaces and their methods are implicitly `abstract`. Their methods are implicitly `public`.

### Tagging Interfaces
The most common use of extending interfaces occurs when the parent interface doesn't have any methods.
Example:
The MouseListener interface in `java.awt.event` extends `java.util.EventListener`, which is defined as follows:  
```java
package java.util;
public interface EventListener {}
```
Thus, an interfact with no methods is a **tagging interface**.  
These have two basic design purposes:  
1. Creates a common parent
2. Adds a data type to a class. A class that implements a tagging interface becomes an interface type through polymorphism

## Java Generics
```java
public static <E> void printArray(E[] array) {
    for (E element : array)
        System.out.print(element + " ");
}
...
// Example usage:
Integer[] intArr = {1, 2, 3};
Double[] doubleArr = {1.1, 2.2, 3.3};

printArray(intArr);
printArray(doubleArr);
```

You can also bound the type of the Generic parameters, e.g.  
```java
public static <T extends Comparable<T>> T max(T x, T y) { ... }
```

Can be applied to Generic classes as well, e.g.  
```java
public class Box<T> {
    private T t;
    ....
 }
 ```

