# CHAPTER 3 : 연산자

## 3.4 이항 연산자

### 오버플로우 탐지

오버플로우 : 타입에 저장될 수 있는 값의 범위를 넘은 값이 저장되는 것!

이를 방지하기 위해 연산 전에 오버플로우인지 아닌지를 탐지해야한다.

```java
public class OverflowExample {
	public static void main(String[] args) {
			int x = 1000000;
			int y = 1000000;
			int z = x * y;
	}
}
```

👉 이때 z의 값은 10^6 * 10^6으로 10^12인데 int에 저장될 수 있는 값의 범위를 초과하여 쓰레기 값인 -7273779968을 저장하게 된다.

👉 이를 방지하기 위해 조건문과 예외 처리 구문을 활용해서 연산 전에 오버플로우를 방지해야한다.

```java
public class CheckOverflowExample {
	public static void main(String[] args) {
		try {
			int result = safeAdd(2000000000, 2000000000);
			System.out.println(result);
		} catch(ArithmeticException e) {
			System.out.println("오버플로우가 발생하여 정확하게 계산할 수 없음");
		}
	}

	public static int safeAdd(int left, int right) {
		if(right > 0) {
			if (left + right > Integer.MAX_VALUE) {
				throw new ArithmeticException("오버플로우 발생");
			}
		} else { // right <= 0일 때 
			if (left + right < Integer.MIN_VALUE) {
				throw new ArithmeticException("오버플로우 발생");
			}
		}
		return left + right;
	}
}
```

### NaN과 Infinity 연산

수를 0으로 나누거나 나머지를 구하면 `ArithmeticExxception` 예외가 발생해서 예외처리 구문으로 방지하면 되지만, 

수를 0.0으로 나누거나 나머지를 구하면 `Infinity` / `NaN` 이 반환된다!

```java
5 / 0 => Infinity
5 % 0 => NaN
```

`Infinity` 와 `NaN` 은 예외처리로 방지할 수 없다.

이때 `Double.isInfinite()` / `Double.isNaN()` 메소드를 사용해서 조건문으로 처리해야한다.

이 메소드들은 수가 `Infinitiy` / `NaN` 이면 `true` 를, 아니면 `false` 를 반환한다.

이를 이용하여 다음과 같은 조건문으로 잘못된 계산을 방지할 수 있다.

```java
if(Double.isInfinite(z) || Double.isNaN(z)) {
		System.out.println("값 산출 불가");
} else {
		System.out.println(z + 2); // 조건이 false일 때만 연산
}
```

### String 타입의 문자열 ==, != 연산자 사용 시 주의할 점(equals() 메소드 사용)

String 타입의 문자열에서 ==, != 연산자를 사용할 때

 `new` 라는 객체 생성자로 생성한 새로운 String 객체에서는 사용하면 원치 않는 결과가 나오게 된다.

```java
String strVar1 = "테스트";
String strVar2 = "테스트";
String strVar3 = new String("테스트");
```

이렇게 strVar3을 `new` 라는 객체 생성자로 생성했을 때, 문자열은 `테스트` 로 같지만 메모리 주소 값이 다르게 된다!

따라서, strVar1 / strVar2와 strVar3을 비교했을 때 값은 같지만 메모리 주소가 다르기 때문에 `false` 를 반환하게 된다.

```java
strVar1 == strVar2 : true 반환
strVar1 == strVar3 : false 반환 
```

따라서, 문자열이 같은지 안 같은지를 비교하기 위해서는 `equals()` 메소드를 사용해야한다!

`equals()` 는 메모리 주소와 상관 없이 주어진 문자열만 비교하기 때문에

`strVar1.equals(strVar3)` 을 하면 `true` 를 반환한다!!