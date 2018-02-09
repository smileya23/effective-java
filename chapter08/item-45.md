## 규칙 45. 지역 변수의 유효범위를 최소화하라

지역 변수의 유효범위를 최소화하면 가독성과 유지보수성이 좋아지고, 오류 발생가능성이 줄어든다.



#### 지역 변수의 유효범위를 최소하하는 방법

* 처음으로 사용하는 곳에서 선언

* 거의 모든 지역 변수 선언에는 초기값이 포함되어야 한다.

* while문 보다는 for 문을 쓰는 것이 좋다.

  * while

    ```
    Iterator<Element> i = c.iterator();
    while(i.hasNext()) {
      doSomething(i.next());
    }
    ​
    ...
    ​
    Iterator<Element> i2 = c2.iterator();
    while(i.hasNext()) { //버그
      doSomething(i.next());
    }
    ```

    * 코드 복사 붙여놓기를 하다보니 생긴 버그로, i를 그대로 써버림.

    * 컴파일 단계에서 알 수 없으며, 실행도 되기 때문에 결과를 봐야만 탐지됨.

  * for

    ```
    for(Iterator<Element> i = c.iterator(); i.hasNext(); ){
      doSomething(i.next());
    }
    ​
    ...
    ​
    for(Iterator<Element> i2 = c2.iterator(); i.hasNext(); ){
      doSomething(i2.next());
    }
    ```

    * i를 찾을 수 없기때문에 컴파일 시점에 오류 발생

    ```
    for(int i = 0, n = expensiveComputation(); i < n; i++){
      doSomething(i);
    }
    ```

    * i, n 2개의 순환문 변수를 사용한 예

    * n이 i의 범위를 제한하는 용도로 쓰이는데, 그 값을 계산하는 비용이 큼. \(함수\)

    * 즉, 미리 계산해 넣어두고 사용하면, for문 안에서 매번 재계산해서 사용할 필요가 없다.

* 메서드의 크기를 줄이고 특정한 기능에 집중하라.

  


