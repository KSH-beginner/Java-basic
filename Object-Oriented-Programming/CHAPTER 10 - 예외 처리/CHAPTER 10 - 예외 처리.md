# CHAPTER 10 : 예외 처리

## 10.1 예외와 예외 클래스

### 오류의 종류

- 에러(Error)
    - 하드웨어의 오동작 또는 고장으로 인한 오류
    - 에러가 발생되면  프로그램 종료
    - 정상 실행 상태로 돌아갈 수 없다.
- 예외(Exception)
    - 사용자의 잘못된 조작 또는 개발자의 잘못된 코딩으로 인한 오류
    - 예외가 발생되면 프로그램 종료
    - **예외 처리를 추가하면 정상 실행 상태로 돌아갈 수 있다!**
    

### 예외의 종류 - 2가지

- 일반(컴파일 체크) 예외(Exception)
    - 예외 처리 코드가 없다면 컴파일이 되지 않는 예외!
    
    → 컴파일러가 예외 처리 코드를 요구함 (컴파일 되지않음을 알려줘서)
    
- 실행 예외(RuntimeException)
    - 예외 처리 코드를 생략하더라도 컴파일이 되는 예외
    - 컴파일러가 알려주지 않기 때문에 경험에 따라 예외 처리 코드를 작성해야한다.

### 예외 클래스

- `java.lang.Exception` 만 상속받는 클래스 : `**일반 예외(Exception)**`
- `java.lang.Exception` + `java.lang.RuntimeException` 을 상속받는 클래스 : `**실행 예외(RuntimeException)**`

![Untitled](CHAPTER%2010%20e5e8a/Untitled.png)

## 10.2 대표적인 실행 예외(RuntimeException)들

### NullPointerException

- 객체 참조가 없는 상태, 즉 null 값을 갖는 참조 변수로 객체 접근 연산자인 도트(.)를 사용했을 때 발생

```java
String data = null;
System.out.println(data.toString());
```

👉 data의 객체가 없기 때문에 data의 toString 메소드를 호출하는 `data.toString()` 은 실행 예외, `NullPointerException` 이 발생한다!

### ArrayIndexOutOfBoundsException

- 배열에서 인덱스 범위를 초과하여 사용할 경우 발생

```java
public static void main(String[] args) {
	String data1 = args[0];
	String data2 = args[1];

	System.out.println("args[0] : " + data1);
	System.out.println("args[1] : " + data2);

}
```

👉 이때, 여기서 사용자가 입력값으로 값을 1개만 줬다면 args[1]은 존재하지 않는다. 따라서, args[1]을 참조하게 되면 `ArrayIndexOutOfBoundsException` 이 발생한다!

### NumberFormatException

- 문자열을 숫자로 변환하는 경우가 많다!
- 문자열을 숫자로 변환하는 메소드
    - 문자열 → int : `Integer.parseInt(String s)` : 주어진 문자열 s를 정수로 변환해서 리턴
    - 문자열 → double : `Double.parseDouble(String s)` : 주어진 문자열 s를 실수로 변환해서 리턴

👉 이때, 숫자로 변환될 수 없는 문자가 포함되어 있을 때 `NumberFormatException` 이 발생한다!

```java
String data1 = "100";
String data2 = "a100";

int value1 = Integer.parseInt(data1);
int value2 = Integer.parseInt(data2);
```

👉 value2에서 data2인 `a100` 의 `a` 는 정수로 변환할 수 없기 때문에 `NumberformatException` 발생

### ClassCastException

- 타입 변환이 되지 않을 경우 발생

![Untitled](CHAPTER%2010%20e5e8a/Untitled%201.png)

이러한 상속&구현 관계에서,

- 정상 코드(예외 발생 X)

```java
// Dog의 객체가 Animal 타입으로 들어갈때 자동 변환이 일어나서 Animal 타입 객체가 된다.
// Dog이 Animal 타입 객체로 자동변환된 animal을 다시 Dog 타입으로 강제 변환하고 싶을 때
// (Dog) animal 강제변환 사용
Animal animal = new Dog();
Dog dog = (Dog) animal;

// 위의 원리와 동일
RemoteControl rc = new Television();
Television tv = (Television) rc;
```

- 예외 발생 코드

```java
// Dog의 객체가 animal로 자동변환 되었는데
// animal의 강제 변환을 Cat으로 시켰을 때 -> 타입변환 예외 발생
Animal animal = new Dog();
Cat cat = (Cat) animal;

RemoteControl rc = new Television();
Audio audio = (Audio) rc;
```

## 10.3 예외 처리 코드(try-catch-finally)

### 예외 처리 코드란?

- 예외가 발생하면 프로그램 종료를 막고, 정상 실행을 유지할 수 있도록 처리하는 코드
    - 일반 예외 : 반드시 작성해야 컴파일 가능
    - 실행 예외 : 컴파일 시 예외 처리 코드를 요구하지 않아서 경험에 의해서 작성해야한다.

- `try-catch-finally` 구문을 이용해서 예외 처리 코드 작성!

```java
try {
    // 예외 발생 가능한 코드(개발자가 짠 코드)
} catch(예외클래스 e) {
    // 예외 처리 코드    
} finally {
    // 예외가 발생했든, 발생하지 않았든 항상 실행되는 코드    
}
```

※ `finally` 는 필수가 아니다.

 1. `finally` 를 작성하면 예외 발생 시 예외 처리 코드 실행 후 finally 안의 코드를 실행하고,

예외가 발생하지 않으면 try문 안의 코드를 실행 후 finally 안의 코드가 실행된다.

1. `finally` 를 작성하지 않으면 예외 발생 시 예외 처리 코드만 실행되고 프로그램이 종료된다.
    
    예외가 발생하지 않으면 try문 안의 코드만 실행된 후 프로그램이 종료된다.
    

## 10.4 예외 종류에 따른 처리 코드

### 다중 catch

```java
try {
    // ArrayIndexOutOfBoundsException 발생
    // NumberFormatException 발생
} catch(ArrayIndexOutOfBoundsException e) {
    // ArrayIndexOutOfBoundsException에 대한 예외 처리 코드
} catch(NumberFormatException e) {
    // NumberFormatException에 대한 예외 처리 코드    
}
```

👉 이렇게 try문 안에 여러 예외가 발생할 경우 예외 코드에 따른 catch를 다중으로 썼을 때, 여러 개 catch 중에서 예외 코드가 먼저 실행된 예외에 대한 예외처리 코드만 실행되고 종료된다.

즉, 여러 개의 catch 중에서 먼저 실행되는 단 1개의 catch 예외 처리 코드만 실행되는 것이다!

 

### catch의 순서

: catch는 단 1개만 실행되기 때문에 순서가 중요하다!

```java
try {
	// ArrayIndexOutOfBoundsException 발생
	// NumberFormatException 발생
} catch(Exception e) {
	// 예외처리 1
} catch(ArrayIndexOutOfBoundsException e) {
	// 예외처리 2
}
```

👉 `ArrayIndexOutOfBoundsException` 에 대한 예외처리 코드를 따로 처리하고 싶어서 `catch(ArrayIndexOutOfBoundsException e)` 를 주려고 할 때,

모든 예외는 `Exception` 클래스를 상속받기 때문에 `ArrayIndexOutOfBoundsException` 도 `Exception` 을 상속 받는다.

따라서, `ArrayIndexOutOfBoundsException` 가 발생하면 제일 위에 있는 catch인 `Exception` 의 예외처리 코드가 실행되고, `ArrayIndexOutOfBoundsException` 를 처리하기 위해 만든 `catch(ArrayIndexOutOfBoundsException e)` 의 코드는 실행되지 않는다.

따라서, 원래 의도대로 `ArrayIndexOutOfBoundsException` 만 따로 처리하고 나머지 예외는 `Exception` 으로 처리하고 싶다면, `catch(ArrayIndexOutOfBoundsException e)` 를 catch 중 제일 위에 작성하면 된다.

```java
try {
	// ArrayIndexOutOfBoundsException 발생
	// NumberFormatException 발생
} catch(ArrayIndexOutOfBoundsException e) {
	// 예외처리 1
} catch(Exception e) {
	// 예외처리 2
}
```

💡 따라서, 보통 `catch(Exception e)` 는 모든 예외를 처리하므로 맨 뒤에 작성하고, 특정 예외에 대한 처리 코드를 앞에 작성한다.

### 멀티 catch

- 자바 7부터는 하나의 catch 블록에서 여러 개의 예외 처리를 할 수 있다!
- 여러 개의 예외를 똑같은 예외 처리 코드 하나로 처리하고 싶을 때 멀티 catch 사용

```java
try {
	//ArrayIndexOutOfBoundsException / NumberFormatException 발생

	// 다른 Exception 발생
} catch(ArrayIndexOutOfBoundsException | NumberFormatException e) {
	// 예외 처리 코드1
} catch(Exception e) {
	// 예외 처리 코드2
}
```

👉 `|` (or)를 사용해서 `ArrayIndexOutOfBoundsException` 와 `NumberFormatException` 을 하나의 예외 처리 코드로 처리할 수 있다!

 

## 10.5 자동 리소스 닫기

### try-with-resources

- 예외 발생 여부와 상관없이 사용했던 리소스 객체(각종 입출력 스트림, 서버 소켓, 소켓, 각종 채널)의 `close()` 메소드를 호출해서 안전하게 리소스를 닫아주는 구문

```java
try(FileInputStream fis = new FileInputStream("file.txt")) {
		~~~
// 끝날 때 or 예외가 발생하는 즉시 fis.close() 자동 호출
} catch(IOException e) {
		~~~
}
```

👉 try 괄호 안에 리소스 객체? 를 선언하면 try 실행 블록이 끝날 때 자동으로 `close()` 메소드를 호출해서 안전하게 리소스를 닫아준다. 또한, 예외가 발생하면 발생하는 즉시 `close()` 메소드를 호출해서 안전하게 리소스를 닫아준다.

※ 리소스 객체를 여러 개 선언할 수도 있다. (선언 끝에 `;`  붙이기)

```java
try (
	FileInputStream fis = new FileInputStream("file1.txt");
	FileInputStream fos = new FileOutputStream("file2.txt")
) {
	~~
} catch(IOException e) {
	~~
}
```

**※ try-with-resources를 사용하기 위한 리소스 객체의 조건**

👉 `**java.lang.AutoCloseable` 인터페이스를 구현하고 있어야 한다!**

ex) `FileInputStream` / `FileOutputStream` 에는 `Autoclosable` 인터페이스가 구현되어 있다.

## 10.6 예외 떠넘기기(throws)

### ★throws★

- 메소드 선언부 끝에 작성

```java
리턴타입 메소드명(매개변수, ...) throws 예외 클래스1, 예외 클래스2, ... {
}
```

- 메소드에서 처리하지 않은 예외를 호출한 곳으로 떠넘기는 역할

```java
public void method1() {
    try {
        method2();
    } catch(ClassNotFoundException e) {
        // 예외 처리 코드
        Syetem.out.println("클래스가 존재하지 않습니다");
    }
}

public void method2() throws ClassNotFoundException {
    Class clazz = Class.forName("java.lang.String2");
}
```

👉 `method2()` 를 호출한 곳으로 예외들을 넘겨준다는 뜻!!

1. `method1()` 에서 `method2()` 를 호출하고 있다.
2. `method2()` 는 메소드 안에 예외가 발생할 수 있는 코드를 가지고 있다.
3. 원래는 예외 발생 처리를 예외가 발생할 수 있는 `method2()` 안에서 하지만, `throws` 를 사용하여 `method2()` 를 호출하는 곳에서 `throws`로 넘긴 예외를 처리하도록 한다!!
4. `method1()` 이 `method2()` 를 호출하고 있으므로 `method1()` 에서 `try-catch` 로 해당 예외를 처리한다.

👉 `method2()` 의 예외를 `method1()` 이 처리하도록 하는 것(떠넘기는 것)!

### **throws가 중요한 이유?**

👉 Java 프로그램을 개발할 때 JDK에서 제공해주는 다양한 API를 사용하는데, 처음 보는 API가 있을 때 그 API가 어떤 예외를 발생시킬 수 있는지 알 수 없다.

👉 이때, 그 API의 멤버에 `throws` 가 있어서 그 다음에 예외 종류가 나와 있다면?

💡 개발자 입장에서, **‘아! 이 API의 이 멤버는 이런 예외가 발생할 수 있으니 사용 시 내가 try-catch문으로 이 예외를 예외처리 해줘야겠다!’** 라고 인지하고 예외처리가 가능하다!!

이러한 이유 때문에, 제공해주는 API에는 throws 형태로 작성된 API가 많다.

 

## 10.7 사용자 정의 예외와 예외 발생

### 사용자 정의 예외 클래스 선언

- 자바 표준 API에서 제공하지 않는 예외
- 애플리케이션 서비스와 관련된 예외
    - 잔고 부족 예외, 계좌 이체 실패 예외, 회원가입 실패 예외, ...
    
    👉 이러한 경우에 사용자가 직접 예외 클래스를 만들어서 써야 한다!
    
    👉 또한, 예외 발생 방법도 알고 있어야 한다!
    
- 사용자 정의 예외 클래스 선언 방법

```java
// 내가 만들고 싶은 예외가 Exception(일반 예외)가 되게하고 싶다면 Exception 상속
// 내가 만들고 싶은 예외가 RuntimeExcetption(실행 예외)가 되게하고 싶다면 
// RuntimeExcetption 상속
public class XXXException extends [Exception | RuntimeException] {
		// 기본 생성자 추가
		public XXXException() {}
		// 생성자 추가 : 예외가 왜 발생했는지에 대한 메시지 정보를 가질 수 있도록
		// 매개변수로 메시지 정보를 받는 생성자 추가
		// 부모 클래스의 생성자를 호출하는 super의 매개값으로 메시지 정보를 줘서
		// 부모 클래스 (Exception / RuntimeException)의 생성자 매개값으로 
		// 메시지 정보 제공
		public XXXException(String message) { super(message); }
}
```

### 예외 발생 시키기 : `throw` 이용 (`throws` 와 다르다!)

```java
throw new XXXException();
throw new XXXException("메시지");
```

👉 `throw` 를 앞에 붙이고 생성자 객체를 생성하면 예외를 발생시킬 수 있다..?

※ 일반적으로 예외 발생 코드는 메소드 안에 넣어준다!

```java
public void method() throws XXXException {
	throw new Exception("메시지");
}
```

👉 메소드 실행 시 조건에 맞지 않으면 예외를 `throw` 로 발생시켜준다.

👉 이때, 예외를 발생시키는 메소드에서 예외 처리를 하는 것이 아니라 `throws` 로 사용자가 정의한 사용자 정의 예외 클래스로 떠넘겨서 사용자 정의 예외 클래스에서 예외 처리를 하도록 한다.

ex) `BalanceInsufficientException` 이라는 사용자 정의 예외 클래스를 만들고, 통장 잔고보다 출금액이 많으면 이 예외 처리를 발생시키기!

① `BalanceInsufficientException` 사용자 정의 예외 클래스

```java
// BalanceInsufficientException는 Exception(일반 예외)로 처리
public class BalanceInsufficientException extends Exception {
	// 기본 생성자 추가
	public BalanceInsufficientException() {}
	// 예외에 대한 메시지를 부모(Exception) 생성자의 매개값으로 보내주는 생성자 추가
	public BalanceInsufficientException(String message) {
		super(message);
	}
}
```

② 계좌 클래스(Account) - 예금은 작성되었다고 가정!

```java
public class Account {
	// 통장에서 출금액 money를 받아 출금하는 withdraw 메소드
	// 발생한 예외는 withdraw를 호출하는 쪽에서 처리하도록 throws 사용
	public void withdraw(int money) throws BalanceInsufficientException {
		// 만약, 출금액이 잔고보다 크다면 BalanceInsufficientException 예외 발생시키기
		if (balance < money) {
			throw new BalanceInsufficientException()
		}
		// 만약, 출금액이 잔고보다 적다면 정상 출금
		balance -= money
	}
}
```

③ 실행 클래스

```java
public class AccountExample {
	public static void main(String[] args) {
		Account account = new Account();
		// 예금하기
		account.deposit(10000);

		// 출금하기
		// withdraw()를 호출할 때 예외 처리를 해주지 않았으므로 컴파일 에러 
		// account.withdraw(1000); 
		
		// 따라서, try-catch를 사용해서 예외 처리 구문으로 호출
		try {
			account.withdraw(30000); // 잔고는 10000원이므로 예외 발생
			System.out.println(account.getBalance());
		} catch (BalanceInsufficientException e) {
				System.out.println("예외 발생");
		}
	}

}
```

👉 `예외 발생` 이 출력된다!!

👉 이러한 과정으로 사용자 정의 예외 클래스를 만들고 호출해서 예외 상황을 처리할 수 있다!! 

## 10.8 예외 정보 얻기 - `getMessage()` / `printStackTrace()`

### getMessage()

- 예외를 발생시킬 때 생성자 매개값으로 사용한 메세지를 리턴
    
    `throw new XXXException("예외 메세지");`
    
- 예외 코드 번호를 포함할 수 있다. (ex : 데이터베이스 예외 코드)
- catch() 절에서 활용

```java
} catch (Exception e) {
		String message = e.getMessage();
}
```

👉 발생된 예외가 e에 저장되는데, `e.getMessage()` 를 하면 위에서 생성한 예외 메세지를 리턴받는다.

### printStackTrace()

- 프로그램을 테스트하면서 오류를 찾을 때 활용된다.
- 예외 발생 코드를 추적한 내용을 모두 콘솔에 출력한다.

👉 이는, 개발자를 위한 도구로 개발 시에만 사용하고 개발이 다 되면 코드를 제거해야 한다.

왜냐하면, 예외가 발생하면 콘솔에 출력되는데 사용자가 보기에 좋지 않기 때문에!

### 예외 정보 얻기 예시

ex) 10.7절 예제

① Account 클래스

```java
public void withdraw(int money) throws BalanceInsufficientException {
	if (balance < money) {
			// 잔고 부족 예외 시 메세지 작성
			throw new BalanceInsufficientException("잔고부족:" + (money-balance) + "모자람");
	}
	balance -= money;
}
```

② 실행 클래스

```java
try {
	account.withdraw(30000);
	System.out.println("예금액" + account.getBalance());
} catch (BalanceInsufficientException e) {
		String message = e.getMessage();
		e.printStackTrace();
} 
```

👉 `String message = e.getMessage();` : message 변수에 Account 클래스의 withdraw 메소드에서 설정한 예외 메세지(잔고부족:20000 모자람 ㅜ)가 저장된다.

👉 `e.printStackTrace();` :  밑의 사진과 같이 예외 정보(메세지, 몇 번째 줄에서 예외 발생)를 출력한다.

![Untitled](CHAPTER%2010%20e5e8a/Untitled%202.png)