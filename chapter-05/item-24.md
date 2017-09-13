##Item 24. 무저검 경고(unchecked warning)를 제거하라
###제네릭프로그램시 컴파일러 경고 메시지 

###컴파일러 경고 메시지 
* 무점검 형변환 경고(unchecked cast warning)
* 무점검 메서드 호출 경고(unchecked method invocation warning)
* 무점검 변환 경고(unchecked conversion warning)

###컴파일러 경고 메시지 처리 방안
* 모든 무점검 경고는 가능하다면 없애야 한다. => 안정성 보장
* 제거할 수 없는 경고 메시지는 형 안전성이 확실할 때만 @SuperssWarnings('unchecked')을 사용해 억제하기 바란다
* @SuperssWarnings은 가능한 작은 범위에 적용하라
  ㄴ 변수 선언, 아주 짧은 메서드 또는 생성자 , 클래스 전체에 적용하지 않는다
* @SuperssWarnings('unchecked')를 사용할 때마다, 왜 형 안전성을 위반하지 않는지 밝히는 주석을 반드시 붙여라


```
// @SuppressWarnings의 적용 범위를 줄이기 위해 지역 변수 사용
public <T> T[] toArray(T[] a) {
  if ( a.length < size) {
     // 아래의 형변환은 배열의 자료형이 인자로 전달된 자료형인 T[]와
     // 같으므로 정확하다.
     @SuppressWarnings("unchecked") T[] result =  
         (T[])Arrays.copyOf(elements, size, a.getClass());
     return result;
  }
  System.arrayCopry(elements, 0, a, 0, size);
  if ( a.length > size ) {
     a[size] = null;
  }
  return a;
}
```