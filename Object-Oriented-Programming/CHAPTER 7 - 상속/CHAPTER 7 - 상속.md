# CHAPTER 7 : 상속

## 7.1 상속 개념

### 상속(Inheritance)이란?

- 자식(하위, 파생) 클래스가 부모(상위) 클래스의 멤버를 물려받는 것
- 자식이 부모를 선택해서 물려받는다
- 상속 대상 : 부모의 필드와 메소드

![Untitled](CHAPTER%207%20%20e73e2/Untitled.png)

상속 화살표 : UML 표기법

화살표 출발하는 쪽 : 자식 클래스 / 화살표 들어가는 쪽 : 부모 클래스

### 상속의 효과

- 부모 클래스를 재사용해서 자식 클래스를 빨리 개발할 수 있다.
- 반복된 코드의 중복을 줄여준다
- 유지 보수 편리
- 객체의 다형성을 구현할 수 있다!

### 상속 대상의 제한

- 부모 클래스의 private 접근을 갖는 필드와 메소드는 제외된다.
- 부모 클래스가 다른 패키지에 있을 경우 default 접근을 갖는 필드와 메소드도 제외된다.

## 7.2 클래스 상속(extends)

### extends 키워드

: 자식 클래스가 상속할 부모 클래스를 지정하는 키워드

```java
public class A {
	int field1;
	void method1() { ... }
}
```

```java
// A 클래스를 부모 클래스로 지정한다!, 상속받는다  (extends A)
public class B extends A {
	String field2;
	void method2() { ... }
}
```

: 자바는 다중 상속을 허용하지 않으므로 부모 클래스는 단 하나만 상속할 수 있다.

## 7.3 부모 생성자 호출 (super)

### 자식 객체를 생성하면 부모  객체도 생성되는가? → YES!

: 부모 없는 자식은 없다!

- 자식 객체를 생성할 때는 부모 객체부터 생성되고 자식 객체가 생성된다.
- 부모 생성자가 호출이 먼저 된 후에 자식 생성자가 나중에 호출된다.
- 자식 생성자를 만들지 않고 자식 객체를 호출하면, 컴파일러가 자식 기본 생성자를 만들고 부모의 기본 생성자를 호출해서 부모의 객체가 먼저 만들어진 후에 자식의 객체가 만들어진다!

명시적으로 부모 생성자 호출

: 부모 객체를 생성할 때, 부모 생성자를 선택해서 호출할 수 있다!

```java
자식 클래스(매개변수선언, ...) {
	super(매개값, ...) // 부모 생성자 호출하는 방법
	...
}
```

- `super(매개값, ...)`는 매개값과 동일한 타입, 개수, 순서가 맞는 부모 생성자를 호출한다.
- 만약 해당하는 부모 생성자가 없다면 컴파일 오류가 발생한다
- **반드시 자식 생성자의 첫 줄에 위치해야한다!!**
- **부모 클래스에 기본 생성자가 없다면 필수적으로 부모 클래스에서 기본 생성자를 작성해야한다.**

## 7.4 메소드 재정의(Override)

### 메소드 재정의(Override)

: 부모 클래스의 상속 메소드를 수정해서 자식 클래스에서 재정의하는 것!

: 메소드 재정의 조건

- 부모 클래스의 메소드 선언부와 동일해야한다.
- 접근 제한을 더 강하게 오버리딩할 수 없다.

→ 부모가 public이라면 자식은 default나 private으로 수정할 수 없다.

→ 반대로, 부모가 default라면 자식은 public으로 수정이 가능하다.

→ 접근 제한이 부모 → 자식일 때 확장되는 범위로 가면 가능하다.

- 새로운 예외(Exception)을 throws 할 수 없다.

### @Override 어노테이션

: 컴파일러에게 부모 클래스의 메소드와 선언부가 동일한지 검사하도록 지시

→ 개발자의 실수로 부모 클래스의 메소드 이름에서 1글자 틀렸다든지, 매개 변수를 빼먹었다든지, 리턴타입을 잘못 넣었다든지 등등 사소한 실수를 저지르면 자식 클래스에서는 이 메소드를 상속받은 메소드를 재정의(Override)하는 것이 아니라, 새로운 메소드를 생성하는 것으로 취급한다.

따라서 이를 방지하기 위해 `@Override` 로 어노테이션을 지정해서 컴파일러에게 검사하도록 지시한다.

따라서, 정확한 메소드 재정의(Override)를 위해 `@Override` 를 붙여주는 것이 좋다!!

### 메소드 재정의(Override) 효과

: 부모 메소드는 숨겨지는 효과 발생

- 재정의된 자식 메소드가 실행된다.

### 부모 메소드 사용(super)

: 메소드 재정의(Override)는 부모 메소드를 숨기는 효과

- 자식 클래스에서는 재정의된 메소드만  호출된다.

: 자식 클래스에서 수정되기 전(Override 전)의 부모 메소드를 호출하고자 할 경우 super 사용

: `super()` 와 여기 `super` 는 다르다!

- `super()` : 부모의 생성자를 호출하는 것
- `super` : 부모 객체를 참조하는 것 (자기 자신의 객체를 참조하는 this와 비슷)

※ `super` 는 자식 클래스 안에서만 사용된다!!

`super.부모메소드();` 

ex) 자식 클래스에서 부모의 `method1` 에 몇 줄의 코드를 더 추가하고 싶어서 Overring할 때

→ `super.method1();` 으로 부모의 원래 메소드를 호출한 후 그 뒤에 코드를 추가한다.

```java
public class Parent {
    void method1() { ... }
}

public class Child extends Parent {
    void method1() {
        super.method1();
        ~~~
    }
}
```

## 7.5 final 클래스와 final 메소드

### final 키워드의 용도

: final 필드 / final 클래스 / final 메소드

`final 필드` : 수정할 수 없는 필드

`final 클래스` : 부모로 사용할 수 없는 클래스

`final 메소드` : 자식이 재정의(Override)할 수 없는 메소드

→ final 클래스, final 메소드는 상속과 관련있다!

### 상속할 수 없는 final 클래스

: 자식 클래스를 만들지 못하도록 final 클래스로 만듦

`public final class 클래스 { ... }`

→ 대표적으로 String 클래스가 있다.

→ JDK에서 표준적으로 제공하는 String 클래스는 부모로 사용할 수 없다.

### 오버라이딩할 수 없는 final 메소드

: 자식 클래스가 재정의 못하도록 부모 클래스의 메소드를 final로 만듦

`public final 리턴타입 메소드(매개변수, ...) { ... }`

## 7.6 protected 접근 제한자

### protected 접근 제한자

: 상속과 관련된 접근 제한자

- 같은 패키지 : 모두 접근 가능
- 다른 패키지 : 자식 클래스만 접근을 허용

![Untitled](CHAPTER%207%20%20e73e2/Untitled%201.png)

## 7.7 타입변환과 다형성(polymorphism)

### 다형성(polymorphism)

: 같은 타입이지만 실행 결과가 다양한 객체를 대입(이용)할 수 있는 성질

- 부모 타입에는 모든 자식 객체가 대입될 수 있다.

→ 자식 타입은 부모 타입으로 자동 타입 변환된다.

- 효과 : 객체를 부품화 시킬 수 있다.

ex) A가 부모 클래스이고, B와 C가 자식 클래스일 때

`A a = new B();` / `A a = new C();` 이렇게 B, C 객체를 A를 통해서 만들 수 있다!!

이렇게 같은 타입(A)으로 다른 객체(B, C)를 생성할 수 있다.

### 자동 타입 변환(Promotion)

: 프로그램 실행 도중에 자동적으로 타입 변환이 일어나는 것

: 자동 타입 변환을 하는 이유 → 다형성을 구현하기 위해!

 ex) `Cat` 클래스(자식)가 `Animal` 클래스(부모)를 상속 받을 때!

```java
// Animal animal = new Cat();
Cat cat = new Cat();
Animal animal = cat; 

cat == animal // true이다!!!
```

![Untitled](CHAPTER%207%20%20e73e2/Untitled%202.png)

`cat` 과 `animal` 이 타입은 각각 `Cat` 과 `Animal` 로 다르지만, 같은 객체 `Cat` 을 참조하고 있다.

이때, `Cat` 타입, 즉 자식 클래스 타입은 부모 클래스 타입(`Animal`)으로 자동 변환된다.

`부모클래스 변수 = 자식클래스타입;` : 자식클래스타입이 부모 클래스 타입으로 자동 변환된다.

 

: 상속 계층에서 어떤 상위 타입이라도 자동 타입 변환이 일어날 수 있다.

![Untitled](CHAPTER%207%20%20e73e2/Untitled%203.png)

`B` , `C`, `D`, `E` 모두 A의 상속을 받으므로 A에 a, b, c, d가 대입될 수 있다.

하지만, 상속 관계가 아닌 B와 E / C와 D는 서로 대입될 수 없다.

: 자동 타입 변환 이후의 효과

- 부모 클래스에 선언된 필드와 메소드만 접근이 가능하다!
- 메소드가 재정의(Overriding)되었다면 자식 클래스의 재정의된 메소드가 호출된다!

```java
public class Parent {
    void method1() { ... }
    void method2() { ... }
}

public class Child extends Parent {
    void method2() { ... } // Overriding
    void method3() { ... }
}

class ChildExample {
    public static void main(String[] args) {
        Child child = new Child();
        
        Parent parent = child; // child는 parent로 자동 타입 변환
        parent.method1();
        parent.method2(); // Overriding된 child의 method2() 실행
        parent.method3(); // 호출 불가
    }
    
}
```

### 필드의 다형성

: 다형성을 구현하는 기술적 방법

- 부모 타입으로 자동 변환
- 재정의된 메소드(Overriding)

![Untitled](CHAPTER%207%20%20e73e2/Untitled%204.png)

1. 자식 클래스인 HankookTire, KumhoTire는 부모 클래스인 Tire의 상속을 받고 있다.
2. Car 클래스에서 4가지 방향의 Tire 객체가 선언되었다.
3. HankookTire / KumhoTire에서 Tire의 roll() 메소드가 재정의(Overriding)되었다.
4. frontRightTire / backLeftTire는 원래 Tire, 부모 클래스로 생성이 되었는데 자식 클래스인 HankookTire / KumhoTire로 생성을 다시 했다.
5. 이때, 부모 클래스로 자동 변환이 되기 때문에 바뀌는 게 없어보이지만 roll() 메소드를 실행시키는 run() 메소드를 실행하면 HankookTire와 KumhoTire로 생성한 frontRightTire, backLeftTire는 부모 클래스인 Tire의 roll() 메소드가 아니라 재정의된 HankookTire와 KumhoTire의 roll() 메소드를 실행시킨다.

### 하나의 배열로 객체 관리

```java
class Car {
    Tire frontLeftTire = new Tire("앞왼쪽", 6);
    Tire frontRightTire = new Tire("앞오른쪽", 2);
    Tire backLeftTire = new Tire("뒤왼쪽", 3);
    Tire backRightTire = new Tire("뒤오른쪽", 4);
}
```

👉 변수를 4개나 지정해서 복잡하다.

👉 이를 배열을 사용하면 더 간단하게 관리할 수 있다.

```java
class Car {
    Tire[] tires = {
        new Tire("앞왼쪽", 6);
        new Tire("앞오른쪽", 2);
        new Tire("뒤왼쪽", 3);
        new Tire("뒤오른쪽", 4);
    }
}
```

### 매개변수의 다형성

: 매개변수가 클래스 타입일 경우

- 해당 클래스의 객체를 대입하는 것이 원칙이지만 자식 객체를 대입하는 것도 허용된다.

→ 자식 객체를 대입하게 되면 자동 타입 변환이 일어나고, 다형성 구현이 가능하다.

```java
class Driver {
	void drive(Vehicle vehicle) {
		vehicle.run();
	}
}
```

👉 Driver 클래스의 drive 메소드는 Vehicle 클래스의 객체를 매개변수로 받는다.

Bus 클래스가 Vehicle 클래스의 자식 클래스 일때,

```java
Driver driver = new Driver();

Bus bus = new Bus();

driver.drive(bus);
```

👉 이렇게 실행하게 되면 Bus 클래스의 객체 bus가 Vehicle 클래스의 객체를 받는 매개변수에 들어가면서 자동으로 부모 클래스인 Vehicle의 객체로 타입 변환을 하게 된다.

하지만, Bus 클래스에서 run 메소드를 재정의했다면 run 메소드는 재정의한 Bus 클래스의 run 메소드가 실행된다!

### 강제 타입 변환(Casting)

: 부모 타입을 자식 타입으로 변환하는 것을 말한다.

`자식클래스 변수 = (자식클래스) 부모클래스타입;` 

: 강제 타입 변환을 할 수 있는 조건

- 대응되는 자식 타입이 부모 타입으로 변환된 상태에서 다시 자식타입으로 변환할 때 유효

```java
// A가 부모클래스, B,C가 자식클래스 일때

A a = new B();
B b = (B) a; // 강제 타입 변환 가능

A a = new A();
B b = (B) a; // 강제 타입 변환 불가능 
// a에는 애초에 부모타입이 선언됐으므로 불가능하다.

A a = new C();
B b = (B) a; // 강제 타입 변환 불가능
// C의 타입이 변환된 것이지, B가 변환된 것이 아니기 때문에 불가능하다.
```

: 강제 타입 변환을 사용하는 목적

- 자식 타입이 부모 타입으로 자동 변환된다면, 부모 타입에 선언된 필드와 메소드만 사용 가능하다. 즉, 자식 타입의 필드와 메소드는 사용을 할 수가 없어진다.
- 이때, 자식 타입에 선언된 필드와 메소드를 다시 사용해야 할 때, 강제 타입 변환을 사용하여 부모 타입으로 자동 변환되었던 자식클래스 객체를 자식 타입으로 강제 타입 변환해서 자식클래스의 필드와 메소드를 사용한다.

![Untitled](CHAPTER%207%20%20e73e2/Untitled%205.png)

### 객체 타입 확인(instanceof) : 강제 타입 변환 시 주의할 점

: 부모 타입이면 모두 자식 타입으로 강제 타입 변환할 수 있는 것이 아니다.

: 현재 자식 타입이 부모 타입으로 변환된 상태인지 확인하는 것이 필요하다!

→ `instanceof` 사용

```java
public void method(Parent parent) {
	Child child = (Child) parent;
}
```

 👉 현재 이렇게만 메소드가 선언되면, 매개변수로 부모 클래스의 객체인 parent가 올 수도 있고, 자식 클래스의 객체인 child가 올 수도 있다.

child가 오게 되면 자동 변환이 되기 때문에 강제 타입 변환을 사용할 수 있지만, 부모 객체가 오게되면 자동 변환이 안 되기 때문에 강제 타입 변환을 할 수 없다.

따라서, **강제 타입 변환 전에 매개변수로 들어온 값이 자식 객체인지 `instanceof`로 확인해야한다!**

**좌항(객체) `instanceof` 우항(타입) : 좌항의 객체가 우항의 타입으로 만들어졌는지 판단**

→ 맞으면 true / 틀리면 false 반환

```java
public void method(Parent parent) { 
	// 매개변수로 들어온 parent가 Child 객체를 참조하는지 조사 후 맞다면 true 반환
	if(parent instanceof Child) {
		Child child = (Child) parent;
	}
}
```

## 7.8 추상 클래스(Abstract Class)

### 추상 클래스 개념

- 실체 클래스들의 공통되는 필드와 메소드를 정의한 클래스
- 추상 클래스는 실체 클래스의 부모 클래스 역할을 할 수 있다.

→ 실체 클래스들은 추상 클래스의 상속을 받을 수 있다.

- 추상 클래스는 단독으로 객체 생성을 할 수 없고, 부모 클래스로만 사용된다. 따라서, 객체 생성은 실체 클래스, 즉 자식 클래스로 해야한다.

### 추상 클래스의 용도

1. 실체 클래스의 공통된 필드와 메소드의 이름을 통일할 목적
- 실체 클래스를 설계하는 사람이 여러 명일 경우, 실체 클래스마다 필드와 메소드의 이름이 다를 수 있다.

1. 실체 클래스를 작성할 때 시간을 절약
- 공통된 필드, 메소드는 부모 클래스인 추상 클래스에 있기 때문에 실체 클래스는 추가적인 필드와 메소드만 선언하면 된다.

1. 실체 클래스 설계 규격을 만들고자 할 때
- 실체 클래스가 가져야할 필드와 메소드를 추상 클래스에 미리 정의해놓고 실체 클래스는 추상 클래스를 무조건 상속받아서 작성하도록 한다.

### 추상 클래스 선언

`abstract` 사용!

`public abstract class 클래스명 { ... }`

### 추상 메소드와 Overriding(재정의)

: 메소드 이름은 동일하나, 실행 내용이 실체 클래스마다 다른 메소드가 있을 수 있다.

ex) 동물은 소리를 내지만, 실체 동물들의 소리는 동물마다 다르다

: 구현 방법

- 추상 클래스에서는 메소드의 선언부만 작성한다. (추상 메소드) `abstract sound();`
- 실체 클래스에서 메소드의 실행 내용을 재정의한다. (Overriding)

이때, 추상 클래스에서 추상 메소드를 선언만 하고, 실체 클래스에서 메소드를 Overriding 하지 않을 경우 컴파일 에러가 뜬다!