# CHAPTER 11 : 기본 API 클래스

## 11.1 자바 API Document

### 자바 API

- 자바에서 기본적으로 제공하는 라이브러리
- 프로그램 개발에 자주 사용되는 클래스 & 인터페이스의 모음

### API Document

- 쉽게 API를 찾아서 이용할 수 있도록 문서화한 것 (공식문서)
- HTML 페이지로 작성되어 있어서 웹브라우저로 바로 볼 수 있다.

[Overview (Java SE 17 & JDK 17)](https://docs.oracle.com/en/java/javase/17/docs/api/index.html)

![Untitled](CHAPTER%2011%2083867/Untitled.png)

- NESTED : 중첩된 내부 클래스
- FIELD : 클래스가 가지고 있는 필드
- CONSTR : 클래스가 가지고 있는 생성자
- METHOD : 클래스가 가지고 있는 생성자

## 11.2 java.lang과 java.util 패키지

### java.lang 패키지

- 자바 프로그램의 기본적인 클래스를 담고 있는 패키지
- 포함된 클래스와 인터페이스는 import 없이 사용할 수 있다.
- 주요 클래스 종류

![Untitled](CHAPTER%2011%2083867/Untitled%201.png)

### java.util 패키지

- 자바 프로그램 개발에 조미료 역할..?
- 대부분 컬렉션 클래스 → 15장에서 학습
- 컬렉션 클래스를 제외한 6가지 주요 클래스

![Untitled](CHAPTER%2011%2083867/Untitled%202.png)

## 11.3 Object 클래스

### Object 클래스 - 자바의 최상위 부모 클래스

- 다른 클래스를 상속하지 않으면 암시적으로 java.lang.Object 클래스를 상속한다.
- Object의 메소드는 모든 클래스에서 사용할 수 있다.

### 객체 비교 equals()

`public boolean equals(Object obj) { ... }`

- 기본적으로 Objcet의 equals()는== 연산자와 동일한 결과를 리턴 (메모리 주소(번지) 비교)

```java
Object obj1 = new Object();
Object obj2 = new Object();

boolean result = obj1.equals(obj2);

boolean result = (obj1 == obj2);
```

👉 두 result는 결과가 `False` 로 같다.

- **논리적 동등**을 위해 오버라이딩이 필요하다.
    - 논리적 동등 : 객체가 같든 다르든 상관없이 객체가 저장하고 있는 데이터가 동일하면
        
        동등한 것으로 보겠다.
        
        → 번지 수가 다르지만, 그 안의 내용이 같으면 동등한 것으로 보겠다.
        
    - Object의 equals() 메소드는 논리적 동등을 하지 않는다.
    
    → 논리적 동등을 하기 위해서는 자식 클래스에서 equals를 재정의하여 논리적 동등을 처리해야한다.
    
    ex) `String` 클래스는 equals를 재정의해서 객체의 번지 수가 달라도 안의 문자열만 같으면 `True` 를 반환하도록 재정의한다.
    
- 보통 `equals()` 메소드는 Object의 자식 클래스에서 재정의해서 그 안의 내용을 비교하기 위한 메소드로 많이 사용된다.

※ 보통 equals()는 안의 문자열이 같은지를 비교할 때 사용했는데 사용 원리

```java
public class Member {
	public String id;
}
```

이렇게 String 타입으로 id라는 필드를 선언하고 id의 문자열을 저장 후 id와 다른 객체의 문자열이 같은지를 비교하기 위해 `id.equals(~~)` 를 하는 순간,

id는 String 타입이기 때문에 equals는 String.equals()와 같아진다.

String.equals는 Objcect.equals를 재정의하여 논리적 동등이면 True를 반환하기 때문에 문자열 비교를 할 수 있다.

### 객체 해시코드 (hashCode())

- 객체의 해시코드란?
    - 객체를 식별할 하나의 정수값을 말한다.
    - Object의 hashCode() 메소드는 객체의 메모리 번지를 이용해서 해시코드를 만들어서 리턴한다. 따라서, 객체가 다르면 해시코드가 모두 다르다.
    
- 논리적 동등을 비교할 때 hashCode() 메소드를 오버라이딩 해야한다!
    - 컬렉션 프레임워크의 HashSet, HashMap, Hashtable과 같은 클래스는 두 객체가 동등한
    
     객체인지 판단할 때 equals()의 리턴값도 판단한다!
    
    원래 재정의를 하지 않으면 : 객체의 값이 같아도 메모리 주소가 다르면 다른 객체로 봤다.
    
    하지만, 컬렉션 프레임 워크에서는 메모리 주소가 달라도 객체의 값이 같을 때 동등 객체로 판단해야한다.
    
    ![Untitled](CHAPTER%2011%2083867/Untitled%203.png)
    
    컬렉션 프레임워크는 다음과 같은 처리 과정을 거치는데, 메모리 주소가 달라도 객체의 값이 같을 때 동등 객체로 판단하기 위해서는 hashCode()를 메모리 주소가 달라도 값이 같으면 True를 리턴하도록 하면 된다.
    
    👉 따라서, 객체 생성 시 생성자의 매개 값으로 받은 값을 hashCode()의 값으로 리턴하게 되면, 메모리 주소가 달라도 값이 같을 때 hashCode()의 리턴 값이 같게 되므로 이렇게 재정의 한다!
    
    ```java
    // 객체 생성자의 매개 값이 number일 때
    @Override
    public int hashCode() {
    	return number;
    }
    ```
    
    👉 이렇게 되면 객체 주소가 달라도 값이 같으면 hashCode()에서 true를 반환해서 동등 객체로 인식한다!
    

★ 완전히 논리적으로 동등한 객체를 만들기 위해서는 hashCode()와 equals()를 재정의해야한다! ★

### 객체 문자 정보(toString())

- 객체의 문자 정보란 객체를 문자열로 표현한 값을 말한다.
- Object 클래스의 toString() 메소드는 객체의 문자 정보를 리턴한다.
    - Object 클래스의 toString() 메소드는 `클래스명@해시코드` 로 구성된 문자 정보를 리턴
    
    ```java
    Object obj = new Object();
    System.out.println(obj.toString());
    ```
    
    👉 `java.lang.Object@de6ced` 이렇게 클래스명과 해시코드가 출력된다.
    
- default의 해시코드는 쓸모없는 정보이므로 일반적으로 재정의해서 의미있는 문자 정보로 만든다.
    - Date 클래스는 toString() 메소드를 재정의해서 현재 시스템의 날짜, 시간 정보 리턴
    - String 클래스는 toString() 메소드를 재정의해서 저장하고 있는 문자열 리턴
    
- System.out.println(Object) 출력 메소드는 Object의 toString()의 리턴값을 출력한다!

→ 이렇게 객체의 문자열을 출력할 수 있는 것이다!

```java
public class SmartPhone {
	private String company;
	private String os;

	public SmartPhone(String company, String os) {
		this.company = company;
		this.os = os;
	}

	// 스마트폰이 가지고 있는 회사명, os 정보를 toString으로 리턴
	@override
	public String toString() {
		return company + "," + os;
	}

}

// 실행 클래스
public class SmartPhoneExample {
    public static void main(String[] args) {
        // 스마트폰 객체 생성 - 생성 시 company, os 정보 생성자로 제공
        SmartPhone myPhone = new SmartPhone("구글", "안드로이드");
        // strObj 변수에 myPhone의 toString으로 받은 정보 저장
        String strObj = myPhone.toString();
        System.out.println(strObj);

				// 변수에 저장하지 않고 바로 객체 타입 변수를 println해도 
        // 자동으로 toString()을 호출하므로 똑같은 결과가 출력된다.
        System.out.println(myPhone);
    }
}
```

👉 출력값으로 우리가 재정의한 폰의 회사명, os값이 나온다!

### 객체 복제 (Clone)

- 원본 객체의 필드값과 동일한 값을 가지는 새로운 객체를 생성하는 것
- 복제 종류
    - 얕은 복제(thin clone) : 단순히 필드 값만 복제(참조 타입 필드는 번지 공유)
    
    ![Untitled](CHAPTER%2011%2083867/Untitled%204.png)
    
    → 배열 객체 하나를 원본 객체, 복제 객체에서 참조
    
    - 깊은 복제(deep clone) : 참조하고 있는 객체도 복제
    
    ![Untitled](CHAPTER%2011%2083867/Untitled%205.png)
    
    → 완전히 똑같은 구조의 객체가 하나 더 만들어진다.
    

- Object의 clone() 메소드는 동일한 필드값을 가진 얕은 복제된 객체를 리턴
    - clone()메소드를 사용하여 복제하려면 원본 객체가 `java.lang.Cloneable` 인터페이스를 구현하고 있어야 복제가 가능하다.
        - 구현하지 않을 경우에 clone()을 호출하면 `CloneNotSupportedException` 예외가 발생한다.

- 깊은 복제를 하려면 clone() 메소드를 원본 객체에서 재정의하고 참조 객체도 복제해야한다.

※ 참조 타입을 얕은 복제 객체로 생성했을 때의 문제점

복제 객체와 원본 객체가 참조 타입 하나의 메모리 주소를 공유하여 참조하기 때문에 복제 객체에서 참조 타입의 값을 바꿨을 때 원본 객체의 참조 타입의 값도 같이 바뀌게 된다!

이때, 깊은 복제를 하면 원본 객체와 복제 객체가 참조 타입을 참조하는 메모리 주소가 다르기 때문에 복제 객체에서 값을 수정해도 원본 객체의 값은 유지가 된다.

💡 **원본 객체를 보존하기 위해서는 깊은 복제를 사용해야 한다!**

### Clone()을 통한 얕은 복제 예제

```java
// clone을 만들기 위해 clone()을 쓰려면
// Cloneable을 구현해야한다.
public class Member implements Cloneable{
    private String id;
    private String name;
    private String password;
    private int age;
    private boolean adult;

    public Member(String id, String name, String password, int age, boolean adult) {
        this.id = id;
        this.name = name;
        this.password = password;
        this.age = age;
        this.adult = adult;
    }

    // getMember의 역할 : Member 객체와 똑같은 데이터를 가지고 있는 복제된 멤버를 만들어서 리턴
    public Member getMember() {
        // Member 타입 변수 cloned 초기화
        Member cloned = null;
        // this.clone() -> Object 타입이므로
        // Member 타입으로 리턴하기위해 강제변환
        try {
            cloned = (Member) clone();
        } catch (CloneNotSupportedException e) {}
        return cloned;
    }
}
```

```java
public class MemberExample {
    public static void main(String[] args) {
        Member original = new Member("blue", "홍길동", "12345", 25, true);

        // cloned에 getMember 메소드를 통해서 복제한 객체 저장
        Member cloned = original.getMember();

        // original 과 cloned는 데이터는 같지만 다른 객체이다.
    }
}
```

### clone()을 통한 깊은 복제 예제

① class Car

```java
public class Car {
    public String model;

    public Car(String model) {
        this.model = model;
    }
}
```

② 복제 클래스

```java
// 복제를 하기 위해선 클래스가 Cloneable 인터페이스를 구현해야한다.
public class Member implements Cloneable {

    // 참조 타입이 아니면 얕은 복제를 해도 데이터가 복사 된다.
    public String name;
    public int age;

    // 배열, 클래스 타입 같은 참조 타입의 경우에는
    // 얕은 복제를 하면 참조 타입의 번지만 복사가 되고
    // 실제 참조를 하고 있는 배열 객체나 클래스 객체는 복제되지 않는다.
    // 객체가 복제되는 것이 아니라 번지 주소를 공유하여 그 곳을 참조하는 것이다.
    // 참조 타입 복제 객체를 생성하기 위해서는 깊은 복제를 해야한다!
    public int[] scores;
    public Car car;

    public Member(String name, int age, int[] scores, Car car) {
        this.name = name;
        this.scores = scores;
        this.age = age;
        this.car = car;
    }

    // 깊은 복제를 위한 clone() 메소드 재정의
    @Override
    protected Object clone() throws CloneNotSupportedException {

        // 얕은 복제 먼저 하는 이유 : 참조 타입 아닌 변수들 먼저 얕은 복제해서 데이터 같게 만들기
        // 그 후 깊은 복제로 참조 타입 복제 객체 생성
        // scores와 똑같은 배열의 객체를 만들기 위해 Arrays.copyOf() 메소드 사용
        // Arrays.copyOf(original, newLength) : 배열을 복사해준다.
        // original : 복제할 원래 배열 / newLength : 새로운 배열의 길이(복제할 경우 원래 배열의 길이)

        // 얕은 복제로 cloned 복제 객체 생성하기 위해
        // 원래 clone()의 클래스 Object의 clone() 호출하기 위해서 super를 붙인다!
        Member cloned = (Member) super.clone();
        // 얕은 복제로 생성된 객체 안에 원래 배열을 복제한 배열을 대입! -> 깊은 객체 만드는 방법
        cloned.scores = Arrays.copyOf(this.scores, this.scores.length);
        // 얕은 복제로 생성된 객체 안에 Car 클래스 객체 대입 -> 깊은 객체 생성
        cloned.car = new Car(this.car.model);
        // 얕은 복제 + 참조 타입 객체 직접 대입을 통해 생성한 깊은 객체 cloned를 return
        return cloned;
    }

    public Member getMember() {
        Member cloned = null;
        // 이때 clone()으로 복제한 객체는 우리가 재정의한 깊은 복제된 객체
        try {
            cloned = (Member) clone();
        } catch (CloneNotSupportedException e) {}
        return cloned;
    }

}
```

③ 실행 클래스

```java
public class MemberExample {
    public static void main(String[] args) {

        Member original = new Member("홍길동", 25, new int[] {90, 90}, new Car("소나타"));

        // 깊은 복제 객체 생성 메소드 getMember()를 통해 cloned에 깊은 복제 객체 저장
       Member cloned = original.getMember();
    }
}
```

### 객체 소멸자 finalize()

- GC(Garbage Collector)는 객체를 소멸하기 직전에 마지막으로 객체의 소멸자 finalize()를 실행시킨다!
- Object의 finalize()는 기본적으로 실행 내용이 없다.
- 객체가 소멸되기 전에 실행할 코드가 있다면 Object의 finalize()를 재정의한다.

```java
@Override
protected void finalize() throws Throwable {
	System.out.println(no + "번 객체의 finalize()가 실행됨");
}
```

- 될 수 있다면 소멸자를 사용하지 않는 것(재정의해서 사용하지 않는 것)이 좋다.
    
    왜나하면,
    
    - GC는 메모리의 모든 쓰레기 객체를 소멸하지 않는다.
        - 메모리의 상태를 보고 일부만 소멸시킨다.
        - 따라서, 남아 있는 객체는 finalize() 메소드가 호출되지 않는다.
    - 또한, GC의 구동 시점이 일정하지 않다.
        - 메모리가 부족할 때 그리고 CPU가 한가할 때에 JVM에 의해서 자동 실행된다.

따라서, 객체 소멸자 finalize()를 사용하지 않고,

**객체 마무리 코드를 직접 작성해서 객체 소멸 전에 직접 호출하는 것이 좋다.**

## 11.4 Objects 클래스

### Objects 클래스

- 객체 비교, 해시코드 생성, null 여부, 객체 문자열 리턴 등의 연산을 수행하는 정적 메소드들로 구성된 Object의 유틸리티 클래스
- 유틸리티 클래스 : 다양한 기능을 제공해주는 클래스

![Untitled](CHAPTER%2011%2083867/Untitled%206.png)

👉 객체를 생성해서 사용하는 것이 아니라 Objects 유틸리티 클래스의 메소드를 이용해서 기능을 이용한다.

### 객체 비교 compare 메소드(Objects.compare(T a, T b, Comparator<T> c))

- a, b 두 객체를 비교자(c)로 비교해서 int값을 리턴

- Comparator<T> 인터페이스
    - 제네릭 타입으로 T타입의 객체를 비교하는 compare(T a, T b) 메소드를 가지고 있다.
    
    ```java
    public interface Comparator<T> {
    	int compare(T a, T b);
    }
    ```
    
    - compare(T a, T b) 메소드를 재정의해서 a와 b를 비교하는 코드를 작성해야 한다!
    
    → `a == b` : 0 리턴 / `a > b` : 양수 리턴 / `a < b` : 음수 리턴하도록 직접 재정의를 해야한다.
    
    ```java
    // Student 타입 객체를 비교하는 Comparator<Student> 인터페이스를 구현하는
    // StudentComparator 구현 클래스
    class StudentComparator implements Comparator<Student> {
        // compare 메소드 재정의
        public int compare(Student a, Student b) {
            // sno -> 학생의 학번
            // a 학번 < b 학번 : -1 리턴
            // a 학번 == b 학번 : 0 리턴
            // a 학번 < b 학번 : 1 리턴
            if(a.sno < b.sno) return -1;
            else if(a.sno == b.sno) return 0;
            else return 1;
    
            // 위의 if문 코드는 return Integer.compare(a.sno, b.sno); 와 같다.
        }
    }
    ```
    
    - Objcets.compare 사용 시에는 매개값으로 비교할 변수 2개, 인터페이스 Comparator<T>를 구현한 구현 클래스의 객체 1개가 들어가야한다!
    

### 동등 비교 equals()와 deepEquals()

- `equals()` : 객체 자체에 대한 비교(객체가 동일한 번지를 가지는 지 or 논리적 동등)
- `deepEquals()` : 비교 대상이 두 배열, 두 배열의 항목 값이 동등한 지

- **Objects.equals(Object a, Object b)** : 기본적으로 번지 비교 (재정의해서 논리적 동등 비교 가능)

![Untitled](CHAPTER%2011%2083867/Untitled%207.png)

👉 비교할 객체 a, b가 둘 다 null이 아니면 → `a.equals(b)` 역할으로 번지 비교 가능

👉 하나만 null일 경우 → `false` 리턴

👉 **둘 다 null일 경우** → `true` 리턴

- **deepEquals(Object a, Object b)**
    - 주로 배열을 비교할 때 쓰인다. 비교할 a, b가 배열이 아니면 Object.equals()와 기능이 같다.
    - 배열의 항목 값이 같으면 true, 다르면 false를 리턴한다.
    
    ![Untitled](CHAPTER%2011%2083867/Untitled%208.png)
    
    👉 비교할 a, b가 배열이 아닌 객체일 경우 : `a.equals(b)` 역할으로 번지 비교
    
    👉 비교할 a, b가 배열인 경우 : `Arrays.deepEquals(a, b)` 로 배열의 항목 값 비교
    
    👉 하나만 null일 경우 → `false` 리턴
    
    👉 **둘 다 null일 경우** → `true` 리턴
    

### 해시코드 생성(hash(), hashCode())

(hash()가 더 중요)

- Objects.hash(Object... values)
    - 매개값으로 주어진 값들을 이용해서 해시 코드를 생성하는 역할
    - 내부적으로 매개 값으로 주어진 값들을 배열로 만든다.
    - Arrays.hashCode(Object[])를 호출해서 해시코드를 얻어서 리턴한다.
    - 클래스의 hashCode()의 리턴값을 생성할 때 유용하게 사용할 수 있다.

```java
// class XXX 정의
class XXX {
	int field1;
	String field2;
	boolean field3;
}

@Override
public int hashCode() {
	return Objects.hash(field1, field2, field3);
}
```

👉 매개값 field1, field2, field3를 받아서 그에 대응되는 hashCode를 생성하는 `Objects.hash(field1, field2, field3)` 를 리턴!

→ 다른 객체가 동일한 field1, field2, field3을 가지고 있다면 동일한 해시코드를 리턴한다.

- Objects.hashCode(Object o)
    - 매개값으로 받은 o의 hashCode() (o.hashCode())를 호출하고 받은 값을 리턴한다.
    
    → 해시코드를 생성하는 hash()와 달리 객체가 가지고 있는 해시코드를 리턴하는 역할만 한다.
    
    - 매개값이 null이면 0을 리턴한다.

```java
static class Student {
	int sno;
	String name;

	// 생성자 매개 값으로 sno, name을 받아서 sno, name필드 초기화
	Student(int sno, String name) {
			this.sno = sno;
			this.name = name;
	}
	
	// 가진 해시코드를 리턴하는 hashCode()를 재정의
	// 해시코드를 필드값에 따라 생성하여 가져서 리턴하도록 재정의 -> hash() 사용
	@Override
	public int hashCode() {
			return Objects.hash(sno, name);
	}

}
```

👉 동일한 sno, name을 가진 객체에서 재정의한 hashCode()로 받은 해시코드는 같은 해시코드를 받게 된다.

### Null 여부 조사 (isNull() , nonNull(), requireNonNull())

- Objects.isNull(Object obj) : obj가 null일 경우 `true`
- Objects.nonNull(Object obj) : obj가 not null일 경우 `true`

- requireNonNull()

![Untitled](CHAPTER%2011%2083867/Untitled%209.png)

👉 obj가 Null이 아닐 경우 그 객체 값을  리턴하고, obj가 Null이라면 예외가 발생한다.

(매개변수 1, 2, 3개 가능) 

(객체, 예외메세지, 람다식관련..?)

```java
try {
	System.out.println(Objects.requireNonNull(str2, "이름이 없습니다"));
} catch(NullPointerException e) {
	System.out.println(e.getMessage());
}
```

👉requireNonNull의 매개 값이 2개로, 2번째 매개 값이 getMessage의 출력값이 되어 str2가 null일 시에 catch구문에서 “이름이 없습니다”를 출력한다.

### 객체 문자정보 (toString())

- 객체의 문자정보를 리턴한다.

![Untitled](CHAPTER%2011%2083867/Untitled%2010.png)

`toString(Object o, String nullDefault)` 중요

👉 Object인 o에 null이 들어오면, nullDefault를 리턴한다.

```java
System.out.println(Objects.toString(str2, "이름이 없습니다!"));
```

👉 str2가 null이라면 “이름이 없습니다!”가 출력된다.

## 11.5 System 클래스

### System 클래스 용도

- 운영체제 기능을 일부 이용
- 프로그램 종료, 키보드로부터 입력, 모니터로 출력, 메모리 정리, 현재 시간 읽기 등등
- 시스템 프로퍼티 읽기, 환경 변수 읽기

ex) `System.out.println("xxx");`

### 프로그램 종료 (exit()) : System.exit(0);

- 강제적으로 JVM을 종료
    - int 매개값을 지정하도록 되어 있는데 이 값을 종료 상태값이라고 한다.
        - 정상 종료일 경우 0으로 지정, 비정상 종료일 경우 0 이외의 다른 값 지정
        - 어떤 값을 주더라도 무조건 종료된다.
    - 만약 특정 상태값이 입력됐을 때만 종료하고 싶다면 자바의 보안관리자 설정
    
    System의 setSecurityManager 메소드 사용, checkExit 메소드 재정의
    
    checkExit → System.exit 실행 시 실행된다.
    
    ![Untitled](CHAPTER%2011%2083867/Untitled%2011.png)
    
    👉 System.exit 실행 시 checkExit가 호출되고, checkExit의 매개값으로 exit의 숫자가 들어가고, checkExit가 정상적으로 실행되면 프로그램이 종료된다. 
    
    Systme.exit에 5이외의 숫자가 들어가면 종료가 되지 않게 하기 위해서는 5이외의 숫자가 들어갔을 때 checkExit가 정상적으로 실행되지 않게 예외를 발생시키면 된다.
    

쓰레기 수집기 실행 요청(gc())

→ gc를 쓰는 이유 : 메모리 정리를 위해(사용하지 않는 객체 제거)

- JVM에게 가능한 빨리 GC를 실행해달라고 요청한다.

 

### 현재 시각 읽기(currentTimeMillis(), nanoTime())

- 현재 시간을 밀리세컨드(1/1000초)와 나노세컨드(1/10^9) 단위의 long 값 리턴

```java
long time = System.currentTimeMillis();
long time = System.nanoTime();
```

- 주로 프로그램 실행 소요 시간을 구할 때 이용한다.
    - 이용 방법 : 프로그램 실행 전, 후에 메소드 사용해서 long 타입으로 리턴받은 값 저장 후 `프로그램 실행 후 리턴 값(시간) - 프로그램 실행 전 리턴 값(시간) = 소요 시간`

### 시스템 프로퍼티 읽기 (getProperty())

- 시스템 프로퍼티 : JVM이 시작할 때 자동 설정되는 시스템의 속성값

- 대표적인 키와 값 (키와 값으로 저장, 키로 호출하면 값을 리턴)

![Untitled](CHAPTER%2011%2083867/Untitled%2012.png)

`String value = System.getProperty(String key);`

`System.getProperty(user.name)`을 주면 value 값인 현재 로그인한 사용자 이름이 리턴된다. 

### 환경 변수 읽기(getenv())

- 운영체제가 제공하는 환경 변수 값을 읽는다.

`String value = System.getenv(String name);`

String name에 환경 변수이름을 넣으면 환경변수에 해당하는 값을 리턴한다.

 

## 11.6 Class 클래스

### Class 클래스 용도

- 클래스와 인터페이스의 메타 데이터를 얻을 수 있다. (리플렉션)
    - 메타데이터 : 클래스의 이름, 생성자 정보, 필드 정보, 메소드 정보
- 문자열로 된 클래스명으로부터 동적 객체 생성을 할 수 있다.

### Class 객체 얻기(getClass(), forName())

- 객체로부터 얻는 방법 : `Class clazz = obj.getClass();`
- 문자열로부터 얻는 방법

```java
// 클래스명으로 입력한 클래스가 없을 경우 예외 발생하므로 try-catch
try {
	Class clazz = Class.forName(String className);
} catch (ClassNotFoundException e) {
}
```

```java
Car car = new Car();
// car 객체로부터 getClass를 통해 Class 객체 얻기
Class clazz1 = car.getClass();

// 클래스명으로 입력한 클래스가 없을 경우 예외 발생하므로 try-catch
try {
	// 패키지명을 포함한 클래스명을 매개값으로 주기
	Class clazz2 = Class.forName("sec06.exam01_class.Car");
} catch (ClassNotFoundException e) {
}
```

### 리플렉션

- 클래스의 생성자, 필드, 메소드 정보를 알아내는 것

```java
Constructor[] constructors = clazz.getDeclaredConstructors();
Field[] fields = clazz.getDeclaredFields();
Method[] methods = clazz.getDeclaredMethods();
```

※ Declared를 뺸 메소드도 존재한다. `getConstructors()` , ...

차이점은,

👉 `getDeclared~~` 는 그 클래스의 멤버만 가져온다.

👉 `get~~` 는 그 클래스의 멤버 뿐만 아니라 클래스가 상속 받고 있는 부모 클래스의 멤버도 가져온다.(public만)

```java
public class Car {
	private String model;
	private String owner;

	public Car() {

	}

	public Car(String model) {
		this.model = model;
	}

	// getter와 setter 생성
	public String getModel() {
    return model;    
	}
	
	public void setModel(String model) {
	    this.model = model;
	}
	
	public String getOwner() {
	    return model;
	}
	
	// setOwner의 접근 제한자 private으로 변경
	private void setOwner(String owner) {
	    this.owner = owner;
	}
	
}
```

```java
public static void main(String[] args) throws Exception {
    // 문자열로부터 Class 객체 얻기
    Class clazz = Class.forName("sec06.exam02_reflection.Car");

    // 클래스 이름 출력
    System.out.println(clazz.getName());

    // 생성자 정보 얻기
    // Car(), Car(String model) -> 배열로 리턴하므로 배열 타입으로 변수 선언
    Constructor[] constructors = clazz.getDeclaredConstructors();
    for(Constructor constructor : constructors){
        System.out.print(constructor.getName)
        // getParameterTypes : 매개변수의 Class 객체들을 배열로 리턴
        Class[]parameters=constructor.getParameterTypes();
        for(int i=0;i<parameters.length;i++){
        System.out.print(parameters[i].getName())
        }

    }

    // 필드 정보 얻기
    Filed[] fields = clazz.getDeclaredFields();
    for(Field filed : fields) {
        // getSimpleName() : 패키지명을 제외한 클래스명만 리턴
        System.out.println(field.getType().getSimpleName())
    }

    // 메소드 정보 얻기
    Method[] methods = clazz.getDeclaredMethods();
    for(Method method : methods) {
        System.out.println(method.getName())
        Class[] parameters = method.getParameterTypes();
        for(int i = 0; i < parameters.length; i++) {
        System.out.print(parameters[i].getName())
        }
    }
}
```

### 동적 객체 생성(newInstance())

- 실행 도중에 클래스 이름이 결정될 경우 동적으로 객체 생성을 할 수 있다.
- 일반적으로, 객체는 `new 클래스명`으로 생성했지만, 클래스명을 코드 작성 시에는 모르고, 실행 시에 결정된다고 하면 new연산자로 객체를 만들 수 없다.
- 이때, `newInstance` 를 사용하여 동적 객체를 생성한다!

```java
Class clazz = Class.forName("SendAction" 또는 "ReceiveAction");
Action action = (Action) clazz.newInstance();
action.execute();
```

👉 Class 객체인 clazz에서 clazz.newInstance()를 하게 되면 forName의 매개값으로 준 클래스의 Object 객체가 리턴된다. 

왜 Object 객체로 리턴될까? → forName의 매개값으로 어떤 클래스가 올지 모르기 때문에 Object 타입으로 범용성있게(모든 클래스는 Object를 상속하므로) 선언해놓고 타입 변환을 통해서 원하는 타입으로 바꿔야 한다!

이때, 두 가지 경우의 클래스(SendAction, ReceiveAction)가 들어올 수 있다고 할 때 SendAction이 들어올지, ReceiveAction이 들어올지 모르기 때문에 타입 변환도 함부로 하기 힘들다.

이때, 두 가지 경우의 클래스를 모두 `Action` 인터페이스를 구현하는 구현 클래스로 만들어 놓는다. 그 후 타입 변환을 `Action`으로 하게 되면 두 가지 클래스 중 어떤 클래스가 와도 그 클래스의 메소드를 사용할 수 있기 때문에 상관없다.

## 11.7 String 클래스

String 타입으로 선언해왔던 것이 클래스 타입 객체를 생성해서 저장한 것이다!

`String name = "홍길동";` == `String name = new String("홍길동");` 인 것이다!

### String 생성자

- byte[] 배열을 문자열로 변환하는 생성자 중요!

※ 중요한 이유 : 파일 내용 읽거나, 네트워크를 통해 받은 데이터는 일반적으로 byte[] 배열이므로 그 데이터를 문자열로 변환하기 위해 byte[] 배열을 생성자 매개값으로 받아서 문자열로 변환하여 이용하는 경우가 많기 때문에 중요하다!

```java
// 1번. 배열 전체를 String 객체 생성
String str = new String(byte[] bytes);

// 2번. 지정한 문자셋으로 디코딩
// byte 배열로 받을 때 인코딩한 것으로 디코딩해줘야하므로 두 번째 매개값 디코딩할 문자셋으로 지정
// 문자셋 - UTF-8, ...
String str = new String(byte[] bytes, String charsetName);

// 3번. 배열의 offset 인덱스 위치부터 length개만큼 String 객체 생성
String str = new String(byte[] bytes, int offset, int length);

// 4번. 3번 생성자 기능 + 지정한 문자셋으로 디코딩
String str = new String(byte[] bytes, int offset, int length, String charsetName);
```

- 키보드로부터 읽은 바이트 배열을 문자열로 변환

※ 키보드로 입력받으려면 `System.in` 사용

```java
byte[] bytes = new byte[100];
// 비어있는 byte 객체에 System.in을 사용하면 입력한 값을 저장한다.
// read 메소드는 입력한 값의 개수 저장
int readByteNo = System.in.read(bytes);
// 3번 생성자 사용 : offset 인덱스 위치부터 length개 만큼 String 객체 생성
// 끝의 enter키를 저장한 2개의 값을 빼고(readByteNo-2) 객체 생성
String str = new String(bytes, 0, readByteNo-2);
```

※ 입력한 값이 byte로 들어간다는 의미?

👉 Hello를 입력하면, byte 배열에 ‘H’, ‘e’, ‘l’, ‘l’, ‘o’ 이렇게 들어가는 것이 아니라, 해당 문자열에 대응되는 keycode로 들어가게 된다.

`72', '101, '108', '108', '111' 이렇게 keycode 형태로 배열에 저장된다.

### String 메소드

- String은 문자열의 추출, 비교, 찾기, 분리, 변환 등과 같은 다양한 메소드가 있다.
- 자주 사용하는 String 메소드

![Untitled](CHAPTER%2011%2083867/Untitled%2013.png)

- 문자 추출 : `charAt()`  → 파이썬 리스트에서 3번 인덱스 값을 리턴하는 list[3] 과 비슷

```java
String subject = "자바 프로그래밍"; // 한 글자씩 배열에 저장
chat charValue = subject.charAt(3); // "프" 리턴
```

- 문자열 비교 : `equals()` → String 클래스에서 문자열 비교하도록 equals()를 재정의

※ 대소문자 상관없이 문자열을 비교하는 String 메소드 : `equalsIgnoreCase()`

- 문자열을 바이트 배열로 변환 : `getBytes()`
    - 시스템의 기본 문자셋으로 인코딩된 바이트 배열 얻기
        
        `byte[] bytes = "문자열".getBytes();`
        

※ 문자열을 바이트 배열로 변환하는 이유?

👉 바이트 배열을 문자열로 변환할 때는 파일 내용을 읽거나, 네트워크를 통해 받은 데이터를 문자열로 사용하기 위해서였다.

반대로, 문자열을 바이트 배열로 변환할 때는 파일에 내용을 쓰거나, 네트워크를 통해 데이터를 보내기 위해서 바이트 배열로 변환한다!

이때, `getBytes()` 로 변환한다.

- 특정 문자셋으로 인코딩하여 바이트 배열을 얻기

```java
try {
	byte[] bytes = "문자열".getBytes("ECU-KR");
	byte[] bytes = "문자열".getBytes("UTF-8");
} catch (UnsupportedEncodingException e) {
}
```

- 찾는 문자열이 시작하는 인덱스찾기 : `indexOf()` → 파이썬의 index()와 비슷
    - 매개값으로 주어진 문자열이 시작되는 인덱스를 리턴
    - 주어진 문자열이 포함되어 있지 않으면 -1 리턴
    
    ```java
    Subject subject = "자바 프로그래밍"; // 배열 형태로 저장
    int index = subject.indexOf("프로그래밍");
    ```
    
    👉 `프로그래밍` 이 시작하는 “프”의 위치인 3번 인덱스를 리턴
    
    - 특정 문자열이 포함되어 있는지 여부에 따라 실행코드를 달리할 때 주로 사용한다!
    
    ```java
    if(문자열.indexOf("찾는 문자열") != -1) {
    		~~ // indexOf의 값이 -1이 아니라면 -> 찾는 문자열이 있다면
    } else {
    		~~ // indexOf의 값이 -1이라면 -> 찾는 문자열이 없다면
    }
    ```
    
- 문자열의 길이 : `length()` → 파이썬의 len()

- 문자열 대체 : `replace()`  → 파이썬의 replace()
    - 첫번째 매개값인 문자열을 찾아서 두번째 매개값인 문자열로 대치한 새로운 문자열 리턴
    
    ```java
    String oldStr = "자바 프로그래밍";
    String newStr = oldStr.replace("자바", "JAVA");
    ```
    
    ![Untitled](CHAPTER%2011%2083867/Untitled%2014.png)
    
    👉 그림과 같이 replace를 하면 원래의 객체에서 문자열이 대체되는 것이 아니라 새로운 객체를 만들어서 거기에 대체된 문자열을 저장한다.
    
    따라서, 새로운 String 타입 변수를 만들어서 거기에 대체된 문자열을 저장해야한다.
    
    `String newStr = oldStr.replace("자바", "JAVA");`
    

- 문자열 슬라이싱 : `substring()` → 파이썬의 슬라이싱 list[A:B]와 비슷!
    - substring(int beginindex, int endindex)
        - 주어진 시작과 끝 사이의 문자열 출력 (list[A:B])
    - substring(int beginindex)
        - 주어진 인덱스 이후부터 끝까지 문자열을 추출 (list[A:])
        - 주의할 점 : substring(7)이면 7번 인덱스~끝이 아니라 7번 이후 인덱스인 8번 인덱스~끝으로 슬라이싱된다.
        
- 알파벳 대,소문자 변경 : `toLowerCase()` / `toUpperCase()`

👉 마찬가지로, 변경한 문자열은 원래 문자열을 건드리지 않고 새로운 객체로 생성된다.

```java
String original = "Java Programming";
String lowerCase = original.toLowerCase();
String upperCase = original.toUpperCase();
```

- 문자열 앞뒤 공백 잘라내기 : `trim()`

→ 사용자가 입력한 데이터를 사용할 때, 사용자가 앞뒤 공백을 주고 입력한 경우가 있다.

→ 이때 주어진 데이터를 잘 사용하기 위해서는 공백을 제거하고 문자열만 받는 것이 좋기 때문에 `trim()` 을 이용한다!

※ trim을 사용하면 앞뒤 공백은 제거되지만 문자열 중간의 공백은 제거되지 않는다!

```java
String oldStr = "    자바 프로그래밍     ";
String newStr = oldStr.trim();
```

- **★문자열 변환★** : `valueOf()`
    - 기본 타입의 값을 문자열로 변환해주는 메소드
    - static으로 정의되어 있는 정적 메소드

```java
String str1 = String.valueOf(10); // "10"으로 변환
String str2 = String.valueOf(10.5); // "10.5"로 변환
String str3 = String.valueOf(ture); // "true"로 변환
```

## 11.8 StringTokenizer 클래스 - 문자열 분리 제공

→ 파이썬의 `split()` 과 비슷! : String 클래스에 split() 메소드가 있다!

※ String 클래스의 split() 메소드와 StringTokenizer 클래스 차이점

- split : 정규 표현식을 이용해서 구분자를 찾는다
- StringTokenizer : 구분자를 매개 값으로 제시한다. (파이썬의 split()과 비슷)

### String의 split()

- 정규 표현식을 구분자로 해서 부분 문자열을 분리한 후, 배열에 저장하고 리턴한다.

```java
String text = "홍길동&이수홍,박연수,김자바-최명호";
String[] names = text.split("&|,|-");
```

👉 `|` 는 or의 의미이므로 구분자로 `&` or `,` or `-` 를 사용해서 구분하겠다는 의미!

구분한 값들을 배열을 만들어서 배열에 저장 후 배열을 리턴

### StringTokenizer

- 구분자의 종류를 String의 split()처럼 다양하게 줄 수 없고, 하나만 줄 수 있다.
- 구분한 값들을 저장만 하고 리턴하지 않는다!
- 따로 값들을 꺼내는 메소드를 사용해야한다.

![Untitled](CHAPTER%2011%2083867/Untitled%2015.png)

토큰 : 구분자로 구분하여 저장한 값

```java
String text = "홍길동/이수홍/박연수";
StringTokenizer st = new StringTokenizer(text, "/");
// StringTokenizer 타입 st 변수에 구분 값들이 저장되어 있다.

// 구분 값들을 저장한 st에 값이 없을 때까지 반복
while(st.hasMoreTokens()) {
	String token = st.nextToken(); // 구분 값을 하나씩 꺼내옴
	System.out.println(token);
}
```

## 11.9 StringBuffer, StringBuilder 클래스

→ String의 단점을 보완한 클래스

→ String은 내부의 문자열을 수정할 수 없다!

```java
String data = "ABC";
data += "DEF";
```

👉 String 타입의 data에 DEF를 붙인 값을 넣고 싶어서 저렇게 코드를 작성하면, data의 원래 객체에 “ABCDEF”가 저장되는 것이 아니고 새로운 객체를 만들어서 “ABCDEF”를 저장하게 된다!

이런 구조이기 때문에 문자열을 수정, 추가, 삭제할 때마다 메모리가 많이 소요된다.

이러한 구조를 보완하여 나온 것이 `StringBuffer` / `StringBuilder` 클래스이다.

### StringBuffer, StringBuilder

- 버퍼(buffer : 데이터를 임시로 저장하는 메모리)에 문자열을 저장한다.
- 버퍼 내부에서 추가, 수정, 삭제를 할 수 있다.

→ 추가, 수정, 삭제할 때마다 버퍼가 하나씩 생기는 것이 아니라 하나의 버퍼 내부에서만 변화가 일어나기 때문에 메모리를 적게 사용하여 값을 변화시킬 수 있다.

- 멀티 스레드 환경 : `StringBuffer` 사용 / 단일 스레드 환경 : `StringBuilder` 사용
    
    (사용 방법은 둘 다 같다.)
    

```java
 // 기본 저장공간으로 StringBuilder 생성
StringBuilder sb = new StringBuilder();

// 16자가 저장될 공간으로 StringBuilder 생성
StringBuilder sb = new StringBuilder(16);

// "Java" 라는 데이터를 가지고 있는 StringBuilder 생성
StringBuilder sb = new StringBuilder("Java");
```

- StringBuilder, StringBuffer 생성 후 StringBuilder, StringBuffer의 메소드를 사용하여 데이터 추가, 수정, 삭제 가능

![Untitled](CHAPTER%2011%2083867/Untitled%2016.png)

```java
StringBuilder sb = new StringBuilder();

// 아무것도 없는 StringBuilder에 값 추가 : append()
sb.append("Java ");
sb.append("Program Study");

// sb에 저장되어 있는 문자열 리턴 : toString()
System.out.println(sb.toString());

// sb의 원하는 인덱스에 값 추가 : insert()
sb.insert(4,"2"); // 4번째 인덱스에 2 추가

// sb의 특정 위치의 문자 변경 : setCharAt()
sb.setCharAt(4, "6"); // 4번째 인덱스의 문자를 6으로 변경

// sb의 범위 안의 내용을 다른 내용으로 대체 : replace()
sb.replace(6, 13, "Book"); // 6번~13번 인덱스의 문자열을 Book으로 대체

// sb의 범위 안의 내용을 삭제
sb.delete(4, 5); // 4~5번 인덱스의 문자열 삭제

```

## 11.10 정규 표현식과 Pattern 클래스

### 정규 표현식(Regular Expression)

- 문자열이 정해져 있는 형식으로 구성되어 있는지 검증할 때 사용
    - 이메일, 전화번호, 비밀번호 등
- 문자 또는 숫자 기호와 반복 기호가 결합된 문자열이다.

### 정규 표현식 작성방법

- 기본적으로 알아야 할 기호들

![Untitled](CHAPTER%2011%2083867/Untitled%2017.png)

ex)

- 전화번호 (010-0123-4567)

`[02|010]-\d{3,4}-\d{4}`

`[02|010]` :  02 or 010 필수 : 010

`-` : 전화번호 사이 사이 문자

`\d` : 숫자를 의미 / `{3, 4}` : 3개 ~ 4개 → `\d{3,4}` : 숫자 3개 ~ 4개 필수 : 0123

`\d{4}` : 숫자 4개 필수 : 4567

- 이메일 (ohk9134@naver.com)

`\w+@\w+\.\w+(\.\w+)?`

`\w` : 1개의 알파벳 or 1개의 숫자 / `+` : 1개 이상 → `\w+` : 1개 이상의 알파벳 or 숫자 : ohk9134

`@` : 이메일 형식 문자 : @

`\.` : . 표시 : .

`(\.\w+)?` : `(\.\w+)` 부분이 없거나 1개 올 수 있다. : naver.co.kr 같은 경우

### Pattern 클래스

- 정규 표현식으로 문자열을 검증하는 역할
- Pattern의 정적 메소드 `matches()` 를 사용하여 검증

`boolean result = Pattern.matches("정규식", "입력된 문자열");`

👉 Pattern.matches의 두번째 매개값으로 입력된 문자열을 줘서 그 문자열이 첫번 째 매개값인 정규식 형식에 맞게 작성되었다면 `true` 리턴, 아니면 `false` 를 리턴한다.

※주의할 점 : 자바에서는 역슬래시(\)가 특수한 용도로 사용되므로 순수한 역슬래시 자체를 얻기 위해서는 역슬래시를 두 번 써야한다. → `\\d` : `\d`로 인식

```java
// 정규식 저장
String regExp = "(02|010)-\\d{3,4}-\\d{4}";
String data = "010-123-4567;

// 사용자가 입력한 데이터(data)가 정규식(regExp) 형식에 맞는지 검증하기 위해
// Pattern.matches() 사용
boolean result = Pattern.matches(regExp, data);
```

## 11.11 Arrays 클래스

- 배열 조작 기능을 가지고 있는 클래스(Java.util 패키지에 포함됨)
    - 배열의 복사, 항목 정렬, 항목 검색에 사용된다.
    
- 제공하는 정적 메소드

![Untitled](CHAPTER%2011%2083867/Untitled%2018.png)

- 배열 복사
    - Arrays.copyOf(원본 배열, 복사할 길이)
        - 0~ (복사할 길이-1)까지 항목 복사
        - 복사할 길이는 원본 배열의 길이보다 커도 되며, 복사한 배열의 크기가 된다.
    
    ```java
    char[] arr1 = {"J", "A", "V", "A"};
    char[] arr2 = Arrays.copyOf(arr1, arr1.length);
    ```
    
    - Arrays.copyOfRange(원본 배열, 시작 인덱스, 끝 인덱스)
        - 시작 인덱스 ~ (끝 인덱스 -1) 까지 항목 복사
    
    ```java
    char[] arr1 = {"J", "A", "V", "A"};
    char[] arr2 = Arrays.copyOfRange(arr1, 1, 3);
    ```
    
    - System.arraycopy()
    
    `System.arraycopy(원본 배열, 원본 시작 인덱스, 타겟 배열(만들 배열), 타겟 시작 인덱스, 복사 개수)`
    
    ![Untitled](CHAPTER%2011%2083867/Untitled%2019.png)
    

- 배열 항목 비교
    - Arrays.equals(배열, 배열)
        - 1차 항목의 값만 비교
        - 중첩된 배열(2차원 배열)의 항목은 비교하지 못 한다.
    - Arrays.deepEquals(배열, 배열)
        - 중첩된 배열의 항목까지 비교
        
- 배열 항목 정렬
    - Arrays.sort(배열)
        - 항목을 오름차순으로 정렬
    - 기본 타입이거나 String 배열은 sort를 썼을 때 자동으로 정렬된다.
    - 사용자 정의 클래스 배열일 경우, 배열 객체들이 Comparable 인터페이스를 구현해야만 정렬된다.
    
- 배열 항목 검색
    - Arrays.binarySearch(배열, 찾는 값)
    - 특정 값이 위치한 인덱스를 얻는 것
    - 얻는 방법 : Arrays.sort(배열)로 정렬 후에, Arrays.binarySearch(배열, 찾는값) 메소드로 항목을 찾을 수 있다.
    - → binarySearch는 찾는 값의 인덱스를 리턴한다.
    

‘

## 11.12 포장(Wrapper) 클래스

### 포장(Wrapper) 객체

- 기본 타입(byte, char, short, int, long, float, double, boolean) 값을 포장하는 객체
- 기본 타입의 값은 외부에서 변경할 수 없다.

→ 수정하려면 기본 타입의 객체를 새로 생성해서 수정할 값을 저장해야한다.

![Untitled](CHAPTER%2011%2083867/Untitled%2020.png)

### 박싱(Boxing)과 언박싱(Unboxing)

- 박싱(Boxing) : 기본 타입의 값을 포장 객체로 만드는 것
- 언박싱(Unboxing) : 포장 객체에서 기본 타입의 값을 얻는 것

- 박싱 코드
    - 생성자 이용
    
    `Integer obj = new Integer(1000);`
    
    ※ 문자열로 줘도 내부적으로 변환되어서 객체로 만들어진다.
    
    `Integer obj = new Integer(1000)` == `Integer obj = new Integer("1000");`
    
    - valueOf() 메소드 이용
    
    ```java
    Integer obj = Integer.valueOf(1000);
    Integer obj = Integer.valueOf("1000");
    ```
    

- 박싱된 값을 얻어내는 언박싱 코드
    
    obj :  박싱된 객체
    
    `num = obj.intValue();`
    
    `num = obj.doubleValue();`
    

### 자동 박싱과 언박싱

- 자동 박싱은 포장 클래스 타입에 기본값이 대입될 경우에 발생

`Integer obj = 100;  // 자동 박싱` → 자동으로 객체가 생성된다.

- 자동 언박싱은 기본 타입에 포장 객체가 대입될 경우에 발생

```java
Integer obj = new Integer(200);
int vaulue1 = obj // 자동 언박싱
int value2 = obj + 100; // 자동 언박싱
```

👉 obj는 Integer 포장 클래스로 생성한 객체인데, 이를 기본 타입인 int 변수에 저장하게 되면 자동 언박싱 된다.

### 문자열을 기본 타입 값으로 변환 - 자주 활용됨!

ex) 문자열 “1000”을 int 타입의 1000으로 바꾸고 싶을 때

`num = Integer.parseInt("1000");`

### 포장값 비교

- 포장 객체는 내부의 값을 비교하기 위해 포장 객체로 ==와 != 연산자를 쓸 수 없다.
    
    (==와 !=는 내부의 값을 비교하는 것이 아니라 번지를 비교하기 때문에!)
    
    - 예외 : 다음 값이라면, 포장 객체로 ==과 != 연산자를 사용해서 내부 값 비교 가능
    
    | 타입 | 값의 범위 |
    | --- | --- |
    | boolean | true, false |
    | char | \u0000~\u007f |
    | byte, short, int | -128~127 |
    
    💡 ==와 !=를 사용가능한 예외가 있긴 하지만, 내부의 값을 비교할 때는 값을 언박싱해서 비교하거나, equals() 메소드로 내부 값을 비교하는 것이 좋다!
    
    `obj1 == obj2` (X)
    
    `obj1.intValue() == obj2.intValue()` (O)
    
    `obj1.equals(obj2)` (O)
    
    👉 이때 equals는 Object 클래스의 equals가 아니라 Wrapper가 재정의한 equals다!
    

## 11.13 Math, Random 클래스

### Math 클래스

- 수학 계산에 사용할 수 있는 정적 메소드를 제공

→ 파이썬과 비슷 (abs, ceil, floor, random, round 등등)

→ max, min도 Math 클래스이다.

- Math.random() → 파이선 random과 같다

- 공식 : 1~n 까지 정수 랜덤

`int num = (int) (Math.random() * n) + start;`

### Random 클래스

- boolean, int, long, float, double 난수를 얻을 수 있다.
- 난수를 만드는 알고리즘에 사용되는 seed를 설정할 수 있다.

👉 seed 값이 같으면 같은 난수를 얻을 수 있다.

- 객체 생성 방법

| 생성자  | 설명 |
| --- | --- |
| Random() | 호출 시 마다 다른 seed 값(현재 시간 이용)이 자동 설정 |
| Random(long seed) | 매개 값으로 주어진 seed 값을 설정 |

👉 Random의 매개변수를 주지 않으면 실행 시 마다 랜덤!

👉 Random의 매개변수로 seed값을 주면 같은 seed 값을 주면 같은 값 리턴!

- 제공 메소드

![Untitled](CHAPTER%2011%2083867/Untitled%2021.png)

## 11.14 Date, Calendar 클래스

### Date 클래스

- 날짜를 표현하는 클래스
- 날짜 정보를 객체 간에 주고 받을 때 주로 사용

```java
Date now = new Date();
// toString으로 Date 객체가 갖고 있는 현재 날짜 정보 받아오기
String strNow1 = now.toString();
System.out.println(strNow1);
// 이렇게 출력할 경우 Thu Dec 19 ~~ 이렇게 출력된다.
// 출력 형태를 바꾸고 싶다면 SimpleDateFormat 객체를 생성해야한다.

SimpleDateFormat sdf = new SimpleDateFormat("yyyy년 MM월 dd일 hh시 mm분 ss초");
// SimpleDateFormat의 format() 메소드로 Date 객체에 형식 적용
String strNow2 = sdf.format(now);
System.out.println(strNow2); 
// 2022년 3월 5일 10시 13분 35초 이렇게 출력된다!
```

### Calendar 클래스

- 달력을 표현한 추상 클래스
- OS에 설정되어 있는 시간대(TimeZone)를 기준으로 한 Calendar 객체 얻기

→ 생성자를 이용해서 객체를 만들 수 없다.

→ 따라서, `getInstance()` 라는 정적 메소드를 이용해서 현재 날짜를 가진 Calendar 객체를 만든다.

`Calendar now = Calendar.getInstance();`

- 다른 시간대의 Calendar 객체 얻기

```java
// 이때도 생성자로 객체 만들 수 없다.
// 따라서 getTimeZone() 정적 메소드로 객체 생성
TimeZone timeZon = Timezone.getTimeZone("America/Los_Angeles");
Calendar now = Calendar.getInstance(timeZon);
```

👉설정한 TimeZone(America/Los_Angeles)에 맞는 날짜를 Calendar 객체로 생성할 수 있다.

- 날짜 및 시간 정보 얻기 : `get` 정적 메소드 사용
    - 매개 값으로 Calendar에 정의되어 있는 상수를 넣어준다!

```java
int year = now.get(Calendar.YEAR);  // 년도 리턴
int month = now.get(Calendar.MONTH) + 1;  // 월 -> 0~11리턴 +1을 해줘야 1~12달 리턴
int day = now.get(Calendar.DAY_OF_MONTH);  // 일 리턴
int week = now.get(Calendar.DAY_OF_WEEK);  // 일 리턴
...
```

## 11.15 Fomat 클래스

### 형식(Format) 클래스

- 숫자와 날짜를 원하는 형식의 문자열로 변환
- 종류
    - 숫자 형식 : DecimalFormat
    - 날짜 형식 : SimpleDateFormat
    - 매개변수화된 문자열 형식 : MessageFormat

### 숫자 형식 클래스 : DecimalFormat

```java
DecimalFormat df = new DecimalFormat("#,###,0");
String result = df.format(1234567.89);
```

👉 DecimalFormat 생성자 매개값으로 형식을 줘서 DecimalFromat 객체를 만든 후 객체에 format()을 사용하면 format의 매개값인 1234567.89가 형식에 맞춰서 1,234,567.9로 저장된다.

![Untitled](CHAPTER%2011%2083867/Untitled%2022.png)

👉 0 : 자릿수를 채우기 위해 0이 들어간다 / # : #의 개수에 상관없이 자릿수에 맞는 수만 출력

### 날짜 형식 클래스 : SimpleDateFormat

```java
SimpleDateFormat sdf = new SimpleDateFormat("yyyy년 MM월 dd일");
String strDate = sdf.format(new Date());
```

### 매개변수화된 문자열 형식 클래스 : MessageFormat

```java
String message = "회원 ID : {0} \n 회원 이름 : {1} \n 회원 전화 : {2}";
String result = MessageFormat.format(message, id, name, tel);
```

👉 message의 {0}, {1}, {2} 순서대로 id, name, tel 매개변수가 들어간다.

👉 python의 `print({0}, .format(~))` 와 비슷

## 11.16 java.time 패키지

### java.time 패키지

- 자바 8부터 추가된 패키지
- 날짜와 시간을 나타내는 여러가지 API 새롭게 추가
- 날짜와 시간을 조작(연산)하거나 비교하는 기능 추가
    - 기존의 Date와 Calendar는 날짜, 시간을 조작하거나 비교하는 기능이 불충분

| 패키지 | 설명 |
| --- | --- |
| java.time | 날짜와 시간을 나타내는 핵심 API인 LocalDate, LocalTime, LocalDateTime, ZonedDateTime을 포함한다. 이 클래스들은 ISO-8601에 정의된 
달력 시스템에 기초한다. |
| java.time.chrono | ISO-8601에 정의된 달력 시스템 이외의 다른 달력 시스템을 사용하고자 할 때 사용할 수 있는 API 포함 (EX : 우리나라의 음력) |
| java.time.format | 날짜와 시간을 파싱하고 포맷팅하는 API들 포함 |
| java.time.temporal | 날짜와 시간을 연산하기 위한 보조 API들 포함 |
| java.time.zone | 타임존을 지원하는 API들 포함 |

### 날짜와 시간 객체 생성

- 날짜와 시간을 표현하는 5개 클래스

| 클래스명 | 설명 |
| --- | --- |
| LocalDate | 로컬 날짜 클래스 |
| LocalTime | 로컬 시간 클래스 |
| LocalDateTime |  로컬 날짜 및 시간 클래스 (LocalDate + LocalTime) |
| ZonedDateTime | 특정 TimeZone의 날짜, 시간 클래스 (다른 나라의 날짜, 시간) |
| Instant | 특정 시점의 Time-Stamp 클래스 |

- LocalDate

```java
// 현재 날짜를 받는 LocalDate 객체 생성
LocalDate currDate = LocalDate.now();

// of 메소드를 사용하여 직접 년, 월, 일을 입력해서 LocalDate 객체 생성
LocalDate targetDate = LocalDate.of(int year, int month, int dayOfMonth);
```

- LocalTime

```java
// 현재 시간을 받는 LocalTime 객체 생성
LocalTime currTime = LocalTime.now();

// of 메소드를 사용하여 직접 시, 분, 초, 나노초를 입력해서 LocalTime 객체 생성
LocalTime targetTime = LocalTime.of(int hour, int minute, int second, int nanoOfSecond);
```

- LocalDateTime

```java
// 현재 날짜, 시간을 받는 LocalDateTime 객체 생성
LocalDateTime currTime = LocalDateTime.now();

// of 메소드를 사용하여 직접 년, 월, 일, 시, 분, 초, 나노초를 입력해서 LocalDateTime 객체 생성
LocalDateTime targetDateTime = LocalDateTime.of(int year, int month, int dayOfMonth, int hour, int minute, int second, int nanoOfSecond);
```

- ZonedDateTime : LocalDateTime + 지역에 대한 정보
    - 저장 형태 : 2022-03-05 T11:35:24.017 +09:00 [Asia/Seoul]
    
    👉 여기서 +09:00부터 Zone에 대한 정보
    
    👉 +09:00 : 시차(존오프셋) / [Asia/Seoul] : 지역(존아이디)
    
- 존오프셋(ZoneOffset)
    - 협정세계시와 차이나는 시간(시차)
- 존아이디(ZoneId)
    - java.util.TimeZone의 getAvailableIDs() 메소드가 리턴하는 유효한 값 중 하나

```java
ZonedDateTime utcDateTime = ZonedDateTime.now(ZoneId.of("UTC"));
ZonedDateTime londonDateTime = ZonedDateTime.now(ZoneId.of("Europe/London"));
ZonedDateTime utcDateTime = ZonedDateTime.now(ZoneId.of("Asia/Seoul"));
```

👉 of() 메소드로 ZoneID에 유효한 값(java.util.TimeZone의 getAvailableIDs() 메소드가 리턴하는 유효한 값)을 지정하고 now() 메소드를 사용하면 그 값으로 ZonedDateTime 객체가 생성된다!

- Instant
    - 날짜와 시간의 정보를 얻거나 조작하는 데에 사용 X
    - 특정 시점의 타임 스탬프로 사용 (특정 시점을 가지고 있는 객체)
    - 주로 특정 두 시점 간의 시간적 우선 순위를 따질 때 주로 사용
    
    👉 `isBefore()` / `isAfter()` 메소드 사용
    
    - java.util.Date와 가장 유사한 클래스
    
    ```java
    Instant instant1 = Instant.now();
    Instant instant2 = Instant.now();
    if (instant1.isBefore(instant2)) {System.out.println("instant1이 빠릅니다");}
    else if (instant1.isAfter(instant2)) {System.out.println("instant1이 늦습니다");}
    else {System.out.println("동일한 시간입니다.");}
    System.out.println("차이(nanos)" + instant1.until(instant2, ChronoUnit.NANOS))
    ```
    
    👉 until() 메소드 : intstant1과 instant2가 얼마나 시간 차이가 나는지 비교하는 메소드
    
    뒤에 매개값은 단위 : ChronoUnit.NANOS → 나노초
    

### 날짜와 시간에 대한 정보 얻기

![Untitled](CHAPTER%2011%2083867/Untitled%2023.png)

- LocalDateTime/ZonedDateTime은 위 표에 나와 있는 대부분의 메소드를 가진다.
    - 단, `isLeapYear()` 는 toLocalDate() 메소드로 LocalDate로 변환 후 사용해야 한다.
    - ZonedDateTime에서 제공하는 추가 메소드
    
    ![Untitled](CHAPTER%2011%2083867/Untitled%2024.png)
    

```java
LocalDateTime now = LocalDateTme.now();
now.getYear();  // 현재 날짜, 데이터에서 년도 추출
now.getMonth(); // 현재 날짜, 데이터에서 월 추출
now.getDayOfMonth(); // 현재 날짜, 데이터에서 월의 일 추출
```

### 날짜와 시간을 조작하기

- 빼기와 더하기

![Untitled](CHAPTER%2011%2083867/Untitled%2025.png)

- 날짜와 시간 변경하기 : with~~

![Untitled](CHAPTER%2011%2083867/Untitled%2026.png)

👉 매개 값으로 준 값으로 변경

`with(TemporalAdjuster adjuster)` : 현재 날짜를 기준으로 상대적인 날짜를 리턴한다.

`TemporalAdjuster adjuster` : 객체! 

`with(TemporalAdjuster.firstDayOfYear)` : 이번 해의 첫일로 변경

- 날짜와 시간 비교

![Untitled](CHAPTER%2011%2083867/Untitled%2027.png)

- Period와 Duration
    - Period : 년, 달, 일의 양을 나타내는 날짜 기준 클래스
    - Duration : 시, 분, 초, 나노초의 양을 나타내는 시간 기준 클래스
    
    ![Untitled](CHAPTER%2011%2083867/Untitled%2028.png)
    
- `between()` 메소드의 차이점
    - Period와 Duration의 between()
        - 년, 달, 일, 초의 단위별 단순 차이를 리턴
        
        👉 ex) 2020년 5월(date1)과 2021년 3월(date2)의 차이 : Period.between(date1, date2) : 단순히 5월과 3월을 비교해서 `5-3 = 2` 로 출력
        
    - ChronoUnit의 between()
        - 전체 시간을 기준으로 차이를 리턴
        
        👉 ex) 2020년 5월(date1)과 2021년 3월(date2)의 차이 : ChronoUnit.MONTHS.between(date1, date2) : 전체를 비교해서 2021년 3월 - 2020년 5월해서 10을 출력
        

### 파싱과 포맷팅

- 파싱 : 주어진 문자열로 날짜와 시간을 생성 (입력 형식..?)
- 포맷팅 : 날짜와 시간을 형식화된 문자열로 변환 (출력 형식)

### 파싱(Parsing) 메소드

![Untitled](CHAPTER%2011%2083867/Untitled%2029.png)

- parse(CharSequence)
    - ISO_LOCAL_DATE 포맷터를 기본으로 사용해서 문자열을 파싱
        - ISO_LOCAL_DATE : DateTimeFommatter의 상수로, “2022-03-05”의 형식
    
    `LocalDate localDate = LocalDate.parse("2022-03-05");`
    

- parse(CharSequence, DateTimeFommatter)
    - 주어진 포맷터(두번째 매개값)로 문자열을 파싱한다.
    - DateTimeFommater(포맷터)는 ofPattern() 메소드로 얻을 수 있다. (형식을 생성)
    
    ```java
    DateTimeFomatter fomatter = DateTimeFomatter.ofPattern("yyyy.MM.dd");
    
    // ofPattern으로 생성한 포맷터로 문자열 파싱(입력)
    LocalDate localDate = LocalDate.parse("2022.03.05", fomatter);
    ```
    
    - 이렇게 ofPattern으로 직접 포맷터를 생성하여 적용하지 않고 표준 포맷터를 사용해도 좋다.
    
    👉 DateTimeFommater의 상수로 정의되어 있다.
    
    ![Untitled](CHAPTER%2011%2083867/Untitled%2030.png)
    

### 포맷팅(Fommating) 메소드

![Untitled](CHAPTER%2011%2083867/Untitled%2031.png)

```java
LocalDateTime now = LocalDateTime.now();
DateTimeFomatter fomatter = DateTimeFomatter.ofPattern("yyyy.MM.dd");

String nowString = now.format(fomatter);
```

👉 nowString은  `yyyy.MM.dd` 형식으로 출력된다.