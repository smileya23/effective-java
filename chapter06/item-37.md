## Item 37. 자료형을 정의할 때 표식 인터페이스를 사용하라

## marker interface

* 아무런 메서드도 선언하지 않는 인터페이스
* 해당 인터페이스를 구현한 클래스는 _어떠한 속성을 만족한다_는 사실을 명시 
* ex. java.io.Serializable
  ```
  public interface Serializable {
  }
  ```

### maker interface vs maker annotation

* \(+\) interface는 자료형, annotaion은 그렇지 않다 
  * 에러를 컴파일 시점에 확인 가능 \(checkedException\)
  * ex. java.io.ObjectOutputStream
    ```
    /**
    * ...
    * <p>Exceptions are thrown for problems with the OutputStream and for
    * classes that should not be serialized.  All exceptions are fatal to the
    * OutputStream, which is left in an indeterminate state, and it is up to
    * the caller to ignore or recover the stream state.
    *
    * ...
    *
    * @throws  NotSerializableException Some object to be serialized does not
    *          implement the java.io.Serializable interface.
    * ...
    */
    public final void writeObject(Object obj) throws IOException {
      if (enableOverride) {
          writeObjectOverride(obj);
          return;
      }
      try {
          writeObject0(obj, false);
      } catch (IOException ex) {
          if (depth == 0) {
              writeFatalException(ex);
          }
          throw ex;
      }
    }
    ```
* \(+\) 적용 범위를 좀 더 세밀하게 지정 가능 
  * 특정 인터페이스를 구현한 클래스만 표시하고 싶을 때
  * 해당 인터페이스를 구현했는지만 확인하면 됨
* \(-\) 한 번 구현 후, 새로운 메서드 추가가 어려움 
  * 인터페이스라는 것이 public으로 공개가 되고 나면 수정이 거의 어려움 \(규칙 18\)

## marker annotation/interface 사용이 적절할 때

* 클래스/인터페이스 외 요소에 적용해야 할 때 -&gt; annotation 
* marker에 더 많은 정보\(메서드가 아닐까?\)를 추가할 가능성이 있을 때 -&gt; annotation 



