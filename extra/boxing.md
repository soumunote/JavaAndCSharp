# ボックス化
- Javaではプリミティブ型に対応するラッパークラスのvalueOf()メソッドが呼ばれる
- C#ではboxというILが存在する
## Java
```Java
import java.util.ArrayList;

public class Main {
  static void main(String... args) {
    var list = new ArrayList<Object>();
    list.add(1);
    System.out.println(list.size());
  }
}
```
```ByteCode
public class Main {
  public Main();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  static void main(java.lang.String...);
    Code:
       0: new           #7                  // class java/util/ArrayList
       3: dup
       4: invokespecial #9                  // Method java/util/ArrayList."<init>":()V
       7: astore_1
       8: aload_1
       9: iconst_1
      10: invokestatic  #10                 // Method java/lang/Integer.valueOf:(I)Ljava/lang/Integer;
      13: invokevirtual #16                 // Method java/util/ArrayList.add:(Ljava/lang/Object;)Z
      16: pop
      17: getstatic     #20                 // Field java/lang/System.out:Ljava/io/PrintStream;
      20: aload_1
      21: invokevirtual #26                 // Method java/util/ArrayList.size:()I
      24: invokevirtual #30                 // Method java/io/PrintStream.println:(I)V
      27: return
}
```
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
