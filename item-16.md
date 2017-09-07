## Item 16. 계승하는 대신 구성하라

interface를 계승할 때는 해당하지 않는 내용이다.

#### 계승은 캡슐화\(encapsulation\) 원칙을 위반한다.

* 하위 클래스가 정상 동작하기 위해서는 상위 클래스에 의존할 수 밖에 없다.
* 상위 클래스의 구현이 릴리스\(release\)가 거듭되면서 바뀔 수 있는데, 그러다 보면 하위 클래스 코드는 수정된 적이 없어도 망가질 수 있다. 



#### 망가질 수 있는, 깨질 수 있는 클래스

```
	// The number of attempted element insertions
	private int addCount = 0;
    	{ 생성자 생략 .. }
	@Override
	public boolean add(E e) {
		addCount++;
		return super.add(e);
	}

	@Override
	public boolean addAll(Collection<? extends E> c) {
		addCount += c.size();
		return super.addAll(c);
	}

	public int getAddCount() {
		return addCount;
	}
}
```

```
InstrumentedHashSet<String> s = new InstrumentedHashSet<String>();
s.addAll(Arrays.asList("Snap", "Crackle", "Pop"));
```



