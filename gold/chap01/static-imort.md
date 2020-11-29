# static import
機能的な差は無いようだ。  
記述方法のみ異なる。  

## Java
```Java
package soumunote.myspace;

import static soumunote.myspace.Utility.*;

class Utility {
    public static String getFoo() { return "Foo"; }
    public static class SubUtility {
        public static String getBar() { return "Bar"; }
    }
}

public class Main {
    public static void main(String... args) {
        System.out.println(getFoo());
        System.out.println(SubUtility.getBar());
    }
}
```
## C#
```C#
using System;
using static MySpace.Utility;

namespace MySpace {
    class Utility {
        public static string Foo => "Foo"; 
        public class SubUtility {
            public static string Bar => "Bar"; 
        }
    }
    
    public class Hello{
        public static void Main(){
            Console.WriteLine(Foo);
            Console.WriteLine(SubUtility.Bar);
        }
    }
}
```
