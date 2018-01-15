## Item 40. 메서드 시그니쳐는 신중하게 설계하라

### method signiture

> Definition: Two of the components of a method declaration comprise the method signature—the method's name and the parameter types.

[https://docs.oracle.com/javase/tutorial/java/javaOO/methods.html](https://docs.oracle.com/javase/tutorial/java/javaOO/methods.html)  
즉, 메서드 이름과 파라미터에 대한 이야기이다,,,

cf. method declarations
```
public double calculateAnswer(double wingSpan, int numberOfEngines,
                              double length, double grossTons) {
    //do the calculation here
}
```

1. Modifiers : such as public, private, and others you will learn about later.
2. The return type : the data type of the value returned by the method, or void if the method does not return a value.
3. The method name : the rules for field names apply to method names as well, but the convention is a little different.
4. The parameter list in parenthesis : a comma-delimited list of input parameters, preceded by their data types, enclosed by parentheses, (). If there are no parameters, you must use empty parentheses.
5. An exception list
6. The method body : the method's code, including the declaration of local variables, goes here.

### 메서드 이름

메서드 이름은 컨벤션을 따라주라. \(규칙 56\)

#### 목표

1. 이해가 쉬움
2. 같은 패키지 내 다른 이름들과 일관성 유지 
3. 관습적인 네이밍에 맞게 
   -&gt; 모르겠다면 자바 라이브러리 API 메서드 이름을 참조해라 

### covenience 메서드

메서드는 _맡은 일이 명확하고 그 역할에 충실_해야 함  
클래스 내 메서드가 너무 많으면 학습, 사용, 테스트, 유지보수 측면에서 어려움

### parameter

#### parameter list는 짧게

가급적 4개 이하가 좋고, 인자의 자료형이 다른 것이 좋다. \(요즘은 IDE가 워낙 좋아서,,,\)

##### parameter list를 줄이는 방법

1. 여러 메서드로 나눠라 \(orthgonality\)

   * ex. \(I\) java.util.List
     * subList의 first/last index를 return 하는 메서드는 없다 \(methods to find the first or last index of an element in a sublist\)
     * getIndexOfElementInSubList\(int fromIndex, int lastIndex, Object o\) 여기에 first/last 여부까지 추가될 수도 있는거 아닌가?
     * subList\(int fromIndex, int toIndex\) + indexOf\(Object o\)/lastIndexOf\(Object o\)

2. helper class를 만들어, parameter를 그룹화 하라

   * 여러 메서드에 자주 등장하는 parameter들은 별도 entity로 만듦 

3. builder pattern 응용

   * builder pattern\(규칙 2\)을 복습해본다 
   * [https://stackoverflow.com/questions/13954672/adapting-the-builder-pattern-for-method-invocation](https://stackoverflow.com/questions/13954672/adapting-the-builder-pattern-for-method-invocation)
   * [https://stackoverflow.com/questions/2432443/best-practice-for-passing-many-arguments-to-method](https://stackoverflow.com/questions/2432443/best-practice-for-passing-many-arguments-to-method)
   ```
   XXXParameter param = new XXXParameter(objA, objB, date1, date2, str1, str2);
   XXXParameter class에 builder패턴 적용 
   ```

#### paramter type은 클래스보단 가급적 인터페이스로

특정 클래스에 종속될 필요는 없음

* ex. HashMap을 파라미터로 받는다면, 그 자체보다는 Map으로 받기 

#### parameter type으로 boolean보단 가급적 enum으로

확장성이 필요한 경우에만 적용되는 이야기 아닐까? 가독성 측면에선 좋을 듯...

```
public enum TemperatureScale { FAHRENHEIT, CELSIUS }

Thermometer.newInstance(TemperatureScale.CELSIUS)
```



