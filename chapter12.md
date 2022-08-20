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
<br>

```Box<T>```에서 T를 ```타입 변수(type variable)```이라고 한다.
타입 변수는 T가 아닌 다른 것을 사용해도 된다.

```ArrayList<E>```의 경우, 타입 변수 E는 ```Element(요소)```에서 가져온 것이다.

타입 변수가 여러개인 경우 Map<K, V>와 같이 ```콤마 ,```를 구분자로 나열하면 된다.

```K```는 key, ```V```는 value를 의미한다.
이처럼 상황에 맞게 의미있는 문자를 선택해서 사용하는 편이 좋다.

Box 지네릭 클래스의 객체를 생성할 때에는 다음과 같이 참조변수와 생성자에 타입 T 대신 실제로 사용될 타입을 지정해야 한다.
```java
Box<String> b = new Box<String>();
b.setItem("ABC");
String item = b.getItem();
```

### 지네릭스의 용어
```
Box<T> : 지네릭 클래스. 'T의 Box' 또는 'T Box`라고 읽는다.
T : 타입 변수 또는 타입 매개변수. (T는 타입 문자)
Box : 원시 타입(raw type)
```
타입 매개변수에 타입을 지정하는 것을 ```지네릭 타입 호출```이라고 하며, 지정된 타입 String을 ```매개변수화된 타입(parameterized type)```이라고 한다.
컴파일 후에는 ```Box<String>```은 이들의 원시 타입인 Box로 변경된다.
즉, 지네릭 타입이 제거된다.

### 지네릭스의 제한
모든 객체에 대해 동일하게 동작해야하는 static 멤버에 타입 변수 T를 사용할 수 없다.
T는 인스턴스 변수로 간주되기 때문이다.  
```java
class Box<T>{
    static T item; // 에러
    static int compare(T t1, T t2) {...} // 에러
}
```
이는 static멤버는 타입 변수에 지정된 타입, 즉 대입된 타입의 종류와 관계없이 동일한 것이어야 하기 때문이다.

지네릭 타입의 배열을 생성하는 것도 허용되지 않는다.
지네릭 배열 타입의 참조 변수를 선언하는 것은 가능하지만, ```new T[10]```과 같이 배열을 생성하는 것은 제한된다.
그 이유는 new 연산자 때문인데, 이 연산자는 컴파일 시점에 타입 T가 뭔지 알아야 하기 때문이다.
instanceof 연산자도 new 연산자와 같은 이유로 T를 피연산자로 사용할 수 없다.

지네릭 배열이 필요하다면, new 연산자 대신 ```Reflaction API```의 ```newInstance()```와 같이 동적으로 객체를 생성하는 메소드로 배열을 생성하거나, Object 배열을 생성해서 복사한 다음 T[]로 형변환하는 방법 등을 사용해야 한다.

## 1.4 지네릭 클래스의 객체 생성과 사용
```java
class Box<T>{
    ArrayList<T> list = new ArrayList<T>();
    
    void add(T item)    {
        list.add(item);
    }
    
    T get(int i)    {
        return list.get(i);
    }
    
    ArrayList<T> getList()    {
        return list;
    }
    
    int size()    {
        return list.size();
    }
    
    public String toString()    {
        return list.toString();
    }
}
```
```Box<T>```의 객체를 생성할 때는 다음과 같아야 한다.
참조변수와 생성자에 대입된 타입(매개변수화된 타입)이 일치해야 한다.
```java
Box<Apple> appleBox = new Box<Apple>(); // OK
Box<Apple> appleBox = new Box<Grape>(); // error
```

## 1.5 제한된 지네릭 클래스
타입 문자로 사용할 타입을 명시하면 한 종류의 타입만 저장할 수 있도록 제한할 수 있지만, 그래도 여전히 모든 종류의 타입을 지정할 수 있다는 것에는 변함이 없다.
다음과 같이 지네릭 타입에 ```extends```를 사용하면, 특정 타입의 자손들만 대입할 수 있게 제한할 수 있다.

```java
class FruitBox<T extends Fruit> { // Fruit의 자손만 타입으로 지정 가능
    ArrayList<T> list = new ArrayList<T>();
    ...
}
```
한 종류의 타입만 담을 수 있는 상태에서 ```Fruit``` 클래스의 자손들만 담을 수 있다는 제한이 추가된 것이다.
```add()```의 매개변수의 타입 T도 Fruit와 그 자손 타입이 될 수 있으므로 다음과 같이 여러 과일을 담을 수 있는 상자가 가능하게 된다.
```java
FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
fruitBox.add(new Apple()); 
fruitBox.add(new Grape());
```
다형성에서 조상타입의 참조변수로 자손타입의 객체를 가리킬 수 있는 것처럼 매개변수화된 타입의 자손 타입도 가능하다.
타입 매개변수 T에 Object를 대입하면 모든 종류의 객체를 저장할 수 있게 된다.

만약 클래스가 아니라 인터페이스를 구현해야 한다는 제약이 필요할 때에도 extends를 사용한다.
```implements```를 사용하지 않는다는 점에 주의해야 한다.

클래스 Fruit의 자손이면서 Eatable 인터페이스도 구현해야 한다면 다음과 같이 ```&``` 기호로 연결한다.
```java
class FruitBox<T extends Fruit & Eatable> { ... }
```

## 1.6 와일드 카드
매개변수에 과일박스를 대입하면 주스를 만들어서 반환하는 Juicer라는 클래스가 있고, 이 클래스에는 과일을 주스로 만들어서 반환하는 makeJuice()라는 static 메소드가 다음과 같이 정의되어 있다고 가정하자.
```java
class Juicer{
    static Juice makeJuice(FruitBox<Fruit> box)    {
        String tmp = "";
        for (Fruit f : box.getList() )
            tmp += f + " ";
        return new Juice(tmp);
    }
}
```
Juicer 클래스는 지네릭 클래스가 아니고, 지네릭 클래스의 static 메소드는 타입 매개변수 T를 매개변수에 사용할 수 없으므로 지네릭스를 사용하지 않거나 위와 같이 타입 매개변수 대신 특정 타입을 지정해줘야 한다.
```java
FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
FruitBox<Apple> appleBox = new FruitBox<Apple>();

System.out.println(Juicer.makeJuice(fruitBox)); // OK. FruitBox<Fruit>
System.out.println(Juicer.makeJuice(appleBox)); // error. FruitBox<Apple>
```
위와 같이 지네릭 타입을 ```FruitBox<Fruit>```로 고정해 놓으면 ```FruitBox<Apple>``` 타입의 객체는 ```makeJuice()```의 매개변수가 될 수 없으므로 다음과 같이 여러 가지 타입의 매개변수를 갖는 ```makeJuice()```를 만들 수 밖에 없다.
```java
static Juice makeJuice(FruitBox<Fruit> box){
    String tmp = "";
    for (Fruit f : box.getList() )
        tmp += f + " ";
    return new Juice(tmp);
}

static Juice makeJuice(FruitBox<Apple> box){
    String tmp = "";
    for (Fruit f : box.getList() )
        tmp += f + " ";
    return new Juice(tmp);
}
```
그러나 위와 같이 오버로딩하면 컴파일 에러가 발생한다.
지네릭 타입이 다른 것만으로는 오버로딩이 성립되지 않기 때문이다.
지네릭 타입은 컴파일러가 컴파일할 때만 사용하고 제거해버리므로 메소드 중복 정의가 된다.

이럴 때 사용하는 것이 와일드 카드로, 기호 ?로 표현하며, 어떠한 타입도 될 수 있다.

?만으로는 Object 타입과 다를 게 없으므로 extends나 super로 상한(upper bound)와 하한(lower bound)를 제한할 수 있다.
```
<? extends T> : 와일드 카드의 상한 제한. T와 그 자손들만 가능.
<? super T> : 와일드 카드의 하한 제한. T와 그 조상들만 가능.
<?> : 제한 없음. 모든 타입이 가능. <? extends Object>와 동일.
```
```java
static Juice makeJuice(FruitBox<? extends Fruit> box){
    String tmp = "";
    for (Fruit f : box.getList() )
        tmp += f + " ";
    return new Juice(tmp);
}
```
그러나 실제로 테스트해보면 문제없이 컴파일되는데 그 이유는 바로 지네릭 클래스 FruitBox를 제한했기 때문이다.
```
class FruitBox<T extends Fruit> extends Box<T> {}
```
컴파일러는 위 문장으로부터 모든 FruitBox의 요소들이 Fruit의 자손이라는 것을 알고 있으므로 문제가 발생하지 않은 것이다.

## 1.7 지네릭 메소드
메소드의 선언부에 지네릭 타입이 선언된 메소드를 지네릭 메소드라 한다.
지네릭 타입의 선언 위치는 반환 타입 바로 앞이다.
```
static <T> void sort(List<T> list, Comparator<? super T> c)
```
지네릭 클래스에 정의된 타입 매개변수와 지네릭 메소드에 정의된 타입 매개변수는 전혀 별개의 것이다.
같은 타입 문자 T를 사용해서 같은 것이 아니다.

또한 static 메소드에도 지네릭 타입을 선언할 수 있다.

메소드에 선언된 지네릭 타입은 지역 변수를 선언한 것과 같다고 생각하면 이해하기 쉽다.
이 타입 매개변수는 메소드 내에서만 지역적으로 사용될 것이므로 메소드가 static이건 아니건 상관이 없는 것이다.

앞서 사용했던 makeJuice()를 지네릭 메소드로 바꾸면 다음과 같다.
```java
static Juice makeJuice(FruitBox<? extends Fruit> box){
    String tmp = "";
    for (Fruit f : box.getList() )
        tmp += f + " ";
    return new Juice(tmp);
}

static <? extends Fruit> Juice makeJuice(FruitBox<T> box){
    String tmp = "";
    for (Fruit f : box.getList() )
        tmp += f + " ";
    return new Juice(tmp);
}
```
이제 이 메소드를 호출할 때에는 아래와 같이 타입 변수에 타입을 대입해야 한다.
```java
FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
FruitBox<Apple> appleBox = new FruitBox<Apple>();

System.out.println(Juicer.<Fruit>makeJuice(fruitBox)); 
System.out.println(Juicer.<Apple>makeJuice(appleBox)); 
```
그러나 대부분의 경우 컴파일러가 타입을 추정할 수 있기 때문에 생략해도 된다.
```java
System.out.println(Juicer.makeJuice(fruitBox)); 
System.out.println(Juicer.makeJuice(appleBox)); 
```
주의할 점으로 지네릭 메소드를 호출할 때, 대입된 타입을 생략할 수 없는 경우에는 참조변수가 클래스 이름을 생략할 수 없다는 것이다.
```java
System.out.println(<Fruit>makeJuice(fruitBox));  // error. 클래스 이름 생략불가
System.out.println(this.<Fruit>makeJuice(fruitBox)); // OK
System.out.println(Juicer.<Fruit>makeJuice(appleBox)); // OK
```
같은 클래스 내에 있는 멤버들끼리는 참조변수나 클래스 이름, 즉 this.나 클래스이름.을 생략하고 메소드 이름만으로 호출이 가능하지만, 대입된 타입이 있을 때는 반드시 써야한다.
```java
public static void printAll(ArrayList<? extends product> list, ArrayList<? extends Product> list2)

public static <T extends Product> void printAll(ArrayList<T> list, ArrayList<T> list2)
```
## 1.8 지네릭 타입의 형변환
```
Box box = null;
Box<Object> objBox = null;

box = (Box)objBox; // OK. 지네릭 타입 -> 원시 타입. 경고 발생
objBox = (Box<Object>)box; // OK. 원시 타입 -> 지네릭 타입. 경고 발생
```
위와 같이 지네릭 타입과 논-지네릭(non-generic) 타입간의 형변환은 항상 가능하며, 경고가 발생한다.
```java
Box<Object> objBox = null;
Box<String> strBox = null;

objBox = (Box<Object>)strBox; // 에러. Box<String> -> Box<Object>
strBox = (Box<String>)objBox; // 에러. Box<Object> -> Box<String?>
```
위와 같이 대팁된 타입이 다른 지네릭 타입 간에는 형변환이 불가능하며, 에러가 발생한다.
```java
Box<? extends Object> wBox = new Box<String>();
```
위와 같은 경우는 ```Box<String>```이 ```Box< ? extends Object >```로 형변환되며, 이로 인해 다형성을 적용시킬 수 있다.
```java
// 매개변수로 FruitBox<Fruit>, FruitBox<Apple>, FruitBox<Grape> 등이 가능
static Juice makeFuice(FruitBox<? extends Fruit> box) {...{

FruitBox<? extends Fruit> box = new FruitBox<Fruit>();
FruitBox<? extends Fruit> box = new FruitBox<Apple>();
FruitBox<? extends Fruit> box = new FruitBox<Grape>();
```
반대의 경우도 가능하지만, 확인되지 않은 형변환이라는 경고가 발생한다.
```
FruitBox<? extends Fruit> box = null;
// OK. 미확인 타입으로 형변환 경고
FruitBox<Apple> appleBox = (FruitBox<Apple>)box;
```
```FruitBox< ? extends Fruit >```에 대입될 수 있는 타입이 여러 개이며, ```FruitBox<Apple>```을 제외한 다른 타입은 ```FruitBox<Apple>```로 변경될 수 없기 때문이다.

## 1.9 지네릭 타입의 제거
컴파일러는 지네릭 타입을 이용해서 소스파일을 체크하고, 필요한 곳에 형변환을 넣어준다.
그리고 지네릭 타입을 제거한다.
즉 컴파일된 파일(.class)에는 지네릭 타입에 대한 정보가 없다.
이렇게 하는 주된 이유는 지네릭이 도입되기 이전의 소스 코드와의 호환성을 유지하기 위해서이다.

다음은 기본적인 지네릭 타입의 제거 과정이다.
1. 지네릭 타입의 경계(bound)를 제거한다. 지네릭 타입의 < T extends Fruit>라면 T는 Fruit로 치환된다. <\T>인 경우는 T는 Object로 치환된다. 클래스 옆의 선언은 제거된다.
```java
class Box<T extends Fruit>{
    void add(T t)    {
        ...
    }
}

class Box{
    void add(Fruit t)    {
        ...
    }
}
```
2. 지네릭 타입을 제거한 후에 타입이 일치하지 않으면 형변환을 추가한다. List의 get()은 Object 타입을 반환하므로 형변환이 필요하다.
```java
T get(int i){
    return list.get(i);
}

Fruit get(int i){
    return (Fruit)list.get(i);
}
```
와일드 카드가 포함되어 있는 경우에는 다음과 같이 적절한 타입으로의 형변환이 추가된다.
```java
static Juice makeJuice(FruitBox<? extends Fruit> box)
{
    Strign tmp = "";
    for (Fruit f : box.getList()) 
        tmp += f + " ";
    return new Juice(tmp);
}

static Juice makeJuice(FruitBox box)
{
    Strign tmp = "";
    Iterator it = box.getList().iterator();
    while (it.hasNext())
    {
        tmp += (Fruit)it.next() + " ";
    }
    return new Juice(tmp);
}
```

## 1.10 열거형(enums)이란?
열거형은 서로 관련된 상수를 편리하게 선언하기 위해 JDK 1.5부터 추가된 것으로 여러 상수를 정의할 때 사용하면 유용하다.

자바의 열거형은 열거형이 갖는 값 뿐만 아니라 타입도 관리하기 때문에 보다 논리적인 오류를 줄일 수 있다.
```java
class Card{
    static final int CLOVER = 0;
    static final int HEART = 1;
    static final int DIAMOND = 2;
    static final int SPADE = 3;
    
    static final int TWO = 0;
    static final int THREE = 1;
    static final int FOUR = 2;
    
    final int kind;
    final int num;
}

class Card
{
    enum Kind { CLOVER, HEART, DIAMOND, SPADE } // 열거형 kind를 정의
    enum Value { TWO, THREE, FOUR } // 열거형 Value를 정의
    
    final Kind kind;
    final Value value;
}
```
자바의 열거형은 타입에 안전한 열거형(typesafe enum)이므로 실제 값이 같아도 타입이 다르면 컴파일 에러가 발생한다.
이처럼 값 뿐만 아니라 타입까지 체크하므로 안전하다고 볼 수 있다.
```java
if (Card.CLOVER == Card.TWO) // true지만 false여야 의미상 맞음.

if (Card.Kind.CLOVER == Card.Value.TWO) // 컴파일 에러. 값은 같지만 타입이 다름
```
상수의 값이 바뀌면 해당 상수를 차몾하는 모든 소스를 다시 컴파일 해야 하지만 열거형 상수를 사용하면 기존의 소스를 다시 컴파일 하지 않아도 된다.

## 1.11 열거형의 정의와 사용
열거형의 정의는 다음과 같다.
```
enum 열거형 이름 { 상수명 1, 상수명 2, ... }
```
이 열거형에 정의된 상수를 사용하는 방법은 ```열거형이름.상수명```이다.
클래스의 static 변수를 참조하는 것과 동일하다.
```java
class Unit{
    int x, y;
    Direction dir;
    
    void init()    {
        dir = Direction.EAST; // 유닛의 방향을 EAST로 초기화
    }
}
```
열거형 상수간의 비교에는 ```==```를 사용할 수 있다.
```equals()```가 아닌 ```==```이므로 빠른 성능을 제공한다.
그러나 비교연산자는 사용할 수 없고, ```compareTo()```를 사용해야 한다.

```switch```문의 조건식에도 열거형을 사용할 수 있다.
주의사항으로는 case문에 열거형의 이름은 적지 말고 상수의 이름만 적어야 한다는 것이다.
```java
void move(){
    switch(dir)    {
        case EAST: x++; break;
        case WEST: x--; break;
        case SOUTH: y++; break;
        case NORTH: y--; break;
    }
}
```

### 모든 열거형의 조상 - java.lang.Enum
```values()``` 메소드는 모든 상수를 배열에 담아 반환하며, 모든 열거형이 가지고 있는 것으로 컴파일러가 자동으로 추가해준다.
```ordinal()```은 모든 열거형의 조상인 ```java.lang.Enum``` 클래스에 정의된 것으로, 열거형 상수가 정의된 순서(0부터 시작)를 정수로 반환한다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185417216-67225dfb-614f-4b34-9997-fa7d36d1f668.png)
<br>
이외에도 values()처럼 컴파일러가 자동적으로 추가해주는 메소드 valueOf()가 있다.

이 메소드는 열거형 상수의 이름으로 문자열 상수에 대한 참조를 얻을 수 있게 해준다.
```java
Direction d = Direction.valueOf("WEST");

System.out.println(d); // WEST;
System.out.println(Direction.WEST == Direction.valueOf("WEST"); // true
```

## 1.12 열거형에 멤버 추가하기
Enum 클래스에 정의된 ordinal()이 열거형 상수가 정의된 순서를 반환하지만, 이 값을 열거형 상수의 값으로 사용하지 않는 편이 좋다.
이 값은 내부적인 용도로만 사용되기 위한 것이기 때문이다.
열거형 상수의 값이 불연속적인 경우에는 다음과 같이 열거형 상수의 이름 옆에 원하는 값을 괄호()와 함께 적어주면 된다.
```java
enum Direction { EAST(1), SOUTH(5), WEST(-1), NORTH(10) }
```
그리고 지정된 값을 저장할 수 있는 인스턴스 변수와 생성자를 새로 추가해야 한다.
주의사항으로는, 열거형 상수를 모두 정의한 다음에 다른 멤버들을 추가해야 한다는 것이다.
또한 열거형 상수의 마지막에 ```;```를 넣어야 한다.

```java
enum Direction{
    EAST(1), SOUTH(5), WEST(-1), NORTH(10);
    
    private final int value;
    Direction(int value) { this. value = value; }
    
    public int getValue() { return value };
}
```
열거형의 인스턴스 변수는 반드시 final일 필요는 없지만 value는 열거형 상수의 값을 저장하기 위한 것이므로 final을 추가했다.

다만 열거형의 생성자는 제어자가 묵시적으로 private으로, 열거형의 인스턴스는 생성할 수 없다.
```java
enum Direction{
    EAST(1, ">"), SOUTH(2, "V"), WEST(3, "<"), NORTH(4, "^");
    
    private final int value;
    private final String symbol;
    
    Direction(int value, String symbol)    {
        this.value = value;
        this.symbol = symbol;
    }
    
    public int getValue() { return value; }
    public String getSymbol() { return symbol; }
}
```
위와 같이 하나의 열거형 상수에 여러 값을 지정할 수 있다.
다만 그에 맞게 인스턴스 변수와 생성자 등을 새로 추가해야 한다.

### 열거형에 추상 메소드 추가하기
다음과 같은 열거형 Transportation은 운송 수단의 종류 별로 상수를 정의하고, 각 운송 수단에는 기본요금(BASIC_FARE)이 책정되어 있다.
```java
enum Transportation{
    BUS(100), TRAIN(150), SHIP(100), AIRPLANE(300);
    
    private final int BASIC_FARE;
    
    private Transportation(int basicFare)    {
        BASIC_FARE = basicFare;
    }
    
    int fare(){ // 운송 요금 반환
        return BASIC_FARE
    }
}
```
여기서 거리에 따라 요금을 계산하는 방식이 각 운송 수단마다 다르기 때문에 열거형에 추상 메소드 fare(int distance)를 선언하면 각 열거형 상수가 이 추상 메소드를 반드시 구현해야 한다.
```java
enum Transportation{
    BUS(100) {
        int fare(int distance) { return distance * BASIC_FARE; }
    }
    , 
    TRAIN(150) { int fare(int distance) { return distance * BASIC_FARE;} },
    , SHIP(100) { int fare(int distance) { return distance * BASIC_FARE;} }
    , AIRPLANE(300) { int fare(int distance) { return distance * BASIC_FARE;} };
    
    abstract itn fare(int distance);
    
    protected final int BASIC_FARE; // protected로 해야 각 상수에서 접근 가능

    private Transportation(int basicFare)    {
        BASIC_FARE = basicFare;
    }
    
    int fare(){ // 운송 요금 반환
        return BASIC_FARE
    }
}
```
열거형에 추상 메소드는 잘 사용되지 않으니 참고용으로만 보고 넘어가도 좋다.

## 1.13 열거형의 이해
열거형 Direction이 다음과 같이 정의되어 있다고 가정하자.
```java
enum Direction { EAST, SOUTH, WEST, NORTH }
```
열거형 상수 하나하나가 Direction 객체라고 볼 수 있다.
위 문장을 클래스로 정의하면 다음과 같을 것이다.
```java
class Direction{
    static final Direction EAST = new  Direction("EAST");
    static final Direction SOUTH = new  Direction("SOUTH");
    static final Direction WEST = new  Direction("WEST");
    static final Direction NORTH = new  Direction("NORTH");
    
    private String name;
    
    private Direction(String name)    {
        this.name = name;
    }
}
```
Direction 클래스의 static 상수 EAST, SOUTH, WEST, NORTH의 값은 객체의 주소이고, 이 값은 바뀌지 않는 값이므로 ```==```로 비교가 가능한 것이다.

## 1.14 애너테이션(annotation)이란?
프로그램의 소스코드 안에 다른 프로그램을 위한 정보를 미리 약속된 형식으로 포함시킨 것이 애너테이션이다.
애너테이션은 주석(comment)처럼 프로그래밍 언어에 영향을 미치지 않으면서도 다른 프로그램에게 유용한 정보를 제공할 수 있다.

애너테이션은 JDK가 기본적으로 제공하는 것과 다른 프로그램에서 제공하는 것들이 있는데, 어느 것이든 그저 약속된 형식으로 정보를 제공하기만 하면 된다.

JDK에서 제공하는 표준 애너테이션은 주로 컴파일러를 위한 것으로 컴파일러에게 유용한 정보를 제공한다.
또한 새로운 애터네이션을 정의할 때 사용하는 메타 애너테이션을 제공한다.

## 1.15 표준 애너테이션
자바에서 기본적으로 제공하는 표준 애너테이션은 다음과 같다.
```*```가 붙은 경우는 메타 애너테이션이다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185419014-7252af2f-f9f0-40c4-a932-07cedcf6b21b.png)
<br>
### @Override
메소드 앞에만 붙일 수 있는 애너테이션으로, 조상의 메소드를 오버라이딩하는 것이라는 걸 컴파일러에게 알려주는 역할을 한다.

이는 오버라이딩을 하고자 하는 메소드의 이름이 틀렸다거나 하는 상황에 사용된다.

메소드 앞에 ```@Override```라고 애너테이션을 붙이면 컴파일러가 같은 이름의 메소드가 조상에 있는지 확인하고 없으면 에러메세지를 출력한다.

오버라이딩할 때 메소드 앞에 ```@Override```를 붙이는 것이 필수는 아니지만, 알아내기 어려운 실수를 방지해주므로 붙이는 편이 좋다.

### @Deprecated
새로운 버전의 JDK가 소개될 때 새로운 기능이 추가될 뿐만 아니라 기존의 부족했던 기능들을 개선하기도 한다.
이 과정에서 기존의 기능을 대체할 것들이 추가되어도, 이미 여러 곳에서 사용되고 있을지 모르는 기존의 것들을 함부로 삭제할 수 없다.
이러한 경우 더 이상 사용되지 않는 필드나 메소드에 ```@Deprecated```를 붙여 해당 애너테이션이 붙은 대상은 다른 것으로 대체되었으니 더 이상 사용하지 않는 것을 권한다.

```@Deprecated```가 붙은 대상을 사용하는 코드를 작성하면 컴파일할 때 특정 메세지가 출력된다.
컴파일은 문제없이 실행되며, 이는 ```@Deprecated```가 붙은 대상을 사용하지 않도록 권장할 뿐 강제성은 없기 때문이다.

### @FunctionalInterface
```함수형 인터페이스(Functional interface)```를 선언할 때, 이 애너테이션을 붙이면 컴파일러가 ```함수형 인터페이스```를 올바르게 선언했는지 확인하고, 잘못된 경우 에러를 발생시킨다.

### @SuppressWarnings
컴파일러가 보여주는 경고메시지가 나타나지 않게 억제해준다.
경고가 발생할 것을 알면서도 묵인해야 할 때 사용된다.
```@SuppressWarnings```로 억제할 수 있는 경고 메세지의 종류는 여러 가지가 있는데 ```deprecation```, ```unchecked```, ```rawtypes```, ```varargs``` 정도이다.
```
deprecation : @Deprecated가 붙은 대상을 사용해서 발생하는 경고
unchecked : 지네릭스로 타입을 지정하지 않았을 때 발생하는 경고
rawtypes : 지네릭스를 사용하지 않아서 발생하는 경고
varargs : 가변인자ㅡ이 타입이 지네릭 타입일 때 발생하는 경고
```
억제하려는 경고 메세지를 애너테이션 뒤에 괄호()안에 문자열로 지정한다.
```java
@SupressWarning("unchecked");
ArrayList list = new ArrayList();
list.add(obj); // 경고 발생
```
둘 이상의 경고를 동시에 억제할 때에는 배열에서처럼 괄호를 추가로 사용해야 한다.
```java
@SupressWarning({"unchecked", "deprecation");
ArrayList list = new ArrayList();
list.add(obj); 
```

### @SafeVarargs
메소드에 선언된 가변인자의 타입이 ```non-reifiable``` 타입일 경우 해당 메소드를 선언하는 부분과 호출하는 부분에서 unchecked 경고가 발생한다.
해당 코드가 없다면 이 경고를 억제하기 위해 ```@SafeVarargs```를 사용한다.

이 애너테이션은 static이나 final이 붙은 메소드와 생성자에만 붙일 수 있다.
즉 오바리이드될 수 있는 메소드에서는 사용할 수 없다.

컴파일 후에도 제거되지 않는 타입을 reifiable 타입이라고 하고, 제거되는 타입을 non-reifiable 타입이라고 한다.

이 애너테이션은 컴파일러에게 해당 메소드의 가변인자는 타입 안정성이 있다고 알리는 것과 같다.

메소드를 선언할 때 ```@SafeVarargs```를 붙인다면 이 메소드를 호출하는 곳에서 발생하는 경고도 억제된다.
반면에 ```@SuppressWarnings("unchecked")```로 경고를 억제하려면, 메소드 선언뿐만 아니라 메소드가 홏ㄹ되는 곳에도 애너테이션을 붙여야 한다.

또한 ```@SafeVarargs```로 ```varargs``` 경고는 억제할 수 없기 때문에 주로 ```@SuppressWarings("varargs")```와 같이 사용된다.
```java
@SafeVarargs // unchecked 경고 억제
@SuppressWarnings("varargs") // varargs 경고 억제
public static <T> List<T> asList(T... a){
    return new ArrayList<>(a);
}
```

## 1.16 메타 애너테이션
메타 애너테이션은 ```애너테이션을 위한 애너테이션```, 즉 애너테이션에 붙이는 애너테이션으로 애너테이션을 정의할 때 애너테이션의 ```적용 대상(target)```이나 ```유지 기간(retention)```등을 지정하는데 사용된다.

### @Target
애너테이션이 적용가능한 대상을 지정하는데 사용된다.

```@Target```으로 지정할 수 있는 애너테이션 적용대상의 종료는 다음과 같다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185420053-2359c891-ca35-4c1e-bbbc-dd8277fa6e68.png)
<br>

### @Retention
애너테이션이 ```유지(retention)``` 되는 기간을 지정하는데 사용된다.
애너테이션의 ```유지 정책(retention policy)```의 종류는 다음과 같다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185420256-83962c35-3a14-4f36-9a71-3b3ea2ef15d0.png)
<br>
```@Override```나 ```@SuppressWarnings``` 처럼 컴파일러가 사용하는 애너테이션은 유지 정책이 ```SOURCE```이다.

유지 정책을 ```UNTIME```으로 하면 실행 시에 리플렉션(reflection)을 통해 클래스 파일에 저장된 애너테이션의 정보를 읽어서 처리할 수 있다.

예를 들어 ```@FunctionalInterface```는 ```@override``` 처럼 컴파일러가 체크해주는 애너테이션이지만 실행 시에도 사용되므로 유지정책이 ```RUNTIME```으로 되어 있다.

유지 정책 ```CLASS```는 컴파일러가 애너테이션의 정보를 클래스 파일에 저장할 수 있게는 하지만, 클래스 파일이 JVM에 로딩될 때는 애너테이션의 정보가 무시되어 실행 시에 애너테이션에 대한 정보를 얻을 수 없다.

이것이 ```CLASS```가 유지정책의 기본값임에도 불구하고잘 사용되지 않는 이유이다.

### @Documented
애너테이션에 대한 정보가 javadoc으로 작성한 문서에 포함되도록 한다.
자바에서 제공하는 기본 애너테이션중에 @override와 @SuppressWarnings를 제외하고는 모두 이 메타 애너테이션이 붙어있다.

### @Inherited
애너테이션이 자손 클래스에 상속되도록 한다.
```@Inherited```가 붙은 애너테이션을 조상 클래스에 붙이면, 자손 클래스도 이 애너테이션이 붙은 것과 같이 인식된다.

### @Repeatable
보통은 하나의 대상에 한 종류의 애너테이션을 붙이는데, ```@Repeatable```이 붙은 애너테이션은 여러 번 붙일 수 있다.

### @Native
```네이티브 메소드(native method)```에 의해 참조되는 ```상수 필드(constant field)```에 붙이는 애너테이션이다.

다음은 ```java.lang.Long``` 클래스에 정의된 상수이다.
```java
@Native
public static final long MIN_VALUE = 0x8000000000000L;
```
네티이브 메소드는 JVM이 설치된 OS의 메소드를 의미한다.
네이티브 메소드는 보통 C언어로 작성되어 있는데, 자바에서는 메소드의 선언부만 정의하고 구현은 하지 않는다.
그래서 추상 메소드처럼 선언부만 있고 몸통이 없다.

Object 클래스의 메소드는 대부분 네이티브 메소드이며, 자바로 정의되어 있기 때문에 호출하는 방법은 자바의 일반 메소드와 다르지 않지만 실제 호출되는 것은 OS의 메소드이다.

자바에 정의된 네이티브 메소드와 OS의 메소드를 연결해주는 작업이 필요한데, 이는 ```JNI(Java Native Interface)```가 수행한다.

## 1.17 애너테이션 타입 정의하기
새로운 애너테이션을 정의하는 방법은 다음과 같다.

@ 기호를 붙이는 것을 제외하면 인터페이스를 정의하는 것과 동일하다.
```java
@interface 애너테이션이름{
    타입 요소이름(); // 애너테이션의 요소를 선언한다.
    ...
}
```
엄밀히 말해서 ```@override```는 ```애너테이션```이고 ```Override```는 ```애너테이션의 타입```이다.

### 애너테이션의 요소
애너테이션 내에 선언된 메소드를 ```애너테이션의 요소(element)```라고 한다.

애너테이션의 요소는 반환값이 있고 매개변수는 없는 추상 메소드의 형태를 가지며, 상속을 통해 구현하지 않아도 된다.
다만 애너테이션을 적용할 때 이 요소들의 값을 빠짐없이 지정해주어야 한다.
요소의 이름도 같이 적어주므로 순서는 상관없다.
```java
@TestInfo
(
    count = 3, testedBy = "Kim",
    testTools = {"JUnit", "AutoTester"},
    testType = TestType.FIRST,
    testDate = @DateTime(yymmdd = "160101", hhmmss = "235959"
)
public class Newclass { ... }
```
애너테이션의 각 요소는 기본값을 가질 수 있으며, 기본값이 있는 요소는 애너테이션을 적용할 때 값을 지정하지 않으면 기본값이 사용된다.

기본값으로는 null을 제외한 모든 리터럴이 가능하다.
```java
@interface TestInfo{
    int count() defult 1; //기본값을 1로 지정
}

@TestInfo("passwd") // @TestInfo(value = "passwd")와 동일
class NewClass { ... } 
```
요소의 타입이 배열인 경우, 괄호{}를 사용해서 여러 개의 값을 지정할 수 있다.
```java
@interface testInfo{
    String[] testTools();
}

@Test(testTools = {"JUnit", "AutoTester"}) // 값이 여러 개인 경우
@Test(testTools = "JUnit") // 값이 하나일 때는 괄호 생략가능
@Test(testTools = {}) // 값이 없을 때는 괄호{}가 반드시 필요
```
기본값을 지정할 때도 마찬가지로 괄호{}를 사용할 수 있다.
```java
@interface TestInfo{
    String[] info() default {"aaa", "bbb"};
    String[] info2() default "ccc";
}

@TestInfo // @TestInfo(info = {"aaa", "bbb"}, info2 = "ccc")와 동일
@TestInfo(info2 = {}) //  @TestInfo(info = {"aaa", "bbb"}, info2 = {} )와 동일
class NewClass { ... }
```
요소의 타입이 배열일 때도 요소의 이름이 value이면, 요소의 이름을 생략할 수 있다.
다음의 경우 요소의 타입이 String이고 이름이 value이다.
```java
@interfae SuppressWarnings{
    String[] value();
}
```
이 경우 애너테이션을 적용할 때 요소의 이름을 생략할 수 있다.
```
// @SuppressWarnings(value = {"deprecation", "unchecked"})
@SuppressWarnings({"deprecation", "unchecked"})
class NewClass { ... }
```

### java.lang.annotation.Annotation
모든 애너테이션의 조상은 Annotation이다.
그러나 애너테이션은 상속이 허용되지 않으므로 명시적으로 Annotation을 조상으로 지정할 수 없다.

또한 Annotation은 애너테이션이 아니라 일반적인 인터페이스로 정의되어 있다.
```java
puckage java.lang.annotation;

public interface Annotation{ // Annotation 자신은 인터페이스이다.
    boolean equals(Object obj);
    int hashCode();
    String toString()
    
    Class<? extends Annotation> annotationType(); // 애너테이션의 타입을 반환
}
```
모든 애너테이션의 조상인 Annotation 인터페이스는 위와 같이 정의되어 있기 때문에 모든 애너테이션 객체에 대해 equals(), hashCode(), toString()과 같은 메소드를 호출하는 것이 가능하다.

### 마커 애너테이션 Marker Annotation
값을 지정할 필요가 없는 경우, 애너테이션의 요소를 하나도 정의하지 않을 수 있다.
```Serializable```이나 ```Cloneable``` 인터페이스처럼 요소가 하나도 정의되지 않은 애너테이션을 마커 애너테이션이라고 한다.

### 애너테이션 요소의 규칙
애너테이션읭 요소를 선언할 때 반드시 지켜야 하는 규칙은 다음과 같다.
```
요소의 타입은 기본형, String, enum, 애너테이션, Class만 허용한다.
()안에 매개변수를 선언할 수 없다.
예외를 선언할 수 없다.
요소를 타입 매개변수로 정의할 수 없다.
```
