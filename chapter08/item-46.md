## Item 46. for문보다는 for-each문을 사용하라 

* for-each
    - js의 for...in + for...of
    ```
      // array
      for (int i = 0; i < a.length; i++) {
        doSomething(a[i]);
      }
      
      // collection
      for (Iterator i = c.iterator(); i.hasNext(); ) { 
        doSomething((Element) i.next());
      }
      
      for (Element e : elements) {
        doSomething(e);
      }
    ```
  - Iterable 인터페이스를 구현하는 객체면 사용 가능
    ```
        public interface Iterable<E> {
            // Returns an iterator over the elements in this iterable
            Iterator<E> iterator();
        }
    ```

* 장점
    - index variable(4번), iterator(3번) 등장 → 다른 변수를 이용하는 실수가 있어도 컴파일 단계에서 잡기 어려움 → 이를 보완
    - ex. 중첩 for 문
    ```
        enum Suit { CLUB, DIAMOND, HEART, SPADE }
        enum Rank { ACE, DEUCE, THREE, FOUR, FIVE, SIX, SEVEN, EIGHT,
        NINE, TEN, JACK, QUEEN, KING }
        ...
        Collection<Suit> suits = Arrays.asList(Suit.values());
        Collection<Rank> ranks = Arrays.asList(Rank.values());
        
        List<Card> deck = new ArrayList<Card>();
        for (Iterator<Suit> i = suits.iterator(); i.hasNext(); )
            for (Iterator<Rank> j = ranks.iterator(); j.hasNext(); )
                deck.add(new Card(i.next(), j.next()));
        
        
        for (Iterator<Suit> i = suits.iterator(); i.hasNext(); ) {
            Suit suit = i.next();
            for (Iterator<Rank> j = ranks.iterator(); j.hasNext(); )
                deck.add(new Card(suit, j.next()));
            }
        }
        
        for (Suit suit : suits)
            for (Rank rank : ranks)
                deck.add(new Card(suit, rank));
    ```
* 사용할 수 없는 경우
    - filtering
        + 단순 순회가 아닌 remove 필요
        + interator.remove() 를 호출해야함
    - transforming
        + 원소의 수정이 있을 경우  
    - parallel iteration
        + 여러 컬렉션을 병렬적으로 순회 
    - 즉,,, 단순 순회가 아닐경우(컬렉션/배열 내 원소의 변경이 있거나 특수한 규칙에 의해 순회) 사용할 수 없음!
