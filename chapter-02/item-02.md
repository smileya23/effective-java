\#\# Item 02. 생성자 인자가 많을 때는 Builder 패턴 적용을 고려하라

\#\#\#\# required field는 몇 개 없고, optional paramter가 많을 때 어떻게 할까?

\* Telescoping Constructor Pattern \(점층적 생성자 패턴\)

    - 필수 parameter만 받는 생성자 하나 정의 후, optional paramter를 갖는 생성자를 계속해서 추가

    - \(-\) paramter가 늘어나면 읽기 복잡하고, 

    - \(-\) 동일한 타입의 parameter가 많다면 순서에 유의해야함\(컴파일러는 틀린지 모른다\)

\* JavaBeans Pattern \(자바빈 패턴\)

    - paramter가 없는 생성자로 객체 생성 후, setMethod를 통해 필드를 채움 

    - \(-\) 일시적으로 객체 일관성\(consistency\)이 깨진다 -&gt; 디버깅이 어려움 

        + 1회의 함수 호출로는 객체 생성이 끝나지 않음 \(set을 해줘야함\)

    - \(-\) immutable한 class 생성이 불가능 



... -\_- 그럼 어쩌라고?

\* Builder Pattern 

    - Telescoping Constrcutor P의 안정성 + JavaBeans P의 가독성 

    - step

        + 필수 paramter로 빌더 객체 생성

            \* Builder class는 객체 내 static class임 

            \* Builder constrcutor의 parameter는 위의 필수 parameter를 받음 

        + 빌더 객체에 정의 된 메서드로 optional field를 채움

            \* JavaBean Pattern의 setMethod가 여기에 구현 

        + build 메서드 호출로 객체 생성\(이 객체는 immutable 하다\)

            \* 여기서 실제 객체 생성 

