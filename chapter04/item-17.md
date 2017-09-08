## Item 17. 계승을 위한 설계와 문서를 갖추거나, 그럴 수 없다면 계승을 금지하라 
상속을 위한 설계와 문서를 갖추기 위해서는,,,  

### 문서화 
override가능한 메서드의 경우, 메서드 주석 마지막에 override에 관한 정보(메서드 내부 동작 원리)를 남겨라  
* 메서드를 어떤 순서로 호출하는지
* 호출 결과가 어떤 영향을 미치는지 
* ex. java.util.AbstractCollection
    ```
    public abstract class AbstractCollection<E> implements Collection<E> {
        public abstract Iterator<E> iterator();

        ....

        /**
         * {@inheritDoc}
         *
         * <p>This implementation iterates over the collection looking for the
         * specified element.  If it finds the element, it removes the element
         * from the collection using the iterator's remove method.
         *
         * <p>Note that this implementation throws an
         * <tt>UnsupportedOperationException</tt> if the iterator returned by this
         * collection's iterator method does not implement the <tt>remove</tt>
         * method and this collection contains the specified object.
         *
         * @throws UnsupportedOperationException {@inheritDoc}
         * @throws ClassCastException            {@inheritDoc}
         * @throws NullPointerException          {@inheritDoc}
         */
        public boolean remove(Object o) {
            ...
        }
    }
    ```
    + using the iterator's remove method
    + collection's iterator method does not implement the <tt>remove</tt> method and this collection contains the specified object
    + => iterator method를 재정의 하면 remove가 영향을 받는다 

cf. *상속은 캡슐화의 원칙을 침해한다*


### 상속을 위한 설계 
#### 내부 동작에 개입할 훅 제공  
클래스 내부 동작에 개입할 수 있는 훅을 protected method의 형태로 제공하라 
* ex. java.util.AbstractList
    ```
    public abstract class AbstractList<E> extends AbstractCollection<E> implements List<E> {
         /**
         * 어쩌구저쩌구 
         * <p>This method is called by the {@code clear} operation on this list
         * and its subLists.  Overriding this method to take advantage of
         * the internals of the list implementation can <i>substantially</i>
         * improve the performance of the {@code clear} operation on this list
         * and its subLists.
         *
         * <p>This implementation gets a list iterator positioned before
         * {@code fromIndex}, and repeatedly calls {@code ListIterator.next}
         * followed by {@code ListIterator.remove} until the entire range has
         * been removed.  <b>Note: if {@code ListIterator.remove} requires linear
         * time, this implementation requires quadratic time.</b>
         *
         * @param fromIndex index of first element to be removed
         * @param toIndex index after last element to be removed
         */
        protected void removeRange(int fromIndex, int toIndex) {
            ...
        }    
    }
    ```
    + called by the {@code clear} operation on this list and its subLists
    + 얘를 구현 하면 -> improve the performance of the {@code clear} operation
    + 만약 없었다면? clear의 성능이 O(n^2)임을 감안하고 사용하거나, subList자체를 다시 짜야함 

#### override 가능한 메서드는 내부 호출 금지 
##### 생성자의 경우 
생성자는 override 가능한 메서드를 호출하면 안됨  
* 인스턴스 생성시 호출 순서 : 상위 클래스 생성자 -> 하위 클래스 생성자 
    + 그래서 명시적으로 super(); 를 하기도 하는군... 
* override한 메서드가 하위 클래스 생성자 내 로직에 의존이 있을 경우, 에러 발생 
* ex.
```
public class Super {
    public Super() {
        overrideMe();
    }
    public void overrideMe() {
    }
}

public final class Sub extends Super {
    private final Date date;

    Sub() {
        // super();
        date = new Date();
    }

    @Override
    public void overrideMe() {
        System.out.println(date);
    }

    public static void main(String[] args) {
        Sub sub = new Sub();
        sub.overrideMe();
        // null
        // Fri Sep 08 10:26:05 KST 2017
    }
}
```
    + Super() -> overrideMe 호출 
    + Sub.overrideMe 실행 
    + Sub() -> date set 
    + sub.overrideMe() 호출 


#### Cloneable/Serializable 인터페이스 사용은 최대한 막자 
* 상속받은 하위 클래스에 너무 큰 책임이 몰림 
* 굳이 쓴다면, 생성자와 비슷하게 동작하는 clone/readObject 메서드는 override가능한 메서드를 호출하지 않도록 
* 굳이 쓰고, readResolve/writeReplace 메서드가 있다면, protected로 선언해야함 => private일 경우 하위 클래스에서 해당 메서드를 무시 (???)


### 테스트 
* protected 멤버는 최대한 적어야 함 -> 구현 세부 사항에 대한 서약이 되기 때문 
* 상속 가능한 클래스가 잘 설계되었는지 테스트하려면, 직접 하위 클래스를 만들어 봐라 
    + 이 사람 경험으로는, 하위 클래스 3개면 충분. 3개중 하나 이상은 상위 클래스 구현자가 아닌 사람인게 좋음 

### 하위 클래스 생성을 금지하는 법
* class를 final로 선언 
* 모든 생성자를 private/default로 선언 후, 생성자 대신 정적 팩터리 메서드 사용 
