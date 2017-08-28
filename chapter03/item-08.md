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
#### 동치,,쒸익
* 반사성(reflexive) : x.equals(x) == true (x != null)
    + 모든 객체는 자신과 같아야 한다 (억지로 깨긴 힘들 것이다^^) 
* 대치성(symmetric) : y.equals(x) == true -> x.equals(y) == true (x, y != null) 
    + 두 객체에 서로 같은지 물어보면 같은 답이 나와야 한다 
* 추이성(transitive) : x.equals(y) == true && y.equals(z) == true -> x.equals(z) == true (x, y, z != null) 
    + 삼단 논법이냐.......
    + 전래기네 
* 일관성(consistent) : 호출 횟수에 상관없이 호출 결과는 항상 같다 (참조가 null이 아닐 때)
* x.equals(null) == false (x != null)

아오 죽겠다 
