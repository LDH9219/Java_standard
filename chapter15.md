# 입출력 I/O
## 1.1 입출력이란?
I/O란 Input과 Output의 약자로 입력과 출력, 입출력을 의미한다.
입출력은 컴퓨터 내부 또는 외부의 장치와 프로그램간의 데이터를 주고받는 것을 의미한다.

## 1.2 스트림(Stream)
자바에서 어느 한 쪽에서 다른 쪽으로 데이터를 전달하려면 두 대상을 연결하고 데이터를 전송할 수 있는 무언가가 필요한데 이를 스트림(stream)이라고 정의했다.

14장의 스트림과는 다른 개념이다.
```
스트림(stream)이란 데이터를 운반하는데 사용되는 연결통로이다.
```
스트림은 단방향통신만 가능하기 때문에 하나의 스트림으로 입력과 출력을 동시에 처리할 수 없다.
그러므로 입력과 출력을 동시에 수행하려면 입력을 위한 입력스트림(input stream)과 출력을 위한 출력스트림(output stream), 모두 2개의 스트림이 필요하다.
스트림은 먼저 보낸 데이터를 먼저 받게 되어 있으며 중간에 건너뜀 없이 연속적으로 데이터를 주고받는다.
큐(queue)와 같은 FIFO(First In First Out)구조로 되어 있다고 볼 수 있다.

## 1.3 바이트 기반 스트림 - InputStream, OutputStream
스트림은 바이트단위로 데이터를 전송하며 입출력 대상에 따라 다음과 같은 입력스트림이 있다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185811724-42317f2a-528f-4d56-87e3-ed89834e6ea7.png)

<br>
위 메소드들은 모두 InputStream 또는 OutputSTream의 자손들이며, 각각 읽고 쓰는데 필요한 추상메소드를 자신에 맞게 구현해 놓았다.

자바에서는 java.io 패키지를 통해서 많은 종류의 입출력관련 클래스들을 제공하고 있으며
입출력을 처리할 수 있는 표준화된 방법을 제공함으로써 입출력의 대상이 달라져도 동일한 방법으로 입출력이 가능하기 때문에 프로그래밍을 하기에 편리하다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185811741-24034804-604c-447d-a390-54f01625ec9c.png)

<br>

위 메소드의 사용법을 알고 있다면 대상의 종류에 관계없이 위 메소드를 호출해서 입출력을 처리할 수 있다.

또한 추상 메소드가 아닌 메소드들은 추상 메소드를 이용해서 작성되었으므로, 추상 메소드를 무조건 구현해야 한다.

## 1.4 보조 스트림
스트림의 기능을 보완하기 위한 보조스트림이 존재한다.
보조스트림은 실제 데이터를 주고받는 스트림이 아니기 때문에 데이터를 입출력할 수 있는 기능이 없지만 스트림의 기능을 향상시키거나 새로운 기능을 추가할 수 있다.

그러므로 일반 스트림을 먼저 생성한 다음에 이를 이용해서 보조스트림을 생성해야한다.

예를 들어 test.txt 파일을 읽기 위해 FileInputStream을 사용할 때, 입력 효율을 높이기 위해 보조스트림 BufferedInputStream을 사용하는 코드는 다음과 같다.

```java
// 먼저 기반스트림을 생성한다.
FileInputStream fis = new FileInputStream("test.txt");

// 기반스트림을 이용해서 보조스트림을 생성한다.
BufferedInputStream bis = new BufferedInputStream(fis);

bis.read(); // 보조스트림인 BufferedInputStream으로부터 데이터를 읽는다.
```

코드 상으로는 보조스트림은 BufferedInputStream이 입력기능을 수행하는 것처럼 보인다.
그러나 실제로는 BufferedInputStream과 연결된 FileInputStream이 수행하고, 보조스트림인 BufferedIputStream을 버퍼만을 제공한다.

버퍼를 사용한 입출력과 사용하지 않는 입출력간의 성능차이는 상당하기 때문에 대부분 버퍼를 이용한 보조스트림을 사용한다.

모든 보조스트림은 InputStream과 OutputStream의 자손들이므로 입출력 방식이 동일하다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185811787-7b5f88de-803f-473d-bc44-6b24c3f07efb.png)

<br>

## 1.5 문자기반 스트림 - Reader, Writer
이전에 확인했던 스트림은 모두 바이트기반 스트림이었다.
바이트기반이므로 입출력의 단위가 1byte가 된다.
여기서 자바는 한 문자를 의미하는 char형이 1byte가 아닌 2byte이기 때문에 바이트기반 스트림으로는 2byte인 문자를 처리하는데 어려움이 있다.
이를 보완하기 위해 문자 기반의 스트림이 제공된다.
```
InputStream -> Reader
OutputStream -> Writer
```

<br>

![image](https://user-images.githubusercontent.com/62749021/185811913-f0e61aa6-acac-48fa-ab95-e99226282aaf.png)

<br>

문자기반 스트림의 이름은 바이트기반 스트림의 이름에서 InputStream은 Reader로 WriteStream을 Writer로 변경하면 된다.
단 ByteArrayInputStream에 대응하는 문자기반 스트림은 char 배열을 상ㅇ하는 CharArrayReader이다.

Reader와 Writer에서도 추상메소드가 아닌 메소드들은 추상메소드를 이용해서 작성되었다.

바이트기반 스트림과 문자기반 스트림은 이름만 조금 다를 뿐 활용방법은 거의 같다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185811921-73466e37-c3bb-4063-8e4d-4fea31f01913.png)

<br>

보조스트림 역시 다음과 같은 문자기반 보조스트림이 존재하며 사용목적과 방식은 바이트기반 보조스트림과 동일하다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185811928-2506e2e7-9d39-4426-909e-4aa8422e1933.png)

<br>

## 1.6 InputStream과 OutputStream
InputStream과 OutputStream은 모든 바이트기반의 스트림의 조상이며 다음과 같은 메소드가 선언되어 있다.

다음은 InputStream 메소드 목록이다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185811939-a029173f-9885-400e-aee7-c62279e46923.png)

<br>

다음은 OutputStream 메소드 목록이다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185811949-b5a31485-77ee-45f5-99f9-14b8b3f394a9.png)

<br>

스트림의 종류에 따라서 mark()와 reset()을 사용하여 이미 이미 읽은 데이터를 되돌려서 다시 읽을 수 있다.
이 기능을 지원하는지 확인하기 위해 markSupported()를 사용한다.

flush()는 버퍼가 있는 출력스트림의 경우에만 의미가 있으며, OutputStream에 정의된 flush()는 아무런 일도 하지 않는다.

프로그램이 종료될 때, 사용하고 닫지 않은 스트림을 JVM이 자동적으로 닫아 주지만, 스트림을 사용해서 모든 작업을 마치고 난 후에는 close()를 호출해서 반드시 닫아주어야 한다.
그러나 ByteArrayInputStream과 같이 메모리를 사용하는 스트림과 System.in, System.out과 같은 표준 입출력 스트림은 닫아 주지 않아도 된다.

## 1.7 ByteArrayInputStream과 ByteArrayOutputStream
ByteArrayInputStream/ByteArrayOutputStream은 메모리, 즉 바이트배열에 데이터를 입출력하는데 사용되는 스트림이다.
주로 다른 곳에 입출력하기 전에 데이터를 임시로 바이트배열에 담아서 변환 등의 작업을 하는데 사용된다.


## 1.8 FileInputStream과 FileOutputStream
FileInputStream/FileOutputStream은 파일에 입출력을 하기 위한 스트림이다.
실제 프로그래밍에서 많이 사용되는 스트림 중의 하나이다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185811993-9a891fd8-b1da-4bcd-b124-b0d4ae5bdc2c.png)

<br>

텍스트파일을 다루는 경우에는 FileInputStream/FileOutputStream 대신 문자기반의 스트림인 FileReader/FIleWriter를 사용하는 것이 더 좋다.

## 1.9 FilterInputStream과 FilterOutputStream
FilterInputStream/FilterOutputStream은 InputStream/OutputStream의 자손이면서 모든 보조스트림의 조상이다.
보조스트림은 자체적으로 입출력을 수행할 수 없기 때문에 기반스트림을 필요로 한다.
다음은 FilterInputStream/FilterOutputStream의 생성자다.
```java
protected FilterInputStream(InputStream in)
public FilterOutputStream(OutputStream out)
```
FilterInputStream/FilterOutputStream의 모든 메소드는 단순히 기반스트림의 메소드를 그대로 호출한다.
즉 FilterInputStream/FilterOutputStream 자체로는 아무런 일도 하지 않음을 의미한다.
FilterInputStream/FilterOutputStream은 상속을 통해 원하는 작업을 수행하도록 읽고 쓰는 메소드를 오버라이딩 해야 한다.

```java
public class FilterInputStream extends InputStream {
    protected volatile InputStream in;
    
    protected FilterInputStream(InputStream in) {
        this.in = in;
    }
    
    public int read() throws IOException {
        return in.read();
    }
    ...
}
```
생성자 FilterInputStream(InputStream in)는 접근 제어자가 protected이기 때문에 Filter InputStream의 인스턴스를 생성해서 사용할 수 없고 상속을 통해서 오버라이딩되어야 한다.
FilterInputStream/FilterOutputStream을 상속받아서 기반스트림에 보조기능을 추가한 보조스트림 클래스는 다음과 같다.

```
FilterInputStream의 자손 : BufferedInputStream, DataInputStream, PushbackInputStream 등
FilterOutputStream의 자손 : BufferedOutputStream, DataOutputStream, PrintStream 등
```

## 1.10 BufferedInputStream과 BufferedOutputStream
BufferedInputStream/BufferedOutputStream은 스트림의 입출력 효율은 높이기 위해 버퍼를 사용하는 보조스트림이다.
한 바이트씩 입출력하는 것 보다는 버퍼(바이트배열)를 이용해서 한 번에 여러 바이트를 입출력하는 것이 빠르기 때문에 대부분의 입출력 작업에 사용된다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812039-7541780b-f28e-4bf3-9290-97a9a4d01905.png)

<br>

BufferedInputStream의 버퍼크기는 입력소스로부터 한 번에 가져올 수 있는 데이터의 크기로 저장하면 좋다.
보통 입력소스가 파일인 경우 4096 정도의 크기로 하는 것이 보통이며, 버퍼의 크기를 변경해가면서 테스트하면 최적의 버퍼크기를 알아낼 수 있다.

프로그램에서 입력소스로부터 데이터를 읽기 위해 처음으로 read메소드를 호출하면, BufferedInputStream은 입력소스로부터 버퍼 크기만큼의 데이터를 읽어다 자신의 내부 버퍼에 저장한다.
이제 프로그램에서는 BufferedInputStream의 버퍼에 저장된 데이터를 읽으면 되는 것이다.
외부의 입력소스로부터 읽는 것보다 내부의 버퍼로부터 읽는 것이 훨씬 빠르기 때문에 그만큼 작업 효율이 높아진다.

프로그램에서 버퍼에 저장된 모든 데이터를 다 읽고 그 다음 데이터를 읽기 위해 read메소드가 호출되면 BufferedInputStream은 입력소스로부터 다시 버퍼크기 만큼의 데이터를 읽어다 버퍼에 저장해 놓는다.

이와 같은 작업이 계속해서 반복된다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812053-a6dc595f-d54e-482e-afa3-cc961d03565f.png)

<br>

BufferedOutputStream 역시 버퍼를 이용해서 출력소스와 작업을 하게 되는데, 입력 소스로부터 데이터를 읽을 때와는 반대로, 프로그램에서 write 메소드를 이용한 출력이 BufferedOutputStream의 버퍼에 저장된다.
버퍼가 가득 차면, 그 때 버퍼의 모든 내용을 출력소스에 출력한다.
이후 버퍼를 비우고 다시 프로그램으로부터의 출력을 저장할 준비를 한다.

버퍼가 가득 찼을 때만 출력소스에 출력을 하기 때문에, 마지막 출력부분이 출력소스에 쓰이지 못하고 BufferedOutputStream에 남아있는 채로 프로그램이 종료될 수 있다는 점을 주의해야한다.
그래서 모든 프로그램에서 모든 출력작업을 마친 후 BufferedOutputStream에 close()나 flush()를 호출해서 마지막에 버퍼에 있는 모든 내용이 출력소스에 출력되도록 해야한다.

## 1.11 DataInputStream과 DataOutputStream
DataInputStream/DataOutputStream도 각각 FilterInputStream/FilterOutputStream의 자손이며 DataInputStream은 DataInput 인터페이스를, DataOutputStream은 DataOutput 인터페이스를 구현했기 때문에 데이터를 읽고 쓰는데 byte 단위가 아닌, 8가지 기본 자료형의 단위로 읽고 쓸 수 있다는 장점이 있다.

각 자료형의 크기가 다르므로, 출력한 데이터를 다시 읽어 올 때에는 출력했을 때의 순서를 염두에 두어야 한다.

DataInputStream의 메소드/생성자는 다음과 같다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812079-4e10420c-d6de-4ea4-9aeb-b7f798d853a9.png)

<br>

DataOutputStream이 출력하는 형식은 각 기본 자료형 값을 16진수로 표현하여 저장한다.
예를 들어 int값을 출력한다면 4 byte의 16진수로 출력된다.
DataOutputStream의 메소드/생성자는 다음과 같다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812094-8879bf6c-a559-46bb-b4c1-ba891c7779cf.png)

<br>

## 1.12 SequenceInputStream
SequenceInputStream은 여러 개의 입력스트림을 연속적으로 연결해서 하나의 스트림으로부터 데이터를 읽는 것과 같이 처리할 수 있도록 도와준다.
SequenceInputStream의 생성자를 제외하고 나머지 작업은 다른 입력스트림과 다르지 않다.
큰 파일을 여러 개의 작은 파일로 나누었다가 하나의 파일로 합치는 것과 같은 작업을 수행할 때 좋다.

SequenceInputStream은 다른 보조스트림들과는 달리 FilterInputStream의 자손이 아닌 InputStream을 바로 상속받아 구현했다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812127-36ed05c2-9d7b-4301-870f-5c2b6bf3341a.png)

<br>

## 1.13 PrintStream
PrintStream은 데이터를 기반스트림에 다양한 형태로 출력할 수 있는 print, println, printf와 같은 메소드를 오버로딩하여 제공한다.

PrintStream은 데이터를 적절한 문자로 출력하는 것이기 때문에 문자기반 스트림의 역할을 수행한다.

JDK 1.1부터 PrintStream보다 향상된 기능의 문자기반 스트림인 PrintWriter가 추가되었으나 System.out이 PrintStream이보다 둘 다 사용하게 되었다.

PrintStream와 PrintWriter는 거의 같은 기능을 가지고 있지만 PrintWriter가 PrintStream에 비해 다양한 언어의 문자를 처리하는데 적합하기 때문에 가능하면 PrintWriter를 사용하는 것이 좋다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812133-2e3d8b88-4bad-4fd9-98ab-71e388d46057.png)

<br>

print()나 println()을 이용해서 출력하는 중에 PrintStream의 기반스트림에서 IOException이 발생하면 checkError()를 통해 인지할 수 있다.
println()이나 print()는 예외를 던지지 않고 내부에서 처리하도록 정의되었는데, 그 이유는 println()과 같은 메소드가 자주 사용되기 때문이다.

## 1.14 문자기반 스트림
문자데이터를 다루는데 사용된다는 것을 제외하고는 바이트기반 스트림과 문자기반 스트림의 사용방법은 거의 동일하다.

## 1.15 Reader와 Writer
바이트기반 스트림의 조상이 InputStream/OutputStream인 것과 같이 문자기반의 스트림에서는 Reader/Writer가 그와 같은 역할을 한다.
그러므로 Reader/Writer는 byte배열 대신 char배열을 사용한다는 것 외에는 InputStream/OutputStream의 메소드와 다르지 않다.

다음은 Reader의 메소드 목록이다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812146-5202f6e6-2a8a-4b56-9c9e-ba4e4334d500.png)

<br>

다음은 Writer의 메소드 목록이다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812162-c0bc77ba-e748-4563-a7fb-359c1f976a56.png)

<br>

문자기반 스트림이라는 것이 단순히 2 byte로 스트림을 처리하는 것만이 아니라, 인코딩(encoding)기능 또한 포함되어 있다.

문자기반 스트림인 Reader/Writer, 그리고 그 자손들은 여러 종류의 인코딩과 자바에서 사용하는 유니코드(UTF-16)간의 변환을 자동적으로 처리해준다.
Reader는 특정 인코딩을 읽어서 유니코드로 변환하고 Writer는 유니코드를 특정 인코딩으로 변환하여 저장한다.

## 1.16 FileReader와 FileWriter
FileReader/FileWriter는 파일로부터 텍스트데이터를 읽고 파일에 쓰는데 사용된다.
사용방법은 FileInputStream/FileOutputStream과 다르지 않다.

## 1.17 PipedReader와 PipeWriter
PipedReader/PipeWriter는 쓰레드 간에 데이터를 주고받을 때 사용된다.
다른 스트림과는 달리 입력과 출력스트림을 하나로 연결(connect)해서 데이터를 주고받는다는 특징이 있다.
스트림을 생성한 다음에 어느 한쪽 쓰레드에서 connect()를 호출해서 입력스트림과 출력스트림을 연결한다.
입출력을 마친 후에는 어느 한쪽 스트림만 닫아도 나머지 스트림은 자동으로 닫힌다.
이 부분을 제외하고는 일반 입출력방식과 동일하다.

## 1.18 StringReader와 StringWriter
StringReader/StringWriter는 CharArrayReader/CharArrayWriter와 같이 입출력 대상이 메모리인 스트림이다.

StringWriter에 출력되는 데이터는 내부의 StringBuffer에 저장되며 StringWriter의 다음과 같은 메소드를 이용해서 저장된 데이터를 얻을 수 있다.

## 1.19 BufferedReader와 BufferedWriter
BufferedReader/BufferedWriter는 버퍼를 이용해서 입출력의 효율을 높일 수 있도록 해주는 역할을 한다.
버퍼를 이용하면 입출력의 효율이 비교할 수 없을 정도로 좋아지기 때문에 사용하는 것이 좋다.
BufferedReader의 readLine()을 사용하면 데이터를 라인 단위로 읽을 수 있고 BufferedWriter는 newLine()이라는 줄바꿈 해주는 메소드를 가지고 있다.

## 1.20 InputStreamReader와 OutputStreamWriter
InputStreamReader/OutputStreamWriter는 바이트기반 스트림을 문자기반 스트림으로 연결해주는 역할을 한다
또한 바이트기반 스트림의 데이터를 지정된 인코딩의 문자데이터로 변환하는 작업을 수행한다.

InputStreamReader의 생성자와 메소드 목록은 다음과 같다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812195-82cd098c-27e4-4c38-86cd-e3dc7bbd3e86.png)

<br>

OutputStreamReader의 생성자와 메소드 목록은 다음과 같다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812203-d624fd38-070d-49ba-9826-933347576eb0.png)

<br>

다른 언어로 작성된 파일을 한글 윈도우에서 읽어올 때 InputStreamReader(InputStream in, String encoding)를 이용해서 인코딩이 중국어로 되어있다는 것을 지정해주어야 파일의 내용이 깨지지 않고 정상적으로 보일 것이다.

이와 마찬가지로 OutputStreamWRiter를 이용해서 파일에 텍스트데이터를 저장할 때 생성자 OutputStreamWriter(OutputStream out, String encoding)를 이용해서 인코딩을 지정하지 않으면 OS에서 사용하는 인코딩으로 데이터를 저장할 것이다.

## 1.21 표준입출력 - System.in, System.out, System.err
표준입출력은 콘솔을 통한 데이터 입력과 콘솔로의 데이터 출력을 의미한다.
자바에서는 표준 입출력(standard I/O)을 위해 3가지 입출력 스트림인 System.in, System.out, System.err을 제공하는데, 이들은 자바 어플리케이션의 실행과 동시에 사용할 수 있께 자동적으로 생성되기 때문에 개발자가 별도로 스트림을 생성하는 코드를 작성하지 않고도 사용이 가능하다.

```
System.in : 콘솔로부터 데이터를 입력받는데 사용
System.out : 콘솔로 데이터를 출력하는데 사용
System.err : 콘솔로 데이터를 출력하는데 사용
```
System클래스의 소스에서 알 수 있듯이 in, out, err는 System클래스에 선언된 클래스 변수(static 변수)이다.
선언부분은 PrintStream이지만 실제로는 버퍼를 이용하는 BufferedInputStream과 BufferedOutputStream의 인스턴스를 사용한다.

## 1.22 표준입출력의 대상변경 - setOut(), setErr(), setIn()
초기에는 System.in, System.out, System.err의 입출력대상이 콘솔이지만 setIn(), setOut(), setErr()를 사용하면 입출력을 콘솔 이외에 다른 입출력 대상으로 변경하는 것이 가능하다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812242-3272271d-0666-446e-998b-9b2a16009633.png)

<br>

그러나 JDK 1.5부터 Scanner 클래스가 제공되면서 System.in으로부터 데이터를 입력받아 작업하는 것이 편해졌다.

## 1.23 RandomAccessFile
자바에서는 입력과 출력이 각각 분리되어 별도로 작업을 하도록 설계되어 있는데, RandomAccessFile만은 하나의 클래스로 파일에 대한 입력과 출력을 모두 할 수 있도록 되어있다.

위 그림과 같이 InputStream이나 OutputStream으로부터 상속받지 않고 DataInput 인터페이스와 DataOutput 인터페이스를 모두 구현했기 때문에 읽기와 쓰기가 모두 가능하다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812263-7b80b9fb-be11-40e3-bf43-d14fe21c6de5.png)

<br>

또한 RandomAccessFile 클래스는 DataInputStream과 DataOutputStream처럼 기본자료형 단위로 데이터를 읽고 쓸 수 있다.

다른 입출력 클래스들은 입출력소스에 순차적으로 읽기/쓰기를 하기 때문에 읽기와 쓰기가 제한적인데 반해서 RandomAccessFile 클래스는 파일에 읽고 쓰는 위치에 제한이 없다.

이것을 가능하기 위해 내부적으로 파일 포인터를 사용하며, 이것은 입출력 시에 작업이 수행되는 위치를 의미한다.
파일 포인터의 위치는 파이르이 제일 첫 부분(0부터 시작)이며, 읽기 또는 쓰기를 수행할 때 마다 작업이 수행된 다음 위치로 이동하게 된다.
순차적으로 읽기나 쓰기를 한다면 파일 포인터를 이동시키기 위해 별도의 작업이 필요하지 않지만 파일의 임의의 위치에 있는 내용에 대해 작엏바고자 한다면 파일 포인터를 원하는 위치로 옮긴 다음 작업을 해야 한다.

현재 작업 중인 파일에서 파일 포인터의 위치를 알고 싶을 때는 getFilePointer()를 사용하면 되고, 파일 포인터의 위치를 옮기기 위해서는 seek(long pos)나 skipBytes(int n)를 사용하면 된다.

RandomAccessFile의 생성자와 메소드 목록은 다음과 같다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812272-3e62a9b9-a62b-4e0f-9bc0-403b91afe32e.png)

<br>

## 1.24 File
파일은 기본적이면서도 가장 많이 사용되는 입출력 대상이다.
자바에서는 File 클래스를 통해서 파일과 디렉토리를 다룰 수 있도록 하고 있다.

다음은 File의 생성자와 메소드 목록이다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812291-942c020a-697d-4691-9d9b-b51633d5c552.png)

<br>

경로와 관련된 File의 멤버변수는 다음과 같다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812305-dcadbfc5-555a-48c7-8ebc-559168474a3b.png)

<br>

파일의 경로(path)와 디렉토리나 파일의 이름을 구분하는데 사용되는 구분자가 OS마다 다를 수 있기 때문에 OS 독립적으로 프로그램을 작성하기 위해서는 바늗시 위의 멤버변수들을 이용해야한다.
만일 윈도우에서 사용하는 구분자를 코드에 직접 적어 놓았다면, 이 코드는 다른 OS에서 오류를 일으킬 수 있다.

절대 경로(absolute path)는 파일시스템의 루트(root)로부터 시작하는 파일의 전체 경로를 의미한다.
OS에 따라 다르지만, 하나의 파일에 대해 둘 이상의 절대경로가 존재할 수 있다.
현재 디렉토리를 의미하는 ```.```과 같은 기호나 링크를 포함하고 있는 경우가 이에 해당한다.
그러나 정규 경로(canonical path)는 기호나 링크 등을 포함하지 않는 유일한 경로를 의미한다.
시스템속성 중에서 user.dir의 값을 확인하면 현재 프로그램이 실행 중인 디렉토리를 알 수 있다.
OS의 시스템변수로 설정하는 classpath 외에 sun.boot.class.path라는 시스템속성에 기본적인 classpath가 있어서 기본적인 경로들은 이미 설정되어 있다.

주의사항으로는 File 인스턴스를 생성했다고 해서 파일이나 디렉토리가 생성되는 것은 아니므로 새로운 파일을 생성할 때에는 주의해야 한다.

```
1. 이미 존재하는 파일을 참조할 때 :
File f = new File("c:\\jdk1.8\\work\\ch15", "FileEx1.java");

2. 기존에 없는 파일을 새로 생성할 때 :
File f = new File("c:\\jdk1.8\\work\\ch15", "NewFile.java");
f.craeteNewFile();
```

## 1.25 직렬화(Serialization)란?
직렬화(Serialization)란 객체를 데이터 스트림으로 만드는 것을 의미한다.
즉 객체에 저장된 데이터를 스트림에 쓰기(write)위해 연속적인(seiral) 데이터로 변환하는 것을 의미한다.
반대로 스트림으로부터 데이터를 읽어서 객체를 만드는 것을 역직렬화(deserialization)라고 한다.

객체를 저장하거나 전송하기 위해서는 직렬화가 필수적이다.

객체는 클래스에 정의된 인스턴스변수의 집합이다.
객체에는 클래스변수나 메소드가 포함되지 않는다.
객체는 오직 인스턴스 변수들로만 구성되어 잇다.

즉 이전에는 이해를 돕기 위해 객체에 메소드까지 포함해서 그림으로 표현했지만, 사실은 인스턴스변수만 있다는 것이다.
인스턴스변수는 인스턴스마다 다른 값을 가질 수 있어야 하기 때문에 별도의 메모리공간이 필요하지만 메소드는 변하지 않으므로 메모리를 낭비해 가면서 인스턴스마다 같은 내용의 코드(메소드)를 포함시킬 이유가 없기 때문이다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812340-0ec83b1a-61e3-4a01-9205-1f9c960ab632.png)

<br>

즉 실제로 오른쪽처럼 인스턴스에 메소드가 포함되지 않는 것이 더 정확한 그림이다.

그러므로 객체를 저장한다는 것은 바로 객체의 모든 인스턴스변수의 값을 저장한다는 것과 같은 의미이다.
어떤 객체를 저장하고자 한다면 현재 객체의 모든 인스턴스변수의 값을 저장하기만 하면 된다.
그리고 저장했던 객체를 다시 생성하려면, 객체를 생성한 후에 저장했던 값을 읽어서 생성한 객체의 인스턴스변수에 저장하면 된다.

클래스에 정의된 인스턴스변수가 단순히 기본형일 때는 인스턴스변수의 값을 저장하는 일이 간단하지만, 인스턴스변수의 타입이 참조형일 때에는 간단하지 않다.

이러한 문제는 객체를 직렬화/역직렬화 할 수 있는 ObjectInputStream과 ObjectOutputStream의 사용법을 알기만 하면 된다.

여기서 두 객체가 동일한지 판단하는 기준은 두 객체의 인스턴스변수의 값들이 같고 다름이다.

## 1.26 ObjectInputStream, ObjectOutputStream
직렬화(스트림에 객체를 출력)에는 ObjectOutputStream을 사용하고 역직렬화(스트림으로부터 객체를 입력)에는 ObjectInputStream을 사용한다.
ObjectInputStream/ObjectOutputStream은 각각 InputStream/OutputStream을 직접 상속받지만 기반스트림을 필요로 하는 보조스트림이다.
그러므로 객체를 생성할 때 입출력(직렬화/역직렬화)할 스트림을 지정해주어야 한다.
```java
ObjectInputStream(InputStream in)
ObjectOutputStream(OutputStream in)
```
만일 파일에 객체를 저장(직렬화)하고 싶다면 다음과 같이 하면 된다.
```java
FileOutputStream fos = new FileOutputStream("objectfile.ser");
ObjectOutputStreaem out = new ObjectOutputStream(fos);

out.writeObject(new UserInfo());
```
위 코드는 objectfile.ser이라는 파일에 UserInfo 객체를 직렬황해서 저장한다.
출력할 스트림(FileOutputStream)을 생성해서 이를 기반스트림으로 하는 ObjectOutputStream을 생성한다.

ObjectOutputStream의 writeObject(Object obj)를 사용해서 객체를 출력하면 객체가 파일에 직렬화되어 저장된다.

역직렬화는 직렬화와는 달리 입력스트림을 사용하고 writeObject(Object obj)대신 readObject()를 사용하여 저장된 데이터를 읽기로 하면 객체로 역직렬화된다.
다만 readObject()의 반환타입이 Object이기 때문에 객체 원래의 타입으로 형변한 해주어야 한다.
```java
FileInputStream fis = new FileInputStream("objectfile.ser");
ObjectInputStream in = new ObjectInputStream(fis);

UserInfo info = (UserInfo)in.readObject();
```
ObjectInputStream과 ObjectOutputStream에는 readObject()와 writeObject() 이외에도 여러 가지 타입의 값을 입출력할 수 있는 메소드를 제공한다.

<br>

![image](https://user-images.githubusercontent.com/62749021/185812386-0cecc510-463e-4894-be82-a206b90d2990.png)

<br>

이 메소드들은 직렬화와 역직렬화를 직접 구현할 때 주로 사용되며, defaultReadObject()와 defaultWriteObject()는 자동 직렬화를 수행한다.
객체를 직렬화/역직렬화하는 작업은 객체의 모든 인스턴스변수가 참조하고 있는 모든 객체에 대한 것이기 때문에 상당히 복잡하며 시간도 오래 걸린다.
readObject()와 writeObject()를 사용한 자동 직렬화가 편리하기는 하지만 직렬화작업시간을 단축시키려면 직렬화하고자 하는 객체의 클래스에 추가적으로 다음과 같은 2개의 메소드를 직접 구현해주어야 한다.

```java
private void writeObject(ObjectOutputStream out) throws IOException {
    // write 메소드를 사용해서 직렬화를 수행한다.
}

private void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException {
    // read 메소드를 사용해서 역직렬화를 수행한다.
}
```

## 1.27 직렬화가 가능한 클래스 만들기 - Serializable, transient
직렬화가 가능한 클래스를 만드려면 java.io.Serializable 인터페이스를 구현하도록 하면 된다.

Serializable 인터페이스는 마커 인터페이스로, 아무런 내용이 없다.

만약 Serializable을 구현한 클래스를 상속받는다면 Serialiable을 구현하지 않아도 된다.
```java
public class SuperUserInfo implements Serializable {
    String name;
    String password;
}

public class UserInfo extends SuperUserInfo {
    int age;
}
```
위 코드의 경우 UserInfo 객체를 직렬화하면 조상인 SuperUserInfo에 정의된 인스턴스 변수 name, password도 같이 직렬화된다.
```java
public class SuperUserInfo{
    String name;
    String password;
}

public class UserInfo extends SuperUserInfo implements Serializable {
    int age;
}
```
위 코드의 경우 조상클래스가 Serializable을 구현하지 않았으므로 자손클래스를 직렬화할 때 조상클래스에 정의된 인스턴스변수 name과 password는 직렬화 대상에서 제외된다.

조상클래스에 정의된 인스턴스변수 name과 password를 직렬화 대상에 포함시키기 위해서는 조상클래스가 Serializable을 구현하거나, UserInfo에서 조상의 인스턴스변수들이 직렬화되도록 처리하는 코드를 추가해야한다.

```java
public class UserInfo implements Serializable {
   String name;
   String password;
   int age;
   
   Object obj = new Object(); // Object 객체는 직렬화할 수 없다.
}
```
위 코드의 UserInfo는 Serializable을 구현하고 있지만 이 클래스의 객체를 직렬화하면 java.io.NotSerializableException이 발생하면서 직렬화에 실패한다.
그 이유는 직렬화할 수 엇없는 클래스의 객체를 인스턴스변수가 참조하고 있기 때문이다.
모든 클래스의 조상인 Object는 Serializable을 구현하지 않았기 때문에 직렬화할 수 없다.
```java
public class UserInfo implements Serializable {
   String name;
   String password;
   int age;
   
   Object obj = new String("abc") // String 객체는 직렬화가 가능하다.
}
```
위 코드의 경우 직렬화가 가능한데, 인스턴스변수 obj타입이 직렬화가 안 되는 Object이기는 하지만 실제로 저장된 객체는 직렬화가 가능한 String 인스턴스이기 때문이다.
즉 인스턴스변수의 타입이 아닌 실제로 연결된 객체의 종류에 의해서 직렬화 가능 유무가 결정된다.

직렬화하고자 하는 객체의 클래스에 직렬화가 안 되는 객체에 대한 참조를 포함하고 있다면 제어자 transient를 붙여서 직렬화 대상에서 제외되도록 할 수 있다.
또는 password와 같이 보안상 직렬화되면 안 되는 값에 대해서 transient를 사용할 수 있다.
다르게 표현하면 transient가 붙은 인스턴스변수의 값은 그 타입의 기본값으로 직렬화된다고 볼 수 있다.
```java
public class UserInfo implements Serializable {
    String name;
    transient String password; // 직렬화 대상에서 제외된다.
    int age;
    
    transient Object obj = new Object(); // 직렬화 대상에서 제외된다.
}
```
위 코드의 UserINfo 객체를 역직렬화하면 참조변수인 obj와 password의 값은 null이 된다.

## 1.28 직렬화가능한 클래스의 버전관리
직렬화를 객체를 역직렬화할 때는 직렬화 했을 때와 같은 클래스를 사용해야한다.
클래스의 이름이 같더라도 클래스의 내용이 변경된 경우 역직렬화는 실패하며 다음과 같은 예외가 발생한다.

```
java.io.INvalidClassException : UserInfo; local clas imcompatible:
stream classdesc serialVersionUID = 6953673583338942489,
local class serialVerisionUID = -6256164443556992367
```
위 예외의 내용은 직렬화 할 때와 역직렬화 할 때의 클래스의 버전이 같아야 하는데 다르다는 의미이다.
객체가 직렬화될 때 클래스에 정의된 멤버들의 정보를 이용해서 serialVersionUID라는 클래스의 버전을 자동생성해서 직렬화 내용에 포함된다.
그러므로 역직렬화 할 때 클래스의 버전을 비교함으로써 직렬화할 때의 클래스의 버전과 일치하는지 확인할 수 있는 것이다.

그러나 static 변수나 상수 또는 transient가 붙은 인스턴스변수가 추가되는 경우에는 직렬화에 영향을 미치지 않기 때문에 클래스의 버전을 다르게 인식하도록 할 필요는 없다.
네트웤으로 객체를 직렬화해서 전송하는 경우 보내는 쪽과 받는 쪽이 모두 같은 버전의 클래스를 가지고 있어야 하는데 클래스가 조금만 변경되어도 해당 클래스를 재배포하는 것은 프로그램을 관리하기 어렵게 만든다.

이럴 때는 클래스의 버전을 수동으로 관리해줄 필요가 있다.

```java
class MyData implements java.io.Serializable {
    int value;
}
```
위와 같이 MyData라는 직렬화가 가능한 클래스가 있을 때, 클래스의 버전을 수동으로 관리하려면 다음과 같이 serialVersioUID를 추가로 정의해야한다.

```java
class MyData implements java.io.Serializable
{
    static final long serialVersionUID = 3518731767529258119L;
    int value;
}
```
위와 같이 클래스 내에 serialVersionUID를 정의하면 클래스의 내용이 변경되어도 클래스의 버전이 자동생성된 값으로 변경되지 않는다.
serialVersionUID의 값은 정ㅅ형이면 어떠한 값으로도 지정할 수 있지만 서로 다른 클래스간에 같은 값을 갖지 않도록 serialver.exe를 사용해서 생성된 값을 사용하는 것이 보통이다.
```java
$> serialver Mydata
MyData : static final long serialVersionUID = 3518731767529258119L;
```
serialver.exe 뒤에 serialVersionUID를 얻고자 하는 클래스의 이름만 적어주면 클래스의 serialVersionUID를 알아낼 수 있다.
serialval.exe는 클래스에 serialVersionUID가 정의되어 있으면 그 값을 출력하고 정의되어 있지 않으면 자동 생성한 값을 출력한다.
serialver.exe에 의해서 생성되는 serialVersionUID값은 항상 클래스의 멤버들에 대한 정보를 바탕으로 하기 때문에 이 정보가 변경되지 않는 한 항상 같은 값을 생성한다.
