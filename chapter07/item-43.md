## Item 43. null 대신 빈 배열이나 컬렉션을 반환하라

### 왜?
* 클라이언트에서 null체크를 해야함. 
    - 빼먹을 시 에러가 발생할 확률이 높고, 에러가 발생하지 않으면 쉽게 찾아내기 어려움 
* 프로바이딩 하는 입장에서도 null 반환하게 구현하는 것이 더 까다로움

### 반론
* 빈 배열/컬렉션 할당에 따른 비용 
    1. 성능을 위해, 적합한 구조를 포기하는 것은 안됨 
    2. 빈 배열/컬렉션은 재사용이 가능한 immutable 객체 
        - ex. Collections.toArray(T[]) 
            + toArray의 파라미터는 return type을 명시 
                ```
                List<String> test = new ArrayList<>();
                String[] testArray = (String[]) test.toArray();
                String[] testArray2 = test.toArray(new String[]);
                ```
            + 컬렉션이 비어있을 때나 파라미터 배열이 컬렉션의 모든 원소를 담을만큼 충분 할 때에는 파라미터 배열을 사용.
            + -> 배열 길이가 0이면 할당에 따른 시간적, 공간적 비용이 들지 않음
        - ex. Colections.emptyList (emptySet/Map)
            + Implementations of this method need not create a separate List object for each call.
            ```
            public static final <T> List<T> emptyList() {
                return (List<T>) EMPTY_LIST;
            }
            ```
