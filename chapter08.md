## 1. 예외처리(exception handling)
### 1.1 프로그램 오류
프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비정상적으로 종료되는 경우가 있다. 이러한 결과를 초래하는 원인을 프로그램 에러 또는 오류라고 한다.
이를 발생시점에 따라 ```컴파일 에러(compile-time error)```와 ```런타임 에러(runtime error)```로 구분할 수 있다.

```
컴파일 에러 = 컴파일 시에 발생하는 에러
런타임 에러 = 실행 시에 발생하는 에러
논리적 에러 = 실행은 되지만, 의도와 다르게 동작하는 것
```

#### 컴파일 에러
컴파일러가 소스파일(.java)에 대해 오타나 잘못된 구문, 자료형 체크 등의 기본적인 검사를 수행하여 오류가 있는지를 알려준다.

#### 런타임 에러
문법적으로는 오류가 없지만 프로그램 실행 시 실행 도중에 발생하는 오류에 의한 에러.
방지를 위해 프로그램 실행 도중 발생할 수 있는 모든 경우의 수를 고려하여 이에 대한 대비를 하는 것이 중요하다.

자바에서는 실행 시 발생할 수 있는 프로그램 오류를 ```에러(error)```와 ```예외(exception)``` 두 가지로 분류했다.

에러는 발생하면 복구할 수 없는 심각한 오류이고, 예외는 발생하더라도 수습될 수 있는 비교적 덜 심각한 것이다.

따라서 에러는 프로그램의 비정상 종료를 막을 수 없지만, 예외는 발생하더라도 프로그램의 비정상적인 종료를 막을 수 있다.

### 1.2 예외 클래스의 계층구조
자바에서는 실행 시 발생할 수 있는 오류(Exception과 Error)를 클래스로 정의했다.
예외 클래스의 계층 구조는 다음 그림과 같다.<br>

![image](https://user-images.githubusercontent.com/62749021/184306962-d4022d0f-ecf6-4164-8f0b-097d1ecc76b1.png)

모든 예외의 최고 조상은 Exception 클래스이며, 상속계층도를 Exception 클래스부터 도식화하면 다음과 같다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184307040-62064ea3-de08-425d-b7ed-0c51bfccbd2d.png)

예외 클래스는 두 그룹으로 분류할 수 있다.
  1. RuntimeException 클래스와 그 자손들을 제외한 Exception 클래스와 그 자손들
  2. 1번을 제외한 RuntimeExceptinon 클래스와 그 자손들

```
Exception 클래스 : 사용자의 실수 같은 외적인 요인에 의해 발생하는 예외
RuntimeException 클래스 : 프로그래머의 실수로 발생하는 예외
```

### 1.3 예외처리하기 - try-catch문

프로그램 실행 도중에 발생하는 에러는 어쩔 수 없지만, 예외는 프로그래머가 이에 대한 처리를 미리 해주어야 안정적인 프로그램을 개발할 수 있다.

예외처리(exception handling)이란, 프로그램 실행 시 발생할 수 있는 예기치 못한 예외의 발생에 대비한 코드를 작성하는 것이다.

예외처리의 목적은 예외의 발생으로 인한 실행 중인 프로그램의 갑작스런 비정상 종료를 막고, 정상적인 실행상태를 유지할 수 있도록 하는 것이다.

정리하면 다음과 같다.

```
예외처리의 정의 - 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성하는 것
예외처리의 목적 - 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것
```

예외를 처리하기 위해서 try-catch문을 사용한다.
```
try{
    // 예외가 발생할 가능성이 있는 문장을 넣는다.
}
catch (Exception1 e1){
    // Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 넣는다.
}
catch (Exception2 e2){
    // Exception2이 발생했을 경우, 이를 처리하기 위한 문장을 넣는다.
}
catch (Exception3 e3){
    // Exception3이 발생했을 경우, 이를 처리하기 위한 문장을 넣는다.
}
```
나의 try 블럭 다음에는 여러 종류의 예외를 처리할 수 있는 하나 이상의 catch 블럭이 올 수 있으며, 이 중 발생한 예외의 종류와 일치하는 단 한 개의 catch 블럭만 수행된다.
발생한 예외의 종류와 일치하는 catch 블럭이 없으면 예외는 처리되지 않는다.

### 1.4 try-catch문에서의 흐름
try-catch문의 흐름은 다음과 같다.
```
try블럭 내에서 예외가 발생한 경우
    - 1. 발생한 예외와 일치하는 catch 블럭이 있는지 확인한다.
    - 2. 일치하는 catch블럭을 찾게 되면, 그 catch블럭 내의 문장들을 수행하고 전체 try-catch문을 빠져나가서 그 다음 문장을 계속 수행한다.
         만약 일치하는 catch 블럭을 찾지 못하면 예외는 처리되지 못한다.
try 블럭 내에서 예외가 발생하지 않은 경우
    - 1. catch 블럭을 거치지 않고 전체 try-catch문을 빠져나가서 수행을 계속한다.
```

### 1.5 예외의 발생과 catch 블럭
catch 블럭은 괄호()와 블럭{} 두 부분으로 나뉘어져 있는데, 괄호() 내에는 처리하고자 하는 예외와 같은 차임의 참조변수 하나를 선언해야한다.

예외가 발생하면, 발생한 예외에 해당하는 클래스의 인스턴스가 만들어진다.
예외가 발생한 문장이 try 블럭에 포함되어 있다면, 이 예외를 처리할 수 있는 catch 블럭이 있는지 찾는다.

첫 번째 catch 블럭부터 차례로 내려가면서 catch블럭의 괄호()내에 선언된 참조변수의 종류와 생성된 예외클래스의 인스턴스에 instanceof 연산자를 이용하여 검사하게 되는데, 검사결과가 true인 catch블럭을 찾을 때까지 검사를 계속한다.

검사결과가 true인 catch블럭을 찾게 되면 블럭에 있는 문장들을 모두 수행한 후에 try-catch문을 빠져나가고 예외는 처리되지만, 검사결과인 true인 catch블럭이 하나도 없으면 예외는 처리되지 않고, JVM의 예외처리기(UncaughtExceptionHandler)에 의해 처리된 뒤 비정상 종료된다.

여기서 모든 예외 클래스는 Exception의 클래스의 자손이므로, catch 블럭에 Exception 클래스 타입의 참조변수를 선언해 놓으면 어떤 종류의 예외가 발생하더라도 이 catch 블럭에 의해서 처리된다.

그러므로 try-catch문의 마지막에 Exception 클래스 타입의 참조변수를 선언한 catch 블럭을 사용하면 어떠한 종류의 예외가 발생하더라도 이 catch블럭에 의해 처리되도록 할 수 있다.

#### printStackTrace()와 getMessage()
```
printStackTrace() : 예외발생 당시의 호출스택(Call Stack)에 있었던 메소드의 정보와 예외 메세지를 화면에 출력한다.
getMessage() : 발생한 예외클래스의 인스턴스에 저장된 메세지를 얻을 수 있다.
```

#### 멀티 catch 블럭
JDK 1.7부터 여러 catch블럭을 ```|``` 기호를 이용하여 하나의 catch 블럭으로 합칠 수 있다.
이를 ```멀티 catch 블럭```이라고 하며, 표현할 수 있는 예외 클래스의 개수에는 제한이 없다.

멀티 catch의 문법은 다음과 같다.
```
try{
    // ...
}
catch (ExceptionA e1){
    e.printStackTrace();
}
catch (ExceptionB e2){
    e.printStackTrace();
}

try{
    // ...
}
catch (ExceptionA | ExceptionB e){
    e.printStackTrace();
}
```
```멀티 catch 블럭```을 이용하여 중복된 코드를 줄일 수 있다.

주의사항으로는 ```|``` 기호로 예외 클래스를 연결할 때, 조상과 자손의 관계라면 오류가 발생한다는 점이다.
두 예외 클래스가 조상과 자손의 관계라면, 그냥 조상 하나만 써도 동일한 동작을 수행하기 때문이다.

또한 멀티 catch는 하나의 catch 블럭으로 여러 예외 처리를 하는것이기 때문에 발생한 예외를 멀티 catch 블럭으로 처리하게 되었을 때, 멀티 catch 블럭 내에서는 실제로 어떤 예외가 발생한 것인지 알 수 없다.
그래서 참조변수 e로 멀티 catch 블럭에 ```|``` 기호로 연결된 예외 클래스들의 공통 분모인 조상 예외 클래스에 선언된 멤버만 사용할 수 있다.

### 1.6 예외 발생시키기
키워드 ```throw```를 사용하여 프로그래머가 고의로 예외를 발생시킬 수 있다.

```
. 먼저 연산자 new를 이용해서 발생시키려는 예외 클래스의 객체를 만든다.
    - Exception e = new Exception("고의로 발생시킴");
2. 키워드 throw를 이용하여 예외를 발생시킨다.
    - throw e;
```

```
class ExceptionEx09 {
	public static void main(String args[]) {
		try {
			Exception e = new Exception("고의로 발생시켰음.");
			throw e;	 // 예외를 발생시킴
		//  throw new Exception("고의로 발생시켰음.");  

		} catch (Exception e)	{
			System.out.println("에러 메시지 : " + e.getMessage());
		     e.printStackTrace();
		}
		System.out.println("프로그램이 정상 종료되었음.");
	}
}
_________결과_______
에러 메시지 : 고의로 발생시켰음.
java.lang.Exception: 고의로 발생시켰음.
프로그램이 정상 종료되었음.
	at ExceptionEx09.main(ExceptionEx09.java:4)
```

### 1.7 메소드에서 예외 선언하기
try-catch문 외에 예외를 메소드에 선언하여 예외를 처리할 수 있다.
```
void method() trhows Exception1, Exception2, Exception3 ... ExceptionN{
    // ...
}
```
이렇게 메소드에서 예외를 선언했을 때 발생한 예외는 해당 메소드를 호출한 곳에서 처리하여야 한다.

메소드를 작성할 때 메소드 내에서 발생할 가능성이 있는 예외를 메소드의 선언부에 명시하여 이 메소드를 사용하는 쪽에서는 이에 대한 처리를 하도록 강요하기 때문에 보다 견고한 프로그램 코드를 작성할 수 있도록 도와준다.
예외를 메소드의 throws에 명시하는 것은 예외를 처리하는 것이 아니라, 자신을 호출한 메소드에게 예외를 전달하여 예외처리를 떠맡기는 방식이다.

그러므로 호출한 메소드로 계속 이동하다가 마지막 main문에서도 발생한 예외를 처리하지 않는다면, main 메소드까지 종료되어 프로그램 전체가 종료된다.

그러므로 결국에는 어디에서든 반드시 try-catch로 예외처리를 해야 한다.

### 1.8 finally 블럭
finally 블럭은 try-catch문과 함께 예외의 발생여부에 상관없이 실행되어야 할 코드를 포함시킬 목적으로 사용된다.

```
try{
    // 예외가 발생할 가능성이 있는 문장을 넣는다.
}
catch (Exception1 e1){
    // 예외처리를 위한 문장을 적는다.
}
finally{
    // 예외의 발생여부에 관계없이 항상 수행되어야하는 문장을 넣는다.
    // finally 블럭은 try-catch문의 맨 마지막에 위치해야한다.
}
```
try 블럭에서 return문이 실행되었따 하더라도 finally 블럭의 문장들이 먼저 실행된 후에 현재 실행 중인 메소드가 종료된다.
이와 마찬가지로 catch 블럭의 문장 수행 중에 return문을 만나도 finally 블럭의 문장들이 수행된다.

### 1.9 자동 자원 변환 - try-with-resources 문
JDK 1.7부터 추가된 문법으로, 입출력 클래스와 같이 자원을 반환해줘야 하는 경우에 사용된다.
```
try{
    fis = new FileInputStream("score.dat");
    dis = new DataInputSTream(fis);
    
}
catch (IOException ie){
    ie.printStackTrace();
}
finally{
    try{
        if (dis != null)
            dis.close();
    }
    catch (IOException ie){
        ie.printStackTrace();
    }
}
```
위 예제와 같이 자원 반환을 의미하는 close 메소드 또한 예외가 발생할 수 있기 때문에 finally 블럭에서도 다시 한 번 try-catch문이 필요하다.
코드가 복잡해지기도 하고, try블럭과 finally블럭에서 모두 예외가 발생하면 try블럭의 예외는 무시된다.

이러한 점을 개선하기 위해 try-with-resources문이 추가되었다.
```
try (FileInputStream fis = new FileInputStream("score.dat");
     DataInputStream dis = new DataInputStream(fis))
{
    fis = new FileInputStream("score.dat");
    dis = new DataInputSTream(fis);
    
}
catch (IOException ie){
    ie.printStackTrace();
}
finally{
    try{
        if (dis != null)
            dis.close();
    }
    catch (IOException ie){
        ie.printStackTrace();
    }
}
```
try-with-resources문의 괄호()안에 객체를 생성하는 문장을 넣으면 이 객체는 따로 close()를 호출하지 않아도 try블럭을 벗어나는 순간 자동적으로 close()가 호출된다.
그 이후 catch블럭 또는 finally블럭이 수행된다.

이처럼 try-with-resources문에 의해 자동으로 객체의 close()가 호출될 수 있으려면 해당 클래스가 AutoCloseable이라는 인터페이스를 구현한 것이어야 한다.
```
void addSuppressed(Throwable exception) : 억제된 예외를 추가
Throwable[] getSuppressed() : 억제된 예외 (배열)을 반환
```

### 1.10 사용자정의 예외 만들기
```
class MyException extends Exception{
    MyException(String msg)    {
        super(msg);
    }
}
```
위와 같이 사용자정의 예외 클래스를 만들었다면, String을 매개변수로 받는 생성자를 추가해야한다.

기존에는 Exception을 주로 상속받아서 ```check예외```로 작성하는 경우가 많았지만, 요즘은 예외처리를 선택적으로 할 수 있도록 RuntimeException을 상속받아서 작성하는 쪽으로 바뀌어가고 있다.
```check예외```의 경우 반드시 예외처리를 해주어야 하기 때문에 예외처리가 불필요한 경우에도 try-catch문을 넣어서 코드가 복잡해지기 때문이다.

그러므로 최근에는 필요에 따라 예외처리의 여부를 선택할 수 있는 ```uncheck예외```가 강제적인 ```checked예외```보다 선호된다.

### 1.11 예외 되던지기
한 메소드에서 발생할 수 있는 예외가 여럿인 경우, 몇 개는 메소드 내부의 try-catch문을 통해서 메소드 내에서 자체적으로 처리하고, 나머지는 메소드 선언부에 지정하여 호출한 메소드에서 처리함으로 함으로써 양쪽에서 나눠서 처리되도록 할 수 있다.

이것은 예외를 처리한 후에 인위적으로 다시 발생시키는 방법을 통해서 가능한데, 이것을 ```예외 되던지기(exception re-throwing)```이라고 한다.

먼저 예외가 발생할 가능성이 있는 메소드에 try-catch문을 사용하여 예외를 처리한 이후 catch문에서 필요한 작업을 행한 후에 throw문을 사용해서 예외를 다시 발생시킨다.
다시 발생한 예외는 이 메소드를 호출한 메소드에게 전달되고 호출한 메소드의 try-catch문에서 예외를 다시 처리한다.

이 방법은 하나의 예외에 대해서 예외가 발생한 메소드와 이를 호출한 메소드 양쪽 모두에서 처리해줘야 할 작업이 있을 때 사용된다.

주의 사항으로 예외가 발생할 메소드에서는 try-catch문을 사용해서 예외처리를 해줌과 동시에 메소드의 선언부에 발생할 예외를 throws에 지정해줘야 한다.

### 1.11 연결된 예외(chained exception)
한 예외가 다른 예외를 발생시킬 수 있다.
예를 들어 예외 A가 예외 B를 발생시켰다면, A를 B의 원인 예외(cause exception)이라고 한다.

여러가지 예외를 하나의 큰 분류의 예외로 묶어서 다루기 위해 사용하거나, checked예외를 unchecked예외로 다루기 위해 사용한다.
```
try{
    startInstall(); // SpaceException 발생
    copyFiles();
}
catch (SpaceException e){
    InstallException ie = new InstallException("설치 중 예외발생"); // 예외 생성
    ie.initCause(e); // InstallException의 원인 예외를 SpaceException으로 지정
    throw ie; // InstallException 예외 발생
}
```
위와 같이 SpaceException 예외로 인해 InstallException 예외가 발생했다.
여기서 initCause(e) 클래스의 경우 Exception 클래스의 조상인 Throwable 클래스에 정의되어 있기 때문에 모든 예외에서 사용할 수 있다.

```
Throwable initCause(Throwable cause) : 지정된 예외를 원인 예외로 등록
Throwable getCause() : 원인 예외를 반환 
```
```
static void startInstall() throws SpaceException, MemoyException{
    if (!enoughSpace())
        throw new SpaceException("설치할 공간이 부족합니다.");
    if (!enoughMemory())
        throw new MemeoryException("메모리가 부족합니다.");
}
```
```
static void startInstall() throws SpaceException, MemoyException{
    if (!enoughSpace())
        throw new SpaceException("설치할 공간이 부족합니다.");
    if (!enoughMemory())
        throw new RuntimeException(MemoryException("메모리가 부족합니다."));
}
```
MemoryException은 Exception의 자손이므로 반드시 예외를 처리해야하는데, 이 예외를 RuntimeException으로 감싸버렸기 때문에(RuntimeException 클래스의 생성자 활용) unchecked예외가 된 상태이다.
그러므로 더 이상 startInstall()의 선언부에 MemeoryException을 선언하지 않아도 된다.




