## Item 21. 전략을 표현하고 싶을 때는 함수 객체를 사용하라

#### 전략패턴(Strategy Pattern)
* 알고리즘군을 정의하고 각각을 캡슐화하여 교환해서 사용
  스트래티지를 활용하면 알고리즘을 사용하는 클라이언트와는 독립적으로 알고리즘을 변경가능

* Java에서 사용중인 전략패턴
  * java.util.Comparator#compare() called by Collections.sort()  
  * javax.servlet,http.HttpServlet#service
  * javax.servlet.Filter#doFilter
  
* Strategy Pattern vs Template Pattern



```
// 문자열 비교하는데 사용 가능
class StringLegthComparator{
   public int compare(String s1,String s2) {
      return s1.length() - s2.length();
   }
  };
```
#### 함수 객체(function object)
* 객체 참조를 통해 포인터와 비슷한 효과를 달성 가능.
객체의 메서드는 호출 대상 객체에 뭔가를 한다. 다른 객체에 작용하는 메서드 즉 인자로 전달된 객체에 뭔가를 하는 메서드를 정의하는 것도 가능하다.
가지고 있는 메서드가 그 메서드 하나 뿐인 객체는 해당 메서드의 포인터 구실을 한다. 

#### 실행 가능 전략(concrete stragety)




#### stateless class 

#### 싱글턴 패턴(singleton pattern)을 따르면 쓸데없는 객체 생성은 피할수 있다

```
public class StringLengthComparator {
    private StringLengthComparator() {}
    public static final StringLengthComparator INSTANCE
    = new StringLengthComparator();
    public int compare(String s1, String s2) {
        return s1.length() - s2.length();
    }
}

```


```
class Host{
    private static class StrLenCmp 
            implements Comparator<String>, Serializable{
        public int compare(String s1, String s2){
            return s1.length() - s2.length();
        }
    }

    public static final Comparator<String>
             STRING_LENGTH_COMPARATOR = new StrLenCmp();

    ... // 나머지 생략
}

```

### 함수 객체의 주된 용도는 전략 패턴을 구현하는 것이다. 
* 전략을 표현하는 인터페이스를 선언하고, 실행 가능 전략 클래스가 전부 해당 인터페이스를 구현하도록 해야 한다. 
* 실행 가능 전략이 한 번만 사용되는 경우에는 보통 그 전략을 익명 클래스 객체로 구현한다. 
* 반복적으로 사용된다면 private static 멤버 클래스로 전략을 표현한 다음(내부 중첩 클래스),
  전략 인터페이스가 자료형인 public static final 필드를 통해 외부에 공개하는 것이 바람직하다.




참조)) 
- https://m.blog.naver.com/PostView.nhn?blogId=2feelus&logNo=220658663151&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F
- http://kimsunzun.tistory.com/entry/Strategy%EC%A0%84%EB%9E%B5-%ED%8C%A8%ED%84%B4
- https://stackoverflow.com/questions/669271/what-is-the-difference-between-the-template-method-and-the-strategy-patterns
