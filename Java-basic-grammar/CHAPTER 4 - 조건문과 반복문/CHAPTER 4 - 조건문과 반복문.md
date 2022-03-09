# CHAPTER 4 : 조건문과 반복문

## 4.2 조건문

### switch문 사용 시 주의할 점

switch문에서 사용할 수 있는 변수의 타입은 정수, 문자, 문자열이다. 소수점이 붙는 실수는 변수로 쓸 수 없다!

## 4.3 반복문

### do-while문

일반적인 while문 같은 경우에는 조건을 먼저 판별하고 그 후에 실행문을 실행한다.

하지만, do-while문은 do 아래 있는 실행문을 먼저 실행한 후 while문의 조건을 판별한다!

**주의할 점 : while문과 달리 while(조건)에 세미롤론(;)을 붙여야 한다!**

① while 문

```java
public class do_whileExample {
	public static void main(String[] args) {
		int i = 10;
		
		while (i != 10) {
			System.out.println(i);
			i++;
		}
	}
}
```

👉 while문의 조건이 맞을 때 실행문을 실행하기 때문에 초기 조건이 안 맞아서 while문이 실행이 되지 않는다!

② do-while문

```java
public class do_whileExample {
	public static void main(String[] args) {
		int i = 10;
		
		do {
			System.out.println(i);
			i++;
		while (i != 10);
		}
	}
}
```

👉 while문과 상황이 똑같지만, do-while문을 사용하면 초기에 실행문을 먼저 실행하기 때문에 i가 11이 되어 조건문으로 들어가므로 무한 루프가 된다.