## Item 34. 확장 가능한 enum을 만들어야 한다면 인터페이스를 이용하라

### enum types vs typesafe enum pattern 
- enum types가 모든 면에서 우월.
- 그러나 enum types의 경우 확장성이 없음 (extends 불가)
- 불편하긴 하지만... 확장 가능한 enum이 꼭 필요한 것이냐?
    + (-) ???
    + (-) ???
    + (-) ???
    + (+) 사용자가 확장하여 사용할 때는 필요 ex. 연산 코드

### 인터페이스를 활용해 enum을 구현 
```
public enum Operation {
  PLUS {double apply(double x, double y){ return x + y;}},
  MINUS{double apply(double x, double y){ return x - y;}},
  TIMES {double apply(double x, double y){ return x * y;}},
  DIVIDE {double apply(double x, double y){ return x / y;}};
​
  abstract double apply(double x, double y);
}

// 사용
public static void main(String[] args) {
  double x = 6;
  double y = 3;
  for (Operation op : Operation.values())
    System.out.printf("%f %s %f = %f%n", x, op, y, op.apply(x, y));
}
```

```
public interface ExtensibleOperation {
    double apply(double x, double y);
}

public enum BasicOperation implements ExtensibleOperation {
    PLUS("+") {
        public double apply(double x, double y) {
            return x + y;
        }
    },
    MINUS("-") {
        public double apply(double x, double y) {
            return x - y;
        }
    },
    TIMES("*") {
        public double apply(double x, double y) {
            return x * y;
        }
    },
    DIVIDE("/") {
        public double apply(double x, double y) {
            return x / y;
        }
    };
    private final String symbol;

    BasicOperation(String symbol) {
        this.symbol = symbol;
    }

    @Override
    public String toString() {
        return symbol;
    }

    public double apply(double x, double y) {
        return 0;
    }
}
```
- 새로운 연산이 필요하다면, extensibleOperation 인터페이스를 구현하여 새로운 enum을 작성하면 됨 
- API에는 인터페이스를 공유하면, 새로 구현한 enum들도 사용 가능.
- (-) enum 자체를 상속받는게 아니기 때문에, 중복 코드가 발생할 수 있음 (생성자, toString 같은 메서드)
    + 중복되는 코드가 많다면, helper class나 static helper method를 사용하자 