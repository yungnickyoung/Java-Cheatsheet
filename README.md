# Java Notes

### Singleton Class
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
    
### Scope of Variables
| **Local**| **Instance** | **Class/Static**  |
| --- |---|---|
| Declared in methods, constructors, or blocks| Declared in a class, but outside a method/block | Same as instance, but with `static` keyword |
| Created when block is entered, destroyed upon block exit | Created when an object is created (`new`). When space is allocated for a block on the heap, a slot for each instance var is created. | Created when program starts, destroyed when program ends. |
| No access modifiers | Access modifiers OK. Visible to all methods & constructors in class. | Access modifiers OK. Visible to all methods & constructors in class. |
| No default values. | Have default values. | Have default values. |

### Access Modifiers and Visibility
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

### `volatile` Keyword
Tells the JVM that a thread accessing the variable must merge its own private copy of the variable with the master copy in memory. `volatile` can only be used on instance variables.

### Bitwise Operations
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
