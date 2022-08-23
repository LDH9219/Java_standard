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