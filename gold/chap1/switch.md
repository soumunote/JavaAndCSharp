# Switch
## Java
- 式に許される型  
  char, byte, short, int, Character, Byte, Short, Integer, String, *enum type*
- 全ての case constant は switch statement の式と互換のある方である必要がある

## C#
switch式及び、switch文のサンプルをmsdnより引用  
[switch式](https://docs.microsoft.com/ja-jp/dotnet/csharp/language-reference/operators/switch-expression)  
[switch文](https://docs.microsoft.com/ja-jp/dotnet/csharp/language-reference/keywords/switch)
- switch式(基本)  
  ```c#
  public static class SwitchExample
  {
      public enum Directions
      {
          Up,
          Down,
          Right,
          Left
      }

      public enum Orientation
      {
          North,
          South,
          East,
          West
      }

      public static void Main()
      {
          var direction = Directions.Right;
          Console.WriteLine($"Map view direction is {direction}");

          var orientation = direction switch
          {
              Directions.Up    => Orientation.North,
              Directions.Right => Orientation.East,
              Directions.Down  => Orientation.South,
              Directions.Left  => Orientation.West,
          };
          Console.WriteLine($"Cardinal orientation is {orientation}");
      }
  }
  ```
- switch式(Patterns and case guaeds)
  ```c#
  public static T ExhaustiveExample<T>(IEnumerable<T> sequence) =>
    sequence switch
    {
        System.Array { Length : 0}       => default(T),
        System.Array { Length : 1} array => (T)array.GetValue(0),
        System.Array { Length : 2} array => (T)array.GetValue(1),
        System.Array array               => (T)array.GetValue(2),
        IEnumerable<T> list
            when !list.Any()             => default(T),
        IEnumerable<T> list
            when list.Count() < 3        => list.Last(),
        IList<T> list                    => list[2],
        null                             => throw new ArgumentNullException(nameof(sequence)),
        _                                => sequence.Skip(2).First(),
    };
  ```
- switch文
  ```c#
  private static void ShowShapeInfo(Shape sh)
  {
    switch (sh)
    {
      // Note that this code never evaluates to true.
      case Shape shape when shape == null:
        Console.WriteLine($"An uninitialized shape (shape == null)");
        break;
      case null:
        Console.WriteLine($"An uninitialized shape");
        break;
      case Shape shape when sh.Area == 0:
        Console.WriteLine($"The shape: {sh.GetType().Name} with no dimensions");
        break;
      case Square sq when sh.Area > 0:
        Console.WriteLine("Information about square:");
        Console.WriteLine($"   Length of a side: {sq.Side}");
        Console.WriteLine($"   Area: {sq.Area}");
        break;
      case Rectangle r when r.Length == r.Width && r.Area > 0:
        Console.WriteLine("Information about square rectangle:");
        Console.WriteLine($"   Length of a side: {r.Length}");
        Console.WriteLine($"   Area: {r.Area}");
        break;
      case Rectangle r when sh.Area > 0:
        Console.WriteLine("Information about rectangle:");
        Console.WriteLine($"   Dimensions: {r.Length} x {r.Width}");
        Console.WriteLine($"   Area: {r.Area}");
        break;
      case Shape shape when sh != null:
        Console.WriteLine($"A {sh.GetType().Name} shape");
        break;
      default:
        Console.WriteLine($"The {nameof(sh)} variable does not represent a Shape.");
        break;
    }
  }
  ```
