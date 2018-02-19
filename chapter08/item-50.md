## 규칙 50. 다른 자료형이 적절하다면 문자열 사용은 피하라

#### **문자열로해서는안되는일**

* **문자열은 값 자료형 \(value type\)을 대신하기에는 부족**

  ```
    데이터가 파일, 네트워크, 키보드를 통해서 들어오면 문자열이다
  ```

  * 숫자라면 int, float, BigInteger 같은 수 자료형으로 변환

  * 예, 아니오 질문의 답은 boolean으로 변환
  * 적절한 값 자료형이 있다면 해당 자료형을 사용

* **문자열을 enum 자료형을 대신하기에는 부족**

* **문자열은 혼합 자료형\(aggregate type\)을 대신하기에는 부족**

```
String compoundKey = className + "#" + i.next();
```

* 필드 구분자가 필드안에 들어가면 문제 발생
* 필드 사용하려면 파싱을 해야함 
  . equals, toString, compareTo 메서드 제공 불가, String의 기능만 사용 가능
* 혼합 자료형 표현할 클래스를 만들어야한다 종종 private static 멤버 클래스로 선언됨

#### **문자열은 권한 \(capablity\)을 표현하기에 부족**

* **문자열을 권한으로 사용하는 잘못된 예**

```
public class ThreadLocal {
    private ThreadLocal(){} //객체 생성 불가
    public static void set(String key, Object value){}// 주어진 이름이 가리키는 지역변수의 값 설정
    public static void get(String key){}//주어진 이름이 가리키는 지역변수의 값 반환
}
```

* 문자열이 스레드 지역변수의 전역적 이름공간, 클라이언트가 제공하는 문자열 키의 유일성이 보장되어야함

* 악의적 클라이언트가 다른 클라이언트와 같은 문자열 사용시 다른 클라이언트의 데이터 접근 가능

* **문자열 대신 위조 불가능\(unforgeable\) 키로 바꾸면 해결\(이런 키를 권한 capability라고 부름\)**

  ```
  public class ThreadLocal {
      private ThreadLocal() {} //객체생성불가

      public static class Key { //권한
          Key() { }
      }
      public static Key getKey() {//유일성이 보장되는, 위조 불가능 키를 생성
          return new Key();
      }
      public static void set(String key, Object value) {}
      public static void get(String key) {}
  }
  ```

* 정적 메서드를 없애고, 키의 메서드로 바꿈, 키를 지역 변수의 키가 아니라 스레드 지역변수로 고침

```
public final class ThreadLocal {
 public ThreadLocal() {};
 public void set(Object value) {};
 public Object get() {
  return null;
 };
}
```

* 변수에서 꺼낼 때 Object 에서 실제 자료형으로 형변환 형 안정성 제공 못함 
* **ThreadLocal 클래스를 제네릭으로 선언하면 형 안정성 보장가능**

```
public final class ThreadLocal<T>{
    public ThreadLocal() {};
    public void set(T value) {};
    public T get() {
        return null;
    };
}
```

**요약**

* 더 좋은 자료형이 있거나 만들 수 있으면 객체를 문자열로 표현하는 것을 피해야 한다.

* 문자열을 제대로 다루지 못하면 성가시고, 유연성이 떨어지고 오류 발생 가능성이 높음

* 문자열이 적합하지 않는 자료형은 기본 자료형, enum 혼합자료형이 있다.



