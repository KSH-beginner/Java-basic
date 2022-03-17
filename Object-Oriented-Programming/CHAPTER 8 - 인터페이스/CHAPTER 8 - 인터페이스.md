# CHAPTER 8 : 인터페이스

## 8.1 인터페이스의 역할

### 인터페이스란?

: 개발 코드와 객체가 서로 통신하는 접점

- 개발 코드는 객체의 내부 구조를 알 필요가 없고 인터페이스의 메소드만 알고 있으면 된다.

![Untitled](CHAPTER%208%20%206e104/Untitled.png)

: 인터페이스의 역할

- 개발 코드가 객체에 종속되지 않게 해서 객체를 교체할 수 있게 하는 역할
- 개발 코드 변경 없이 리턴값, 실행 내용이 다양해질 수 있다(다형성)

![Untitled](CHAPTER%208%20%206e104/Untitled%201.png)

→ 개발 코드는 인터페이스를 참조하므로 객체가 바뀌어도 개발 코드는 바뀌지 않는다.

## 8.2 인터페이스 선언

: 인터페이스 이름

- 자바 식별자 작성 규칙에 따라 작성 : 클래스와 동일

: 소스 파일 생성

- 인터페이스 이름과 대소문자가 동일한 소스 파일 생성 `인터페이스명.java`

: 인터페이스 선언

`[public] interface 인터페이스명 { ... }`

: 인터페이스의 구성 멤버

- 상수 : 원래는 `static final` 을 붙이지만,

인터페이스에는 `static final` 이 기본 값으로 설정되어 있다!

따라서, `타입 상수명 = 값;` 만 주면 된다.

- 추상 메소드 : `타입 메소드명(매개변수, ...);`
- 디폴트 메소드 : `default 타입 메소드명(매개변수, ...) { ... }`
- 정적 메소드 : `static 타입 메소드명(매개변수) { ... }`

① 상수 필드 선언

- 인터페이스는 상수 필드가 선언 가능하다

→ 데이터를 저장하지 않기 때문에 데이터를 저장할 인스턴스, 정적 필드가 필요없다!

- 인터페이스에서 선언된 필드는 모두 public static final;
- 상수명은 대문자
- 선언과 동시에 초기값을 지정해야한다!

```java
public interface RemoteControl {
/* 선언 시 초기화 X -> 컴파일 에러 발생
	int MAX_VOLUME; 
	int MIN_VOLUME; 	
*/

	int MAX_VOLUME = 10; 
	int MIN_VOLUME = 0; 
}
```

→ 인터페이스는 `public static field` 가 기본값이라서 `int MAX_VOLUME = 10;` 이렇게 실행해도 컴파일 시 자동으로 `public static final int MAX_VOLUME = 10;` 이렇게 컴파일된다!

② 추상 메소드 선언

- 인터페이스를 통해 호출된 메소드는 최종적으로 객체에서 실행된다.
- 인터페이스의 메소드는 기본적으로 실행 블록이 없는 추상 메소드로 선언한다!
- `리턴타입 메소드명(매개변수, ...);` : `public abstract` 는 기본값으로, 선언 X해도 된다

```java
public interface RemoteControl {
	// 실행 블록이 없는 추상 메소드로 선언
	void turnOn();
	void turnOff();
	void setVolume(int volume);
}
```

③ 디폴트 메소드 선언

- 자바8에서 추가된 인터페이스의 새로운 멤버

`[public] default 리턴타입 메소드명(매개변수, ...) { ... }`

- 실행 블록을 가지고 있는 메소드이다.
- default 키워드를 반드시 붙여야 한다.
- 기본적으로 public 접근 제한을 가지므로 생략하더라도 컴파일 시 `public` 이 생성된다.
- 일반 클래스에서 메소드를 작성하는 것처럼 작성하되, 반드시 default 붙이기!

```java
public interface RemoteControl {
	/* default가 없으면 안 된다 
	void setMute(boolean mute) {
			~~~
	}
	*/

	default void setMute(boolean mute) {
			~~~
	}
}
```

④ 정적 메소드 선언

- 자바 8에서 추가된 인터페이스의 새로운 멤버

`[public] static 리턴타입 메소드명(매개변수, ...) { ... }`

- 일반 클래스의 정적 메소드와 똑같다! static으로 선언

```java
public interface RemoteControl {
	static void changeBattery() {
		~~
	}
}
```

## 8.3 인터페이스 구현

### 구현 객체와 구현 클래스

: 인터페이스의 추상 메소드에 대한 실체 메소드를 가진 객체 = 구현 객체

: 구현 객체를 생성하는 클래스 = 구현 클래스

### 구현 클래스 선언

: 자신의 객체가 인터페이스 타입으로 사용할 수 있음을 `implements` 키워드로 명시

: 추상 메소드의 실체 메소드를 작성하는 방법

- 메소드의 선언부가 정확히 일치해야 한다.
- 인터페이스의 모든 추상 메소드를 재정의하는 실체 메소드를 작성해야한다!

→ 일부만 재정의할 경우 `abstract` 를 사용하여 추상클래스로 선언해야한다!!

```java
// RemoteControl의 추상 메소드는 3개인데 Television 클래스에서 
// 2개만 재정의 할 때 -> Television 클래스를 추상 클래스로 선언
public abstract class Television implements RemoteControl {
	public void tunrOn() {...}
	public void turnOff() {...}
}
```

→ 이런 경우는 잘 없다. 보통 인터페이스를 구현을 하기 위해 구현 클래스를 선언할 때는 인터페이스에 있는 모든 추상 메소드를 재정의한다! 

- 인터페이스의 모든 메소드는 public 접근 제한을 갖기 때문에 public보다 더 낮은 접근 제한으로 작성할 수 없다.
- `@Override` 어노테이션을 이용해서 정확히 재정의되었는지 확인하는 것이 좋다.

: 인터페이스를 사용한 구현 객체를 생성하는 방법

1. `인터페이스 변수 = 구현 객체;`
2. `인터페이스 변수;`
    
    `변수 = 구현객체;`
    
    ```java
    RemoteControl rc;
    // 이 RemoteControl 인터페이스를 implements한 클래스만 객체 생성 가능
    // 구현 객체 = Television, Audio
    rc = new Television();
    rc = new Audio(); 
    ```
    

※ 인터페이스는 직접 객체를 생성할 수 없다. 따라서, 구현 객체를 사용해야 한다!

`RemoteControl rc = new RemoteControl();` (X)

### ★익명 구현 객체★

: 명시적인 구현 클래스 작성을 생략하고 바로 구현 객체를 얻는 방법 (클래스 파일 생성 생략)

- 이름이 없는 구현 클래스 선언과 동시에 객체를 생성한다.

```java
인터페이스 변수 = new 인터페이스() {
	// 익명 구현 객체를 만드는 코드(실행 블록)
	// 인터페이스에 선언된 추상 메소드를 재정의하는 실체 메소드를 여기서 선언
};
```

- 인터페이스의 추상 메소드를 모두 재정의하는 실체 메소드가 있어야 한다.
- 추가적으로 필드와 메소드를 선언할 수 있지만 익명 객체 안에서(블록 안에서)만 사용할 수 있고, 인터페이스 변수로 접근할 수 없다.

: 주로 사용되는 곳

- UI 프로그래밍(Swing, javaFX, Android)에서 이벤트를 처리하기 위해 주로 사용
- 임시 작업 스레드를 만들기 위해 사용
- 자바 8부터 지원하는 람다식은 내부적으로 익명 구현 객체를 사용

: 익명 구현 객체도 클래스(바이트 코드)파일을 가지고 있다.

- 클래스$번호.class 파일명으로 생성된다.

### 다중 인터페이스 구현 클래스

```java
public class 구현클래스명 implements 인터페이스 A, 인터페이스 B {
		// 인터페이스 A에 선언된 추상 메소드의 실체 메소드 선언
		// 인터페이스 B에 선언된 추상 메소드의 실체 메소드 선언
}
```

이렇게 `implements` 뒤에 여러 인터페이스를 선언하여 한꺼번에 구현할 수 있다.

## 8.4 인터페이스 사용

### 인터페이스에 구현 객체를 대입하는 방법

대입 위치가 다양하다 : 필드, 생성자, 메소드, 로컬 변수 등등

```java
public class MyClass {
    // 필드
    RemmoteControl rc = new Television();
    
    // ★생성자
    MyClass(RemoteControl rc) {
        this.rc = rc;
    }

    // 메소드
    void methodA() {
        // 로컬 변수
        RemoteControl rc = new Audio();
    }
    // ★메소드
    void methodB(RemoteControl rc) { ... }
    
}
```

→ 생성자와 메소드의 매개변수에 구현 객체를 대입하게 되면 인터페이스를 구현하는 어떠한 객체든 그 생성자와 메소드의 매개변수로 올 수 있다!

### 추상 메소드 사용

```java
RemoteContro rc = new Television();
rc.turnOn();  // Television의 turnOn() 실행
rc.turnOff(); // Television의 turnOff() 실행
```

### 디폴트 메소드 사용 (클래스의 인스턴스 메소드와 비슷)

: 인터페이스만으로는 사용할 수 없다.

- 구현 객체가 인터페이스에 대입되어야 호출할 수 있는 인스턴스 메소드이다.

```java
RemoteControl.setMute(true); // X

// 가능
RemoteControl rc = new Television();
rc.setMute(true); 
```

- 모든 구현 객체가 가지고 있는 기본 메소드로 사용한다.

→ 필요에 따라 구현 클래스가 디폴트 메소드를 재정의해서 사용할 수 있다.

※ 재정의할 때는 `default` 를 붙이지 않는다.

### 정적 메소드 사용 (클래스의 정적 메소드와 비슷)

: 인터페이스로 바로 호출이 가능하다!

```java
public interface RemoteControl {
	static void changeBattery() {
		~~
	}
}

----------
// 구현 객체 없이 클래스의 정적 메소드처럼
// 인터페이스로 바로 호출 가능
RemoteControl.changeBattery(); 
```

## 8.5 타입변환과 다형성

### 다형성

: 하나의 타입에 여러가지 객체를 대입해서 다양한 실행 결과를 얻는 것

→ 하나의 타입 (상속 : 부모 / 인터페이스 : 인터페이스) 

→ 여러가지 객체 (상속 : 자식 / 인터페이스 : 구현 객체)

: 다형성을 구현하는 기술

- 상속 또는 인터페이스의 자동 타입 변환(Promotion)
- Overriding(재정의)

: 다형성의 효과

- 다양한 실행 결과를 얻을 수 있다!
- 객체를 부품화시켜서 유지 보수가 편리하다!

### 인터페이스를 이용한 다형성 개념

![Untitled](CHAPTER%208%20%206e104/Untitled%202.png)

 👉 실행 클래스에서 인터페이스를 호출하는 코드는 수정이 필요가 없다. 

(`i.method1();` , `i.method2();` )

👉  대신, 인터페이스 I의 method1, 2는 추상 메소드로 선언이 되었으므로, 그 인터페이스를 구현하는 A, B 클래스가 Overriding 하는 내용에 따라 메소드 내용이 달라진다.

👉  따라서, 실행 클래스에서 인터페이스 타입 변수 i에 객체 A를 대입하느냐, B를 대입하느냐에 따라 method1, method2의 내용이 달라진다.

👉  즉, A의 Overriding된 method1, 2를 쓰고 싶다면 구현 객체로 A를, B의 Overriding된 method1, 2를 쓰고 싶다면 구현 객체로 B를 실행 클래스에서 인터페이스 타입 변수에 넣어서 사용하면 된다.

: 인터페이스는 매개변수 타입으로 자주 등장한다.

- 메소드 호출 시 매개값으로 여러가지 종류의 구현 객체를 대입할 수 있어서 메소드 실행 결과가 다양하게 나온다.

ex) `RemoteControl` 이라는 인터페이스의 구현 클래스로 `Television` / `Audio` 가 있을 때

`public void useRemoteControl(RemoteControl rc) { ... }`

👉 여기서 `useRemoteControl` 메소드의 인터페이스 타입 매개변수 `RemoteControl rc` 에 구현 클래스인 `Television` / `Audio` 로 만든 구현 객체가 매개변수로 둘 다 들어갈 수 있다.

`useRemoteControl (new Television());`

`useRemoteControl (new Audio());`

이때, 구현 클래스마다 재정의한 메소드가 다르면 실행결과도 다르게 되므로 다형성이 구현된다.

### 자동 타입 변환(Promotion)

`인터페이스 변수 = 구현객체;` : 이때, 자동 타입 변환이 발생한다.

![Untitled](CHAPTER%208%20%206e104/Untitled%203.png)

👉 b, c, d, e 모두 자동 타입 변환된다.

### 인터페이스 타입 필드의 다형성

```java
public class Car {
    Tire frontLeftTire = new HankookTire();
    Tire frontRightTire = new HankookTire();
    Tire backLeftTire = new HankookTire();
    Tire backRightTire = new HankookTire();
}

void run() {
    frontLeftTire.roll();
    frontRightTire.roll();
    backLeftTire.roll();
    backRightTire.roll();
}
```

Car 클래스와, run 메소드가 이렇게 선언되어 있고, Tire 인터페이스가 구현 클래스로 HankookTire와 KumhoTire를 가지고 있을 때 

먼저 모든 바퀴(필드)의 구현 객체로 HankookTire를 선언한 후

```java
Car myCar = new Car();
myCar.frontLeftTire = new KumhoTire();
myCar.frontRightTire = new KumhoTire();
```

이렇게 앞 바퀴들(필드)에만 KumhoTire로 구현 객체를 바꿔주면

앞 바퀴(필드) : KumhoTire

뒷 바퀴(필드) : HankookTrie가 적용되어, 서로 다르게 재정의한 메소드가 실행되어서 다형성이 구현된다!!

### 인터페이스 배열로 구현 객체 관리

```java
Tire[] tires = {
    new HankookTire(),
    new HankookTire(),
    new HankookTire(),
    new HankookTire()
};

tires[1] = new KumhoTire();

void run() {
    for (Tire tire : tires) {
        tire.roll();    
    }
}
```

인터페이스 타입의 배열 tires를 생성하여 4개의 값을 HankookTire로 구현하고, 

나중에 tires의 1번 인덱스 값만 KumhoTire로 바꾸어서 향상된 for문으로 

각각의 객체의 roll 메소드를 실행시키면 1번 인덱스에만 KumhoTire의 roll() 메소드가 실행되고 나머지는 HankookTire의 메소드가 실행된다.

### 인터페이스 타입 매개변수의 다형성

- 메소드 호출 시 매개값으로 여러가지 종류의 구현 객체를 대입할 수 있어서 메소드 실행 결과가 다양하게 나온다.

ex) `RemoteControl` 이라는 인터페이스의 구현 클래스로 `Television` / `Audio` 가 있을 때

`public void useRemoteControl(RemoteControl rc) { rc.turnOn() }`

👉 여기서 `useRemoteControl` 메소드의 인터페이스 타입 매개변수 `RemoteControl rc` 에 구현 클래스인 `Television` / `Audio` 로 만든 구현 객체가 매개변수로 둘 다 들어갈 수 있다.

`useRemoteControl (new Television());`

`useRemoteControl (new Audio());`

이때, 구현 클래스마다 재정의한 메소드 `turnOn` 의 내용이 다르면 실행결과도 다르게 되므로 다형성이 구현된다.

### 강제 타입 변환(Casting)

: 인터페이스 타입으로 자동 변환 후, 다시 구현 클래스 타입으로 변환

- 강제 타입 변환의 목적 : 구현 클래스 타입에 선언된 다른 멤버를 사용하기 위해!

```java
interface Vehicle {
    void run();
}

class Bus implements Vehicle {
    void run() {...};
    void checkFare() {...}
}
```

현재 인터페이스 Vehicle에는 추상 메소드로 `run` 이 선언되어 있고 구현 클래스 Bus에서는 Vehicle 인터페이스의 추상 메소드를 Overriding한 `run` 메소드가 있고, 추가적으로 인터페이스 Vehicle에는 없는 Bus 클래스가 가지고 있는 `checkFare` 메소드가 있다.

```java
Vehicke vehicle = new Bus();

vehicle.run();  // 가능
vehicle.checkFare();  // 불가능

Bus bus = (Bus) vehicle; // 강제 타입 변환

bus.run();  // 가능
bus.checkFare();    // 가능
```

이때, 인터페이스 타입 변수에 Bus 구현 객체를 저장하고 인터페이스의 추상 메소드인 run을 실행하면 Bus에서 재정의된 run이 잘 실행된다.

하지만, Bus 클래스의 인터페이스에는 없는 추가적인 메소드 checkFare를 사용하는 것은 불가능하다.

따라서, Bus 구현 객체를 저장한 인터페이스 타입 변수인 vehicle을 다시 Bus 클래스의 객체로 강제 변환하면 run과 checkFare 메소드를 둘 다 사용할 수 있다!

### 객체 타입 확인(instanceof 연산자)

: 강제 타입 변환 전에 구현 클래스 타입을 조사한다.

조사하는 이유는, 강제 타입 변환이 될 수 있는지 없는지를 판별하기 위해!

만약, 현재 인터페이스 타입 변수의 구현 클래스 타입이 우리가 강제 타입 변환을 시키려는 타입이라면 강제 타입 변환을 할 수 있다.

하지만, 현재 인터페이스 타입 변수의 구현 클래스 타입과 강제 타입 변환을 시키려는 타입이 다르다면 강제 타입 변환을 할 수 없다!!

```java
public void method(Vehicle vehicle) {
	// 만약, 현재 인터페이스 타입 변수인 vehicle이 Bus 구현 클래스의 객체라면,
	if (vehicle instanceof Bus) {
	// 강제 타입 변환 가능 
		Bus bus = (Bus) vehicle;
	}
}
```

만약, vehicle 인터페이스의 구현 클래스로 Bus와 Taxi 이렇게 2개가 있을 때, 인터페이스 타입 변수를 매개변수로 갖는 method에 Bus의 강제 타입 변환을 하려고 할 때,

`if (vehicle instanceof Bus)` 코드를 생략하게 되면 인터페이스 타입 변수로 Taxi의 구현 객체가 저장된 인터페이스 타입 변수가 들어 왔을 때에도 `Bus bus = (Bus) vehicle;` 이 실행되어서 컴파일 에러를 발생시킨다.

따라서, Bus의 구현 객체가 저장된 인터페이스의 타입 변수만 Bus 타입으로 강제 타입 변환시키기 위해 `if (vehicle instanceof Bus)` 코드를 작성해야한다!!

## 8.6 인터페이스 상속

### 인터페이스 간에도 상속이 가능하다!

`public interface 하위인터페이스 extends 상위인터페이스1, 상위인터페이스2 { ... }`

※ 클래스는 단일 상속만 가능하지만, 인터페이스는 다중 상속이 가능하다!!

: 위의 상속 관계의 하위 인터페이스를 구현하는 구현 클래스는 아래 추상 메소드를 

모두 재정의해야한다.

- 하위 인터페이스의 추상 메소드
- 상위 인터페이스1의 추상 메소드
- 상위 인터페이스2의 추상 메소드

: 인터페이스 자동 타입 변환

- 해당 타입의 인터페이스에 선언된 메소드만 호출할 수 있다!

인터페이스 C에 인터페이스 A, B가 상속되었다면, 인터페이스 C를 타입으로 하는 객체에서는 인터페이스 A, B에 있는 메소드도 사용 가능하지만

인터페이스 A를 타입으로 하는 객체에서는 인터페이스 B에 있는 메소드를 사용할 수 없다!

## 8.7 디폴트 메소드와 인터페이스 확장

### 디폴트 메소드

- 실행 내용을 가지고 있는 디폴트 메소드 (자바 8부터 허용)
    - 인터페이스에 선언은 되지만, 인터페이스만으로 사용이 불가능하다.
    - 구현 객체의 인스턴스 메소드이므로 실행 블록에 코드를 넣어야한다.
    

### 디폴트 메소드의 필요성 - 인터페이스를 확장하기 위해서!!

- 기존 인터페이스에 추상 메소드를 추가할 수 없다.
    - 기존 인터페이스에 추상 메소드를 추가하면 기존 구현 클래스들에 모두 에러 발생
        
        ex) 
        
        1. 인터페이스 A에 method1이 추상메소드로 선언되어 있었다.
        2. A를 구현하는 구현 클래스 B가 method1을 재정의하고 있었다.
        3. 여기서, 인터페이스 A에 method2라는 추상 메소드를 추가하고 싶어서 추가했다.
        4. 이렇게 되면, 구현 클래스 B에 method2를 재정의하지 않았으므로 에러가 발생한다.
        
        👉 이를 해결하기 위해 디폴트 메소드 사용!!
        

- 디폴트 메소드는 추상 메소드가 아니다!
    - 따라서, 디폴트 메소드를 추가하더라도 기존 구현 클래스들은 문제 없이 사용 가능하다.
        
        (디폴트 메소드는 구현 클래스에서 재정의하지 않아도 에러가 발생하지 않기 때문에)
        
    - 인터페이스에서 기능(메소드)을 추가할 때, 디폴트 메소드로 추가하여 인터페이스의 추상 메소드뿐만 아니라 디폴트 메소드를 재정의하는 새로운 구현 클래스를 만들어서 기존 기능과 더불어서 추가 기능을 새로운 구현 클래스를 사용하여 구현할 수 있다.
    
- 디폴트 메소드가 있는 인터페이스 상속
    - 부모 인터페이스의 디폴트 메소드를 자식 인터페이스에서 활용하는 방법
        - 디폴트 메소드를 단순히 상속만 받는다.
        - 디폴트 메소드를 재정의(Override)해서 실행 내용을 변경한다.
        - 디폴트 메소드를 추상 메소드로 재선언한다.
        
        → 자식 인터페이스에서 디폴트 메소드를 재정의할 때 `defualt` 를 빼고 
        
        추상 메소드 형태로 재정의한다!