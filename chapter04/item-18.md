# Item 18. 추상 클래스 대신 인터페이스를 사용하라

* Abstract Class : abstract method가 하나 이상 포함, abstract로 정의된것
* Interface : 모든 method 가 abstract method 인것\( 자바 8에서는 default keyword를 이용해 일반 method의 구현 가능\)
  해당 함수의 구현을 강제함으로써 구현 객체의 같은 동작을 보장할 수 있다

## 추상 클래스 대신 인터페이스 사용 장점

* 이미 있는 클래스 개조해서 새로운 인터페이스를 구현하는 것은 간단  
* Interface는 믹스인\(mixin\) 정의하는데 이상적

  * 믹스인\(mixin\)?

    클래스가 주 자료형 \(primary type\)이외에 추가로 구현 할 수 있는 자료형,  
    어떤 선택적 기능을 제공한다는 사실을 선언하기 위해 사용  
    자료형의 주된 기능에 선택적인 기능을 혼합\(mix in\) 할 수 있도록 한다  
    ex\) Comparable은 어떤 클래스가 자기 객체는 다른 객체와의 비교 결과에 따른 순서를 갖는다고 선언할 때 mixin interface

  * Mixin 패턴이란?

    Mixin 패턴은 상속이 아닌 포함의 구조를 가진다. 즉, 공통 모듈을 가진 부모 객체를 상속하여 메시지를 교환하는 방식이 아니라 필요한 부분만 포함하여 사용한다는 뜻이다. Mixin 패턴의 장점은 복잡한 상속 구조의 애매함을 피할 수 있으며 행위를 분리함으로써 명확하고 다양한 인터페이스를 사용할 수 있다. Scala, Ruby, Python, Lisp 언어는 Mixin 패턴과 매우 유사한 기능을 언어 레벨에서 제공하고 있다. 그 외 Java, Javascript 언어들은 Mixin 기능을 우회하여 구현할 수 있다.

* 비 계층적인 자료형 프레임워크를 만들 수 있도록 한다

* 인터페이스를 사용하면 wrapper class 를 통해 안전하면서도 강력한 기능 개선이 가능
* abstract skeletal implementation 클래스를 중요 인터페이스마다 두면 인터페이스의 장점과 추상 클래스의 장점을 결합할 수 있다

## 추상 클래스 대신 인터페이스 사용 단점

* 추상클래스가 발전시키기기 쉽다
* 인터페이스가 공개되고 널리 구현된 다음에는 인터페이스 수정이 거의 불가능하다

  ㄴ 자바 1.8부터는 'default' 메서드를 통해 기존 클래스를 깨드리지 않고 새 메서드를 추가할 수 있다.  
     [http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)



