## 규칙 56. 일반적으로 통용되는 작명 관습을 따르라 

### terms
- 작명 관습 : naming convention
    + 철자(typographical) 
    + 문법(grammatical)

### typographical convention
- package
    + 마침표를 구분점으로 사용하여 계층을 표현 
    + 알파벳 소문자 사용. 숫자는 거의 사용하지 않음 
    + 도메인 이름 + 8자 이하의 단어/약어 컴포넌트들 
    + ex) org.joda.time.format
    + 예외) 표준 라이브러리와 옵션 패키지 : java.* javax.*
- class/interface/enum
    + 하나 이상의 단어. 첫 글자는 대문자
    + 두문자, 널리 쓰이는 약어만 사용 가능.
        * 두문자의 경우 전체 대문자로 써야하냐는 취향차이...
        * HTTPURL vs HttpUrl
- method/field
    + 위(class/interface)와 동일. 대신 첫 글자는 소문자 
    + 예외) 상수 필드는 하나 이상의 대문자 단어. 단어와 단어 사이는 _ 사용 
- variable
    + field와 동일. 약어 허용 
- type parameter
    + 하나의 대문자 
    + ex) T,U,V/T1,T2(임의 자료형), E(컬렉션 자료형), K,V(키,밸류)
- typographical convention의 경우 그 양이 얼마 많지 않아 철저하게 지키는 것이 좋음 

### grammatical convention
- package
    + 문법적인 요소는 없어 문제가 되지 않음 (단어의 나열이기 때문에...)
- class/interface/enum
    + 명사, 명사구
        * ex) Timer, BufferedWriter, Collection
    + (able, ible이 붙은) 형용사
        * ex) Runnable, Observable
- method
    + 동작을 수행할 시 동사/동사구 
        * ex) append, drawImage
    + boolean을 return하는 메서드의 경우, is/has로 시작 
        * ex) isEmpty, isEnabled
    + 그 외 return하는 메서드의 경우, 명사/명사구/get 으로 시작 
    + 객체의 자료형 변환 : to{Type}
        * ex) toString, toArray
    + 인자로 받은 객체의 다른 자료형의 뷰객체를 반환 : as{Type}
        * ex) asList
- field/variable
    + boolean 타입일 때, 메서드와 같은 이름을 붙이되 is/has는 생략 
        * ex) initialized
    + 크게 중요하진 않음...

