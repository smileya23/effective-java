## Item 44. 모든 API 요소에 문서화 주석을 달라

### 문서화 주석은 API 문서를 만드는 가장 효과적인 방법이다. 

* ##### 좋은 API 문서를 만들려면 API에 포함된 모든 클래스, 인터페이스, 생성자, 메서드, 그리고 필드 선언에 문서화 주석을 달아야 한다.
* #####  직렬화가 가능한 클래스라면 직렬화 형식도 밝혀야 한다.

#### **메서드주석**

메서드에 대한 문서화 주석은 메서드와 클라이언트 사이의 규약을 간명하게 설명해야 한다.\(무엇을 하는지 설명\)

* 선행조건은 클라이언트가 메서드를 호출하려면 반드시 참이 되어야 하는 조건들이다. 
* 후행조건은 메서드 실행이 성공적으로 끝난 다음에 만족되어야 하는 조건들이다. 
* 보통 선행조건은 무점검 예외\(unchecked exception\)에 대한 @throws 태그를 통해 암묵적으로 기술한다.

* 선행조건과 후행조건 외에도, 메서드는 부작용\(side effect\)에 대해서도 문서화 해야한다.
*  예를 들어 어떤 메서드가 후면 스레드\(background thread\)를 실행한다면 문서에는 그 사실이 명시되어야 한다.

```
/**
* Returns the element at the specified position in this list
*
* <p>This method is <i>not</i> guaranteed to run in constant time. In some implementations it may run in time proportional to the element position.
*
* @param index index of element to return; must be
* non-negative and less than the size of this list
* @return the element at the specified position in this list
* @throws IndexOutOfBoundsException if the index is out of range
*  ({@code index < 0 || index >= this.size()})
*/
E get(int indxe);
```

* 관습상 @param, @return, @throws 태그다음에오는구나절에는마침표를찍지않는다.

 @throws 절에 폼함된 코드 주변에 {@code} 태그가 사용된 것도 유의하자.

* 첫 번째는 해당 코드가 코드 서체로 표시되도록 하는 것이고 
* 두 번째는 그 안에 포함된 모든 HTML 마크업이나 Javadoc 태그가 위력을 발휘하지 못하도록 하는 것이다.

 {@literal} 태그를 사용하면 그 태그 안에 포함된 HTML 마크업이나 Javadoc 태그는 전부 단순 문자로 취급된다.

 {@code} 태그와 유사하지만, 코드 서체로 표시되지는 않는다는 차이가 있다.

모든 문서화 주석의 첫 번째 “문장”은, 해당 주석에 담긴 내용을 요약한 것이다.

*  메서드나 생성자의 경우, 요약문은 메서드가 무슨 일을 하는지 기술하는 완전한 동사구여야 한다.
*  클래스나 인터페이스의 요약문은 해당 클래스나 인터페이스로 만들어진 객체가 무엇을 나타내는지를 표현하는 명사구여야 한다.
*  필드의 요약문은 필드가 나타내는 것이 무엇인지를 설명하는 명사구여야 한다.
* 
#### **제네릭, enum,애노테이션문서화주석**

제네릭 자료형이나 메서드에 주석을 달 때는 모든 자료형 인자들을 설명해야 한다.



#### 애노테이션 자료형에 주석

* 모든 멤버에도 주석을 달아야한다.
* 멤버에는 마치 필드인 것처럼 명사구 주석을 달라. 
* 자료형 요약문에는 동사구를 써서, 언제 이 자료형을 애노테이션으로 붙어야 하는지 설명하라.



#### 문서화 주석의 오류 체크 \(javadoc 실행 결과로 만드어진 파일\) HTML 유효성 검사 도구\(validity checker\)로 검사 



