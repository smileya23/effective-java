## 규칙51. 문자열 연결 시 성능에 주의하라



문자열 연결 연산자 +는 문자열을 하나로 합치는 편리한 수단.

n개의 문자열에 연결 연산자를 반복 적용해서 연결하는데 드는 시간은 n^2에 비례한다.



문자열은 변경 불가능하기 때문\(규칙15\). 문자열 두 개를 연결할 때, 그 두 문자열의 내용은 전부 복사됨.

```
public String statement() {
  String result = "";
  for (int i = 0; i < numItems(); i++) {
    result += lineForItem(i); // String concatenation
  }
  return result;
}
```



만족스런 성능을 얻기 위해선, String 대신 StringBuilder를 써야된다.

StringBuilder는 내부적으로 충분한 크기의 StringBuilder객체를 미리 할당해두고 있음.

```
public String statement() {
  StringBuilder b = new StringBuilder(numItems() * LINE_WIDTH);
  for (int i = 0; i < numItems(); i++) {
    b.append(lineForItem(i));
  }
  return b.toString();
}
```



JDK1.5 부터는 String 을 사용하더라도 컴파일 시 내부적으로 StringBuilder로 변경되어 성능차이는 없어졌음!

  


