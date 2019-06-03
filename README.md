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

### Access Modifiers
|  | **Public**| **Protected** | **Private**  | **Default**  |
|:---:|:---:|:---:|:---:|:---:|
| Same Class | Y | Y |  |  |
| Same Package Subclass | Y | Y | Y | Y |
| Same Package Non-subclass | Y | Y | Y | N |
| Different Package Subclass | Y | Y | N | N |
| Different Package Non-subclass | Y | N | N | N |

- Classes & interfaces cannot be `private` or `protected`
- Methods declared `public` in superclass must be `public` in all subclasses
- Methods declared `protected` in superclass must be `public` or `protected` in subclasses
- Private methods are not inherited

