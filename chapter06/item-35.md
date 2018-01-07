### Item 35. 작명대신 어노테이션을 사용하라

#### **Java 1.5  이전 작명패턴 사용\(ex. JUnit 테스트 이름을 test로 시작하는 것\)의 문제점**

* 철자를 틀린 경우 알아차리기 힘들다
* 특정 프로그램 요소만 적용되도록 만들 수 없다
* 프로그램 요소에 인자를 전달할 마땅한 방법이 없다

#### **Annotation 등장**

* Annotation 종류

> * @Override
>
>   * 해당 메소드가 부모 클래스에 있는 메소드를 오버라이드 한 것을 명시적 선언
>
> * @Deprecated
>
>   * 더이상 사용되지 않는 클래스나 메소드 앞에 추가
>
> * @SuppressWarnings
>
>   * 컴파일러 warning을 제거하여 컴파일러에게 명시적으로  warning 제거
>
> * Meta Annotation
>
>   * @Target : 어떤 것에 annotation을 적용할지 선언할 때 사용
>   * @Retention : annotation 유지 시점 선언
>
>   * @Documented : 해당 annotaion information이 JavaDocs\(API\) 문서에 포함된다는 것을 선언
>
>   * @Inherited : 모든 자식 클래스가 부모 클래스의 annotation을 사용 할 수 있다는 것을 선언
>
>   * @Interface : annotation을 선언 할 때 사용

* Annotation 특징 : 상속이 되지 않음



