## Item 17. 계승을 위한 설계와 문서를 갖추거나, 그럴 수 없다면 계승을 금지하라 
override를 위한 설계와 문서를 갖추기 위해서는,,,  

#### 문서화 
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

cf. *override는 캡슐화의 원칙을 침해한다*

#### 뭐라 제목을 지어야 할지 
클래스 내부 동작에 개입할 수 있는 훅(hooks)을 신중하게 고른 protected method/field를 제공하라 
* ex. java.util.AbstractList
```
```

#### 테스트 

#### 제약사항 
1. 생성자는 override 가능한 메서드를 호출하면 안됨
2. 
