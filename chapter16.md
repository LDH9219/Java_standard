# 네트워킹(Networking)
## 1. 네트워킹(Networking)
네트워킹이란 두 대 이상의 컴퓨터를 케이블로 연결하여 네트워크를 구성하는 것을 말한다.
자바에서 제공하는 java.net 패키지를 사용하면 이러한 네트워크 어플리케이션의 데이터 통신 부분을 쉽게 장성할 수 있으며, 간단한 네트워크 어플리케이션은 단 몇 줄의 자바코드 만으로도 작성이 가능하다.

## 1.1 클라이언트 / 서버
컴퓨터간의 관계를 역할로 구분하는 개념이다.
```
서버 = 서비스를 제공하는 컴퓨터
클라이언트 = 서비스를 사용하는 컴퓨터
```

서버 기반 모델과 P2P 모델의 특징과 장단점
|서버기반 모델 | P2P 모델|
|:----|:-----|
|-안정적인 서비스의 제공이 가능하다. <br> -공유 데이터의 관리와 보안이 용이하다. <br> -서버구축비용과 관리비용이 든다. | -서버구축 및 운용비용을 절감할 수 있다. <br> -자원의 활용을 극대화 할 수 있다. <br> -자원의 관리가 어렵다. <br> -보안이 취약하다.|

## 1.2 IP 주소
-IP주소는 컴퓨터를 구별하는데 사용되는 고유한 값으로 인터넷에 연결된 모든 컴퓨터는 IP 주소를 갖는다.
-IP주소는 4byte(32 bit)의 정수로 구성되어 있으며, 4개의 정수가 마침표를 구분자로 a,b,c,d와 같은 형식으로 표현된다(a,b,c,d는 부호없는 1byte값, 즉 0~255 사이의 정수)
-IP주소는 네트워크주소와 호스트주소로 구성되어 있다.
-네트워크주소가 같은 두 호스트는 같은 네트워크에 존재한다.
-IP주소와 서브넷마스크를 ```&```연산하면 네트워크 주소를 얻는다.

## 1.3 InetAddress
<br>

![image](https://user-images.githubusercontent.com/62749021/186193911-92956461-c3a1-4fad-bda9-9d504526761d.png)

<br>

## 1.4 URL
인터넷에 존재하는 서버들의 자원에 접근할 수 있는 주소
```
http://www.javachobo.com:80/sample/hello.html?referer=javachobo#index1

프로토콜 : 자원에 접근하기 위해 서버와 통신하는데 사용되는 통신규약(http)
호스트명 : 자원을 제공하는 서버의 이름(www.javachobo.com)
포트번호 : 통신에 사용되는 서버의 포트번호(80)
경로명 : 접근하려는 자원이 저장된 서버상의 위치
파일명 : 접근하려는 자원의 이름(hello.html)
쿼리(query) : URL에서 '?'이후의 부분(referer=javachobo)
참조(anchor) : URL에서 '#'이후의 부분(index1)
```
<br>

![image](https://user-images.githubusercontent.com/62749021/186195136-a8f3207d-dc53-4a04-955b-db5043ca83c9.png)

<br>

![image](https://user-images.githubusercontent.com/62749021/186195233-0152db21-17fe-4050-b30a-5f902db3a939.png)

<br>

![image](https://user-images.githubusercontent.com/62749021/186195357-be4bf5f6-4d95-4c67-8b0d-7b0334afa289.png)

<br>

## 2. 소켓 프로그래밍
소켓을 이용한 통신 프로그래밍을 뜻한다.
```
소켓 : 프로세스간의 통신에 사용되는 양쪽 끝단(end point)
```
양쪽에 소켓이 필요하다.

## 2.1 TCP와 UDP
TCP/IP프로토콜에 포함된 프로토콜. OSI 7계층의 전송계층에 해당
<br>
![image](https://user-images.githubusercontent.com/62749021/186195937-0b417626-a017-4b2f-b972-147076772c11.png)

## 2.2 TCP 소켓 프로그래밍
클라이언트와 서버간의 1:1 소켓 통신
서버가 먼저 실행되어 클라이언트의 연결 요청을 기다리고 있어야 한다.
```
1. 서버는 서버 소켓을 사용해서 서버의 특정 포트에서 클라이언트의 연결 요청을 처리할 준비를 한다.
2. 클라이언트는 접속할 서버의 IP 주소와 포트정보로 소켓을 생성해서 서버에 연결을 요청한다.
3. 서버소켓은 클라이언트의 연결요청을 받으면 서버에 새로운 소켓을 생성해서 클라이언트의 소켓과 연결되도록 한다.
4. 이제 클라이언트의 소켓과 새로 생성된 서버의 소켓은 서버소켓과 관계없이 1:1 통신을 한다.
```

## 2.3 UDP 소켓 프로그래밍
TCP소켓 프로그래밍에서는 Socket과 ServerSocket을 사용하지만 UDP소켓 프로그래밍에서는 DatagramSocket과 DatagramPacket을 사용.
UDP는 연결지향적이지 않으므로 연결요청을 받아줄 서버소켓이 필요없다.
DatagramSocket간에 데이터(DatagramPacket)를 주고 받는다.

### TCP 실습
```java
//client
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;
import java.net.UnknownHostException;

public class Client {
	
	final static String SERVER_IP = "127.0.0.1"; 
	final static int SERVER_PORT = 1225;
	final static String MESSAGE_TO_SERVER = "Hi, Server";
	
	public static void main(String[] args) {
		
		Socket socket = null;
		
		try {
			/** 소켓통신 시작 */
			socket = new Socket(SERVER_IP,SERVER_PORT);
			System.out.println("socket 연결");
		
			/**	Client에서 Server로 보내기 위한 통로 */
			OutputStream os = socket.getOutputStream();
			/**	Server에서 보낸 값을 받기 위한 통로 */
			InputStream is = socket.getInputStream();
			
			os.write( MESSAGE_TO_SERVER.getBytes() );
			os.flush();
			
			byte[] data = new byte[16];
			int n = is.read(data);
			final String resultFromServer = new String(data,0,n);
			
			System.out.println(resultFromServer);
			
			socket.close();
		} catch (UnknownHostException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
		
	}
}
```
```java
//Server
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class Server extends Thread {
	
	final static int SERVER_PORT = 1225;
	final static String MESSAGE_TO_SERVER = "Hello, Client";
	
	public static void main(String[] args) {
		
		ServerSocket serverSocket = null;
		
		try {
			serverSocket = new ServerSocket(SERVER_PORT);
			
			
		} catch (IOException e) {
			e.printStackTrace();
		}
		
		try {
			while (true) {
				System.out.println("socket 연결 대기");
				Socket socket = serverSocket.accept();
				System.out.println("host : "+socket.getInetAddress()+" | 통신 연결 성공");
				
				/**	Server에서 보낸 값을 받기 위한 통로 */
				InputStream is = socket.getInputStream();
				/**	Server에서 Client로 보내기 위한 통로 */
				OutputStream os = socket.getOutputStream();
				
				byte[] data = new byte[16];
				int n = is.read(data);
				final String messageFromClient = new String(data,0,n);
				
				System.out.println(messageFromClient);
				
				os.write( MESSAGE_TO_SERVER.getBytes() );
				os.flush();
				
				is.close();
				os.close();
				socket.close();
			}
			
		} catch (IOException e) {
			e.printStackTrace();
		}
		
	}
}

class SocketRun implements Runnable {

	private Socket socket = null;
	
	SocketRun( Socket socket ){
		this.socket = socket;
	}
	
	@Override
	public void run() {
		
	}
}
```

### UDP 실습
```java
//Server
package networkingEx; 
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.text.SimpleDateFormat;
import java.util.Date;

import org.omg.CORBA.PUBLIC_MEMBER;

public class UdpServer {
    final static int SERVER_PORT = 8080;

     public void start() throws IOException {       
		// 포트 8080번을 사용하는 소켓을 생성한다.
		DatagramSocket socket = new DatagramSocket(SERVER_PORT);
      	DatagramPacket inPacket, outPacket;

		byte[] inMsg = new byte[10];
 		byte[] outMsg;         

		System.out.println("UDP 서버가 실행되었습니다.");

		while (true) {
			// 데이터를 수신하기 위한 패킷을 생성한다.
			inPacket = new DatagramPacket(inMsg, inMsg.length);
			// 패킷을 통해 데이터를 수신(receive)한다.            
			socket.receive(inPacket);
			// 수신한 패킷으로 부터 client의 IP주소와 Port를 얻는다.
			InetAddress address = inPacket.getAddress();
			int port = inPacket.getPort();
			// 서버의 현재 시간을 시분초 형태([hh:mm:ss])로 반환한다.            
			SimpleDateFormat sdf = new SimpleDateFormat("[hh:mm:ss]");
			String time = sdf.format(new Date());
			outMsg = time.getBytes(); // time을 byte배열로 반환한다.

			// 패킷을 생성하여 client에게 전송(send)한다.
			outPacket = new DatagramPacket(outMsg, outMsg.length, address, port);
			socket.send(outPacket);
        }    
	}
	public static void main(String args[]) {
        try {
            // UDP 서버를 실행시킨다.
            new UdpServer().start();
        } catch (IOException ie) {
            ie.printStackTrace();
        }
    }
}
```
```java
//client
package networkingEx;
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.UnknownHostException;

public class UdpClient {
	final static int SERVER_PORT = 7777;
	final static String SERVER_IP = "127.0.0.1";

	public void start() throws IOException, UnknownHostException {
		DatagramSocket datagramSocket = new DatagramSocket();
		InetAddress serverAddress = InetAddress.getByName(SERVER_IP);

		// 데이터가 저장된 공간으로 byte배열을 생성한다.
		byte[] msg = new byte[100];
		DatagramPacket outPacket = new DatagramPacket(msg, 1, serverAddress, SERVER_PORT);
		DatagramPacket inPacket = new DatagramPacket(msg, msg.length);

		// DatagramPacket을 전송한다.
		datagramSocket.send(outPacket);

		// DatagramPacket을 수신한다.
		datagramSocket.receive(inPacket);

		System.out.println("current server time : " + new String(inPacket.getData()));

		datagramSocket.close();
	}

	public static void main(String[] args) {
		try {
			new UdpClient().start();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```
