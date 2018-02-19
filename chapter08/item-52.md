## 규칙 52. 객체를 참조할 때는 그 인터페이스를 사용하라 

[규칙 40](https://byjo.gitbooks.io/effective-java/content/chapter07/item-40.html) 에서, 메서드의 파라미터로 클래스보단 인터페이스를 사용하라 했는데, 이와 비슷한 이야기임 

```
List<String> test = new ArrayList<>();
```

### 장점
- 프로그램 유연성 
  + 구현의 성능이 더 좋거나, 필요한 추가 기능이 있을 때 쉽게 변겨아 가능 

### 주의할 점 
- 다른 구현으로 바꿀 때, 기존에 사용하던 구현을 커버가능한지 확인이 필요 
- 참조할 적절한 인터페이스가 없을 땐, 클래스 계층 내에서 가장 일반적인 클래스를 참조 
  + ex. value class, class-based framework에 속한 class 

