## 규칙 48. 객체화 된 기본 자료형 대신 기본 자료형을 이용하라 

### terms
- primitive type : int, double, boolean
- reference type : String, List 
- boxed primitve type : 각 primitve type에 대응되는 reference type
    + ex) int - Integer, dobule - Double, boolean - Boolean 
    + autoboxing - auto-unboxing

### primitive type vs boxed primitive type
1. boxed primitive type는 값 이외에 identity를 가짐
    - 같은 값을 갖더라도 신원이 다를 수 있기 때문에, boxed primitive type에서 `==` 연산자 사용은 항상 오류 
    ```
    Comparator<Integer> naturalOrder = new Comparator<Integer>() {
        public int compare(Integer first, Integer second) {
            return first < second ? -1 : (first == second ? 0 : 1);
        }
    };

    Comparator<Integer> naturalOrder = new Comparator<Integer>() {
       public int compare(Integer first, Integer second) {
           int f = first;   // Auto-unboxing
           int s = second;  // Auto-unboxing
           return f < s ? -1 : (f == s ? 0 : 1); // No unboxing
        } 
    };
    ```
2. boxed primitive type의 값에는 아무 기능도 없는 값(null)이 있음  
    - 즉, primitive type의 값에 대응되지 않는 value가 있음 
    - boxed primitive type의 초기값은 null 
    ```
    // NullPointerException 발생 
    public class Unbelievable {
       static Integer i;
       public static void main(String[] args) {
           if (i == 42)
               System.out.println("Unbelievable");
        } 
    }
    ```
3. boxed primitive type 사용시, 시간적/공간적 비용이 더 생김 
    - primitive type과 boxed primitive type이 한 연산에 있을 경우, boxed primitve type은 auto unboxing됨 
    ```
    public static void main(String[] args) {
        Long sum = 0L;
        for (long i = 0; i < Integer.MAX_VALUE; i++) {
           sum += i;
        }
       System.out.println(sum);    
    }
    ```

### boxed primitve type을 사용해야 할 경우 
- 컬렉션 내에서 사용할 때 
- 형인자 자료형에서 사용할 때 
- 리플렉션을 사용헤 메서드 호출 시, 파라미터 type 선언할 때 

