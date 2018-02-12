## 규칙 48. 정확한 답이 필요하다면 float와 double은 피하라

* float과 double은 기본적으로 과학 또는 엔지니어링 관련 계산에 쓰일 목적,,,

* 이진 부동 소수점 연산

* float 와 double은 각각 8,388,608 \(2^23\), 4,503,599,627,370,496\(2^52\) 까지 표시할 수 있고, 유효자리수\(소수점 아래의 숫자\)는 float는 7자리, double은 16자리이다.

* 특히 돈과 관계된 계산에는 적합하지 않음.

* #### 돈계산 예제 \(1달러로 10,20,30 센트의 사탕을 몇개사냐,, 거스름돈은 얼마인고,,\)

  ```
    double funds = 1.00;
    int itemsBought = 0;
    for (double price = .10; funds >= price; price += .10) {
      funds -= price;
      itemsBought++;
    }
    System.out.println(itemsBought + " items bought.");
    System.out.println("Change: $" + funds);
  }
  ​
  /* 실행결과
  3 items bought.
  Change: $0.3999999999999999
  */
  ```
* #### BigDecimal을 사용해 해결해버자

  ```
    final BigDecimal TEN_CENTS = new BigDecimal(".10");
  ​
    int itemBought = 0;
    BigDecimal funds = new BigDecimal("1.00");
    for(BigDecimal price = TEN_CENTS; funds.compareTo(price) >= 0; price=price.add(TEN_CENTS)){
      funds = funds.subtract(price);
      itemBought++;
    }
    System.out.println(itemBought + " item bought.");
    System.out.println("Meney left over: $" + funds);
  }
  ​
  /*실행결과
  4 item bought.
  Meney left over: $0.00
  */
  ```

  * 하지만, 기본 산술연산 자료형\(primitive arithmetic type\)보다 사용이 불편하고 느림
* #### int를 사용해 해결해버자

  ```
    int itemsBought = 0;
    int funds = 100;
    for(int price=10; funds >= price; price+=10) {
      funds-=price;
      itemsBought++;
    }
    System.out.println(itemsBought + " item bought.");
    System.out.println("Money left over: " + funds + " cents");
  }
  ​
  /*실행결과
  4 item bought.
  Money left over: 0 cents
  */
  ```

  * long과 int 둘 중 하나를 사용할 수 있는데 수의 크기, 소숫점 이하 몇 자리 표현인지에 따라 결정

#### 정리를 하면,

* 정확한 답 요구하는 문제 풀 때는 float이나 double을 쓰면 안되요

* 소수점 처리를 시스템에서 해야하고, 기본 자료형보다 사용하기 불편하고 성능이 떨어져도 괜찮으면 BigDecimal을 사용한다.

* 성능이 중요하고, 소수점 아래 수를 직접 관리해도 되고. 계산할 수가 심하게 크지 않으면 int나 long을 사용한다.

* 10진수 9개 이하 표현 가능은 int, 18개 이하로 표현 가능은 long, 그 이상은 BigDecimal을 사용해야 한다.

#### 참고,,

```
Type 
  Bits 
  Range of Values
----------------------------------------------------------------------------------------
byte 
 8bits 
  -2^7 ~ 2^7-1 (-128 ~ 127)
short 
 16bits 
  -2^15 ~ 2^15-1 (-32768 ~ 32767)
int 
 32bits 
  -2^31 ~ 2^31-1 (-2147483648 ~ 2147483647)
long 
  64bits 
  -2^63 ~ 2^63-1 (-9223372036854775808 ~ 9223372036854775807)
float 
 32bits 
  0x0.000002P-126f ~ 0x1.fffffeP+127f (3.40282347 x 1038, 1.40239846 x 10-45)
double 
  64bits 
  0x0.0000000000001P-1022 ~ 0x1.fffffffffffffP+1023 (1.7976931348623157 x 10308, 4.9406564584124654 x 10-324)
char 
  16bits 
  \u0000 ~ \uffff (0 ~ 2^15-1) * 자바에서 unsgined로 동작하는 자료형
boolean 
  1bit 
 true, false
```



