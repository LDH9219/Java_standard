# Java.lang 패키지와 유용한 클래스(java.lang package & util classes)
## 1.1 java.lang 패키지

```java.lang```패키지는 자바 프로그래밍에 가장 기본이 되는 클래스들을 포함하고 있다.
```java.lang```패키지는 ```import```문 없이도 사용 가능하다.

## 1.2 Object 클래스
```Object``` 클래스는 모든 클래스의 최고 조상이므로 ```Object``` 클래스의 멤버는 모든 클래스에서 사용 가능하다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184493851-945ccfc3-eca2-4cd5-92ab-51340d0d8d4e.png)
<br>

이 중 ```notify()```, ```notifyAll()```, ```wait()```는 스레드 관련 메소드이며, 현재 챕터에서는 다루지 않는다.
또한, 나머지 메소드 중 중요한 몇 가지만 우선적으로 확인한다.

#### equals(Object obj)

매개변수로 객체의 참조변수를 받아 비교하여 그 결과를 ```boolean```으로 출력한다.
```java
public boolean equals(Object obj){
    return (this == obj);
}
```
위 코드가 ```equals()```메소드의 원형이며, 두 객체의 같고 다름을 참조변수의 값으로 판단한다.
그러므로 서로 다룬 두 객체를 비교하면 항상 false를 출력한다.
#### hashcode()
해당 메소드는 ```해싱(hasing)``` 기법에 사용되는 ```해시 함수(hash function)```를 구현한 것이다.
해싱은 데이터 관리 기법중 하나로 다량의 데이터를 저장하고 검색하는 데 유용하다.
해시함수는 찾고자하는 값을 입력하면 그 값이 저장된 위치를 알려주는 ```해시코드(hash code)```를 반환한다.
일반적으로 해시코드가 같은 두 객체가 존재하는 것이 가능하지만 ```Object``` 클래스에 정의된 ```hashCode()``` 메소드는 객체의 주소값을 이용하여 해시코드를 만들어 반환하기 때문에 서로 다른 두 객체는 결코 같은 해시코드를 가질 수 없다.
그러므로 클래스의 인스턴스 데이터로 객체의 같고 다름을 판단하려면 ```equals()``` 메소드 뿐만 아니라 ```hashCode()``` 메소드까지 오버라이딩을 해야 한다.
#### toString()

```toString()```메소드는 인스턴스에 대한 정보를 문자열(String)으로 제공할 목적으로 정의한 것이다.

```Object```클래스에 정의된 ```toString()```은 다음과 같다.
```java
public String toString(){
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```










