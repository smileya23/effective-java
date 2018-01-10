## Item 39. 필요하다면 방어적 복사본을 만들라

* 자바는 안전한 언어이다.....이유가 어렵다.

  * This means that in the absence of native methods it is immune to buffer overruns, array overruns, wild pointers, and other memory corruption errors that plague unsafe languages such as C and C++

* 하지만, 안전하다 해도 완전한 자유는 아닌데, 클라이언트가 불변식\(invariant\)를 망가뜨리기 위해 최선을 다할 거라는 가정하에, 최대한 방어적으로 프로그래밍 해야한다.

* 해당 책에서서는 생성자/접근자 에 대한 방어적 복사본을 만들라고 언급

#### 객체 불변

```
// Broken "immutable" time period class
public final class Period {
 private final Date start;
 private final Date end;
​
 /**
  * @param start the beginning of the period
  * @param end the end of the period; must not precede start 
  * @throws IllegalArgumentException if start is after end
  * @throws NullPointerException if start or end is null
  */
 public Period(Date start, Date end) {
  if (start.compareTo(end) >0)
     throw new IllegalArgumentException(start + " after " + end);
  this.start = start;
  this.end = end;
 }
​
 public Date start() { return start; }
 public Date end() { return end; }
 ...  // Remainder omitted
}
```

* 첫번째 공격! \(생성자 공격\)

  ```
  // Attack the internals of a Period instance
  Date start = new Date();
  Date end = new Date();
  Period p = new Period(start, end); 
  end.setYear(78); // Modifies internals of p!
  ​
  // Repaired constructor - makes defensive copies of parameters
  public Period(Date start, Date end) {
   this.start = new Date(start.getTime());
   this.end = new Date(end.getTime());
   if (this.start.compareTo(this.end) > 0)
    throw new IllegalArgumentException(start +" after "+ end);
  }
  ```

  * 인자의 유효성을 검사하기 전에, 방어적 복사본을 만들어서 유효성 검사는 복사본에 대해서 시행

  * 취약 구간\(인자를 검사한 직후 복사본이 만들어지기 직전까지의 기간\) 동안에 다른 스레드가 인자 변경하는 것을 막기 위한 것

  * defensive copies are made before checking the validity of the parameters \(Item 38\), and the validity check is performed on the copies rather than on the originals

  * 인자로 전달 된 객체의 자료형이 제3자가 계승할 수 있을 경우, 방어적 복사본을 만들 때 clone을 사용하지 않는다.

    필드가 final이 아니므로 이미 공격을 위해 설계된 객체의 하위가 반환 될 수 있기 때문.

* 두번째 공격! \(생성자가 아닌 접근자 공격\)

  ```
  // Second attack on the internals of a Period instance
  Date start = new Date();
  Date end = new Date();
  Period p = new Period(start, end); 
  p.end().setYear(78); // Modifies internals of p!
  ​
  // Repaired accessors - make defensive copies of internal fields
  public Date start() {
   return new Date(start.getTime());
  }
  public Date end() {
   return new Date(end.getTime());
  }
  ```

  * 접근자의 경우는 생성자와 달리 clone을 사용 해도 됨.

  * 경험이 많은 개발자들은 new Date\(\) 인 reference 자료형 대신 primitive Long 타입인 Date.getTime\(\) 을 주로 사용함

  ​

#### 방어적 복사

* 계속 나오는 "방어적 복사"라는 내용은, 변경 불가능 클래스에서만 쓰이진 않는다.

* 클라이언트가 제공한 객체를 내부 자료구조에 반영하는 경우 \(Set, Map,,, 자료구죠형 등\), 물론 클라이언트에게 내부 컴포넌트를 반환할 때도 마찬가지.

* 길이가 0이 아닌 배열도 항상 변경 가능. 방어적 복사를 만들거나 변경 불가능 뷰를 만들어 반환 할 것

* 객체의 컴포넌트는 가능하다면 변경 불가능 객체를 사용 해야 함

#### 요악하자면,

* 클라이언트로 받거나 반화되는 변경 가능 컴포넌트는 반드시 방어적으로 복사해야 한다

* 하지만 복사의 오버헤드가 크고, 클라이언트가 부적적하게 사용하지 않는다는 보장이 있다면 해당 사실만 문서에 명시하고 넘어갈 수 있다…



