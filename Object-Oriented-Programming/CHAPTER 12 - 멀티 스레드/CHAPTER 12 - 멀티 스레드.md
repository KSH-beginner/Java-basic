# CHAPTER 12 : 멀티 스레드

## 12.1 멀티 스레드 개념

### 프로세스(process)

- 실행중인 하나의 프로그램을 말한다.
    - 하나의 프로그램은 다중 프로세스를 만들기도 한다.
    
    ex) 인터넷 브라우저(크롬)를 여러 개 생성 : 다중 프로세스
    

### 스레드(thread)

- 코드의 실행 흐름!

### 멀티 태스킹 (multi tasking)

- 두가지 이상의 작업을 동시에 처리하는 것
- 멀티 태스킹을 구현하는 방법 2가지
    - 멀티 프로세스 : 독립적으로 프로그램들을 실행하고 여러가지 작업 처리
    - 멀티 스레드 : **한 개의 프로그램**을 실행하고 내부적으로 여러가지 작업 처리
    
    ![Untitled](CHAPTER%2012%200c2c9/Untitled.png)
    
    👉 프로세스1, 4 : 하나의 프로그램 안에 스레드가 여러 개인 멀티 스레드!
    
    멀티 스레드의 대표적인 예 : 메신저
    
    👉 채팅(스레드1)을 하면서 파일 전송(스레드2) 동시에 실행 가능
    

### 메인(main) 스레드

- 자바의 main() 메소드를 실행하는 스레드가 메인 스레드이다! → JVM이 메인 스레드를 생성
- 모든 자바 프로그램은 메인 스레드가 main() 메소드를 실행하면서 시작된다.
- main() 메소드의 첫 코드부터 아래로 순차적으로 실행한다.
- main() 메소드의 마지막 코드를 실행하거나, return 문을 만나면 실행이 종료된다.

- 메인 스레드는 작업 스레드들을 만들어서 병렬로 코드들을 실행할 수 있다.

→ JVM이 메인 스레드를 만들고, 메인 스레드가 작업 스레드들을 만든다.

→ 즉, 멀티 스레드를 생성해서 멀티 태스킹을 수행한다!

![Untitled](CHAPTER%2012%200c2c9/Untitled%201.png)

- 프로세스의 종료 시점
    - 싱글 스레드 : 메인 스레드가 종료하면 프로세스도 종료된다!
    - 멀티 스레드 : 실행중인 스레드가 하나라도 있다면, 메인 스레드가 종료돼도 프로세스는 종료되지 않는다!
    

## 12.2 작업 스레드 생성과 실행

### 몇 개의 작업을 병렬로 실행할지 결정

ex) 메인 스레드 - 프로그램 시작 시 생성

해야할 작업 : 네트워킹 / 드로잉

👉 작업 스레드 1을 생성해서 네트워킹 작업 / 작업 스레드 2를 생성해서 드로잉 작업 : 작업 스레드 총 2개 생성

### 작업 스레드 생성 방법

- Thread 클래스로부터 직접 생성
- Thread 하위 클래스로부터 생성

### Thread 클래스로부터 직접 생성

- 동시에 실행해야할 작업이 있다면 그 작업을 처리할 클래스 생성
- 동시에 실행한다는 의미로 `Runnable` 이라는 인터페이스 구현
- `Runnable` 구현 의미 : 작업 스레드가 실행할 수 있는 클래스로 만들겠다!
- Runnable 인터페이스에는 `run()` 메소드가 있는데, 이 클래스를 재정의해야한다!
- run() 메소드 안에 작업 스레드가 실행할 작업 코드를 넣어야 한다! (메인 스레드가 실행될 동안 동시에 실행될 코드)

```java
class Task implements Runnable {
	public void run() {
		// 스레드가 실행할 코드
	}
}
```

- **스레드로부터 run 메소드를 실행하게 만드는 방법 3가지**
    - 첫번째 방법
    
    ```java
    Runnable task = new Task();
    Thread thread = new Thread(task);
    ```
    
    👉 작업 클래스 객체 생성 후 Runnable 타입(인터페이스)으로 대입 후, Thread 타입 객체를 만들어서 작업 클래스 객체 task를 생성자 매개 값으로 주기
    
    Thread 객체가 task를 실행하면서 Task 클래스 안의 run()을 실행하면서 작업한다.
    
    - 두번째 방법
    
    ```java
    Thread thread = new Thread(new Runnable() {
    	@Override
    	public void run() {
    		// 스레드가 실행할 코드
    	}
    });
    ```
    
    👉 Thread 객체를 생성할 때 생성자 매개값에 직접 익명 객체를 만들어서 작성한다. (첫번째 방법과 원리가 동일하다! 방법이 다를뿐!)
    
    Thread 생성자의 매개값으로 들어갈 객체를 따로 정의하지 않고 매개값 안에 익명 객체로 그 안에서 직접 정의하여 사용하는 방법
    
    - 세번째 방법 : 람다식 이용
    
    ```java
    Thread thread = new Thread( () -> {
    	// 스레드가 실행할 코드;
    }
    ```
    
    👉 람다식은 Runnable이라는 객체로 생성된다. → 두번째 방법과 같다.
    
    💡 첫번째, 두번째, 세번째 중 하나의 방법을 선택하여 Thread 객체를 만들어서 `thread.start()` 를 호출하면 스레드가 실행되는데 이때 `run()` 메소드를 호출하여 그 안의 코드를 실행한다! 
    

Ex) 비프음 5번 실행함과 동시에 “띵” 문자열을 5번 출력하기

```java
public class BeepPrintExample1 {
    public static void main(String[] args) {
        // 비프음 5번 반복해서 소리나게 하는 작업 : Toolkit의 beep() 메소드 사용
        Toolkit toolkit = Toolkit.getDefaultToolkit();
        for (int i = 0; i < 5; i++) {
            toolkit.beep();
            // 0.5초 쉬었다가 실행
            try {Thread.sleep(500)} catch(Exception e) {}
        }
        
        // "띵" 문자열을 5번 출력하는 작업
        for (int i = 0; i < 5; i++) {
            System.out.println("띵");
            try {Thread.sleep(500)} catch(Exception e) {}
        }
    }
}
```

👉 이렇게 하게 되면 메인 스레드가 실행되면서 비프음이 5번 반복이 된 후 “띵” 문자열을 5번 출력한다.  → 싱글 스레드

우리가 원하는 것은 비프음이 5번 반복할 때 “띵” 문자열을 5번 출력하는 것이므로 이때

멀티 스레드를 사용해야 한다!

👉 비프음 5번 반복해서 소리나게 하는 작업을 별도의 작업 클래스로 만들기

**※ 작업 클래스를 만들 때는 반드시 `Runnable` 인터페이스를 구현해야하고, `run()` 메소드를 재정의해야한다.**

```java
public class BeepTask implements Runnable {
    @Override
    public void run() {
        // 비프음 5번 반복해서 소리나게 하는 작업 : Toolkit의 beep() 메소드 사용
        Toolkit toolkit = Toolkit.getDefaultToolkit();
        for (int i = 0; i < 5; i++) {
            toolkit.beep();
            // 0.5초 쉬었다가 실행
            try {Thread.sleep(500)} catch(Exception e) {}
        }
    }
}
```

- 실행 클래스에서 Thread 객체의 생성자로 작업 클래스를 주기!

```java
public class BeepPrintExample2 {
    public static void main(String[] args) {
        Runnable beepTask = new BeepTask();
        Thread thread = new Thread(beepTask);
        thread.start();

        // "띵" 문자열을 5번 출력하는 작업
        for (int i = 0; i < 5; i++) {
            System.out.println("띵");
            try {Thread.sleep(500)} catch(Exception e) {}
        }
    }
}
```

👉 Thread 객체의 생성자로 작업 클래스 객체 `beepTask` 를 주고, `thread.start()` 를 하면 작업 클래스인 BeepTask의 run() 메소드가 실행된다!

💡 이때 프로그램의 실행 순서

1. 메인 스레드 실행
2. Thread 객체를 이용해서 thread 생성
3. `thread.start()` 로 스레드 작업 시작
4. 스레드 작업 시작과 동시에 “띵” 문자열을 5번 출력하는 작업 메인 스레드에서 시작

👉 `thread.start()` 를 하는 시점과 “띵” 문자열 5번 출력 작업 시점이 같아서 비프음과 동시에 “띵” 문자열이 출력된다!!

👉 **이것이 바로 멀티 스레드!!**

### Thread 하위 클래스로부터 작업 클래스 생성

👉 Thread 하위 클래스로부터 작업 클래스를 생성하면 Thread를 상속받고 있기 때문에 Thread 클래스로부터 직접 생성할 때 Runnable 인터페이스를 구현해야 했던 것과 달리 구현할 필요가 없다!

```java
public class WorkerThread extends Thread {
    @Override
    public void run() {
        // 스레드가 실행할 코드
    }
}
Thread thread = new WorkerThread();
```

👉 작업 클래스 생성 시 따로 `Runnable` 인터페이스를 구현하지 않아도 되고, 객체 생성 시 따로 `Runnable` 인터페이스 객체를 만들지 않아도 된다.

- 두번째 방법은 더 간단하게 작성 가능

```java
Thread thread = new Thread() {
	public void run() {
			// 스레드가 실행할 코드;
	}
}
```

👉 익명 객체를 사용하면 Thread를 상속하는 자식 클래스의 내용을 간단하게 객체 생성 시 작성 가능하다!

마지막으로, `thread.start()` 실행하면 작업 스레드 실행!

Ex) 비프음 5번 실행함과 동시에 “띵” 문자열을 5번 출력하기(예제는 위와 똑같다!)

```java
// Thread를 상속받는 작업 클래스 생성
public class BeepThread extends Thread {
    @Override
    public void run() {
        // 비프음 5번 반복해서 소리나게 하는 작업 : Toolkit의 beep() 메소드 사용
        Toolkit toolkit = Toolkit.getDefaultToolkit();
        for (int i = 0; i < 5; i++) {
            toolkit.beep();
            // 0.5초 쉬었다가 실행
            try {Thread.sleep(500)} catch(Exception e) {}
				}
		}
}
```

- 실행 클래스

```java
public class BeepPrintExample3 {
	public static void main(String[] args) {
		Thread thread = new BeepThread();
		thread.start();

		// "띵" 문자열을 5번 출력하는 작업
        for (int i = 0; i < 5; i++) {
            System.out.println("띵");
            try {Thread.sleep(500)} catch(Exception e) {}
        }
	}
}
```

### 스레드의 이름

- 메인 스레드 이름 : `main`
- 작업 스레드 이름 : `Thread-n`

`thread.getName();` : 작업 스레드 이름을 얻을 수 있다.

- 작업 스레드의 이름 변경 : `setName()`

`thread.setName("스레드 이름");` : default 이름이 변경된다.

- 코드를 실행하는 도중에 스레드의 이름을 가져올 때→ 스레드의 인스턴스를 얻어야 한다.

: Thread의 정적 메소드인 `currentThread()` 사용

`Thread thread = Thread.currentThread();` 이렇게 `currentThread()` 로 객체 생성 후

`thread.getName();` 으로 작업중인 스레드 이름 얻기

## 12.3 스레드 우선 순위

- 여러 스레드가 있을 때 어떤 것을 먼저 실행할 지?

### 동시성과 병렬성

- 동시성(Concurrency)
    - 멀티 작업을 위해 하나의 코어에서 멀티 스레드가 번갈아가며 실행하는 성질
    
    ![Untitled](CHAPTER%2012%200c2c9/Untitled%202.png)
    
    👉 하나의 스레드가 실행될 때 다른 스레드는 대기 상태로, 동시에 실행될 수 없다.
    
    하지만, 스레드1에서 스레드2로 넘어가는 시간이 엄청 빠르기 때문에 동시에 실행되는 것처럼 보인다.
    
- 병렬성(Parallelism)
    - 멀티 작업을 위해 멀티 코어에서 개별 스레드를 동시에 실행하는 성질
    
    ![Untitled](CHAPTER%2012%200c2c9/Untitled%203.png)
    

### 스레드 스케쥴링

- 스레드의 개수가 코어의 수보다 많을 경우
    - 스레드를 어떤 순서로, 동시성으로 실행할 것인가를 결정 : 스레드 스케쥴링
    - 스레드 스케쥴링에 의해 스레드들은 번갈아가면서 그들의 run() 메소드를 조금씩 실행
    
    ![Untitled](CHAPTER%2012%200c2c9/Untitled%204.png)
    
- 자바의 스레드 스케쥴링
    - 우선 순위 방식(Priority)과 순환 할당 방식(Round-Robin)을 사용한다.
        - 우선 순위 방식 : 코드로 제어 가능
        
        👉 우선 순위가 높은 스레드가 실행 상태를 더 많이 가지도록 스케쥴링 하는 방식
        
        - 순환 할당 방식 : 코드로 제어할 수 없다
        
        👉 시간 할당량(Time Slice)을 정해서 하나의 스레드를 정해진 시간만큼 실행하는 방식
        
    

### 스레드 우선 순위

- 스레드들이 동시성을 가질 경우 우선적으로 실행할 수 있는 순위
- 우선 순위는 1(낮음)에서부터 10(높음)까지 부여
    - 모든 스레드들은 default로 5의 우선 순위를 가진다.
- 우선 순위 변경 방법
    - `thread.setPriority(우선순위 숫자);` : 직접 변경
    - 우선 순위 상수로 변경
    
    ```java
    thread.setPriority(Thread.MAX_PRIORITY);  // 가장 높은 수인 10
    thread.setPriority(Thread.NORM_PRIORITY);  // default 값인 5
    thread.setPriority(Thread.MIN_PRIORITY);// 가장 낮은 수인 1
    ```
    
- 우선 순위 효과
    - 싱글 코어 경우
        - 우선 순위가 높은 스레드가 실행 기회를 더 많이 가져서 우선 순위가 낮은 스레드보다 계산 작업을 더 빨리 끝낸다.
    
    - 멀티 코어의 경우
        - 쿼드 코어(코어 4개)의 경우는 4개의 스레드가 병렬성으로 실행될 수 있기 때문에 4개 이하의 스레드를 실행할 경우에는 우선 순위 방식의 영향이 별로 없다.
        - 최소한 5개 이상의 스레드가 실행되어야 우선 순위 방식이 효과가 있다.
        

## 12.4 동기화 메소드와 동기화 블록 : 객체 잠금!

### 공유 객체를 사용할 때 주의할 점

- 멀티 스레드가 하나의 객체를 공유해서 생기는 오류

![Untitled](CHAPTER%2012%200c2c9/Untitled%205.png)

👉 유저1 스레드와 유저2 스레드는 Calculator 객체를 공유하고 memory 필드를 사용한다.

1. 유저1 스레드 실행 : `memory = 100`
2. 유저1 스레드 2초간 일시정지
3. 유저2 스레드 실행 : `memory = 50`
4. 유저2 스레드 2초간 일시정지
5. 유저1 스레드 일시정지 끝나고 memory 출력

👉 유저1 스레드는 memory에 100을 넣었는데 50이 출력된다!!

💡 멀티 스레드에서 하나의 객체를 공유하면 스레드1에서 수정한 값이 스레드2에도 영향을 미친다.

### 동기화 메소드 및 동기화 블록 : synchronized 메소드, 블록

- 공유 객체 사용 시 문제점을 일부 해소해주는 기능!
- 단 하나의 스레드만 실행할 수 있는 메소드 또는 블록을 말한다.
- 다른 스레드는 메소드나 블록의 실행이 끝날 때까지 대기해야 한다.

![Untitled](CHAPTER%2012%200c2c9/Untitled%206.png)

👉 스레드-1, 스레드-2는 하나의 객체를 공유하고 그 객체 안에는 동기화 메소드, 블록, 일반 메소드가 있다.

👉 스레드-1이 동기화 메소드, 동기화 블록, 일반 메소드를 사용하고 있을 때, 스레드-2는 동기화 메소드, 동기화 블록을 사용할 수 없고 일반 메소드만 사용 가능하다!

💡 동기화 메소드 or 동기화 블록을 스레드-1이 사용하고 있다면, 다른 스레드는 동기화 메소드 and 동기화 블록을 사용할 수 없다! 

객체를 잠그는 역할!!

1. 유저1 스레드 동기화 메소드 or 동기화 블록 실행 : `memory = 100`
2. 유저1 스레드 2초간 일시정지

👉 2초간 일시정지를 해도 동기화 메소드 or 동기화 블록을 실행했기 때문에 다른 스레드가 객체의 동기화 메소드 or 동기화 블록에 접근할 수 없다.

 3.  유저1 스레드 출력 : 저장한대로 100 출력!

1. 유저1 스레드 동기화 메소드, 블록 사용 종료
2. 유저2 스레드 실행

💡 동기화 메소드, 블록 : 공유 객체를 사용할 때 한 번에 하나의 스레드만 사용할 수 있게끔 해주는 것!

- 동기화 메소드 생성 : `synchronized` 붙이기

```java
public synchronized void method() {
	임계 영역; // 단 하나의 스레드만 실행
}
```

👉 단 하나의 스레드만 실행되는 영역 : `임계 영역`

- 동기화 블록 : 메소드에 `synchronized` 를 붙이는 것이 아니라, 메소드의 일부분만 `synchronized` 를 붙여 임계영역으로 만드는 것!

```java
public void method() {
	// 여러 스레드가 실행 가능한 영역

	synchronized(공유객체) {
		임계 영역 // 단 하나의 스레드만 실행
	}

	// 여러 스레드가 실행 가능한 영역
}
```

👉 공유 객체 = 잠금 객체가 된다. 보통 `this` 를 써서 이 메소드를 호출하는 객체를 잠근다.

## 12.5 스레드 상태

![Untitled](CHAPTER%2012%200c2c9/Untitled%207.png)

![Untitled](CHAPTER%2012%200c2c9/Untitled%208.png)

👉 스레드의 상태를 열거 상수로 나타낸다.

`BLOCKED` : 다른 스레드가 동기화 메소드, 블록을 사용하고 있을 때의 상태, 다른 스레드의 동기화 메소드, 블록 사용이 끝나면 `RUNNABLE` 실행 대기 상태가 된다.

`WAITING` : Object 클래스의 wait 메소드를 호출하면 스레드는 WATING 상태가 되고, 다른 스레드가 `notify()` 메소드를 호출해야 실행 대기 `RUNNABLE` 로 갈 수 있다.

`TIMED_WAITING` : 스레드의 sleep() 메소드를 이용하여 시간을 줄 수 있는데, 그 시간동안 스레드는 `TIMED_WAITING` 일시 정지 상태가 된다. 주어진 시간이 지나면 `RUNNABLE` 로 간다.

- Thread.State 열거타입의 getState() 메소드로 스레드의 현재 상태를 얻을 수 있다.

## 12.6 스레드 상태 제어

### 상태 제어

- 실행 중인 스레드의 상태를 변경하는 것을 말한다.
- 상태 변화를 가져오는 메소드의 종류

![Untitled](CHAPTER%2012%200c2c9/Untitled%209.png)

### 주어진 시간동안 일시정지 - sleep()

```java
try {
	Thread.sleep(1000);
} catch(InterruptedException e) {
	// interrupt() 메소드가 호출되면 실행
}
```

- 주어진 시간(밀리세컨드 (1/1000)단위) 동안 일시정지 상태로 있는다.
- 일시정지 상태에서 `interrupt()` 메소드가 호출되면 `InterruptedException` 예외 발생

### 다른 스레드에게 실행 양보 - yield()

- 무의미한 반복을 하지 않고 다른 스레드에게 실행을 양보할 때 실행

![Untitled](CHAPTER%2012%200c2c9/Untitled%2010.png)

👉 스레드 1이 실행 중일때 스레드 1에서 `yield()` 를 호출하면, 스레드 1과 동일 또는 높은 우선 순위를 가지는 스레드가 실행된다.

- 스레드 1의 run() 메소드

```java
public void run() {
	while(true) {
	// work 변수가 true일 경우 실행
		if(work) {
			System.out.println("ThreadA 작업 내용");
		}
	}
}
```

👉 이때 work 변수가 false일 경우 아무 코드도 실행하지 않고 무의미한 반복을 하기 때문에, else로 `yield()` 를 줘서 다른 스레드에게 실행을 양보하는 것이 좋다!

```java
public void run() {
	while(true) {
	// work 변수가 true일 경우 실행
		if(work) {
			System.out.println("ThreadA 작업 내용");
		} else {
			Thread.yield();
		}
	}
}
```

### 다른 스레드의 종료를 기다림 - join()

![Untitled](CHAPTER%2012%200c2c9/Untitled%2011.png)

👉 스레드 A에서 `threadB.start()` 로 스레드 B를 실행시키고, `threadB.join()` 을 하게 되면 일시정지가 되는데, 이때 스레드 B가 일시정지가 되는 것이 아니고 스레드 B를 실행시킨 스레드 A가 일시정지 되는 것이다!

👉 스레드 B가 run() 메소드를 종료할 때까지 스레드 A가 일시정지 되는 것이다. 스레드 B가 run() 메소드를 종료하면 스레드 A는 일시정지에서 벗어나서 실행 대기 상태가 된다.

- 스레드 B가 계산 작업을 하는 역할, 스레드 A가 그 결과를 받아서 작업을 하는 구조에서 계산 작업하는 스레드 B의 작업이 끝날 때까지 A는 일시정지하고 B에서 계산이 끝나면 결과 값을 받아서 그 후에 A 작업을 마저 진행하는 구조일 때 주로 사용된다.

EX) 1부터 100의 합을 계산해주는 sumThread, sumThread가 계산한 값을 받아서 출력하는 메인 스레드가 있다고 할 때, 메인 스레드에서 join을 하지 않게되면 출력 값이 이상하게 나올 수 있다.

∵ 메인 스레드 실행 도중에 계산이 끝나지 않았을 경우에 계산 도중의 값을 출력하기 때문에!

따라서, 메인 스레드에 `join()`을 추가해서 계산이 끝났을 때 출력하도록 해야한다!

### 스레드간 협업 - wait() / notify() / notifyAll()

- 이러한 메소드들은 스레드가 갖고 있는 메소드가 아니고, Object의 메소드들이다.

👉 모든 객체가 가지고 있는 메소드이다.

- 동기화 메소드 또는 동기화 블록에서만 호출이 가능하다!!

- `wait()`
    - 호출한 스레드는 일시 정지가 된다.
    - 다른 스레드가 `notify()` or `notifyAll()` 을 호출해야 실행 대기 상태로 갈 수 있다.
    
- `wait(long timeout) / wait(long timeout, int nanos)`
    - notify()가 호출되지 않아도 매개값으로 지정한 시간이 지나면 스레드가 자동으로 실행 대기 상태가 된다! → `sleep()` 과 비슷하다
    - 지정한 시간 전에 다른 스레드에서 `notify()` or `notifyAll()` 을 호출할 경우 지정한 시간이 되지 않았지만 그 즉시 실행 대기 상태로 된다.
    
    👉 이것이 `sleep()`과 다른점이다! `sleep()`은 `notify()` or `notifyAll()` 상관없이 무조건 지정된 시간이 지나야 실행 대기 상태가 된다.
    
- 두 개의 스레드가 교대로 번갈아가며 실행해야 할 경우에 이 메소드들을 주로 사용한다!

![Untitled](CHAPTER%2012%200c2c9/Untitled%2012.png)

👉 생산자 스레드 : 공유 객체에 데이터를 저장 / 소비자 스레드 : 공유 객체의 데이터를 읽기

👉 공유 객체를 두 스레드가 번갈아가면서 사용!

👉 이러한 상황에서 사용하기 때문에 **동기화 메소드, 동기화 블록에서만 호출이 가능하다!**

1. 현재 소비자 스레드가 `wait()` 메소드를 호출해서 일시 정지 상태
2. 생산자 스레드가 공유 객체에 데이터를 저장
3. 생산자 스레드에서 데이터를 저장한 후 `“데이터를 저장했으니 소비자가 가져가세요!"` 라는 의미로 `notify()` 를 호출해서 일시정지된 소비자 스레드를 실행 대기로 만듦
4. 그 후 생산자 스레드 자기 자신은 `wait()` 로 일시 정지를 해서 소비자 스레드가 공유 객체의 데이터를 다 이용할 때까지 일시 정지함
5. 소비자 스레드가 실행 대기 상태로 되어서 실행해서 공유 객체의 데이터를 읽음
6. 소비자 스레드가 데이터를 읽은 후 `"데이터를 다 읽었습니다!"` 라는 의미로 `notify()` 를 호출해서 일시정지된 생산자 스레드를 실행 대기로 만듦
7. 그 후 소비자 스레드 자기 자신은 `wait()` 로 일시 정지를 해서 생산자 스레드가 공유 객체에 데이터를 다 저장할 때까지 일시 정지함

👉 이 사이클을 반복한다!!

### 스레드의 안전한 종료 - stop 플래그, interrupt()

- 경우에 따라서, 실행 중인 스레드를 즉시 종료할 필요가 있다. (ex- 음악 듣고 있다가 종료)
- `stop()` 메소드 → 사용 X
    - 스레드를 즉시 종료시킨다.
    - 스레드를 갑자기 종료하게 되면 사용중이던 자원들이 불안전한 상태로 남겨진다.
    - 사용하지 말기!!
    
- 스레드 종료 2가지 방식 : stop 플래그 / interrupt()

- stop 플래그를 이용하는 방법
    - stop 플래그로 run() 메소드의 정상 종료를 유도한다.
    
    ```java
    public class ThreadA extends Thread {
        // stop 플래그 필드 생성 (boolean의 기본값 : false)
    		// stop의 값은 스레드 내부에서도 바꾸지만
        // 스레드를 호출하는 곳에서 ThreadA.stop = true; 이렇게 바꿀 수도 있다.
        private boolean stop;
    
        @Override
        public void run() {
            // stop이 true면 반복문 종료 / false면 반복
           while(!stop) {
               // 스레드가 반복 실행하는 코드
           }
           // 스레드가 사용한 자원 정리
        }
    }
    ```
    
    👉 일시 정지 상태의 스레드는 종료하지 못한다.
    
- `interrupt()` 메소드를 이용하는 방법
    - 일시 정지 상태의 스레드도 종료를 할 수 있다.
    - 일시 정지 상태일 경우 InterruptedException을 발생시킨다.
        
        ![Untitled](CHAPTER%2012%200c2c9/Untitled%2013.png)
        
    
    👉 `threadB.start()` 로 스레드 B를 실행시켰을 때 스레드 B의 run() 메소드 안에 `Thread.sleep(1)` 이 있어서 일시정지 상태가 잠깐 되는데, 이때 스레드 A에 `threadB.interrupt()` 코드가 있기 때문에 일시정지 상태일 때 `InterruptedException` 예외를 발생시켜서 스레드 B는 while문을 빠져나가서 catch문으로 이동해서 빠져나가게 된다!
    
    → 스레드 종료
    
    👉 InterruptedException을 발생시켜서 스레드를 종료시키는 방법인데, 일시 정지 상태에서만 이 예외가 발생하고, 실행대기나 실행 상태에서는 이 예외가 발생하지 않아서 스레드 종료를 시킬 수 없다.
    
- 하지만, 이렇게 `Thread.sleep()` 을 사용해서 강제적으로 일시 정지를 하는 것은 좋지 않다.

👉 이때, 일시 정지 상태로 만들지 않고 while문을 빠져나오는 방법이 있다.

`interrupted()` : Thread의 정적 메소드로, Thread가 interrupt 되었는지를 알 수 있다.

`isInterrupted()` : Thread의 인스턴스 메소드로, Thread가 interrupt 되었는지를 알 수 있다.

```java
boolean status = Thread.interrupted();
boolean status = objThread.isInterrupted(); // objThread : 스레드 객체
```

👉 스레드 A에서 `threadB.interrupt()` 했을 때, 스레드 B에서 interrupt() 메소드가 호출되었다면 `Thread.interrupted()` 메소드 / `objThread.isInterrupted()` 메소드는 true를 리턴한다.

```java
while(true) {
		// 스레드가 interrupt 되었다면 반복문 종료, 아니면 계속 반복
	if (Thread.interrupted()) {
		break;
	}

}
```

## 12.7 데몬 스레드

### 데몬 스레드

- 주 스레드의 작업을 돕는 보조적인 역할을 수행하는 스레드
- 주 스레드가 종료되면 데몬 스레드는 강제적으로 자동 종료
    - 데몬 스레드 ex)
        - 워드 프로세스의 자동 저장 → 워드 프로세스 : 주 스레드 / 자동 저장 : 데몬 스레드
        - 미디어 플레이어의 동영상 및 음악 재생 → 미디어 플레이어 : 주 스레드 / 동영상 및 음악 재생 : 데몬 스레드
        - 가비지 컬렉터 → JVM : 주 스레드 / GC : 데몬 스레드
        
- 데몬 스레드 설정
    - 주 스레드가 데몬이 될 스레드의 `setDaemon(true)` 를 호출한다!
    
    ```java
    public static void main(String[] args) {
        AutoSaveThread thread = new AutoSaveThread();
    		// 메인 스레드에서 AutoSaveThread의 객체에 setDaemon(true) 설정
    		// 메인 스레드 : 주 스레드가 되고, AutoSaveThread : 데몬 스레드가 된다.
        thread.setDaemon(true); 
        thread.start();
        ...
    }
    ```
    
    - 반드시 start() 메소드 호출 전에 `setDaemon(true)` 를 호출해야한다!
        - 그렇지 않으면 IllegalThreadStateException 예외가 발생한다.
        
- 현재 스레드가 데몬 스레드인지 확인하는 방법
    - `isDaemon()` 메소드의 리턴 값이 true면 데몬 스레드이다.

## 12.8 스레드 그룹

### 스레드 그룹

- 관련된 스레드를 묶어서 관리할 목적으로 이용
- 스레드 그룹은 계층적으로 하위 스레드 그룹을 가질 수 있다.

- 자동 생성되는 스레드 그룹
    - system 그룹 : JVM 운영에 필요한 스레드들을 포함
    - system/main 그룹(system의 하위 그룹인 main 그룹) : 메인 스레드를 포함
    
- 스레드는 반드시 하나의 스레드 그룹에 포함
    - 기본적으로 자신을 생성한 스레드와 같은 스레드 그룹에 속하게 된다.
    - 명시적으로 스레드 그룹에 포함시키지 않으면 기본적으로 system/main 그룹에 속한다.

### 스레드 그룹 이름 얻기 : ThreadGroup클래스의 `getThreadGroup()` 메소드 사용

```java
ThreadGroup group = Thread.currentThread.getThreadGroup();
String groupName = group.getName();
```

👉 ThreadGroup 객체 생성 시 `getThreadGroup()` 을 통해 객체를 생성하고 객체에서 `getName()` 을 사용하면 스레드 그룹의 이름을 얻을 수 있다.

### 스레드 그룹 생성

```java
// ThreadGroup 객체 생성자의 매개 값으로 이름을 주는 경우
ThreadGroup tg = new ThreadGroup(String name);

// ThreadGroup 객체 생성자의 매개 값으로 ThreadGroup 클래스로 상위 그룹을 주고, 이름도 주는 경우
// 생성되는 그룹은 매개값의 상위 그룹의 하위 그룹으로 설정된다.
ThreadGroup tg = new ThreadGroup(ThreadGroup parent, String name);
```

- 부모(parent) 그룹을 지정하지 않으면 현재 스레드가 속한 그룹의 하위 그룹으로 생성
- 스레드를 그룹에 명시적으로 포함시키는 방법

```java
// 스레드 생성자의 첫번째 매개값 : 현재 스레드가 속할 ThreadGroup 클래스의 그룹
// 스레드 생성자의 두번째 매개값 : 스레드가 실행할 코드를 가지고 있는 Runnable 객체
Thread t = new Thread(ThreadGroup group, Runnable target);

// 스레드 생성자의 세번째 매개값 : 스레드의 이름
Thread t = new Thread(ThreadGroup group, Runnable target, String name);

// 스레드 생성자의 네번째 매개값 : 스레드가 사용할 Stacksize
Thread t = new Thread(ThreadGroup group, Runnable target, String name, long stacksize);

// 스레드 생성자의 첫번째 매개값 : 현재 스레드가 속할 ThreadGroup 클래스의 그룹
// 스레드 생성자의 두번째 매개값 : 스레드의 이름
// 스레드를 상속해서 만든 경우에 매개값으로 Runnable 객체를 주지 않아도 된다.
Thread t = new Thread(ThreadGroup group, String name); 
```

```java
// 익명객체로 스레드 생성 시, 스레드를 그룹에 명시적으로 포함
Thread thread = new Thread(ThreadGroup group, new Runnable() { ... } 

// 스레드를 상속해서 스레드 생성 시 스레드를 그룹에 명시적으로 포함
class XXXThread extends Thread {
	XXXThread(ThreadGroup threadGroup, String threadName) {
		// 클래스 생성자로 부모 클래스인 Thread에 스레드 그룹, 스레드 이름 매개값으로 선언
		super(threadGroup, threadName); 
	}
}
```

### ThreadGroup이 제공하는 메소드

| 메소드 | 설명 |
| --- | --- |
| 리턴 타입 : int / activeCount() | 현재 그룹 및 하위 그룹에서 활동중인 모든 스레드의 수를 리턴 |
| 리턴 타입 : int / activeGroupCount() | 현재 그룹에서 활동중인 모든 하위 그룹의 수를 리턴  |
| 리턴 타입 : void / checkAccess() | 현재 스레드가 스레드 그룹을 변경할 권한이 있는지 체크,
만약 권한이 없다면 SecurityException 예외를 발생시킨다. |
| 리턴 타입 : void / destory() | 현재 그룹 및 하위 그룹을 모두 삭제한다.
단, 그룹 내에 포함된 모든 스레드들이 종료상태여야 한다. |
| 리턴타입 : boolean / isDestoryed() | 현재 그룹이 삭제되었는지 여부를 리턴 |
| 리턴 타입 : int / getMaxPriority() | 현재 그룹에 있는 스레드의 우선순위 최댓값을 리턴 |
| 리턴 타입 : void / setMaxPriority(int pri) | 현재 그룹에 있는 스레드가 가질 수 있는 최대 우선순위를 설정 |
| 리턴 타입 : String / getName() | 현재 그룹의 이름을 리턴 |
| 리턴 타입 : ThreadGroup / getParent() | 현재 그룹의 부모 그룹을 리턴 |
| 리턴 타입 : boolean /
parentOf(ThreadGroup g) | 현재 그룹이 매개값으로 지정한 스레드 그룹의 부모 그룹인지 여부를 리턴 |
| 리턴 타입 : boolean / isDaemon() | 현재 그룹이 데몬 그룹인지 여부를 리턴 |
| 리턴 타입 : void / 
setDaemon(boolean daemon) | 현재 그룹을 데몬 그룹으로 설정한다. |
| 리턴 타입 : void / list() | 현재 그룹에 포함된 스레드와 하위 그룹에 대한 정보 출력 |
| ★리턴 타입 : void / interrupt()★ | 현재 그룹에 포함된 모든 스레드들을 interrupt 한다. |

👉 interrupt() 메소드가 가장 중요하다! 

👉 그룹 내에 있는 모든 스레드들을 안전하게 종료하고 싶다면 ThreadGroup에서 `interrupt()`를 한 번만 호출해주면 스레드 그룹에 소속되어 있는 모든 스레드가 interrupt가 실행된다.

### 스레드 그룹의 일괄 interrupt()

- 스레드 그룹의 interrupt()를 호출하면 소속된 모든 스레드의 interrupt()가 호출된다!!

![Untitled](CHAPTER%2012%200c2c9/Untitled%2014.png)

## 12.9 스레드 풀