# CHAPTER 9 : 중첩 클래스 / 중첩 인터페이스

## 9.1 중첩 클래스와 중첩 인터페이스란?

### 중첩클래스 / 중첩 인터페이스

- 중첩 클래스 : 클래스 멤버로 선언된 클래스

```java
class ClassName {
	// 중첩 클래스
	class NestedClassName {
		~~~
	}
}
```

- 중첩 인터페이스 : 클래스 멤버로 선언된 인터페이스

```java
class ClassName {
	interface NestedInterfaceName
}
```

- 중첩 클래스와 중첩 인터페이스의 용도
    - 해당 클래스에서만 사용하는 클래스와 인터페이스가 필요할 때 활용된다.
    - 중첩 인터페이스는 UI 컴포넌트 내부 이벤트 처리에 많이 활용된다.
    

## 9.2 중첩 클래스

### 중첩 클래스의 분류

![Untitled](CHAPTER%209%20%203dae6/Untitled.png)

1. 인스턴스 멤버 클래스 : 클래스 객체가 생성되어야 중첩 클래스를 사용할 수 있는 클래스
2. 정적 멤버 클래스 : 클래스 객체 생성 없이 중첩 클래스에 접근할 수 있는 클래스, `static` 사용
3.  로컬 클래스 : method()가 실행할 때 사용할 수 있는 B 중첩 클래스 

- 중첩 클래스의 바이트 코드 파일

`A$B.class` : 멤버 클래스 바이트 코드 파일/ `A$1B.class` : 로컬 클래스 바이트 코드 파일

A : 바깥 클래스 / B : 멤버, 로컬 클래스

### 인스턴스 멤버 클래스

```java
class A {
	// 인스턴스 멤버 클래스
	class B {
		B() {}                       // 생성자 O
		int field1;                  // 인스턴스 필드 O
		// static int field2;        // 정적 필드 X
		void method1() {}            // 인스턴스 메소드 O
    // static void method2() {}  // 정적 메소드 X
	}
}	
```

👉 바깥 클래스인 A의 객체가 생성되어야 B의 구성 멤버들을 사용할 수 있다.

```java
A a = new A();
A.B b = a.new B();
b.field1 = 3;
b.method1();
```

👉 바깥 클래스 A 객체 생성 후 B의 객체를 생성할 때는 타입에는 `A.` / 생성자에는 `a.` 을 사용하여 생성한다.

### 정적 멤버 클래스

```java
class A {
	// 정적 멤버 클래스
	static class C {
		C() {}                    // 생성자 O
		int field1;               // 인스턴스 필드 O
		static int field2;        // 정적 필드 O
		void method1() {}         // 인스턴스 메소드 O
    static void method2() {}  // 정적 메소드 O
	}
}	
```

👉 바깥 클래스인 A의 객체가 생성되지 않아도 C의 구성 멤버들을 사용할 수 있다.

```java
A.C c = new A.C();
c.field1 = 3;
c.method1();
A.C.field2 = 3;
A.C.method2();
```

👉 정적 필드, 메소드인 `field1` / `method1()` 은 정적 멤버 클래스 C의 객체를 만들지 않고 바로 접근이 가능하고, 인스턴스 필드, 메소드인 `field2` / `method2` 에는 C의 객체를 만들고 접근이 가능하다.

### 로컬 클래스

```java
class A {
	void method() {
		// 로컬 클래스
		class D {
		D() {}                       // 생성자 O
		int field1;                  // 인스턴스 필드 O
		 //static int field2;        // 정적 필드 X
		void method1() {}            // 인스턴스 메소드 O
	   //static void method2() {}  // 정적 메소드 X
		}
		D d = new D();	
		d.field1 = 1;
		d.method1();
	}
}
```

👉 method 안에서 생성된 로컬 클래스 D이므로 바깥 클래스 A를 호출하지 않고 객체 생성이 

`D d = new D();` 이렇게 가능하다.

※ 나중에 스레드 배울 때 메인 스레드와 다른 작업 스레드 코드를 로컬 클래스로 작성

```java
void method() {
	class DownloadThread extends Thread { ... }      // 로컬 클래스 생성
	DownloadThread thread = new DownloadThread();    // 객체 생성
	thread.start();  // 로컬 클래스의 객체를 이용한 메소드 호출
}
```

**※ 로컬 클래스는 선언된 메소드를 벗어나서 사용이 불가능하다.**

→ 위의 예제에서 로컬 클래스 D는 외부의 실행 클래스에서 사용할 수 없다.

→ 외부의 실행 클래스에서는 로컬 클래스 D를 선언한 메소드를 호출할 수 있을 뿐, 로컬 클래스 D의 내부 필드, 메소드를 호출할 수는 없다.

## 9.3 중첩 클래스의 접근 제한

### 바깥 필드와 메소드에서 사용 제한

```java
public class A {
	// 인스턴스 멤버 클래스
	class B {}

	// 정적 멤버 클래스
	static class C {}
}
```

A 클래스의 중첩 클래스로 B, C가 선언되었다.

```java
public class A {
	// 인스턴스 필드
	B field1 = new B();
	C field2 = new C();

	// 인스턴스 메소드
	void method1() {
		B var1 = new B();
		C var2 = new C();
	}
	
	// 정적 필드 초기화
	// static B field3 = new B();  : 선언 불가능  
	static C field4 = new C();

	// 정적 메소드
	static void method2() {
		// B var1 = new B();         : 선언 불가능
		C var2 = new C();
	}
}
```

👉 인스턴스 필드와 인스턴스 메소드는 인스턴스 멤버 클래스인 B, 정적 멤버 클래스인 C 상관없이 둘다 사용이 가능하지만,

👉 정적 필드와 정적 메소드는 정적 멤버 클래스로 선언이 된 C에서만 사용이 가능하다!

왜냐하면, A의 인스턴스 멤버 클래스인 B는 A 객체가 있어야만 B 객체를 만들 수가 있다.

`static B field3 = new B();` 에서 B타입 변수 field3이 static, 정적 변수로 선언되었으므로 field3은 나중에 호출될 때 A의 객체를 생성하지 않고도 호출이 가능해야한다.

하지만, new B();로 B의 객체를 만들어서 저장하면, 결국 field3이 호출될 때 B의 객체를 호출하게 되는데, 클래스 B는 인스턴스 클래스라서 A 객체가 있어야만 호출이 가능하므로 성립이 되지 않는다. 

**어려우니... 그냥 중첩 인스턴스 멤버 클래스(B)로는 정적 필드, 정적 메소드를 초기화할 수 없다고 생각하자..**

### 멤버 클래스에서의 사용 제한

: 바깥 클래스에 선언된 필드, 메소드를 중첩 클래스 내부에서 어떻게 접근해서 사용하는지?

![Untitled](CHAPTER%209%20%203dae6/Untitled%201.png)

`B` 는 인스턴스 멤버 클래스 / `C` 는 정적 멤버 클래스

B는 바깥 클래스 A에 선언된 필드, 메소드를 이름만으로 접근해서 전부 사용할 수 있다.

C는 바깥 클래스 A에 선언된 필드, 메소드 중 이름만으로 접근하여 정적 필드, 정적 메소드만 사용할 수 있다.

### 로컬 클래스에서의 사용 제한

```java
public class A {
    public void method2(int arg) {
        int localVariable = 1;
        // arg = 100; (X)
        // localVariable = 100; (X)

        class Inner {
						// final int arg = 매개변수로 받은 값;
						// final int localVariable = 1;

            public void method() {
                int result = arg + localVariable;
            }
        }
    }
}
```

👉 기본적으로 로컬 클래스를 호출하는 메소드의 매개변수와 로컬 변수는 `final` 이 생략되어서 일반 변수처럼 보이지만 `final` 이 붙어서 수정할 수 없는 `final 변수`이다.

따라서, 메소드 안에서 값을 새로 줘서 수정할 수 없다.

또한, 로컬 클래스를 호출하는 메소드의 매개변수와 로컬 변수는 **로컬 클래스의 필드**로 복사된다!

`// final int arg = 매개변수로 받은 값;` / `// final int localVariable = 1;` 이 두개의 선언을

컴파일러가 자동으로 컴파일 상에서 선언해줘서 로컬 클래스의 메소드인 `method` 에서 로컬 클래스를 호출하는 메소드인 `method2` 의 매개변수, 로컬 변수를 사용할 수 있게 해준다!

### 중첩 클래스에서 바깥 클래스의 참조 얻기 (안드로이드에서 자주 나온다)

- `바깥클래스명.this` 를 사용하여 참조를 할 수 있다!

![Untitled](CHAPTER%209%20%203dae6/Untitled%202.png)

👉 중첩 클래스 `Nested` 의 메소드 `print` 에서 `this.field` 와 `this.method()` 는 자기 자신의 필드와 메소드, 즉 Nested.filed, Nested.method()를 가리켜서 `Nested-field` / `Nested-method` 가 출력된다.

👉 중첩 클래스 `Nested` 의 메소드 `print` 에서 `Outter.this.field` , `Outter.this.method()`는 바깥 클래스의 필드와 메소드, 즉 Outter.field와 Outter.method()를 가리켜서 `Outter-field` 가 출력된다.

## 9.4 중첩 인터페이스

### 중첩 인터페이스 선언

- 중첩 인터페이스 : 클래스 안에 선언된 인터페이스
- 중첩 인터페이스 사용 이유
    - 해당 인터페이스가 바깥 클래스를 벗어나서는 사용하지 않는, 즉 바깥 클래스하고만 관계를 가지는 인터페이스일 때 중첩 인터페이스로 사용한다.

```java
public class Button {
    // 중첩 인터페이스
    interface OnClickListener {
        void onClick();
    }

    // 인터페이스 타입 필드
    OnClickListener listener;

    // Setter 메소드 선언 : 매개변수의 다형성
    void setOnClickListener(OnClickListener listener) {
        this.listener = listener;
    }

    // 구현 객체의 onClick() 메소드 호출
    void touch() {
        listener.onClick();
    }
}
```

👉 중첩 인터페이스 `OnClickListener` 를 선언하고, 인터페이스 타입 필드로 `OnClickListener listener` 을 선언한 후 매개변수로 인터페이스 구현 객체를 받아서 `listener` 에 저장한다.

버튼을 클릭(touch)할 때, 버튼을 클릭한 객체가 매개변수에 오고, 결국 그 객체가 버튼의 클릭 이벤트를 처리한다.

이벤트 처리 순서 : 외부에서 setter인 `setOnClickListener(인터페이스 구현 객체)`를 이용하여 listener를 구현하는 객체를 저장 후 → 버튼이 터치되면 `touch()` 메소드 호출 → 인터페이스 타입 필드인 `listener` , 즉 저장된 객체의 `onClick()`을 호출하여 이벤트 처리

**※ 중첩 인터페이스의 구현 클래스를 작성할 때는 implements 다음에 바로 중첩 인터페이스의 이름이 아니라 `바깥클래스.중첩인터페이스이름` 이렇게 선언해야한다.**

`public class CallListener implements Button.OnclickListener { ... }`

## 9.5 익명 객체

### 익명 객체 : 이름이 없는 객체

→ 자식 클래스나 구현 클래스를 따로 파일을 만들어서 선언하지 않고 객체 생성 시 클래스 내용을 선언하는 객체이다.

- 익명 객체는 단독으로 생성할 수 없다!
    - **클래스를 상속하거나 인터페이스를 구현해야만 생성할 수 있다.**
    
- 사용 위치
    - 필드의 초기값, 로컬 변수의 초기값, 매개변수의 매개값으로 주로 사용
    - UI 이벤트 처리 객체나, 스레드 객체를 간편하게 생성할 목적으로 주로 활용

### 익명 자식 객체 생성

- 필드 초기 값

```java
class A {
    // 익명 객체 생성
    Parent field = new Parent() {
        // Parent를 상속받는 자식 클래스의 내용 생성
        int childField;
        void childMethod() {
            // Parent의 메소드를 오버라이딩
            @Override
            void parentMethod () { }
        }
    };
}
```

👉 익명 자식 객체를 Parent 타입의 field에 대입할 수 있다.

- 로컬 변수 초기 값

```java
class A {
    void method() {
        // 로컬 변수 선언 시 익명 자식 객체로 로컬 변수에 대입
        Parent localVar = new Parent() {
						// 자식 클래스의 내용 생성
            int childField;
            void childMethod() { }
						// Parent의 메소드를 오버라이딩
            @Override
            void parentMethod() { }
        };
    }
}
```

👉 익명 자식 객체를 Parent 타입의 로컬 변수 localVar에 대입할 수 있다.

- 메소드의 매개값

```java
class A {
    void method1(Parent parent) { }
    
    void method2() {
        // method1()의 매개 값으로 익명 하위 클래스를 이용해서 객체로 대입
        method1(
          new Parent() {
              int childField;
              void childMethod() { }
              @Override
              void parentMethod() { }
          }
        );
    }
}
```

👉 익명 하위 클래스(자식 클래스)의 내용을 생성해서를 Parent 타입의 매개 변수 parent에 대입할 수 있다.

- 익명 객체에 새롭개 정의된 필드와 메소드
    - 익명 객체 내부에서만 사용된다.
    - 외부에서는 익명 객체의 필드와 메소드에 접근할 수 없다.
    
    → 왜냐하면, 익명 객체는 부모 타입 변수에 대입되는 것이므로 부모 타입에 선언된 것만 사용할 수 있다! 
    
    (익명 객체는 하위 클래스를 생성하는 것이므로 익명 객체의 필드와 메소드를 사용할 수 없다.)
    
    ```java
    class A {
        Parent field = new Parent() {
            int childField;
            void childMethod() { }
            @Override
            void parentMethod() {
                childField = 3;
                childMethod();
            }        
        };
        
        void method() {
            // field.childField;  : 하위 클래스의 내용이므로 Parent 타입의 field에서 접근 불가능
            // field.childMethod();   : 하위 클래스의 내용이므로 Parent 타입의 field에서 접근 불가능
            field.parentMethod();  // 부모의 메소드를 재정의한 것이므로 접근 가능
        }
    }
    ```
    

### 익명 구현 객체 생성

- 필드 초기값

```java
class A {
    // class A의 필드 선언
    // RemoteControl 인터페이스의 구현 객체 익명 객체로 생성
    RemoteControl field = new RemoteControl() {
        // RemoteControl 인터페이스의 추상 메소드에 대한 실체 메소드 재정의        
        @Override
        void turnOn() { }
    };
}
```

- 로컬 변수 초기값

```java
class A {
    void method() {
        // 메소드의 로컬 변수로 익명 구현 객체를 생성해서 선언
        RemoteControl localVar = new RemoteControl() {
            @Override
            void turnOn() { }
        };
    }
}
```

- 메소드의 매개변수 값

```java
class A {
    void method1(RemoteControl rc) { }

    void method2() {
        method1 (
            // method1의 매개값으로 익명 구현 클래스를 생성해서 객체로 대입
           new RemoteControl() {
                @Override
                void turnOn() { }
            }
        );
    }
}
```

## 9장 확인 문제

1. **인스턴스 멤버 중첩 클래스 / 정적 멤버 중첩 클래스에서 객체 생성하는 방법**

```java
public class Car {
	class Tire { }
	static class Engine { }
}
```

👉 인스턴스 멤버 중첩 클래스 : `Tire` / 정적 멤버 중첩 클래스 : `Engine`

```java
public class NestedClassExample {
	public static void main(String[] args) {
		
		Car myCar = new Car();

		Car.Tire tire = myCar.new Tire();
		
		Car.Engine engine = new Car.Engine();
		
	}
}
```

1. 먼저 변수의 클래스 타입은 원래 클래스명인데 중첩 클래스는 중첩 클래스를 참조하기 위해서는 바깥 클래스부터 참조해서 들어가야 하므로 `.` 으로 바깥 클래스부터 참조
    
    : `Car.Tire` / `Car.Engine`
    
2. Tire는 인스턴스 멤버 중첩 클래스이므로 객체를 생성하기 위해서는 바깥 클래스의 객체가 반드시 필요하다. 따라서, 바깥 클래스의 객체를 저장한 변수인 myCar로 바깥 클래스를 참조한 후 `.` 을 통해 Tire 객체를 만든다.
    
    : `myCar.new Tire();`
    
3. Engine은 정적 멤버 중첩 클래스이므로 객체를 생성하기 위해서 다른 객체를 참조할 필요가 없다. 따라서, 그 위치로 가서 생성하면 되므로 바깥 클래스의 객체를 저장한 myCar 변수가 아닌 바깥 클래스 자체인 Car에서 `.` 을 통해 Engine을 참조하여 객체를 생성한다.
    
    : `new Car.Engine();`