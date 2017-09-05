## Item 11. clone을 재정의할 때는 신중하라

### Clonable 
clone 허용을 알리는데 쓰려고 고안된 mixin interface. 
but clone 메서드는 존재하지 않는다 + Object의 clone 메서드는 protected
=> 즉, Cloneable을 구현해도 clone 메서드를 호출할 순 없다 (병맛)

대신 Object의 protected clone 메서드가 어떻게 동작할지를 정의한다. 
어떤 class가 Cloneable을 구현하면, Object의 clone 메서드에서 해당 객체를 필드 단위로 복사한 객체를 반환.  
Cloneable을 구현하지 않은 클래스의 clone 메서드를 사용시, Exception이 떨어짐.  
=> Object.clone() 메서드 호출시, 해당 class가 Cloneable을 구현했는지 확인하고 아니면 CloneNotSupportedExcepiton 을 던지는 것임 

*인터페이스를 굉장히 이상하게 사용(상위 클래스의 protected 멤버가 어떻게 동작할지 규정)하는 예로 이렇게 쓰면 안된다* 


```
public interface Cloneable {
    // 비 어 있 다 
}

public class Object {
    ...
    protected native Object clone() throws CloneNotSupportedException;
}

class Test impletments Cloneable (extends Object) {
    
}
```


#### Object.clone 메서드의 규약 
* x.clone() != x : always true
* x.clone().getClass() == x.getClass() : not an absolute requirement
* x.clone().equals(x) : not an absolute requirement

뭐...뭔소릴 하는겨?



### 제대로 동작하는 clone 메서드를 구현하는 방법
*Cloneable을 구현한 모든 class는, return type이 자기 자신인 public clone 메서드를 구현해야 한다*
* public clone 메서드 내부
    - 맨 처음 super.clone() 호출
    - 위에서 만들어진 객체에서, 수정해야하는 필드 수정
        + 반환할 객체의 type을 Cloneable을 구현한 class에 맞춘다
            * ex. PhoneNumber
            ```
            public PhoneNumber implements Cloneable {
                @Override
                public PhoneNumber clone() [throw CloneNotSupportedException] {
                    return (PhoneNumber) super.clone();
                }
            }        
            ```
            * 라이브러리가 할 일을 클라이언트에게 미루지 마라
        + 내부 복제일 때는 clone을 재귀적으로 호출
            * ex. Stack
            ```
            public Stack implements Cloneable {
                @Override
                public Stack clone() [throw CloneNotSupportedException] {
                    Stack result = (Stack) super.clone();
                    result.elements = elements.clone();
                    return result;
                }
            }        
            ```
            * clone 메서드는 또 다른 형태의 생성자. 원래 객체를 손상시키면 안된다. 
            * 만약 elements 필드가 final이면 불가능. => final 선언을 지워야 할 수도 있음
        + 내부 복제 재귀의 재귀,,, 재귀랄 
            * ex. HashTable
            * deepCopy 이용 : 내부 필드를 다시 복제하도록 
            * 모든 필드를 초기상태로 돌리고, 상위 레벨 메서드를 다시 호출(ex. setter)하여 객체 상태를 다시 만듬 
                - 간단 및 우아..하지만 빠르진 않음 
    - 필드가 전부 기본 자료형이거나 immutable이면 필드를 수정할 필요가 없음 
    - throw CloneNotSupportedException은 생략 
        + 컴파일 시 예외 처리 여부를 검사를 강요하는(checked exception) 메서드는 사용하기 불편 
        + but, 클래스가 계승을 위해 설계된 부모 클래스일 경우엔 Object.clone을 따라, protected/throw CloneNotSupportedException/Cloneable implements x 를 모두 충족해야함 
            * 그래야 하위 클래스에서 Cloneable을 implements 할 수 있겠지 

### clone 메서드를 정의하면 좋은 경우

### clone 메서드의 대안 
* copy constructor
    - 같은 클래스의 객체를 paramter로 받는 생성자 
    ```
    public Yum(Yum yum);
    ```
* copy factory method
    - 같은 클래스의 객체를 paramter로 받는 static factory 메서드 
    ```
    public static Yum newInstance(Yum yum);
    ```

장점은 Cloneable/clone이 가진 문제점을 안가진다는 것이겠지 ....  
