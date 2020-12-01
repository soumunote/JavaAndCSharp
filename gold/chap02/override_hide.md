# オーバーライドと隠蔽
<table>
    <tr>
        <th></th>
        <th>Java</th>
        <th>c#</th>
    </tr>
    <tr>
        <td>仮想メソッド</td>
        <td>全て</td>
        <td>virtualキーワードを指定した場合のみ</td>
    </tr>
    <tr>
        <td>
            メンバメソッドの隠蔽
        </td>
        <td>不可(必ずオーバーライドされる)</td>
        <td>newモディファイアを指定(しなくてもwarningのみ)</td>
    </tr>
    <tr>
        <td>メンバ変数の隠蔽</td>
        <td>可</td>
        <td>newモディファイアを指定(しなくてもwarningのみ)</td>
    </tr>
</table>

## Java
```Java
import java.util.*;

class Super {
    protected int i;
    protected int j;
    protected Super(int i, int j) {
        this.i = i;
        this.j = j;
    }
    public int getSuperI() { return i; }
    public int getJ() { return j; }
}

class Derived extends Super {
    private int i = 11;
    private int j = 22;
    public Derived(int i, int j) {
        super(i, j);
    }
    public int getI() { return i; }
    public int getJ() { return j; }
}


public class Main {
    public static void main(String[] args) throws Exception {
        var c = new Derived(1, 2);
        System.out.println(c.getI());
        System.out.println(c.getSuperI());
        System.out.println(c.getJ());
        Super s = new Derived(1, 2);
        System.out.println(s.getJ());
    }
}
```
実行結果
```
11  <- Derived#getI()
1   <- Super#getSuperI()
22  <- Derived#getJ()
22  <- Derived#getJ() ... 変数の型でなく値の型で呼ばれる
```

## C#
```c#
using System;

class Super {
    protected int i;
    protected int j;
    protected Super(int i, int j) {
        this.i = i;
        this.j = j;
    }
    public int SuperI() => i;
    public int J() => j;
}

class Derived : Super {
    private int i = 11;
    private int j = 22;
    public Derived(int i, int j) : base(i, j){
    }
    public int I() => i;
    public int J() => j;
}

public class Hello{
    public static void Main(){
        var c = new Derived(1, 2);
        Console.WriteLine(c.I());
        Console.WriteLine(c.SuperI());
        Console.WriteLine(c.J());
        Super s = new Derived(1, 2);
        Console.WriteLine(s.J());
    }
}
```
実行結果  
```
11  <- Derived.I()
1   <- Super.I()
22  <- Derived.J()
2   <- Super.J() ... 値の型でなく変数の型で呼ばれる
```
