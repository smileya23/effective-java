# Item 22. 멤버 클래스는 가능하면 static으로 선언하라

### 중첩 클래스(nested class)

다른 클래스 안에 정의된 클래스로 해당 클래스가 속한 클래스 안에서만 사용됨.

바깥 클래스에 대한 접근이 필요 없는 멤버 클래스를 정의할 때는 항상 static member class를 만들어라!

중첩 클래스에 static을 생략하면 (비-정적) 내부적으로 바깥 객체에 대한 참조를 유지하기 때문에, 시간과 공간 요구량이 늘어난다!! (단점은 아래 있음)

- 정적 멤버 클래스(static member class)
  - 바깥 클래스의 모든 멤버(private 까지도) 접근 가능
  - 중첩된 클래스의 객체가 바깥 클래스 객체와 독립적으로 존재
  - 정적 멤버 클래스는 비-정적과 다르게 독립적으로 존재할 수 있다.
- 비-정적 멤버 클래스(nonstatic member class) -  내부클래스
  - 바깥 클래스 객체와 자동적으로 연결됨.
  - 바깥 클래스의 메소드 호출 가능, this 구문으로 바깥 객체에 대한 참조 획득 가능. 즉, 바깥 클래스 객체없이는 존재 못함.
  - 어댑터 정의할 때 자주 사용됨. Set/List 컬렉션 인터페이스를 구현하는 클래스
  -     public class Myset<E> extends AbstractSet<E> {
        public Iterator<E> iterator() {
        return new MyIterator();
        }
        
        private class MyIteraotr implements Iteraotr<E> {
        .....
        }
- 익명 클래스(anonymous class) - 내부클래스
  - 클래스 이름이 없다.
  - 함수 객체를 정의할 때 널리 쓰임
  - 바깥 클래스의 멤버도 아니다.
  - 멤버로 선언하지도 않으며, 사용하는 순간에 선언하고 객체를 만든다.
  - 예를들면, Comparator 객체 처럼 중간중간 등장. 단, 짧게 가독성 좋게 작성 되야 함.
- 지역 클래스(local class) - 내부클래스
  - 메소드 내에서 지역변수를 사용할 때
  - 4가지 중 사용 빈도가 가장 낮다
  - 지역변수(local variable)가 선언될 수 있는 곳 어디서든 선언 가능
  - 지역변수와 동일한 유효범위(scoping rule)를 따름.



### 요약하면

1. 각 4가지 중첩 클래스마다 특징과 어울리는 자리가 다름.
2. 중첩 클래스를 메서드 밖에서 써야하거나, 너무 긴 경우에는 멤버 클래스로 정의하되,
3. 멤버 클래스의 객체 각각이 바깥 클래스의 객체를 참조해야하는 경우에는 비정적으로 만들어라. 이것 빼곤 static member class가 좋다.



### 참고

http://jsnote.tistory.com/21
