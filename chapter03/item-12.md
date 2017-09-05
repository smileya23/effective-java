## Item 12. Comparable 구현을 고려하라.

Comparable Interface
_동치성 검사 & 순서비교가 가능_


```
public interface Comparable<T> {
	int compareTo(T t)
}
```

### compareTo 메서드의 생성 규약

```
* 비교대상 > 대상 => 음수
* 비교대상 < 대상 => 양수
* 비교대상 == 대상 => 0
* 서로 비교대상이 아닐때 ClassCastException 처리
```

1. x,y에 대해 sgn(x.compareTo(y)) == -sgn(y,compareTo(x))
   ㄴ 대소 관계의 역이 성립
2. 추이성(transitivity) 만족
  (x.compareTo(y) > 0 && y.compareTo(z) > 0) => x.compareTo(z) > 0
3. x.compareTo(y) == 0 , sgn(x.compareTo(z)) == sgn(y.compareTo(z)) 모든 z에 대해 성립
추천조건)) (x.compareTo(y) == 0) == (x.equals(y)) -> equals에 부합하지 않을 시 명시하는 것이 좋다 



* compareTo 규약을 준수하지 않은 클래스는 비교 연산에 기반한 클래스들을 오동작 시킬수 있다

 비교연산기반클래스
 TreeSet, TreeMap 같은 sorted collection
 Arrays, Collection같은 유틸 클래스 
 
* Comparable 인터페이스가 자료형을 인자로 받는 제네릭 인터페이스이므로 compareTo 메서드의 인자 자료형은 컴파일 시간에 정적으로 결정된다

인자로 받은 객체의 자료형을 검사하거나 형 변환할 필요가 없다. 잘못된 자료형 객체를 인자로 넘길 경우 아예 컴파일이 되지 않으므로 호출이 불가능하다.
null로 받을 경우 compareTo 메서드는 반드시 NullPointerException 발생
객체 참조 필드는 compareTo 메서드를 재귀적으로 호출하여 비교한다
Comparable을 구현하지 않고 있거나 좀 특이한 순서 관계를 사용해야할 경우 Comparator를 명시적으로 사용할 수 있다.

* Comparable vs equals
* Comparable vs Comparator 
 - http://developer88.tistory.com/75