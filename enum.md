# Java
- [クラス名#valueOf("値名")](https://github.com/openjdk/jdk/blob/9a19eb6918e1f766ccf1b1671ea1161a76fee571/src/java.base/share/classes/java/lang/Enum.java#L265) から値インスタンスを取得できる
- クラス名#values() にて値の配列を得ることができる
- クラス名#ordinal() にて定義順を得ることができると同時に、 [クラス名#compareT()](https://github.com/openjdk/jdk/blob/9a19eb6918e1f766ccf1b1671ea1161a76fee571/src/java.base/share/classes/java/lang/Enum.java#L195) による比較も可能

## decompluie してみる
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

# C#

