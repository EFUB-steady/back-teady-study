# private 생성자나 열거 타입으로 싱글턴임을 보증하라

> **싱글톤**이란?
인스턴스를 오직 하나만 생성할 수 있는 클래스
예시) 무상태(stateless) 객체, 설계상 유일해야 하는 시스템 컴포넌트
> 
- 클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려울 수 있다.
    
    → 왜? 타입을 인터페이스로 정의한 다음 그 인터페이스를 구현해서 마든 싱글턴이 아니라면 싱글턴 인스턴스를 mock(가짜) 구현으로 대체할 수 없기 때문이다.
    

## 싱글턴을 만드는 방식

### ▪️public static final 필드 방식의 싱글턴

```java
public class Aespa {
	public static final Aespa INSTANCE = new Aespa();
	private Aespa() {} //private 생성자 딱 한번 호출됨

	public void leaveTheBuilding() {}
}
```

private 생성자는 public static final 필드인 Aespa.INSTANCE를 초기화할 때 **딱 한번만!** 호출된다.

public, protected 생성자가 없기 때문에 Elvis 클래스가 초기화될 때 만들어진 **인스턴스가 전체 시스템에서 하나뿐임이 보장된다.**

- 장점
    - 해당 클래스가 싱글턴임이 API에 명백히 드러난다. (public static 필드가 애초에 final로 박아져있으니까!)
    - 간결함

### ▪️정적 팩터리 방식의 싱글턴

```java
public class Aespa {
	private static final Aespa INSTANCE = new Aespa();
	private Aespa() {}
	public static Aespa getInstance() {return INSTANCE;} //정적 팩터리 방식의 싱글턴
	
	public void leaveTheBuilding(){}
}
```

*Aespa.getInstance*는 항상 같은 객체의 참조를 반환하므로 제2의 Aespa 인스턴스란 만들어지지 않는다.

- 장점
    - 상황에 따라 API를 바꾸지 않더라도 싱글턴이 아니게 변경할 수 있다.
    - 원한다면 정적 팩터리를 제네릭 싱글턴 팩터리로 만들 수 있다.
    - 정적 팩터리의 메서드 참조를 공급자(supplier)로 사용할 수 있다. (Aespa::getInstance를 Supplier<Aespa>로 사용하는 식)

⇒ 두 방법 중에서 정적팩터리 방식의 싱글턴에서 언급한 장점이 필요한 상황이 아니라면 public 필드 방식이 좋다.

### ▪️열거 타입 방식의 싱글턴 🥇

바람직한 방법. public 필드 방식과 비슷하지만 더 간결하고 추가 노력없이 직렬화할 수 있으며, 아주 복잡한 직렬화 상황이나 리플렉션 공격에서도 제2의 가짜 인스턴스가 생기는 일을 막아준다.

```java
public enum Aespa {
	INSTANCE;

	public void leaveTheBuilding() {}
}
```

단 만들려는 싱글턴이 Enum 외의 클래스를 상속해야 한다면 이 방법은 사용할 수 없다.
