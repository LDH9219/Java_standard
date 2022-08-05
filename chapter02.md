# 변수(Variable)

## 변수(Variable)
변수 : 값을 저장할 수 있는 ```메모리 상의 공간``` 을 의미, 하나의 변수에 단 하나의 값

## 변수의 선언과 초기화
변수의 사용을 위해서는 변수를 선언해야하는데 변수의 선언방법은 다음과 같다.
```
int age; //age라는 이름의 변수 선언
변수타입 변수이름;
```
변수 타입 = 변수에 저장될 값이 어떤 타입인지 지정 ex)정수형, 실수형, 문자형
<br>
변수 이름 = 변수에 붙인 이름. 메모리 공간에 이름을 붙여주는 것. 같은 이름의 변수가 여러 개 존재해서는 안된다.

### 변수의 초기화
쓰레기값이 남아있을 수 있으므로 초기화를 반드시 해주어야 한다.

타입이 같은 경우 콤마를 통해 여러 변수를 한 줄에 선언할 수도 있다.
```
int a , b; 
int x = 0, y=0;
```

### 변수의 명명규칙
1. 대소문자가 구분되며 길이에 제한이 없다.
2. 예약어를 사용해서는 안 된다.
3. 숫자로 시작해서는 안된다.
4. 특수문자는 _ 와 $만 허용한다.

***
## 변수의 타입
### 기본형과 참조형
1. 기본형<br>
  -boolean, char, byte,short,int,long, float,double<br>
  -계산을 위한 실제 값을 저장한다.
2. 참조형<br>
  -객체의 주소를 저장한다. 기본형을 제외한 나머지 타입    
  
```
boolean은 true와 false 두 가지 값만 표현할 수 있으므로 가장 작은 크기인 1byte.
char은 자바에서 유니코드(2 byte 문자체계)를 사용하므로 2byte.
byte 는 크기가 1 byte라서 byte.
int(4 byte)를 기준으로 짧아서 short(2 byte), 길어서 long(8byte).
float는 실수값을 부동소수점(floating-point)방식으로 저장하기 때문에 float.
double은 float 보다 두 배의 크기(8 byte)를 갖기 때문에 double.

자료형:bit:byte
boolean:8:1
char:16:2
byte:8:1
short:16:2
int:32:4
long:64:8
float:32:4
double:64:8
```

### 상수와 리터럴(constant & literal)
1.상수    
상수를 선언하는 방법은 변수와 동일하며 변수의 타입 앞에 키워드 'fianl' 을 붙여준다. 상수의 이름은 모두 대문자로 하는것이 암묵적인 관례이다.
```
final int MAX_SPEED = 10;
```

2. 리터럴
그 자체로 값을 의미하는 것.
리터럴의 접미사
```
정수형 : L
실수형 : f, d
```

3. 형식화된 출력 - printf()
```
지시자 = 설명
%b = 불리언(boolean) 형식으로 출력
%d = 10진(decimal)정수의 형식으로 출력
%o = 8진(octal) 정수의 형식으로 출력
%x, %X = 16진(hexa-decimal) 정수의 형식으로 출력
%f = 부동 소수점(floating-point)의 형식으로 출력
%e, %E = 지수(exponent) 표현식의 형식으로 출력
%c = 문자(character)로 출력
%s = 문자열(string)로 출력
```

## 화면에서 입력받기 - Scanner
Scanner 클래스를 사용하기 다음 문장의 추가가 필요하다.
```
import java.util.Scanner;
```
그 다음, Scanner 클래스의 객체를 생성한다.
```
Scanner Sc = new Scanner(System.in);
```
그리고 nextLine()이라는 메서드를 호출하면 입력 대기 상태에 있다가 입력을 마치고 '엔터키(Enter)'를 누르면 입력한 내용이 문자열로 반환된다.
```
String input =sc.nextLine(); // 입력받은 내용을 input에 저장
int num = Integer.parseInt(input); // 입력받은 내용을 int 타입의 값으로 변환
Float.parseFloat()도 가능하다.
-- 
int num =sc.nextInt()
float num = sc.nextfloat()
```

## 진법
### 비트(bit)와 바이트(byte)
한 자리의 2진수를 '비트(bit, bianary digit)'라고 하며, 1비트는 컴퓨터가 값을 저장할 수 있는 최소 단위이다.    
그러나 1 비트는 너무 작은 단위이기 때문에 1 비트 8개를 묶어서 '바이트(byte)'라는 단위로 정의해서 데이터의 기본 단위로 사용한다.

```
n비트로 표현할 수 있는 10 진수
값의 개수 : 2^n
값의 범위 : 0 ~ 2^n-1
```
![image](https://user-images.githubusercontent.com/62749021/183057269-d2752a93-7330-4560-82d4-07cd147c6d27.png)

8진수는 2 진수 3자리를, 16진수는 2 진수 4자리를 각각 한자리로 표현할 수 있기 때문에 자리수가 짧아져서 알아보기도 쉽고 변환도 매우 간단하다.

### 음수의 2진 표현 - 2의 보수법
2의 보수 = 1의 보수 + 1
```
음수의 2진 표현을 구하는 방법
1. 음수의 절대값을 2진수로 변환한다. ex)5 => 0101
2. 1.에서 구한 2 진수의 1<->0 을 교체한다. ex)0101=>1010
3. 2.의 결과에 1을 더한다. ex)1010+1 => 1011
```
***
## 특수문자를 표현하는 방법
```
tab = \t
backspace = \b
form feed = \f
new line = \n
carriage return = \r
역슬래쉬 = \\
작은 따옴표 = \'
큰 따옴표 = \"
유니코드 문자 = \u유니코드 ex) char a ='\u0041'
```
