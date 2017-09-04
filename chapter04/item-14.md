## Item 14. public 클래스 안에는 public 필드를 두지 말고 접근자 메서드를 사용하라 
```
public class Point {
    public double x;
    public double y;

}
```
무엇이 문제인가? => encapsulation  

* public class의 경우, 필드는 private으로 접근자/수정자(getter/setter) 메서드는 public으로 만들어라
    ```
    class Point {
        private double x;
        private double y;

        public double getX() { return x; }
        public double getY() { return y; }

        public void setX(double x) { this.x = x; }
        public void setY(double y) { this.y = y; }
    }
    ```
    + 이런 것을 잘 안 지킨게, java.awk 내 Point, Dimension 
        - 성능 때문에 어쩔 수 없었음 (규칙 55)
    + public class 내, immutable field(final)의 경우, public이여도 상관없지만 굳이 그럴 필요가 있나...?
* package-private/private nested class의 경우, 필드가 공개여도 뭐...



