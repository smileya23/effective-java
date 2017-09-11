## Item 21. 전략을 표현하고 싶을 때는 함수 객체를 사용하라

## 전략패턴(Strategy Pattern)


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

## 함수 객체의 주된 용도는 전략 패턴을 구현하는 것이다. 
* 전략을 표현하는 인터페이스를 선언하고, 실행 가능 전략 클래스가 전부 해당 인터페이스를 구현하도록 해야 한다. 
* 실행 가능 전략이 한 번만 사용되는 경우에는 보통 그 전략을 익명 클래스 객체로 구현한다. 
* 반복적으로 사용된다면 private static 멤버 클래스로 전략을 표현한 다음(내부 중첩 클래스),
  전략 인터페이스가 자료형인 public static final 필드를 통해 외부에 공개하는 것이 바람직하다.






