# 지네릭스, 열거형, 애너테이션(generics, enumeration, auuotation)
## 1.1 지네릭스
지네릭스는 다양한 타입의 객체들을 다루는 메소드나 컬렉션 클래스에 컴파일 시의 타입 체크(compile-time type check)를 해주는 기능이다.
객체의 타입을 컴파일 시에 체크하기 때문에 객체의 타입 안정성을 높이고 형변환의 번거로움을 줄어들인다.

타입 안정성을 높인다는 것은 의도하지 않은 타입의 객체가 저장되는 것을 막고, 저장된 객체를 꺼내올 때 원래의 타입과 다른 타입으로 잘못 형변환되어 발생할 수 있는 오류를 줄여준다는 의미이다.
지네릭스의 장점
```
타입 안정성을 제공한다.
타입체크와 형변환을 생략할 수 있으므로 코드가 간결해진다.
```

## 1.2 지네릭 클래스의 선언
지네릭 타입은 클래스와 메소드에 선언할 수 있다.
지네릭 클래스의 선언 방법은 다음과 같다.
```java
class Box{
    Object item;
    
    void setItem(Object item)    {
        this.item = item;
    }
    
    Object getItem()    {
        return item;
    }
}

class Box<T>{ // 지네릭 타입 T를 선언
    T item;
    
    void setItem(T item)    {
        this.item = item;
    }
    
    T getItem()    {
        return item;
    }
}
```

Box<T>에서 T를 ```타입 변수(type variable)```이라고 한다.
타입 변수는 T가 아닌 다른 것을 사용해도 된다.

ArrayList<E>의 경우, 타입 변수 E는 ```Element(요소)```에서 가져온 것이다.

타입 변수가 여러개인 경우 Map<K, V>와 같이 콤마 ```,```를 구분자로 나열하면 된다.

K는 key, V는 value를 의미한다.
이처럼 상황에 맞게 의미있는 문자를 선택해서 사용하는 편이 좋다.

Box 지네릭 클래스의 객체를 생성할 때에는 다음과 같이 참조변수와 생성자에 타입 T 대신 실제로 사용될 타입을 지정해야 한다.

```java
Box<String> b = new Box<String>();
b.setItem("ABC");
String item = b.getItem();
```
