# 컬렉션 프레임웤(Collection Framework)
## 1.1 컬렉션 프레임웤(Collection Framework)
```컬렉션 프레임웤(Collections Framework)```이란 **데이터 군을 저장하는 클래스들을 표준화한 설계**를 뜻한다.

```컬렉션(Collection)```은 다수의 데이터 - 즉 데이터 그룹을, 프레임웤은 표준화된 프로그래밍 방식을 의미한다.

Java API 문서에는 컬렉션 프레임웤을 **데이터 군을 다루고 표현하기 위한 단일화된 구조(architecture)**라고 정의하고 있다.

## 1.2 컬렉션 프레임웤의 핵심 인터페이스
컬렉션 프레임웤에서는 컬렉션데이터 그룹을 크게 3가지 타입으로 분류했다.
이렇게 분류한 3가지 타입 중 ```List```와 ```Set```의 공통 부분을 추상화하여 ```Collection``` 인터페이스를 추가로 정의했다.

Map은 나머지 두 개와는 전혀 다른 형태로 컬렉션을 다루기 때문에 상속계층도에 포함되지 못했다.

이러한 설계는 객체지향언어의 장점을 극명히 보여준다.
실제로 컬렉션 프레임웤의 실제 소스를 분석해보면 객체지향적인 설계능력을 향상시킬 수 있을 것이다.

컬렉션 프레임웤의 종류는 다음과 같다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184796728-73c52da6-f603-498c-8598-aa431d63dbab.png)
<br>

### Collection 인터페이스
![image](https://user-images.githubusercontent.com/62749021/184796777-fee771cf-5a2c-441a-908b-53469eaa91a6.png)
<br>

### List 인터페이스
```List``` 인터페이스는 중복을 허용하면서 저장순서가 유지되는 컬렉션을 구현하는데 사용한다. ```List``` 인터페이스에서 ```Collection``` 인터페이스와 중복되지 않는, 정의된 메소드는 다음과 같다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184797027-d19dc28d-3125-456c-9261-2d869f912f8e.png)
<br>

### Set 인터페이스
```Set``` 인터페이스는 중복을 허용하지 않고 저장순서가 유지되지 않는 컬렉션 클래스를 구현하는데 사용한다.

### Map 인터페이스
```Map``` 인터페이스는 ```키(key)```와 ```값(value)```을 하나의 쌍으로 묶어서 저장하는 컬렉션 클래스를 구현하는데 사용된다.
키는 중복될 수 없지만 값은 중복을 허용한다.
기존에 저장된 데이터와 중복된 키와 값을 저장하면 기존의 값은 없어지고 마지막에 저장된 값이 남게된다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184797140-fd720a7f-99af-4320-866b-638406a018dd.png)
<br>
```values()```에서는 반환타입이 ```Collection```이고, ```KeySet()```에서는 반환타입이 Set이다.
```Map``` 인터페이스에서는 값(value)의 중복을 허용하므로 ```values()```는 반환타입이 ```Collection```이고, 키(key)는 중복을 허용하지 않기 때문에 ```Set``` 타입으로 변환한 것이다.

### Map.Entry 인터페이스
```Map.Entry``` 인터페이스는 ```Map``` 인터페이스의 내부 인터페이스이다.
내부 클래스와 같이 인터페이스도 인터페이스 안에 인터페이스를 정의하는 ```내부 인터페이스(inner interface)```를 정의하는 것이 가능하다.

```Map```에 저장되는 ```key-value```쌍을 다루기 위해 내부적으로 ```Entry``` 인터페이스를 정의해놓았다.
이것은 보다 객체지향적으로 설계하도록 유도하기 위한 것으로 ```Map``` 인터페이스를 구현하는 클래스에서는 ```Map.Entity``` 인터페이스도 함께 구현해야한다.
```java
public interface Map{
    ...
    interface Entry    {
        Object getKey();
        Object getValue();
        Object setValue(Object value);
        boolean equals(Object o);
        int hashCode();
        ... // JDK 8.0부터 추가된 메소드는 생략
    }
}
```
![image](https://user-images.githubusercontent.com/62749021/184797355-4e6b5f07-d64e-4a7f-bf7e-e7df96f133b9.png)
<br>

## 1.3 ArrayList
```ArrayList```는 컬렉션 프레임웤에서 제일 많이 사용되는 컬렉션 클래스이다.

```ArrayList```는 ```List``` 인터페이스를 구현하기 때문에 데이터의 저장순서가 유지되고 중복을 허용한다.

```ArrayList```는 기존의 ```Vector```를 개선한 것으로 Vector의 구현원리와 기능적인 측면에서 동일하다고 볼 수 있다.

그러므로 호환성을 제외한다면 ```ArrayList```를 선호한다.

```ArrayList```는 ```Object``` 배열을 이용해서 데이터를 순차적으로 저장된다.

배열에 더 이상 저장할 공간이 없으면 보다 큰 배열을 새롭게 생성하여 기존의 배열에 저장된 내용을 새로운 배열로 복사한 다음 저장한다.

선언된 배열의 타입이 모든 객체의 최고 조상인 ```Object```이기 때문에 ```ArrayList```는 모든 종류의 객체를 담을 수 있다.

![image](https://user-images.githubusercontent.com/62749021/184799502-2f4a89ef-c9fa-4dce-a740-4b9a653ef09c.png)
<br>
```java
import java.util.*;

class ArrayListEx1{
	public static void main(String[] args) {
		ArrayList list1 = new ArrayList(10);
		list1.add(new Integer(5));
		list1.add(new Integer(4));
		list1.add(new Integer(2));
		list1.add(new Integer(0));
		list1.add(new Integer(1));
		list1.add(new Integer(3));

		ArrayList list2 = new ArrayList(list1.subList(1,4)); 
		print(list1, list2);

		Collections.sort(list1);	// list1과 list2를 정렬한다.
		Collections.sort(list2);	// Collections.sort(List l)
		print(list1, list2);

		System.out.println("list1.containsAll(list2):" + list1.containsAll(list2));

		list2.add("B");
		list2.add("C");
		list2.add(3, "A");
		print(list1, list2);

		list2.set(3, "AA");
		print(list1, list2);
		
		// list1에서 list2와 겹치는 부분만 남기고 나머지는 삭제한다.
		System.out.println("list1.retainAll(list2):" + list1.retainAll(list2));	
		print(list1, list2);
		
		//  list2에서 list1에 포함된 객체들을 삭제한다.
		for(int i= list2.size()-1; i >= 0; i--) {
			if(list1.contains(list2.get(i)))
				list2.remove(i);
		}
		print(list1, list2);
	} // main의 끝

	static void print(ArrayList list1, ArrayList list2) {
		System.out.println("list1:"+list1);
		System.out.println("list2:"+list2);
		System.out.println();		
	}
} // class
```
위 예제는 ArrayList의 기본적인 메소드를 다루는 방법을 보여주기 위한 예제이며, 실행 결과는 다음과 같다.
```
list1:[5, 4, 2, 0, 1, 3]
list2:[4, 2, 0]

list1:[0, 1, 2, 3, 4, 5]
list2:[0, 2, 4]

list1.containsAll(list2):true
list1:[0, 1, 2, 3, 4, 5]
list2:[0, 2, 4, A, B, C]

list1:[0, 1, 2, 3, 4, 5]
list2:[0, 2, 4, AA, B, C]

list1.retainAll(list2):true
list1:[0, 2, 4]
list2:[0, 2, 4, AA, B, C]

list1:[0, 2, 4]
list2:[AA, B, C]
```

## 1.14 LinkedList
배열은 가장 기본적인 형태의 잘구조로 구조가 간단하며 사용하기 쉽고 데이터를 읽어오는데 걸리는 시간(접근 시간, access time)이 가장 빠르다는 장점을 가지고 있다.

배열의 단점은 다음과 같다.
```
1. 크기를 변경할 수 없다.
  크기를 변경할 수 없으므로 새로운 배열을 생성해서 데이터를 복사해야한다.
  실행속도를 향상시키기 위해서는 충분히 큰 크기의 배열을 생성해야 하므로 메모리가 낭비된다.
2. 비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다.
  차례대로 데이터를 추가하고 마지막에서부터 데이터를 삭제하는 것은 빠르지만, 배열의 중간에 데이터를 추가하려면 빈자리를 만들기 위해 다른 데이터들을 복사해서 이동해야 한다.
```
이러한 배열의 단점을 보완하기 위한 것이 링크드 리스트(Linked List)라는 자료구조이다.
배열은 모든 데이터가 연속적으로 존재하지만 링크드 리스트는 불연속적으로 존재하는 데이터를 서로 연결(link)한 형태로 구성되어 있다.

링크드 리스트의 각 요소(node)들은 자신과 연결된 다음 요소에 대한 참조(주소값)와 데이터로 구성되어 있다.
```java
class Node{
    Node next;
    Object ob;
}
```
링크드 리스트에서의 데이터 삭제는 삭제하고자 하는 요소의 이전 요소가 삭제하고자 하는 요소의 다음 요소를 참조하고자 하면 된다.
단 하나의 참조만 변경하면 되므로 처리속도가 빠르다.

링크드 리스트는 이동방향이 단방향이기 때문에 다음 요소에 대한 접근은 쉽지만 이전 요소에 대한 접근은 어려운데, 이러한 부분을 보완한 것이 ```더블 링크드 리스트(이중 연결리스트, doubly linked list)```이다.

더블 링크드 리스트는 단순히 링크드 리스트에 참조변수를 하나 더 추가하여 이전 요소에 대한 참조에 대한 참조가 가능한 구조이다.
```java
class Node{
    Node next;
    Node prev;
    Object ob;
}
```
더블 링크드 리스트의 접근성을 보다 향상시킨 것이 ```더블 써큘러 링크드 리스트(이중 원형 연결리스트, doubly circular linked list)```인데, 단순히 더블 링크드 리스트의 첫 번째 요소와 마지막 요소를 서로 연결한 것이다.
이렇게 하면 마지막 요소의 다음 요소가 첫 번째 요소가 되고, 첫 번째 요소의 이전 요소가 마지막 요소가 된다.

실제로 LinkedList 클래스는 이름과 달리 ```링크드 리스트```가 아닌 ```더블 링크드 리스트```로 구현되어 있으며, 이는 링크드 리스트의 단점인 ```낮은 접근성(accessability)```를 높이기 위한 것이다.

LinkedList 클래스의 메소드는 다음과 같다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184822760-a6d583a6-95e8-4ff4-8e8a-30bb578d5014.png)
<br>
![image](https://user-images.githubusercontent.com/62749021/184822864-13f82a2b-62d7-4e01-b32f-1c647aaabd4f.png)

## 1.5 Stack과 Queue
```Stack```은 마지막에 저장(push)한 데이터를 가장 먼저 꺼내게(pop) 되는 ```LIFO(Last In First Out)```구조로 되어 있다.
```Queue```는 처음에 저장(offer)한 데이터를 가장 먼저 꺼내게(poll) 되는 ```FIFO(First in First Out)```구조로 되어 있다.
```Stack```같은 경우 순차적으로 데이터를 추가하고 삭제하므로 ```ArrayList```와 같은 배열기반의 컬렉션 클래스가 적합하다.

```Queue```는 데이터를 꺼낼 때 항상 첫 번째로 저장된 요소를 삭제하므로, 데이터의 추가/삭제가 쉬운 ```LinkedList```로 구현하는 것이 바람직하다.

### Stack의 메소드
![image](https://user-images.githubusercontent.com/62749021/184823304-1b93a28d-de85-4745-b388-3ec4fd1ac13a.png)

### Queue의 메소드
![image](https://user-images.githubusercontent.com/62749021/184823373-3ae2e5f9-7dfd-47f4-9410-7bc3b845104a.png)
<br>

### 스택과 큐의 활용
스택과 큐의 활용 예는 다음과 같다.
```
스택의 사용 예 - 수식계산, 수식괄호검사, 워드프로세서의 undo/redo, 웹브라우저 뒤로/앞으로
큐의 활용 예 - 최근사용문서, 인쇄작업 대기목록, 버퍼(buffer)
```

### PriorityQueue
```Queue``` 인터페이스의 구현체 중의 하나로, 저장한 순서에 관계없이 우선순위(Priority)가 높은 것부터 꺼내게 된다.
또한 null을 저장할 수 없다.
```PriorityQueue```는 저장공간으로 배열을 사용하며, 각 요소를 ```힙(heap)```이라는 자료구조의 형태로 저장한다.
힙은 이진 트리의 한 종류로 가장 큰 값이나 가장 작은 값을 빠르게 찾을 수 있다.


### Deque(Double-Ended Queue)
```Queue```의 변형으로, 한 쪽 끝으로만 추가/삭제가 가능하던 ```Queue```와는 달리, ```Deque(덱 또는 디큐)```는 양쪽 끝에 추가/삭제가 가능하다.
```Deque```의 조상은 ```Queue```이며, 구현체로는 ```ArrayDeque```과 ```LinkedList``` 등이 있다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184823954-f5611a6d-8a4e-41dc-8a01-5eaaa8a50a4f.png)
<br>

## 1.6 Iterator, ListIterator, Enumeration
Iterator, ListIterator, Enumeration 모두 컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스이다.
Enumeration은 Iterator의 구버전이며, ListIterator는 Iterator의 기능을 향상시킨 것이다.

### Iterator
컬렉션 프레임웤에서는 컬렉션에 저장된 요소들을 읽어오는 방법을 표준화하였다.

컬렉션에 저장된 각 요소에 접근하는 기능을 가진 Iterator 인터페이스를 정의하고
Collection 인터페이스에서는 ```Iterator(Iterator를 구현한 클래스의 인스턴스)```를 반환하는 ```iterator()```를 정의하고 있다.
```java
public interface Iterator {
    boolean hasNext();
    Object next();
    void remove();
}

public interface Collection {
    ...
    public Iterator iterator();
    ...
}
```
```iterator()```는 Collection 인터페이스에 정의된 메소드이므로 이를 구현한 ```List```와 ```Set```에도 포함되어 있다.
그러므로 ```List```나 ```Set``` 인터페이스를 구현하는 컬렉션은 ```iterator()```가 각 컬렉션의 특징에 알맞게 구현되어있다.

컬렉션 클래스에 대해 ```iterator()```를 호출하여 ```Iterator```를 얻은 다음 반복문을 사용하여 컬렉션 클래스의 요소들을 읽어올 수 있다.

여기서 선택적 기능은 반드시 구현하지 않아도 되는 메소드이다.
다만 추상 메소드이므로 클래스에서 오버라이딩 자체는 해야하며, 주로 다음과 같이 예외를 던지도록 처리한다.
```java
public void remove() {
    throw new UnsupportedOperationException();
}
```
빈 몸통만 구현할 경우 호출하는 쪽에서는 소스를 구해보기 전까지는 해당 기능이 바르게 동작하지 않는 이유를 알 수 없기 때문이다.
이러한 선택적 기능의 경우 자바 API에서 어떠한 예외로 처리해야하는지를 명시해놓은 것을 확인할 수 있다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184824408-54c8b175-0d6b-445c-8ed3-02edbd9a28f6.png)
<br>
```Iterator```를 이용해서 컬렉션의 요소를 읽어오는 방법을 표준화힜기 때문에 코드의 재사용성을 높일 수 있다.
이처럼 공통 인터페이스를 정의해서 표준을 정의하고 구현하여 표준을 따르게 함으로써 코드의 일관성을 유지하여 재사용성을 극대화하는 것이 객체지향 프로그래밍의 중요한 목적 중의 하나이다.

```Map``` 인터페이스를 구현한 컬렉션 클래스는 키(key)와 값(value)를 쌍(pair)으로 저장하고 있기 때문에 ```iterator()```를 직접 호출할 수 없고
대신 ```keySet()```이나 ```entrySet()```과 같은 메소드를 통해서 키와 값을 각각 따로 Set의 형태로 얻어 온 후 다시 ```iterator()```를 호출해야 ```Iterator```를 얻을 수 있다.
```java
Map map = new HashMap();
...
Iterator it = map.keySet().iterator();
```

### ListIterator와 Enumeration
호환성을 위해 남겨둘 뿐 ```Iterator```의 사용을 권장한다.

```ListIterator```는 ```Iterator```를 상속받아서 기능을 추가한 것으로, 컬렉션의 요소에 접근할 때 ```Iterator```는 단방향으로만 이동할 수 있는데 반해
```ListIterator```는 양방향으로의 이동이 가능하다.
다만 ```ArrayList```나 ```LinkedList```와 같이 ```List``` 인터페이스를 구현한 컬렉션에서만 사용할 수 있다.
```
Enumeration : Iterator의 구버전
ListIterator : Iterator에 양방향 조회기능추가(List를 구현한 경우만 사용가능)
```
```Enumeration``` 인터페이스의 메소드는 다음과 같다.
![image](https://user-images.githubusercontent.com/62749021/184824880-28f32bca-c79b-4d8f-90d0-415bfe393ace.png)
<br>
```ListIterator``` 인터페이스의 메소드는 다음과 같다.
![image](https://user-images.githubusercontent.com/62749021/184824964-87cd50f8-17fa-4a92-ab83-2fa780ae7cd7.png)
<br>

## 1.7 Arrays
```Arrays``` 클래스에는 배열을 다루는데 유용한 메소드가 정의되어 있다.
해당 클래스 내에는 오버로딩이 적용된 메소드가 많으므로, 매개변수의 타입이 int 배열인 메소드에 대한 사용법만을 확인한다.

### 배열의 복사 - copyOf(), copyOfRange()
copyOf()는 배열 전체를, copyOfRange()는 배열의 일부를 복사해서 새로운 배열을 만들어 반환한다.
copyOfRange()에 지정된 범위의 끝은 포함되지 않는다.

### 배열 채우기 - fill(), setAll()
fill()은 배열의 모든 요소를 지정된 값으로 채운다.
setAll()은 배열을 채우는데 사용할 함수형 인터페이스를 매개변수로 받는다.
이 메소드를 호출할 때는 함수형 인터페이스를 구현한 객체로 매개변수로 지정하던가 람다식을 지정해야한다.
```java
int[] arr = new int[5];
Arrays.fill(arr, 9); // arr = [9, 9, 9, 9, 9]
Arrays.setAll(arr, () -> (int)(Math.random() * 5) + 1); // arr = [1, 5, 2, 1, 1]
```

### 배열의 정렬과 검색 - sort(), binarySearch()
```sort()```는 배열을 정리할 때 사용한다.
```binarySearch()```는 배열에 저장된 요소를 검색할 때 사용한다.
```binarySearch()```의 경우 반드시 배열이 정렬된 상태여야 올바른 결과를 얻으며, 검색한 값과 일치하는 요소들이 여러 개 있다면 이 중에서 어떤 것의 위치가 반환될지 알 수 없다.

배열의 첫 번째 요소부터 검사하는 ```순차 검색(linear search)```는 배열이 정렬되어 있을 필요는 없지만 배열의 요소 하나씩 비교하므로 시간이 많이 걸린다.

반면에 ```이진 검색(binary search)```는 배열의 검색할 범위를 반복적으로 절반씩 줄여가면서 검색하기 때문에 검색속도가 상당히 빠르며, 배열의 길이가 10배가 늘어나더라도 검색 횟수는 3 ~ 4회 밖에 늘어나지 않으므로 큰 배열의 검색에 유리하다.
다만 배열이 정렬되어야만 한다.

### 문자열의 비교와 출력 - equals(), toString()
```toString()```은 배열의 모든 요소를 문자열로 출력한다.
```toString()```은 일차원 배열에만 사용할 수 있으므로, 다차원 배열의 경우 ```deepToString()```을 사용해야한다.

equals()는 두 배열에 저장된 모든 요소를 비교하여 같으면 ```true```, 다르면 ```false```를 반환한다.
equals()는 일차원 배열에만 사용할 수 있으므로, 다차원 배열의 경우 ```deepToEquals()```를 사용해야한다.

### 배열을 List로 변환 - asList(Object... a)
```asList()```는 배열을 ```List```에 담아서 반환한다.
매개변수의 타입이 가변인자이므로 배열 생성없이 저장할 요소들만 나열하는 것도 가능하다.

주의사항으로는 ```asList()```가 반환한 ```List```의 크기를 변환할 수 없다는 것이다.
즉 추가 또는 삭제가 불가능하다.
다만 저장된 내용은 변경 가능하다.

### parallelXXX(), spliterator(), stream()
```parellel```로 시작하는 메소드들은 보다 빠른 결과를 얻기 위해 여러 스레드가 작업을 나누어 처리하도록 한다.

```spliterator()```는 여러 쓰레드가 처리할 수 있게 하나의 작업을 여러 작업으로 나누는 ```Spliterator```를 반환한다.

```stream()```은 컬렉션을 스트림으로 변환한다.

해당 메소드들은 14장에서 더 자세히 다룬다.

## 1.8 Comparator와 Comparable
```Arrays.sort()``` 메소드의 경우 ```Character``` 클래스의 ```Comparable```의 구현에 의해 정렬된다.
```Comparator```와 ```Comparable```은 모두 인터페이스로 컬렉션을 정렬하는데 필요한 메소드를 정의하고 있다.

```Comparable```을 구현하고 있는 클래스들은 같은 타입의 인스턴스끼리 서로 비교할 수 있는 클래스들(wrapper 클래스, String 클래스, Date 클래스 등)과 같은 것들이며
주로 오름차순으로 정렬되도록 한다.

그러므로 ```Comparable```을 구현한 클래스는 정렬이 가능하다는 것을 의미한다.

```Comparator```와 ```Comparable```의 실제 코드는 다음과 같다.
```java
public interface Comparator{
    int compare(Object o1, Object o2);
    boolean equals(Object obj);
}

public interface Comparable{
    public int compareTo(Object o);
}
```
```compare()```와 ```compareTo()```는 선언 형태와 이름이 약간 다를 뿐 두 객체를 비교하는 같은 기능을 목적으로 고안되었다.

```compare()```와 ```compareTo()``` 모두 비교하는 두 객체가 다르면 0, 비교하는 값보다 작으면 음수, 크면 양수를 반환하도록 구현해야한다.

```equals()``` 메소드는 모든 클래스가 가지고 있는 공통적인 메소드이지만 ```Comparator```를 구현하는 클래스는 오버라이딩이 필요할 수도 있다는 것을 알리기 위해서 정의한 것일 뿐
그냥 ```compare(Object o1, Object 02)```만 구현하면 된다.

```Comparable```을 구현한 클래스들이 기본적으로 오름차순으로 정렬되어 있지만 내림차순으로 정렬하는 등의 다른 기준으로 정렬되도록 하고 싶을 때
```Comparator```를 통해 정렬 기준을 제공할 수 있다.
```
Comparable : 기본 정렬기준을 구현하는데 사용
Comparator : 기본 정렬기준 외에 다른 기준으로 정렬하고자 할 때 사용
```
```String```의 ```Comparable``` 구현은 문자열이 사전 순으로 정렬되도록 작성되어있다.

대소문자를 구분하지 않고 비교하는 ```Comparator```를 상수의 형태로 제공한다.
```java
public static final Comparator CASE_INSENSITIVE_ORDER
```
이 ```Comparator```를 이용하면 문자열을 대소문자 구분없이 정렬할 수 있다.
```java
Arrays.sort(strArr, String.CASE_INSENSTIVE_ORDER);
```
이를 내림차순으로 정렬하려면 다음과 같이 ```compareTo()```의 결과에 -1만 곱해줘도 된다.
다만 ```compare()```의 매개변수가 ```Object``` 타입이기 때문에 ```compareTo()```를 바로 호출할 수는 없고 ```Comparable```로 형변환을 해야한다.

### Comparable과 Comparator의 차이점
```
Comparable : 정렬 수행 시 기본적으로 적용되는 정렬 기준이 되는 메소드를 정의하는 인터페이스
    -> 해당 인터페이스를 구현했다면 기본적으로 적용되는 정렬 기준이 있다는 의미이기 때문에 구현 클래스는 정렬이 가능

Comparator : 기본 정렬 기준과 다르게 정렬하고 싶을 때 사용하는 인터페이스
    -> Comparable은 오름차순인데 내림차순으로 정렬해야 할 필요가 있을 때 Comparator를 사용
```

## 1.9 HashSet
```HashSet```은 ```Set``` 인터페이스를 구현한 가장 대표적인 컬렉션이며, 중복된 요소를 저장하지 않는다.

```HashSet```은 저장순서를 유지하지 않으므로 저장순서를 유지하고자 한다면 ```LinkedHashSet```을 사용해야한다.

```HashSet```은 내부적으로 ```HashMap```을 이용했으며, ```해싱(hasing)```을 이용하여 구현했다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184826576-337f60d0-e86b-4d8b-b4f2-5d7b368112ec.png)
<br>
hashCode()의 경우 String 클래스에 잘 구현되어 있기 때문에 이를 활용하여 처리했다.

이를 java.util.Objects 클래스의 hash()로 표현하면 다음과 같다.
```java
public int hashCode(){
    return Objects.hash(name, age); // int hash (Object.. values)
}
```
위와 같은 방식으로 ```hashCode()```를 오버라이딩 하는 것을 추천한다.
오버라이딩을 통해 작성된 hashCode()는 다음의 세 가지 조건을 만족해야 한다.
```
1. 실행 중인 애플리케이션 내의 동일한 객체에 대하여 여러 번 hashCode()를 호출해도 동일한 결과를 반환해야 한다.
    (단 equals() 메소드의 구현에 사용된 멤버변수의 값이 바뀌지 않았다고 가정한다.)
2. equals() 메소드를 이용한 비교에 의해 true를 얻은 두 객체에 대해 각각 hashCode()를 호출해서 얻은 결과는 반드시 같아야 한다.
3. equals() 메소드를 호출했을 때 false를 반환하는 두 객체는 hashCode() 호출에 의해 같은 값을 반환하는 경우가 있어도 괜찮지만
    해싱(hasing)을 사용하는 컬렉션의 성능을 향상시키기 위해 다른 값을 반환하도록 하는 편이 좋다.
```

## 1.10 TreeSet
```TreeSet```인 ```이진 검색 트리(binary search tree)```라는 자료구조의 형태로 데이터를 저장하는 컬렉션 클래스이다.
이진 검색 트리는 정렬, 검색, 범위검색(range search)에 높은 성능을 보이는 자료구조이며 ```TreeSet```은 이진 검색 트리의 성능을 향상시킨 ```레드-블랙 트리(Red-Black tree)```로 구현되어 있다.

```Set``` 인터페이스를 구현했으므로 중복된 데이터의 저장을 허용하지 않으며 정렬된 위치에 저장하므로 저장순서를 유지하지도 않는다.

이진 트리(binary tree)는 링크드리스트처럼 여러 개의 노드(node)가 서로 연결된 구조로, 각 노드에 최대 2개의 노드를 연결할 수 있으며
```루트(root)```라고 불리는 하나의 노드에서부터 시작해 계속 확장해 나갈 수 있다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184827182-a2abb60e-94a6-486a-a694-802384c0f18d.png)
<br>
```java
class TreeNode{
    TreeNode left;
    Object element;
    TreeNode right;
}
```
예를 들어 이진검색트리에 7, 4, 9, 1, 5의 순서로 값을 저장한다고 하면 다음과 같은 순서로 진행된다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184827365-cd677585-3c48-4254-968e-147337fd4e62.png)
<br>
```TreeSet```에 저장되는 객체가 ```Comparable```을 구현하거나, ```Comparator```를 제공해서 두 객체를 비교할 방법을 알려줘야 사용할 수 있다.

왼쪽 마지막 값에서부터 오른쪽 값까지의 값을 ```왼쪽 노드 -> 부모 노드 -> 오른쪽 노드``` 순으로 읽어오면 오름차순으로 정렬된 순서를 얻을 수 있다.
```TreeSet```은 이처럼 정렬된 상태를 유지하므로 단일 값 검색과 범위 검색이 매우 빠르다.
즉 검색효율이 뛰어난 자료구조이다.
트리는 데이터를 순차적으로 저장하는 것이 아니라 저장위치를 찾아서 저장해야하고, 삭제는 경우 트리의 일부를 재구성해야하므로 링크드 리스트보다 데이터의 추가/삭제는 비효율적이다.
대신 배열이나 링크드 리스트에 비해 검색과 정렬기능이 더 뛰어나다.

**이진 검색 트리의 특징**
```
모든 노드는 최대 두 개의 자식 노드를 가질 수 있다.
왼쪽 자식노드의 값은 부모노드의 값보다 작고 오른쪽 자식노드의 값은 부모노드의 값보다 커야 한다.
노드의 추가 삭제에 시간이 걸린다.(순차적으로 저장하지 않으므로)
검색(범이검색)과 정렬에 유리하다.
중복된 값을 저장하지 못한다.
```
<br>

![image](https://user-images.githubusercontent.com/62749021/184827657-e57cec52-3bf4-4f66-8a21-4dd007b14edf.png)

<br>

## 1.11 HashMap과 Hashtable
```Hashtable```은 호환성을 위해 남겨둔 클래스로 ```HashMap```을 사용할 것을 권장한다.

```HashMap``` 또한 ```Map``` 인터페이스를 구현했으므로 키(key)와 값(value)를 묶어서 하나의 데이터(entry)로 저장한다.
또한 해싱(hasing)을 사용하여 많은 양의 데이터를 검색하는데 뛰어난 성능을 가진다.

```HashMap```은 키와 값을 각각 ```Object``` 타입으로 저장한다.
키는 주로 ```String```을 쓰는 편이다.
```
키(key) : 컬렉션 내의 키(key) 중에서 유일해야 한다.
값(value) : 키(key)와 달리 데이터의 중복을 허용한다.
```
![image](https://user-images.githubusercontent.com/62749021/184827906-92ea3762-e0eb-4890-a805-bd4d59741f01.png)
<br>

### 해싱과 해시함수
해싱이란 해시함수(hash function)을 이용해서 데이터를 해시테이블(hash table)에 저장하고 검색하는 기법을 의미한다.
해시함수는 데이터가 저장되어 있는 곳을 알려 주기 때문에 다량의 데이터 중에서도 원하는 데이터를 빠르게 찾을 수 있다.
해싱에서 사용하는 자료구조는 다음과 같이 배열과 링크스 리스트의 조합으로 되어 있다.
저장할 데이터의 키를 해시함수에 넣으면 배열의 한 요소를 얻게 되고, 다시 그 곳에 연결되어 있는 링크드 리스트에 저장하게 된다.
링크드 리스트는 검색에 불리한 자료구조이기 때문에 링크드 리스트의 크기가 커질수록 검색 속도가 떨어지게 된다.
배열은 배열의 크기가 커져도 원하는 요소가 몇 번째에 있는지만 알면 되므로 빠르게 원하는 값을 찾을 수 있다.
하나의 링크드 리스트에 최소한의 데이터만 저장하려면 저장될 데이터의 크기를 고려해서 HashMap의 크기를 적절하게 지정해야 하고
해시함수가 서로 다른 키에 대해 중복된 해시코드의 반환을 최소화해야 한다.
그래야 HashMap에서 빠른 검색시간을 얻을 수 있다.

## 1.12 TreeMap
```TreeMap```은 이진검색트리의 형태로 키와 값의 쌍으로 이루어진 데이터를 저장한다.
그래서 검색과 정렬에 적합한 컬렉션 클래스이다.
다만 검색에 관한 대부분의 경우에서 ```HashMap```이 ```TreeMap```보다 뛰어나므로 범위검색이나 정렬이 필요한 경우가 아니라면 ```HashMap```을 사용하는 것을 권장한다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184828316-fae82838-808e-444a-a082-273ce892554b.png)
<br>

## 1.13 Properties
```Properties```는 ```HashMap```의 구버전인 ```Hashtable```을 상속받아 구현한 것으로 (String, String)의 형태로 저장하는 보다 단순화된 컬렉션클래스이다.

주로 애플리케이션의 환경설정과 관련된 속성(property)을 저장하는데 사용되며 데이터를 파일로부터 읽고 쓰는 편리한 기능을 제공한다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184828454-e7b93144-4231-417d-bef4-92d3ee7de253.png)
<br>

## 1.14 Collections
```Arrays```가 배열에 관련된 메소드를 제공하는 것처럼, ```Collections```는 컬렉션과 관련된 메소드를 제공한다.

### 컬렉션의 동기화
```멀티 쓰레드(multi-thread)``` 프로그래밍에서는 하나의 객체를 여러 쓰레드가 동시에 접근할 수 있기 때문에 ```데이터의 일관성(consistency)```를 유지하기 위해
공유되는 객체에 동기화```(synchronization)```가 필요하다.

```Vector```와 같은 구버전의 클래스들은 자체적으로 동기화 처리가 되어있으나, 멀티 쓰레드 프로그래밍이 아닌 경우 불필요한 기능이 되어 성능 저하의 원인이 된다.

그래서 새롭게 추가된 ```ArrayList```나 ```HashMap```과 같은 컬렉션은 동기화를 자체적으로 처리하지 않고 필요한 경우에만
```java.util.Collections``` 클래스의 동기화 메소드를 이용하여 동기화 할 수 있도록 변경했다.
```java
static Collection synchronizedCollection(Collection c)
static List synchronizedList(List list)
static set synchronizedSet(Set s)
static Map synchronizedMap(Map m)
static SortedSet synchronizedSortedSet(SortedSet s)
static SortedMap synchronizedSortedMap(SortedMap m)
```
위 메소드들 중 필요에 따라 선택하여 다음과 같이 사용하면 된다.
```java
List sncList = Collections.synchronizedList(new ArrayList(...));
```
### 변경불가 컬렉션 만들기
컬렉션에 저장된 데이터를 보호하기 위해서 컬렉션을 변경할 수 없게 읽기 전용으로 반드는 방법이다.
주로 멀티 쓰레드 프로그래밍에서 여러 쓰레드가 하나의 컬렉션을 공유하다보면 데이터가 손상될 수 있는데, 이를 방지하기 위함이다.
```java
static Collection unmodifiableCollection(Collection c)
static List unmodifiableList(List list)
static Set unmodifiableSet(Set s)
static NavigableSet unmodifiableNavigableSet(NavigableSet s)
static SortedSet unmodifiableSortedSet(SortedSet s)
static NavigableMap unmodifiableNavigableMap(NavigableMap m)
static SortedMap unmodifiableSortedMap(SortedMap m)
```

### 싱글톤 컬렉션 만들기
인스턴스를 new 연산자가 아닌 메소드를 통해서만 생성할 수 있게 함으로써 생성할 수 있는 인스턴스의 개수를 제한하는 방법이다.
```java
static List singletonList(Object o)
static Set singleton(Object o)
static Map singletonMap(Object key, Object value)
```

### 한 종류의 객체만 저장하는 컬렉션 만들기
컬렉션에 모든 종류의 객체를 저장할 수 있다는 것은 장점이기도 하고 단점이기도 하다.
컬렉션에 지정된 종류의 객체만 저장할 수 있도록 제한하고자 할 때에는 다음의 메소드를 이용한다.
```java
static Collection checkedCollection(Collection c)
static List checkedList(List list, Class type)
static Set checkedSet(Set s, Class type)
static Map checkedMap(Map m, Class keyType, Class valueType)
static Queue checkedQueue(Queue queue, Class type)
static NavigableSet checkedNavigableSet(NavigableSet s, Class type)
static SortedSet checkedSortedSet(SortedSet s, Class type)
static NavigableMap checkedNavigableMap(NavigableMap m, Class type)
static SortedMap checkedSortedMap(SortedMap m, Class type)
```
사용법은 다음과 같다.
```java
List list = new ArrayList();
List checkedList = checkedList(list , String.class);
```
메소드 뿐만 아니라 ```지네릭스(generices)```로도 처리할 수 있으며, 그럼에도 불구하고 이러한 메소드를 제공하는 이유는 호환성 때문이다.
```지네릭스```는 JDK 1.5부터 도입된 기능이므로 JDK 1.5 이전에 작성된 코드에서는 이 메소드가 필요할 수도 있었다.

## 1.15 요약
<br>

![image](https://user-images.githubusercontent.com/62749021/184829244-47dad01a-8ce6-4e9c-8c8a-e8dca69f6d97.png)

<br>

![image](https://user-images.githubusercontent.com/62749021/184829162-b4d779c2-cb9e-4c32-848a-0119e70de4c0.png)
