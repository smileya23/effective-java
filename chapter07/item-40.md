## Item 40. 메서드 시그니쳐는 신중하게 설계하라 
### method signiture
> Definition: Two of the components of a method declaration comprise the method signature—the method's name and the parameter types.

https://docs.oracle.com/javase/tutorial/java/javaOO/methods.html  
즉, 메서드 이름과 파라미터에 대한 이야기이다,,,

### 메서드 이름
메서드 이름은 컨벤션을 따라주라. (규칙 56)

#### 목표 
1. 이해가 쉬움
2. 같은 패키지 내 다른 이름들과 일관성 유지 
3. 관습적인 네이밍에 맞게 
-> 모르겠다면 자바 라이브러리 API 메서드 이름을 참조해라 

### covenience 메서드
메서드는 *맡은 일이 명확하고 그 역할에 충실*해야 함  
클래스 내 메서드가 너무 많으면 학습, 사용, 테스트, 유지보수 측면에서 어려움


### parameter 
#### paramter list는 짧게
가급적 4개 이하가 좋고, 인자의 자료형이 다른 것이 좋다. (요즘은 IDE가 워낙 좋아서,,,)
##### paramter list를 줄이는 방법 
1. 여러 메서드로 나눠라 (orthgonality)
    - ex. (I) java.util.List
        - subList의 first/last index를 return 하는 메서드는 없다 (methods to find the first or last index of an element in a sublist)
        - getIndexOfElementInSubList(int fromIndex, int lastIndex, Object o) 여기에 first/last 여부까지 추가될 수도 있는거 아닌가?
        - subList(int fromIndex, int toIndex) + indexOf(Object o)/lastIndexOf(Object o)

2. helper class를 만들어, parameter를 그룹화 하라 
여러 메서드에 자주 등장하는 parameter들은 별도 entity로 만듦 

3. builder pattern 응용
    + builder pattern(규칙 2)을 복습해본다 
    + 그래도 뭔소리야,,,
    + https://stackoverflow.com/questions/13954672/adapting-the-builder-pattern-for-method-invocation
    + https://stackoverflow.com/questions/2432443/best-practice-for-passing-many-arguments-to-method


#### paramter type은 클래스보단 가급적 인터페이스로 
특정 클래스에 종속될 필요는 없음 
- ex. HashMap을 파라미터로 받는다면, 그 자체보다는 Map으로 받기 

#### parameter type으로 boolean보단 가급적 enum으로 
확장성이 필요한 경우에만 적용되는 이야기 아닐까? 가독성 측면에선 좋을 듯...
```
public enum TemperatureScale { FAHRENHEIT, CELSIUS }

Thermometer.newInstance(TemperatureScale.CELSIUS)
```
