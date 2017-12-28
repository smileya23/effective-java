## Item 30. int 상수 대신 enum을 사용하라

고정 개수의 상수들로 값이 구성되는 자료형



#### int enum 패턴 :_사용하면안된다_

```
public static final int APPLE_FUJI = 0;
public static final int APPLE_PIPPIN =1;
...
int i = (APPLE_FUJI - ORANGE_TEMPLE) / APPLE_PIPPIN; // 변수가 섞임
```

* int enum 그룹의 크기를 알기 어렵다

* 디버깅이 불편하다

  ​

#### enum의 특징!

```
public enum Apple { FUJI, PIPPIN, GRANNY_SMITH}
public enum Orange { NAVEL, TEMPLE, BLOOD }
```

* C/C++/C\# 언어가 제공하는 enum 이랑 다르게, JAVA에서는 참조 자료형 \(다른 언어는 int\)

* 열거 상수 별로 public static final 필드 형태로 제공

* 클라이언트에서 접근할 수 있는 생성자는 없음. 새로운 객체 생성이나 계승 불가능

* 컴파일 시점에 형 안전성 제공. \(Item 21 참고\)

* 이름 공간 분리되어 있기 때문에 같은 이름의 상수가 공존 가능

* toString 메서드를 호출 가능해서 문자열로 쉽게 변경 가능

* Object 에 정의된 메서드들이 포함 되어 있음

* Comparable, Serializable 인터페이스 구현되어 있음

  ​

#### enum 자료형에 메서드나 필드 추가

```
public enum Planet {
  MERCURY (3.302e+23, 2.439e6),
  VENUS (3.302e+23, 2.439e6),
  EARTH (5.975e+24, 6.367e6),
  MARS (3.302e+23, 2.439e6),
  JUPITER (3.302e+23, 2.439e6),
  SATURE (3.302e+23, 2.439e6),
  URANUS (3.302e+23, 2.439e6),
  NEPTUNE (3.302e+23, 2.439e6);
​
  private final double mass; // 킬로그램 단위
  private final double radius; // 미터 단위
  private final double surfaceGravity;
  private static final double G = 6.67300E-11; //중력상수
​
  //생성자
  Planet(double mass, double radius){
    this.mass = mass;
    this.radius = radius;
    surfaceGravity = G * mass / (radius * radius);
  }
​
  // 접근자
  public double mass() { return mass; }
  public double radius() { return radius; }
  public double surfaceGravity() { return surfaceGravity; }
​
  // 추가 메서드
  public double surcfaceWight(double mass){
    return mass * surfaceGravity; // F = ma
  }
​
  // 사용
  public static void main(String[] args) {
    double earthWeight = 3.3333;
    double mass = earthWeight / Planet.EARTH.surfaceGravity();
  ​
    // values 라는 static 함수 존재. 이외에도 기본 제공함수들이 많음!
    for(Planet p : Planet.values() ){
      System.out.println(p + " surcfaceWight >>>" +  p.surcfaceWight(mass));
    }
  }
}
```

* enum 상수 묶음에서 출발해서 점차로 완전한 기능을 갖춘 추상화 단위로 발전 가능

* enum 상수에 데이터를 넣으려면 객체 필드를 선언하고, 생성자를 통해 받은 데이터를 그 필드에 저장

* enum 도 클라이언트에게 공개할 이유가 없다면 private 나 package-private로 선언

* 공개한다면 최상위 public 클래스로 생성하거나, 최상위 클래스의 멤버 클래스로 선언

* enum 상수를 선언된 순서대로 저장하는 배열을 반환하는 static values 메소드 기본으로 정의되어 있음

  ​

#### 상수별 메서드 구현 \(constant-specific method implementation\)

```
public enum Operation {
  //상수별 메서드 구현
  PLUS {double apply(double x, double y){ return x + y;}},
  MINUS{double apply(double x, double y){ return x - y;}},
  TIMES {double apply(double x, double y){ return x * y;}},
  DIVIDE {double apply(double x, double y){ return x / y;}};
​
  abstract double apply(double x, double y);
​
  // 사용
  public static void main(String[] args) {
    double x = 6;
    double y = 3;
    for (Operation op : Operation.values())
      System.out.printf("%f %s %f = %f%n", x, op, y, op.apply(x, y));
  }
}
```

* enum 상수별 각 다른 동작이 필요할 때는, switch 문 대신 상수별 메서드를 구현해야 한다!

* abstract 메서드가 모든 상수에 반드시 구현해야 하기 때문에 구현 잊을 가능성이 거의 없다

* 단, 단점은 enum 상수끼리 공유하는 코드를 만들기는 어렵다

  ​

#### 코드를 공유하면서 분기가 필요할 경우

```
enum PayrollDay {
  MONDAY(PayType.WEEKDAY),
  TUESDAY(PayType.WEEKDAY),
  WEDNESDAY(PayType.WEEKDAY),
  THURSDAY(PayType.WEEKDAY),
  FRIDAY(PayType.WEEKDAY),
  SATURDAY(PayType.WEEKEND),
  SUNDAY(PayType.WEEKEND);
​
  private final PayType payType;
​
  PayrollDay(PayType payType) {
    this.payType = payType;
  }
​
  double pay(double hoursWorked, double payRate) {
    return payType.pay(hoursWorked, payRate);
  }
​
  //정책 enum 자료형
  private enum PayType {
    WEEKDAY {
     double overtimePay(double hours, double payRate) {
       return hours <= HOURS_PER_SHIFT ? 0 : (hours - HOURS_PER_SHIFT) * payRate / 2;
     }
    },
    WEEKEND {
      double overtimePay(double hours, double payRate) {
        return hours * payRate / 2;
      }
    };
  ​
    private static final int HOURS_PER_SHIFT = 8;
  ​
    // 추상 메소드! 확장성에 좋다!
    abstract double overtimePay(double hrs, double payRate);
  ​
    double pay(double hoursWorked, double payRate) {
     double basePay = hoursWorked * payRate;
     System.out.println( basePay + ", " + overtimePay(hoursWorked, payRate));
     return basePay + overtimePay(hoursWorked, payRate);
    }
    }
}
```

* 예를들어 주중은 초과 근무시간만 추가수당으로 계산되고, 주말은 모든 근무시간이 추가 수당으로 계산 되야 하는 경우

* 주중과 / 주말로 분기를 태우면 분기 하나가 더 추가 될 경우에 대한 위험 발생

* 정책 enum 자료형을 선택하라



#### 마무리 정리를 하자면,

* enum 자료형은 int 상수에 비해 더 많은 장점을 가지고 있다.

* enum은 코드 가독성이 높고, 안전하며, 더 강력

* enum은 생성자나 멤버가 필요 없으나, 데이터 또는 그 관련된 메스드를 추가해서 기능을 향상시킬 수 있다.

* 다른 동작의 상수별 구현을 위해서는 상수별 메서드를 구현하라.

* 여러 enum 상수가 공통 기능을 이용해야 하는 경우, 정책 enum 패턴을 고려하라.











  


