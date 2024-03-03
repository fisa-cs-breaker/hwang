# [2회차] Collection & Generic

생성일: 2024년 2월 25일 오후 10:11

# [2회차] Collection & Generic

stream 공부하려다보니 collection을 아는게 먼저더라 또 그러다보니 제네릭도 알아야하고

---

---

# Collection

## 컬렉션이란?

> 많은 데이터 요소를 효율적으로 관리하기 위한 자료구조
> 
- 많은 수의 데이터를 그 사용 목적에 적합한 자료구조로 묶어 하나로 그룹화한 객체

![Untitled](%5B2%E1%84%92%E1%85%AC%E1%84%8E%E1%85%A1%5D%20Collection%20&%20Generic%204b11cc7c7471416ebc3b591709d3e461/Untitled.png)

### 왜 Collection을 쓸까

배열도 있잖아?

배열과의 차이점은 정적 메모리 할당이 아닌 동적 메모리 할당을 한다는 것

⇒ 공간 효율성 높여줌

그리고 다수의 데이터를 다루는데 표준화된 클래스들을 제공해줘서 자료구조를 직접 구현하지 않고 편하게 사용할 수 있기 때문에

### 컬렉션 종류

- List
    - 데이터 순서에 따라 관리
    - 중복O, 순서O
    - ArrayList, LinkedList, Vector
- Set
    - 중복 허용되지 않는 데이터 관리
    - 중복X, 순서X
    - HashSet, TreeSet, LinkedHashSet
- Map
    - Key, Value로 짝을 이루어 관리
    - Key값은 중복 허용안함
    - 순서X
    - Hashtable, HashMap, TreeMap
- Queue
    - 데이터 인아웃을 FIFO 방식으로 관리
- Stack
    - 데이터 인아웃을 LIFO 방식으로 관리

### 컬렉션 특징

컬렉션이 데이터를 다룰 때 데이터는 기본적으로 **객체**만 가능

그래서 char, int, float와 같은 기본형을 사용할 수 없고 대신 Wrapper 클래스 사용. 

- 오토 박싱과 오토 언박싱으로 사용자가 기본형 다룰 수 있는 것처럼 사용할 수 있음.

컬렉션과 관련된 인터페이스나 클래스가 정의된 코드를 보면 제네릭 형태로 구현되엉 있으므로 사용자는 원하는 데이터 타입을 지정해서 사용할 수 있음.

```java
public interface List<E> extends Collection<E> {   ...   }
```

### 컬렉션 인터페이스의 주요 메소드

List, Set, Queue는 Collection 인터페이스를 상속받고 있으며 이 인터페이스가 가지는 주요 메소드들

- `boolean add(E e)` : 현재 컬렉션에 데이터 객체 e를 추가
- `boolean addAll(Collection<? extends E> c`) : 현재 컬렉션에 컬렉션 c의 모든 데이터를 추가
- `boolean contains(Object o)` : 현재 컬렉션에 객체 o의 포함 여부를 반환
- `boolean containsAll(Collection<?> c`) : 현재 컬렉션에 컬렉션 c의 모든 데이터가 포함되어있는지 여부를 반환
- `boolean remove(Object o)` : 현재 컬렉션에서 객체 o를 삭제
- `boolean removeAll(Collection<?> c)` : 현재 컬렉션에서 컬렉션 c와 일치하는 데이터를 삭제
- `boolean retainAll(Collection<?> c)` : 현재 컬렉션에서 컬렉션 c와 일치하는 데이터만 남기고 나머지는 삭제
- `void clear( )` : 현재 컬렉션의 모든 데이터를 삭제
- `int size( )` : 현재 컬렉션에 포함된 데이터 개수를 반환
- `boolean isEmpty( )` : 현재 컬렉션이 비어있는지 여부를 반환
- `Iterator<E> iterator( )` : 현재 컬렉션의 모든 요소에 대한 iterator를 반환
- `Object[ ] toArray( )` : 현재 컬렉션에 저장된 데이터를 Object 배열로 반환
- `<T> T[ ] toArray(T[ ] a )` : 현재 컬렉션에 저장된 데이터를 배열 a에 담고 배열 a를  반환

# Generic

## 제네릭은 뭘까

> 한 번의 정의로 여러 종류의 데이터 타입을 다룰 수 있도록 하는 방법

클래스 외부에서 사용자에 의해 지정되는 것
> 

예를 들어 ArrayList<E> 형태로 클래스가 정의되어 있다면, 사용자는 데이터로 String을 사용할 때는 ArrayList<String>으로, Integer를 사용할 때는ArrayList<Integer>로 지정하여 사용할 수 있음

```java
public class ArrayList<E> extends AbstractList<E> 
		implements List<E>, RandomAccess, Cloneable, java.io.Serializable{   
  ...
}

//사용예시1
ArrayList<String> a1 = new ArrayList<String>( );
//사용예시2 
ArrayList<Integer> a2 = new ArrayList<Integer>( );
```

### 제네릭의 장점

- 잘못된 타입이 들어올 수 있는 것을 컴파일 단계에서 방지할 수 있음
- 클래스 외부에서 타입을 지정해주므로 따로 타입을 체크하거나 변환해줄 필요가 없음. 즉, 관리하기가 편해짐
- 비슷한 기능을 지원한다면 코드의 재사용성이 높아짐

### 제네릭 사용 방법

| 타입 | 설명 |
| --- | --- |
| <T> | Type |
| <E> | Element |
| <K> | Key |
| <V> | Value |
| <N> | Number |
1. **클래스 및 인터페이스 선언**

```java
public class ClassName <T> { ... }
public interface InterfaceName <T> { ... }
```

T 타입은 해당 블럭 {…} 안에서까지 유효함.

```java
public class ClassName <T, K> { ... }
public interface InterfaceName <T, K> { ... }

public class Main {
	public static void main(String[] args) {
		ClassName<String, Integer> a = new ClassName<String, Integer>();
	}
}
```

이렇게 제네릭 타입을 두 개로 둘 수도 있음

위 예시대로라면 T는 String이 되고, K는 Integer가 됨

<aside>
💡 이 때 주의해야 할 점은 타입 파라미터로 명시할 수 있는 것은 참조 타입(Reference Type)밖에 올 수 없다.

즉, int, double, char 같은 primitive type은 올 수 없다는 것. 그래서 int형 double형 등 primitive Type의 경우 Integer, Double 같은 Wrapper Type으로 씀

또한 바꿔 말하면 참조 타입이 올 수 있다는 것은 사용자가 정의한 클래스도 타입으로 올 수 있다는 것

</aside>

1. **제네릭 클래스**

```java
// 제네릭 클래스 
class ClassName<K, V> {	
	private K first;	// K 타입(제네릭)
	private V second;	// V 타입(제네릭) 
	
	void set(K first, V second) {
		this.first = first;
		this.second = second;
	}
	
	K getFirst() {
		return first;
	}
	
	V getSecond() {
		return second;
	}
}
 
// 메인 클래스 
class Main {
	public static void main(String[] args) {
		
		ClassName<String, Integer> a = new ClassName<String, Integer>();
		
		a.set("10", 10);
 
 
		System.out.println("  fisrt data : " + a.getFirst());
		// 반환된 변수의 타입 출력 
		System.out.println("  K Type : " + a.getFirst().getClass().getName());
		
		System.out.println("  second data : " + a.getSecond());
		// 반환된 변수의 타입 출력 
		System.out.println("  V Type : " + a.getSecond().getClass().getName());
	}
}
```

1. **제네릭 메소드**

```java
public <T> T genericMethod(T o) {	// 제네릭 메소드
		...
}
 
[접근 제어자] <제네릭타입> [반환타입] [메소드명]([제네릭타입] [파라미터]) {
	// 텍스트
}
```

클래스와는 다르게 반환타입 이전에 <> 제네릭 타입을 선언함

클래스에서 지정한 제네릭유형과 별도로 메소드에서 독립적으로 제네릭 유형을 선언하여 쓸 수 있음

**제네릭이 사용되는 메소드를 정적메소드로 두고 싶은 경우 제네릭 클래스와 별도로 독립적인 제네릭이 사용되어야 한다.**