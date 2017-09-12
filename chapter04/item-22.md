**_****_**## Item 22. 멤버 클래스는 가능하면 static으로 선언하라

#### 

#### 중첩 클래스\(nested class\) 

다른 클래스 안에 정의된 클래스로 해당 클래스가 속한 클래스 안에서만 사용됨 

* 정적 멤버 클래스\(static member class\) - 중첩클래스
  * 바깥 클래스의 모든 멤버\(private 까지도\) 접근 가능
  * 바깥 클래스와 함께 사용할 때만 유용한 public 도움 클래스\(hel**_~~






$$

$$



$$

$$



```

```



```

```



```

```



```

```


~~_**per class\)를 정의
* 비-정적 멤버 클래스\(nonstatic member class\) - 내부클래스
  * 바깥 클래스 객체에 접근할 필요가 없는 멤버 클래스를 정의할 때는, 항상 선언문 앞에 static을 붙여 비=정적 멤버 클래스 대신 정적 멤버 클래스로 만들자
  * 
  * ```
    public class Myset<E> extends AbstractSet<E> {
        public Iterator<E> iterator() {
            return new MyIterator();
        }


        private class MyIteraotr implements Iteraotr<E> {
            .....
        }
    }
    ```

* 익명 클래스\(anonymous class\) - 내부클래스
  * 클레으 이름이 없다. 
  * 바깥 클래스의 멤버도 아니다.
  * 멤버로 선언하지도 않으며, 사용하는 순간에 선언하고 객체를 만든다.
  * 함수 객체를 정의할 때 널리 쓰임
* 지역 클래스\(local class\) - 내부클래스
  * 4가지중 가장 사용빈도가 낮다.
  * 지역변수\(local variable\)가 선언될 수 있는 곳 어디서든 선언 가능.
  * 지역변수와 동일한 유효범위 규칙\(scoping rule\)을 따름
  * 



