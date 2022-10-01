## 1. 날짜와 시간
### 1.1 Calendar와 date
```Date``` 클래스는 JDK 1.0부터 ```Calendar``` 클래스는 JDK 1.1부터 지원한 날짜와 시간을 다룰 수 있는 클래스이다.
최근에는 JDK 1.8부터 새롭게 나온 ```java.time``` 패키지 내에 속한 클래스들로 시간을 관리한다.

#### Calendar와 GregorianCalendar
```Calendar```는 추상 클래스이므로 직접 객체를 생성할 수 없고, 메소드를 통해 완전히 구현된 클래스의 인스턴스를 얻어야 한다.
```java
Calendar cal = Calendar.getInstance();
```
```Calendar```를 상속받아 완전히 구현한 클래스로는 ```GregorianCalendar```와 ```BuddhistCalendar```가 있는데
```getInstance()``` 메소드는 시스템의 국가와 지역설정을 확인하여 태국인 경우는 ```BuddhistCalendar```, 그 외에는 ```GregorianCalendar```를 반환한다.

인스턴스를 직접 생성해서 사용하지 않고 이처럼 메소드를 통해 인스턴스를 반환받게 하는 이유는 최소한의 변경으로 프로그램이 동작할 수 있도록 하기 위한 것이다.

```getInstance()``` 메소드가 ```static```인 이유는 메소드 내의 코드에서 인스턴수 변수나 메소드를 호출하지 않으면서
```getInstance()```가 ```static```이 아니라면 위와 같이 객체를 생성한 다음에 호출해야 하는데 ```Calendar```는 추상클래스이기 때문에 객체를 생성할 수 없기 때문이다.

#### Date와 Calendar간의 변환
```Calendar```가 추가되면서 ```Date```는 대부분의 메소드가 ```deprecated``` 되었으므로 잘 사용되지 않는다.

그럼에도 불구하고 종종 ```Date```를 사용해야 할 필요가 생기므로 다음과 같이 변환이 가능하다.

```java
// 1. Calender -> Date 변환
Calendar cal = Calendar.getInstance();
Date d = new Date(cal.getTimeInMIllis());

// 2. Date -> Calendar 변환
Date d = new Date();
Calendar cal = Calendar.getInstance();
cal.setTime(d);
```

### 1.2 형식화 클래스
형식화 클래스란 형식화에 사용될 패턴을 정의하여 데이터를 정의된 패턴에 맞춰 형식화하거나 역으로 형식화된 데이터에서 원래의 데이터를 얻어낼 수도 있다.

### 1.3 DecimalFormat
```DecimalFormat``` 클래스는 숫자 데이터를 정수, 부동소수점, 금액 등의 다양한 형식으로 표현할 수 있으며, 그 역으로 텍스트 데이터를 숫자로 변환하는 것 또한 가능하다.

형식화 클래스이니만큼 원하는 형식으로 표현 또는 변환하기 위해서 패턴을 정의하는 것이 매우 중요하다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184584022-adb21a84-6fde-44ca-ac9f-8e33314b5601.png)
<br>

### 1.4 SimpleDateFormat
```Date``` 클래스와 ```Calendar``` 클래스를 사용하여 계산한 날짜를 ```SimpleDateFormat``` 클래스를 통해 원하는 양식으로 출력할 수 있다.

물론 ```Date``` 클래스와 ```Calendar``` 클래스만으로도 원하는 형태로 출력할 수 있지만, 매우 복잡하므로 ```SimpleDateFormat``` 클래스를 사용하는 것을 권장한다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184584174-a29ffe7b-c240-4c59-93ae-cb58c436e96b.png)
<br>

### 1.5 ChoiceFormat
```ChoiceFormat``` 클래스는 특정 범위에 속하는 값을 문자열로 변환한다.
연속적 또는 불연속적인 범위의 값들을 처리하는 데 있어 if문이나 switch문이 적절하지 못할 때 사용한다.

```ChoiceFormat``` 클래스를 통해 복잡한 코드를 간결하게 할 수 있다.

### 1.6 MessageFormat
```MessageFormat``` 클래스는 데이터를 정해진 양식에 맞게 출력할 수 있도록 도와주는 클래스이다.

주로 데이터가 들어갈 자리를 마련해 놓은 양식을 미리 작성하고 프로그램을 이용하여 다수의 데이터를 같은 양식으로 출력할 때 사용된다.

### 1.7 java.time 패키지
```java.time``` 패키지는 ```Date```와 ```Calendar```의 단점을 개선하기 위해 JDK 1.8 부터 제공하는 패키지이다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184584409-505300da-c35f-45d9-81a1-dbcc02d0e244.png)
<br>
위 패키지에 속한 클래스는 불변(immutable) 이다.
그러므로 날짜나 시간을 변경하는 메소드들은 기존의 객체를 변경하는 대신 항상 변경된 새로운 객체를 반환한다.
기존 Calendar 클래스는 변경 가능하므로, 멀티 쓰레드 환경에서 안전하지 못하다.

멀티 쓰레드 환경에서는 동시에 여러 쓰레드가 같은 객체에 접근할 수 있기 때문에 변경 가능한 객체는 데이터가 잘못될 가능성이 있으며, 이를 쓰레드에 안전(thread-safe) 하지 않다고 한다.

새로운 패키지가 도입되었지만, 앞으로도 기존에 작성된 프로그램과의 호환성 때문에 Date와 Calendar는 여전히 사용할 것이다.

### 1.8 java.time패키지의 핵심 클래스
```java.time``` 패키지는 날짜와 시간을 별도의 클래스로 분리해놓았다.
시간을 표현할 때에는 ```LocalTime``` 클래스를, 날짜를 표현할 때에는 ```LocalDate``` 클래스를 사용한다.
시간과 날짜가 모두 필요할 경우 ```LocalDateTime``` 클래스를 사용한다.
여기서 ```시간대(time-zone)```까지 다뤄야 한다면, ```ZonedDateTime```클래스를 사용한다.

```Calendar``` 클래스는 ```ZonedDateTime``` 클래스처럼 날짜와 시간 그리고 시간대까지 모두 가지고 있다.
Date 클래스와 유사한 ```java.time``` 패키지의 클래스는 Instant가 있는데 이 클래스는 날짜와 시간을 초 단위(나노초)로 표현한다.
날짜와 시간을 초단위로 표현한 값을 ```타임스탬프(time-stamp)```라고 부르는데 이 값은 날짜와 시간을 하나의 정수로 표현할 수 있어 날짜와 시간의 차이를 계산하거나 순서를 비교하는데 유리해서 데이터베이스에 많이 사용된다.

#### period와 Duration
날짜와 시간의 간격을 표현하기 위한 클래스이다.
```
날짜 - 날짜 = period
시간 - 시간 = Duration
```

#### 객체 생성하기 - now(), of()
```java.time``` 패키지에 속한 클래스의 객체를 생성하는 가장 기본적인 방법은 ```now()```와 ```of()```를 사용하는 것이다.

```now()```는 현재 날짜와 시간을 저장하는 객체를 생성한다.
```java
LocalDate date = LocalDate.now();
LocalTime time = LocalTime.now();
LocalDateTime dateTime = LocalDateTime.now();
ZonedDateTime dateTimeInKr = ZonedDateTime.now();
```

```of()```는 단순히 해당 필드의 값을 순서대로 지정해 주면 된다.
각 클래스마다 다양한 종류의 ```of()```가 정의되어 있다.
```java
LocalDate date = LocalDate.of(2015, 11, 23);
LocalTime time = LocalTime.of(23, 59, 59);

LocalDiteTime dateTime = LocalDateTime.of(data, time);
ZonedDateTime zDateTime = ZonedDateTime.of(dataTime, ZoneId.of("Asia/Seoul"));
```

#### TemporalUnit과 TemporalField
날짜와 시간의 단위를 정의해 놓은 것이 ```TemporalUnit``` 인터페이스이고, 이 인터페이스를 구현한 것이 열거형 ```ChronoUnit```이다
```TemporalField```는 년, 월, 일 등 날짜외 시간의 필드를 정의해 놓은 것으로, 열거헝 ```ChronoField```가 이 인터페이스를 구현하였다.
```java
LocalTime now = LocalTime.now(); // 현재 시간
int minute = now.getMinute(); // 현재 시간에서 분(minute)만 뽑아낸다.
int minute = now.get(ChronoField.MINUTE_OF_HOUR); // 위와 동일
```
날짜와 시간에서 특정 필드의 값만을 얻을 때는 get()이나, get으로 시작하는 이름의 메소드를 이용한다.
```java
LocalDate today = LocalDate.now(); // 오늘
LocalDate tomorrow = today.plus(1, ChronoUnit.DAYS); // 오늘에 1일을 더한다.
LocalDate tomorrow = today.plusDays(1); // 위와 동일
```

위와 같이 특정 날짜와 시간에서 지정된 단위의 값을 더하거나 뺄 때는 ```plus()``` 또는 ```minus()```에 값과 함께 열거형 ```ChronoUnit```을 사용한다.

다음은 ```get()```과 ```plus()```의 정의이다.
```java
int get(TemporalField field)
LocalDate plus(long amountToAdd, TemporalUnit unit)
```
특정 ```TemporalField```나 ```TemporalUnit```을 사용할 수 있는지 확인하는 메소드는 다음과 같다.
```java
boolean isSupported(TemporalUnit unit) // Temporal에 정의
boolean isSupported(TempralField field) // TemporalAccessor에 정의
```
해당 메소드들은 날짜와 시간을 표현하는 데 사용하는 모든 클래스에 포함되어 있다.

### 1.9 LocalDate와 LocalTime
```LocalDate```와 ```LocalTime```은 ```java.time``` 패키지의 가장 기본이 되는 클래스이며, 나머지 클래스들은 이들의 확장이므로 이 두 클래스만 잘 이해하면 된다.

객체를 생성하는 방법은 현재의 날짜와 시간을 ```LocalDate```와 ```LocalTime```으로 각각 변환하는 ```now()```와 지정된 날짜와 시간으로 ```LocalDate```와 ```LocalTime``` 객체를 생성하는 ```of()```가 있다.

둘 모두 ```static```으로, 객체 생성 없이 호출할 수 있다.
```java
LocalDate today = LocalDate.now();
LocalTime now = LocalDate.now();

LocalDate birthDate = LocalDate.of(1999, 12, 31); 
LocalTime birthTime = LocalTime.of(23, 59, 59);
```
```of()```는 다음과 같이 여러 가지 메소드가 제공된다.
```java
static LocalDate of(int year, Month month, int dayOfMonth)
static LocalDate of(int year, int month, int dayOfMonth)

static LocalTime of(int hour, int min)
static LocalTime of(int hour, int min, int sec)
static LocalTime of(int hour, int min, int sec, int nanoSecond)
```
일 단위나 초 단위로도 지정할 수 있다.

```parser()```를 이용하면 문자열을 날짜와 시간으로 변환할 수 있다.
```java
LocalDate birthDate = LocalDate.parse("1999-12-31");
LocalTime birthTime = LocalTime parse("23:59:59");
```

#### 특정 필드의 값 가져오기 - get(), getXXX()
```LocalDate```와 ```LocalTime```의 객체에서 특정 필드의 값을 가져올 때에는 다음과 같은 메소드를 사용하면 된다.

주의할 점으로는 ```Calendar```와 다르게 월(month)의 범위가 1 ~ 12이고, 요일은 월요일부터 1로 시작한다는 점이다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184591953-1da7bb0e-e11d-462b-86b8-6d6f94a89dd6.png)
<br>
이 외에도 ```get()```과 ```getLong()```이 있는데, 원하는 필드를 직접 지정할 수 있다.
대부분의 필드는 int 타입의 범위에 속하지만, 몇몇 필드는 int타입의 범위를 넘을 수 있으며, 이런 경우는 ```get()``` 대신 ```getLong()```을 사용해야 한다.
```java
int get(TemporalField field)
long getLong(TemporalField field)
```
위 메소드들의 매개변수로 사용할 수 있는 ```ChronoField```의 목록은 다음과 같다.
이 중 ```*```이 적힌 필드는 ```getLong()```을 사용해야 하는 필드이다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184592139-8e3e5ac6-f2c5-4c10-88cd-f2f7d9957632.png)
<br>
이 목록은 ```ChronoField```에 정의된 모든 상수의 목록일 뿐, 사용할 수 있는 필드는 클래스마다 다르다.

특정 필드가 가질 수 있는 값의 범위를 확인하고자 한다면 다음과 같이 코드를 실행하면 된다.
```java
System.out.println(ChronoField.CLOCK_HOUR_OF_DAY.range()); // 1 - 24
System.out.println(ChronoField.HOUR_OF_DAY.range()); // 0 - 23
```
#### 필드의 값 변경하기 - with(), plus(), minus()
날짜와 시간에서 특정 필드 값을 변경하려면, 다음과 같이 with로 시작하는 메소드를 사용하면 된다.

```with()```를 사용하면, 원하는 필드를 직접 지정할 수 있다.

필드를 변경하는 메소드들은 항상 새로운 객체를 반환하는 것을 유의해야 한다.
```java
LocalDate withYear(int year)
LocalDate withMonth(int month)
LocalDate withDayOfMonth(int dayOfMonth)
LocalDate withDayOfYear(int dayOfYear)

LocalTime withHour(int hour)
LocalTime withMinute(int minute)
LocalTime withSecond(int second)
LocalTime withNano(int nanoOfSecond)
```
다음과 같이 ```plus()``` 또는 ```minus()``` 메소드 또한 존재하며, 다음은 ```plus()``` 관련 메소드들만 명시한 목록이다.

```java
LocalTime plus(TemporalAmount amountToAdd)
LocalTime plus(long amountToAdd, TemporalUnit unit)

LocalDate plus(TemporalAmount amountToAdd)
LocalDate plus(long amountToAdd, TemporalUnit unit)

LocalDate plusYearsa(long yearsToAdd)
LocalDate plusMonths(long monthsToAdd)
LocalDate plusDays(long daysToAdd)
LocalDate plusWeeks(long weeksToAdd)

LocalTime plusHours(long hoursToAdd)
LocalTime plusMinutes(long minutesToAdd)
LocalTime plusSeconds(long secondsToAdd)
LocalTime plushNanos(long nanosToAdd)
```
```LocalTime```의 ```truncatedTo()``` 메소드는 지정된 것보다 작은 단위의 필드를 0으로 만든다.
```java
LocalTime time = LocalTime.of(12, 34, 56); // 12시 34분 56초
time = time.truncatedTo(ChronoUnit.HOURS); // 시 (hour)보다 작은 단위를 0으로 설정한다.
System.out.println(time); // 12:00
```
```LocalTime```과 달리 ```LocalDate```는 ```truncatedTo()```가 없는데, 그 이유는 ```LocalDate```의 필드인 년, 월, 일은 0이 될 수 없기 때문이다.

#### 날짜와 시간의 비교 - isAfter(), isBefore(), isEqual()
```LocalDate```와 ```LocalTime```도 ```compareTo()```가 오버라이딩 되어 있어, 다음과 같이 비교할 수 있다.
```java
itn result = dete1.compareTo(data2); // 같으면 0, date1이 이전이면 -1, 이후면 1
```
이보다 편리하게 비교할 수 있는 메소드를 제공한다.
```java
boolean isAfter(ChronoLocalDate other)
boolean isBefore(ChronoLocalDate other)
boolean isEqual(ChronoLocalDate other) // LocalDate에만 있음
```
```equals()```가 있는데도 ```isEqual()```을 제공하는 이유는 연표(chronology)가 같은 두 날짜를 비교하기 위해서이다. ```isEquals()```는 오직 날짜만 비교하기 때문이다.
```java
LocalDate kDate = LocalDate.of(1999, 12, 31);
JapaneseDate jDate = LocalDate.of(1999, 12, 31);

System.out.println(kDate.equals(jDate)); // false 연대가 다름
System.out.println(KDate.isEqual(jData)); // true
```

### 1.10 Instant
```Instant``` 클래스는 에포크 타임(EPOCH TIME, 1970-01-01 00:00:00 UTC)부터 경과된 시간을 나노초 단위로 표현한다.
단일 진법으로만 다루기 때문에 계산이 편리하다.
```java
Instant now = Instant.now();
Instant now2 = Instant.ofEphochSecond(now.getEpochSecond());
Instant now3 = Instant.ofEpochSecond(now.getEpochSecond(), now.getNano());
```
```Instant``` 인스턴스를 생성할 대에는 위와 같이 now()와 ofEpochSecond()를 사용한다.

```java
long epochSec = now.getEpochSecond();
int nano = now.getNano();
```
필드의 저장된 값을 가져오려면 위와 같이 코드를 작성하면 된다.

```Instant```는 시간을 초 단위와 나노초 단위로 나누어 저장한다.
오라클 데이터베이스의 타임스탬프(timestamp)처럼 밀리초 단위의 EPOCHTIME을 필요로 하는 경우를 위해 ```toEpochMilli()``` 메소드가 정의되어 있다.

```Instant``` 클래스는 항상 UTC(+00:00)를 기준으로 하기 때문에 ```LocalTime```과는 차이가 있을 수 있다.
시간대를 고려해야 하는 경우 ```OffsetDateTime```을 사용하는 편이 더 나은 선택이다.

#### Instant와 Date간의 변환
```Instant``` 클래스는 기존의 ```java.util.Date```를 대체하기 위한 것이며, JDK 1.8부터 ```Date```에 ```Instant``` 클래스로 변환할 수 있는 새로운 메소드가 추가되었다.
```java
static Date from(Instant instant); // Instant -> Date
Instant toInstant(); // Date -> Instant
```

### 1.11 LocalDateTimer과 ZonedDateTime
```LocalTime```과 ```LocalDate```를 합친 것이 ```LocalDateTime```이며, ```LocalDateTime```에 시간대(time zone)을 추가한 것이 ```ZonedDateTime```이다.

#### LocalDate와 LocalTime으로 LocalDateTime만들기
다음과 같은 방식으로 ```LocalDateTime```을 만들 수 있다.
```java
LocalDate date = LocalDate.of(2015, 12, 31);
LOcalTime time = LocalTime.of(12, 34, 56);

LocalDateTime dt = LocalDateTime.of(date, time);
LocalDateTime dt2 = date.atTime(time);
LocalDateTime dt3 = time.at(Date(date);
LocalDateTime dt4 = date.atTime(12, 34, 56);
LocalDateTime dt5 = time.atDate(LocalDate.of(2015, 12, 31));
LOcalDateTime dt6 = date.atStartOfday(); // dt6 = date.atTime(0, 0, 0);
```
```LocalDateTime```에도 날짜와 시간을 직접 지정할 수 있는 다양한 버전의 ```of()```와 ```now()```가 정의되어 있다.
```java
// 2015년 12월 31일 12시 34분 56초
LocalDateTime dateTime = LocalDateTime.of(2015, 12, 31, 12, 34, 56);
LocalDateTime today = LocalDateTime.now();
```
#### LocalDateTime의 변환
```LocalDateTime```을 ```LocalDate``` 또는 ```LocalTime```으로 변환할 수 있다.
```java
LocalDateTime dt = LocalDateTime.of(2015, 12,31, 12, 34, 56);
LocalDate date = dt.toLocalDate(); // LocalDateTime -> LocalDate
LocalTime time = dt.toLocalTime(); // LocalDateTime -> LocalTime
```
#### LocalDateTime으로 ZonedDateTime 만들기
```LocalDateTime```에 시간대(time-zone)를 추가하면 ```ZonedDateTime```이 된다.
기존에는 ```TimeZone``` 클래스로 시간대를 다뤘지만 새로운 시간 패키지에서는 ```ZoneId```라는 클래스를 사용한다.
```ZoneID```는 일광 절약시간(DST, Daylight Saving Time)을 자동적으로 처리해주므로 더 편하다.

```LocalDateTime```에 ```atZone()```으로 시간대 정보를 추가하면, ```ZoneDateTime```을 얻을 수 있다.
```java
ZoneId zid = ZoneId.of("Asia/Seoul");
ZoneddateTime zdt = dateTime.atZone(zid);
System.out.println(zdt); // 2015-11-27T17:47:50.451+09:00[Asia/Seoul]
```
```LocalDate```에 ```atStartOfDay()``` 메소드를 통해 ```ZoneId```를 매개변수로 지정하면 동일하게 ```ZonedDateTime```을 얻을 수 있다.
```java
ZonedDateTime zdt = LocalDate.now().atStartOfDay(zid);
System.out.prinln(zdt); // 2015-11-27T00:00+09:00[Asia/Seoul]
```
위 방식은 메소드의 이름```(atStartOfDay())```에서 알 수 있듯이 0시 0분 0초로 되어있는 것을 확인할 수 있다.

#### ZoneOffset
UTC로부터 얼마만큼 떨어져 있는지를 ```ZoneOffset``` 클래스로 표현한다.
서울은 ```+9```, UTC보다 9시간(32400초 = 60 60 9초)이 빠르다.
```java
ZoneOffset krOffset = ZonedDateTime.now().getOffset();
// ZoneOffset krOffset = ZoneOffset.of("+9"); // +-h, +-hh, +-hhmm, +-hh:mm
int krOffsetInSec = krOffset.get(ChronoField.OFFSET_SECONDS); // 32400초
```

#### OffsetDateTime
```ZonedDateTime```은 ```ZoneId```로 구역을 표시하는데, ```ZoneId```가 아닌 ```ZoneOffset```을 사용하는 것이 ```OffsetDateTime```이다.
```ZoneId```는 일광절약시간처럼 시간대와 관련된 규칙들을 포함하고 있는데, ZoneOffset은 단지 시간대를 시간의 차이로만 구분한다.
컴퓨터에게 일광절약시간처럼 계절별로 시간의 차이가 있는 것은 위험하다.
이무런 변화 없이 일관된 시간체계를 유지하는 것이 더 안전하다.
같은 지역 내의 컴퓨터 간에 데이터를 주고받을 때 전송시간을 표현하기에 ```LocalDateTime```이면 충분하지만 서로 다른 시간대에 존재하는 컴퓨터간의 통신에는 ```OffsetDateTime```이 필요하다.
```java
ZonedDateTime zdt = ZonedDateTime.of(date, time, zid);
OffsetDateTime odt = OffsetDateTime.of(date, time, krOffset);

// ZonedDatetime -> OffsetDateTime
OffsetDateTime odt = zdt.toOffsetDateTime();
```
```OffsetDateTime```은 ```ZonedDateTime```처럼 ```LocalDate```와 ```LocalTime```에 ```ZoneOffset```을 더하거나 ```ZoneDateTime```에 ```toOffsetDateTime()```을 호출해서 얻을 수 있다.

#### ZonedDateTime의 변환
```ZonedDateTime```도 ```LocalDateTime```처럼 날짜와 시간에 관련된 다른 클래스로 변환하는 메소드를 가지고 있다.
```java
LocalDate toLocalDate()
LocalTime toLocalTime()
LocalDateTime toLocalDateTime()
OffsetDateTime toOffsetDateTime()
long toEpochSecond()
Instant toInstant()
```
```ZonedDateTime```은 ```GregorianCalendar```와 가장 유사한 클래스이다.
```GregorianCalendar```와 ```ZonedDateTime```간의 변환방법만 알면 위에 나열한 메소드들을 이용하여 다른 날짜와 시간 클래스로 변환할 수 있다
```java
GregorianCalendar from(ZonedDateTime zdt) // ZonedDateTime -> GregorianCalendar
ZonedDateTime toZonedDateTime() // GregorianCalendar -> ZonedDateTime
```

### 1.12 TemporalAdjusters
```plus()```, ```minus()``` 만으로 하기 어려운 시간 계산을 위한 메소드들을 정의해 놓은 것이 ```TemporalAdjusters``` 클래스이다.

메소드의 목록은 다음과 같다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184622700-694edb46-92cd-4f56-b683-f3fcd6c30cdb.png)
<br>
#### TemporalAdjuster 직접 구현하기
```TemporalAdjuster``` 클래스의 메소드가 충분하지 않을 시 자주 사용되는 날짜계산을 해주는 메소드를 직접 만들 수 있다.

```LocalDate```의 ```with()```는 다음과 같이 정의되어있으며, ```TemporalAdjuster``` 인터페이스를 구현한 클래스의 객체를 매개변수로 제공해야한다.
```java
LocalDate with(TemporalAdjuster adjuster)
```
```with()```는 ```LocalTime```, ```LocalDateTime```, ```ZonedDateTime```, ```Instant``` 등 대부분의 날짜와 시간에 관련된 클래스에 포함되어 있다.

```TemporalAdjuster``` 인터페이스는 다음과 같이 추상 메소드 하나만 정의되어 있으며, 이 메소드만 구현하면 된다.
```java
@FunctionalInterface
public interface TemporalAdjuster
{
    Temporal adjustInfo(Temporal temporal);
}
```
실제로 구현해야하는 것은 ```adjustInto()```지만, ```TemporalAdjuster```와 같이 사용해야 하는 메소드는 ```with()```이다.

```with()```와 ```adjustInto()``` 중에서 어느 쪽을 사용해도 되지만 ```adjustInto()```는 내부적으로만 사용할 의도로 작성된 것이므로 ```with()``` 메소드를 사용하는 것이 바람직하다.

### 1.13 Period와 Duration
Period와 Duration은 다음과 같다.
```
날짜 - 날짜 = Period
시간 - 시간 = Duration
```
#### between()
두 날짜 date1과 date2의 차이를 나타내는 Period는 between으로 구할 수 있다.
```java
LocalDate date1 = LocalDate.of(2014, 1, 1);
LocalDate date2 = LocalDate.of(2015, 12, 31);

Period pe = Period.between(date1, date2);
```
시간차이를 구할 때는 ```Duration```을 사용하는 것을 제외하고는 ```Period```와 사용방법이 동일하다.

```Period```, ```Duration```에서 특정 필드의 값을 얻을 때는 ```get()```을 사용한다.
```java
long year = pe.get(ChronoUnit.YEARS);
long month = pe.get(ChronoUnit.MONTHS);
long day = pe.get(ChronoUnit.DAYS);

long sec = du.get(ChronoUnit.SECONDS);
int nano = du.get(ChronoUnit.NANOS);
```
```getUnits()```이라는 메소드로 ```get()```에 사용할 수 있는 ```ChronoUnit```의 종류를 확인할 수 있다.
```java
System.out.println(pe.getUnits)); // [Years, Months, Days]
System.out.println(du.getUnits)); // [Seconds, Nanos]
```
```Period```와 달리 ```Duration```은 ```getHours()```, ```getMinites()```와 같은 메소드가 없다.

그러므로 ```Duration```에서 시간과 분을 안전하게 얻기 위해서는 ```LocalTime```으로 변환하여 ```get()``` 메소드를 활용하는 편이 좋다.
```java
LocalTime tmpTime = LocalTime.of(0, 0).plusSecond(du.getSeconds());

int hour = tempTime.getHour();
int min = tmpTime.getMinute();
int sec = tmpTime.getSecond();
int nano = du.getNano();
```
#### between()과 until()
```until()```은 ```between()```과 거의 같은 일을 한다.
다만 ```between()```은 ```static``` 메소드이고, ```until()```은 인스턴스 메소드이다.
```java
Period pe = Period.between (today, myBirthday);
Period pe = today.until(myBirthDay);

long dday = today.until(myBirthDay, ChronoUnit.DAYS);
```
```Period```는 년월일을 분리해서 저장하므로 D-day를 구하려는 경우 두 개의 매개변수를 받는 ```until()```을 사용하는 것이 낫다.
날짜가 아닌 시간에도 ```until()```을 사용하는 것이 낫다.

#### of(), with()
Period는 of(), ofYears(), ofMonths(), ofWeeks(), ofDays()가 있다.
Duration은 of(), ofHours(), ofMinutes(), ofSeconds() 등이 있다.
또한 with() 메소드도 존재한다.
사용법은 LocalDate와 LocalTime과 동일하다.

#### 사칙연산, 비교연산, 기타 메소드
```java
pe = pe.minusYears(1).multipliedBy(2);
du = du.plusHours(1).dividedBy(60);
```
```plus()```, ```minus()``` 뿐만 아니라 곱셈과 나눗셈을 위한 메소드도 존재한다.

```Period```에는 나눗셈을 위한 메소드가 없는데, ```Period```는 날짜의 기간을 표현하는데 사용되므로 나눗셈을 위한 메소드가 유용하지 않아 제외한 것이다.

```isNegative()```는 해당 값이 음수인지 확인하며, ```isZero()```는 0인지 확인한다.

#### 다른 단위로 변환 - toTotalMonths(), toDays(), toHours(), toMinutes()
이름이 ```to```로 시작하는 메소드들은 ```Period```와 ```Duration```을 다른 단위의 값으로 변환하는 메소드이다.

```get()```은 특정 필드의 값을 그대로 가져오지만, ```to()``` 메소드들은 특정 단위로 변환한 결과를 반환한다는 차이가 있다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184623666-82bb3c09-a744-42e5-ab8b-c0d5d65de65b.png)
<br>

### 1.14 파싱과 포맷
형식화(formatting)와 관련된 클래스들은 ```java.time.format``` 패키지에 위치하며, 이 중에서 ```DateTimeFormatting``` 클래스가 핵심이다.
이 클래스는 자주 쓰이는 다양한 형식들을 기본적으로 정의하고 있으며, 그 외의 형식이 필요하다면 직접 정의해서 사용할 수 있다.

날짜와 시간의 형식화에는 ```format()``` 메소드를 사용한다.
이 메소드는 ```DateTimeFormatter``` 뿐만 아니라 ```LocalDate```나 ```LocalTime```에도 존재한다. 동일한 기능을 수행한다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184623829-7126556e-a357-4386-b713-c10ea5d921ff.png)
<br>

#### 로케일에 종속된 형식화
```DateTimeFormatter```의 static 메소드 ```ofLocalizedDate()```, ```ofLocalizedTime()```, ```lofLocalizedDateTime()```은 로케일(locale)에 종속적인 포맷터를 생성한다.
```java
DateTimeFormatter formatter = DateTimeFormatter.ofLocalizedDate(FormatStyle.SHORT);
String shortFormat = formatter.format(LocalDate.now());
```
FormatStyle의 종류에 따른 출력 형태는 다음과 같다.
<br>
![image](https://user-images.githubusercontent.com/62749021/184623942-0346d2bc-3bb3-4b40-9783-6d9bb655fa4c.png)
<br>
#### 출력형식 직접 정의하기
```DateTimeFormatter```의 ```ofPattern()```으로 원하는 출력형식을 직접 작성할 수 있다.
```java
DateTimeFormatter formatter = DtaeTimeFormatter.ofPattern("yyyy/MM/dd");
```
<br>
![image](https://user-images.githubusercontent.com/62749021/184624063-b8836039-3ce5-4ec1-9239-f7bff42779f0.png)
<br>

#### 문자열을 날짜와 시간으로 파싱하기
문자열을 날짜 또는 시간으로 변환하려면 static 메소드 ```parser()```를 사용하면 된다.
```java
static LocalDateTime parse(CharSequence text)
static LocalDateTime parse(CharSequence text, DateTimeFormatter formatter)
```
위와 같이 두 개의 ```parser()``` 메소드가 자주 사용된다.
```java
LocalDate date = LocalDate.parse("2016-01-02", DateTimeFormatter.ISO_LOCAL_DATE);
```
```DateTimeFormatter```에 상수로 정의된 형식을 사용할 때는 위와 같이 사용한다.
```java
LocalDate newDate = Localdate.parse("2001-01-01");
LocalTime newTime = LocalTime.parse("23:59:59");
LocalDateTime newDateTime = LocalDateTime.parse("2001-01-01T23:59:59");
```
자주 사용되는 기본적인 형식의 문자열은 ISO_LOCAL_DATE와 같은 형식화 상수를 사용하지 않고도 파싱이 가능하다.
```java
DateTiemFormatter pattern = DateTimeFormatter.ofpattern("yyyy-MM-dd HH:mm:ss");
LocalDateTime endofYear = LocalDateTime.parser("2015-12-31 23:59:59", pattern);
```
위와 같이 ```ofPattern()```을 이용하여 파싱이 가능하다.
