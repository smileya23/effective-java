## Item 23. 새 코드에는 무인자 제네릭 자료형을 사용하지 마라 

### terms
* generics : Java 1.5 이후부터 지원하는 개념. 클래스/인터페이스/메서드에 어떤 타입의 객체가 사용될지를 (컴파일러에게) 명시하는 것.  
* 제네릭 자료형(generic type) : 선언부에 type이 parameter로 표현된 클래스/인터페이스 
    + 선언부에 형인자(type parameter)가 포함된 클래스/인터페이스 
        - type parameter
        - type paramter naming convention
            * E - Element (used extensively by the Java Collections Framework)
            * K - Key
            * N - Number
            * T - Type
            * V - Value
            * S,U,V etc. - 2nd, 3rd, 4th types 
    + <> (Diamond)사이에 실 형인자(actual type paramter) 목록을 붙인다  
    + 무인자 자료형(raw type) : 실 형인자 없이 사용되는 제네릭 자료형   
    + List<E> vs List
        -  List<E> : List 내 원소의 자료형이 E 이다 
        -  List : List<E>의 무인자 자료형 
    

* 형인자(type parameter) : 자료형을 인자로 나타내는 것?
* 형식 형인자(formal type parameter)  


### 제네릭은 왜 탄생 하였나? 
제네릭 도입 이전에는, 컬렉션(List, Map...)에서 객체를 읽을 때마다, type casting이 필요했음.  
=> 타입 캐스팅 하기 귀찮다... 
=> 더 중요한건 다른 자료형을 넣으면 *런타임*시 ClassCastException 발생! : 컴파일 시 에러를 발견할 수 있다 
=> 코드 re-use


### 무인자 자료형이 허용되는 이유
*호.환.성*  
그니까 새로운 코드에는 무인자 제네릭 자료형을 사용하지 말라거 ㅡㅡ

#### List vs List<Object>
모든 클래스는 (명시 없이도) Object를 extends. 그럼 List === List<Object> 아닙니꺼?  
=> List<Object>는 List내에 아무 객체나 넣을 수 있음을 컴파일러에게 명시, but List의 경우 자료형 검사 자체가 없음  
    * paramter의 타입이 List 일 때, List<String>은 전달 가능(List의 하위 자료형 List<String>)
    * paramter의 타입이 List<Object>일 때, List<String> 넘길 수 없음(List<String>은 List<Obejct>의 하위 자료형이 아니다 by 규칙 25)
    ```
        public static void main(String[] args) {
            List<String> strings = new ArrayList<>();
            unsafeAdd1(strings, new Integer(42)); // 문제 없음 
            unsafeAdd2(strings, new Integer(42)); // 컴파일 에러 
            String s = strings.get(0); // 
        }

        // List <- List<String> 가능 
        private static void unsafeAdd1(List list, Object o) {
            list.add(o);
        } 

        // List<Object> <- List<String> 불가능
        private static void unsafeAdd2(List<Object> list, Object o) {
            list.add(o);
        } 
    ```
    * 즉, parameter로 컬렉션을 넘길 때, 컴파일 시점에 자료형 검사를 할 수 있음 


#### 제네릭을 쓰고 싶지만, 실제 형인자를 모르거나/신경쓰고 싶지 않을때 
형인자를 ?로 써라 
#### ? vs E 

### 무인자 자료형을 써도 되는 경우 
1. 클래스 리터럴(class literal)
ex. List.class, int.class ... 
2. instaceof 
제네릭 자료형 정보는 런타임시 지워짐. (???)
