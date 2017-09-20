## Item 25. 배열 대신 리스트를 써라

배열과 제네릭을 뒤섞어 쓰다가 컴파일 오류나 경고메시지를 만나게되면, 배열을 리스트로 바꿔야겠다는 생각이 본능적으로 들어야 한다.!!



#### 배열과 리스트의 차이점

1. 배열은 공변 자료형\(covariant\), 리스트는 불변 자료형\(sivariant\)

   * 공변? 다른 현상의 변화에 따라 언제나 변하는 현상. Sub가 Super의 자식이라면, 이를 통해 정의한 Sub\[\] 배열 역시 Super\[\] 배열의 자식이라고 볼 수 있음.

   * 불변? 하나의 내용이 변한다고 해서 다른 내용도 함께 변하는 것은 아니다. 서로 다른 타입 Type1, Type2가 존재하는데, 불변타입이니 Generic은 List&lt;Type1&gt;과 List&lt;Type2&gt;는 아무런 관계가 없다.

   * 가장 큰 차이점! 배열 사용하여 에러나는 경우는 런타임 시 그 내용을 알 수 있고, Generic을 사용하여 에러가 발생하는 경우는 컴파일 시 내용을 알 수 있어 더 빠른 시점에 알 수 있다.

     ```
     Object[] objectArray = new Long[1];  
     //ArrayStoreException 예외 발생
     objectArray[0] = "I don't fit in";
     ​
     //컴파일 되지 않는 코드
     List <Object> ol = new ArrayList<Long>();  
     ol.add("I don't fit in");
     ```

2. 배열은 실체화\(reification\) 되는 자료형

   * 배열의 각 원소의 자료형은 실행할 때 결정됨.

   * 리스트는 컴파일 시점에만 자료형에 대한 조건들이 적용됨. 즉, 실제 실행할 때는 자료형에 대한 정보가 사라진다. 자료형 삭제덕에, 리스트 자료형은 Generic을 사용하지 않고 작성된 오래된 코드와도 문제없이 연동됨. \(규칙23..\)

#### 

#### 그리하여, 저 둘은 섞어 쓰기 어렵다.

```
List<String>[] stringLists = new List<String>[1];  // (1)
List<Integer> intList = Arrays.asList(42); // (2)
Object[] objects = stringLists; // (3)
objects[0] = intList;  // (4)
String s = stringLists[0].get(0); // (5)
```

1. 컴파일 안되지만, 일단 된다고 가정해보자 \(제네릭 배열은 만들 수 없ㅇ ㅓ!!\)

2. 42라는 하나의 원소를 갖는 배열 List&lt;Integer&gt;초기화

3. List&lt;Strring&gt;배열을 Object 배열 변수에 대입. 배열은 공변 자료형\(Object가 Super\)이므로 가능

4. List&lt;Ineter&gt;를 Object 배열에 있는 유일한 원소에 대입. 제네릭이 형 삭제를 통해 구현되므로 하자 없음.

5. 꺼낸 자료형을 자동적으로 String으로 변환. 사실은 integer이므로 프로그램 실행 도중에 ClassCastException 발생.

-&gt; 이런 일을 막으려면 \(1\) 처럼 제네릭 배열을 만들려고 하면 컴파일 할 때 오류가 발생해야 함.

