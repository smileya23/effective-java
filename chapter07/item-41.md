## Item 41. 오버로딩할 때는 주의하라

### Overloading

```
//잘못된 프로그램
public class CollectionClassifier
{
    public static String classify(Set<?> s){return "Set";}
    public static String classify(List<?> lst){return "List";}
    public static String classify(Collection<?> c){ return "Unknown Collection";}

    public static void main(String[] args){
        Collection<?>[] collections = 
            {new HashSet<String>(),new ArrayList<BigInteger>(),new HashMap<String, String>().values()};
            for(Collection<?> c : collections){
                System.out.println(classify(c));
            }
    }
}
/*
Unknown Collection
Unknown Collection
Unknown Collection
*/
```

* 오버로딩된 메서드 컴파일 시점에 결정

인자의 컴파일 시점 자료형은 전부`Collection<?>`으로 동일하다.

각 인자의 실행시점 자료형\(runtime type\)은 전부 다르지만, 선택 과정에는 영향을 끼치지 못한다.

인자의 컴파일 시점 자료형이`Collection<?>`이므로 호출되는 것은 항상`classify(Collection<?>)`메서드다.

### Overriding

그렇다면 재정의된 메서드란 무엇인가? 상위 클래스에 선언된 메서드와 같은 시그니처를 갖는 하위 클래스 메서드가 재정의된 메서드다. 하위 클래스에서 재정의한 메서드를 하위 클래스 객체에 대해 호출하면, 해당 객체의 컴파일 시점 자료형과는 상관없이, 항상 하위 클래스의 재정의 메서드가 호출된다.재정의된 메서드의 경우 선택 기준은 메서드 호출 대상 객체의 자료형이다.

객체 자료형에 따라 실행 도중에 결정되는 것이다.

```
class Wine {String name() { return "wine"; }}
class SparklingWine extends Wine{@Override String name(){ return "sparklingWine"; }}
class Champagne extends SparklingWine{ @Override String name(){ return "champagne"; }}

public class Overriding{
        public static void main(String[] args){
                Wine[] wines = {new Wine(), new SparklingWine(), new Champagne()};
                for(Wine wine : wines){
                        System.out.println(wine.name());
                }
    }
}
/*
wine
sparklingWine
champagne
*/
```

* **재정의 메서드 가운데 하나를 선택할 때 객체의 컴파일 시점 자료형은 영향을 주지 못한다. **

### Overloading vs Overriding

실행될 메서드는 컴파일 시에, 인자의 컴파일 시점 자료형만을 근거로 결정된다.

오버로딩된 메서드는 정적\(static\)으로 선택되지만, 재정의된 메서드는 동적\(dynamic\)으로 선택되기 때문이다.

**재정의 메서드 가운데 하나를 선택할 때 객체의 컴파일 시점 자료형은 영향을 주지 못한다. 오버로딩에서는 반대로 실행시점 자료형이 아무 영향도 주지 못한다. **

### 오버로딩된 메서드를 정상적으로 실행

* 실행시점 자료형을 근거로 인자의 자료형을 구별하기 위해 `instanceof`연산자를 이용해 자료형을 검사해야 한다.

```
public static String classify(Collection<?> c) {
         return c instanceof Set ? "Set" :
                c instanceof List ? "List" : "Unknown Collection";
}
```

오버로딩은 직관적인 예측에 반하므로 혼란스럽다. 따라서 오버로딩을 사용할 때는 혼란스럽지 않게 주의해야 한다.

혼란을 피하는 안전하고 보수적인 전략은,

* **같은 수의 인자를 갖는 두 개의 오버로딩 메서드를 API에 포함시키지 않는 것이다.**

  * 생성자는 다른 이름을 사용할 수 없으니 항상 오버로딩
    * 그것이 문제라면 정적 팩터리 메서드를 사용하는 옵션이 있다.

* ObjectOutputStream의 write메서드는 writeBoolean, writeInt, WriteLong과 같이 write를 재정의 하는 대신 작명 패턴을 사용해 정의되어 있다.

  * 각 메서드에 대응되는 read 메서드를 정의할 수 있게 된다. \(readBoolean\(\), readInt\(\), readLong\(\) 등을 정의할 수 있게 된다\).

* 같은 수의 인자를 받는 오버로딩 메서드가 많더라도 두 개의 오버로딩 메서드 비교했을 때 그 형인자 가운데 적어도 확실히 다르면 혼란을 겪지 않는다.

  * 확실히 다르다는 것은 서로 형변환\(cast\)할 수 없다면 확실히 다른것
    * 예를 들어 ArrayList에는 int를 받는 생성자와 Collection을 인자로 받는 생성자가 있다.
  * * 이 조건 충족시 실행 시점 자료형에 따라 오버로딩 메서드가 결정되고 컴파일 시점 자료형에 구애되지 않는다.

### 자동 객체화\(autoboxing\)의 문제

**remove\(E\) vs remove\(int\)**  
자바 1.5 이전에는 모든 기본 자료형은 참조 자료형과는 확실히 달랐다.

하지만 자동 객체화\(autoboxing\)라는 기능이 도입된 후 이제는 “확실히 다르다”라고 말할 수 없게 됐다.

```
public class SetList{

    public static void main(String[] args){
        Set<Integer> set = new TreeSet<>();
        List<Integer> list = new ArrayList<>();

        for(int i = -3; i <3; i++){
            set.add(i);
            list.add(i);
        }    
        for(int i = 0; i<3; i++){
            set.remove(i);
            list.remove(i);
        }
        System.out.println(set + " "+ list);
    }
}
/*
[-3, -2, -1] [-2, 0, 2] 
*/
```

* set.remove\(i\)는 인자의 값을 가진 모든 원소가 제거된다. 하지만 list.remove\(i\)는 해당 i번째 요소의 원소값을 지우게 된다.
  * 발생하는 원인은,`List<E>`인터페이스에 remove\(E\)와 remove\(int\)라는 오버로딩 메서드 두 개가 존재하기 때문이다.\(remove\(E\)는 인자로 들어온 값을 지우는 메서드, remove\(int\)는 인자 position의 값을 지우는 메서드\) 제네릭이 도입된 자바 1.5 이전에는 List 인터페이스에 remove\(E\) 대신 remove\(Object\)가 있었다. Object와 int는 완전히 다른 자료형이므로 문제가 될 것이 없었다. 하지만 제네릭과 자동 객체화\(autoboxing\)가 도입되면서, E와 int는 더 이상 완전히 다르다고 말할 수 없게 되었다.

### 문제 해결

* Integer로 형변환하여 올바른 오버로딩을 하거나 Integer.valueOf를 적용

```
for(int i =0; i<3; i++){
            set.remove(i);// 인자로 들어온 값을 지우고 싶기 때문에 remove(int)가 아닌 remove(E)형태가 되야 한다.            
            list.remove(Integer.valueOf(i)); // 아니면 remove((Integer) i);
}
```

### String클래스의 contentEquals

```
public boolean contentEquals(StringBuffer sb){return contentEquals((CharSequence) sb);}
```

* String클래스에는 contentEquals\(StringBuffer\)라는 메서드가 포함되어 있다.
* StringBuffer, StringBuilder, String, CharBuffer 같은 클래스에 공통 적용할 CharSequence가 새롭게 추가.
  * String contentEquals\(CharSequence\)가 새롭게 오버로딩됨
  * 전달되는 인자가 같을 경우 기존 메서드와 똑같이 동작한다.
* 이렇게 구현하는 표준 방법은 좀 더 구체적인 오버로딩 메서드가 좀 더 일반적인 오버로딩 메서드를 호출하게 한다.

### 정리

* 인자 개수가 같은 오버로딩 메서드는 일반적으로 피해야 한다

* 생성자는 위의 충고를 따르지 못하므로 형변환을 했을 때 여러 오버로딩 메서드를 호출할 수 있는 상황은 피하는 것이 좋다

* 위의 경우를 하지 못하면 새로운 인터페이스를 구현하도록 클래스를 개선하는데 같은 인자를 넘겨 호출했을 때 모든 오버로딩 메서드가 똑같이 동작하도록 해야한다.



