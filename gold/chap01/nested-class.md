# Nested Class

## Java

## C#
- `static class`と指定した場合、`static`メンバのみを作成すできるクラスとなる。  
  Java と意味合いは異なる。  
- 逆に、Java での static inner class のみしか定義できず、外側のクラスのインスタンスにアクセスするためには、
  以下のように、外側のクラスの参照を保持する必要がある。  
- Java のローカルクラスに匹敵する機能は無い
```C#
using System;

class Container
{
    private string name;
    public Container(string name)
    {
        this.name = name;
    }

    public class Nested
    {
        Container container;
        public string ParentName => container.name;
        public Nested(Container container)
        {
            this.container = container;
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        var container = new Container("Parent");
        var nested = new Container.Nested(container);
        Console.WriteLine(nested.ParentName);
    }
}

```
