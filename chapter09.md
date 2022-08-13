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
그러므로 오버라이딩 하지 않은 ```toString()``` 메소드를 호출하면 클래스이름에 16진수의 해시코드가 반환된다.

#### clone()
```clone()``` 메소드는 자신을 복사하여 새로운 인스턴스를 생성한다.
기존 인스턴스의 값을 보존한 채 작업을 하고자 할 때 사용한다.
```Object``` 클래스에 정의된 ```clone()``` 메소드는 단순히 인스턴스 변수의 값만을 복사하기 때문에, 참조 타입의 인스턴스 변수가 있는 클래스에는 얕은 복사가 진행되므로 완전한 인스턴스 복제가 이루어지지 않는다.
같은 인스턴스 주소를 참조하게 되기 때문에, 어느 쪽을 수정하더라도 양쪽이 변경되기 때문이다.
```java
class Point implements Cloneable { // Cloneable 인터페이스를 구현한 클래스에서만 clone()을 호출할 수 있다.
	int x;
	int y;

	Point(int x, int y) {
		this.x = x;
		this.y = y;
	}

	public String toString() {
		return "x="+x +", y="+y;
	}

	public Object clone() {
		Object obj = null;
		try {
			obj = super.clone();  // clone()은 반드시 예외처리를 해주어야 한다.
		} catch(CloneNotSupportedException e) {}
		return obj;
	}
}

class CloneEx1 {
	public static void main(String[] args){
		Point original = new Point(3, 5);
		Point copy = (Point)original.clone(); // 복제(clone)해서 새로운 객체를 생성
		System.out.println(original);
		System.out.println(copy);
	}
}
```
```clone()``` 메소드를 사용하려면 인스턴스의 데이터를 보호하기 위해 복제할 클래스가 ```Cloneable``` 인터페이스를 구해야한다.
```Cloneable```인터페이스가 구현되어 있다는 것은 클래스 작성자가 복제를 허용했다는 의미이기 때문이다.
```clone()``` 메소드를 오버라이딩하려면, 접근 제어자를 ```protected```에서 ```public```으로 변경한다.
그래야 아무 관계가 없는 다른 클래스에서 오버라이딩된 ```clone()``` 메소드를 호출할 수 있기 때문이다.

#### 공변 변환타입
JDK 1.5부터 ```공변 반환타입(covariant return type)```이라는 것이 추가되었다.
이 기능은 오버라이딩할 때 조상 메소드의 반환 타입을 자손 클래스의 타입으로 변환하는 것을 허용하는 것이다.
```공변 반환타입```을 사용한다면, 조상의 타입이 아닌 실제로 반환되는 자손 객체의 타입으로 변환할 수 있기 대문에 번거로운 형변한을 줄일 수 있다.
```java
class Foo {
    public A foo() {
        return new A();
    }
}

class Bar extends Foo {
    @Override
    public B foo() {
        return new B();
    }
}

class A { }
class B extends A { }
```
#### 얕은 복사와 깊은 복사

```clone()```은 단순히 객체에 저장된 값을 그대로 복제할 뿐, 객체가 참조하고 있는 객체까지는 복제하지는 않는다.
객체 배열을 ```clone()```으로 복제하는 경우에는 원본과 복제본이 모두 같은 객체를 공유하므로 완전한 복제라고 보기 어렵다.
이러한 복사를 ```얕은 복사(shallow copy)```라고 한다. ```얕은 복사```는 원본을 변경하면 복사본도 영향을 받는다.

원본이 참조하고 있는 객체까지 복제하는 것을 ```깊은 복사(deep copy)```라고 하며, 깊은 복사에서는 원본과 복사본이 서로 다른 객체를 참조하기 때문에 원본의 변경이 복사본에 영향을 미치지 않는다.

```java
import java.util.*;

class Circle implements Cloneable {
	Point2 p;  // 원점
	double r; // 반지름

	Circle(Point2 p, double r) {
		this.p = p;
		this.r = r;
	}

	public Circle shallowCopy() { // 얕은 복사
		Object obj = null;

		try {
			obj = super.clone();
		} catch (CloneNotSupportedException e) {}

		return (Circle)obj;
	}

	public Circle deepCopy() { // 깊은 복사
		Object obj = null;

		try {
			obj = super.clone();
		} catch (CloneNotSupportedException e) {}

		Circle c = (Circle)obj; 
		c.p = new Point2(this.p.x, this.p.y); 

		return c;
	}

	public String toString() {
		return "[p=" + p + ", r="+ r +"]";
	}
}

class Point2 {
	int x;
	int y;

	Point2(int x, int y) {
		this.x = x;
		this.y = y;
	}

	public String toString() {
		return "("+x +", "+y+")";
	}
}

class ShallowCopy {
	public static void main(String[] args) {
		Circle c1 = new Circle(new Point2(1, 1), 2.0);
		Circle c2 = c1.shallowCopy();
		Circle c3 = c1.deepCopy();
	
		System.out.println("c1="+c1);
		System.out.println("c2="+c2);
		System.out.println("c3="+c3);
		c1.p.x = 9;
		c1.p.y = 9;
		System.out.println("= c1의 변경 후 =");
		System.out.println("c1="+c1);
		System.out.println("c2="+c2);
		System.out.println("c3="+c3);
	}
}
__________결과____________
c1=[p=(1, 1), r=2.0]
c2=[p=(1, 1), r=2.0]
c3=[p=(1, 1), r=2.0]
= c1의 변경 후 =
c1=[p=(9, 9), r=2.0]
c2=[p=(9, 9), r=2.0]
c3=[p=(1, 1), r=2.0]
```

#### getClass()
```getClass()``` 메소드는 자신이 속한 클래스의 Class 객체를 반환하는 메소드이다.
Class객체는 이름이 ```Class```인 클래스의 객체이다.

```java
public final class Class implements ... {
    // ...
}
```
```Class``` 객체는 위와 같이 정의되어 있고, 클래스에 대한 모든 정보를 가지고 있으며, 클래스 당 1개만 존재한다.
클래스 파일이 ```클래스 로더(ClassLoader)```에 의해 메모리에 올라갈 때 자동으로 생성된다.
```클래스 로더```는 실행 시 필요한 클래스를 동적으로 메모리에 로드하는 역할을 한다.
먼저 기존에 생성된 클래스 객체가 메모리에 존재하는지 확인하고, 있으면 객체의 참조를 반환하며 없으면 ```클래스 패스(classpath)```에 지정된 경로를 따라서 클래스 파일을 찾는다.
못 찾았다면 ```ClassNotFoundException```이 발생하고, 찾으면 해당 클래스 파일을 읽어서 Class 객체로 변환한다.
클래스 파일을 읽어서 사용하기 편한 형태로 저장해 놓은 것이 클래스 객체이다.

#### Class 객체를 얻는 법
```java
Class cObj = new Card().getClass(); // 생성된 객체로부터
Class cObj = Card.class; // 클래스 리터럴(*.class)로부터
Class cObj = Class.forName("Card"); // 클래스 이름으로부터 
_______ex)
Card c = new Card(); // new 연산자를 이용하여 객체 생성
Card c = Card.class.newInstance(); // Class 객체를 이용하여 객체 생성
```
Class 객체를 이용하면 클래스에 정의된 멤버의 이름이나 개수 등 클래스에 대한 모든 정보를 얻을 수 있기 때문에 Class객체를 통해서 객체를 생성하고 메소드를 호출하는 등 보다 동적인 코드를 작성할 수 있다.

### 1.3 Sting 클래스
기본의 다른 언어에서는 문자열을 char형 배열로 다루었으나 자바에서는 문자열을 위한 클래스를 제공한다.
```String``` 클래스는 문자열을 저장하고 이를 다루는데 필요한 메소드를 제공한다.

#### 변경 불가능한(immutable) 클래스
String 클래스에는 문자열을 저장하기 위해 문자열 배열 변수 value를 인스턴스 변수로 정의해놓고 있다.
인스턴스 생성 시 생성자의 매개변수로 입력받는 문자열은 이 인스턴수 변수(value)에 문자형 배열(char [])로 저장된다.

String 클래스의 원형은 다음과 같다.
```java
public final class String implements java.io.Serializable, Comparable
{
    private char[] value;
    ...
}
```
한 번 생성된 String 인스턴스가 갖고 있는 문자열은 읽어올 수 만 있고, 변경할 수는 없다.
String 연산을 할 때마다 String 인스턴스가 생성되어 메모리공간을 차지하게 되므로 가능한 결합횟수를 줄이는 것이 좋다.
혹은 StringBuffer 클래스나 StringBuilder 클래스를 통해 문자열을 다루는 방법도 있다.

#### 문자열의 비교
문자열의 비교를 하기 전, 문자열을 생성 방식에 따른 차이를 확인할 필요가 있다.
```java
String str1 = "abc"; // 문자열 리터럴 "abc"의 주소가 str1에 저장됨
String str2 = "abc"; // 문자열 리터럴 "abc"의 주소가 str1에 저장됨
String str3 = new String("abc"); // 새로운 String 인스턴스 생성
String str4 = new String("abc"); // 새로운 String 인스턴스 생성
```
String 클래스의 생성자를 이용한 경우에는 new 연산자에 의해서 메모리할당이 이루어지므로 항상 새로운 String 인스턴스가 생성된다.
문자열 리터럴의 경우 이미 존재하는 것을 재사용한다.

#### 문자열 리터럴
자바 소스파일에 포함된 모든 문자열 리터럴은 컴파일 시에 클래스 파일에 저장된다.
이 때 같은 내용의 문자열 리터럴은 한 번만 저장된다.
문자열 리터럴도 String 인스턴스이고, 한 번 생성하면 내용은 변경할 수 없으므로 하나의 인스턴스를 공유하면 되기 때문이다.

#### 빈 문자열(empty string)
char형 배열도 길이가 0인 배열을 생성할 수 있고, 이 배열을 내부적으로 가지고 있는 문자열이 빈 문자열이다.
```java
String s = "";
char c = ''; // 에러 발생
```
String은 빈 문자열이 가능하지만, char는 빈 문자를 지정하는 것이 불가능하다.
```java
String s = null;
char c = '\u0000';

String s = "";
char c = ' ';
```
따라서 위 같은 방법으로 초기화를 진행한다.

#### String 클래스의 생성자와 메소드
![image](https://user-images.githubusercontent.com/62749021/184494760-e58f4215-0d77-4496-bf89-921dc610218b.png)

#### join()과 StringJoiner
join()은 여러 문자열 사이에 구분자를 넣어서 결합한다.
구분자로 문자열을 자르는 split()과 반대의 동작을 진행한다.

StringJoiner 클래스 또한 문자열을 결합할 수 있다.
```java
import java.util.StringJoiner;

class StringEx4 {
	public static void main(String[] args) {
		String animals = "dog,cat,bear";
		String[] arr   = animals.split(",");

		System.out.println(String.join("-", arr));

		StringJoiner sj = new StringJoiner("/","[","]");
		for(String s : arr)
			sj.add(s);

		System.out.println(sj.toString());
	}
}
```
결과는 다음과 같다.
```
dog-cat-bear
[dog/cat/bear]
```

#### 유니코드의 보충문자
String 클래스의 메소드 중 매개변수의 타입이 char인 것들도 있고, int인 것들도 있다.
문자를 다루는데 int를 쓰는 이유는 바로 확장된 유니코드를 다루기 위해서이다.
유니코드는 원래 2 byte - 16비트 문자체계인데, 현재는 20비트로 확장된 상태이다.
그러므로 하나의 문자를 char 타입으로 다루지 못하고, int 타입으로 다룰 수 밖에 없다.
확장에 의해 새로 추가된 문자들을 보충 문자(supplementary characters)라고 하는데 String 클래스의 메소드 중에서는 보충 문자를 지원하는 것이 있고 지원하지 않는 것이 있다.
int를 매개변수로 사용한다면 보충 문자를 지원해주는 것이다.

#### 문자 인코딩 변환
```getBytes(String charsetName)```을 사용하면 문자열의 문자 인코딩을 다른 인코딩으로 변경할 수 있다.
자바가 UTF-16을 사용하지만, 문자열 리터럴에 포함되는 문자들은 OS의 인코딩은 사용한다.
한글 windows의 경우 문자 인코딩을 CP949를 사용하며, UTF-8로 변경하려면 다음과 같이 하면 된다.
```java
byte[] utf8_str = "가".getBytes("UTF-8");
String str = nwe String(utf8_str, "UTF-8");
```
서로 다른 문자 인코딩을 사용하는 컴퓨터 간에 데이터를 주고받을 때에는 적절한 문자 인코딩이 필요하다.

#### String.format()
```format()```은 형식화된 문자열을 만들어내는 간단한 방법이다.```printf()```와 사용방법은 동일하다.

#### 기본형 값을 String으로 변환
String을 기본형 타입으로 변환하고자 한다면 valueOf() 메소드를 사용하거나 parseInt()를 사용하면 된다.
```java
int i = Interger.parseInt("100");
int i2 = Integer.parseInt("100");
```
원래 ```valueOf```의 반환 타입은 ```int```가 아닌 ```Integer```이지만 ```오토박싱(auto-boxing)```에 의해 ```Integer```가 ```int```로 자동 변환된다.

예전에는 ```parseInt()```와 같은 메소드를 사용했지만 현재는 메소드의 이름을 통일하기 위해 ```vauleOf()```가 나중에 추가되었다.
```valueOf(String s)```는 메소드 내부에서 그저 ```parseInt(String s)```를 호출할 뿐이므로, 두 메소드는 반환 타입만 다르지 같은 메소드이다.

기본형과 문자열간의 변환방법을 정리하면 다음과 같다.
|기본형->문자열 | 문자열->기본형|
|-------------|-------------|
|String String.valueOf(boolean b)<br>String String.valueOf(char c)<br>String String.valueOf(int i)<br>String String.valueOf(long l)<br>String String.valueOf(float f)<br>String String.valueOf(double d)|boolean Boolean.parseBoolean(String s)<br>byte Byte.parseByte(String s)<br>short Short.parseShort(String s)<br>int Integer.parseInt(String s)<br>long Long.parseLong(String s)<br>float Float.parseFloat(String s)<br>double Double.parseDouble(String s)|

```Boolean```, ```Byte```와 같이 기본형 타입의 이름이 첫 글자가 대문자인 것은 ```래퍼 클래스(wrapper class)```이다.
기본형 값을 감싸고 있는 클래스라는 뜻에서 붙여진 이름으로 기본형을 클래스로 표시한 것이다.

```parseInt()``` 나 ```parseFloat()``` 같은 메소드는 문자열에 공백 또는 문자가 포함되어 있는 경우 변환 시 예외(NumberFormatException)가 발생할 수 있다.
```java
int val = Integer.parseInt(" 123 ".trim());
```
그러나 부호를 의미하는 ```+```이나 소수점을 의미하는 ```.```, ```float```를 의미하는 ```f```와 같은 자료형 접미사는 허용된다.

### 1.4 StringBuffer 클래스와 StringBuilder클래스
```String``` 클래스는 인스턴스를 생성할 때 지정된 문자열을 변경할 수 없지만 ```StringBuffer``` 클래스는 변경이 가능하다.
내부적으로 문자열 편집을 위한 ```버퍼(Buffer)```를 가지고 있으며, ```StringBuffer``` 인스턴스를 생성할 때 그 크기를 지정할 수 있다.

```java
public final class StringBuffer implements java.io.Serializable {
    private char[] value;
}
```
```StringBuffer```클래스의 경우 위와 동일하다.

#### StringBuffer의 생성자
```StringBuffer``` 클래스의 인스턴스를 생성할 때, 적절한 길이의 char형 배열이 생성되고 이 배열은 문자열을 저장하고 편집하기 위한 ```공간(buffer)```으로 사용된다.

```StringBuffer``` 인스턴스를 생성할 때에는 ```생성자 StringBuffer(int length)```를 사용해서 StringBuffer 인스턴스에 저장될 문자열의 길이를 고려하여 충분히 여유있는 크기로 지정하는 것이 좋다.

길이를 지정하지 않는 경우, 16개의 문자를 저장할 수 있는 크기의 버퍼를 생성한다.

#### StringBuffer의 변경
```StringBuffer```는 내부의 ```char[] value```의 값을 변경할 수 있다.

```append()``` 메소드를 이용하여 맨 뒤에서부터 내용을 붙여나갈 수 있다.

```append()``` 메소드의 반환 타입 또한 ```StringBuffer```이기 때문에, ```append()```를 연속하여 쓰는것이 가능하며, 이를 ```체인 패턴(chain pattern)```이라고 한다.

#### StringBuffer의 비교
```String``` 클래스와 달리 ```StringBuffer``` 클래스는 ```equals()``` 메소드를 오버라이딩하지 않았기 때문에 ```equals()``` 메소드를 호출한 결과와 ```==``` 연산자로 비교한 값이 동일하다.
반면에 ```toString()``` 메소드는 오버라이딩 되어 있기 때문에 ```StringBuffer``` 인스턴스를 ```toString()``` 메소드를 통해 문자열로 치환한 뒤, ```String``` 클래스의 ```equals()``` 메소드로 비교를 해야한다.

#### StringBuffer 클래스의 생성자와 메소드
![image](https://user-images.githubusercontent.com/62749021/184496252-bc6ee420-f944-4bd7-989f-36f5730445f3.png)
<br>

#### StringBuilder란?
StringBuffer 클래스는 멀티쓰레드에 안전(thread safe)하도록 동기화되어 있다.
이러한 동기화는 성능을 저하하는 요소가 된다.
멀티쓰레드로 작성한 프로그램이 아닌 경우, StringBuffer의 동기화는 불필요해지므로 성능만 떨어뜨리는 상황이 된다.
그래서 StringBuffer에서 쓰레드의 동기화 기능만 제거한 StringBuilder가 새로 추가되었다.
StringBuffer 대신 StringBuilder를 사용하도록 바꾸기만 하면 된다.
다만 StringBuffer 또한 성능이 충분히 좋기 때문에 성능향상이 반드시 필요한 경우를 제외하면 굳이 변경할 필요는 없다.

### 1.5 Math 클래스
```Math``` 클래스는 기본적인 수학계산에 유용한 메소드로 구현되어 있다.

#### 올림, 버림, 반올림
```Math.round()```는 소수점 첫 번째 자리에서 반올림을 진행하여 그 결과를 ```long``` 타입으로 반환한다.
원하는 자리 수에서 반올림을 진행하기 위해서는 원하는 자리 수가 소수점 첫 번째 자리로 올 수 있도록 하면 된다.
<br>
```Math.rint()``` 또한 ```Math.round()```처럼 소수점 첫 번째 자리에서 반올림하지만, ```doubl```e을 반환한다.
또한 반환값이 가장 가까운 짝수 값으로 반환한다.
<br>
음수에서 ```Math.floor()```를 진행하면 절댓값이 가장 큰 쪽으로 변경된다.

#### 예외를 발생시키는 메소드
메소드 이름에 ```Exact```가 포함된 메소드의 경우 정수형간의 연산에서 발생할 수 있는 오버플로우(overflow)를 감지할 수 있다.

일반 연산자의 경우 단지 결과를 반환할 뿐, 오버플로우의 발생여부에 대해 알려주지 않는다.
단 아래와 같이 ```Exact```가 포함된 메소드를 사용할 경우 오버플로우가 발생하면 예외(ArithmeticException)을 발생시킨다.

```
int addExact(int x, int y) // x + y
int subtractExact(int x, int y) // x - y
int multiplyExact(int x, int y) // x * y
int incrementExact(int a) // a++
int decrementExact(int a) // a--
int negateExact(int a) // -a
int toIntExact(long value) // (int)value
```

#### 삼각함수와 지수, 로그
위 내용은 필요할 때 마다 자바 API를 참고하여 사용하면 된다.

#### StrictMath 클래스
```Math``` 클래스는 최대한의 성능을 얻기 위해 JVM이 설치된 OS의 메소드를 호출해서 사용한다.
즉 OS에 의존적인 계산을 하게 된다.
예를 들어 부동소수점 계산의 경우, 반올림의 처리방법 설정이 OS마다 다를 수 있기 때문에 자바로 작성된 프로그램임에도 불구하고 컴퓨터마다 결과가 다를 수 있다.
이러한 차이를 없애기 위해 다소 성능을 포기하는 대신, 어떤 OS에서 실행되도 항상 같은 결과를 얻을 때 사용하는 클래스가 ```StrictMath``` 클래스이다.

### 1.6 래퍼(wrapper) 클래스
기본형(primitive type) 변수도 어쩔 수 없이 객체로 다뤄야 하는 경우(매개변수가 객체를 요구할 때, 기본형 값이 아닌 객체로 저장해야할 때 등)가 있는데, 이 경우 ```래퍼(wrapper) 클래스```를 사용한다.
8개의 기본형을 대표하는 8개의 래퍼 클래스가 있는데, 이 클래스들을 이용하면 기본형 값을 객체로 다룰 수 있다.

래퍼 클래스의 생성자는 매개변수로 문자열이나 각 자료형의 값들을 입력받는데, 래퍼 클래스에 맞는 자료형의 값을 입력해야한다.

다음은 int 형의 래퍼 클래스인 ```Integer``` 클래스의 코드 일부이다.
```java
public final class Integer extends Number implements Comparable {
    private int value;
}
```
이처럼 래퍼 클래스들은 객체생성 시에 생성자의 인자로 주어진 각 자료형에 알맞은 값을 내부적으로 저장하고 있으며, 이에 관련된 여러 메소드가 정의되어있다.
![image](https://user-images.githubusercontent.com/62749021/184496400-66a614e9-a853-4375-b883-fc2bba0b9304.png)
<br>
래퍼 클래스들은 모두 ```equals()``` 메소드가 오버라이딩되어 있어 주소값이 아닌 객체가 가지고 있는 값을 비교한다.
또한 래퍼 클래스들은 객체이기 때문이 비교 연산자를 사용할 수 없으며, 대신 ```compareTo()``` 메소드를 제공한다.
```toString()``` 또한 오버라이딩되어 있어 객체가 가지고 있는 값을 문자열로 반환할 수 있다.
이 외에도 ```MAX_VALUE```, ```MIN_VALUE```, ```SIZE```, ```BYTES```, ```TYPE```등의 static 상수를 공통적으로 가지고 있다.

#### Number 클래스
```Number```클래스는 추상클래스로 내부적으로 숫자를 멤버변수로 갖는 래퍼 클래스들의 조상이다.
|object|object의 자손|Number의 자손|
|:------:|:-------:|:--------:|
|Object|Boolean<br>Character<br>Number|Byte<br>Short<br>Integer<br>Long<br>Float<br>Double<br>Biginteger<br>BigDecimal|

그 외에 long으로도 다룰 수 없는 큰 범위의 정수를 다룰 수 있게 해 주는 ```BigInteger``` 클래스와 ```double```로도 다룰 수 없는 큰 범위의 부동 소수점을 다룰 수 있게 해 주는 ```BigDecimal```이 있다.

#### 문자열을 숫자로 변환하기
다음과 같은 세 메소드 중 하나를 선택하면 된다.
```java
int i = new Integer("100").intValue();
int i2 = Integer.parseInt("100");
Integer i3 = Integer.valueOf("100");
```
JDK 1.5부터 도입된 ```오토박싱(autoboxing)``` 기능으로 인해 반환값이 기본형일 때와 래퍼클래스일 때의 차이가 없어졌다.
그래서 ```valueOf()``` 메소드로 통일하기도 하지만, 성능 면에서는 ```valueOf()``` 메소드가 조금 더 느리다.
```java
static int parseInt(String s, int radix) // 문자열 s를 radix 진법으로 인식
static Integer valueOf(String s, int radix)
```
위와 같이 10진수가 아닌 경우 진수를 지정할 수도 있다.
진수가 없으면 10진수로 간주한다.

#### 오토박싱 & 언박싱(autoboxing & unboxing)
JDK 1.5 이후부터 지원한 기능으로, 기본형과 참조형 간의 덧셈이 가능해지도록 자동으로 래퍼 클래스의 값을 기본형으로 변환하여 계산해준다.
|컴파일 전|	컴파일 후|
|---------|------------|
|int i = 5;<br>Integer iObj = new Integer(7);<br>int sum = i + iObj;|int i = 5;<br>Integer iObj = new Integer(7);<br>int sum = i + iObj.intValue();|

위와 같이 기본형 값을 래퍼 클래스의 객체로 자동 변환해주는 것을 ```오토박싱(autoboxing)```이라고 하며, 반대로 변환하는 것을 ```언박싱(unboxing)```이라고 한다.

### 1.10 java.util.Objects 클래스
Object 클래스의 보조 클래스로 Math 클래스처럼 모든 메소드가 static이다.
객체의 비교나 널 체크(null check)시에 유용하다.

```
static boolean isNull(Object obj)
static boolean nonNull(Object obj)

static <T> T requireNonNull(T obj)
static <T> T requireNonNull(T obj, String message)
static <T> T requireNonNull(T obj, Supplier<String> messageSupplier)

static int compare(Object a, Object b, Comparator c)

static boolean equals(Object a, Object b)
static boolean deepEquals(Object a, Object b)

static String toString(Object o)
static String toString(Object o, String nullDefault)

static int hashCode(Object o)
static int hash(Object... values)
```
위와 같은 메소드들이 있으며, 필요할 때마다 자바 API 에서 확인하면 될 것이다.

### 1.11 java.util.Random 클래스
난수를 얻을 수 있는 클래스이다.
```Math.random()``` 메소드로도 난수를 얻을 수 있지만, ```Math.random()``` 메소드 또한 ```Random``` 클래스를 내부적으로 사용하기 때문에 둘 중 편한것을 선택하면 된다.
```java
double randNum = Math.random();
double randNum = new Random().nextDouble();
```
위 두 코드는 동일한 결과를 수행한다.

```Math.random()```과 ```Random```의 가장 큰 차이점은 ```종자값(seed)```을 설정할 수 있다는 것이다.
종자값이 같은 ```Random``` 인스턴스들은 항상 같은 난수를 같은 순서대로 반환한다.

#### Random 클래스의 생성자와 메소드
생성자 ```Random()```은 아래와 같이 종자값을 ```System.currentTimeMillis()```로 하기 때문에 실행할 때마다 얻는 난수가 달라진다.
```
public Random(){
    this(System.currentTimeMillis());
}
```
![image](https://user-images.githubusercontent.com/62749021/184496896-58f07c3c-c431-4453-8703-ba5ca581cb8d.png)
<br>
```nextBytes()```는 ```BigInteger(int signum, byte[] magnitude```)와 함께 사용하면 int의 범위인 ```-2^31 ~ 2^31 - 1```보다 넓은 범위의 난수를 얻을 수 있다.

### 1.12 정규식(Regular Expression) - java.util.regex 패키지
```정규식```이란 텍스트 데이터 중에서 원하는 조건(패턴, pattern)과 일치하는 문자열을 찾아내기 위해 사용하는 것으로 미리 정의된 기호와 문자를 이용해서 작성한 문자열을 의미한다.

```java
import java.util.regex.*;	// Pattern과 Matcher가 속한 패키지

class RegularEx1 {
	public static void main(String[] args) 
	{
		String[] data = {"bat", "baby", "bonus",
				    "cA","ca", "co", "c.", "c0", "car","combat","count",
				    "date", "disc"};		
		Pattern p = Pattern.compile("c[a-z]*");	// c로 시작하는 소문자영단어

		for(int i=0; i < data.length; i++) {
			Matcher m = p.matcher(data[i]);
			if(m.matches())
				System.out.print(data[i] + ",");
		}
	}
}
```
실행 결과는 다음과 같다
```
ca,co,car,combat,count,
```
위 예제의 실행 과정은 다음과 같다.
```
1. 정규식을 매개변수로 하여 Pattern 클래스의 static 메소드인 Pattern compile(String regex)을 호출하여 Pattern 인스턴스를 얻는다.
    Patterin p = Pattern.compile("c[a-z]*");
2. 정규식으로 비교할 대상을 매개변수로 Pattern 클래스의 Matcher matcher(CharSequence input)를 호출해서 matcher 인스턴스를 얻는다.
    Matcher m = p.matcher(data[i]);
3. Matcher 인스턴스에 boolean matches()를 호출하여 정규식에 부합하는지 확인한다.
    if (m.matches())
```
정규식은 매우 광범위하기 때문에 필요할 때 마다 검색하여 정규식의 작성 예를 보고 응용할 수 있을 정도로만 학습하고 넘어가는 것이 좋다.

### 1.13 java.util.Scanner 클래스
```Scanner``` 클래스는 화면, 파일, 문자열과 같은 입력소스로부터 문자데이터를 읽어오는데 도움을 줄 목적으로 JDK 1.5부터 추가되었다.
```
Scanner (String source)
Scanner (File source)
Scanner (InputStream source)
Scanner (Readable source)
Scanner (ReadableByteChannel source)
Scanner (Path source)
```
위와 같이 다양한 생성자를 통해 다양한 입력소스로부터 데이터를 읽어올 수 있다.

또한 ```Scanner``` 클래스는 ```정규식 표현 (Regular expression)```을 이용한 라인단위의 검색을 지원하며 ```구분자(delimiter)```에도 정규식 표현을 사용할 수 있어 복잡한 형태의 구분자도 처리할 수 있다.

```java
Scanner useDelimiter(Pattern pattern)
Scanner useDelimiter(String pattern)
```
또한 ```Scanner``` 클래스는 다음과 같은 메소드를 제공함으로써 형변환을 하는 수고를 덜어 준다.
```java
boolean nextBoolean();
byte nextByte();
short nextShort();
int nextInt();
long nextLong();
double nextDouble();
float nextFloat();
String nextLine();
```

### 1.14 java.util.StringTokenizer 클래스
```StringTokenizer``` 클래스는 긴 문자열을 지정된 ```구분자(delimiter)```를 기준으로 ```토큰(token)```이라는 여러 개의 문자열로 잘라내는 데 사용된다.

```String``` 클래스의 ```split(String regex)```나 ```Scanner```의 ```useDelimiter(String pattern)``` 또한 동일한 결과를 확인할 수 있겠지만, 정규 표현식을 써야 하므로 익숙하지 않은 경우 ```StringTokenizer``` 클래스를 활용하는 편이 간단하면서도 명확한 결과를 얻을 수 있을 것이다.

#### StringTokenizer의 생성자와 메소드
![image](https://user-images.githubusercontent.com/62749021/184497097-c7a46fa0-009d-4086-a676-5b091dd87bc8.png)

### 1.15 java.math.BigInteger 클래스
정수형으로 표현할 수 있는 값의 한계가 있다.
```long```으로 표현할 수 있는 범위보다 더 큰 범위를 다뤄야 할 경우 ```BigInteger``` 클래스를 사용한다.
```BigInteger``` 클래스는 내부적으로 int 배열을 사용해서 값을 다루므로 ```long``` 타입보다는 훨씬 큰 값을 다룰 수 있지만, 성능은 떨어진다.

```BigInter``` 클래스는 ```String``` 클래스처럼 ```불변(immutable)```이며, 값을 2의 보수로 저장한다.

#### BigInteger의 생성
```BigInteger```를 생성하는 방법은 여러 가지가 있는데, 문자열로 숫자를 표현하는 것이 일반적이다.
정수형 리터럴로는 표현할 수 있는 값의 한계가 있기 때문이다.
```java
BigInteger val;

val = new BigInteger("1234567891234567890");
```

#### 다른 타입으로의 변환
```BigInteger```를 문자열, 또는 byte 배열로 변환하는 메소드는 다음과 같다.
```
String toString(); // 문자열로 반환
String toString(int radix); // 지정된 진법(radix)의 문자열로 반환
byte[] toByteArray() // byte 배열로 반환
```
```BigInteger```도 ```Number```로부터 상속받은 기본형으로 변환하는 메소드를 가지고 있다.
```java
int	intValue();
long longValue();
float floatValue();
double doubleValue();
```
정수형으로 변환하는 메소드 중 이름 끝에 ```Exact```가 붙은 것들은 변환한 결과가 변환한 타입의 범위에 속하지 않으면 ArithmeticException을 발생시킨다.
```java
byte byteValueExact();
int intValueExact();
long longValueExact();
```

#### BigInteger의 연산
```BigInteger```에는 정수형에 사용할 수 있는 모든 연산자와 수학적인 계산을 쉽게 해주는 메소드들이 정의되어 있다.

해당 연산자들은 자바 API에서 확인할 수 있다.

```BigInteger```는 불변이므로 반환타입이 ```BigInteger```라는 것은 새로운 인스턴스가 반환된다는 것이다.

#### 비트연산 메소드
워낙 큰 숫자를 다루기 위한 클래스이므로, 성능을 향상시키기 위해 비트단위로 연산을 수행하는 메소드들을 가지고 있다.

```and, or, xor, not``` 뿐만 아니라 다음과 같은 메소드들도 제공한다.

```java
int bitCount() // 2진수로 표현했을 때 1의 개수(음수는 0의 개수)를 반환
int bitLength() // 2진수로 표현했을 때 값을 표현하는데 필요한 bit 수
boolean testBit(int n) // 우측에서 n +1 번째가 1이면 true
BigInteger setBit(int n) // 우측에서 n + 1번째 비트를 1로 변경
BigInteger clearBit(int n) // 우측에서 n + 1번째 비트를 0으로 변경
BigInteger flipBit(int n) // 우측에서 n + 1번째 비트를 변환(1->0, 0->1)
```
가능하면 산술연산 대신 비트연산으로 처리하는 편이 좋다.

### 1.16 java.math.BigDecimal 클래스
```double``` 타입으로 표현할 수 있는 값은 범위가 넓지만, 정밀도가 최대 13자리밖에 되지 않고 실수형의 특성상 오차를 피할 수 없다.
```BigDecimal```은 실수형과 달리 정수를 이용해서 실수를 표현한다.
즉 오차가 없는 2진 진수로 변환해서 값을 다루기 때문에, 오차가 발생하지 않는다.
```BigDecimal```은 실수를 정수와 10의 제곱의 곱으로 표현한다.
```
(정수 * 10^-scale)
```
```scale```은 0부터 Integer.MAX_VALUE 사이의 범위에 있는 값이다.
```BigDecimal```은 정수를 저장하는데 ```BigInteger```를 사용한다.

```BigDecimal``` 또한 ```불변(immutable)``` 이다.

#### BigDecimal의 생성
값을 문자열로 입력한다.

#### 다른 타입으로의 변환
```BigDecimal```을 문자열로 변환하는 메소드는 다음과 같다.
```java
String toPlainString() // 다른 기호없이 숫자로만 표현
String toString() // 필요하면 지수형태로 표현할 수 있음
```
대부분의 경우 두 메소드의 반환결과가 같지만 ```BigDecimal```을 생성할 때 ```1.0e-22```와 같은 지수형태의 리터럴을 사용했을 때 다른 결과가 나올수도 있다.

```BigDecimal ```또한 ```Number```로부터 상속받은 기본형으로 변환하는 메소드를 가지고 있다.
```java
int	intValue();
long longValue();
float floatValue();
double doubleValue();
```
정수형으로 변환하는 메소드 중 이름 끝에 ```Exact```가 붙은 것들은 변환한 결과가 변환한 타입의 범위에 속하지 않으면 ArithmeticException을 발생시킨다.
```java
byte byteValueExact();
int intValueExact();
long longValueExact();
```

#### BigDecimal의 연산
```BigDecimal```에는 실수형에 사용할 수 있는 모든 연산자와 수학적인 계산을 쉽게 해주는 메소드들이 정의되어 있다.

해당 연산자들은 자바 API에서 확인할 수 있다.

```BigDecimal```는 불변이므로 반환타입이 ```BigDecimal```라는 것은 새로운 인스턴스가 반환된다는 것이다.

한 가지 알아둬야 할 경우 연산결과의 정수, 지수, 정밀도가 달라진다는 것이다.
```java
BigDecimal bd1 = new BigDecimal("123.456"); // 123456, 3, 6
BigDecimal bd2 = new BigDeciaml("1.0"); // 10, 1, 2
BigDecimal bd3 = bd1.multiply(bd2); // 124560, 4, 7
```
곱셈에서는 두 피연산자의 ```scale```을 맞추고, 나눗셈에서는 뺀다.
덧셈과 뺄셈에서는 둘 중에서 자리수가 높은 쪽으로 맞추기 위해서 두 ```scale``` 중 큰 쪽이 결과가 된다.

#### 반올림 모드 - devide()와 setScale()
다른 연산과 달리 나눗셈을 처리하기 위한 메소드는 다음과 같이 다양한 버전이 존재한다.
나눗셈의 결과를 어떻게 반올림(roundingMode) 처리할 것인지와, 몇 번째 자리(scale)에서 반올림할 것인지를 지정할 수 있다.
```BigDecimal```이 아무리 오차가 없다고 하더라도 나눗셈에서 발생하는 오차는 어쩔 수 없다.
```java
BigDecimal divide (BigDecimal divisor)
BigDecimal divide (BigDecimal divisor, int roundingMode)
BigDecimal divide (BigDecimal divisor, RoundingMode roundingMode)
BigDecimal divide (BigDecimal divisor, int scale, int roundingMode)
BigDecimal divide (BigDecimal divisor, int scale, RoundingMode roundingMode)
BigDecimal divide (BigDecimal divisor, int scale, MathContext mc)
```
roundingMode는 반올림 처리방법에 대한 것으로 BigDecimal에 정의된 ROUND_로 시작하는 상수들 중에 하나를 선택해서 사용하면 된다.
RoundingMode는 이 상수들을 열거형으로 정의한 것이다.
가능하면 열거형 RoundingMode를 사용하는 것을 권장한다.
![image](https://user-images.githubusercontent.com/62749021/184497507-68a8b5ff-8835-4561-903b-7d156bed3a80.png)

#### java.math.MathContext

```MathContext``` 클래스는 반올림 모드와 정밀도(precision)을 하나로 묶어 놓은 것일 뿐 별 다른 것은 없다.
다만 주의할 점으로는 divide()에서는 scale이 소수점 이하의 자리수를 의미하는데, ```MatContext```에서는 precision이 정수와 소수점 이하를 몯 포함한 자리수를 의미한다는 것이다.
```java
BigDecimal bd1 = new BigDecimal("123.456");
BigDecimal bd2 = new BigDecimal("1.0");

System.out.println(bd1.divide(bd2, 2, HALF_UP)); // 123.46
System.out.println(bd1.divide(bd2, new MathContext(2, HALF_UP)); // 1.2E+2
```

#### scale의 변경
```BigDecimal```을 10으로 곱하거나 나누는 대신 sacle의 값을 변경함으로써 같은 결과를 얻을 수 있다.
```BigDecimal```의 ```scale```은 ```setScale()``` 메소드를 통해 변경할 수 있다.
```java
BigDecimal setScale(int newScale)
BigDecimal setScale(int newScale, int roundingMode)
BigDecimal setScale(int newScale, RoundingMode mode)
```
```setScale()```로 scale의 값을 줄이는 것은 10^n으로 나누는 것과 가으므로 ```divide()```를 호출할 때 처럼 오차가 발생할 수 있기 때문에 반올림 모드를 지정해주어야 한다.
