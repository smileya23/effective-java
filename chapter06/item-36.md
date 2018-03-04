## Item 36. 어노테이션은 일관되게 사용하라

* 어노테이션을 일관되게 사용하면 끔찍한 human error로 인한 버그들을 방지 할 수 있다.

* 대부분의 프로그래머에게 가장 중요하게 쓰이는 것은 Override이다.

  * 메서드 선언부 사용, 상위 자료형에 정의된 메서드를 재정의한다는 사실 표현

  * 이 어노테이션 일관되게 사용하면 끔찍한 버그를 방지

* 알파벳 두개로 구성된 Bigram 객체 26개를 반복해서 집합에 넣는 작업

  ```
  public class Bigram {
      private final char first;
      private final char second;

      public Bigram(char first, char second) {
          this.first = first;
          this.second = second;
      }

      public boolean equals(Bigram b){
          return b.first == first && b.second == second;
      }

  //    @Override
  //    public boolean equals(Object o) {
  //        if(!(o instanceof  Bigram)) return false;
  //        Bigram b = (Bigram) o;
  //        return  b.first == first && b.second == second;
  //    }

      public int hashCode() {
           return 31 * first + second;
      }

       public static void main(String[] args) {
           Set<Bigram> s = new HashSet<>();
           for (int i = 0; i < 10; i++)
                   for (char ch = 'a'; ch <= 'z'; ch++){
                       s.add(new Bigram(ch, ch));
                   }
           System.out.println(s.size());
       }
   }
  ```

  * 26 번 반복이라 정상적이라면 26이 출력이지만, 모든 오브젝트가 다르게 인식되어 260개가 출력됨 \(Set은 중복 허용하지 않음\)

  * equals\(Object b\)가 아닌 equals\(Bigram b\)로 잘못 Override 함.

  * equals 메소드를 super class인 Object class로 부터 재정의하지 못하고 Overloading 됨.

  * 즉 해당 class는 Object의 equals가 계승 됨. Object의 equals은 둘이 같은 객체인지만 비교함. \(같은 객체의 참조인지\)

  * _@Override_주석을 equals 함수에 넣어주면 컴파일 에러 발생해서 실수 방지 가능 함

* 따라서, 상위 클래스에 선언된 메서드를 재정의할 때는 반드시 선언부에 Override 어노테이션을 붙여야 한다.

* 하지만 예외는 있는데, abstract 클래스에서 abstract 메소드를 재정의 할 때는 안 붙여도 된다. abstract 메소드는 재정의하지 않으면 컴파일러가 오류를 내주기 때문이지.

* 어노테이션 검사 기능을 갖춘 최신 IDE를 잘 활용하는 것도 방법.



