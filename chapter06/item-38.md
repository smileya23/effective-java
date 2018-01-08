### Item 38. 인자의 유효성을 검사하라

메서드나 생성자를 구현시 받을 수 있는 인자에 대한 제한이 있다면 메서드 앞에서 유효성 검사를 해라

* #### 유효성 검사 필요성

  ```
    발생가능 에러 : 어떤 객체의 상태가 비정상적으로 바뀔 우려가 있음
  ```
* #### 유효성 검사 방법

> * public 메서드
>
>   ```
>    인자 유효성이 위반되었을 경우에 발생하는예외를 Javadoc의 @throws 태그를 사용 
>
>    보통 IllegalArgumentException, IndexOutOfBoundsException, NullPointerException이 이용된다.
>   ```
>
> * public 이 아닌 메서드
>
>   ```
>    확증문\(assertion\)을 사용
>
>      - 확증 조건은 항상 참 , 조건에 만족하지 않으면 AssertionError 발생
>
>      - 활성화하지 않은 확정문은 실행되지 않는다. 
>
>        (활성화 java 인터프리터에 -ea( - enableassertions) 옵션 주어야한다)
>
>   // 재귀적으로정렬하는 private 도움함수
>
>      private static void sort(long a[], int offset, int length) {
>
>       assert a != null;
>
>       assert offset >= 0 && offset <= a.length;
>
>       assert length >=0 && length <= a.length - offset;
>
>      … // 계산수행
>
>    }
>   ```
>
> * 생성자
>
>   ```
>    private선언
>   ```

* #### 결론

  ```
  메서드는 일반적으로 적용될 수 있도록 설계하는 것이 바람직하므로 인자의 제약이 적을수록 좋다.
  ```



