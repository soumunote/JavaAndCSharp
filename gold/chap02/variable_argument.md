# 可変長引数
JavaとC#比較のために、汎用的な Sum<T> を作成してみた。  
内部的に operator + を使用するのであるが、Java, C# の双方とも　四則演算を保証するインタフェースを持っていない。  
どちらの言語も以下のような記述で積んでしまう。  
- `T Sum<T extends 四則演算able` ... 四則演算able みたいなのが無い  
- `T Sum<T> where T : I四則演算able {}` ... I四則演算able みたいなのが無い  
よって、どちらの言語も、関数定義内で引数に対して +-*/ を記述できない  
- C#に関しては、DLRを使用して逃げれた。

## Java

## C#
以下の実行例からの推測で以下の法則性を見出した  
- 基本的に型の合致を最優先させる
```C#
using System;
using System.Linq;

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine(Sum(1L, 2L)); // deSum<long>()
        Console.WriteLine(Sum(1M, 2M, 3M)); // 3引数なので、Sum(params decimal[])
        Console.WriteLine(Sum("foo", "bar")); // Sum<string>()
        Console.WriteLine(Sum(new object(), new object())); Sum<object>()
    }

    static double Sum(double a, double b)
    {
        Console.WriteLine("Sum(double a, double b) called.");
        return a + b;
    }

    static T Sum<T> (T a, T b)
    {
        Console.WriteLine("Sum(T a, T b) called.");
        dynamic da = a;
        dynamic db = b;
        return (T)(da + db);
    }


    static decimal Sum(params decimal[] n)
    {
        Console.WriteLine("Sum(params decimal[] n) called.");
        return n.Sum();
    }
}
```
実行結果
```
Sum(T a, T b) called.
3
Sum(params decimal[] n) called.
6
Sum(T a, T b) called.
foobar
Sum(T a, T b) called.
Unhandled exception. Microsoft.CSharp.RuntimeBinder.RuntimeBinderException: Operator '+' cannot be applied to operands of type 'object' and 'object'
   at CallSite.Target(Closure , CallSite , Object , Object )
   at System.Dynamic.UpdateDelegates.UpdateAndExecute2[T0,T1,TRet](CallSite site, T0 arg0, T1 arg1)
   at Program.Sum[T](T a, T b) in C:\Users\maeda\Source\Repos\ConsoleApp8\ConsoleApp8\Program.cs:line 25
   at Program.Main(String[] args) in C:\Users\maeda\Source\Repos\ConsoleApp8\ConsoleApp8\Program.cs:line 11
```
