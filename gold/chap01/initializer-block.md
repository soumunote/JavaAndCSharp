# Initializer block
## Java
```java
import java.util.*;

class SuperClass {
    static {
        System.out.println("SuperClass::Static Initializer");
    }
    {
        System.out.println("SuperClass::Initializer");
    }
    SuperClass() 
    {
        System.out.println("SuperClass::Constructor");
    }
}

class DerivedClass extends SuperClass {
    static {
        System.out.println("DerivedClass::Static Initializer");
    }
    {
        System.out.println("DerivedClass::Initializer");
    }
    DerivedClass()
    {
        System.out.println("DerivedClass::Constructor");
    }
}

public class Main {
    public static void main(String[] args) throws Exception {
        var foo = new DerivedClass();
    }
}
```
実行結果
```
SuperClass::Static Initializer
DerivedClass::Static Initializer
SuperClass::Initializer
SuperClass::Constructor
DerivedClass::Initializer
DerivedClass::Constructor
```
## C#
