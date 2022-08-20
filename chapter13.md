# 쓰레드(thread)
## 1.1 프로세스와 쓰레드
```프로세스(process)```란 ```실행 중인 프로그램(program)```이다.
프로그램을 실행하면 OS로부터 실행에 필요한 자원(메모리)을 할당받아 프로세스가 된다.

프로세스는 프로그램을 수행하는 데 필요한 데이터와 메모리 등의 자원 그리고 쓰레드로 구성되어 있으며 프로세스의 자원을 이용해서 실제로 작업을 수행하는 것이 쓰레드이다.

그러므로 모든 프로세스에는 최소한 하나 이상의 쓰레드가 존재하며, 둘 이상의 쓰레드를 가진 프로세스를 ```멀티쓰레드 프로세스(multi-threaded process)```라고 한다.
하나의 프로세스가 가질 수 있는 쓰레드의 개수는 제한되어 있지 않으나 쓰레드가 작업을 수행하는데 개별적인 메모리 공간(호출 스택)을 필요로 하기 때문에 프로세스의 메모리 한계에 따라 생성할 수 있는 쓰레드의 수가 결정된다.

### 멀티태스킹과 멀티쓰레딩
대부분의 OS는 멀티태스킹(multi-tasking, 다중 작업)을 지원하므로 여러 개의 프로세스가 동시에 실행될 수 있다.

멀티쓰레딩은 하나의 프로세스 내에서 여러 쓰레드가 동시에 작업을 수행하는 것이다.
CPU의 코어(core)가 한 번에 단 하나의 작업만 수행할 수 있으므로 실제로 동시에 처리되는 작업의 개수는 코어의 개수와 일치한다.
그러나 쓰레드의 수는 코어의 개수보다 많기 때문에 각 코어가 아주 짧은 시간 동안 여러 작업을 번갈아 가며 수행함으로써 여러 작업들이 모두 동시에 수행되는 것처럼 보이게 한다.

그래서 프로세스의 성능이 단순히 쓰레드의 비례하는 것은 아니다.

### 멀티쓰레딩의 장단점
```
CPU의 사용률을 향상시킨다.
자원을 보다 효율적으로 사용할 수 있다.
사용자에 대한 응답성이 향상된다.
작업이 분리되어 코드가 간결해진다.
```
여러 사용자에게 서비스를 해주는 서버 프로그램의 경우 멀티 쓰레드로 작성하는 것은 필수적이어서 하나의 서버 프로세스가 여러 개의 쓰레드를 생성해서 쓰레드와 사용자의 요청이 일대일로 처리되도록 프로그래밍 해야 한다.

만약 싱글쓰레드로 서버 프로그램을 작성한다면 사용자의 요청마다 새로운 프로세스를 생성해야하는데 프로세스를 생성하는 것은 쓰레드를 생성하는 것에 비해 더 많은 시간과 메모리 공간이 필요하기 때문에 많은 수의 사용자 요청을 서비스하게 어렵다.

단점으로는 ```동기화(synchronization)```, ```교착상태(dead)```와 같은 문제들이 있다.

## 1.2 쓰레드의 구현과 실행
쓰레드를 구현하는 방법은 ```Thread``` 클래스를 상속받는 방법과 ```Runnable``` 인터페이스를 구현하는 방법, 총 두 가지가 있다.

```Thread``` 클래스를 상속받을 시 다른 클래스를 상속받을 수 없기 때문에 ```Runnable``` 인터페이스를 구현하는 것이 일반적이다.

```Runnable``` 인터페이스를 구현하는 방법은 ```재사용성(reusability)```이 높고 코드의 ```일관성(consistency)```를 유지할 수 있기 때문에 보다 객체지향적인 방법이라고 볼 수 있다.

```java
// Thread 클래스 상속
class MyThread extends Thread{
    public void run(){ // Thread 메소드의 run()을 오버라이딩
        ...
    }
}

// Runnable 인터페이스 구현
class MyThread implements Runnable{
    public void run(){ // Runnable 인터페이스의 run()을 구현
        ...
    }
}
```
```Runnable``` 인터페이스는 오직 ```run()```만 정의되어 있다.

쓰레드를 구현한다는 것은 위의 두 방법 중 어떤 것을 선택하던지 그저 쓰레드를 통해 작업하고자 하는 내용으로 run()의 몸통{}을 채우는 것일 뿐이다.
```java
class ThreadEx01 {
	public static void main(String args[]) {
		ThreadEx1_1 t1 = new ThreadEx1_1();

		Runnable r  = new ThreadEx1_2();
		Thread   t2 = new Thread(r);	  // 생성자 Thread(Runnable target)

		t1.start();
		t2.start();
	}
}

class ThreadEx1_1 extends Thread {
	public void run() {
		for(int i=0; i < 5; i++) {
			System.out.println(getName()); // 조상인 Thread의 getName()을 호출
		}
	}
}

class ThreadEx1_2 implements Runnable {
	public void run() {
		for(int i=0; i < 5; i++) {
			// Thread.currentThread() - 현재 실행중인 Thread를 반환한다.
		    System.out.println(Thread.currentThread().getName());
		}
	}
}
```
위 예제는 ```Thread``` 클래스를 상속받은 경우와 ```Runnable```을 상속받은 경우의 인스턴스 생성 방식의 차이를 확인하기 위한 예제이다.
```java
ThreadEx1_1 t1 = new ThreadEx1_1(); // Thread의 자손 클래스의 인스턴스를 생성

Runnable r = new ThreadEx1_2(); // Runnable을 구현한 클래스의 인스턴스를 생성
Thread t2 = new Thread(r); // 생성자 Thread(Runnable target)

Thread t2 = new Thread(new ThreadEx1_2()); // 위의 두 줄을 하나로 통합
```
Runnable 인터페이스를 구현한 경우, Runnable 인터페이스를 구현한 클래스의 인스턴스를 생성한 다음 이 인스턴스를 Thread 클래스의 생성자의 매개변수로 제공해야 한다.
```
public class Thread{
    private Runnable r; // Runnable을 구현한 클래스의 인스턴스를 참조하기 위한 변수
    
    public Thread(Runnable r)    {
        this.r = r;
    }
    
    public void run()    {
        if (r != null)
            r.run(); // Runnable 인터페이스를 구현한 인스턴스의 run() 호출
    }
}
```
Thread 클래스는 Runnable 타입의 변수 r을 인스턴스로 선언해 놓고 생성자를 통해서 Runnable 인터페이스를 구현한 인스턴스를 참조하도록 되어 있다.
이를 통해 run()을 호출 시 참조변수 r을 통해서 Runnable 인터페이스를 구현한 인스턴스의 run()을 호출하고 있음을 확인할 수 있다.

위 방식은 상속을 통해 run()을 오버라이딩 하지 않고도 외부로부터 run()을 제공받을 수 있게 된다.

Thread 클래스를 상속받으면 자손 클래스에서 조상인 Thread 클래스의 메소드를 직접 호출할 수 있지만 Runnable을 구현하면 Thread 클래스의 static 메소드인 currentThread()를 호출하여 쓰레드에 대한 참조를 얻어 와야만 호출이 가능하다.
```java
static Thread currentThread() : 현재 실행중인 쓰레드의 참조를 반환한다.
String getName() : 쓰레드의 이름을 반환한다.
```
그러므로 ThreadEx1_2에서 getName()을 호출하기 위해서는 ```Thread.currentThread.getName()```과 같이 실행중인 쓰레드의 참조를 가져와야한다.

쓰레드의 이름은 다음과 같이 생성자나 메소드를 통하여 지정 또는 변경할 수 있다.
```java
Thread(Runnable target, String name)
Thread(String name)
void setName(String name)
```
### 쓰레드의 실행 - start()
쓰레드를 생성했다고 해서 자동으로 실행되는 것은 아니고, start() 메소드를 호출해야만 쓰레드가 실행된다.

start() 메소드가 호출되었다고 해서 바로 실행되는 것이 아니라, 일단 실행되기 상태에 있다가 자신의 차례가 되어야 실행된다.
실행대기중인 쓰레드가 하나도 없을 시에는 곧장 실행상태가 된다.

쓰레드의 실행순서는 OS의 스케쥴러가 작성한 스케쥴에 의해 결정된다.

그리고 한 번 실행이 종료된 쓰레드는 다시 실행햘 수 없다.
즉 하나의 쓰레드에 대해 start()가 한 번만 호출될 수 있다는 것이다.

그러므로 쓰레드의 작업을 한 번 더 수행해야 한다면 새로운 쓰레드의 인스턴스를 생성하고 start()를 호출해야 한다.

그렇지 않는다면 ```IllegalThreadStateException```이 발생한다.

## 1.3 start()와 run()
main 메소드에서 run()을 호출하는 것은 생성된 쓰레드를 실행시키는 것이 아니라 단순히 클래스에 선언된 메소드를 호출하는 것일 뿐이다.
start()는 새로운 쓰레드가 작업을 실행하는데 필요한 호출스택(call stack)을 생성한 다음에 run()을 호출해서 생성된 호출스택에 run()을 첫 번째로 올라가게 한다.

모든 쓰레드는 독립적인 작업을 수행하기 위해 자신만의 호출스택을 필요로 하기 때문에 새로운 쓰레드를 생성하고 실행시킬 때마다 새로운 호출스택이 생성되고 쓰레드가 종료되면 작업에 사용된 호출스택은 소멸된다.
```
1. main 메소드에서 쓰레드의 start()를 호출한다.
2. start()는 새로운 쓰레드를 생성하고, 쓰레드가 작업하는데 사용될 호출스택을 생성한다.
3. 새로 생성된 호출스택에 run()이 호출되어, 쓰레드가 독립된 공간에서 작업을 수행한다.
4. 이제는 호출스택이 2개이므로 스케쥴러가 정한 순서에 의해 번갈아 가면서 실행된다.
```
호출스택에서는 가장 위에 있는 메소드가 현재 실행중인 메소드이고 나머지 메소드들은 대기상태에 있는 상황이다.
그러나 위의 그림에서와 같이 쓰레드가 둘 이상일 때는 호출스택의 최상위에 있는 메소드라고 할지라도 대기상태에 있을 수 있다.

스케쥴러는 실행 대기중인 쓰레드들의 우선수위를 고려하여 실행순서와 실행시간을 결정하고, 각 쓰레드들은 작성된 스케줄에 따라 자신의 순서가 되면 지정된 시간동안 작업을 수행한다.

이 때 주어진 시간동안 작업을 마치지 못한 쓰레드는 다시 자신의 차례가 돌아올 때 까지 대기상태로 있게 되며, 작업을 마친 쓰레드, 즉 run()의 수행이 종료된 쓰레드는 호출스택이 모두 비워지면서 이 쓰레드가 사용하던 호출스택은 사라진다.

### main 쓰레드
main 메소드의 작업을 수행하는 것도 쓰레드이며, 이를 main 쓰레드라고 한다.

프로그램을 실행하면 기본적으로 하나의 쓰레드를 생성하고, 그 쓰레드가 main 메소드를 호출해서 작업이 수행된다.
main 쓰레드 하나만 존재할 경우 main 메소드가 종료되면 프로그램이 종료되지만, 위와 같이 main 메소드가 수행을 다 마쳤다고 하더라도 다른 쓰레드가 아직 작업을 마치지 않은 상태라면 프로그램이 종료되지 않는다.

```
실행 중인 사용자 쓰레드가 하나도 없을 때 프로그램은 종료된다.
```
```java
 class ThreadEx02 {
	public static void main(String args[]) throws Exception {
		ThreadEx2_1 t1 = new ThreadEx2_1();
		t1.start();
	}
}

class ThreadEx2_1 extends Thread {
	public void run() {
		throwException();
	}

	public void throwException() {
		try {
			throw new Exception();		
		} catch(Exception e) {
			e.printStackTrace();	
		}
	}
}
```
해당 예제는 새로 생성한 쓰레드에서 고의로 예외를 발생시키고 printStackTrace()를 이용해서 예외가 발생한 그 당시의 호출스택을 출력하는 예제이며, 실행 결과는 다음과 같다.
```
java.lang.Exception
	at ThreadEx2_1.throwException(ThreadEx02.java:15)
	at ThreadEx2_1.run(ThreadEx02.java:10)
```
호출스택의 첫 번째 메소드가 main 메소드가 아닌 run 메소드임을 확인할 수 있다.

한 쓰레드에서 예외가 발생되서 종료되더라도 다른 쓰레드에 영향을 미치지 않기 때문에, main 쓰레는 새로 생성한 쓰레드를 start() 시킨 이후 종료되기 때문에 호출 스택에 존재하지 않는다.
```java
class ThreadEx03 {
	public static void main(String args[]) throws Exception {
		ThreadEx3_1 t1 = new ThreadEx3_1();
		t1.run();
	}
}

class ThreadEx3_1 extends Thread {
	public void run() {
		throwException();
	}

	public void throwException() {
		try {
			throw new Exception();		
		} catch(Exception e) {
			e.printStackTrace();	
		}
	}
}
```
위 예제는 start()가 아닌 run()을 호출하여 새로운 호출스택이 아닌 기존의 호출스택에 쓰레드의 run() 메소드를 호출하는 예제이며 실행 결과는 다음과 같다.
```
java.lang.Exception
	at ThreadEx3_1.throwException(ThreadEx03.java:15)
	at ThreadEx3_1.run(ThreadEx03.java:10)
	at ThreadEx03.main(ThreadEx03.java:4)
```
그러므로 예외 메세지에 호출 스택에 main 메소드까지 포함되어있는 것을 확인할 수 있다.

## 1.4 싱글쓰레드와 멀티쓰레드
두 개의 작업을 하나의 쓰레드(th1)로 처리하는 경우와 두 개의 쓰레드(th1, th2)로 처리하는 경우를 가정할 경우, 하나의 쓰레드로 두 작업을 처리하는 경우는 한 작업을 마친 후에 다른 작업을 시작하지만, 두 개의 쓰레드로 작업하는 경우는 짧은 시간동안 2개의 쓰레드(th1, th2)가 번갈아 가면서 작업을 수행해서 동시에 두 작업이 처리되는 것과 같이 느끼게 된다.
하나의 쓰레드로 두 개의 작업을 수행한 시간과 두개의 쓰레드로 두 개의 작업을 수행한 시간은 거의 같다.
오히려 두 개의 쓰레드로 작업한 시간이 싱글쓰레드로 작업한 시간보다 더 걸리게 되는데 그 이유는 쓰레드간의 작업 전환(context switcing)에 시간이 걸리기 때문이다.

작업 전환을 할 때는 현재 진행 중인 작업의 상태, 예를 들면 다음에 실행해야할 위치(PC, 프로그램 카운터) 등의 정보를 저장하고 읽어 오는 시간이 소요된다.

그래서 싱글 코어에서 단순히 CPU만을 사용하는 계산작업이라면 오히려 멀티쓰레드보다 싱글쓰레드로 프로그래밍하는 것이 더 효율적이다.
```java
 class ThreadEx04 {
	public static void main(String args[]) {
		long startTime = System.currentTimeMillis();

		for(int i=0; i < 500; i++)
			System.out.printf("%s", new String("-"));		

		System.out.print("소요시간1:" +(System.currentTimeMillis()- startTime)); 

		for(int i=0; i < 500; i++) 
			System.out.printf("%s", new String("|"));		

		System.out.print("소요시간2:"+(System.currentTimeMillis() - startTime));
	}
}
```
위 예제는 ```-```를 출력하는 작업과 ```|```를 출력하는 작업을 하나의 쓰레드가 연속적으로 처리하는 시간을 측정하는 예제이며, 실행 결과는 다음과 같다.
컴퓨터의 성능이나 실행환경에 의해 변경될 수 있다.

### 멀티쓰레드로 실행했을 때 시간이 더 걸린 이유는 두 가지가 있다.
```
두 쓰레드가 번갈아가면서 작업을 처리하므로 쓰레드간의 작업전환시간이 소요된다.
한 쓰레드가 화면에 출력하고 있는 동안 다른 쓰레드는 출력이 끝나기를 기다려야 하는데, 이 때 발생하는 대기시간 때문이다.
```
싱글 코어일 경우 멀티쓰레드라도 하나의 코어가 번갈아가면서 작업을 수행하기 때문에 두 작업이 절대 겹치지 않는다.

멀티 코어일 경우 멀티쓰레드로 두 작업을 수행하면 동시에 두 쓰레드가 수행될 수 있으므로 두 작업 A, B가 겹치는 부분이 발생한다.
그러므로 화면(console)이라는 자원을 놓고 두 쓰레드가 경쟁하게 된다.

위 예제는 실행할 때마다 다른 결과를 얻을 수 있는데, 그 이유는 프로그램이 OS의 프로세스 스케줄러의 영향을 받기 때문이다.
매 순간 상황에 따라 프로세스에게 할당되는 실행시간이 일정하지 않고 쓰레드에게 할당되는 시간 역시 일정하지 않으므로, 쓰레드는 이러한 불확실성을 가지고 있다는 것을 염두해야 한다.

자바가 OS(플랫폼) 독립적이라고 하지만 실제로는 OS 종속적인 부분이 몇 부분 있는데 쓰레드도 그 중의 하나이다.

두 쓰레드가 서로 다른 자원을 사용하는 작업의 경우 싱글쓰레드 프로세스보다 멀티쓰레드 프로세스가 더 효율적이다.
사용자로부터 입력받는 작업(A)과 화면에 출력하는 작업(B)을 하나의 쓰레드로 처리한다면 첫 번째 그래프처럼 사용자가 입력을 마칠 때까지 아무 일도 하지 않고 기다리기만 해야 한다.

그러나 두 개의 쓰레드로 처리한다면 사용자의 입력을 기다리는 동안 다른 쓰레드가 작업을 처리할 수 있기 때문에 보다 효율적인 CPU의 사용이 가능하다.

## 1.5 쓰레드의 우선순위
쓰레드는 ```우선순위(priority)```라는 속성(멤버변수)을 가지고 있는데, 이 우선순위의 값에 따라 쓰레드가 얻는 실행시간이 달라진다.

쓰레드가 수행하는 작업의 중요도에 따라 쓰레드의 우선순위를 서로 다르게 지정하여 특정 쓰레드가 더 많은 작업시간을 갖도록 할 수 있다.

주로 시각적인 부분이나 사용자에게 빠르게 반응해야하는 작업을 하는 쓰레드의 우선순위는 다른 작업을 수행하는 쓰레드의 비해 높다.

### 쓰레드의 우선순위 지정하기
쓰레드의 우선순위와 관련된 메소드와 상수는 다음과 같다.
```java
void setPriority(int newPriority) : 쓰레드의 우선순위를 지정한 값으로 변경한다.
int getPriority() : 쓰레드의 우선순위를 반환한다.
```
쓰레드가 가질 수 있는 우선순위의 범위는 1 ~ 10이며 숫자가 높을수록 우선순위가 높다.
또한 쓰레드의 우선순위는 쓰레드를 생성한 쓰레드로부터 상속받는다.
main 메소드를 수행하는 쓰레드는 우선순위가 6이며, 그러므로 main 쓰레드 내에서 생성하는 쓰레드의 우선순위는 자동으로 5가 된다.

우선순위의 세팅은 쓰레드를 실행하기 전에만 변경할 수 있다.
우선순위가 같다면 각 쓰레드에게 거의 같은 양의 실행시간이 주어지지만, 우선순위가 다르다면 우선순위가 높은 th1에게 상대적으로 th2보다 더 많은 양의 실행시간이 주어지고 결과적으로 작업 A가 B보다 빨리 완료될 수 잇다.

다만 멀티코어에서는 쓰레드의 우선순위에 따른 차이가 거의 없다.
이 경우 쓰레드에 높은 우선순위를 주면 더 많은 실행시간과 실행기회를 갖게 될 것이라고 기대할 수 밖에 없다.

쓰레드는 OS의 스케쥴러에 종속저이여서 어느 정도 예측만 가능한 정도일 뿐이므로 차라리 작업에 우선순위를 두어 PriorityQueue에 저장하고, 우선순위가 높은 작업이 먼저 처리되도록 하는 것이 나을 수 있다.

## 1.6 쓰레드 그룹
쓰레드 그룹은 서로 관련된 쓰레드를 그룹으로 다루기 위한 것으로, 쓰레드 그룹을 생성해서 쓰레드를 그룹으로 묶어서 관리할 수 있다.

쓰레드 그룹에 다른 쓰레드 그룹을 포함시킬 수 있다.

쓰레드 그룹은 보안상의 이유로 도입된 개념으로, 자신이 속한 쓰레드 그룹이나 하위 쓰레드 그룹은 변경할 수 있지만 다른 쓰레드 그룹의 쓰레드를 변경할 수 없다.

ThreadGroup을 사용해서 생성할 수 있으며, 주요 생성자와 메소드는 다음과 같다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185662959-8be88d29-48f8-4838-b22d-1da6a0a30b07.png)
<br>
쓰레드를 쓰레드 그룹에 포함시키려면 Thread의 생성자를 이용해야 한다.
```
Thread(ThreadGroup group, String name)
Thread(ThreadGroup group, Runnable target)
Thread(ThreadGroup group, Runnable target, String name)
Thread(ThreadGroup group, Runnable target, String name, long stackSize)
```
모든 쓰레드는 반드시 쓰레드 그룹에 포함되어 있어야 하기 때문에, 위와 같이 쓰레드 그룹을 지정하는 생성자를 사용하지 않은 쓰레드는 기본적으로 자신을 생성한 쓰레드와 같은 쓰레드 그룹에 속하게 된다.

자바 애플리케이션이 실행되면, JVM은 main과 system이라는 쓰레드 그룹을 만들고 JVM운영에 필요한 쓰레드들을 생성해서 이 쓰레드 그룹에 포함시킨다.
```
main 쓰레드 -> main 쓰레드 그룹
Finalizer 쓰레드(GC) -> system 쓰레드 그룹
```
이 외에 Thread의 쓰레드 그룹과 관련된 메소드는 다음과 같다.
```
ThreadGroup getThreadGroup() : 쓰레드 자신이 속한 쓰레드 그룹을 반환한다.
void uncaughtException(Thread t, Throwable e) : 쓰레드 그룹의 쓰레드가 처리되지 않은 예외에 의해 실행이 종료되었을 때, JVM에 의해 이 메소드가 자동적으로 호출된다.
```

## 1.7 데몬 쓰레드
데몬 쓰레드는 다른 일반 쓰레드(데몬 쓰레드가 아닌 쓰레드)의 작업을 돕는 보조적인 역할을 수행하는 쓰레드이다.
일반 쓰레드가 모두 종료되면 데몬 쓰레드는 강제적으로 자동 종료된다.
그 외에는 일반 쓰레드와 동일하다.

데몬 쓰레드는 무한루프와 조건문을 이용해서 실행 후 대기하고 있다가 특정 조건이 만족되면 작업을 수행하고 다시 대기하도록 작성한다.

데몬 쓰레드는 일반 쓰레드의 작성방법과 실행방법이 같으며 다만 쓰레드를 생성한 다음 실행하기 전에 setDaemon(true)를 호출하면 된다.

데몬 쓰레드가 생성한 쓰레드는 자동적으로 데몬 쓰레드가 된다.
```java
boolean isDaemon() : 쓰레드가 데몬 쓰레드인지 확인한다.
void setDaemon(boolean on) : 쓰레드를 데몬 쓰레드로 또는 사용자 쓰레드로 변경한다. 매개변수 on의 값을 true로 지정하면 데몬 쓰레드가 된다.
```

## 1.8 쓰레드의 실행제어
쓰레드 프로그래밍이 어려운 이유는 ```동기화(synchronization)```과 ```스케줄링(scheduling)``` 때문이다.

효울적인 멀티쓰레드 프로그램을 만들기 위해서는 보다 정교한 스케줄링을 통해 프로세스에게 주어진 자원과 시간을 여러 쓰레드가 낭비없이 잘 사용하도록 프로그래밍 해야한다.

쓰레드의 스케줄링과 관련된 메소드는 다음과 같다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185663339-4571ec22-179c-4094-a004-9e153f8e8ff7.png)
<br>
쓰레드의 상태는 다음과 같다
![image](https://user-images.githubusercontent.com/62749021/185663425-33de5e19-923a-402c-b7f1-bd89505b05c5.png)

### 쓰레드의 라이프 사이클
```
1. 쓰레드를 생성하고 start()를 호출하면 바로 실행되는 것이 아니라 실행대기열에 저장되어 자신의 차례가 될 때까지 기다린다. 실행대기열은 큐(queue)와 같은 구조로 먼저 실행대기열에 들어온 쓰레드가 먼저 실행된다.
2. 실행대기상태에 있다가 자신의 차례가 되면 실행상태가 된다.
3. 주어진 실행시간이 다되거나 yield()를 만나면 다시 실행대기상태가 되고 다음 차례의 쓰레드가 실행상태가 된다.
4. 실행 중에 suspend(), sleep(), wait(), join(), I/O block에 의해 일시정지상태가 될 수 있다. 이런 경우 일시정지 상태에 있다가 처리가 끝나면 다시 실행대기 상태가 된다.
5. 지정된 일시정지시간이 다되거나(time-out), notify(), resume(), interrupt()가 호출되면 일시정지상태를 벗어나 다시 실행대기열에 저장되어 자신의 차례를 기다리게 된다.
6. 실행을 모두 마치거나 stop()이 호출되면 쓰레드는 소멸된다.
```

### sleep(long millis) - 일정시간동안 쓰레드를 멈추게 한다.
sleep() 메소드의 원형은 다음과 같다.
```
static void sleep(long millis)
static void sleep(long millis, int nanos)
```
밀리세컨드(millis, 1000분의 1초)와 나노세컨드(nanos, 10억분의 일초)의 단위로 값을 세밀하게 지정할 수 있지만 어느 정도 오차가 발생할 수 있다.

sleep()에 의해 일시정지 상태가 된 쓰레드는 지정된 시간이 다 되거나 interrupt()가 호출되면(InterruptedException 발생), 실행대기 상태가 된다.

그래서 sleep()을 호출할 때에는 항상 try-catch문으로 예외를 처리해야한다.
```java
void delay(long millis){
    try    {
        Thread.sleep(millis);
    }
    catch (InterruptException c){
    ...
    }
}
```
위와 같이 sleep()에 try-catch까지 포함하는 새로운 메소드를 생성하는 것이 일반적이다.

### interrupt()와 interrupted() - 쓰레드의 작업을 취소한다.
interrupt()는 쓰레드에게 작업을 멈추라고 요청한다.
단지 머무라고 요청만 하는 것일 뿐 쓰레드를 강제로 종료시키지는 못한다.
즉 interrupt()는 쓰레드의 interrupted 상태를 바꾸는 것일 뿐이다.

interrupted()는 쓰레드에 대해 interrupt()가 호출되었는지 알려준다.

isInterrupted()도 쓰레드의 interrupt()가 호출되었는지 확인하는데 사용할 수 있지만 interrupted()와 달리 isInterrupted()는 쓰레드의 interrupted 상태를 false로 초기화하지 않는다.
```java
void interrupt() : 쓰레드의 interrupted 상태를 false에서 true로 변경
boolean isInterrupted() : 쓰레드의 interrupted 상태를 반환
static boolean interrupted() : 현재 쓰레드의 interrupted 상태를 반환 후, false로 변경
```
쓰레드가 sleep(), wait(), join()에 의해 일시정지 상태(WAITING)에 있을 때, 해당 쓰레드에 대해 interrupt()를 호출하면 sleep(), wait(), join()에서 InterruptedException이 발생학 쓰레드는 실행대기 상태(RUNNABLE)로 바뀐다.

### suspend(), resume(), stop()
suspend()는 sleep()처럼 쓰레드를 멈추게 한다.
suspend()에 의해 정지된 쓰레드는 resume()을 호출해야 다시 실행대기 상태가 된다.
stop()은 호출되는 즉시 쓰레드가 종료된다.

여기서 suspend()와 stop()은 교착상태(deadlock)을 일으키기 쉽게 되어있으므로 사용이 권장되지 않아 deprecated가 된 상태이다.

### yield() - 다른 쓰레드에게 양보한다.
yield()는 쓰레드 자신에게 주어진 실행시간을 다음 차례의 쓰레드에게 양보(yield)한다.

yield()와 interrupt()를 적절히 사용하면 프로그램의 응답성을 높이고 보다 효율적인 실행이 가능하게 할 수 있다.

### join() - 다른 쓰레드의 작업을 기다린다.
쓰레드 자신이 하던 작업을 잠시 멈추고 다른 쓰레드가 지정된 시간동안 작업을 수행하도록 할 때 join()을 사용한다.
```java
void join()
void join(long millis)
void join(long millis, int nanos)
```
시간을 지정하지 않으면 해당 쓰레드가 작업을 모두 마칠 때까지 기다리게 된다.
작업 중에 다른 쓰레드의 작업이 먼저 수행되어야할 필요가 있을 때 join()을 사용한다.

join()도 sleep()처럼 interrupt()에 의해 대기상태에서 벗어날 수 있으며, join()이 호출되는 부분을 try-catch 문으로 감사야 한다.

join()과 sleep()은 유사한 점이 많은데, 차이점으로는 join()은 현재 쓰레드가 아닌 특정 쓰레드에 대해 동작하므로 static 메소드가 아니다.

## 1.9 쓰레드의 동기화
싱글쓰레드 프로세스의 경우 프로세스 내에서 단 하나의 쓰레드만 작업하기 때문에 프로세스의 자원을 가지고 작업하는데 문제가 없지만 멀티쓰레드 프로세스의 경우 여러 쓰레드가 같은 프로세스 내에 자원을 공유해서 작업하기 때문에 서로의 작업에 영향을 주게 된다.

이러한 일이 발생하는 것을 방지하기 위해서 한 쓰레드가 특정 작업을 끝마치기 전까지 다른 쓰레드에 의해 방해받지 않도록 하는 것이 필요하다.

그래서 도입된 개념이 바로 임계 영역(critical section)과 잠금(락, lock) 이다.

공유 데이터를 사용하는 코드 영역을 임계 영역으로 지정해놓고 공유 데이터(객체)가 가지고 있는 lock을 획득한 단 하나의 쓰레드만 이 영액 내의 코드를 수행할 수 있게 한다.
그리고 해당 쓰레드가 임계 영역 내의 모든 코드를 수행하고 벗어나서 lock을 반납해야만 다른 쓰레드가 반납된 lock을 획독하여 임계 영역의 코드를 수행할 수 있게 된다.

이처럼 한 쓰레드가 진행 중인 작업을 다른 쓰레드가 간섭하지 못하도록 막는 것을 쓰레드의 동기화(synchronization)이라고 한다.

## 1.10 synchronized를 이용한 동기화
이 키워드는 임계 영역을 설정하는데 사용된다.
```java
1. 메소드 전체를 임계 영역으로 지정
public synchronized void calcSum(){
}

2. 특정한 영역을 임계 영역으로 지정
synchronized(객체의 참조변수){
}
```
첫 번째 방법은 메소드 앞에 synchronized를 붙이는 것인데 synchronized를 붙이면 메소드 전체가 임계 영역으로 설정된다.
쓰레드는 synchronized 메소드가 호출된 시점부터 해당 메소드가 포함된 객체의 lock을 얻어 작업을 수행하다가 메소드가 종료되면 lock을 반환한다.

두 번째 방법은 메소드 내의 코드 일부를 블럭{}으로 감싸고 블럭 앞에 synchronized(참조변수)를 붙이는 것인데, 이 때 참조변수는 락을 걸고자하는 객체를 참조하는 것이어야 한다.
이 블럭을 synchronized 블럭이라고 하며, 이 블럭의 영역 안으로 들어가면서부터 쓰레드는 지정된 객체의 lock을 얻게 되고, 이 블럭을 벗어나면 lock을 반환한다.

두 방법 모두 lock의 획득과 반납이 모두 자동적으로 이루어지므로 임계 영역만 설정해주면 된다.

모든 객체는 lock을 하나씩 가지고 있으며, 해당 객체의 lock을 가지고 있는 쓰레드만 임계 영역의 코드를 수행할 수 있다.

임계 영역은 멀티쓰레드 프로그램의 성능을 좌우하기 때문에 가능하면 메소드 전체에 락을 거는 것보다는 synchronized 블럭으로 임계영역을 최소화해서 보다 효율적인 프로그램이 되도록 노력해야 한다.

## 1.11 wait()와 notify()
특정 쓰레드가 객체의 락을 가진 채로 오랜 시간을 보내지 않도록 해야 한다.
이를 처리하기 위해 고안된 것이 wait()와 notify()이다.

동기화된 임계영역의 코드를 수행하다가 작업을 더 이상 진행할 상황이 아니면 wait()를 호출하여 쓰레드가 락을 반납하고 기다리게 한다.
그러면 다른 쓰레드가 락을 얻어 해당 객체에 대한 작업을 수행할 수 있게 된다.
나중에 작업을 진행할 수 있는 상황이 되면 notify()를 호출해서 작업을 중단했던 쓰레드가 다시 락을 얻어 진행할 수 있게 된다.

다만 이 경우 오랜 시간동안 기다린 쓰레드가 즉시 락을 얻는다는 보장이 없다.
wait()가 호출되면 실행 중이던 쓰레드는 해당 객체의 대기실(waiting pool)에서 통지를 기다린다.
notify()가 호출되면 해당 객체의 대기실에 있던 모든 쓰레드 중 임의의 쓰레드만 통지를 받는다.
notifyAll()은 기다리고 있는 모든 쓰레드에게 통보를 하지만 lock을 얻을 수 있는 것은 대기실에 있던 쓰레드 중 하나의 쓰레드일 뿐이다.

이러한 wait()와 notify()는 특정 객체에 대한 것이므로 Object 클래스에 정의되어있다.
```
void wait()
void wait(long timeout)
void wait(long timeout, int nanos)
void notify()
void notifyAll()
```
wait()는 notify() 또는 notifyAll()이 호출될 때까지 기다리지만 매개변수가 있는 wait()는 지정한 시간동안만 기다린다. 즉 지정된 시간이 지난 후에 자동적으로 notify()를 호출하는 것과 같다.

그리고 waiting pool은 객체마다 존재하는 것이므로 notifyAll()이 호출된다고 해서 모든 객체의 waiting pool에 있는 쓰레드가 깨워지는 것은 아니다.
notifyAll()이 호출된 객체의 waiting pool에 대기중인 쓰레드만 해당된다.
```
wait(), notify(), notifyAll()
    - Object에 정의되어 있다.
    - 동기화 블록(synchronized 블록) 내에서만 쓸 수 있다.
    - 보다 효율적인 동기화를 가능하게 한다.
```
### 기아 현상과 경쟁 상태
특정 쓰레드가 락을 얻지 못하고 무한정 대기하는 상태를 ```기아(starvation)``` 현상이라고 한다.

이러한 현상을 막는 방법 중 하나는, notify()가 아닌 notifyAll()을 사용하는 것이다.

여러 쓰레드끼리 하나의 lock을 얻기 위해 서로 경쟁하는 것을 경쟁 상태(race condition)이라고 한다.
이는 선별적인 통지를 통해 해결할 수 있으며, wait()와 notify()로는 불가능하다.

## 1.12 13-12. Lock과 Condition을 이용한 동기화
동기화는 synchronized 블럭의외에도 java.util.concurrent.locks 패키지가 제공하는 lock 클래스들을 이용하는 방법도 있다.

synchronized 블럭으로 동기화를 하면 자동적으로 lock이 잠기고 풀리기 때문에 편리하지만, 같은 메소드 내에서만 lock을 걸 수 있다는 제약이 있다.

이러한 제약을 해결하기 위한 방법으로 lock 클래스를 사용한다.
```
ReentrantLock : 재진입이 가능한 lock. 가장 일반적인 배타lock
ReentrantReadWriteLock : 읽기에는 공유적이고 쓰기에는 배타적인 lock
StampedLock : ReentrantReadWriteLock에 낙관적인 lock의 기능을 추가
```
ReentranLock은 가장 일반적인 lock이다.

ReentrantReadWriteLock은 읽기 / 쓰기를 위한 lock을 제공한다.
ReentrantLock은 배타적인 lock이라서 무조건 lock이 있어야만 임계영역의 코드를 수행할 수 있지만 ReentrantReadWriteLock은 읽기 lock이 걸려있으면 다른 쓰레드가 읽기 lock을 중복해서 걸고 읽기를 수행할 수 있다.
읽기는 내용을 변경하지 않으므로 동시에 여러 쓰레드가 읽어도 문제가 되지 않기 때문이다.
단 읽기 lock이 걸린 상태에서 쓰기 lock을 거는 것은 허용되지 않으며, 그 반대도 마찬가지이다.

StampedLock은 lock을 걸거나 해지할 때 스탬프(long 타입의 정수값)을 사용하며, 읽기와 쓰기를 위한 lock 외에 낙관적 읽기 lock(optimistic reading lock)가 추가된 클래스이다.
읽기 lock이 걸려있으면 쓰기 lock을 걸기 위해서는 읽기 lock이 풀릴 때까지 기다려야 하는데 비해 낙관적 읽기 lock(optimistic reading lock)은 쓰기 lock에 의해 바로 풀린다.
무조건 읽기 lock을 걸지 않고, 쓰기와 읽기가 충돌할 때만 쓰기가 끝난 후 읽기 lock을 거는 것이다.
```java
int getBalance(){
    long stamp = lock.tryOptimisticRead(); // 낙관적 읽기 lock을 건다.
    
    int curBalance = this.balance; // 공유 데이터인 balance를 읽어온다.
    
    if (!lock.validate(stamp)){ // 쓰기 lock에 의해 낙관적 읽기 lock이 풀렸는지 확인
        stamp = lock.readLock(); // lock이 풀렸으면 읽기 lock을 얻으려고 기다린다.
        
        try{
            curBalance = this.balance; // 공유 데이터를 다시 읽어온다.
        }
        finally{
            lock.unlockRead(stamp); // 읽기 lcok을 푼다.
        }
    }
    return curBalance; // 낙관적 읽기 lock이 풀리지 않았다면 곧바로 읽어온 값을 반환
}
```
### ReentrantLock의 생성자
ReentrantLock은 다음과 같이 두 개의 생성자를 가지고 있다.
```java
ReentrantLock()
ReentrantLock(boolean fair)
```
생성자의 매개변수로 true를 주면 lock 풀렸을 때 가장 오래 기다린 쓰레드가 lock을 획득할 수 있게(공정하게) 처리한다.
공정하게 처리할 경우 어떤 쓰레드가 가장 오래 기다렸는지 확인해야하므로 성능이 떨어진다.

대부분의 경우 굳이 공정하게 처리하지 않아도 되므로 공정하게 처리하기보다는 성능을 선택한다.
```
void lock() : lock을 잠근다.
void unlock() : lock을 해제한다.
boolean isLocked() : lock이 잠겼는지 확인한다.
```
ReentranLock은 가장 일반적인 lock이다.

ReentrantReadWriteLock은 읽기 / 쓰기를 위한 lock을 제공한다.
ReentrantLock은 배타적인 lock이라서 무조건 lock이 있어야만 임계영역의 코드를 수행할 수 있지만 ReentrantReadWriteLock은 읽기 lock이 걸려있으면 다른 쓰레드가 읽기 lock을 중복해서 걸고 읽기를 수행할 수 있다.
읽기는 내용을 변경하지 않으므로 동시에 여러 쓰레드가 읽어도 문제가 되지 않기 때문이다.
단 읽기 lock이 걸린 상태에서 쓰기 lock을 거는 것은 허용되지 않으며, 그 반대도 마찬가지이다.

StampedLock은 lock을 걸거나 해지할 때 스탬프(long 타입의 정수값)을 사용하며, 읽기와 쓰기를 위한 lock 외에 낙관적 읽기 lock(optimistic reading lock)가 추가된 클래스이다.
읽기 lock이 걸려있으면 쓰기 lock을 걸기 위해서는 읽기 lock이 풀릴 때까지 기다려야 하는데 비해 낙관적 읽기 lock(optimistic reading lock)은 쓰기 lock에 의해 바로 풀린다.
무조건 읽기 lock을 걸지 않고, 쓰기와 읽기가 충돌할 때만 쓰기가 끝난 후 읽기 lock을 거는 것이다.
```java
int getBalance(){
    long stamp = lock.tryOptimisticRead(); // 낙관적 읽기 lock을 건다.
    
    int curBalance = this.balance; // 공유 데이터인 balance를 읽어온다.
    
    if (!lock.validate(stamp)){// 쓰기 lock에 의해 낙관적 읽기 lock이 풀렸는지 확인
        stamp = lock.readLock(); // lock이 풀렸으면 읽기 lock을 얻으려고 기다린다.
        
        try{
            curBalance = this.balance; // 공유 데이터를 다시 읽어온다.
        }
        finally{
            lock.unlockRead(stamp); // 읽기 lcok을 푼다.
        }
    }
    return curBalance; // 낙관적 읽기 lock이 풀리지 않았다면 곧바로 읽어온 값을 반환
}
```
위 예제는 낙관적 읽기 lock을 사용하는 일반적인 예시이다.

### ReentrantLock의 생성자
ReentrantLock은 다음과 같이 두 개의 생성자를 가지고 있다.
```java
ReentrantLock()
ReentrantLock(boolean fair)
```
생성자의 매개변수로 true를 주면 lock 풀렸을 때 가장 오래 기다린 쓰레드가 lock을 획득할 수 있게(공정하게) 처리한다.
공정하게 처리할 경우 어떤 쓰레드가 가장 오래 기다렸는지 확인해야하므로 성능이 떨어진다.

대부분의 경우 굳이 공정하게 처리하지 않아도 되므로 공정하게 처리하기보다는 성능을 선택한다.
```java
void lock() : lock을 잠근다.
void unlock() : lock을 해제한다.
boolean isLocked() : lock이 잠겼는지 확인한다.
```
자동적으로 lock의 잠금과 해제가 관리되는 synchronized 블럭과 달리, ReentrantLcok과 같은 lock 클래스들은 수동으로 lock을 잠그고 해제해야 한다.
그러므로 대부분 다음과 같은 코드를 통해 사용한다.
```java
ReentranLock lock = new ReentrantLock();
try{
    // 임계 영역
}
finally{
    lock.unlock();
}
```
이렇게 하면 try 블럭 내에서 어떤 일이 발생해도 finally 블럭에 있는 unlock()이 수행되어 lock()이 풀리지 않는 일은 발생하지 않는다.
대부분의 경우 lock() & unlock() 대신 synchronized 블럭을 사용할 수 있으며, 그럴 때는 그냥 synchronized 블럭을 사용하는 것이 더 나을 수도 있따.

tryLock() 메소드의 경우 lock()과는 달리 다른 쓰레드에 의해 lock이 걸려 있으면 lock을 얻으려고 기다리지 않는다.
또는 지정된 시간만큼 기다린다.
lock을 얻으면 true를 반환하고, 얻지 못하면 false를 반환한다.
```java
boolean tryLock()
boolean tryLock(long timeout, TimeUnit unit) throws InterruptedException
```
lock()은 lock을 얻을 때까지 쓰레드를 블락(block) 시키므로 쓰레드의 응답성이 나빠질 수 있다.
응답성이 중요한 경우 tryLock()을 이용해서 지정된 시간동안 lock을 얻지 못하면 다시 작업을 시도할지 포기할지를 사용자가 결정할 수 있게 하는 편이 좋다.

또한 이 메소드는 InterruptedException을 발생시킬 수 있는데, 이것은 지정된 시간동안 lock을 얻으려고 기다리는 중에 interrupt()에 의해 작업이 취소될 수 있도록 코드를 작성할 수 있다는 의미이다.

### ReentrantLock과 Condition
wait()와 notify()는 해당 메소드가 동작할 쓰레드를 지정할 수 없다.
Condition은 이러한 문제점을 해결하기 위한 것이다.
wait()와 notify()는 쓰레드의 종류를 구분하지 않고 공유 객체의 waiting pool에 같이 몰아넣는 대신 특정 쓰레드를 위한 Condition을 만들어서 각각의 waiting pool에서 따로 기다리도록 하면 된다.

Condition은 이미 생성된 lock으로부터 newCondition()을 호출하여 생성한다.
```java
private ReentrantLock lock = new ReentrantLock(); // lock을 생성

// lock으로 condition을 생성
private Condition forCook = lock.newCondition();
private Condition forCust = lock.newCondition();
```
위와 같이 생성한 Condition을 통해 wait() & notify() 대신 await() & signal()을 사용하면 된다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185671885-f7b2590b-6f82-410b-9995-681285a5889d.png)
<br>

## 1.13 volatile
싱글 코어 프로세서가 장착된 컴퓨터에서는 정상적으로 작동하던 프로그램이 멀티 코어 프로세서를 장착한 컴퓨터에서는 오작동할 수 있다.

그 이유는 멀티 코어 프로세서에는 코어마다 별도의 캐시를 가지고 있기 때문이다.
<br>
![image](https://user-images.githubusercontent.com/62749021/185671991-62fb208e-bce1-4fb0-89cb-cc632c187a5a.png)
<br>
코어는 메모리에서 읽어온 값을 캐시에 저장하고 캐시에서 값을 읽어서 작업한다.
다시 같은 값을 읽어올 때는 먼저 캐시에 있는지 확인하고 없을 때만 메모리에서 읽어온다.

그러다보니 도중에 메모리에 저장된 변수의 값이 변경되었는데도 캐시에 저장된 값이 갱신되지 않아서 메모리에 저장된 값이 다른 경우가 발생한다.

그림에서는 변수 stopped의 값이 바뀌어서 종료가 되어야 하는데도 불구하고 캐시의 값을 읽어와 쓰레드가 멈추지 않고 계속 실행된다.
```java
boolean suspended = false;
boolean stopped = false;

volatile boolean suspended = false;
volatile boolean stopped = false;
```
위와 같이 변수 앞에 volatile을 붙이면 코어가 변수의 값을 읽어올 때 캐시가 아닌 메모리에서 읽어오기 때문에 캐시와 메모리간의 값의 불일치가 해결된다.

변수에 volatile을 붙이는 대신에 synchronized 블럭을 사용해도 같은 효과를 얻을 수 있다.
쓰레드가 synchronized 블럭으로 들어갈 때와 나올 때, 캐시와 메모리간의 동기화가 이루어지기 때문에 값의 불일치가 해소되기 때문이다.

### volatile로 long과 double을 원자화
JVM은 데이터를 4 byte(=32 bit)단위로 처리하기 때문에 int와 int보다 작은 타입들은 한 번에 읽거나 쓰는 것이 가능하다.
이 말은 단 하나의 명령어로 읽거나 쓰기가 가능하다는 의미이다.
하나의 명령어는 더 이상 나눌 수 없는 최소의 작업단위이므로, 작업의 중간에 다른 쓰레드가 끼어들 수 없다.

그러나 크기가 8 byte 이상인 long과 double 타입의 변수는 하나의 명령어로 값을 읽거나 쓸 수 없기 때문에 변수의 값을 읽는 과정에서 다른 쓰레드가 끼어들 여지가 있다.
다른 쓰레드가 끼어들지 못하게 하려고 변수를 읽고 쓰는 모든 문장을 synchronized 블럭으로 감쌀수도 있지만, 변수를 선언할 때 volatie를 붙이는 식으로 간단하게 처리할 수 있다.

상수의 경우 변하지 않는 값이므로 멀티쓰레드에 안전(thread-safe)이므로 volatile를 붙일 필요가 없으며, 붙일 수도 없다.

```java
volatile long sharedVal; // long 타입의 변수 (8 byte)를 원자화
volatile double sharedVal; // double 타입의 변수 (8 byte)를 원자화
```
volatile은 해당 변수에 대한 읽거나 쓰기가 원자화된다.
원자화라는 것은 작업을 더 이상 나눌 수 없게 한다는 의미인데, synchronized 블럭도 일종의 원자화라고 할 수 있다.
synchronized 블럭은 여러 문장을 원자화함으로써 쓰레드의 동기화를 구현한 것이라고 보면 된다.

volatile은 변수의 읽거나 쓰기를 원자화할 뿐, 동기화하는 것은 아니라는 점에 주의해야 한다.
즉 동기화가 필요할 때 synchronized 블럭 대신 volatile을 쓸 수 없다.
```java
volatile long balance; // 인스턴스 변수 balance를 원자화 한다.

synchronized int getBalance(){ // balance의 값을 반환한다.
    return balance;
}

synchronized void withdraw(int money){ // balance의 값을 변경
    if (balance >= money){
        balance -= money
    }
}
```
인스턴스변수 balance를 volatile로 원자화했으니까 이 값을 읽어서 반환하는 메소드 getBalance()를 동기화할 필요가 없다고 생각할 수 있다.

그러나 getBalance()를 synhronized로 동기화하지 않으면 withdraw()가 호출되어 객체에 lock을 걸고 작업을 수행하는 중인데도 getBalance()가 호출되는 것이 가능해진다.
출금이 진행 중일 때는 기다렸다가 출금이 끝난 후에 잔고를 조회할 수 있게 하려면 getBalance()에 synchronized를 붙여서 동기화를 해야한다.

## 1.14 fork & join 프레임웤
JDK 1.7부터 fork & join 프레임웤이 추가되었고, 이 프레임웤은 하나의 작업을 작은 단위로 나눠서 여러 쓰레드가 동시에 처리하는 것을 쉽게 만들어준다.

먼저 수행할 작업에 따라 RecursiveAction과 RecursiveTask 두 클래스 중에서 하나를 상속받아 구현해야한다.
```
RecursiveAction : 반환값이 없는 작업을 구현할 때 사용
RecursiveTask : 반환값이 있는 작업을 구현할 때 사용
```
두 클래스 모두 compute()라는 추상 메소드를 가지고 있으며, 상속을 통해 이 추상 메소드를 구현하기만 하면 된다.

이후 쓰레드 풀과 수행할 작업을 생성하고, invoke()로 작업을 시작한다.
```java
ForkJoinPool pool = new ForkJoinPool(); // 쓰레드 풀을 생성 
SumTask task = new SumTask(from, to); // 수행할 작업을 생성

Long result = pool.invoke(task); // invoke()를 호출해서 작업을 시작
```
ForkJoinPool은 fork & join 프레임웤에서 제공하는 쓰레드 풀(thread pool)로, 지정된 수의 쓰레드를 생성해서 미리 만들어 놓고 반복해서 재사용할 수 있게 한다.
ForkJoinPool을 사용하면 쓰레드를 반복해서 생성하지 않아도 된다는 장점과 너무 많은 쓰레드가 생성되어 성능이 저하되는 것을 막아준다는 장점이 있다.

쓰레드 풀은 쓰레드가 수행해야하는 작업이 담긴 큐를 제공하며, 각 쓰레드는 자신의 작업 큐에 담긴 작업을 순서대로 처리한다.

### compute()의 구현
compute()를 구현할 때는 수행할 작업 외에도 작업을 어떻게 나눌 것인가에 대해서도 명시해야한다.
```java
public Long compute(){
    long size = to - from + 1; // from <= i <= to
    
    if (size <= 5) // 더할 숫자가 5개 이하면
        return sum(); // 숫자의 합을 반환. sum()은 from부터 to까지의 수를 더해서 반환
    
    // 범위를 반으로 나눠서 두 개의 작업을 생성
    long half = (from + to) / 2;
    
    SumTask leftSum = new SumTask(from, half);
    SumTask rightSUm = new SumTask(half + 1, to);
    
    leftSUm.fork(); // 작업 (leftzSum)을 작업 큐에 넣는다.
    
    return rightSum.compute() + leftSum.join();
}
```
실제 수행할 작업은 sum() 뿐이고 나머지는 수행할 작업의 범위를 반으로 나눠서 새로운 작업을 생성해서 실행시키기 위한 것이다.
위 예제는 지정된 범위를 절반으로 나누어 나눠진 범위의 합을 계산하기 위한 새로운 SumTask를 생성하며, 이 과정은 작업이 더 이상 나눠질 수 없을 때(size가 5보다 작거나 같을 때)까지 반복된다.

compute()의 구조는 일반적인 재귀호출 메소드와 동일하다.

### 다른 쓰레드의 작업 훔쳐오기
fork()가 호출되어 작업 큐에 추가된 작업 역시 compute()에 의해 더 이상 나눌 수 없을 때 까지 반복해서 나뉘고, 자신의 작업 큐가 비어있는 쓰레드는 다른 쓰레드의 작업 큐에서 작업을 가져와서 수행된다.
이것을 작업 훔쳐오기(work stealing)라고 하며, 이 과정은 모두 쓰레드 풀에 의해 자동적으로 이루어진다.

### fork()와 join()
fork()와 join()의 특징은 다음과 같다.
```
fork() : 해당 작업을 쓰레드 풀의 작업 큐에 넣는다. 비동기 메소드
join() : 해당 작업의 수행이 끝날 때까지 기다렸다가, 수행이 끝나면 그 결과는 반환한다. 동기 메소드
```
비동기 메소드는 일반적인 메소드와 달리 메소드만 호출할 뿐, 그 결과를 기다리지 않는다.
내부적으로는 다른 쓰레드에게 작업을 수행하도록 지시만 하고 결과를 기다리지 않고 돌아오는 것이다.

fork & join 프레임웤으로 계산한 것보다 for문으로 계산한 결과가 더 빠를수도 있는데, 그 이유는 작업을 나누고 다시 합치는데 걸리는 시간이 있기 때문이다.
재귀호출보다 for문이 더 빠른 것과 같은 이유이며, 항상 멀티쓰레드로 처리하는 것이 빠르다고 생각해서는 안 된다.
