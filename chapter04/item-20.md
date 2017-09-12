## Item 20. 태그 달린 클래스 대신 클래스 계층을 활용하라
태그 달린 클래스...?   
=> 태그 필드가 있는 클래스  
=> 태그 필드 : 인스턴스의 형태 지정 

ex. 도형 (식상하다)
```
class Figure {
    enum Shape { RECTANGLE, CIRCLE };

    // tag field : 인스턴스의 형태 지정 
    final Shape shape;

    // Shape.RECTANGLE 일 때 사용하는 필드 
    double width;
    double height;

    // Shape.CIRCLE 일 때 사용하는 필드 
    double radius;

    // Shape.CIRCLE 생성자 
    Figure(dobule radius) {
        shape = Shape.CIRCLE;
        this.radius = radius;
    }

    // Shape.RECTANGLE 생성자 
    Figure(double height, double width) {
        shape = Shape.RECTANGlE;
        this.height = height;
        this.width = width;
    }

    // 도형의 넓이를 구하는 메서드 
    double area() {
        switch(shape) {
            case RECTANGLE :
                return width * height;
            case CIRCLE :
                return Math.PI * (radius * radius) ;
            default :
                thorw new AssertionError();
        }
    }
}
```

### 문제점 
1. 필요없는 필드가 많다 
    * 메모리 요구량 상승
    * 관리 이슈
        + 모든 필드에 final 추가시 : 관련 없는 필드를 초기화 해주어야 함 
        + 그렇다고 안하면? 생성자에서 잘못된 필드 초기화 시 runtime시에 뻗을 수 있음 (컴파일 단계에서 알 수 없다)
            - final로 선언된 필드는 반드시 초기화가 되어야함. 안되면 컴파일 시 에러 `Variable 'var' might not have been initialized.`
2. boilerplate가 반복된다 : enum, tag field, switch-case
    * 새로운 기능 추가시 switch-case 또 추가 
3. 인스턴스의 자료형만 보고, 인스턴스가 어떤 기능을 하는지 알기 어렵다 : 가독성 떨어짐 


### 해결방법 : subtyping(하위 자료형 정의)
1. 태그 필드에 따라 다르게 동작하는 메서드를 abtract method로 갖는 추상 클래스 정의 
    * area() 
    * ex.  
    ```
    abstract class Figure {
        abstract double area();
    }
    ```
2. 1의 추상 클래스에 태그 필드 값에 좌우되지 않는 메서드/필드 추가 
    * 
3. 태그 필드에 맞춰 동작하는 기능을 추상 클래스의 하위 클래스로 정의 
    * Rectangle, Circle class
    * ex.  
    ```
    abstract class Figure {
        abstract double area();
    }

    class Circle extends Figure {
        double area {
            return Math.PI * (radius * radius);
        }
    }

    class Rectangle extends Figure {
        double area {
            return width * height;
        }
    }
    ```
4. 3의 하위 클래스에 기능을 위한 메서드/필드 추가 
    * constructor, radius/width,height
    * ex.  
    ```
    abstract class Figure {
        abstract double area();
    }

    class Circle extends Figure {
        final dobule radius;

        Circle(dobule radius) {
            this.radius = radius;
        }

        double area {
            return Math.PI * (radius * radius);
        }
    }

    class Rectangle extends Figure {
        final double width;
        final double height;

        Rectangle(double height, double width) {
            this.height = height;
            this.width = width;
        }
        double area {
            return width * height;
        }
    }
    ```
