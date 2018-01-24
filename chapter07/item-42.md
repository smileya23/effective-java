# 규칙 42. varargs는 신중히 사용하라



#### 가변 인자 메서드 \(variable arity method\)

* Java 1.5부터 공식적으로 가변 인자 메소드라 부르는 varargs가 추가됨.

* 이 메소드는 지정된 자료형의 인자를 0개 이상 받을 수 있다.

* 클라이언트에서 전달한 인자 수에 맞는 배열이 자동 생성되고, 배열이 메서드에 인자로 전달됨.



#### varargs 사용 예

```
static int sum(int... args) {
  int sum = 0;
  for (int arg : args)
  sum += arg;
  return sum;
}
```



#### 0 이상이 아닌 1개 이상 필요할 때 - 흉한 예

```
static int min(int... args) {
 if (args.length == 0) {
  throw new IllegalArgumentException("Too few arguments");
 }
 int min = args[0];
 for(int i = 1; i <args.length; i++) {
  if(args[i] <min) {
   min = args[i];
  }
 }
return min;
}
```

* 문제점 발생..

  * 클라이언트가 인자 없이 호출이 가능함.

  * 즉, 컴파일 시점이 아니라 실행 도중에 오류가 난다.

  * args의 유효성 검사해야 하고, min을 Integer.MAX\_VALUE로 초기화 하지 않는한 for-each를 사용할 수도 없음. \(?\)

  * 보기 흉해,,



#### 0 이상이 아닌 1개 이상 필요할 때 - 좋은 예

```
static int min(int firstArg, int... ramaininArgs) {
  int min = firstArg;
  for(int arg : ramaininArgs) {
    if(arg <min) {
      min = arg;
    }
  }
  return min;
}
```

* 문제점 해결..

  * 메서드가 인자를 두 개 받도록 선언,,

  * 하나는 지정된 자료형을 갖는 일반 인자이나, 다른 하나는 같은 자료형의 varargs 인자



#### 배열을 마지막 인자로 받는 메서드 -&gt; varargs를 마지막 인자로 받는 메서드로

```
System.out.println(Arrays.asList("to","too", "two"));
```

* Java 1.5 이전에는 객체 참조 자료형만 동작하고, 기본 자료형값의 배열에 적용하면 컴파일도 안됨.

* Java 1.5 이후 varargs로 개선됨. 오류없이 컴파일 됨. List&lt;int\[\]&gt;같은 객체가 리턴되어 toString해도 주소값이 반환. 그래서 Arrays.toString 메서드가 구비됨

* 무조건 vargargs로 뜯어고치지 말고, 정말 임의의 갯수 인자를 처리 할 수 있는 메서드일 경우만 써라..

* 성능이 중요한 곳이라면, varargs 호출할 때 마다 배열이 만들어지고 초기화 과정에 대한 고려도 필요함.

```
public void foo() {};
public void foo(int a1) {};
public void foo(int a1, int a2) {};
public void foo(int a1, int a2, int a3) {};
public void foo(int a1, int a2, int a3, int... rest) {};
```

* 인자갯수가 3보다 클때 varargs를 사용하는 예제.

* 가령 95프로는 인자가 3건 이하이면, 이 방법을 사용하면 5프로 정도의 호출에 대해서만 새로운 배열이 만들어 짐

* EnumSet 클래스 정적 팩터리 메서드들은 이 기법으로 enum 집합 생성비용을 최소로 낮춘다.



#### 요약을 해보자

* varargs메서드는 인자 개수가 가변적인 메서드 정의할 때만 편리하지만, 남용은 곤란하다...

* 부적절하게 사용하면 혼란스럽기만 하다.

  


