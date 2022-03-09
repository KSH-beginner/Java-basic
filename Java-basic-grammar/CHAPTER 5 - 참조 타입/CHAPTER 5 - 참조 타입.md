# CHAPTER 5 : 참조 타입

## 5.1 데이터 타입 분류

데이터 타입은 기본 타입(primitive type), 참조 타입(reference type)으로 나뉘고, 참조 타입은 객체를 참조하는 타입이다!

### 참조 타입의 종류

: 배열 타입 / 열거 타입 / 클래스 / 인터페이스

기본 타입 변수는 값이 저장될 때 Stack 영역에 변수와 값이 모두 저장된다.

참조 타입 변수는 값을 저장할 때 값이 Stack이 아닌 Heap 영역에 생성되고, Stack 영역에 생성된 참조 타입 변수가 Heap영역의 값의 메모리 주소를 참조하는 구조다.

## 5.2 메모리 사용 영역

JVM은 OS에서 할당받은 메모리 영역(Runtime Data Area)을 3개로 구분한다!

**① 메소드 영역(Method Area)**

: JVM 시작 시 생성

: 로딩된 클래스 바이트 코드 내용 분석 후 저장

: 모든 스레드가 공유된다.

**＊메소드 영역에는 클래스 코드들이 올라간다!**

**② 힙 영역(Heap Area)**

: JVM 시작 시 생성

: 객체, 배열이 생성되는 영역

: 사용되지 않는 객체는 GC(Garbage Collector)가 자동으로 제거해준다.

**＊힙 영역에는 객체가 생성이 된다!**

![Untitled](CHAPTER%205%20%2081a08/Untitled.png)

**③ JVM 스택**

: 스레드별로 생성된다.

: 메소드를 호출할 때마다 Frame을 스택에 추가(push)

: 메소드가 종료되면 Frame 제거(pop)

: Frame 안에는 변수들이 들어있다.

**＊JVM 스택 영역에는 변수들이 생성된다!**

```java
public class A {
	public static void main(String[] args) {
			int sum = 0;
			if (sum == 0)  {
				int v2 = 10;
				int v3 = 20;
				sum = add(v2, v3);
			}

			System.out.plrintln(sum);

	}

	public static int add(int a, int b) {
			return a+ b;
	}

}
```

**이 코드 실행시 메모리 영역 중요 포인트!**

**① 메소드가 호출될 때마다 JVM 스택에 Frame이 추가된다.**

: main() 메소드 프레임 1개 생성 / add() 메소드 프레임 1개 생성

**② 메소드 프레임 안에는 그 메소드의 매개변수가 들어간다.**

: main() 메소드에는 매개변수인 args가 저장되고, add() 메소드에는 매개변수인 a, b가 저장된다.

: add 메소드는 add(v2, v3)로 호출되었으므로, add메소드 프레임에 a와 b의 값이 각각 v2, v3의 값으로 저장된다. (a = 10, b = 20)

**③ 닫는 중괄호 `}` 를 만나면 그 안에서 선언된 변수는 메모리에서 지워지고, 호출된 메소드는 프레임이 지워진다.**

: if문에서 사용된 변수, 메소드는 `v2`, `v3`, `add()` 이다.

: if문이 끝나고 나면, main() 메소드 프레임의 `v2` , `v3` 가 지워지고, `add()` 메소드 자체가 지워져서 프레임이 지워진다.

: 이때, `add()` 메소드에서 리턴한 값인 a+b = 30은 `main()` 메소드의 변수인 sum에 저장되고, if문 밖에서 선언되었기 때문에 sum은 `add()` 가 끝나고 프레임이 없어져도 30으로 남아있는다.

## 5.4 null과 NullPointerException

**① null**

null 값은 변수가 참조하는 객체가 없을 경우 초기값으로 사용가능하다.

따라서, 참조 타입의 변수에만 저장할 수 있다.

null로 초기화된 참조 변수는 Stack 영역에 null값이 저장된다.

**② NullPointerException**

NullPointerException은 예외의 한 종류로, 참조 변수가 null 값을 가지고 있을 때 객체의 필드나 메소드를 사용하려고 했을 때 발생하는 에러이다.

```java
int [] intArray = null;
intArray[0] = 10;  // NullPointerException 에러 발생
```

👉 배열이 없는 상태(null)에서 0번째 인덱스를 참조하려고 하면 NullPointerException 에러가 발생한다.

## 5.5 String 타입

: 문자열을 저장하는 ‘클래스’타입

String도 참조 변수이기 때문에 값을 저장하면 값은 Heap영역에 저장되고, Stack영역에서 Heap 영역의 메모리 주소를 참조하여 저장한다.

이때, 문자열 리터럴이 동일하다면 String 객체의 메모리 주소(Heap 영역)를 공유한다.

```java
String name1 = "신용권";
String name2 = "신용권";
```

![Untitled](CHAPTER%205%20%2081a08/Untitled%201.png)

**※ new 연산자를 이용한 String 객체 생성**

**new 연산자 : 객체 생성 연산자로, new로 생성하면 독립된 객체를 하나 생성한다.**

new 연산자로 String 객체를 생성하게 되면 같은 문자열이라고 하더라도 메모리 주소가 다르게, 즉 Heap 영역에서 같은 값을 가졌지만 메모리 주소가 다른 객체가 2개 생성된다.

따라서, 값이 같아도 다른 메모리 주소를 가지기 때문에 `name1 == name2` 는 `false` 이다.

따라서, 이때 문자열을 비교하려면 비교 연산자 `==` 대신 `equals()` 메소드를 사용해야한다.

```java
String name1 = new String("신용권");
String name2 = new String("신용권");
name1 == name2;   // false 반환
name1.equals(name2); // true 반환
```

![Untitled](CHAPTER%205%20%2081a08/Untitled%202.png)

## 5.6 배열 타입

### **배열 변수로 선언하는 방법 2가지**

**① 타입[ ] 변수; → `int[] intArray;`**

② 타입 변수 []; → `int intArray[];`

일반적으로 1번 방법을 많이 쓴다!!

### **배열을 생성하는 방법 2가지**

① 배열 변수 선언과 동시에 값 목록 대입

`String[] names = {"신용권", "홍길동", "김자바"};`

이렇게 선언과 동시에 값 목록을 대입하면 값 목록인 `{”신용권”,”홍길동”,”김자바”}` 가 인덱스를 가지고 Heap 영역에 저장되고, 변수 `names` 는 Heap 영역 값 목록의 메모리 주소를 참조한다.

**② 변수 선언 후에 값 목록 대입 : 대입할 때 `new 타입[]` 을 값 목록 앞에 붙여야한다!**

→ 이러한 경우는 배열을 매개변수로 가지는 메소드 호출 시 배열을 직접 생성해서 매개변수 값으로 줄 경우에 주로 사용한다. 

```java
String[] names = null;
names = new String[] {"신용권", "홍길동", "김자바"};
```

```java
int add(int[] scores) {...} // add 메소드는 scores 배열 변수를 매개변수로 받는다.
// 이때 매개변수로 scores라는 배열 변수를 받는다고 선언을 한 것이므로
// scores라는 배열 변수가 미리 선언이 되어 있는 것이다.
// 따라서 메소드 호출 시 변수 선언 후에 값 목록을 대입하는 방식을 사용해야한다.

// int result = add({95, 85, 90}); // 이렇게 하면 배열 변수가 미리 선언되었으므로 에러
int reuslt = add(new int[] {95, 85, 90}); // new 타입[]을 사용하여 값 목록을 대입 
```

**③ 배열 생성 시 값 목록이 없고, 나중에 값들을 저장할 배열을 미리 생성하고 싶은 경우**

→ new 연산자로 배열 생성

`타입[] 변수 = new 타입[길이];` 

`타입[] 변수;  / 변수 = new 타입[길이];`

```java
int[] intArray = new int[5];

int[] intArray;
intArray = new int[5];
```

: int 값 5개를 저장할 수 있는 배 열 객체를 Heap 영역에 만들겠다라는 의미

![Untitled](CHAPTER%205%20%2081a08/Untitled%203.png)

### **배열을 생성만 하고 값을 주지 않으면 해당 타입의 기본값들이 모든 인덱스에 저장된다.**

`int[] scores = new int[30];` 이렇게 배열의 크기만 정하고 값을 주지 않았을 때 scores 배열에는 int의 기본값인 0이 각각 하나씩, 30개 들어가있다.

---

### 5.6.6 커맨드 라인 입력

cmd에서 `java 클래스명` 을 입력하고 실행하면 `main()` 메소드를 실행하게 되는데 main 메소드는 String 타입의 args 배열을 매개변수로 받는다.

이때, `java 클래스명` 만 입력한다면  매개변수로 `String[] args = {};` 을 주는 것과 같다.

따라서, 매개변수의 배열 값을 주고 싶다면 cmd에서 자바 실행 시

 `java 클래스명 문자열0 문자열 1 문자열2 ... 문자열 n-1` 이렇게 띄어쓰기로 구분해서 값을 주면 args 배열에 값이 들어간다.

`String[] args = { 문자열0, 문자열1, 문자열2, ... , 문자열 n-1 };` 이렇게 주는 것과 동일하다..

`System.exit(0)` : 프로그램 강제 종료 코드

`Integer.parseInt(문자열)` : 문자열을 숫자로 바꿔주는 코드

→ `Integer.parseInt("80")` : 숫자 80으로 바꿔준다.

---

### 5.6.7 다차원 배열

2차원 배열 이상을 다차원 배열이라고 한다.

- 2차원 배열을 만드는 방법

①

`int[][] scores = new int[2][3];` : 2행 3열을 가지는 2차원 배열이 만들어진다.

(인덱스는 0, 1행 / 0, 1, 2열)

![Untitled](CHAPTER%205%20%2081a08/Untitled%204.png)

만들어지는 원리 : 먼저 크기가 2인 1차원 배열을 만들고 각각의 인덱스에서 크기가 3인 배열을 추가 생성해서 메모리 주소를 참조한다.

```java
scores.length; // 2 반환 -> 1차적으로 1차원 배열의 길이를 참조(배열 A의 길이)
scores[0].length; // 3 반환 -> scores[0]은 배열 B를 참조하므로 배열 B의 길이 반환
scores[1].length; // 3 반환 -> scores[1]은 배열 C를 참조하므로 배열 C의 길이 반환
```

② 값 목록을 이용한 2차원 배열

`타입[][] 변수 = { {값1, 값2, ...}, {{값 1, 값 2, ...}, ...};`

ex) `int[][] scores = { {95, 80}, {92, 96} };`

```java
String[] names = {
		{"홍길동", "신용권", "김자바"},
		{"이나영", "김혜원", "박수지"}
};
```

---

### 5.6.9 배열 복사

: 배열은 한 번 생성하면 크기를 변경할 수 없기 때문에 더 큰 크기의 배열을 만들고 싶다면, 큰 배열을 새로 만들고 이전 배열의 값들을 복사해야한다.

**배열 복사 하는 방법**

**① for문 이용**

```java
int[] oldIntArray = { 1, 2, 3};
int[] newIntArray = new int[5];

for(int i = 0; i < newIntArray.length; i++) {
	newIntArray[i] = oldIntArray[i];
} 
```

**② `System.arrayCopy()` 메소드 이용**

: `System.arrayCopy(Object src, int srcPos, Object dest, int destPos, int length);`

Object src : 이전 배열

int srcPos : 이전 배열의 복사 위치 : 몇 번 인덱스부터 복사할 건지

Object dest : 새로운 배열

int destPos : 새로운 배열의 붙여넣기 위치 : 몇 번 인덱스부터 붙여넣기할 건지

int length : 몇 개를 복사할 지

```java
String[] oldStrArray = {"java", "array", "copy"};
String[] newStrArray = new String[5];

System.arraycopy(oldStrArray, 0, newStrArray, 0, oldStrArray.length);
// oldStrArray를 0번 인덱스부터 oldStrArray.length만큼 복사하여
// newStrArray의 0번 인덱스에 붙여넣기
```

---

### 5.6.10 향상된 for문 : 파이썬의 한줄 for문과 비슷하다!

: for문에서 인덱스를 이용하지 않고 바로 항목 요소를 반복할 수 있다!

`for (타입 변수명(임의로 지정) : 배열명) { 실행문 }`

```java
int[] scores = { 95, 71, 84, 93, 87 };

int sum = 0;
for (int score : scores) { // scores의 값들(95, 71, 84, 93, 87)이 순차적으로
														//score에 저장
	sum = sum + score
}
```

## 5.7 열거 타입

: 한정된 값만을 갖는 데이터 타입 (ex : 요일 - 월,화,수,목,금,토,일 / 계절- 봄,여름,가을,겨울 / ...)

: 한정된 값 하나 하나는 열거 상수(Enumeration Constant)로 정의해야한다

### 열거 타입 선언

열거 타입 소스 파일 생성 : ~.java

※ 열거 타입 이름의 첫 문자는 대문자로 작성

`Week.java` / `MemberGrade.java` / `ProductKind.java`

### 소스 작성 방법

: 파일 이름과 동일한 이름으로 다음과 같이 선언

`public enum 열거타입이름 { ... }`

### 한정된 값인 열거 상수 정의 : 열거 상수의 이름은 모두 대문자로 작성 / _로 연결

`public enum Week { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, ... }`

`public enum LoginResult { LOGIN_SUCCESS, LOGIN_FAILED }`

### 열거 타입도 int, double, String처럼 타입의 한 종류이므로 열거 타입 변수를 선언할 수 있다.

`Week today;` / `Week reservationDay;`

### 열거 타입 변수 값 저장

`Week today = Week.SUNDAY;` 대신 이렇게 열거 변수의 값은 열거 상수 중 하나여야 하고, 이렇게 `열거타입.열거상수` 형태로 저장한다.

열거 타입 변수는 참조 타입이므로 null값을 저장할 수 있다.

### 열거 타입도 참조 타입이므로 객체를 참조하여 값을 가진다.

→ 열거 객체는 Heap 영역에 생성된다.

→ 메소드 영역에서 열거 객체를 참조한다.

![Untitled](CHAPTER%205%20%2081a08/Untitled%205.png)

---

### 5.7.3 열거 객체의 메소드

열거 타입은 컴파일 시 `java.lang.Enum` 클래스를 상속받아서 `java.lang.Enum` 의 메소드인

`name()` / `ordinal()` / `compareTo()` / `valueOf(String name)` / `values()` 를 사용할 수 있다.

`Week = { MONDAY, TUESDAY, ... , SUNDAY };` 일때,

1. `name()` : 열거 객체의 문자열을 리턴

```java
Week today = Week.SUNDAY;
String name = today.name(); // name에 "SUNDAY" 저장

```

1. `ordinal()` : 열거 객체의 순번을 리턴(인덱스 값)

```java
Week today = Week.SUNDAY;
int ordinal = today.ordinal(); // ordinal에 SUNDAY의 순번(인덱스)인 6 저장
```

1. `compareTo()` : 열거 객체를 비교해서 순번 차이(인덱스 차이)를 리턴

```java
Week day1 = Week.MONDAY;
Week day2 = Week.WEDNESDAY;
int result1 = day1.compareTo(day2); // -2
int result2 = day2.compareTo(day1); // 2
```

1.  `valueOf(String name)` : 주어진 문자열의 열거 객체를 리턴 

→ 주어진 문자열에 해당하는 메모리 주소?를 리턴 

→ 해당 변수가 문자열에 해당하는 객체를 가리키게 할 때 사용한다.

```java
Week weekDay = Week.valueOf("SATURDAY"); 
// weekDay 변수는 SATURDAY에 해당하는 객체를 가리키게 된다. (메모리 주소를 참조하게된다)
```

1. `values()` : 모든 열거 객체들을 배열으로 리턴

```java
Week[] days = Week.values();
for(Week day : days) {
	System.out.println(day);
}
```