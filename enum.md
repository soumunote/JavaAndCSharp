# Enum
enumに関しては、内部的な実装がかなり異なる
|  |java|C#|
+--+----+--+
| enumキーワードで定義した型は、| `Enum<E extends Enum<E>>の派生クラスになる` | Enumクラスの派生クラスになる |
| 列挙される各値は | 列挙型のインスタンスである(プリミティブ型ではない) | 整数値型の値である。列挙型自体も値型 |
| ベースクラスの実装は | [かなりシンプル](https://github.com/AdoptOpenJDK/openjdk-jdk11/blob/master/src/java.base/share/classes/java/lang/Enum.java) 
                   | [かなり複雑](https://github.com/AdoptOpenJDK/openjdk-jdk11/blob/master/src/java.base/share/classes/java/lang/Enum.java) |
| 列挙型自体に機能を追加したい場合 | メソッドを定義できる | 拡張メソッドを定義する |

## Java
- [クラス名#valueOf("値の文字列表現")](https://github.com/openjdk/jdk/blob/9a19eb6918e1f766ccf1b1671ea1161a76fee571/src/java.base/share/classes/java/lang/Enum.java#L265) から値インスタンスを取得できる
- [変数名#toString()](https://github.com/openjdk/jdk/blob/9a19eb6918e1f766ccf1b1671ea1161a76fee571/src/java.base/share/classes/java/lang/Enum.java#L150) から値の文字列表現を得ることができる
- クラス名#values() にて値の配列を得ることができる
- クラス名#ordinal() にて定義順を得ることができると同時に、 [クラス名#compareT()](https://github.com/openjdk/jdk/blob/9a19eb6918e1f766ccf1b1671ea1161a76fee571/src/java.base/share/classes/java/lang/Enum.java#L195) による比較も可能  
enum から内部的に classへコンパイルされる際に、定数名に応じた定数名も生成される。  
1. staticイニシャライザブロックで各定数のインスタンスが生成される
2. 各定数の初期化時に、Enum<E extends Enum<E>>のコンストラクタに name(定数の文字列表現), ordinal が渡される  
   (name, ordinal はコンパイラが自動生成)
### decompluie してみる
enumは最終的にclassとしてコンパイルされるが、詳細を確認するため、classファイルをでコンパイルしてみる  
以下のツールを使用  
- [JDProject](http://java-decompiler.github.io/)Java Decompiler  
  enum を enum としてDecomplieしてくれるので、高性能なのだろうが使用できない
- [JAD Java Decompiler](https://varaneckas.com/jad/)  
  *こちらを使用* 
### 簡単な定義
<details>
<summary>Source</summary>

```Java:Week.java
public enum Week {
  SUNDAY,
  MONDAY,
  TUESDAY,
  WEDNESDAY,
  THURSDAY,
  FRIDAY,
  SATURDAY, // カンマで終われるのが良いね。
}
```

</details>
<details>
<summary>Decompile</summary>

```Java:Week.java
public final class Week extends Enum
{

    public static Week[] values()
    {
        return (Week[])$VALUES.clone();
    }

    public static Week valueOf(String s)
    {
        return (Week)Enum.valueOf(Week, s);
    }

    private Week(String s, int i)
    {
        super(s, i);
    }

    public static final Week SUNDAY;
    public static final Week MONDAY;
    public static final Week TUESDAY;
    public static final Week WEDNESDAY;
    public static final Week THURSDAY;
    public static final Week FRIDAY;
    public static final Week SATURDAY;
    private static final Week $VALUES[];

    static 
    {
        SUNDAY = new Week("SUNDAY", 0);
        MONDAY = new Week("MONDAY", 1);
        TUESDAY = new Week("TUESDAY", 2);
        WEDNESDAY = new Week("WEDNESDAY", 3);
        THURSDAY = new Week("THURSDAY", 4);
        FRIDAY = new Week("FRIDAY", 5);
        SATURDAY = new Week("SATURDAY", 6);
        $VALUES = (new Week[] {
            SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
        });
    }
}
```

</details>

### コンストラクタを再定義し追加情報を付与
<details>
<summary>Source</summary>

```Java:JpWeek.java
public enum JpWeek {
  SUNDAY("日"),
  MONDAY("月"),
  TUESDAY("火"),
  WEDNESDAY("水"),
  THURSDAY("木"),
  FRIDAY("金"),
  SATURDAY("土"),
  ;

  private String jpName;
  private JpWeek(String jpName) { this.jpName = jpName; }
  public String getJpName() { return this.jpName; }
}
```

</details>
<details>
<summary>Decompile</summary>
  
```Java:JpWeek.java
public final class JpWeek extends Enum
{

    public static JpWeek[] values()
    {
        return (JpWeek[])$VALUES.clone();
    }

    public static JpWeek valueOf(String s)
    {
        return (JpWeek)Enum.valueOf(JpWeek, s);
    }

    private JpWeek(String s, int i, String s1)
    {
        super(s, i);
        jpName = s1;
    }

    public String getJpName()
    {
        return jpName;
    }

    public static final JpWeek SUNDAY;
    public static final JpWeek MONDAY;
    public static final JpWeek TUESDAY;
    public static final JpWeek WEDNESDAY;
    public static final JpWeek THURSDAY;
    public static final JpWeek FRIDAY;
    public static final JpWeek SATURDAY;
    private String jpName;
    private static final JpWeek $VALUES[];

    static 
    {
        SUNDAY = new JpWeek("SUNDAY", 0, "\u65E5");
        MONDAY = new JpWeek("MONDAY", 1, "\u6708");
        TUESDAY = new JpWeek("TUESDAY", 2, "\u706B");
        WEDNESDAY = new JpWeek("WEDNESDAY", 3, "\u6C34");
        THURSDAY = new JpWeek("THURSDAY", 4, "\u6728");
        FRIDAY = new JpWeek("FRIDAY", 5, "\u91D1");
        SATURDAY = new JpWeek("SATURDAY", 6, "\u571F");
        $VALUES = (new JpWeek[] {
            SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
        });
    }
}
```

</details>

## C#
- [Enum.Parse<Type>("値の文字列表現")(.NET5)](https://github.com/dotnet/corert/blob/c6af4cfc8b625851b91823d9be746c4f7abdc667/src/System.Private.CoreLib/shared/System/Enum.cs#L274) から値インスタンスを取得できる
 
  [Enum.Parse(Type)](https://docs.microsoft.com/ja-jp/dotnet/api/system.enum.parse?view=net-5.0)
- [変数名.ToString()](https://github.com/dotnet/corert/blob/c6af4cfc8b625851b91823d9be746c4f7abdc667/src/System.Private.CoreLib/shared/System/Enum.cs#L896) から値の文字列表現を得ることができる  
- [Enum.GetValues(Type)](https://docs.microsoft.com/ja-jp/dotnet/api/system.enum.getvalues?view=net-5.0)列挙体に含まれている値の配列を返す  
  Java側の対応メソッドは、クラス名#values()よりも、クラス名#ordinal()が近い
- [Enum.GetNames(Type)](https://docs.microsoft.com/ja-jp/dotnet/api/system.enum.getnames?view=net-5.0) にて値の配列を得ることができる

### サンプル
#### 値<=>文字列の相互変換
<detail>
<summary>ソース</summary>

```C#
using System;

enum Week : ushort
{
    MONDAY = 0,
    TUESDAY = 1,
    WEDNESDAY = 2,
    THURSDAY = 3,
    FRIDAY = 4,
    SATURDAY = 5,
    SUNDAY = 6,
}

class Program
{
    static void Main(string[] args)
    {
        var w1 = (Week)4;
        Console.WriteLine(w1.ToString());
        var w2 = Enum.Parse(typeof(Week), "SATURDAY");
        Console.WriteLine(w2.ToString());
        var w3 = Enum.Parse(typeof(Week), "ONE DAY");
        Console.WriteLine(w3.ToString());
    }
}
```
</detail>
<details>
<summary>実行結果</summary>

```
FRIDAY
SATURDAY
Unhandled exception. System.ArgumentException: Requested value 'ONE DAY' was not found.
```
</details>

<details>
<summary>IL</summary>

```il
.class private auto ansi sealed Week extends [System.Runtime]System.Enum
{
  .field public specialname rtspecialname uint16 value__
  .field public static Week MONDAY
  .field public static Week TUESDAY
  .field public static Week WEDNESDAY
  .field public static Week THURSDAY
  .field public static Week FRIDAY
  .field public static Week SATURDAY
  .field public static Week SUNDAY
} // End of class Week

.class private auto ansi beforefieldinit Program extends [System.Runtime]System.Object
{
  .method private hidebysig static default void Main(string[] args) cil managed
  {
    // Method begins at Relative Virtual Address (RVA) 0x2050
    .entrypoint
    // Code size 81 (0x51)
    .maxstack 2
    .locals init(Week V_0)
    IL_0000: ldc.i4.4
    IL_0001: stloc.0
    IL_0002: ldloca.s class V_0
    IL_0004: constrained. class Week
    IL_000a: callvirt instance string class [System.Runtime]System.Object::ToString()
    IL_000f: call void class [System.Console]System.Console::WriteLine(string)
    IL_0014: ldtoken class Week
    IL_0019: call [System.Runtime]System.Type class [System.Runtime]System.Type::GetTypeFromHandle(valuetype [System.Runtime]System.RuntimeTypeHandle)
    IL_001e: ldstr "SATURDAY"
    IL_0023: call [System.Runtime]System.Object class [System.Runtime]System.Enum::Parse([System.Runtime]System.Type, string)
    IL_0028: callvirt instance string class [System.Runtime]System.Object::ToString()
    IL_002d: call void class [System.Console]System.Console::WriteLine(string)
    IL_0032: ldtoken class Week
    IL_0037: call [System.Runtime]System.Type class [System.Runtime]System.Type::GetTypeFromHandle(valuetype [System.Runtime]System.RuntimeTypeHandle)
    IL_003c: ldstr "ONE DAY"
    IL_0041: call [System.Runtime]System.Object class [System.Runtime]System.Enum::Parse([System.Runtime]System.Type, string)
    IL_0046: callvirt instance string class [System.Runtime]System.Object::ToString()
    IL_004b: call void class [System.Console]System.Console::WriteLine(string)
    IL_0050: ret
  } // End of method System.Void Program::Main(System.String[])
```
</details>

#### 値の一覧
<details>
<summary>ソース</summary>

```c#
using System;

enum Week : ushort
{
    MONDAY = 0,
    TUESDAY = 1,
    WEDNESDAY = 2,
    THURSDAY = 3,
    FRIDAY = 4,
    SATURDAY = 5,
    SUNDAY = 6,
}

class Program
{
    static void Main(string[] args)
    {
        foreach (ushort value in Enum.GetValues(typeof(Week)))
        {
            Console.WriteLine(value);
        }
        foreach (var value in Enum.GetValues(typeof(Week)))
        {
            Console.WriteLine(value);
        }
        foreach (var name in Enum.GetNames(typeof(Week)))
        {
            Console.WriteLine(name);
        }
    }
}
```
</details>

<detail>
<summary>実行結果</summary>
   
```
0
1
2
3
4
5
6
MONDAY
TUESDAY
WEDNESDAY
THURSDAY
FRIDAY
SATURDAY
SUNDAY
MONDAY
TUESDAY
WEDNESDAY
THURSDAY
FRIDAY
SATURDAY
SUNDAY
```
</detail>

<detail>
<summary>IL</summary>
   
```IL
.class private auto ansi sealed Week extends [System.Runtime]System.Enum
{

  .field public specialname rtspecialname uint16 value__

  .field public static Week MONDAY

  .field public static Week TUESDAY

  .field public static Week WEDNESDAY

  .field public static Week THURSDAY

  .field public static Week FRIDAY

  .field public static Week SATURDAY

  .field public static Week SUNDAY
} // End of class Week


.class private auto ansi beforefieldinit Program extends [System.Runtime]System.Object
{

  .method private hidebysig static default void Main(string[] args) cil managed
  {
    // Method begins at Relative Virtual Address (RVA) 0x2050
    .entrypoint
    // Code size 166 (0xA6)
    .maxstack 2
    .locals init(class [System.Runtime]System.Collections.IEnumerator V_0, class [System.Runtime]System.IDisposable V_1, string[] V_2, int32 V_3)
    IL_0000: ldtoken class Week
    IL_0005: call [System.Runtime]System.Type class [System.Runtime]System.Type::GetTypeFromHandle(valuetype [System.Runtime]System.RuntimeTypeHandle)
    IL_000a: call [System.Runtime]System.Array class [System.Runtime]System.Enum::GetValues([System.Runtime]System.Type)
    IL_000f: callvirt instance [System.Runtime]System.Collections.IEnumerator class [System.Runtime]System.Array::GetEnumerator()
    IL_0014: stloc.0
    IL_0015: br.s     IL_0027
    IL_0017: ldloc.0
    IL_0018: callvirt instance [System.Runtime]System.Object class [System.Runtime]System.Collections.IEnumerator::get_Current()
    IL_001d: unbox.any class System.UInt16
    IL_0022: call void class [System.Console]System.Console::WriteLine(int32)
    IL_0027: ldloc.0
    IL_0028: callvirt instance bool class [System.Runtime]System.Collections.IEnumerator::MoveNext()
    IL_002d: brtrue.s     IL_0017
    IL_002f: leave.s     IL_0042
    IL_0031: ldloc.0
    IL_0032: isinst class System.IDisposable
    IL_0037: stloc.1
    IL_0038: ldloc.1
    IL_0039: brfalse.s     IL_0041
    IL_003b: ldloc.1
    IL_003c: callvirt instance void class [System.Runtime]System.IDisposable::Dispose()
    IL_0041: endfinally
    IL_0042: ldtoken class Week
    IL_0047: call [System.Runtime]System.Type class [System.Runtime]System.Type::GetTypeFromHandle(valuetype [System.Runtime]System.RuntimeTypeHandle)
    IL_004c: call [System.Runtime]System.Array class [System.Runtime]System.Enum::GetValues([System.Runtime]System.Type)
    IL_0051: callvirt instance [System.Runtime]System.Collections.IEnumerator class [System.Runtime]System.Array::GetEnumerator()
    IL_0056: stloc.0
    IL_0057: br.s     IL_0064
    IL_0059: ldloc.0
    IL_005a: callvirt instance [System.Runtime]System.Object class [System.Runtime]System.Collections.IEnumerator::get_Current()
    IL_005f: call void class [System.Console]System.Console::WriteLine([System.Runtime]System.Object)
    IL_0064: ldloc.0
    IL_0065: callvirt instance bool class [System.Runtime]System.Collections.IEnumerator::MoveNext()
    IL_006a: brtrue.s     IL_0059
    IL_006c: leave.s     IL_007f
    IL_006e: ldloc.0
    IL_006f: isinst class System.IDisposable
    IL_0074: stloc.1
    IL_0075: ldloc.1
    IL_0076: brfalse.s     IL_007e
    IL_0078: ldloc.1
    IL_0079: callvirt instance void class [System.Runtime]System.IDisposable::Dispose()
    IL_007e: endfinally
    IL_007f: ldtoken class Week
    IL_0084: call [System.Runtime]System.Type class [System.Runtime]System.Type::GetTypeFromHandle(valuetype [System.Runtime]System.RuntimeTypeHandle)
    IL_0089: call string[] class [System.Runtime]System.Enum::GetNames([System.Runtime]System.Type)
    IL_008e: stloc.2
    IL_008f: ldc.i4.0
    IL_0090: stloc.3
    IL_0091: br.s     IL_009f
    IL_0093: ldloc.2
    IL_0094: ldloc.3
    IL_0095: ldelem.ref
    IL_0096: call void class [System.Console]System.Console::WriteLine(string)
    IL_009b: ldloc.3
    IL_009c: ldc.i4.1
    IL_009d: add
    IL_009e: stloc.3
    IL_009f: ldloc.3
    IL_00a0: ldloc.2
    IL_00a1: ldlen
    IL_00a2: conv.i4
    IL_00a3: blt.s     IL_0093
    IL_00a5: ret
  } // End of method System.Void Program::Main(System.String[])
```
</detail>

#### 拡張メソッド
<details>
<summary>ソース</summary>

```c#
using System;

enum Week : ushort
{
    MONDAY = 0,
    TUESDAY = 1,
    WEDNESDAY = 2,
    THURSDAY = 3,
    FRIDAY = 4,
    SATURDAY = 5,
    SUNDAY = 6,
}

static class Program
{
    public static string WeekType(this Week week)
    {
        return week switch {
            Week w when w == Week.SATURDAY || w == Week.SUNDAY => "weekend",
            _ => "weekday",
        };
    }
    
    static void Main(string[] args)
    {
        foreach (Week value in Enum.GetValues(typeof(Week)))
        {
            Console.WriteLine($"{value.ToString()} is {value.WeekType()}");
        }
    }
}
```
</details>

<details>
<summary></summary>

```
MONDAY is weekday
TUESDAY is weekday
WEDNESDAY is weekday
THURSDAY is weekday
FRIDAY is weekday
SATURDAY is weekend
SUNDAY is weekend
```
</details>
