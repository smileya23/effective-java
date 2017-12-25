### Item 32. 비트 필드\(bit field\) 대신 EnumSet을 사용하라.

#### 비트 필드 {#as-is}

> public class Text{
>
> ```
>         public static final int STYLE_BOLD = 1 << 0 ; // 1
>
>         public static final int STYLE_ITALIC = 1 << 1 ; //2
>
>         public static final int STYLE_UNDERLINE = 1 << 2 // 4
>
>         public static final int STYLE_STRIKETHROUGH = 1 << 3 //8
>
>         public void applyStyle(int style) {....}  //비트별 OR 한 값이거나 0
> ```
>
> }
>
> text.applyStyle\(Text.STYLE\_BOLD \| Text.STYLE\_ITALIC\)

**장점**

* 비트 필드로 나타내면 비트 단위 산술 연산을 통해 집합 연산을 효율적으로 실행.

**단점**

* int enum 패턴과 똑같은 단점
* 비트 필드를 출력한 결과는 int enum 상수를 출력한 결과보다 이해하기 어려움
* 비트 필드에 포함된 모든 요소를 순차적으로 살펴보기도 어려움

#### java.util.EnumSet {#to-be}

> public class Text {
>
> ```
>  public enum Style { BOLD , ITALIC , UNDERLINE, STRIKETHROUGH }
>
>  public void applyStyles(Set<Style> styles){...}
>
> text.applyStyles(EnumSet.of(Style.BOLD, Style.ITALIC));
> ```
>
> }

**특징**

* 인자가 Set을 받도록 선언
  * 인자의 자료형으로는 클래스보다 인터페이스가 좋다\(규칙 52\) &gt; 다형성
  * 클래스를 사용하면 특정한 구현에 종속

**장점**

* `Set`인터페이스를 구현하기 때문에`Set`의 기능 제공
* 형 안전성, 다른 Set 구현들과 같은 수준의 상호운용성\(interoperability\) 제공
* 내부적으로 bit vector 사용
  * enum 갯수가 64 이하인 경우`EnumSet`은 long 값 하나만 사용. 그러므로, 비트 필드에 필적하는 성능이 나옴.
  * `removeAll()`이나`retainAll()`같은 일괄 연산도 비트 단위 산술 연산을 통해 구현

**단점**

* 자바 1.6에서는 immutable EnumSet 객체를 만들 수 없음 =&gt;
  Collections.unmodifiableSet으로 포장하거나, Guava 라이브러리\(Google\) 사용으로 해결



