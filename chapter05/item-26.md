## Item 26. 가능하면 제네릭 자료형으로 만들 것 
### 클래스를 제네릭화 하기 

```
public class Stack {
    private Object[] elements;
    private int size = 0;
    private static final int DEFAULT_INITIAL_CAPACITY = 16;
        
    public Stack() {
        elements = new Object[DEFAULT_INITIAL_CAPACITY];
    }

    public void push(Object e) { 
        ensureCapacity(); elements[size++] = e;
    }

    public Object pop() { 
        if (size == 0) throw new EmptyStackException();
        
        Object result = elements[--size];
        elements[size] = null; // Eliminate obsolete reference 

        return result;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    private void ensureCapacity() {
        if (elements.length == size)
            elements = Arrays.copyOf(elements, 2 * size + 1);
    } 
}
```
1. 선언부에 형인자(type parameter)를 추가 
```
public class Stack<E> {
    ...   
}
```
2. 기존의 자료형을 사용하는 부분을 형인자로 대체 
```
public class Stack<E> {
    private E[] elements;

    public Stack() {
        elements = new E[DEFAULT_INITIAL_CAPACITY]; // 에러 발생 : 실체화 불가능 자료형(E) 로는 배열 생성 불가능 
    }

    public void push(E e) { 
        ensureCapacity(); elements[size++] = e;
    }

    public E pop() { 
        if (size == 0) throw new EmptyStackException();
        
        E result = elements[--size];
        elements[size] = null; // Eliminate obsolete reference 

        return result;
    }
}
```
3. 에러나는 부분 잡기 
    * 방법 1 : 배열은 Object로 만들고, 형변환 
        + warning : 형 안정성 보장이 되지 않음 
        + 해당 배열에는 언제나 E 타입만 들어가기 때문에, @SupressWarnings("unchecked") 어노테이션추가 
    * 방법 2 : element의 자료형을 Object[] 로 변환
        + elements에서 꺼낸 자료형을 E로 타입 캐스팅  