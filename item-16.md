## Item 16. 계승하는 대신 구성하라

interface를 계승할 때는 해당하지 않는 내용이다.

#### 계승은 캡슐화\(encapsulation\) 원칙을 위반한다.

* 하위 클래스가 정상 동작하기 위해서는 상위 클래스에 의존할 수 밖에 없다.
* 상위 클래스의 구현이 릴리스\(release\)가 거듭되면서 바뀔 수 있는데, 그러다 보면 하위 클래스 코드는 수정된 적이 없어도 망가질 수 있다. 



#### 망가질 수 있는, 깨질 수 있는 클래스

```
public class InstrumentedHashSet<E> extends HashSet<E> {
    // The number of attempted element insertions
    private int addCount = 0;
    { 생성자 생략 .. }
    @Override
    public boolean add(E e) {
        addCount++;
        return super.add(e);
    }

    @Override
    public boolean addAll(Collection<? extends E> c) {
        addCount += c.size();
        return super.addAll(c);
    }

    public int getAddCount() {
        return addCount;
    }
}
```

```
InstrumentedHashSet<String> s = new InstrumentedHashSet<String>();
s.addAll(Arrays.asList("Snap", "Crackle", "Pop"));
```

* 3건을 추가하면 size가 3일 것 같지만 놉. HashSet의 addAll이 내부적으로 add를 이용해서 구현하기 때문.
  * InstrumentedHashSet의 addAll\(\) 메소드 호출하여 집계. addCount=3
  * HashSet의 addAll\(\)을 호출. HashSet의 메서드는 합입할 원소마다 add 메서드 호출
  * InstrumentedHashSet의 add를 3번 호출. addCount=6
  * HashSet의 add 메서드 호출
* 2가지의 문제점이 도출되는데, 메소드 재정의를 안하면 괜찮을 것이라는 판단을 할 수 있다. 하지만 위험성이 완전히 사라지는 것은 아니다

  * 재정의한 addAll을 삭제하면 해결은 가능함. 이 해결 방법은 addAll이 add를 기반으로 구현했다는 사실에 근거한 것인데, 이는 릴리스가 거듭되며 바뀔 수 있지..
  * 다음 릴리스 때 상위 클래스에 새로운 메소드가 추가될 수도 있다

  #### 

#### 계승대신 구성하라!

* 구성\(composition\) - 위 언급된 문제를 피하는 방법은, 계승 대신 새로운 객체에 기존 클래스 객체를 참조하는 private 필드를 만드는 것!
* 전달\(forwarding\) - 기존 클래스의 메소드를 호출하여 그 결과를 반환하는 것!
* 전달 클래스

  ```
  public class ForwardingSet<E> implements Set<E> {
      private final Set<E> s;

      public ForwardingSet(Set<E> s) {
          this.s = s;
      }

      public void clear() {
          s.clear();
      }
      public boolean contains(Object o) {
          return s.contains(o);
      }
      public boolean isEmpty() {
          return s.isEmpty();
      }
      { .. 나머지 메서드 생략 .. }
      @Override
      public String toString() {
          return s.toString();
      }
  }
  ```

  * 기존 클래스의 구현 세부사항에 종속되지 않기 때문에 견고하다

* 상속받는 포장 클래스\(wrapper\)

  ```
  public class InstrumentedSet<E> extends ForwardingSet<E> {
      private int addCount = 0;

      public InstrumentedSet(Set<E> s) {
          super(s);
      }

      @Override
      public boolean add(E e) {
          addCount++;
          return super.add(e);
      }

      @Override
      public boolean addAll(Collection<? extends E> c) {
          addCount += c.size();
          return super.addAll(c);
      }

      public int getAddCount() {
          return addCount;
      }
  }
  ```

#### 계승은 언제 해야 할까?

* 상속은 하위 클래스가 상위 클래스의 하위 자료형\(subtype\)이 확실한 경우에만 바람직
* 클래스 B는 클래스 A와 "IS-A"관계가 성립할 때만 A를 계승 해야 한다. "아니다" 라면 B안에 A객체를 참조하는 private 필드를 두라!



