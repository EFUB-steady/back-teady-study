## Chapter08. 예외처리(exception handling)
### 프로그램 오류
- 컴파일 에러 : 컴파일 시에 발생하는 에러
- 런타임 에러 : 실행 시에 발생하는 에러
- 논리적 에러 : 실행은 되지만, 의도와 다르게 동작하는 것

자바에서는 실행 시(runtime) 발생할 수 있는 프로그램 오류를 '에러'와 '예외', 두 가지로 구분하였다.
- 에러(error) : 프로그램 코드에 의해서 수습될 수 없는 심각한 오류, ex) 메모리 부족(OutOfMemoryError), 스택오버플로우(StackOverFlowError)
- 예외(exception) : 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류

예외를 다음과 같이 두 그룹으로 나누어 보도록 하겠다.
- RuntimeException 클래스들을 제외한 나머지 Exception 클래스들 (checked 예외) : 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외. 컴파일러가 예외처리를 확인함.
- RuntimeException클래스와 그 자손 클래스들 (unchecked 예외) : 프로그래머의 실수로 발생하는 예외. 컴파일러가 예외처리를 확인하지 않음. ex) ArrayIndexOutOfBoundsException, NullPointerException, ClassCastException, ArithmeticException 

### 예외처리하기 
예외처리(exception handling)
- 정의 : 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성하는 것
- 목적 : 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는것

**try-catch 문**
예외를 처리하기 위해서는 try-catch 문을 사용하며, 그 구조는 다음과 같다.

```
try{
  //예외가 발생할 가능성이 있는 문장들
} catch (Exception1 e1) {
  //Exception1 예외가 발생했을 경우, 이를 처리하기 위한 문장
} catch (Exception2 e2) {
  //Exception2 예외가 발생했을 경우, 이를 처리하기 위한 문장
} catch (ExceptionN eN) {
  //ExceptionN 예외가 발생했을 경우, 이를 처리하기 위한 문장
}
```

try 블럭 내에서 예외가 발생한 경우, 발생한 예외가 일치하는 catch 블럭이 있다면 그 catch 블럭 내의 문장들을 수행하고 전체 try-catch 문을 빠져나가서 그 다음 문장을 계속 수행한다.
예외가 발생한 위치 이후에 있는 try 블럭의 문장들은 수행되지 않음에 주의해야 한다.

catch 블럭의 괄호 ()에 Exception 클래스 타입의 참조변수를 선언해 놓으면 모든 예외 클래스는 Exception클래스의 자손이므로 instanceof 연산자가 true를 리턴해 어떤 종류의 예외에 대해서도 처리가 가능하다.

**printStackTrace()와 getMessage()**
- printStackTrace() : 예외발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력
- getMessage() ; 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

**멀티 catch 블럭**
- 멀티 catch 블럭 : 여러 catch 블럭을 '|' 기호를 이용해서, 하나의 catch 블럭으로 합친 것

```
try{
  //...
} catch (ExceptionA | ExceptionB e){
  e.printStackTrace();
}
```

멀티 catch 블럭을 이용해서 중복된 코드를 줄일 수 있다.
주의점은 만약 '|' 기호로 연결된 예외 클래스가 조상과 자손의 관계에 있다면 컴파일 에러가 발생한다. 어차피 조상 클래스만 써주는 것과 똑같기 때문이다. (불필요한 코드는 제거하라는 의미)
또한 하나의 catch 블럭으로 여러 예외를 처리하는 것이므로 참조변수 e로 '|' 기호로 연결된 예외 클래스들의 공통 분모인 조상 예외 클래스에 선언된 멤버만 사용 가능하다. (instanceof를 이용해 개별적으로 처리 가능하긴 하다.)

**예외 발생시키기**
연산자 new 를 이용해서 발생시키려는 예외 클래스의 객체를 만든 후, 키워드 throw 를 이용해서 예외를 발생시킬 수 있다.
Exception 인스턴스 생성 시, 생성자에 String을 넣어주면 이 String이 Exception 인스턴스에 메시지로 저장되고, getMessage()를 통해 얻을 수 있다.

**메서드에 예외 선언하기**
```
void method() throws Exception1, Exception2, ... ExceptionN {
  //메서드 내용
}
```

예외를 메서드의 throws에 명시하는 것은 예외를 처리하는 것이 아니라. 자신을 호출한 메서드에게 예외를 전달하여 예외처리를 떠맡기는 것이다.
따라서 예외가 발생한 메서드에게 예외처리를 하지 않고 자신을 호출한 메서드에게 예외를 넘겨줄 수는 있지만, 이것으로 예외가 처리된 것은 아니기에 결국 어느 한 곳에서는 반드시 try-catch 문으로 예외처리를 해야 한다.

예외가 발생한 메서드 내에서 자체적으로 처리해도 되는 경우 메서드 내에서 try-catch 문을 사용해서 처리하고, 메서드에 호출 시 넘겨받아야 할 값을 다시 받아야 하는 등 메서드 내에서 자체적으로 해결이 안되는 경우에는 예외를 메서드에 선언해서, 호출한 메서드에서 처리해야 한다.

**finally 블럭**
```
try{
  //예외가 발생할 가능성이 있는 문장들
} catch (Exception1 e1) {
  //예외처리를 위한 문장들
} finally {
  //예외 발생여부에 관계없이 항상 수행되어야 하는 문장들
}
```

finally 블럭의 문장들은 try, catch 블럭에서 return 문이 실행되어도 finally 블럭의 문장들이 먼저 수행된다.


### 자동 자원 반환 - try-with-resources
```
try{
  fis = new FileInputStream("score.dat");
  dis = new DataInputStream(fis);
} catch (IOException ie) {
  ie.printStackTrace();
} finally {
  try{
    if(dis!=null)
      dis.close();
  } catch (IOException ie) {
    ie.printStackTrace();
  }
}
```

주로 입출력에 사용되는 클래스 중에서는 사용한 후 꼭 자원을 반환해 줘야 하는 경우가 있다. 이 경우 다음과 같이 길고 복잡한 코드를 작성해야 한다.
또 문제점은, `try 블럭과 finally 블럭에서 모두 예외가 발생할 경우, try 블럭의 예외는 무시된다`는 것이다.

이러한 점을 개선하기 위해 try-with-resources 문이 추가되었다.
```
try( FileInputStream fis = new FileInputStream("score.dat"); //괄호 안에 두 문장 이상을 넣을 경우 ;로 구분
     DataInputStream dis = new DataInputStream(fis)) {
    while(true){
      score = dis.readInt();
      System.out.println(score);
      sum += score;
    }
  } catch (EOFException e) {
    System.out.println("점수의 총합은 " + sum + "입니다.");
  } catch (IOException ie) {
    ie.printStackTrace();
  }
```

try-with-resources 문의 괄호 () 안에 객체를 생성하는 문장을 넣으면, 이 객체는 따로 close()를 호출하지 않아도 try 블럭을 벗어나는 순간 자동적으로 close()가 호출된다. 
그 다음에 catch 블럭 또는 finally 블럭이 수행된다.
또한 try-with-resources 문을 사용하면 try 블럭에서도 예외가 발생하고 try 문을 빠져나가서 호출된 close() 에서도 예외가 발생할 경우, `CloseException은 실제로 try 블럭에서 발생한 예외에 억제된 예외로 저장`된다.

### 사용자 정의 예외 만들기
```
class MyException extends Exception {
  private final int ERR_CODE;
  
  MyException(String msg, int errCode){
    super(msg);
    ERR_CODE = errCode;
  }
  
  MyException(String msg) {
    this(msg, 100);
  }
  
  public int getErrCode(){
    return ERR_CODE;
  }
}
```

기존에는 예외 클래스를 주로 Exception을 상속받아서 checked 예외로 작성하는 경우가 많았지만, 요즘은 예외처리를 선택적으로 할 수 있도록 RuntimeException을 상속받아 작성하는 쪽으로 바뀌어 가고 있다고 한다.
checked 예외의 경우, 예외처리가 불필요한 경우에도 try-catch 문을 넣어서 코드를 복잡하게 만들기 때문이다. 

### 예외 되던지기(exception re-throwing)
하나의 예외에 대해서 예외가 발생한 메서드와 이를 호출한 메서드 양쪽 모두에서 처리해줘야 할 작업이 있을 때 사용된다. 
이 때 주의할 점은 `예외가 발생할 메서드에서는 try-catch 문을 사용해서 예외처리를 해줌`과 동시에 `메서드의 선언부에 발생할 예외를 throws로 지정`해줘야 한다.

### 연결된 예외(chained exception)
예외 A가 예외 B를 발생시켰다면, A를 B의 `원인 예외(cause exception)`라고 한다.

- Throwable initCause(Throwable cuase) : 지정한 예외를 원인 예외로 등록
- Throwable getCause() : 원인 예외를 반환

```
try{
  startInstall();
  copyFiles();
} catch (SpaceException e){
  InstallException ie = new InstallException("설치 중 예외 발생");
  ie.initCause(e); //원인 예외 지정
  throw ie;
} catch (MemoryException me){
  //...
}
```

발생한 예외를 그냥 처리하지 않고, 원인 예외로 등록해서 다시 예외를 발생시키는 이유는 `여러가지 예외를 하나의 큰 분류의 예외로 묶어서 다루기 위해`서이다.

만약 위의 예제에서 InstallException을 SpaceException과 MemoryException의 조상으로 해서 catch 블럭을 작성하면, 실제로 발생한 예외가 어떤 것인지 알 수 없으며, SpaceException과 MemoryException의 상속 관계를 변경해야 한다는 부담이 생긴다.

또한 checked 예외를 unchecked 예외로 바꿀 수 있도록 하기 위해서이다.
```
static void startInstall() thows SpaceException{
  if(!enoughSpace())
    throw new SpaceException("설치할 공간이 부족합니다.");
    
  if(!enoughMemory())
    throw new RuntimeException(new MemoryException(""메모리가 부족합니다.")); //RuntimeException(Throwable cause) : 원인 예외를 등록하는 생성자
}
```
checked 예외를 unchecked 예외로 바꾸면 예외처리가 선택적이 되므로 억지로 예외처리를 하지 않아도 된다.
