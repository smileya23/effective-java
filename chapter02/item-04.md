## Item 04. 객체 생성을 막을 때는 private 생성자를 사용하라 
* static method/field만 모인 클래스의 필요성
    - primitive type, array에 적용되는 method를 모을 때
        + ex. java.lang.Math, java.util.Arrays
    - static method를 모을 때
        + 목적 : interface 구성 (factory method pattern)
        + ex. java.util.Collections
    - final 클래스에 사용되는 method를 모을 때 ???

* 이런 유틸리티성 class는 인스턴스 생성을 하는 것이 이상함. 
    - 그러나 생성자를 생략하면 default constructor(public)가 생김 
    - 클래스를 abstract로 선언하면, 이 클래스를 extends 하면 인스턴스 생성이 가능해짐 

* 그럼 어쩌라고?
    - private constructor를 하나 만들자!
        + constructor가 private이므로 외부에서 사용 불가
        + 하위 클래스에서도 접근 가능한 constructor가 없으므로, 하위 클래스 생성 불가 
        ```
        public class Test {
            private Test() {
            }
        }

        public class SubTest extends Test {
            => There is no default constrcutor available in 'Test'
        }
        ```