# CHAPTER 6 : 클래스

## 6.1 객체 지향 프로그래밍(OOP)

OOP : Object Oriented Programming

: 부품 객체를 먼저 만들고 객체들을 하나씩 조립해서 완성된 프로그램을 만드는 기법

### **객체** : 자신의 속성과 동작을 가지는 것!

속성 : 객체의 data(자동차의 색깔, 만든 회사, ...)

동작 :  객체가 하는 동작(자동차가 달린다, 멈춘다, ...)

→ 객체는 속성은 무조건 가지고 있고, 동작은 없을 수도 있다.

→ 필드(속성)과 메소드(동작)로 구성된 자바 객체로 모델링할 수 있다.

### **객체의 상호작용**

: 객체들은 서로 간의 기능(동작)을 이용하고 데이터를 주고 받는다!

→ 자바의 객체도 서로의 메소드를 호출하고 결과를 받는다!

`리턴값 = 전자계산기객체.메소드(매개값 1, 매개값 2, ...);`

`int result = Calculator.add(10, 20);` : 리턴 값을 int 변수에 저장

`Caculator` : 객체 / `add` : 객체가 가지고 있는 메소드(동작)

객체의 필드, 메소드를 호출할 때는 `.` 을 사용하여 객체에 접근한다!

### **객체간의 관계**

: 객체는 다른 객체와 관계를 맺는다.

① 집합 관계 : 완성품과 부품의 관계

→ 자동차 객체는 엔진, 타이어, 핸들이 조립되어서 만들어진 객체

② 사용 관계 : 객체가 다른 객체를 사용하는 관계

→ 사람 객체가 자동차 객체를 사용

③ 상속 관계 : 종류 객체와 구체적인 사물 객체 관계

→ 기계 객체에 자동차 객체가 포함된다!

![Untitled](CHAPTER%206%20%20bad2a/Untitled.png)

### 객체 지향 프로그래밍의 특징 (캡슐화, 상속, 다형성)

① 캡슐화 

: 객체의 필드, 메소드를 하나로 묶고 외부에 감추는 것!(그 안에서 노출시킬 필드, 메소드 결정)

: 외부 객체는 객체가 노출한 필드와 메소드만 이용할 수 있다!

: 객체의 필드, 메소드를 캡슐화하는 이유(숨기는 이유)는 외부에서 필드, 메소드를 잘못 수정 및 사용하여 객체가 손상되는 것을 방지하기 위해서이다!

: 노출시킬 필드, 메소드를 결정하기 위해 접근 제한자(Access Modifier)를 사용!

② 상속

: 부모(상위) 객체의 필드와 메소드를 자식(하위) 객체에게 물려주는 것!

: 하위 객체는 상위 객체를 확장해서 추가적인 필드, 메소드를 가질 수 있다.

![Untitled](CHAPTER%206%20%20bad2a/Untitled%201.png)

- 상속의 효과
1. 상위 객체를 재사용해서 하위 객체를 빠르게 개발
2. 반복된 코드 중복 제거
3. 유지 보수 편리
4. 객체의 다형성 구현 가능

③ 다형성

: 같은 타입이지만 실행 결과가 다양한 객체를 대입할 수 있는 성질

- 다형성의 효과
1. 객체를 부품화시킬 수 있다.

→ 여러 객체를 만들어 놓고 필요할 때마다 바꿔서 사용할 수 있다.

1. 유지 보수 편리

## 6.2 객체와 클래스

### **객체 : 실제로 사용하는 대상**

### **클래스 : 객체를 만드는 설계도**

: 클래스에는 객체를 생성하기 위한 필드와 메소드가 정의되어 있다.

: 클래스로부터 만들어진 객체를 해당 클래스의 인스턴스라고 한다!!!

: 하나의 클래스로부터 여러 개의 인스턴스를 만들 수 있다.

클래스로부터 인스턴스(객체)를 만드는 과정을 `인스턴스화` 라고 한다!

## 6.4 객체 생성과 클래스 변수

### **new 연산자**

객체 생성 : new 연산자로 생성한다!

`new 클래스();` : 클래스()는 생성자를 호출하는 코드

생성된 객체는 heap 영역에 생성된다.

**new 연산자는 객체를 생성한 후, 객체 생성 번지(메모리 주소)를 리턴한다!**

### **클래스 변수**

: new 연산자에 의해 리턴된 객체의 번지(메모리 주소)를 저장하는 참조 타입 변수!!

: Heap 영역에 생성된 객체를 사용하기 위해 사용된다!!

```java
// 생성 1 : 변수를 따로 선언 후 new 연산자로 생성된 객체의 메모리 주소 저장
클래스 변수;
변수 = new 클래스();

// 생성 2 : 변수 선언과 함께 new 연산자로 생성된 객체의 메모리 주소 저장
클래스 변수 = new 클래스();
```

ex) `Student` 클래스가 있을 때

`Student s1 = new Student();`

`Student s2 = new Student();` 

![Untitled](CHAPTER%206%20%20bad2a/Untitled%202.png)

### **클래스의 용도**

1. 라이브러리(API : Application Program Interface)용

: 자체적으로 실행되지 않고, 다른 클래스에서 이용할 목적으로 만든 클래스

1. 실행용

: `main()` 메소드를 가지고 있는 클래스로, 실행할 목적으로 만든 클래스

**1개의 애플리케이션 = 1개의 실행 클래스 + n개의 라이브러리 클래스**

## 6.5 클래스의 구성 멤버

: `필드(Field)` / `생성자(Constructor)` / `메소드(Method)` 

1. 필드 : 객체의 데이터가 저장되는 곳

→ 일반 변수와 비슷하지만, 일반 변수는  생성자 내부, 메소드 내부에 선언되고,

필드는 클래스 실행 블록 내부에 변수처럼 선언된다!

1. 생성자 : 객체 생성 시 초기화 역할

→ 클래스 이름과 동일한 이름으로 리턴 타입이 없는 메소드 형태로 선언한다.

→ `Student s1 = new Student();` 에서 new 옆의 `Student()`가 생성자를 호출하는 것이다.

1. 메소드 : 객체의 동작에 해당하는 실행 블록

![Untitled](CHAPTER%206%20%20bad2a/Untitled%203.png)

## 6.6 필드

### **필드의 내용**

: 객체의 고유 데이터

: 객체가 가져야할 부품 객체

: 객체의 현재 상태 데이터

![Untitled](CHAPTER%206%20%20bad2a/Untitled%204.png)

### **필드 사용 : 필드 값을 읽고, 변경하는 작업**

### **필드 사용 위치**

1. 객체 내부 : 필드이름으로 바로 접근

```java
public class Car {
	//필드
	int speed;

	// 생성자
	Car() {
		speed = 0;  // 이렇게 필드 이름으로 바로 접근 가능
	}
}
```

1. 객체 외부 : 변수.필드이름으로 접근

```java
// Car 클래스 외부에서 접근 시
public class Person {
	void method() {
		Car myCar = new Car();
		myCar.speed = 60; // 이렇게 클래스 변수.필드이름으로 접근
	}

}
```

## 6.7 생성자

: new 연산자에 의해 호출되어 객체의 초기화를 담당

### **기본 생성자(Default Constructor)**

: 모든 클래스는 생성자가 반드시 존재하고, 하나 이상의 생성자를 가질 수 있다.

: 생성자 선언을 생략하면 컴파일러가 자동으로 기본 생성자를 추가시킨다.

### **생성자 선언**

: 디폴트 생성자 대신 개발자가 직접 선언하는 방법

```java
public class Car {
		Car(String model, String color, int maxSpeed) {
				~~~
		}
}

Car myCar = new Car("그랜저", "검정", 300);
```

👉 model : 그랜저 / color : 검정 / maxSpeed : 300을 갖는 객체 생성 후 myCar 변수에 메모리 주소 저장

### **생성자의 매개값으로 필드 초기화**

```java
public class Korean {
	// 필드	
	String nation = "대한민국"; // 필드 선언과 동시에 초기화
	String name;
	String ssn;

	// 생성자
	// new 연산자로 생성자 호출 시 생성자의 매개값으로 필드 값을 넘겨줘서 초기화
	public Korean(String n, String s) {
		name = n;
		ssn = s;
	}
}
// 생성자 매개변수로 n = "김자바", s = "980111-1111111"이 되어 필드값이 초기화된다.
Korean k1 = new Korean("김자바", "980111-1111111");
```

### **생성자의 매개변수와 필드명이 동일할 경우 - this 사용**

: 관례적으로 매개변수 값을 필드값으로 설정할 경우 매개변수와 필드명을 동일하게 준다.

→ 위의 예제의 경우에서 이렇게 주는 것이 관례적이다.

```java
public Korean(String name, String ssn) {
		name = name;
		ssn = ssn;
}
```

👉 하지만, 이렇게 쓰면 누가 매개변수인지, 필드인지 알 수 없고 생성자 내부에서 매개변수가 필드보다 우선 순위가 높기 때문에 필드가 아니라 매개변수에 저장하게 된다.

따라서, 필드임을 명시하기 위해 `this.필드명` 이렇게 사용한다!

여기서 this는 객체 자신을 참조하는 것으로, 자기 자신의 번지(메모리 주소)를 가지고 있다.

따라서, `this.필드명` 은 내 자신의 객체에서 필드명을 호출하므로 필드가 된다.

```java
public Korean(String name, String ssn) {
		this.name = name;
		this.ssn = ssn;
}
```

👉 메소드에서도 동일하게 this를 사용한다!

### **생성자를 다양하게 만들어야 하는 이유**

: 객체를 생성할 때 new 연산자로 생성자를 호출할 때 그 매개변수의 값으로 객체를 초기화할 필요가 있다!

따라서, 외부의 값이 어떤 타입으로 몇 개가 제공될 지 모르기 때문에 미리 생성자를 다양하게 만들어 놔야 한다!

### **생성자 오버로딩(Overloading)**

: 매개변수의 타입, 개수, 순서가 다른 생성자를 여러 개 선언하는 것!

```java
public class Car {
	// 필드
	String company = "현대";
	String model;
	String color;
	int maxSpeed;

	// 생성자
	Car() {
	}

	Car(String model) {
		this.model = model;
	}

	Car(String model, String color) {
		this.model = model;
		this.color = color;
	}

	Car(String model, String color, int maxSpeed) {
		this.model = model;
		this.color = color;
		this.maxSpeed = maxSpeed;
	}

}
```

### **생성자에서 다른 생성자를 호출하는 방법**

: 생성자 오버로딩 시 생성자 간의 중복 코드가 발생할 수 있다.

: 초기화 내용이 중복될 때 사용한다.

`this()` 를 사용하여 중복을 줄일 수 있다.

`this()` 는 다른 생성자를 호출한다는 뜻이다.

```java
DCar(String model) {
		// this.model = model;
		this(model, null, 0);
	}

	Car(String model, String color) {
		// this.model = model;
		// this.color = color;
		this(model, color, 0);
	}

	Car(String model, String color, int maxSpeed) {
		this.model = model;
		this.color = color;
		this.maxSpeed = maxSpeed;
	}

```

![Untitled](CHAPTER%206%20%20bad2a/Untitled%205.png)

👉 중복되는 코드가 있는 생성자들에서 가장 크기가 큰 생성자에는 정상적으로 코드를 몰아서 적고, 나머지 중복 코드가 있는 생성자에는 `this()` 를 사용하여 가장 크기가 큰 생성자를 호출하는 방법이다.

이때, 생성자 자신이 필요한 값에만 매개변수를 입력하면 되고, 필요 없는 값은 초기값을 입력한다.

`this(model, null, 0)` / `this(model, color, 0)`

**※ 주의할 점 : 다른 생성자를 호출하는 `this()` 는 항상 생성자의 첫 줄에 와야한다!**

## 6.8 메소드

### **메소드란?**

: 객체의 동작(기능)을 말하며, 호출해서 실행할 수 있는 중괄호 {} 블록

### **메소드 선언**

`리턴타입 메소드이름(매개변수선언) { ... }`

`double divider(int x, int y) { ... }`

### **매개변수의 수를 모를 경우 방법**

1. 매개변수를 배열 변수로 선언 : `int sum1(int[] values) { ... }`
    
    호출 : `int result = sum1(new int[] {1, 2, 3, 4, 5});`
    
2. 타입을 적고 ... 후에 매개변수 선언 : `int sum2(int ... values) { ... }`
    
    호출 : `int result = sum2(1, 2, 3);`
    

→ 방법 1, 2는 형태는 다르지만 내용은 같다. (방법 2도 배열 변수로 값을 받는다)

→ 방법 1에서는 호출 시 배열을 만들어서 줘야하지만 방법 2는 배열없이 값만 줘도 된다.

### **메소드 오버로딩(Overloading) : 생성자 오버로딩과 기능, 목적 똑같다!**

: 클래스 내에 같은 이름의 메소드를 여러 개 선언

: 하나의 메소드 이름으로 다양한 매개값을 받기 위해 오버로딩한다!

## 6.9 인스턴스 멤버와 this

### 인스턴스 멤버란?

: 객체(인스턴스)마다 가지고 있는 필드와 메소드를 말한다!

: **인스턴스 멤버는 객체에 소속된 멤버이기 때문에 객체가 없이는 사용할 수 없다!**

```java
public class Car {
  // 필드
	int gas;

	// 메소드
	void setSpeed(int speed) { ... {
}
```

여기서, Car 클래스 안의 필드와 메소드(인스턴스 멤버)를 사용하기 위해서는 

`Car myCar = new Car();` 이렇게 객체를 만들어야 사용이 가능하다.

### this

: 객체(인스턴스) 자신의 번지(메모리 주소)를 가지고 있는 키워드이다!

: 객체 내부에서 인스턴스 멤버(필드, 메소드)임을 명확히 하기위해 this를 붙인다!

: 주로 매개변수와 필드명이 동일할 경우에 필드명을 구분하기 위해 `this.필드명`을 사용한다!

```java
// 생성자
Car(String model) {
	this.model = model;  // this.필드명으로 매개변수와 필드명 구분
}

// 메소드
void setModel(String model) {
	this.model = model;  // this.필드명으로 매개변수와 필드명 구분
}
```

## 6.10 정적 멤버와 static + 싱글톤

### 정적 멤버란?

: 클래스에 고정된 필드와 메소드를 말한다! → 이들을 정적 필드, 정적 메소드라고 부른다.

: 정적 멤버는 클래스에 소속된 멤버로, 객체 내부에 존재하지 않고, 메소드 영역에 존재한다.

→ **따라서 객체마다 생성되는 것이 아니므로 클래스로 바로 접근해서 사용해야한다.**

→ **객체를 만들 이유가 없다!**

### 정적 멤버 선언

: 필드 또는 메소드를 선언할 때 `static` 키워드를 붙이면 된다!

```java
public class 클래스 {
	// 정적 필드
	static 타입 필드;

	// 정적 메소드
	static 리턴타입 메소드(매개변수선언, ...) { ... }
}
```

### 정적 멤버 사용

: 클래스 이름과 함께 `.` 도트 연산자로 접근할 수 있다.

**정적 멤버를 사용할 때는 클래스명으로 바로 접근할 수 있으므로 객체를 생성하지 말자!**

```java
public class Calculator {
	static double pi = 3.14159;
	static int plus(int x, int y) { ... }
}

// Calculator myCalcu = new Calculator(); 이러한 객체 선언 필요 X

double result1 = 10 * 10 * Calculator.pi;
int result2 = Calculator.plus(10, 5);
```

### 인스턴스 멤버 선언 VS 정적 멤버 선언의 기준

- 필드

: 객체마다 가지고 있어야할 데이터 → 인스턴스 필드

: 공용으로 사용할 데이터 → 정적 필드

```java
public class Calculator {
    String color; // 객체마다(계산기마다) 색깔이 다를 수 있다
    static double pi = 3.14159; // 계산기에서 사용하는 파이 값은 모두 동일하다
}
```

- 메소드

: 작업하는 필드가 인스턴스 필드 → 인스턴스 메소드

: 작업하는 필드가 인스턴스 필드X → 정적 메소드

```java
public class Calculator {
    String color;
    void setColor(String color) {
        this.color = color;
    }
    static int plus(int x, int y) {
        return x + y;
    }    
    static int minus(int x, int y) {
        return x - y;
    }
    
}
```

👉 `setColor` 메소드는 인스턴스 필드인 `color` 를 사용하므로 static을 붙이면 안된다.

`this` 가 있으면 static을 사용하면 안 된다!

`plus` / `minus` 메소드는 인스턴스 필드를 사용하지 않으므로 static을 붙여 정적 메소드로 사용해도 된다.

### 정적 초기화 블록

: 클래스가 메소드 영역으로 로딩될 때 자동으로 실행하는 블록이다!

: 인스턴스의 필드 및 메소드를 실행 시 작업하는 생성자와 비슷한 역할이다.

: 정적 필드 초기화, 정적 메소드 호출 가능 

: 인스턴스 필드 초기화, 인스턴스 메소드 호출 불가능

```java
public class XXX {
    static int x;
    static {
        // 초기화 블록 안에서 for, if 문 등 복잡한 식 계산 가능
        x = 10; 
    }
}
```

### 정적 메소드와 정적 초기화 블록 작성 시 주의할 점

: 객체가 없어도 실행할 수 있어서 객체가 있어야 쓸 수 있는 인스턴스 필드, 인스턴스 메소드를 사용할 수 없다 + 객체를 참조하는 this를 사용할 수 없다.

```java
//인스턴트 필드와 메소드
int field1;
void method1() { ... }

//정적 필드와 메소드
static int field2;
static void method2() { ... }

//정적 블록
static {
    field1 = 10;  // 에러
    method1();    // 에러
    field2 = 10;
    method2();
}

//정적 메소드
static void Method3 {
    this.field1 = 10; // 에러
    this.method1();   // 에러
    field2 = 10;    
    method2();
}
```

※ 정적 메소드나 정적 초기화 블록 내에서 객체를 만들고 그 객체를 참조하면 인스턴스 필드, 메소드를 사용할 수 있다.

```java
static void Method() {
    ClassName obj = new ClassName();
    obj.field1 = 10;
    obj.method1();
}
```

※ `main()` 메소드도 정적 메소드이다!!

따라서, main() 메소드 안에서 인스턴스 필드, 메소드를 사용하고 싶다면 객체 생성 후 객체로 참조하여 사용해야한다.

### 싱글톤(Singleton)

: 하나의 애플리케이션 내에서 단 하나만 생성되는 객체를 말한다!

### 싱글톤을 만드는 방법

1. 외부에서 new 연산자로 생성자를 호출할 수 없도록 `private` 을 사용해서 막는다.
2. 클래스 자신의 타입으로 정적 필드를 선언하고 자신의 객체를 생성해 초기화한다.
3. 외부에서 호출할 수 있는 정적 메소드인 `getInstance()`를 선언한다.

→ 메소드의 이름은 상관없지만 관례적으로 `getInstance` 를 사용한다.

→ 정적 필드에서 참조하고 있는 자신의 객체를 return 하도록 한다.

```java
public class Singleton {
    //정적 필드
    private static Singleton singleton = new Singleton();
    
    //생성자
    private Singleton() {}
    
    //정적 메소드
    static Singleton getInstance() {
        return singleton;
    }
}
```

👉 이렇게하면 외부에서 객체를 얻고 싶을 때는 new로 생성할 수 없으므로 반드시 `getInstance()` 메소드를 거쳐서 singleton 객체를 얻어야 한다.

그래서 결국 `getInstance()` 메소드는 하나의 객체만 리턴하므로 이 클래스는 싱글톤이 된다.

### 싱글톤 얻는 방법

```java
Singleton obj1 = Singleton.getInstance();
Singleton obj2 = Singleton.getInstance();
```

![Untitled](CHAPTER%206%20%20bad2a/Untitled%206.png)

obj1과 obj2는 모두 같은 싱글톤 객체를 가리키게 된다!

## 6.11 final 필드와 상수(static final)

### final 필드

: 최종적인 값을 가지고 있는 필드 = 값을 변경할 수 없는 필드

: final 필드의 딱 한 번 초기값 지정하는 방법

1. 필드 선언 시 주는 방법
2. 생성자

```java
public class Person {
    final String nation = "Korea";
    final String ssn;
    String name;

    public Person(String ssn, String name) {
        this.ssn = ssn;
        this.name = name;
    }
}
```

`nation` 필드는 final 선언 후 값이 초기화가 되었으므로 절대 변경할 수 없지만, `ssn` 필드는 final이 선언된 후 초기화가 되지 않았다.

이때, 초기화는 생성자에서만 초기화가 가능하고 초기화가 되면 그 값을 수정할 수 없다.

외부의 값을 처음에 받아서 그 값을 유지하고 싶을 때 final을 선언하고 초기값을 주지 않고 생성자에서 외부 값을 받아 초기화가 되게 한다.

### 상수(static final)

- 상수 : 정적 final 필드

: final 필드 : 객체마다 가지는 불변의 인스턴스 필드

: static 필드(상수) : 객체마다 가지고 있지 않고, 메소드 영역에 클래스별로 관리되는 불변의 정적 필드, 공용 데이터로 사용한다.

- 상수 선언과 초기화

: 상수 이름 : 전부 대문자로 작성하는 것이 관례!

: 상수의 초기화 

1. 선언과 동시에 초기화
2. 정적 초기화 블록에서 초기화

```java
static final double EARTH_RADIUS = 6400;
static final double EARTH_SURFACE_AREA;

static {
    EARTH_SURFACE_AREA = 4 * Math.PI * EARTH_RADIUS * EARTH_RADIUS;
}
```

※ 상수도 final이므로 값이 한 번 초기화되면 수정할 수 없다!

## 6.12 패키지

### 패키지란?

1. 클래스를 기능별로 묶어서 그룹 이름을 붙여 놓은 것을 말한다!
- 파일들을 관리하기 위해 사용하는 폴더(디렉토리)와 비슷한 개념
- 패키지의 물리적인 형태는 파일 시스템의 폴더이다!

1. 패키지는 클래스 이름의 일부이다.
- 전체 클래스 이름 = 상위패키지.하위패키지.클래스
- 클래스명이 같아도 패키지명이 다르면 다른 클래스이다.

1. 클래스를 선언할 때 패키지가 결정된다.
- 클래스를 선언할 때 포함될 패키지를 선언해야한다.
- 클래스 파일은 선언된 패키지와 동일한 폴더안에서만 동작한다.

### 패키지 선언

: 패키지 선언은 클래스 선언 첫 줄에 해야한다.

: 상위 패키지와 하위 패키지는 도트(.)로 구분한다.

```java
package 상위패키지.하위패키지;

public class ClassName { ... }
```

: 패키지 이름 규칙

- 전부 알파벳 소문자로 작성하는 것이 관례
- 보통 회사 도메인의 역순으로 패키지 이름을 만든다.

### 패키지 선언이 포함된 클래스 컴파일

: 명령 라인에서 컴파일 할 경우

1. `javac XXX.java`
- `.class` 바이트 코드 파일만 생성되고 패키지가 자동으로 만들어지지 않아서 패키지별로 폴더를 직접 만들어서 바이트 코드 파일을 저장하고 사용해야한다.

1. `javac -d [패키지가 생성될 위치] XXX.java` : 패키지 폴더가 자동생성되고 바이트 코드 파일이 저장된다.

```java
javac -d . ClassName.java // 현재 폴더 내에 생성
javac -d ../bin ClassName.java // 현재 폴더와 같은 위치의 bin 폴더에 생성
javac -d C:\Temp\bin ClassName.java // C:\Temp\bin 폴더에 생성
```

### 패키지 선언이 포함된 클래스 실행

: 상위 패키지가 시작하는 곳에서 실행해야한다.

`java 상위패키지.하위패키지.클래스` 

### import문

: 패키지 내에 같이 포함된 클래스 간에는 클래스 이름으로 사용 가능

: 패키지가 다른 클래스를 사용해야할 경우 2가지 방법이 있다!

- 패키지명이 포함된 전체 클래스 이름으로 사용

```java
package com.mycompany;

public class Car {
	com.hankook.Tire tire = new com.hankook.Tire();
}
```

- import문으로 패키지를 지정하고 사용

```java
package com.mycompany;

import com.hankook.Tire;
//또는 import com.hankook.*;

public class Car {
    Tire tire = new Tire();
}
```

## 6.13 접근 제한자(Access Modifier)

: 클래스 및 클래스의 구성 멤버에 대한 접근을 제한하는 역할을 한다!

- 다른 패키지에서 클래스를 사용하지 못하도록 막는다. (클래스 제한)
- 클래스로부터 객체를 생성하지 못하도록 막는다 (생성자 제한)
- 특정 필드와 메소드를 숨김 처리한다. (필드와 메소드 제한)

![Untitled](CHAPTER%206%20%20bad2a/Untitled%207.png)

※ `default` 는 접근제한자로 아무것도 붙지 않은 것을 의미한다. (default로 따로 붙이는 것 X)

`private` 이 붙은 모든 요소들은 선언된 class 내부에서만 사용이 가능하다.

`default` , 즉 아무것도 붙지 않은 요소들은 같은 패키지의 클래스 내에서만 사용 가능하다.

`public` 이 붙은 모든 요소들은 아무런 제약 조건없이 어디에서나 사용가능하다.

### 클래스의 접근 제한

```java
//default 접근 제한
class 클래스 { ... }

//public 접근 제한
public class 클래스 { ... }
```

![Untitled](CHAPTER%206%20%20bad2a/Untitled%208.png)

### 생성자 접근 제한

`public` : new 연산자로 호출 가능

`default` : 같은 패키지에서만 new 연산자로 호출 가능

`private` : 현재 클래스를 제외한 다른 곳에서 new 연산자로 호출 불가능!

### 필드와 메소드 접근 제한

`public` / `default` / `private` 의 원리는 동일하다!

## 6.14 Getter와 Setter

: 클래스를 선언할 때 필드는 일반적으로 private 접근 제한을 한다!

- 읽기 전용 필드가 있을 수 있다. (Getter의 필요성)

→ 수정은 못 하지만 Getter를 사용하면 필드 값을 리턴해서 읽기가 가능하다.

- 외부에서 엉뚱한 값으로 변경할 수 없도록 한다. (Setter의 필요성)

→ Setter를 사용하면 외부에서 준 값이 올바른 값인지 검사할 수 있다.

### Getter

- private 필드의 값을 리턴하는 역할을 한다. (필요할 경우 필드 값을 가공해서 리턴)
- `getFieldName()` or `isFieldName()` 메소드를 말한다!

(필드 타입이 `boolean` 이면 `isFieldName()` , 나머지는 `getFieldName()` )

- Getter 작성 방법

① `getFieldName()`

```java
class Car {
    private double speed;
    
    public double getSpeed() {
        return speed;
    }
}
```

👉 field의 값을 접근 못하도록 `private` 으로 막아놓고 클래스 내부의 Getter 메소드는 `public` 으로 외부에서 접근이 가능하게 만들고 Getter는 클래스 내부에 있으므로 필드 값에 접근이 가능해서 Getter를 통해 필드 값을 읽을 수 있다.

② `isFieldName()` 

```java
public class Car {
    private boolean stop;
    public boolean isStop() {
        return stop;
    }
}
```

### Setter

- 외부에서 주어진 값을 필드값으로 수정한다.

→ 필요할 경우 외부의 값을 유효성 검사한다.

- `setFieldName(타입 변수)` 메소드이다.

→ 매개변수 타입은 필드의 타입과 동일하다.

```java
public class Car {
    private double speed;

    // 외부에서 speed 수정을 가능하게 하고 싶은데
    // 조건을 걸어서 조건에 맞는 값으로만 수정이 가능하게 하고 싶을때
    // speed가 0보다 작은 값으로 수정될 때는 0으로 수정되게 하고
    // 나머지 경우에는 수정되게 하기
    public void setSpeed(double speed) {
        if (speed < 0) {
            this.speed = 0;
            return;
        } else {
            this.speed = speed;
        }
    }
}
```

※ 인텔리제이 Getter / Setter 만드는 단축키 : 필드 선택 후 `ALT + INSERT`

## 6.15 어노테이션(Annotation)

### 어노테이션이란?

- 프로그램에게 추가적인 정보를 제공해주는 메타데이터(metadata)이다.

- 어노테이션 용도
1. 컴파일러에게 코드 작성 문법 에러를 체크하도록 정보를 제공
2. 소프트웨어 개발 툴이 빌드나 배치 시 코드를 자동으로 생성할 수 있도록 정보 제공
3. 실행 시(런타임 시) 특정 기능을 실행하도록 정보 제공

### 어노테이션 타입 정의와 적용

- 어노테이션 타입 정의

: 소스 파일 생성 : `AnnotationName.java`

: 소스 파일 내용 : `public @interface AnnotationName { ... }`

- 어노테이션 타입 적용 : `@AnnotationName`

- 어노테이션의 엘리멘트(element) 멤버

: 어노테이션을 코드에 적용할 때 외부의 값을 입력받을 수 있도록 하는 역할

: 엘리멘트 선언

```java
public @interface AnnotationName {
    //타입 elementName() (default 값);
    String elementName1();
    int elementName2() default 5;
}
```

: 어노테이션 적용 시 엘리먼트 값 지정하는 방법

`@AnnotationName(elementName1="값", elementName2=3);`

ex) 스프링에서 해당 클래스가 컨트롤러 역할을 하게 만들 때

```java
@Controller(name="main")
public class MainController { ... }
```

: 기본 엘리먼트 value

```java
public @interface AnnotatitonName {
    // 기본 엘리먼트 선언 (value)
    String value();
    int elementName() default 5;
}
```

- 어노테이션을 적용할 때 엘리먼트명 생략 가능

`@AnnotationName("값");` : value에 값이 저장된다.

```java
@WebServlet("/main") // "/main"은 @WebServlet의 value에 들어간다.
public class MainServlet { ... }
```

- 두 개 이상의 속성을 기술할 때는 `value = 값` 형태로 기술

`@AnnotationName(value="값", elementName=3);`

### 어노테이션 적용 대상

: 코드 상에서 어노테이션을 적용할 수 있는 대상

: java.lang.annotation.ElementType 열거 상수로 정의되어 있다!

![Untitled](CHAPTER%206%20%20bad2a/Untitled%209.png)

: 어노테이션 적용 대상 지정 방법

- `@Target` 어노테이션으로 적용 대상 지정
- `@Target` 의 기본 엘리먼트인 value의 타입은 ElementType 배열 (배열 값 목록`{}`으로 지정)

`ElementType.열거상수` 로 적용 대상을 지정한다!

```java
@Target{(ElementType.TYPE, ElementType.FIELD, ElementType.METHOD)}
public @interface AnnotatitonName {
}
```

👉 타입, 필드, 메소드에 이 어노테이션을 적용할 수 있다는 의미!

```java
@AnnotationName // 클래스에 어노테이션 적용 O
public class ClassName {
    @AnnotationName // 필드에 어노테이션 적용 O
    private String fieldName; 
    
    @AnnotationName // 생성자는 적용대상이 아니므로 어노테이션 적용 X
    public ClassName() {}
    
    @AnnotationName // 메소드에 어노테이션 적용 O
    public void methodName() {}
}
```

### 어노테이션 유지 정책

: 어노테이션 적용 코드가 유지되는 시점을 지정하는 것

(이 어노테이션의 정보를 언제까지 얻을 수 있는가? 를 지정)

: java.lang.annotation. RetentionPolicy 열거 상수로 정의되어 있다

![Untitled](CHAPTER%206%20%20bad2a/Untitled%2010.png)

※ 리플렉션(Reflection) : 런타임(실행 시)에 클래스의 메타정보를 얻는 기능

- 클래스의 메타정보 : 클래스의 필드, 생성자, 메소드 등의 개수, 종류 등등의 정보
- 런타임 시(실행 시)에 어노테이션 정보를 얻으려면 유지 정책을 `RUNTIME` 으로 설정해야 함

`SOURCE` : 개발자가 소스를 분석할 때만 의미가 있고, 컴파일 시 컴파일러가 어노테이션을 지운다. 바이트 코드 파일(.class)에 정보가 남지 않는다.

`CLASS` : 컴파일러가 바이트코드를 만들 때까지 정보를 유지한다. 하지만, 어노테이션의 값을 클래스를 실행시킬 때 얻을 수 없다. 클래스 상에만 존재하고 실행 시에는 정보를 얻지 못한다.

`**RUNTIME**` : 바이트코드 파일까지도 유지 + 실행 시에도 어노테이션의 정보를 읽고 이용 가능하다. 따라서, 대부분은 RUNTIME을 이용한다!

: 유지 정책 지정 방법 

- `@Retention` 어노테이션으로 유지 정책을 지정
- `@Retention`의 기본 엘리먼트인 value의 타입은 RetentionPolicy(RetentionPolicy.열거상수 로 지정)

```java
@Target{(ElementType.TYPE, ElementType.FIELD, ElementType.METHOD)}
@Retention(RetentionPolicy.RUNTIME) 
public @interface AnnotatitonName {
}
```

### 런타임 시에 어노테이션 정보 사용하기

: 클래스에 적용된 어노테이션 정보 얻기

- 클래스.class의 어노테이션 정보를 얻는 메소드를 이용

: 필드, 생성자, 메소드에 적용된 어노테이션 정보 얻기

- 클래스.class의 메소드를 이용해서 java.lang.reflect 패키지의 Field, Constructor, Method 클래스의 배열을 얻어냄

![Untitled](CHAPTER%206%20%20bad2a/Untitled%2011.png)

- 배열을 얻어낸 후 Field, Constructor, Method의 어노테이션 정보를 얻는 메소드 사용

: 어노테이션 정보를 얻기 위한 메소드 

![Untitled](CHAPTER%206%20%20bad2a/Untitled%2012.png)

`isAnnotationPresent(클래스명..?)` : 어노테이션이 적용되었으면 true 리턴

`getAnnotation(클래스명..?)` : 지정한 어노테이션이 적용되어 있으면 어노테이션을 리턴, 아니면 null 리턴

`getAnnotations()` : 적용된 어노테이션의 개수만큼 배열로 리턴, 적용된 어노테이션이 없을 경우에는 길이가 0인 배열 리턴

`getDeclaredAnnotations()` : 부모 클래스에 정의된 어노테이션을 제외하고 현재 클래스에 적용된 어노테이션만 포함해서 리턴한다!

### 어노테이션 예제

```java
[method1]
-------------
실행 내용 1

[method2]
*************
실행 내용2

[method3]
######################
실행 내용3
```

이렇게 출력을 하고 싶을 때, 어노테이션을 사용하여 구현하기!

- 어노테이션 설계
1. element는 2가지의 정보를 갖고 있어야 할 것이다.
    
    ① 구분선으로 사용될 문자의 정보를 담는 element
    
    ② 구분선의 문자가 몇 번 출력될 것인지를 정하는 element
    
2. 적용대상 / 유지 정책 설정

`@Target{(ElementType.METHOD)}`

`@Retention(RetentionPolicy.RUNTIME)`

```java
@Target{(ElementType.METHOD)}
@Retention(RetentionPolicy.RUNTIME)
pulic @interface PrintAnnotation {
    String value() default "-"; // 기본 elemet 생성-> 기본적으로 `-`가 구분선으로 생성되도록 선언
    int number() default 15; // 기본값으로 15개 구분선이 생성되도록 선언
}
```

- 어노테이션 적용 클래스 설계

: 실행 내용을 출력하는 메소드 3개 생성

: 실행 내용이외에도 구분선 출력하려면 어노테이션 적용

```java
public class Service {
    @PrintAnnotation // 아무 값도 선언 안했으므로
    // 어노테이션 엘리먼트는 모두 디폴트값으로 실행되어
    // 구분선은 "-", 개수는 15개로 실행될 것이다.
    public void method1() {
        System.out.println("실행 내용1");
    }

    @PrintAnnotation("*") // 어디에 저장될 지 선언을 안했으므로
    // "*"은 value 엘리먼트에 저장되어서
    // 구분선은 "*", 개수는 15개로 실행될 것이다.
    public void method2() {
        System.out.println("실행 내용2");
    }
    
    @PrintAnnotation(value="#", number=20) // value와 number 엘리먼트에 선언했으므로
    // "*"은 value 엘리먼트, 20은 number 엘리먼트에 저장되어서
    // 구분선은 "#", 개수는 20개로 실행될 것이다
    public void method3() {
        System.out.println("실행 내용3");
        }
}
```

- 실행 클래스 설계
1. 현재 어노테이션 적용 클래스의 메소드의 정보를 알아내야한다.

→ `getDeclaredMethods()` 사용하여 배열로 정보 얻어오기

1. 얻어온 배열 정보에서 이름 정보 추출 : method의 `getName()` 메소드 이용
2. 어노테이션이 적용되어 있는지를 검사해서 적용되었을 때 실행
    
    : `method.isAnnotationPresent(PrintAnnotation.class)` 이용
    
3. 어노테이션의 엘리먼트 값(구분선 종류, 개수) 추출 
    
    : method의 `getAnnotation(PrintAnnotation.class)` 이용
    
4. 메소드 호출 시 `invoke(메소드를 가지고 있는 객체, 매개변수가 있다면 매개변수)`
    
    : `method.invoke(new Service());` 이용
    
    → `Service service = new Service();` + `service.method1();`와 같은 기능이다!
    

```java
public class PrintAnnotationExample {

    import java.lang.reflect .*;

    public static void main(Stirng[] args) {
        // 메소드에 대한 정보를 Service.class.getDeclaredMethods();로 배열로 받아오고
        // 그 정보를 declaredMethods 변수에 저장하겠다!
        // Method[]는 리턴타입으로, 앞에 붙여줘야한다.
        // 이때, Method[]는 다른 패키지의 클래스이기때문에
        // import java.lang.reflect.*; 을 해줘야한다!
        Method[] declaredMethods = Service.class.getDeclaredMethods();

        // Service 클래스에서 작성된 메소드의 이름을 출력하는 코드 작성
        // 향상된 for문으로 작성
        for (Method method : declaredMethods) {
            // PrintAnnotation이 적용이 되어 있는지를
            // isAnnotationPresent를 통해서 검사
            if (method.isAnnotationPresent(PrintAnnotation.class)) {
                //getName() : 메소드의 이름을 반환하는 메소드
                System.out.println(method.getName());
                // 어노테이션의 엘리먼트 값을 추출하기 위해 method의 getAnnotation(PrintAnnotation.class) 이용
                // 받은 정보를 printAnnotation이라는 객체!에 저장
                PrintAnnotation printAnnotation = method.getAnnotation(PrintAnnotation.class);
                // 객체이므로 엘리먼트 값을 사용하고 싶을 때는
                // printAnnotation.value() / printAnnotation.number() 이런 식으로 사용

                // 메소드 이름 출력
                System.out.println("[" + method.getName() + "]");
                // 구분선 출력
                for (int i = 0; i < printAnnotation.number(); i++) {
                    System.out.print(printAnnotation.value());
                }
                System.out.println();

                // 메소드 호출 (예외처리는 덤..)
                try {
                    method.invoke(new Service());
                } catch (Exception e) {
                }
                System.out.println();
            }
        }
    }
}
```