## Item 33. ordinal을 배열 첨자로 사용하는 대신 EnumMap을 이용하라



* ordinal을 사용해 여러 자바 자료구조와 Enum형을 맵핑시키면 안된다

  * 형 안전성을 제공하지 못하고, index를 넘길 경우 ArrayIndexOutOfBoundsExceptoin을 만들 수 있기 때문

* java.util.EnumMap을 사용하여 enum 상수를 키로 사용하는 맵을 만들자

  ```
  public class Herb {
      public enum Type {
          ANNUAL, PERENNIAL, BIENNIAL
      }

      private final String name;
      private final Type type;

      Herb(String name, Type type) {
           this.name = name;
           this.type = type;
      }

       @Override
       public String toString() {
           return name;
       }


       public static void main(String[] args) {
           //이런 짓은 곤란합니다..
           Herb[] garden = { new Herb("Basil", Type.ANNUAL),
                          new Herb("Carroway", Type.BIENNIAL),
                           new Herb("Dill", Type.ANNUAL),
                           new Herb("Lavendar", Type.PERENNIAL),
                           new Herb("Parsley", Type.BIENNIAL),
                           new Herb("Rosemary", Type.PERENNIAL) };

           // Enum 형 Herb.Type 을 키로 사용!!
           Map<Type, Set<Herb>> herbsByType = new EnumMap<>(Herb.Type.class);
           System.out.println("herbsByType >> " + herbsByType);

           for (Herb.Type t : Herb.Type.values()){
               herbsByType.put(t, new HashSet<>());
               System.out.println("t >> " + t);
           }

           System.out.println(herbsByType);

           for (Herb h : garden){
               herbsByType.get(h.type).add(h);
               System.out.println("h.type >> " + h.type + ", " + h);
           }

           System.out.println(herbsByType);
       }
  }

  ```

* new EnumMap&lt;Type, Set&lt;Herb&gt;&gt;\(Herb.Type.class\)

  * 생성자에 키의 자료형을 나타내는 Class 객체를 인자로 받는다,,

  * Class객체를 한정적 자료형 토큰을 이용해서 실행 시점에 제네릭 자료형 정보를 제공

  * EnumMap 은 내부적으로**new Object\[length\]**형식으로 되어있다.

  * EnumSet은 static factory로 생성되어, 내부 적으로_e.getDeclaringClass\(\)_를 가져와서 타입정보를 가져오기 때문에 EnumMap처럼 생성할 때 타입 정보를 넘기지 않아도 된다.

* 표현해야 하는 단계가 다차원 적이라면**EnumMap&lt;… , EnumMap&lt;…&gt;&gt;**사용

* 즉, ordinal 값을 배열 첨자로 사용하는 것이 적절하지 않다는 것이다. EnumMap을 써라



