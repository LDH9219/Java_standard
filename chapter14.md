# 람다와 스트링
## 1.1 람다식
람다식은 JDK 1.8부터 도입되었다.
람다식의 도입으로 인해 자바는 객체지향언어인 동시에 함수형 언어가 되었다.

## 1.2 람다식이란?
람다식은 메소드를 하나의 식(expression)으로 표현한 것이다.
람다식은 함수를 간략하면서도 명확한 식으로 표현할 수 있게 해준다.
메소드를 람다식으로 표현하면 메소드의 이름과 반환값이 없어지므로 람다식을 ```익명 함수(anonymous function)```이라고도 한다.

```java
int[] arr = new int[5];
Arrays.setAll(arr, (i) -> (int)(Math.random() * 5) + 1);
```
위 예시의 ```() -> (int)(Math.random() * 5 ) + 1```이 람다식이며, 메소드로 표현하면 다음과 같다.
```java
int method(){
    return (int)(Math.random() * 5) + 1;
}
```
메소드의 경우 클래스에 포함되어야 하므로 클래스를 새로 만들어야 하고, 만든 클래스의 객체를 생성해야 메소드를 호출할 수 있다.
람다식의 경우 메소드의 모든 절차를 생략하고 오직 람다식 자체만으로도 이 메소드의 역할을 대신할 수 있다.

또한 람다식은 메소드의 매개변수로 전달되는 것이 가능하고, 메소드의 결과로 반환될 수 있다.
람다식으로 인해 메소드를 변수처럼 다루는 것이 가능해진 것이다.

자바에서의 메소드는 다른 프로그래밍 언어의 함수와 동일한 의미이지만, 메소드는 함수와는 달리 특정 클래스에 반드시 속해야 하므로 함수와 분리하기 위해 메소드라고 부른 것이다.
그러나 이제 람다식을 통해 메소드가 하나의 독립적인 기능을 하기 때문에 함수라는 용어를 사용할 수 있게 되었다.

## 1.3 람다식 작성하기
람다식은 익명 함수답게 메소드에서 이름과 반환타입을 제거하고 매개변수 선언부와 몸통{} 사이에 ->을 추가한다.
```
반환타입 메소드이름 (매개변수 선언){

    문장들
}

(매개변수 선언) -> { 문장들 }
```
```java
int max(int a, int b){
    return a > b ? a : b;
}

(int a, int b){ ->
    return a > b ? a : b;
}
```
반환값이 있는 메소드의 경우, return문 대신 ```식(expression)```으로 대신 할 수 있다.
식의 연산결과가 자동적으로 반환값이 된다.
아 때는 ```문장(statement)```이 아닌 식이므로 끝에 ```;```를 붙이지 않는다.
```java
(int a, int b) -> { return a > b ? a : b; }

(int a, int b) -> a > b ? a : b
```
람다식에 선언된 매개변수의 타입은 추론이 가능할 경우 생략할 수 있다.
대부분의 경우 생략 가능하다.
람다식에 반환타입이 없는 이유도 항상 추론이 가능하기 때문이다.
```java
(int a, int b) -> a > b ? a : b

(a, b) -> a > b ? a : b
```
다음과 같이 선언된 매개변수가 하나뿐인 경우에는 괄호()를 생략할 수 있다.
단 매개변수의 타입이 있으면 괄호()를 생략할 수 없다.
```java
(a) -> a * a
(int a) -> a * a

a -> a * a // OK
int a -> a * a // ERROR
```
마찬가지로 괄호{} 안의 문장이 하나일 때는 괄호{}를 생략할 수 있다.
이 때 문장의 끝에 ;를 붙이지 않아야 한다.
```java
(String name, int i) -> 
{
    System.out.println(name + " = " + i);
}

(String name, int i) ->
    System.out.println(name + " = " + i);
```
그러나 괄호{} 안의 문장이 return문일 경우 괄호{}를 생략할 수 없다.
```java
(int a, int b) -> { return a > b ? a : b; } // OK
(int a, int b) -> return a > b ? a : b // Error
```
다음 표는 메소드를 람다식으로 변환한 예시이다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185722117-7547f33b-8bf6-4f86-ac0d-3b14755205ee.png)
<br>
```java
int sumArr(int[] arr){
    int sum = 0;
    for (int i : arr)
        sum += i;
    return sum;
}

(int[] arr) ->
{
    int sum = 0;
    for (int i : arr)
        sum += i;
    return sum;
}
```
## 1.4 함수형 인터페이스(Functional Interface)
지금가지는 람다식이 메소드와 동등한 것처럼 설명했지만, 사실 람다식은 익명 클래스의 객체와 동등하다.
```
(int a, int b) -> a > b ? a : b

new Object(){
    int max(int a, int b)    {
        return a > b ? a : b;
    }
}
```
위의 두 번째 코드에서 메소드 이름 max는 임의로 붙인 것일 뿐 의미는 없다.
람다식으로 정의된 익명 객체의 메소드를 호출하기 위해서는 우선 참조변수가 있어야 한다.
```java
타입 f = (int a, int b) -> a > b ? a : b
```
그러나 위와 같이 참조변수 f의 타입을 어떤 것으로 둘 지가 문제가 된다.
이 경우 인터페이스를 이용한 익명 클래스로 처리할 수 있다.
```java
interface MyFunction{
    public abstract int max(int a, int b);
}

MyFunction f = new MyFunction() {
                   public int max(int a, int b)                   {
                       return a > b ? a : b;
                   }
               };

int big = f.max(5, 3); // 익명 객체의 메소드를 호출
```
MyFunction 인터페이스에 정의된 메소드 max()는 람다식 (int a, int b) -> a > b ? a : b과 메소드의 선언부가 일치한다.
그러므로 위 예제의 익명 객체를 람다식으로 아래와 같이 대체할 수 있다.

```java
MyFunction f = (int a, int b) -> a > b ?  a : b; // 익명 객체를 람다식으로 대체
int big = f.max(5, 3); // 익명 객체의 메소드 호출
```
이처럼 MyFunction 인터페이스를 구현한 익명 객체를 람다식으로 대체가 가능한 이유는 람다식도 실제로는 익명 객체이고, MyFunction 인터페이스를 구현한 익명 객체의 메소드 max()와 람다식의 매개변수의 타입과 개수, 반환값이 일치하기 때문이다.

위와 같이 인터페이스를 통해 람다식을 다루기로 하며, 람다식을 다루기 위한 인터페이스를 함수형 인터페이스(functional interface)라고 부른다.
```java
@FunctionalInterface
interface MyFunctional {// 함수형 인터페잇 MyFunction을 정의
    public abstract int max(int a, int b);
}
```
단 함수형 인터페이스에는 오직 하나의 추상 메소드만 정의되어야 한다.
그래야 람다식과 인터페이스의 메소드가 1:1로 연결될 수 있기 때문이다.
반면에 static 메소드와 default 메소드의 개수는 제약이 없다.

또한 @FunctionalInterface는 컴파일러가 함수형 인터페이스를 올바르게 정의했는지 확인하므로 붙여주는 편이 좋다.
```java
List<String> list = Arrays.asList("abc", "aaa", "bbb", "ddd", "aaa");

Collections.sort(list, new Comparator<String>() {
    public int compare(String s1, String s2)    {
        return s2.compareTo(s1);
    }
});
```
위와 같은 코드를 람다식으로 표현하면 다음과 같이 표현할 수 있다.
```java
List<String> list = Arrays.asList("abc", "aaa", "bbb", "ddd", "aaa");
Collections.sort(list, (s1, s2) -> s2.compareTo(s1));
```

### 함수형 인터페이스 타입의 매개변수와 반환타입
다음과 같은 함수형 인터페이스 MyFunction이 정의되어 있다고 가정하자.
```java
@FunctionalInterface
interface MyFunction{
    void myMethod(); // 추상 메소드
}
```
메소드의 매개변수가 MyFunction 타입이면 이 메소드를 호출할 때 람다식을 참조하는 참조변수를 매개변수로 지정해야 한다는 의미이다.

```java
void aMethod(MyFunction f) {// 매개변수의 타입이 함수형 인터페이스
    f.myMethod(); /// MyFunction에 정의된 메소드 호출
}

MyFunction f = () -> System.out.println("myMethod()");
aMethod(f);
```
또는 참조변수 없이 다음과 같이 같이 직접 람다식을 매개변수로 지정하는 것도 가능하다.
```java
aMethod(() -> System.out.println("myMethod()")); // 람다식을 매개변수로 지정
```
또한 메소드의 반환타입이 함수형 인터페이스라면 이 함수형 인터페이스의 추상메소드와 동등한 람다식을 가리키는 참조변수를 반환하거나 람다식을 직접 반환할 수 있다.
```java
MyFunction myMethod(){
    MyFunction f = () -> {};
    return f;
}

MyFunction myMethod(){
    return () -> {};
}
```
람다식을 참조변수로 다룰 수 있다는 것은 메소드를 통해 람다식을 주고받을 수 있다는 것을 의미한다.
즉 변수처럼 메소드를 주고받는 것이 가능해진 것이다.
사실상 메소드가 아니라 객체를 주고받는 것이라 근본적으로 달라진 것은 없지만 람다식 덕분에 예전보다 코드가 더 간결하고 이해하기 쉬워졌다.

### 람다식의 타입과 형변환
함수형 인터페이스로 람다식을 참조할 수 있는 것일 뿐, 람다식의 타입이 함수형 인터페이스의 타입과 일치하는 것은 아니다.

람다식은 익명 객체이며, 익명 객체는 타입이 없기 때문이다.
정확히 표현하자면 타입은 있지만 컴파일러가 임의로 이름을 정하기 때문에 알 수 없다.

그러므로 대입 연산자의 양변의 타입을 일치시키기 위해 다음과 같이 형변환이 필요하다.
```java
@FuntionalInterface
interface MyFunction{
    void method();
}

MyFunction f = (MyFunction) ( ()->{} ); // 양변의 타입이 다르므로 형변환 필요
```
람다식은 MyFunction 인터페이스를 직접 구현하지 않았지만, 이 인터페이스를 구현한 클래스의 객체와 완전히 동일하기 때문에 위와 같은 형변환을 허용한다.
또한 이 형변환은 생략 가능하다.

람다식은 이름이 없을 뿐 분명히 객체인데도, 다음과 같이 Object 타입으로 형변환을 할 수 없다.
```java
Object obj = (Object)( ()->{} ); // ERROR
```
Object로 형변환을 하고자 한다면, 먼저 함수형 인터페이스로 변환해야 한다.
```java
Object obj = (Object)(MyFunction)( () -> {} );
String str = ( (Object)(MyFunction) ( () -> {} ) ).toString();
```
람다식은 오직 함수형 인터페이스로만 형변한이 가능하다.

### 외부 변수를 참조하는 람다식
람다식도 익명 객체, 즉 익명 클래스의 인스턴스이므로 람다식에서 외부에 선언된 변수에 접근하는 규칙은 익명 클래스와 동일하다.
```java
@FunctionalInterface
interface MyFunction3 {
	void myMethod();
}

class Outer {
	int val=10;	// Outer.this.val				

	class Inner {
		int val=20;	// this.val

		void method(int i) {  // 	void method(final int i) {
			int val=30; // final int val=30;
//			i = 10;      // 에러. 상수의 값을 변경할 수 없음.

			MyFunction3 f = () -> {
				System.out.println("             i :" + i);
				System.out.println("           val :" + val);
				System.out.println("      this.val :" + ++this.val);
				System.out.println("Outer.this.val :" + ++Outer.this.val);	
			};

			f.myMethod();
		}
	} // Inner클래스의 끝
} // Outer클래스의 끝

class LambdaEx3 {
	public static void main(String args[]) {
		Outer outer = new Outer();
		Outer.Inner inner = outer.new Inner();
		inner.method(100);
	}
}
```
위 예제는 람다식에서 외부에 선언된 접근하는 규칙을 확인하기 위한 예제이며, 실행 결과는 다음과 같다.
```
             i :100
           val :30
      this.val :21
Outer.this.val :11
```
람다식 내에서 참조하는 지역변수는 final이 붙지 않아도 상수로 간주된다.
람다식 내에서 지역변수 i와 val을 참조하고 있으므로 람다식 내에서나 다른 어느 곳에서도 이 변수들의 값을 변경할 수 없다.

Inner 클래스와 Outer 클래스의 인스턴스 변수인 this.val과 Outer.this.val은 상수로 간주되지 않으므로 값을 변경할 수 있다.

또한 외부 지역변수와 같은 이름의 람다식 매개변수는 허용하지 않는다.

## 1.5 java.util.function 패키지
대부분의 메소드는 메소드 선언부가 비슷하다.
매개변수의 경우 없거나 하나에서 두 개, 반환 값은 없거나 하나이다.
게다가 지네릭 메소드로 정의하면 매개변수나 반환 타입이 달라도 문제가 되지 않는다.

가능하다면 매번 새로운 함수형 인터페이스를 정의하는 것이 아닌, java.util.function 패키지의 인터페이스를 활용하는 편이 좋다.

그래야 함수형 인터페이스에 정의된 메소드 이름도 통일되고, 재사용성이나 유지보수 측면에서도 좋다.

자주 쓰이는 가장 기본적인 함수형 인터페이스는 다음과 같다.
매개변수와 반환값의 유무에 따라 4개의 함수형 인터페이스가 정의되어 있다.
Function의 변형으로 Predicate가 있는데, 반환값이 boolean이라는 것만 제외하면 Function과 동일하다.

타입 문자 T는 Type을, R은 Return Type을 의미한다.

### 조건식의 표현에 사용되는 Predicate
Predicate는 조건식을 람다식으로 표현하는데 사용된다.
```java
Predicate<String> isEmptyStr = s -> s.length() == 0;
String s = "";

if (isEmptyStr.test(s)) // if(s.length() == 0)
    System.out.println("This is an empty String.");
```

### 매개변수가 두 개인 함수형 인터페이스
매개변수의 개수가 2개인 함수형 인터페이스는 이름 앞에 접두사 Bi가 붙는다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185722523-4c75289f-6ed7-44f9-8552-5af3f8cabac1.png)
<br>
두 개 이상의 매개변수를 갖는 함수형 인터페이스가 필요하다면 직접 정의해서 사용해야 한다.
```java
@FunctionalInterface
interface TriFunction<T, U, V, R>{
    R apply(T t, U u, V v);
}
```

### UnaryOperator와 BinaryOperator
Function의 또 다른 변형으로 UnaryOperator와 BinaryOperator가 있는데, 매개변수의 타입과 반환타입의 타입이 모두 일치한다는 점만 제외하고는 Function과 같다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185722540-7f0e246e-23a5-4a71-9f02-9a42eb6cdf37.png)
<br>

### 컬렉션 프레임웤과 함수형 인터페이스
컬렉션 프레임웤의 인터페이스에 다수의 디폴트 메소드가 추가되었는데, 그 중의 일부는 함수형 인터페이스를 사용한다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185722549-6eee6fae-765e-4468-87c5-05a74fa7560b.png)
<br>
Map 인터페이스에 있는 compute로 시작하는 메소드들은 맵의 value를 변환하는 일을 하고 merge()는 Map을 병합하는 일을 한다.

### 기본형을 사용하는 함수형 인터페이스
이전의 함수형 인터페이스는 매개변수와 반환값이 타입이 모두 지네릭 타입이다 보니 기본형 타입의 값을 처리할 때 래퍼클래스를 사용했다.
기본형 대신 래퍼클래스를 사용하는 것은 비효율적이므로 기본형을 사용하는 함수형 인터페이스들이 제공된다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185722566-c64dfe57-93da-40b8-b064-30c22be66f7d.png)
<br>

## 1.6 Function의 합성과 Predicate의 결합
java.util.function 패키지의 함수형 인터페이스에는 추상메소드 외에도 디폴트 메소드와 static 메소드가 정의되어 있다.

대부분의 함수형 인터페이스의 메소드는 유사하므로, Function과 Predicate에 정의된 메소드만 살펴봐도 충분하다.
```java
// Function
default <V> Function<T, V> andThen(Function<? super R, ? extends V> after)
default <V> Function<V, R> compose(Function<? super V, ? extends T> before>
static <T> Function<T, T> identity()

// Predicate
default Predicate<T> and(Predicate<? super T> other)
default Predicate<T> or(Predicate<? super T> other)
default Predicate<T> negate()
static <T> Predicate<T> isEqual(Object targetRef)
```
### Function의 합성
두 람다식을 합성해서 새로운 람다식을 만들 수 잇다.
두 함수의 합성은 어느 함수를 먼저 적용하느냐에 따라 달라진다.
함수 f, g가 있을 때 f.andThen(g)는 함수 f를 먼저 적용하고, 그 다음에 함수 g를 적용한다.
f.compose(g)는 반대로 g를 먼저 적용하고 f를 적용한다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185722610-1ce95059-9743-438e-9d9c-5813f3c2ecb0.png)
<br>
예를 들어 문자열을 숫자로 변환하는 함수 f와 숫자를 2진 문자열로 변환하는 함수 g를 andThen()으로 합성하여 새로운 함수 h를 만들어낼 수 있다.
```java
Function<String, Integer> f = (s) -> Integer.parseInt(s, 16);
Function<Integer, String> g = (i) -> Integer.toBinaryString(i);
Function<String, String> h = f.andThen(g);
```
함수 h의 지네릭 타입이 <String, String>이다.
즉 String을 입력 받아서 String을 결과로 반환한다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185722629-7132d9df-e9d5-4cbc-b39d-2841cbe2fcea.png)
<br>
```java
Function<String, Integer> f = (s) -> Integer.parseInt(s, 16);
Function<Integer, String> g = (i) -> Integer.toBinaryString(i);
Function<Integer, Integer> h = f.compose(g);
```
위 코드는 이전과 달리 함수 h의 지네릭 타입이 <Integer, Integer>이다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185722641-2373596b-2b51-4756-9065-b3f32995fdde.png)
<br>
identify()는 함수를 적용하기 이전과 이후가 동일한 항등 함수가 필요할 때 사용한다.
```java
Function<String, String> f = x -> x;
Function<String, String> f = Function.identity(); // 위 문장과 동일
```
identity()는 잘 사용되지 않으며, map()으로 변환작업을 할 때 변환없이 그대로 처리하고자할 때 사용된다.

### Predicate의 결합
여러 Predicate를 and(), or(), negate()로 연결해서 하나의 새로운 Predicate로 결합할 수 있다.
```java
Predicate<Integer> p = i -> i < 100;
Predicate<Integer> q = i -> i < 200;
Predicate<Integer> r = i -> i % 2 == 0;
Predicate<Integer> notP = p.negate(); // i >= 100;

// 100 <= i && (i < 200 || i % 2 == 0)
Predicate<Integer> all = notP.and(q.or(r));
System.out.println(all.test(150)); // true;
```
위 코드를 다음과 같이 람다식으로 표현해도 된다.
```java
Predicate<Integer> all = notP.and(i -> i < 200).or(i -> i % 2 == 0);
```
Predicate의 끝에 negate()를 붙이면 조건식 전체가 부정이 된다는 점을 주의해야 한다.

static 메소드인 isEqual()은 두 대상을 비교하는 Predicate를 만들 때 사용한다.
```java
Predicate<String> p = Predicate.isEqual(str1);
boolean result = p.test(str2);

boolean result = Predicate
```
위 코드와 같이 isEqual()의 매개변수로 비교대상을 하나 지정하고, 또 다른 비교대상은 test()의 매개변수로 지정한다.
```java
boolean result = Predicate.isEqual(str1).test(str2);
```
위와 같이 체인 패턴으로 합칠 수 있다.

## 1.7 메소드 참조
람다식이 하나의 메소드만 호출하는 경우에는 메소드 참조(method reference)라는 방식으로 람다식을 간략히 할 수 있다.
```java
Function<String, Integer> f = (String s) -> Integer.parseInt(s);
```
위 람다식을 메소드로 표현하면 다음과 같다.
```java
Integer wrapper(String s){ // 메소드의 이름은 의미없다.
    return Integer.parseInt(s);
}
```
이 경우 그저 값을 받아서 Integer.parseInt()에게 넘겨주는 일만 하므로, 곧장 Integer.parseInt()를 호출할 수 있다.
```java
Function<String, Integer> f = (String s) -> Integer.parseInt(s);

Function<String, Integer> f = Integer::parseInt; // 메소드 참조
```
컴파일러는 생략된 부분을 우변의 parseInt 메소드의 선언부로부터, 또는 좌변의 Function 인터페이스에 지정된 지네릭 타입으로부터 유추할 수 있다.
```java
BiFunction<String, String, Boolean> f = (s1, s2) -> s1.equals(s2);

BiFunction<String, String, Booelan> f = String::equals; // 메소드 참조
```
참조변수의 f를 보면 람다식이 두 개의 String 타입의 매개변수를 받는것을 알 수 있다.
그러므로 람다식의 매개변수들은 없앨 수 있다.
그러면 equals() 메소드만 존재하게 되는데, equals()라는 이름의 메소드는 다른 클래스에도 존재할 수 있기 때문에 equals() 메소드 앞에 클래스 이름은 반드시 필요하다.

메소드 참조를 사용할 수 있는 경우가 한 가지 더 있는데, 이미 생성된 객체의 메소드를 람다식에서 사용한 경우에는 클래스 이름 대신 그 객체의 참조변수를 적어줘야 한다.
```java
MyClass obj = new MyClass();
Function<String, boolean> f = (x) -> obj.equals(x); // 람다식
Function<STring, boolean> f2 = obj::equals; // 메소드 참조
```
### 정리
![image](https://user-images.githubusercontent.com/62749021/185722758-aa9c3add-fad6-4b67-9be0-b00966334827.png)
<br>

### 생성자의 메소드 참조
생성자를 호출하는 람다식도 메소드 참조로 변환할 수 있다.
```
Supplier<MyClass> s = () -> new MyClass(); // 람다식
Supplier<MyClass> x = MyClass::new; // 메소드 참조
```
매개변수가 있는 생성자라면 매개변수의 개수에 따라 알맞은 함수형 인터페이스를 사용하면 된다.
필요하다면 함수형 인터페이스를 새로 정의해야 한다.
```java
Function<Integer, MyClass> f = (i) -> new MyClass(i); // 람다식
Function<Integer, MyClass> f2 = MyClass::new; // 메소드 참조

BiFunction<Integer, String, MyClass> bf = (i, s) -> new MyClass(i, s); // 람다식
BiFunction<Integer, String, MyClass> bf2 = MyClass::new; // 메소드 참조
```
배열 생성은 다음과 같다.
```java
Function<Integer, int[]> f = x-> new int[x]; // 람다식
Function<Integer, int[]> f2 = int[]::new; // 메소드 참조
```
메소드 참조는 람다식을 마치 static 변수처럼 다룰 수 있게 해준다.
메소드 참조는 코드를 간략히 하는데 유용해서 자주 사용된다.

## 1.8 스트림이란?
스트림(stream)은 데이터 소스를 추상화하고 데이터를 다루는데 자주 사용되는 메소드들을 정의해 놓았다.

데이터 소스를 추상화했다는 것은 데이터 소스가 무엇이던 간에 같은 방식으로 다룰 수 있게 되었다는 의미이므로 코드의 재사용성이 높아진다는 것을 의미한다.

스트림을 이용하면, 배열이나 컬렉션뿐만 아니라 팡리에 저장된 데이터도 모두 같은 방식으로 다룰 수 있다.

또한 코드가 간결해지고 이해하기 쉬워진다.
```java
String[] strArr = { "aaa", "ddd", "ccc" };
List<String> strList = Arrays.asList(strArr);

Arrays.sort(strArr);
Collections.sort(strList);

for(String str : strArr){
    System.out.println(str);
}

for(String str : strList){
    System.out.println(str);
}
```
```java
String[] strArr = { "aaa", "ddd", "ccc" };
List<String> strList = Arrays.asList(strArr);

Stream<String> strStream1 = strList.stream(); // 스트림을 생성
Stream<String> strStream2 = Arrays.stream(strArr); // 스트림을 생성

strStream1.sorted().forEach(System.out::println);
strStream2.sorted().forEach(System.out::println);
```

### 스트림은 데이터 소스를 변경하지 않는다.
스트림은 데이터 소스로부터 데이터를 읽기만 할 뿐, 데이터 소스를 변경하지 않는다.

필요한 경우 정렬된 결과를 컬렉션이나 배열에 담아서 반환할 수 있다.
```java
// 정렬된 결과를 새로운 List에 담아서 반환한다.
List<String> sortedList = strStream2.sorted().collect(Collections.toList());
```

### 스트림은 일회용이다.
스트림은 Iterator처럼 일회용이다.
Iterator로 컬렉션의 요소를 모두 읽고 나면 다시 사용할 수 없는 것처럼, 스트림도 한 번 사용하면 닫혀서 다시 사용할 수 없다.
필요하다면 스트림을 다시 생성해야한다.
```java
strStream1.sorted().forEach(System.out::println);
int numOfStr = strStream1.count(); // 에러. 스트림이 이미 닫혔음.
```

### 스트림은 작업을 내부 반복으로 처리한다.
스트림을 이용한 작업이 간결할 수 있는 이유는 내부 반복 때문이다.
내부 반복이라는 것은 반복문을 메소드의 내부에 숨길 수 있다는 것을 의미한다.
forEach()는 스트림에 정의된 메소드 중에 하나로 매개변수에 대입된 람다식을 데이터 소스의 모든 요소에 적용한다.
```java
for(String str : strList)
    System.out.println(str);
    
stream.forEach(System.out::println);
```
즉 forEach()는 메소드 안으로 for문을 넣은 것이다.
수행할 작업은 다음과 같이 매개변수로 받는다.
```java
void forEach(Consumer<? super T> action){
    Objects.requireNonNull(action); // 매개변수의 널 체크
    
    for(T t : src){ // 내부 반복
        action.accept(T);
    }
}
```
### 스트림의 연산
스트림이 제공하는 다양한 연산을 이용해서 복잡한 작업들을 간단히 처리할 수 있다.

스트림에 정의된 메소드 중에서 데이터 소스를 다루는 작업을 수행하는 것을 연산(operator)이라고 한다.

스트림이 제공하는 연산은 중간 연산과 최종 연산으로 분류할 수 있는데, 중간 연산은 연산결과를 스트림으로 반한하기 때문에 중간 연산을 연속해서 연결할 수 있다.
반면에 최종 연산은 스트림의 요소를 소모하면서 연산을 수행하므로 단 한번만 연산이 가능하다.

```
중간 연산 : 연산 결과가 스트림인 연산. 스트림에 연속해서 중간 연신할 수 있음
최종 연산 : 연산 결과가 스트림이 아닌 연산. 스트림의 요소를 소모하므로 단 한번만 가능
```
스트림에 정의된 연산은 다음과 같다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185722926-4769c3a0-1fdb-409b-a48b-15ce35e0929a.png)
<br>
![image](https://user-images.githubusercontent.com/62749021/185722932-a4ef5f8d-8d74-4b59-8fe1-12084b163e85.png)
<br>
중간 연산은 map()과 flatMap(), 최종 연산은 reduce()와 collect()가 핵심이다.

### 지연된 연산
스트림 연산에서 한 가지 중요한 점은 최종 연산이 수행되기 전까지는 중간 연산이 수행되지 않는다는 것이다.
스트림에 대해 distinct()나 sort()같은 중간 연산을 호출해도 즉각적인 연산이 수행되는 것은 아니다.
중간 연산을 호출하는 것은 단지 어떤 작업이 수행되어야하는지를 지정해주는 것일 뿐이다.
최종 연산이 호출되어야 비로소 스트림의 요소들이 중간 연산을 거쳐 최종 연산에서 소모된다.

### ```Stream<Integer>```와 IntStream
요소의 타입이 T인 스트림은 기본적으로 ```Stream<T>```이지만, 오토박싱 & 언박싱으로 인한 비효율을 줄이기 위해 데이터 소스의 요소를 기본형으로 다루는 스트림, IntStream, LongStream, DoubleStream이 제공된다.
일반적으로 ```Stream<Integer>``` 대신 IntStream을 사용하는 것이 더 효율적이며, 작업하는데 유용한 메소드들이 포함되어 있다.

### 병렬 스트림
스트림으로 데이터를 다룰 때의 장점 중 하나가 바로 병렬 처리가 쉽다는 것이다.
병렬 스트림은 내부적으로 fork&join 프레임웤을 사용하여 자동적으로 연산을 병렬로 수행한다.
스트림에 parallel()이라는 메소드를 호출해서 병렬로 연산을 수행하도록 지시하면 된다.
```java
int sum = strStream.parallel() // strStream을 병렬 스트림으로 전환
                   .mapToInt(s -> s.length())
                   .sum();
```
반대로 병렬로 처리디지 않게 하려면 sequential()을 호출하면 된다.
모든 스트림은 기본적으로 병렬 스트림이 아니므로 sequential()은 parallel()을 호출한 것을 취소할 때만 사용한다.

## 1.9 스트림 만들기
스트림의 소스가 될 수 있는 대상은 배열, 컬렉션, 임의의 수 등으로 다양하다.

### 컬렉션
컬렉션의 최소 조상인 Collection에 stream()이 정의되어 있다.
그러므로 Collection의 자손인 List와 Set을 구현한 컬렉션 클래스들은 모두 이 메소드로 스트림을 생성할 수 있다.
```java
Stream<T> Collection.stream()
```
List로부터 스트림을 생성하는 코드는 다음과 같다.
```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5); // 가변인자
Stream<Integer> intStream = list.stream(); // list를 소스로 하는 컬렉션 생성
```
forEach()는 지정된 작업을 스트림의 모든 요소에 대해 수행한다.
다음 코드는 스트림의 모든 요소를 화면에 출력한다.
```java
intStream.forEach(System.out::println); // 스트림의 모든 요소를 출력한다.
intStream.forEach(System.out::println); // 에러. 스트림이 이미 닫혔다.
```
### 배열
배열을 소스로 하는 스틀미을 메소드는 다음과 같이 Stream과 Arrays에 static 메소드로 정의되어 있다.
```java
Stream<T> Stream.of(T... values) // 가변 인자
Stream<T> Stream.of(T[])
Stream<T> Arrays.stream(T[])
Stream<T> Arrays.stream(T[] array, int startInclusive, int endExclusive)
```
예를 들어 문자열 스트림은 다음과 같이 생성한다.
```java
Stream<String> strStream = Stream.of("a", "b", "c"); // 가변 인자
Stream<String> strStream = Stream.of(new String[]{"a", "b", "c"});
Stream<String> strStream = Arrays.stream(new String[]{"a", "b", "c"});
Stream<String> strStream = Arrays.stream(new String[]{"a", "b", "c"}, 0, 3);
```
기본형 배열을 소스로 하는 스트림의 메소드는 다음과 같다.
```java
IntStream IntStream.of(int... values) // Stream이 아니라 IntStream
IntStream IntStream.of(int[])
IntStream Arrays.stream(int[])
IntStream Arrays.stream(int[] array, int startInclusive, int endExclusive)
```

### 특정 범위의 변수
IntStream과 LongSTream은 다음과 같이 지정된 범위의 연속된 정수를 스트림으로 생성해서 반환할 수 있다.
```java
IntStream IntStream.range(int begin, int end)
IntStream IntStream.rangeClosed(int begin, int end)
```
range()의 경우 경계의 끝인 end가 범위에 포함되지 않고, rangeClosed()의 경우는 포함된다.
```java
IntStream intStream = IntStream.range(1, 5); // 1, 2, 3, 4
IntStream intStream = IntStream.rangeClosed(1, 5); // 1, 2, 3, 4, 5
```
int보다 큰 범위의 스트림을 생성하려면 LongStream에 있는 동일한 이름의 메소드를 사용하면 된다.

### 임의의 수
난수를 생성하는데 사용하는 Random 클래스에는 다음과 같은 인스턴스 메소드들이 포함되어 있다.
```java
IntStream ints()
LongStream longs()
DoubleStream doubles()
```
이 메소드들은 해당 타입의 난수들로 이루어진 스트림을 반환한다.
이 메소드들이 반환하는 스트림은 크기가 정해지지 않은 무한 스트림(infinite stream)이므로 limit()도 같이 사용해서 스트림의 크기를 제한해 주어야 한다.

limit()는 스트림의 개수를 지정하는데 사용되며, 무한 스트림을 유한 스트림으로 만들어 준다.
```java
IntStream intStream = new Random().ints(); // 무한 스트림
intStream.limit(5).forEach(System.out::println); // 5개의 요소만 출력한다.
```
다음 메소드들은 매개변수로 스트림의 크기를 지정해서 유한 스트림을 생성해서 반환하므로 limit()를 사용하지 않아도 된다.
```java
IntStream ints(long streamSize)
LongStream longs(long streamSize)
DoubleStream doubles(long streamSize)

IntStream intStream = new Random.ints(5); // 크기가 5인 난수 스트림을 반환
```
위 메소드들에 의해 생성된 스트림의 난수는 다음과 같은 범위를 갖는다.
```java
Integer.MIN_VALUE <= ints() <= Integer.MAX_VALUE
Long.MIN_VALUE <= longs() <= Long.MAX_VALUE
0.0 <= doubles() < 1.0
```
지정된 범위(begin - end)의 난수를 발생시키는 스트림을 얻는 메소드는 다음과 같다.
단, end는 범위에 포함되지 않는다.
```java
IntStream ints(int begin, int end)
LongStream longs(long begin, long end)
DoubleStream doubles(double begin, double end)
IntStream ints(long streamSize, int begin, int end)
LongStream longs(long streamSize, long begin, long end)
DoubleStream doubles(long streamSize, double begin, double end)
```

### 람다식 - iterate(), generate()
Stream 클래스의 iterate()와 generate()는 람다식을 매개변수로 받아서 이 람다식에 의해 계산되는 값들을 요소로 하는 무한 스트림을 생성한다.
```java
static <T> Stream<T> iterate(T seed, UnaryOperator<T> f)
static <T> STream<T> generate(Supplier<T> s)
```
iterate()는 씨앗값(seed)으로 지정된 값부터 시작해서, 람다식 f에 의해 계산된 결과를 다시 seed값으로 해서 계산을 반복한다.

다음 evenStream은 0부터 시작해서 값이 2씩 계속 증가한다.
```java
Stream<Integer> evenStream = Stream.iterate(0, n -> n + 2); // 0, 2, 4, 6...
```
generate()도 iterate()처럼 람다식에 의해 계산되는 값을 요소로 하는 무한 스트림을 생성해서 반환하지만, iterate()와 달리 이전 결과를 이용해서 다음 요소를 계산하지는 않는다.
```java
Stream<Double> randomStream = Stream.generate(Math::random);
Stream<Integer> oneStream = Stream.generate(() -> 1);
```
generate()에 정의된 매개변수의 타입은 ```Supplier<T>```이므로 매개변수가 없는 람다식만 허용된다.
한 가지 주의할 점은 iterate()와 generate()에 의해 생성된 스트림을 아래와 같이 기본형 스트림 타이브이 참조변수로 다룰 수 없다는 것이다.
```java
IntStream evenStream = Stream.iterate(0, n -> n + 2); // 에러.
DoubleStream randomStream = Stream.generate(Math::random); // 에러.
```
굳이 필요하다면 다음과 같이 mapToInt()와 같은 메소드로 변환을 해야 한다.
IntStream 타입의 스트림을 ```Stream<Integer>``` 타입으로 변경하려면 boxed()를 사용하면 된다.
```java
IntStream evenStream = stream.iterate(0, n -> n + 2).mapToInt(Integer::valueOf);
Stream<Integer> stream = evenSTream.boxed(); // IntStream -> Stream<Integer>
```

### 파일
java.nio.file.Files는 파일을 다루는데 필요한 메소드들을 제공하는데, list()는 지정된 디렉토리(dir)에 있는 파일의 목록을 소스로 하는 스트림을 생성해서 반환한다.
```java
Stream<Path> Files.list(Path dir)
```
파일의 한 행(line)을 요소로 하는 스트림을 생성하는 메소드 또한 존재한다.
```java
Stream<String> Files.lines(Path path)
Stream<String> Files.lines(Path path, Charset cs)
Stream<String> lines() // BufferedReader 클래스의 메소드
```
위 메소드들은 BufferedReader 클래스에 속한 메소드인데, 파일 뿐만 아니라 다른 입력대상으로부터도 데이터를 행단위로 읽어올 수 있다.

### 빈 스트림
요소가 하나도 없는 비어있는 스트림을 생성할 수도 있다.
스트림에 연산을 수행한 결과가 하나도 없을 때, null보다 빈 스트림을 반환하는 것이 낫다.
```java
Stream emptyStream = Stream.empty(); // empty()는 빈 스트림을 생성해서 반환한다.
long count = emptyStream.count(); // count의 값은 0
```
count()는 스트림 요소의 개수를 반환하므로 변수 count의 값은 0이 된다.

### 두 스트림의 연결
Stream의 static 메소드인 concat()을 사용하면, 두 스트림을 하나로 연결할 수 있다.
연결하려는 두 스트림의 요소는 같은 타입이어야 한다.
```java
String[] str1 = {"123", "456", "789"};
String[] str2 = {"ABC", "abc", "DEF"};

Stream<String> strs1 = Stream.of(str1);
Stream<String> strs2 = Stream.of(str2);
Stream<String> strs3 = Stream.concat(strs1, strs2); // 두 스트림을 하나로 연결
```

## 1.10 스트림의 중간연산
### 스트림 자르기 - skip(), limit()
skip()과 limit()는 스트림의 일부를 잘라낼 때 사용한다.
skip(3)은 처음 3개의 요소를 건너뛰고, limit(5)는 스트림의 요소를 5개로 제한한다.
```java
Stream<T> skip(long n)
Stream<T> limit(long maxSize)
```
예를 들어 10개의 요소를 가진 스트림에 skip(3)과 limit(5)를 순서대로 적용하면 4번재 요소부터 5개의 요소를 가진 스트림이 반환된다.
```java
IntStream intStream = IntStream.rangeClosed(1, 10); // 1 ~ 10의 요소를 가진 스트림
intStream.skip(3).limit(5).forEach(System.out::print); // 45678
```
기본형 스트림에도 skip()과 limit()가 정의되어 있는데, 반환 타입이 기본형 스트림이라는 점을 제외하고는 동일하다.
```java
IntStream skip(long n)
IntStream limit(long maxSize)
```

### 스트림의 요소 걸러내기 - filter(), distinct()
distinct()는 스트림에서 중복된 요소들을 제거하고, filter()는 주어진 조건(Predicate)에 맞지 않는 요소를 걸러낸다.
```java
Stream<T> filter(Predicate<? super T> predicate)
Stream<T> distinct()
```
distinct()의 사용 방법은 다음과 같다.
```java
IntStream intStream = IntStream.of(1, 2, 2, 3, 3, 3, 4, 5, 5, 6);
intStream.distinct().forEach(System.out::print); // 123456
```
filter()는 매개변수로 Predicate를 필요로 한다.
```java
IntStream intStream = IntStream.rangeClosed(1, 10); // 1 ~ 10
intStream.filter(i -> i % 2 == 0).forEach(System.out::print); // 246810
```
위 코드와 같이 연산결과가 boolean인 람다식을 사용할 수 있으며, 필요하다면 filter()를 다른 조건으로 여러 번 사용할 수도 있다.
```java
// 아래의 두 문장은 동일한 결과를 얻는다.
intStream.filter(i -> i % 2 != 0 && i % 3 != 0).forEach(Systsem.out::print); // 157
intStream.filter(i -> i % 2 != 0).filter(i -> i % 3 != 0).forEach(System.out::print);
```

### 정렬 - sorted()
스트림을 정렬할 때에는 sorted()를 사용한다.
```java
Stream<T> sorted()
Stream<T> sorted(Comparator<? super T> comparator)
```
sorted()는 지정된 Comparator로 스트림을 정렬하는데, Comparator 댓니 int값을 반환하는 람다식을 사용하는 것도 가능하다.
Comparator를 지정하지 않으면 스트림 요소의 기본 정렬 기준(Comparable)으로 정렬한다.
단 스트림의 요소가 Comparable을 구현한 클래스가 아니라면 예외가 발생한다.
```java
Stream<String> strStream = Stream.of("dd", "aaa", "CC", "cc", "b");
strStream.sorted().forEach(System.out::print); // CCaaabccdd
```
위 코드는 문자열 스트림을 String에 정의된 기본 정렬(사전순 정렬)로 정렬해서 출력한다.

다음 표는 문자열 스트림(strStream)을 다양한 방법으로 정렬한 후에 forEach(System.out::print)로 출력한 결과이다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185738327-8e6334d8-2eaf-4686-b0eb-16d9afdce4fe.png)
<br>
JDK 1.8부터 Comparator 인터페이스에 추가된 staitc 메소드와 디폴트 메소드를 이용하면 손쉽게 정렬을 할 수 있다.
이 메소드들은 모두 ```Comparator<T>```를 반환한다.

다음은 지네릭에서 와일드카드를 제거하여 간단히 한 메소드들이다.
```java
// Comparator의 default 메소드
reverse()
thenComparing(Comparator<T> other)
thenComparing(Function<T, U> keyExtractor)
thenComparing(Function<T, U> keyExtractor, Comparator<U> keyComp)
thenComparingInt(ToIntFunction<T> keyExtractor)
thenComparingLong(ToLongFunction<T> keyExtractor)
thenComparingDouble(ToDoubleFunction<T> keyExtractor)

// Comparator의 static 메소드
naturalOrder()
reverseOrder()
comparing(Function<T, U> keyExractor)
comparing(Function<T, U> keyExtractor, Comparator<U> keyComparator)
comparingInt(ToIntFunction<T> keyExtractor)
comparingLong(ToLongFUnction<T> keyExtractor)
comparingDouble(ToDoubleFunction<T> keyExtractor)
nullsFirst(Comparator<T> comparator)
nullsLast(Comparator<T> comparator)
```
정렬에 사용되는 메소드의 개수가 많지만, 가장 기본적인 메소드는 comparing()이다.
```java
comparing(Function<T, U> keyExtractor)
comparing(Function<T, U> keyExtractor, Comparator<U> keyComparator)
```
스트림의 요소가 Comparable을 구현한 경우 ,매개변수 하나짜리를 사용하면 되고 그렇지 않은 경우 추가적인 매개변수로 정렬기준(Comparator)을 따로 지정해 줘야한다.
```java
comparingInt(ToIntFunction<T> keyExtractor)
comparingLong(ToLongFunction<T> keyExtractor)
comparingDouble(ToDoubleFunction<T> keyExtractor)
```
비교대상이 기본형인 경우, comparing() 대신 위의 메소드를 사용하면 오토박싱과 언박싱 과정이 없어서 더 효율적이다.
```java
thenComparing(Comparator<T> other)
thenComparing(Function<T, U> keyExtractor)
thenComparing(Function<T, U> keyExtractor, Comparator<U> keyComp)
```
### 변환 - map()
map()은 스트림의 요소에 저장된 값 중에서 원하는 필드만 뽑아내거나 특정 형태로 변환해야 할 때 사용한다.

이 메소드의 선언부는 다음과 같다.
```java
Stream<R> map(Function<? super T, ? extends R> mapper)
```
매개변수로 T타입을 R타입으로 변환해서 반환하는 함수를 지정해야한다.

예를 들어 File의 스트림에서 파일의 이름만 뽑아서 출력하고자 할 때, 다음과 같이 map()을 이용하면 File 객체에서 파일의 이름(String)만 뽑아낼 수 있다.
```java
Stream<File> fileStream = Stream.of(new File("Ex1.java"), new File("Ex1"), new File("Ex1.bak", new FIle"Ex2.java", new File("Ex1.txt"));

// map()으로 Stream<File>을 Stream<String>으로 변환
Stream<String> filenameStream = fileStream.map(File::getName);
filenameStream.forEach(System.out::println); // 스트림의 모든 파일이름을 출력
```
map() 역시 중간 연산자이므로 연산결과는 String을 요소로 하는 스트림이다.
map()으로 ```Stream<File>```을 ```Stream<String>```으로 변환했다고 볼 수 있다.

map()도 filter()처럼 하나의 스트림에 여러 번 적용할 수 있다.
다음 코드는 File의 스트림에서 파일의 확장자만을 뽑은 다음 중복을 제거해서 출력한다.
```java
fileStream.map(File::getName) // Stream<File> -> Stream<String>
          .filter(s -> s.indexOf('.') != -1) // 확장자가 없는 것은 제외
          .map(s -> s.substring(s.indexOf('.') + 1)) // Stream<String> -> Stream<String>
          .map(String::toUpperCase) // 모두 대문자로 변환
          .distinct() // 중복 제거
          .forEach(System.out::print); // JAVABAKTXT 출력
```

### 조회 - peek()
peek()는 연산과 연산 사이에 올바르게 처리되었는지 확인하고자 할 때 사용한다.
forEach()와 달리 스트림의 요소를 소모하지 않으므로 자유롭게 사용할 수 있다.
```java
fileStream.map(File::getName) // Stream<File> -> Stream<String>
          .filter(s -> s.indexOf('.') != -1) // 확장자가 없는 것은 제외
          .peek(s->System.out.printf("filename = %s%n", s)); // 파일명을 출력한다.
          .map(s -> s.substring(s.indexOf('.') + 1)) // 확장자만 추출
          .distinct() // 중복 제거
          .forEach(System.out::println);
```
### mapToInt(), mapToLong(), mapToDouble()
map()은 연산의 결과로 ```Stream<T>``` 타입의 스트림을 반환하는데, 스트림의 요소를 숫자로 변환하는 경우 IntStream과 같은 기본형 스트림으로 변환하는 것이 더 유용할 수 있다.
```Stream<T>``` 타입의 스트림을 기본형 스트림으로 변환할 때 다음의 메소드를 사용한다.
```java
DoubleStream mapToDouble(ToDoubleFunction<? super T> mapper)
IntStream mapToInt(ToIntFunction<? super T> mapper)
LongStream mapToLong(ToLongFunction<? super T> mapper)
```
IntStream과 같은 기본형 스트림은 다음과 같이 숫자를 다루는데 편리한 메소드들을 제공한다.
```java
int sum() : 스트림의 모든 요소의 총합
OptionalDouble average() : sum() / (double)count()
OptionalInt max() : 스트림의 요소 중 제일 큰 값
OptionalInt min() : 스트림의 요소 중 제일 작은 값
```
스트림의 요소가 하나도 없을 때 sum()은 0을 반환하면 되지만 다른 메소드들은 단순히 0을 반환할 수 없다.
여러 요소들을 합한 평균이 0일 수도 있기 때문이다.
이를 구분하기 위해 단순히 double값을 반환하는 대신 double 타입의 값을 내부적으로 가지고 있는 OptionalDouble을 반환하는 것이다.
OptionalInt, OptionalDouble 등을 일종의 래퍼 클래스로 각각 내부적으로 int값과 Double값을 가지고 있다.

위 메소드들은 최종연산이기 때문에 호출 후에 스트림이 닫힌다.
그래서 sum()과 average()를 모두 호출해야 할 때, 스트림을 또 생성해야하므로 불편하기 때문에 summaryStatistics()라는 메소드가 제공된다.
```java
IntSummaryStatistics stat = scoreStream.summaryStatistics();

long totalCount = stat.getCount();
long totalScore = stat.getSum();
double avgScore = stat.getAverage();
int minScore = stat.getMin();
int maxScore = stat.getMax();
```
IntSummaryStatistics는 다양한 메소드를 제공하며, 이 중에서 필요한 것을 사용하면 된다.

IntStream을 ```Stream<T>```로 변환할 때에는 mapToObj()를 사용하며, ```Stream<Integer>```로 변환할 때에는 boxed()를 사용한다.
```java
Stream<U> mapToObj(IntFunction<? extends U> mapper)
Stream<Integer> boxed()
```
CharSequence에 정의된 chars()는 String이나 StringBuffer에 저장된 문자들을 IntStream으로 다룰 수 있게 해준다.
```java
IntStream charStream = "12345".chars(); // defaults IntStream chars()
int charSum = charStream.map(ch -> ch - '0').sum();
```
mapToInt()와 함께 자주 사용하는 메소드는 Integer의 parseInt()나 valueOf가 있다.
```java
Stream<String> -> IntStream 변환 시 mapToInt(Integer::parseInt)
Stream<Integer> -> IntStream 변환 시 mapToInt(Integer::intValue)
```
### flatMap() - ```Stream<T[]>```를 ```Stream<T>```로 변환
스트림의 요소가 배열이거나 map()의 연산결과가 배열인 경우, 즉 스트림의 타입이 ```Stream<T[]>``` 인 경우 ```Stream<T>```로 다루는 것이 더 편리할 때가 있다.
그럴 경우 flatMap()을 사용하면 된다.  
```java
  Stream<String[]> strArrStrm = Stream.of{
    new String[] {"abc", "def", "ghi" },
    new String[] {"ABC", "GHI", "JKLMN"}
}
```
위 ```Stream<String[]>``` 의 요소들을 모두 합쳐 문자열이 요소인 스트림 ```Stream<String>```으로 만들고자 한다.

스트림의 요소를 변경하므로 map()을, 배열을 스트림으로 만들어주는 Arrays.stream(T[])를 사용하면 다음 코드와 같다.
```java
Stream<Stream<String>> strStrStrm = strArrStrm.map(Arrays::stream);
```
<br>
![image](https://user-images.githubusercontent.com/62749021/185747966-b3dc8ad9-06c2-4a21-9650-1a687ccbd176.png)
<br>
각 요소의 문자열들이 합쳐지지 않고 스트림의 스트림 형태로 된 것을 확인할 수 있다.
이 때 map() 대신 flatMap()을 사용하면 된다.
```java
// Stream<Stream<String>> strStrStrm = strArrStrm.map(Arrays::stream);
Stream<String> strStrm = strArrStrm.flatMap(Arrays::stream);
```
위 코드를 그림으로 표현하면 다음과 같다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185747984-ba0b077c-8841-4509-9476-c82e49ee346a.png)

<br>

flatMap()은 map()과 달리 스트림의 스트림이 아닌 스트림으로 만들어 준다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185747993-f11f87fc-4bf2-436e-bf49-8e5c189d920f.png)

<br>
```java
String[] lineArr = {
    "Believe or not It is true",
    "Do or do not There is no try",
};

Stream<String> lineStream = Arrays.stream(lineArr);
Stream<Stream<String>> strArrStream = lineStream.map(line -> Stream.of(line.split(" +")));
```
여러 문장을 요소로 하는 스트림에서 split()으로 각 요소를 나눠 요소가 단어인 스트림을 만드는 경우에도 map()을 사용하면 ```Stream<Stream<String>>```이 되는 것을 확인할 수 있다.

여기서 map() 대신 flatMap()을 사용하면 원하는 결과를 얻을 수 있다.
```java
// Stream<Stream<String>> strArrStream = lineStream.map(line -> Stream.of(line.split(" +")));
Stream<String> strStream = lineStream.flatMap(line -> Stream.of(line.split(" +")));
```
이를 정리하면 다음과 같다.
<br>

![image](https://user-images.githubusercontent.com/62749021/185748150-2721b23c-2191-4e80-a5ae-07965e79d393.png)
<br>

strStream의 단어들을 모두 소문자로 변환하고, 중복된 단어들을 제거한 다음 정렬해서 출력하는 문장은 다음과 같다.
```java
strStream.map(String::toLowerCase) // 모든 단어를 소문자로 변경
         .distinct() // 중복된 단어를 제거
         .sorted() // 사전 순으로 정렬
         .forEach(System.out::println); // 화면에 출력
```
다음과 같이 요소의 타입이 ```Stream<String>```인 ```스트림(Stream<Stream<String>>)```이 있다고 가정하자.
```java
Stream<String> strStrm = Stream.of("abc", "def", "jklmn");
Stream<String> strStrm2 = Streamof("ABC", "GHI", "JKLMN");

Stream<Stream<String>> strmStrm = Stream.of(strStrm, strStrm2);
```
이 스트림을 ```Stream<String>```으로 변환하려면 다음과 같이 map()과 flatMap()을 함께 사용해야 한다.
```java
Stream<String> strStream = 
    strStrm.map(s -> s.toArray(String[]::new)) // Stream<Stream<String>> -> Stream<String[]>
           .flatMap(Arrays::stream); // Stream<String[]> -> Stream<String>
```
toArray()는 스트림을 배열로 변환해서 반환한다.
매개변수를 지정하지 않으면 Object[]을 반환하므로 위와 같이 특정 타입의 생성자를 지정해줘야 한다.
여기서는 String 배열의 생성자(String[]::new)를 지정했다.
그 다음엔 flatMap()으로 ```Stream<String[]>```을 ```Stream<String>```으로 변환한다.

## 1.11 ```Optional<T>``` 와 ```Optionalint```
```Optional<T>```는 지네릭 클래스로 T타입의 객체를 감싸는 래퍼 클래스이다.
그래서 Optional 타입의 객체에는 모든 타입의 참조변수를 담을 수 있다.
```java
public final class Optional<T> {
    private final T value; // T 타입의 참조변수
    ...
}
```
최종 연산의 결과를 그냥 반환하는 게 아니라 Optional 객체에 담아서 반환하면, 반환된 결과가 null인지 매번 if문으로 체크하는 대신 Optional에 정의된 메소드를 통해서 간단히 처리할 수 있다.
이제 널 체크를 위한 if문 없이도 NullPointerException이 발생하지 않는 보다 간결하고 안전한 코드를 작성하는 것이 가능하다.

Objects 클래스에 isNull(), nonNull(), requireNonNull()과 같은 메소드가 있는 것도 Optional과 동일한 의미라고 볼 수 있다.
즉 널 체크를 위한 if문을 메소드 안으로 넣어서 코드의 복잡도를 낮추기 위한 것이다.

### Optional 객체 생성하기
Optional 객체를 생성할 때는 of() 또는 ofNullable()을 사용한다.
```java
String str = "abc";
Optional<String> optVal = Optional.of(str);
Optional<String> optVal = Optional.of("abc");
Optional<String> optVal = Optional.of(new String("abc"));
```
만일 참조변수의 값이 null일 가능성이 있으면 of() 대신 ofNullable()을 사용한다.
of()는 매개변수의 값이 null이면 NullPointException이 발생하기 때문이다.
```java
Optional<String> optVal = Optional.of(null); // NullPointerException 발생
Optional<String> optVal = Optional.ofNullable(null); // OK
```
```Optional<T>``` 타입의 참조변수를 기본값으로 초기화할 때는 empty()를 사용한다.
null로 초기화하는 것이 가능하지만, empty()로 초기화하는 것이 바람직하다.
empty()는 지네릴 메소드이기 때문에 ```<T>```를 붙인 것이며, 추정 가능하므로 생략 가능하다.

```java
Optional<String> optVal = null; // 널로 초기화 
Optional<String> optVal = Optional.<String>empty(); // 빈 객체로 초기화
```

### Optional객체의 값 가져오기
Optional 객체에 저장된 값을 가져올 때는 get()을 사용한다.
값이 null일 때는 NoSuchElementException이 발생하며, 이를 대비해서 orElse()로 대체할 값을 지정할 수 있다.
```java
Optional<String> optVal = Optional.of("abc");
String str1 = optVal.get(); // optVal에 저장된 값을 반환. null이면 예외발생
String str2 = optVal.orElse("") // optVal에 저장된 값이 null일 대는 ""를 반환
```
orElse()의 변형으로 null을 대체할 값을 반환하는 람다식을 지정할 수 있는 orElseGet()과 null일 때 지정된 예외를 발생시키는 orElseThrow()가 있다.
```java
T orElseGet(Supplier<? extends T> other)
T orElseThrow(Supplier<? exteds X> exceptionSupplier)
```
사용법은 다음과 같다.
```java
String str3 = optVal2.orElseGet(String::new); // () -> new String()과 동일
String str4 = optVal2.orElseThrow(NullPointException::new); // 널이면 예외 발생
```
Stream첨러 Optional 객체에도 filter(), map(), flatMap()을 사용할 수 있다.
map()의 연산결과가 ```Optional<Optional<T>>``` 일 때, flatMap()을 사용하면 ```Optional<T>```를 결과로 얻는다.
만일 Optional객체의 값이 null이면, 이 메소드들은 아무 일도 하지 않는다.
```java
int reuslt = Optional.of("123")
                     .filter(x -> x.length > 0)
                     .map(Integer::parseInt).orElse(-1); // result = 123;
                     
result = Optional.of("")
                 .filter(x -> x.length() > 0)
                 .map(Integer::parseInt).orElse(-1); // result = -1;
```
parseInt()를 사용하기 위해 예외처리된 메소드를 만든다면 다음과 같을 것이다.
```java
static int optStrToInt(Optional<String> optStr, int defaultValue) {
    try {
        return optStr.map(Integer::parseInt).get();
    } catch (Exception e)
        return defaultValue;
    }
```
isPresent()는 Optional객체의 값이 null이면 false를, 아니면 true를 반환한다.
```ifPresent(Comsumer<T> block)```은 값이 있으면 람다식을 실행하고, 없으면 아무 일도 하지 않는다.

```java
if (str != null) {
    System.out.prinltn(str);
}

if (Optional.ofNullable(str).isPresent())
    System.out.println(str);
}
```
위 두 개의 if문은 동일한 의미를 가진다.

위의 코드를 ifPresent()를 이용하면 더 간단히 할 수 있다.
```java
Optional.ofNullable(str).ifPresent(System.out::println);
```
ifPresent()는 ```Optional<T>```를 반환하는 findAny()나 findFirst()와 같은 최종 연산과 잘 어울린다.
Stream 클래스에 정의된 메소드 중에서 ```Optional<T>```를 반환하는 것들은 다음과 같다.
```java
Optional<T> findAny()
Optional<T> findFirst()
Optional<T> max(Comparator<? super T> comparator)
Optional<T> min(Comparator<? super T> comparator)
Optional<T> reduce(BinaryOperator<T> accumulator)
```
### OptionalInt, OptionalLong, OptionalDouble
IntStream과 같은 기본형 스트림에는 Optional도 기본형을 값으로 하는 OptionalInt, OptionalLong, OptionalDouble을 반환하다.

다음 목록을 IntStream에 정의된 메소드들이다.
```java
OptionalInt findAny()
OptionalInt findFirst()
OptionalInt reduce(IntBinaryOperator op)
OptionalInt max()
OptionalInt min()
OptionalDouble average()
```
반환 타입이 ```Optional<T>```가 아니라는 것을 제외하고는 Stream에 정의된 것과 유사하다.
기본형 Optional에 저장된 값을 꺼낼 때 사용하는 메소드들의 이름이 Stream에 정의된 것과 약간 다르다는 것에 주의해야 한다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185748489-3b4b5fa7-ceb7-42df-a64d-0a65b57e3005.png)

<br>

OptionalInt는 다음과 같이 정의되어 있다.
```java
public final class OptionalInt {
    ...
    private final boolean isPresent; // 값이 저장되어 있으면 true
    private final int value; // int 타입의 변수
```
저장된 값이 없는 것과 값이 0이 저장된 것은 isPresent라는 인스턴스 변수로 구분이 가능하다.
isPresent()는 이 인스턴스변수의 값을 반환한다.
다음과 같이 구분할 수 있다.
```java
OptionalInt opt = OptionalInt.of(0); // OptionalInt에 0을 저장
OptionalInt opt2 = OptionalInt.empty(); // OptionalInt에 0을 저장

System.out.println(opt.isPresent()); // true
System.out.println(opt2.isPresent()); // false

System.out.println(opt.getAsInt()); // 0
System.out.println(opt.getAsInt()); // NoSuchElementException 발생

System.out.prinltn(opt.equals(opt2)); // false
```
Optional 객체의 경우 null을 저장하면 비어있는 것과 동일하게 취급한다.
```java
Optional<String> opt = Optional.ofNullable(null);
Optional<String> opt2 = Optional.empty();

System.out.println(opt.equals(opt2)); // true
```

## 1.12 스트림의 최종연산
최종 연산은 스트림의 요소를 소모해서 결과를 만들어낸다.
그러므로 최종 연산후에는 스트림이 닫히게 되고 더 이상 사용할 수 없다.

### forEach()
forEach()는 peek()와 달리 스트림의 요소를 소모하는 최종연산이다.
반환 타입이 void이므로 스트림의 요소를 출력하는 용도로 많이 사용된다.
```java
void forEach(Consumer<? super T> action)
```
### 조건 검사 - allMatch(), anyMatch(), noneMatch(), findFirst(), findAny()
스트림의 요소에 대해 지정된 조건에 모든 요소가 일치하는지, 일부가 일치하는지, 아니면 어떤 요소도 일치하지 않는지 확인하는데 사용할 수 있는 메소드이다.
이 메소드들은 모두 매개변수로 Predicate를 요구하며 연산결과로 boolean을 반환한다.

```java
boolean allMatch(Predicate<? super T> predicate)
boolean anyMatch(Predicate<? super T> predicate)
boolean noneMatch(Predicate<? super T> predicate)
```
스트림의 요소 중에서 조건에 일치하는 첫 번째 것을 반환하는 findFirst()가 있는데, 주로 filter()와 함께 사용되어 조건에 맞는 스트림의 요소가 있는지 확인하는데 사용된다.
병렬 스트림의 경우에는 findFirst()대신 findAny()를 사용해야 한다.
```java
Optional<Student> stu = stuStream.filter(s -> s.getTotalScore() <= 100).findFirst();
Optional<Student> stu = parallelStream.filter(s -> s.getTotalScore() <= 100).findAny();
```
findAny()와 findFirst()의 반환 타입은 ```Optional<T>```이며, 스트림의 요소가 없을 때는 비어있는 Optional 객체를 반환한다.
 
### 통계 - count(), sum(), average(), max(), min()
IntStream과 같은 기본형 스트림에는 스트림의 요소들에 대한 통계정보를 얻을 수 있는 메소드들이 있다.

그러나 기본형 스트림이 아닌 경우에는 통계와 관련된 메소드들이 다음과 같이 3개뿐이다.
```java
long count()
Optional<T> max(Comparator<? super T> comparator)
Optional<T> min(Comparator<? super T> comparator(
```
대부분의 경우 위의 메소드를 사용하기보단 기본형 스트림으로 변환하거나, reduce()와 collect() 를 사용해서 통계 정보를 얻는다.

### 리듀싱 - reduce()
reduce()는 스트림의 요소를 줄여나가면서 연산을 수행하고 최종결과를 반환한다.
그러므로 매개변수의 타입이 ```BinaryOperator<T>```이다.
```BinaryOperator<T>```는 BiFunction의 자손이며, ```BiFunction<T, T, T>```와 동등하다.

처음 두 요소를 가지고 연산한 결과를 가지고 그 다음 요소와 연산한다.
이 과정에서 스트림의 요소를 하나씩 소모하게 되며, 스트림의 모든 요소를 소모하게 되면 그 결과를 반환한다.
```java
Optional<T> reduce(BinaryOperator<T> accumulator)
```
이 외에도 연산결과의 초기값(identity)을 갖는 reduce()도 있다.
이 메소드들은 초기값과 스트림의 첫 번째 요소로 연산을 시작한다.
스트림의 요소가 하나도 없는 경우, 초기값이 반환되므로 반환 타입이 ```Optional<T>```가 아닌 T이다.

```java
T reduce(T identity, BinaryOperator<T> accumulator)
U reduce(U identity, BiFunction<U, T, U> accumulator, BinaryOperator<U> combiner)
```
위의 두 번째 메소드의 마지막 매개변수인 combiner는 병렬 스트림에 의해 처리된 결과를 합칠 때 사용하기 위해 사용하는 것이다.

앞서 소개한 최종 연산 count()와 sum() 등은 내부적으로 모두 reduce()를 이용해서 다음과 같이 작성된 것이다.
```java
int count = intStream.reduce(0, (a, b) -> a + 1); // count()
int sum = intStream.reduce(0, (a, b) -> a + b); // sum();
int max = intStream.reduce(Integer.MIN_VALUE, (a, b) -> a > b ? a : b); // max();
int min = intStream.reduce(Integer.MAX_VALUE, (a , b) -> a < b ? a : b); // min();
```
위 문장들에서 람다식을 Integer 클래스의 static 메소드 max()와 min()을 이용해서 메소드 참조로 바꾸면 다음과 같다.
```java
// OptionalInt reduce(IntBinaryOperator accumulator)
OptionalInt max = intStream.reduce(Integer::max); // int max(int a, int b)
OptionalInt min = intStream.reduce(Integer::min); // int min(int a, int b)
```
OptionalInt에 저장된 값을 꺼내려면 다음과 같이 하면 된다.
```java
int maxValue = max.getAsInt(); // OptionalInt에 저장된 값을 maxValue에 저장
```
reduce()로 스트림의 모든 요소를 더하는 과정에서의 내부 동작을 for문으로 표현하면 다음과 같다.
```java
int a = identity; // 초기값을 a에 저장한다.

for (int b : stream)
    a = a + b; // 모든 요소의 값을 a에 누적한다.
```

## 1.13 collect()
스트림의 최종 연산 중에서도 가장 복잡하면서도 유용하게 활용될 수 있는 collect() 메소드이다.

collect()는 스트림의 요소를 수집하는 최종 연산으로 앞서 배운 리듀싱(reducing)과 유사하다.
collect()가 스틀미의 요소를 수집하려면 어떻게 수집할 것인가에 대한 방법이 정의되어야 하는데, 이 방법을 정의한 것이 바로 컬렉터(collector)이다.
컬렉터는 Collector 인터페이스를 구현한 것으로, 직접 구현도 가능하며 미리 작성된 것도 사용 가능하다.
Collectors 클래스는 미리 작성된 다양한 종류의 컬렉터를 반환하는 static 메소드를 가지고 있으며, 이 클래스를 통해 제공되는 컬렉터만으로도 많은 일들을 할 수 있다.
```java
collect() : 스트림의 최종연산. 매개변수로 컬렉터를 필요로 한다.
Collector : 인터페이스. 컬렉터는 이 인터페이스를 구현해야한다.
Collectors : 클래스, static 메소드로 미리 작성된 컬렉터를 제공한다.
```
collect()는 매개변수의 타입이 Collector인데, 매개변수가 Collector를 구현한 클래스의 객체여야 한다는 것이다.
그리고 collect()는 이 객체에 구현된 방법대로 스트림의 요소를 수집한다.
sort()할 때 Comparator가 필요한 것처럼 collect()할 때는 Collector가 필요하다고 볼 수 있다.
```java
Object collect(Collector collector) // Collector를 구현한 클래스의 객체를 매개변수로
Object collect(Supplier supplier, BiConsumer accumulator, BiConsumer combiner)
```
매개변수가 3개인 collect()는 잘 사용되지 않지만, Collector 인터페이스를 구현하지 않고 람다식으로 수집할 때 사용하면 편리하다.

### 스트림을 컬렉션과 배열로 반환 - toList(), toSet(), toCollection(), toArray()
스트림의 모든 요소를 컬렉션에 수집하려면, Collectors 클래스의 toList()와 같은 메소드를 사용하면 된다.
List나 Set이 아닌 특정 컬렉션을 지정하려면, toCollection()에 해당 컬렉션의 생성자 참조를 매개변수로 넣어주면 된다.

```java
List<String> names = stuStream.map(Student::getName).collect(Collections.toList());
ArrayList<String> list = names.stream().collect(Collections.toCollection(ArrayList::new));
```
Map의 경우 키와 값의 쌍으로 저장해야하므로 객체의 어떤 필드를 키로 사용할지, 값으로 사용할지를 지정해줘야 한다.
```java
Map<String, Person> map = personStream.collect(Collections.toMap(p -> p.getRegId(), p -> p);
```
위 문장은 요소의 타입이 Persion인 스트림에서 사람의 주민번호(regId)를 키로 하고 값으로 Person 객체를 그대로 저장한다.
항등 함수를 의미하는 람다식 p->p 대신 Function.identity()를 사용해도 된다.

스트림에 저장된 요소들을 T[]타입의 배열로 변환하려면 toArray()를 사용하면 된다.
단 해당 타입의 생성자 참조를 매개변수로 지정해줘야 한다.
매개변수를 지정하지 않으면 반환되는 배열의 타입은 Object[] 이다.

```java
Student[] stdNames = studentStream.toArray(Student[]::new); // OK
Student[] stuNames = studentStream.toArray(); // ERROR
Object[] stuNames = studentStream.toArray(); // OK
```
### 통계 - counting(), summingInt(), averagingInt(), maxBy(), minBy()
최종 연산들이 제공하는 통계 정보를 collect()로도 얻을 수 있다.
주로 groupingBy()와 함께 사용한다.

```java
import static java.util.stream.Collectors.*;

long count = stuStream.count();
long count = stuStream.collect(counting()); // Collectors.counting();

long totalScore = stuStream.mapToInt(Student::getTotalScore).sum();
long totalScore = stuStream.collect(summingInt(Student::getTotalScore));

OptionalInt topScore = studentStream.mapToInt(Student::getTotalScore).max();

Optional<Student> topStudent = stuStream.max(Comparator.comparingInt(Student::getTotalScore));
Optional<Student> topStudent = stuStream.collect(maxBy(Comparator.comparingInt(Student::getTotalScore));

IntSummaryStatistics stat = stuStream.mapToInt(Student::getTotalScore).summaryStatistics();
IntSummaryStatistics stat = stuStream.collect(summarizingInt(Student::getTotalScore));
```

### 리듀싱 - reducing()
리듀싱 또한 Collect()로 가능하다.
```java
IntStream intStream = new Random().ints(1, 46).distinct().limit(6);

OptionalInt max = intStream.reduce(Integer::max);
Optional<Integer> max = intStream.boxed().collect(reducing(Intger::max));

long sum = intStream.reduce(0, (a, b) -> a + b);
long sum = intStream.boxed().collect(reducing(0, (a, b) -> a + b));

int grandTotal = stuStream.map(Student::getTotalScore).reduce(0, Integer::sum);
int grandTotal = stuStream.collect(reducing(0, Student::getTotalScore, Integer::sum));
```

### 문자열 결합 - joining()
문자열 스트림의 모든 요소를 하나의 문자열로 연결해서 반환한다.
구분자, 접두사, 접미사를 지정할 수 있다.
스트림의 요소가 String이나 StringBuffer처럼 CharSequence의 자손인 경우에만 결합이 가능하므로 스트림의 요소가 문자열이 아닌 경우 먼저 map()을 이용해서 스트림의 요소를 문자열로 변환해야 한다.

```java
String studentNames = stuStream.map(Student::getName).collect(joining());
String sutdentNames = stuStream.map(Student::getName).collect(joining(","));
String studentNames = stuStream.map(Student::getName).collect(joining(",", "[", "]"));
```
만일 map() 없이 스트림에 바로 joining()을 할 경우, 스트림의 요소에 toString()을 호출한 결과를 결합한다.

### 그룹화와 분할 - groupingBy(), partitioningBy()
그룹화는 스트림의 요소를 특정 특정 기준으로 그룹화하는 것을 의미한다.
분할은 스트림의 요소를 두 가지, 지정된 조건에 일치하는 그룹과 일치하지 않는 그룹으로의 분할을 의미한다.

groupingBy()는 스트림의 요소를 Function으로, partitioningBy()는 Predicate로 분류한다.
```java
Collector groupingBy(Function classifier)
Collector groupingBy(Function classifier, Collector downstream)
Collector groupingBy(Function classifier, Supplier mapFactory, Collector downstream)

Collector partitioningBy(Predicate predicate)
Collector partitioningBy(Predicate predicate, Collector downstream)
```
스트림을 두 개의 그룹으로 나눌 때는 partitioningBy()로 분할하고, 그 외의 경우에는 groupingBy()를 사용한다.

그룹화와 분할의 결과는 Map에 담겨 반환된다.

### partitioningBy()에 의한 분류
가장 기본적인 분할인 학생들을 성별로 나누어 List에 담았다.
```java
// 1. 기본 분할
Map<Boolean, List<Student>> stdBySex = stuStream.collect(partitioningBy(Student::isMale)); // 학생들을 성별로 분할
List<Student> maleStudent = stuBySex.get(true); // Map에서 남학생 목록을 얻는다.
List<Student> femaleStudent = stuBySex.get(false); // Map에서 여학생 목록을 얻는다.
```
이후 counting()을 추가해서 남학생의 수와 여학생의 수를 구했다.
couting() 대신 summingLong()을 사용하면 남학생과 여학생의 총점을 구할 수 있다.
```java
// 2.기본 분할 + 통계 정보
Map<Boolean, Long> stuNumBySex = stuStream.collect(partitioningBy(Student::isMale, counting()));
System.out.println("남학생 수 : " + stuNumBySex.get(true)); // 남학생 수 : 8
System.out.prinltn("여학생 수 : " + stuNumBySex.get(false)); // 여학생 수 : 10
```
남학생 1등과 여학생 1등은 다음과 같이 구할 수 있다.
```java
Map<Boolean, Optional<Student>> topScoreBySex = 
    stuStream.collect(
        partitioningBy(Student::isMale, maxBy(comparingInt(Student::getScore))
    )
);

System.out.println("남학생 1등 : " + topScoreBySex.get(true));
System.out.prinltn("여학생 1등 : " + topScoreBySex.get(false));
```
maxBy()는 반환타입이 ```Optional<Student>```이므로 위와 같은 결과가 나왔다.
```Optional<Student>```가 아닌 Student로 반환 결과를 얻으려면, 다음과 같이 collectingAndThen()과 Optional::get을 함께 사용하면 된다.

```java
Map<Boolean, Student> topScoreBySex =
    stuStream.collect(
        partitioningBy(Student::isMale, collectingAndThen(
            maxBy(comparingInt(Student::getScore)), Optional::get
        )
    )
);
```
성적이 150점 아래인 학생들을 불합격처리하기 위해 불합격자를 성별로 분류하여 얻어내고자 하면, partitioningBy()를 한 번 더 사용해서 이중 분할을 하면 된다.
```java
Map<Boolean, Map<Boolean, List<Student>>> failedStuBySex = 
    stuSream.collect(
        partitioningBy(Student::isMale, 
            partitioningBy(s -> s.getScore() < 150)
        )
    )
);
```

### groupingBy()에 의한 분류
stuStream을 반 별로 그룹지어 Map에 저장하는 가장 간단한 그룹화는 다음과 같다.
```java
Map<Integer, List<Student>> stuByBan = 
    stuStream.collect(groupingBy(Student::getBan)); // toList() 생략
```
groupingBy()로 그룹화를 하면 기본적으로 ```List<T>```에 담든다.
그래서 위 문장은 다음과 동일하다.
```java
Map<Integer, List<Student>> stuByBan = 
    stuStream.collect(groupingBy(Student::getBan, toList())); // toList() 생략가능
```
원한다면 toList()대신 toSet()이나 toCollection(HashSet::new)를 사용할 수 있따.
단 Map의 지네릭 타입도 적절히 변경해줘야 한다.
```java
Map<Integer, HashSet<Student>> stuByHak = 
    stuStream.collect(groupingBy(Student::getHak, toList())); // toList() 생략가능
```
stuStream을 성적의 등급(Student.Level)을 기준으로 HIGH, MID, LOW로 분류하는 코드는 다음과 같다.
```java
Map<Student.Level, Long> stuByLevel = 
    stuStream.collect(groupingBy(s -> {
        if (s.getScore() >= 200)
            return Student.Level.HIGH;
        else if (s.getScore() >= 100)
            return Student.Level.MID;
        else
            return Student.Level.LOW;
    }, counting())
}; 
```
groupingBy()를 여러 번 사용하면, 다수준 그룹화가 가능하다.
학년별로 그룹화한 뒤, 반별로 그룹화하고자 한다면 다음과 같이 하면 된다.
```java
Map<Integer, Map<Integer, List<Student>>> =
    stuStream.collect(groupingBy(Student::getHak,
        groupingBy(Student::getBan)
    ));
```
각 반의 1등을 출력하고자 한다면, collectingAndThen()과 maxBy()를 써서 다음과 같이 하면 된다.
```java
Map<Integer, Map<Integer, Student>> topStuByHakAndBan =
    stuStream.collect(groupingBy(Student::getHak,
        groupingBy(Student::getBan,
            collectingAndThen(
                maxBy(comparingInt(Student::getScore)), Optional::get
            )
        )
    ));
```
학년과 반별로 그룹화한 다음, 성적그룹으로 변환(mapping)하여 Set에 저장하는 코드는 다음과 같다.
```java
Map<Integer, Map<Integer, Set<Student::Level>>> stuByHakAndBan = 
    stuStream.collect(groupingBy(Student::getHak,
        groupingBy(Student::getBan,
            mapping(s -> {
                if (s.getScore() >= 200)
                    return Student.Level.HIGH;
                else if (s.getScore() >= 100)
                    return Student.Level.MID;
                else
                    return Student.Level.LOW;
            }, toSet())
        )
    )
);
```

## 1.14 Collector 구현하기
컬렉터를 작성한다는 것은 Collector 인터페이스를 구현한다는 것을 의미하는데, Collector 인터페이스는 다음과 같이 정의되어 있다.
```java
public interface Collector<T, A, R> {
    Supplier<A>	supplier();
    BiConsumer<A, T> accumulator();
    BinaryOperator<A> combiner();
    Functio<A, R> finisher();
    
    Set<Characteristics> characteristics(); // 컬렉터의 특성이 담긴 ㄴet을 반환
    ...
```
직접 구현해야하는 것은 위의 5개의 메소드이며 characteristics()를 제외하면 모두 반환타입이 함수형 인터페이스이다.
즉 4개의 람다식을 작성하면 된다.
```java
supplier() : 작업 결과를 저장할 공간을 제공
accumulator() : 스트림의 요소를 수집(collect)할 방법을 제공
combiner() : 두 저장공간을 병합할 방법을 제공(병합 스트림)
finisher() : 결과를 최종적으로 변환할 방법을 제공
```
finisher()의 경우 작업결과를 변환하는 일을 하는데 변환이 필요없다면 항등 함수인 Function.identity()를 반환하면 된다.
```java
public Function finisher() {
    return Function.identity(); // 항등 함수를 반환. return x -> x;와 동일
}
```
characteristics()는 컬렉터가 수행하는 작업의 속성에 대한 정보를 제공하기 위한 것이다.
```java
Characteristics.CONCURRENT : 병렬로 처리할 수 있는 작업
Characteristics.UNORDERED : 스트림의 요소의 순서가 유지될 필요가 없는 작업
Characteristics.IDENTITY_FINISH : finisher()가 항등 함수인 작업
```
위의 세 가지 속성 중에서 해당하는 것을 다음과 같이 Set에 담아서 반환하도록 구현하면 된다.
```java
public Set<Characteristics> characteristics() {
    return Collections.unmodifiableSet(EnumSet.of(
               Collector.Characteristics.CONCURERENT,
               Collector.Characteristics.UNORDERED
           ));
```
만일 아무런 속성도 지정하고 싶지 않다면, 다음과 같이 하면 된다.
```java
Set<Characteristics> characteristics() {
    return Collections.emptySet(); // 지정할 특성이 없는 경우 Set을 반환
}
```
finisher()를 제외하고 supplier(), accumulator(), combiner()는 리듀싱에서 나온 개념과 동일하다.
결국 Collector도 내부적으로 처리하는 과정이 리듀싱과 같다는 것을 의미한다.

즉 reduce()와 collect()는 근본적으로 하는 일이 같다.
차이점으로는 collect()는 그룹화와 분할, 집계 등에 유용하게 사용되고, 병렬화에 있어 collect()가 reduce()보다 유리하다.

## 1.15 스트림의 변환
스트림 간의 변환을 할 때 사용하는 메소드는 다음과 같다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185749229-c45a3d2d-7aee-4fb7-8669-51199ed1e696.png)

<br>

![image](https://user-images.githubusercontent.com/62749021/185749234-aa683cad-a33f-4442-bf64-7f187f17dd3f.png)

<br>

![image](https://user-images.githubusercontent.com/62749021/185749241-09098a0b-f4c2-4201-b3b7-6002939f0b40.png)

<br>
