## Item 06. 유효기간이 지난 객체 참조는 폐기하라 
쓰레기 수집기...........
#### memory leak이 발생하는 상황
* 직접 메모리를 관리할 때
    + ex. stack
    + 객체에 대한 만기참조(obsolete reference)를 제거하지 않음
    + 해결하려면? 안쓰는 객체 참조는 null로 만든다 
        - 더이상 사용하지 않는 참조에 접근하면 바로 NullPointerException이 발생하기 때문에 오류 잡기에도 좋음 
        - 근데 이건 규범이 아니라 예외처리야 ^^ (이해가 안됨)
        - 변수를 정의할 때 유효범위를 최대한 좁게 만들어라 (규칙 45)
* 캐시(cache)
    + 해결 1. WeakHashMap으로 캐시 구현 
        - WeakHashMap ? 쉽게 생각하면 외부에서 참조가 존재하는 key-value 엔트리만 갖고 있는 HashMap. 참조가 없다면 바로 삭제된다. (WeakReference가 어쩌고 하는데...)
    + 해결 2. background thread 
        - 사용 안하는 항목을 찾고 지움
    + 해결 3. LinkedHashMap으로 캐시 구현 
        - 새로운 항목이 추가될 때, 사용 안하는 항목을 지움 
        - removeEldestEntry method가 제공됨  
* callback
    + 해결법 : callback은 weak reference만 저장하라 

#### java gc에 대해 더 읽어볼만한 글들 
* [Java Garbage Collection](http://d2.naver.com/helloworld/1329)
* [Java Reference와 GC](http://d2.naver.com/helloworld/329631)