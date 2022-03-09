# CHAPTER 2 : 변수와 타입

---

## 2.3 타입 변환

① 자동 타입 변환 : Promotion

② 강제 타입 변환 : Casting

### 자동 타입 변환 Promotion

```java
byte byteValue = 10;
int intValue = byteValue;
```

→ `byte` 타입인 byteValue를 `int` 타입인 intValue의 값으로 저장하는 순간 byteValue의 값인 10은 `int` 타입으로 바뀐다!

이때, 자동변환되는 값에는 변화가 없지만, int가 double로 변환될 때는 값의 형태에 소수점 .0이 추가된다.

```java
int intValue = 200;
double doubleValue = intValue; // doubleValue는 200이 아닌 200.0이 저장된다.
```

### 연산식에서의 자동 타입 변환

: 연산은 같은 타입의 값들로 수행되기 때문에 타입이 다르면 타입 크기가 큰 쪽의 타입으로 변환된다.

```java
int intValue = 10;
double doubleValue = 5.5;

double result = intValue + doubleValue;  // result = 15.5
// 이때, intValue와 doubleValue는 타입이 int, double로 다르므로
// 연산 시 하나의 타입으로 변환되는데, 크기가 작은 타입이 큰 타입으로 변환되므로
// intValue가 double으로 변환되어 연산을 수행한다.
```

### 강제 타입 변환 Casting

: 큰 크기 타입의 값을 작은 크기 타입에 저장할 때는 Promotion이 되지 않으므로 강제적으로 타입을 변환하는 Casting을 해줘야한다!

**방법 : (타입) 값** (byte) Value

**원리 : 큰 타입을 작은 타입의 단위로 쪼개고, 끝에 한 부분만 작은 타입으로 강제 변환하는 것!**

**모든 메모리가 작은 타입으로 변환되는 것이 아니다!**

```java
int intValue = 10309770;
byte byteValue = (byte) intValue;
```

![Untitled](CHAPTER%202%20%20df51d/Untitled.png)

👉 원래 값은 10309770으로 4바이트에 나눠져서 저장되어 있지만 byte로 Casting하게 되면 마지막 바이트만 가져와서 값이 10이 된다!!

→ 이렇게 값이 보존되지 않는 경우는 Casting을 하는 의미가 없다고 본다!

👉  따라서, 원래 데이터가 작은 크기 타입 범위 안에 있었다면 값이 보존되지만, 작은 크기 타입 범위를 넘는 값이면 값이 보존되지 않는다.

**∴ Casting을 하더라도 값은 보존되도록 하는 것이 중요하다!!**

따라서, 변환할 값이 작은 크기 타입의 범위에 있는지 확인하고 변환하는 것이 좋다!

이를 확인하는 방법으로는, 타입에 따른 최대값, 최소값을 자바에서 기본 상수로 제공하므로 이를 활용하여 조건문으로 실행한다.

ex) Byte.MAX_VALUE / Byte.MIN_VALUE / Integer.MAX_VALUE / Integer.MIN_VALUE / Double.MAX_VALUE / Double.MIN_VALUE / ...

```java
// double을 int로 변환하려고 할 때
double A = ~~~;

// A가 int의 최소값보다 작거나 int의 최대값보다 크다면 변환하지 말고, 아니라면 변환
if (A < integer.MIN_VALUE || A > integer.MAX_VALUE) {
	System.out.println("integer 타입으로 변환할 수 없습니다!");
} else {
	int i = (int) A;
}
```

---