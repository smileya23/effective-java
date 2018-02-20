## 규칙 53. 리플렉션 대신 인터페이스를 이용하라



#### 기능

* java.lang.reflect의 핵심 리플렉션 기능을 이용하면 메모리에 적재된 클래스의 정보를 가져오는 프로그램을 작성 할 수 있다.
* Class 객체가 주어지면 해당 객체의 생성자, 메서드, 필드를 나타내는 Constructor, Method, Filed 객체를 가져올 수 있다.
* * 클래스의 멤버 이름, 필드 자료형, 메서드 시그니처등의 정보를 얻을 수 있음
  * 실제 생성자, 메서드, 필드를 반영적\(reflectively\)로 조작 가능, 객체 생성, 메서드 호출, 필드 접근 가능
  * 객체에 정의된 어떤 메서드도 호출 가능, 컴파일 시 존재하지 않는 클래스 이용 가능

```
public static void main(String[] args) {
 // 클래스이름을 Class 객체로변환
 Class<?> cl = null;

 try {
  cl = Class.forName(args[0]);
 } catch(ClassNotFoundException e) {
  System.err.println("Class not found.");
  System.exit(1);
 }

 // 해당 클래스의 객체 생성
 Set<String> s = null;

 try {
  s = (Set<String>) cl.newInstance();
 } catch(IllegalAccessException e) {
  System.err.println("Class not accessible.");
  System.exit(1);
 } catch(InstantiationException e) {
  System.err.println("Class not instantiable.");
  System.exit(1);
 }

 // Set 이용
 s.addAll(Arrays.asList(args).subList(1, args.length));
 System.out.println(s);
}
```

* 프로그램의 첫번째 인자의 이름의 클래스를 이용해 Set&lt;String&gt; 개게를 만들고 나머지 인자를 집합에 넣고 출력 
* 리플렉션의 단점
* * 세가지 실행시점 오류 발생, 리플렉션이 아니라면 컴파일 시점에 검사할 수 있음
  * 클래스 객체 생성하기 위해 쓸데없는 코드 사용
* 리플렉션을 제한적 이용시 실제 프로그램 대부분에는 영향이 없다

**적절한사용법**

* 핵심 리플렉션 기능은 컴포넌트 기반 응용프로그램 제작 도구를 위해 설계
* 리플렉션이 필요한 프로그램
* 클래스 브라우저, 객체 검사도구, 코드 분석도구, 해석적 내장형 시스템, 스텁 컴파일러가 없는 원격 프로시저 호출
* **일반적인프로그램은 프로그램 실행 중에 리플렉션을 통해 객체를 이용하려하면 안됨**
* **리플렉션을 아주 제한적으로만 사용하면 오버헤드를 피하면서도 리플렉션의 다양한 장점을 누릴 수 있다**
* **객체 생성은 리플렉션으로 하고 객체 참조는 인터페이스나 상위 클래스를 통하면 된다**



