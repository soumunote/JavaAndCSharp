# ボックス化
## Java
## C#
```C#
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        var list = new List<object>();
        list.Add(1);
        Console.WriteLine(list.Count);
    }
}
```
```IL
  .method private hidebysig static default void Main(string[] args) cil managed
  {
    // Method begins at Relative Virtual Address (RVA) 0x2050
    .entrypoint
    // Code size 28 (0x1C)
    .maxstack 8
    IL_0000: newobj instance void class [System.Collections]System.Collections.Generic.List`1<System.Object>::.ctor()
    IL_0005: dup
    IL_0006: ldc.i4.1
    IL_0007: box class System.Int32
    IL_000c: callvirt instance void class [System.Collections]System.Collections.Generic.List`1<System.Object>::Add(var)
    IL_0011: callvirt instance int32 class [System.Collections]System.Collections.Generic.List`1<System.Object>::get_Count()
    IL_0016: call void class [System.Console]System.Console::WriteLine(int32)
    IL_001b: ret
  } // End of method System.Void Program::Main(System.String[])
```
