## Item 19. 인터페이스는 자료형을 정의할 때만 사용하라

인터페이스를 구현하는 클래스를 만들게 되면, 그 인터페이스는 해당 클래스의 객체를 참조할 수 있는 자료형 역할을 하게 된다

인터페이스의 주 목적은, 해당 클래스의 객체로 어떤 일을 할 수 있는지 클라이언트에게 알리는 행위!

다른 목적으로는 적절하지 못하다

#### 인터페이스를 잘못 사용한 예

* 상수 인터페이스 \(constant interface\)

  ```
  public interface PhysicalConstants {
      static final double AVOGADROS_NUMBER = 6.02214199e23;
      static final double BOLTZMANN_CONSTANT = 1.3806503e-23;
      static final double ELECTRON_MASS = 9.10938188e-31;
  }
  ```

  인터페이스 안에 메서드가 없고 static final만 존재. 상수 이름앞에 클래스 이름을 붙이는 번거로움을 피하기 위해 사용하지만, 잘몬된 예!           클래스가 어떤 상수를 사용하느냐는 구현 세부사항임!  \(ex. StreamConstants\)

* 상수 유틸리티는 아래와 같이 작성되어야 함

  ```
  public class PhysicalConstants {
      private PhysicalConstants(){} // 객체 생성을 막음

      public static final double AVOGADROS_NUMBER = 6.02214199e23;
      public static final double BOLTZMANN_CONSTANT = 1.3806503e-23;
      public static final double ELECTRON_MASS = 9.10938188e-31;
  }
  ```

  사용 시 보통 PhysicalConstant.AVOGADRO\_NUMBER과 같이 클래스명을 붙여야 하지만, JDK 1.5부터 도임된 static import를 사용하면 클래스 명 제거 가능함

* static import 적용 시

  ```
  import static com.yhkim.test.PhysicalConstants.*;

  public class Test {
     double atoms(double mols) {
        return AVOGADROS_NUMBER * mols;
     }
     // PhysicalConstants를 사용할 일이 많다면 정적 임포트가 적절하다.
  }
  ```



