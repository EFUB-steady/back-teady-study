# Chapter 08 예외처리(exception handling)

## 1.1 프로그램 오류

>- 컴파일 에러 : 컴파일 시에 발생하는 에러
>- 런타임 에러 : 실행 시에 발생하는 에러
>- 논리적 에러 : 실행은 되지만 의도와 다르게 동작하는 것

<br>

실행 시(runtime) 발생할 수 있는 프로그램 오류
- 에러(error) : 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
ex) 메모리 부족(OutOfMemoryError), 스택오버플로우(StackOverFlow) 등
- 예외(exception) : 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류

<br>

## 1.2 예외 클래스의 계층구조
![](https://velog.velcdn.com/images/chlqls/post/acf8f111-e847-4449-9cd5-9781b126cdb9/image.png)

모든 예외의 최고 조상은 `Exception`이다.

<br>

![](https://velog.velcdn.com/images/chlqls/post/f85e870c-e3a3-46fc-acb5-21be223ad696/image.png)

`Exception클래스`와 `RuntimeException클래스` 중심의 상속계층도

<br>

## 1.3 예외처리하기(try-catch문)
>- 정의 : 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성하는 것
>- 목적 : 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것

<br>

```java
try{
	// 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception e1) {
	//Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch (Exception e2) {
	// Exception2가 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
...
} catch (Exception eN) {
	//ExceptionN이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
}
```

- **하나의 try블럭** 다음에는  **하나 이상의 catch블럭**이 올 수 있음
- 여러 개의 catch블럭 중 발생한 예외의 종류와 일치하는 **단 한 개의 catch블럭만 수행**됨
- 발생한 예외의 종류와 일치하는 catch블럭이 없으면 예외 처리되지 않음

<br>

## 1.4 try-catch문에서의 흐름
###  - try블럭 내에서 예외가 발생하지 않은 경우 
 1. catch블럭을 거치지 않고  전체 try-catch문을 빠져나가서 수행을 계속한다.

```java
public class ExceptionEx4 {

	public static void main(String[] args) {
		
		System.out.println(1);
		System.out.println(2);
		
		try {
			System.out.println(3);
			System.out.println(4);
		} catch (Exception e) { //예외 발생하지 않음
			System.out.println(5); //실행되지 않음
		}
		System.out.println(6); //언제나 실행
	}
}

```

![](https://velog.velcdn.com/images/chlqls/post/bdbfcc35-3052-4571-9746-640185e3e975/image.png)

<br>

### -  try블럭 내에서 예외가 발생한 경우
1. 발생한 예외와 일치하는 catch블럭이 있는지 확인
2. 일치하는 catch블럭을 찾게 되면, 그 catch블럭 내의 문장들을 수행하고 전체 try-catch문을 빠져나가서 그 다음 문장을 계속해서 수행한다. 만일 일치하는 catch블럭이 찾지 못하면, 예외는 처리되지 못한다

<br>

```java
public class ExceptionEx5 {

	public static void main(String[] args) {
		
		System.out.println(1);
		System.out.println(2);
		
		try {
			System.out.println(3);
			System.out.println(0/0); //예외 발생
			System.out.println(4); //실행되지 않음
		} catch (ArithmeticException ae) {
			System.out.println(5); //실행
		}
		System.out.println(6); //언제나 실행
	}
}

```
![](https://velog.velcdn.com/images/chlqls/post/53b7b531-1e56-4949-8f03-e2786d3cb2d5/image.png)

<br>

## 1.5 예외의 발생과 catch블럭

> - `printStackTrace()` : 예외발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.
> - `getMessage()` : 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

<br>

```java
public class ExceptionEx8 {

	public static void main(String[] args) {
		System.out.println(1);
		System.out.println(2);
		try {
			System.out.println(3);
			System.out.println(0/0); //예외 발생
			System.out.println(4); //실행되지 않음
		} catch (ArithmeticException ae) {
			ae.printStackTrace();
			System.out.println("예외메시지: " + ae.getMessage());
		}
		System.out.println("6");
	}
}

```
![](https://velog.velcdn.com/images/chlqls/post/a325c2e7-132e-43c6-b5a2-80b4ef76e0f4/image.png)

<br>

## 1.6 예외 발생시키기
> **1. 연산자 `new`를 이용해서 발생시키려는 예외 클래스의 캑체 생성**
> 
> **2. 키워드 `throw`를 이용해서 예외 발생**

<br>

```java
public class ExceptionEx6 {

	public static void main(String[] args) {
		
		try {
			Exception e = new Exception("고의로 발생시켰음"); // 1번
			throw e; // 2번
			// throw new Exception("고의로 발생시켰음"); //위의 두 줄을 한 줄로 줄여 쓸 수 있음
		} catch(Exception e) {
			System.out.println("에러 메시지: " + e.getMessage());
			e.printStackTrace();
		}
		System.out.println("프로그램이 정상 종료되었음");
	}
}

```
![](https://velog.velcdn.com/images/chlqls/post/320b2145-2371-4a45-be63-de1b4b4ea4ee/image.png)

<br>

> - unchecked예외 : 컴파일러가 예외처리를 확인하지 않는 RuntimeException클래스들
> - checked예외: 컴파일러가 예외처리를 확인하는 Exception클래스들

<br>

## 1.7 메서드에 예외 선언하기
>- 메서드의 선언부에 키워드 `throws`를 사용해서 메서드 내에서 발생할 수 있는 예외 적기
>- 예외가 여러 개일 경우 쉼표(,)로 구분

<br>

```java
void mehtod() throws Exception1, Exception2, .... ExceptionN {
	//메서드의 내용
}
```

<br>

**예시1. `method1()`에서 예외처리**
- 에외가 발생한 메서드(method1)에서 처리하기 때문에 호출한 메서드에서는 예외 발생 사실을 모름
```java
public class ExceptionEx13 {

	public static void main(String[] args) {
		method1();
	}
	
	static void method1() {
		try {
			throw new Exception();
		} catch(Exception e) {
			System.out.println("method1메서드에서 예외처리되었습니다.");
			e.printStackTrace();
		}
	}
}

```

<br>

**예시2. `main메서드`에서 예외처리**
- `method1()`에서 예외를 선언하여 자신을 호출하는 메서드(main메서드)에 예외 전달
- 호출한 메서드(main메서드)에서는 try-catch문으로 예외처리
- `method1()`을 호출한 라인에서 예외가 발생한 것으로 간주

```java
public class ExceptionEx14 {

	public static void main(String[] args) {
		try {
			method1();
		} catch(Exception e) {
			System.out.println("main메서드에서 예외가 처리되었습니다.");
			e.printStackTrace();
		}
		
	}
	
	static void method1() throws Exception {
			throw new Exception();
	}
}
```

<br>

## 1.8 finally블럭
>- 사용 목적 : 에외의 발생여부에 상관없이 실행되어야할 코드를 포함
>- 실행 순서
>	- (예외 발생시) try -> catch -> finally
>	- (에외 발생하지 않을 시) try -> finally
    

<br>

```java
try {
	// 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception e1) {
	// 예외처리를 위한 문장을 적는다.
} finally {
	// 예외의 발생여부에 관계없이 항상 수행되어야하는 문장들을 넣는다.
	//finally블러은 tru-catch문의 맨 마지막에 위치해야한다.
}
```

<br>

## 1.9  자동 자원 반환(try-with-resouces문)
```java

public class TryWithResouceEx {

	public static void main(String[] args) {

		try(CloseableResource cr = new CloseableResource()) { //괄호 안에 객체 생성
			cr.exceptionWork(false); //예외 발생하지 않음
		} catch(WorkException e) {
			e.printStackTrace();
		} catch(CloseException e) {
			e.printStackTrace();
		}
		System.out.println();
		
		try(CloseableResource cr = new CloseableResource()) {
			cr.exceptionWork(true); //예외 발생
		} catch(WorkException e) {
			e.printStackTrace();
		} catch(CloseException e) {
			e.printStackTrace();
		}
		System.out.println();
	}

}

class CloseableResource implements AutoCloseable {
	public void exceptionWork(boolean exception) throws WorkException {
		System.out.println("exceptionWork(" + exception + ")가 호출됨");
		
		if(exception)
			throw new WorkException("WorkException발생!!!");
	}
	
	public void close() throws CloseException {
		System.out.println("clode()가 호풀됨");
		throw new CloseException("CloseException발생!!!");
	}
}

class WorkException extends Exception {
	WorkException(String msg) {
		super(msg);
	}
}

class CloseException extends Exception {
	CloseException(String msg) {
		super(msg);
	}
}

```

![](https://velog.velcdn.com/images/chlqls/post/87e1f424-74e8-4d48-b3f8-cbf5c664ad4e/image.png)

- try-with-resouces문의 괄호 안에서 객체를 생성하면, 이 객체는 따로 close() 호출 없이 try블럭을 벗어나는 순간 자동적으로 `close()`가 호출됨
- 단, 클래스가 `AutoCloseable`이라는 인터페이스 구현해야 함.

```java
public interface AutoCloseable {
	void close() throws Exception;
}
```


<br>

## 1.10 사용자정의 예외 만들기
>- 보통 `Exception클래스` 또는 `RuntimeException클래스`로부터 상속받아 새로운 예외 클래스 정의
>- 새로운 예외 클래스를 만들기보다 ***기존의 예외클래스를 활용***하는 것을 추천
>- 요즘은 Exception보다 ***`RuntimeException`을 상속받아서 작성***하는 추세


<br>

```java
class MyException extends Exception {
	MyException(String msg) { //문자열을 매개변수로 받는 생성자
    	super(msg); //조상인 Exception클래스의 생성자 호출
	}
}
```
- Exception클래스와 같이 생성 시에 String값을 받아서 메시지로 저장하려면 위와 같이 **생성자를 추가**해야 함

<br>

## 1.11 예외 되던지기(exception re-throwing)

> - 하나의 예외에 대해 예외가 발생한 메서드와 호출한 메서드, ***양 쪽에서 처리***
>- 예외를 처리한 후 인위적으로 ***다시 예외 발생***시키는 방법
>- 주의할 점 : 예외가 발생할 메서드에서 ***try-catch문***으로 예외처리와 동시에 메서드의 선언부에는 발생할 예외를 ***throws***에 지정해줘야 함

```java
public class ExceptionEx17 {

	public static void main(String[] args) {

		try {
			method1();
		} catch (Exception e) {
			System.out.println("main메서드에서 예외가 처리되었습니다.");
		}
	}

	static void method1() throws Exception {
		try {
			throw new Exception();			
		} catch (Exception e) {
			System.out.println("method1메서드에서 예외가 처리되었습니다.");
			throw e; // 다시 예외 발생
		}
	}
}
```

![](https://velog.velcdn.com/images/chlqls/post/c8a0f31c-5247-453c-8b2d-8bc9590f1464/image.png)

<br>

## 1.12 연결된 예외(chained exception)
>- 한 예외가 다른 예외를 발생시키는 경우
>- 예외 A가 예외 B를 발생키셨다면 A를 B의 ***'원인 예외(cause exception)'*** 라고 한다
>- 사용 이유
>    1. 여러가지 예외를 하나의 큰 분류의 예외로 묶어서 다루기 위해
>    2. checked예외를 **unchecked예외**로 바꿀 수 있도록 하기 위해

<br>

**예시1 - 사용 이유 1번의 예**
```java
try {
	startInstall(); //SpaceException 발생
    copyDiles();
} catch (SpaceException e) {
	InstallException ie = new InstallException("설치 중 예외 발생"); //예외 생성
    ie.initCause(e); // InstallException의 원인 예외를 SpaceException으로 지정
    throw ie; // InstallException을 발생시킴
} catch() {
...
```

- `initCause()`는 Exception클래스의 조상인 Throwable클래스에 정의되어 있기 때문에 모든 예외에서 사용 가능

<br>

**예시2 - 사용 이유 2번의 예**
```java
static void startInstall() throws SpaceException {
	if(!enoughSpace()) // 충분한 설치 공간이 없을 때
    	throw new SpaceException("설치할 공간이 부족합니다,");
        
	if(!enoughMemory()) // 충분한 메모리가 없을 때
    	throw new RuntimeException(new MemoryException("메모리가 부족합니다."))'
}
```
- 반드시 예외 처리해야하는 MemoryException을 RuntimeException로 감쌌기 때문에 더 이상 `startInstall()`의 선언부에 MemoryException을 선언하지 않아도 됨

