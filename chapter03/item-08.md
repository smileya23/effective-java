## Item 08. equals를 재정의할 때는 일반 규약을 따르라 
### 웬만하면 equals method를 오버라이드 하지 마라! (ㅋㅋㅋㅋㅋㅋ)
기본 equals method는 오로지 자기 자신하고만 같은데(객체 동일성 체크), 아래 사항에 하나라도 해당하면 기본 eqauls를 써도 된다 
1. 각각의 객체가 고유하다 : 기본 equals method가 로직에 부합한다 
    * ex. Thread : 클래스의 인스턴스가 value가 아닌 활성 개체를 나타냄 
2. 동일성 검사가 필요 없다 
    * ex. java.util.Random
3. 상위 클래스의 equals가 사용하기 적절하다
    * ex. Set(extends AbstractSet), List(extends AbstarctList), Map(extends AbstractMap)  
4. private 클래스이고, eqauals를 쓸 일이 없다 

### 그럼 언제 오버라이드하냐? 
클래스가 논리적 동일성(logical equality)을 지원해야 할 때, 상위 클래스의 equals가 필요를 충족하지 못할 때
* 대부분의 값 class(value calss)
    + ex. Integer, Date
    + 다만, 객체가 싱글턴으로만 사용되는 value class의 경우 오버라이드가 필요 없음  
        - ex. enum

### equals method 오버라이드시 지켜야할 규약 
equals는 동치 관계(equivalence)를 구현
#### 동치
* 반사성(reflexive) : x.equals(x) == true (x != null)
    + 모든 객체는 자신과 같아야 한다 (억지로 깨긴 힘들 것이다^^) 
* 대치성(symmetric) : y.equals(x) == true -> x.equals(y) == true (x, y != null) 
    + 두 객체에 서로 같은지 물어보면 같은 답이 나와야 한다 
* 추이성(transitive) : x.equals(y) == true && y.equals(z) == true -> x.equals(z) == true (x, y, z != null) 
    + 삼단 논법이냐.......
    + ex. Poing(x, y) 이를 extends한 ColorPoint(x, y, c)
        - equals가 없다 : color 정보를 무시하므로 좋은 equals가 아님 
        - ColorPoint이며, color value가 같을 때만 true : 대치성 x (ColorPoint -> Point)
        - Point일 땐, color값은 비교하지 않음 : 대치성 o, but 추이성 x (x=ColorPoint, y=Point, z=ColorPoint 라면 ?)
    + 어쩌라고...?
        - 객체를 생성하면서 동치를 지키긴 어려움 
        - 결국... 객체를 상속(Inheritance)하는 대신 구성(composition)하라 (결국은 객체지향의 기본으로...ㅠㅠ)
* 일관성(consistent) : 호출 횟수에 상관없이 호출 결과는 항상 같다 (참조가 null이 아닐 때)
* x.equals(null) == false (x != null)
    + 이를 피하기 위해서 
    ```
    if (o == null) {
        return false;
    }
    ```
    와 같은 뻘짓을 하지만 필요 없다. o instanceOf xx 를 사용하면, o가 null일 때 항상 false를 반환하기 때문.

#### 정리를 해보자면
1. == 을 사용해 자기 자신 인지 확인 (reflexive) 후, 맞다면 return true
2. instanceOf 를 사용해 자료형 확인 후, 아니면 return false (null에 대한 비동치성)
3. parameter를 정확한 자료형으로 변환 (2번에 의해 에러가 안남)
4. 모든 필드가 동일한지 검사
    * 기본 자료형(float, double 제외) : ==
    * float, double : Float.compare, Double.compare
    * 객체 참조는 equals 재귀적 사용 
        + 필드 중, null 이 허용 될 때, 아래 중 하나 사용 
        ```
        (field == null ? o.field == null : field.equals(o.field))
        (field == o.field || (field != null && field.equals(o.field)))
        ```
    * 성능을 위해서라면 
        + 다를 가능성이 높은 필드, 비교비용이 낮은 필드부터 조사  
        + 중복 필드는 검사 안하는게 좋지만, 요약 정보가 있는 중복 필드의 경우 먼저 검사
5. equals 메서드 구현 후, 대칭성, 추이성, 일관성 맞는지 test 
6. equals 구현 시, hashCode도 재정의하라 (규칙 9래)
7. equals의 paramter를 Object로 사용
    * 특정 자료형에 대해서만 받을 시, Object.equals의 오버라이딩이 아닌 오버로딩인 셈 
