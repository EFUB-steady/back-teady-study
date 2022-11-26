# 1. 자바에서의 입출력
## 1.1 입출력이란?
> I/O(Input&Output) 컴퓨터 내부 또는 외부의 장치와 프로그램 간의 데이터를 주고받는 것을 말함

<br>

## 1.2 스트림(stream)

> 스트림이란 데이터를 운반하는데 사용되는 연결통로

- 스트림은 단방향통신만 가능하기 때문에 하나의 스트림으로 입출력 동시 처리 불가
- 큐와 같은 FIFO구조 (먼저 보낸 데이터 먼저 받음)

<br>

## 1.3 바이트기반 스트림 - InputStream, OutputStream

[입력스트림과 출력스트림의 종류]

|입력스트림|출력스트림|입출력 대상의 종류
|:--|:--|:--
|`FileInputStream`|`FileOutputStream`|파일
|`ByteArrayStream`|`ByteArray`|메모리(byte배열)
|`PipedInputStream`|`PipedOutputStream`|프로세스(프로세스 간의 통신)
|`AudioInputStream`|`AudioOutputStream`|오디오장치

- 이들은 모두 `InputStream`과 `OutputStream`의 자손들

<br>

[InputStream과 OutputStream에 정의된 읽기와 쓰기를 수행하는 메서드]

|InputStream|OutputStream
|:--|:--
|`abstract int read ()`|`abstract int read ()`
|`int read (byte[] b)`|`void write (byte[] b)`
|`int read (byte[] b. int len)`|`void write (byte[] b, int off, int len)`

<br>


## 1.4 보조스트림
- 보조스트림은 실제 데이터를 주고 받는 스티림이 아님
- 스트림의 기능을 향상시키거나 새로운 기능 추가
- 스트림 먼저 생성 후 이를 이용해서 보조스트림 생성

[보조스트림의 종류]

|입력|출력|설명
|:--|:--|:--
|`FilterInputStream`|`FilterOutputStream`|필터를 이용한 입출력 처리
|`BufferedInputStream`|`BufferedOutputStream`|버퍼를 이용한 입출력 성능향상
|`DataInputStream`|`DataOutputStream`|`int`, `float`와 같은 기본형 단위(primitive type)로 데이터를 처리하는 기능
|`SequenceInputStream`|없음|두 개의 스트림을 하나로 연결
|`LineNumberInputStream`|없음|읽어 온 데이터의 라인 번호를 카운트
|`ObjectInputStream`|`ObjectOutputStream`|데이터를 객체단위로 읽고 쓰는데 사용. 주로 파일을 이용하며 객체 직렬화와 관련있음
|없음|`PrintStream`|버퍼를 이용하며, 추가적인 `print`관련 기능
|`PushbackInputStream`|없음|버퍼를 이용해서 읽어 온 데이터를 다시 되돌리는 기능

<br>

## 1.5 문자기반 스트림 - Reader, Writer
- InputStream, OutputStream은 바이트기반의 스트림
- 바이트기반이란 입출력 단위가 1byte라는 뜻
- 문제는 java에서는 문자인 `char`형이 2byte 
- 문자기반의 스트림 : `Reader`, `Writer`

> `InputStream` -> `Reader`
> 
> `OutputStream` -> `Writer`


 <br>

# 2. 바이트기반 스트림
## 2.1 InputStream과 OutputStream
- 모든 바이트기반의 스트림의 조상
- 다음과 같은 메서드 선언되어 있음

<br>

[InputStream의 메서드]

|메서드|설명
|:--|:--
|`int available()`|스트림으로부터 읽어 올 수 있는 데이터의 크기를 반환
|`void close()`|스트림을 닫음으로써 사용하고 있던 자원을 반환
|`void mark(int readlimit)`|현재위치를 표시해 놓는다. 후에 `reset()`에 의해서 표시해 놓은 위치로 다시 돌아갈 수 있음 `readlimit`은 되돌아갈 수 있는 byte의 수
|`boolean markSupported()`|`mark()`와`reset()`을 지원하는지를 알려줌
|`abstract int read()`|1byte를 읽어옴(0~255사이의 값) 더이상 읽어 올 데이터가 없으면 -1 반환
|`int read(byte[] b)`|배열 b의 크기만큼 읽어서 배열을 채우고 읽어 온 데이터의 수를 반환
|`int read(byte[] b, int off, int len)`|최대 len개의 byte를 읽어서 배열 b의 지정된 위치(off)부터 저장
|`void reset()`|스트림에서의 위치를 마지막으로 `mark()`이 호출되었던 위치로 되돌림
|`long skip(long n)`|스트림에서 주어진 길이(n)만큼을 건너띔

<br>

[OutputStream의 메서드]

|메서드|설명
|:--|:--
|`void close()`|입력소스를 닫음으로써 사용하고 있던 자원을 반환
|`void flush()`|스트림의 버퍼에 있는 모든 내용을 출력소스에 씀
|`abstract void write(int b)`|주어진 값을 출력소스에 씀
|`void write(byte[] b)`|주어진 배열 b에 저장된 모든 내용을 출력소스에 씀
|`void write(byte[] b, int off, int len)`|주어진 배열 b에 저장된 내용 중에서 off번째부터 len개만큼만 읽어서 출력소스에 씀



<br><br>

# 6. 표준입출력과 File
## 6.1 표준입출력 - System.in, System.out, System.err
> `System.in` : 콘솔로부터 데이터를 입력받는데 사용
> 
> `System.out` : 콘솔로 데이터를 출력하는데 사용
> 
> `System.err` : 콘솔로 데이터를 출력하는데 사용

<br>

## 6.2 표준입출력의 대상변경 - setOut(), setErr(), setIn() 
|메서드|설명
|:--|:--
|`static void setOut(PrintStream out)`|System.out의 출력을 지정된 PrintStream으로 변경
|`static void setErr(PrintStram err)`|System.err의 출력을 지정한 PrintStream으로 변경
|`static void setIn(InputStream in)`|System.in의 입력을 지정한 InputStream으로 변경

<br>

## 6.4 File
- **절대경로(absolute path)** : 파일시스템의 루트로부터 시작하는 파일의 전체 경로룰 의미, 하나의 파일에 대해 둘 이상의 절대경로가 존재할 수 있음
- **정규경로(canonical path)** : 기호나 링크 등을 포함하지 않는 유일한 경로를 의미

<br>

[File인스턴스 생성할 경우]
```java
//1. 이미 존재하는 파일을 참조할 때
File f = new File("c:\\jdk1.8\\work\\ch15", "FileEx1.java");

//2. 기존에 없는 파일을 새로 생성할 때
File f = new File("c:\\jdk1.8\\work\\ch15", "NewFile.java");
f.createNewFile(); // 새로운 파일이 생성됨
```

<br>
<br>

# 7. 직렬화(Serialization)
## 7.1 직렬화란?
> 직렬화(serialization)란 객체를 데이터 스트림으로 만드는 것

- 즉, 직렬화는 객체에 저장된 데이터를 스트림에 쓰기위해 연속적인 데이터로 변환하는 것

<br>

> 역직렬화(deserialization) : 반대로 스트림으로부터 데이터를 읽어서 객체를 만드는 것

- 직렬화에는 `ObjectOutputStream`을 사용하고 역직렬화에는 `ObjectInputStream`을 사용

<br>

## 7.3 직렬화가 가능한 클래스 만들기 - Serializable, transient
### - Serializable
- 직렬화가 가능한 클래스를 만드는 방법 : 직렬화하고자 하는 클래스가 `java.io.Serializable`인터페이스를 구현하도록 하면 됨
- `Serializable` 인터페이스는 아무런 내용도 없는 빈 인터페이스
- 직렬화를 고려하여 작성한 클래스인지를 판단하는 기준이 됨
- Serializable을 구현한 클래스를 상속받는다면,  Serializable을 구현하지 않아도 됨

### - transient
- 직렬화하고자 하는 객체의 클래스에 직렬화가 안 되는 객체에 대한 참조를 포함하고 있을 때 사용
- 제어자 `transient`를 붙여서 직렬화 대상에서 제외시킴
- `transient`가 붙은 인스턴스변수의 값은 그 타입의 기본값으로 직렬화된다고 볼 수 있음

