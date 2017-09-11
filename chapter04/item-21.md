## Item 21. 전략을 표현하고 싶을 때는 함수 객체를 사용하라

#### 전략패턴(Strategy Pattern)
* 일반적으로는 객체의 메소드에 대한 알고리즘이 미리 정해져있고, 정해진 알고리즘에 따라 어플리케이션 수행중에 그 메소드가 불리면 정해진 알고리즘이 불리게 된다. 
  전략패턴에서는 이 알고리즘이 여러개가 설정되어있고, 수행중에 이 알고리즘중에 하나가 선택되어 가변적으로 수행하는 방식이다.
  
* Java에서 사용중인 전략패턴
  * java.util.Comparator#compare() called by Collections.sort()
    컬렉션 객체들에 대해서 sort를 할때는 객체의 어떤 값을 기준으로 객체간의 비교를 정할지를 정하는 알고리즘이 필요하다.
    미리 전략을 구성하고, 구성된 전략을 감싸주는 Comparator 인터페이스안에 로직을 넣어 감싸준뒤, 
    실제 대상 함수인 sort에 배열객체와 함께 알고리즘이 포함된 Comparator 객체를 넘겨주면,
    실제 sort함수가 불릴때는 comparator 알고리즘에 따라 정렬을 수행한다.
 
  * javax.servlet,http.HttpServlet#service
    톰캣은 하나의 jvm인스턴스에서 돌아간다. 톰캣위에서는 여러개의 웹서비스가 동시에 동작하고 있다. 
    url request가 오면 port와 url context에  따라 거기에 맞는 각각의 웹서비스의 service()함수가 불리게 된다. 
    이때 service함수는 유저가 직접 작성한 것이 수행되게 된다. 여기서 service가 전략이 된다.
    톰캣입장에서 볼때는 전략 패턴의 방식으로 url을 처리하는 것이다.

  * javax.servlet.Filter#doFilter
   url에 대해 유저가 처리하는 함수 service()를 부르기전, 유저가 주입한 filter에 따라서 url이 처리될수도 있고, 거절될수도 있고,
    특정 page로 direct 될수도 있다. 이것또한 tomcat입장에서 볼때 전략패턴의 방식으로 url을 필터링하는 것이다.

* Strategy Pattern vs Template Pattern

#### 함수 객체(function object)

#### 실행 가능 전략(concrete stragety)

```
// 문자열 비교하는데 사용 가능
class StringLegthComparator{
   public int compare(String s1,String s2) {
      return s1.length() - s2.length();
   }
  };
```


#### stateless class 

#### 싱글턴 패턴(singleton pattern)

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
