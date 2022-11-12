# 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라

많은 클래스가 하나 이상의 자원(다른 클래스)에 의존한다.

예를 들어 SpellChecker(맞춤법 검사기) 클래스가 dictionary(사전)이라는 유틸리티 클래스를 사용한다고 가정해보자. 이런 클래스를 구현하는 방법은 여러가지인데, 정적 유틸리티나 싱글턴을 이용해 구현한 경우를 흔하게 볼 수 있다. 그러나 이는 바람직하지 못한 방법이다.

먼저 이 둘에 대해 살펴보자.

## 정적 유틸리티 클래스

```java
public class SpellChecker {
	private static final Lexicon dictionary = ...;
	
	private SpellChecker() {} //private으로 생성자 만들어서 객체 생성 방지(인스턴스화 방지) - 아이템4 참고

	public static boolean isValid(String word) {...}
	public static List<String> suggestions(String typo) {...}

}

//사용
SpellChecker.isValid(word);
```

## 싱글턴

```java
public class SpellChecker {
	private final Lexicon dictionary = ...;
	
	private SpellChecker(...) {} //인스턴스화 방지
	public static SpellChecker INSTANCE = new SpellChecker(...);

	public boolean isValid(String word) {...}
	public List<String> suggestions(String typo) {...}
}

//사용은 이렇게
SpellChecker.INSTANCE.isValid(word);
```

두 방법 모두 **확장에 유연하지 않고 테스트하기 어렵다.**

두 방법은 SpellChecker가 사전을 단 하나만 사용한다고 가정하고 구현한 것인데, dictionary 하나만으로 여러 종류의 사전(한국어 사전, 영어 사전, 특수 어휘용 사전 등)의 역할을 모두 수행하기가 어렵다.

**사용하는 자원에 따라 동작이 달라지는 클래스에는 `정적 유틸리티 클래스`나 `싱글턴` 방식이 적합하지 않다.**

그렇다면 아래와 같이 final을 삭제하고 사전을 교체하는 메소드를 작성하는 것을 어떨까?

```java
public class SpellChecker {
	private static Lexicon dictionary = ...;

	...
	public static void changeDictionary(Lexicon new) {
		dictionary = new;
	}
}

//사용은 이렇게
SpellChecker.changeDictionary(newDictionary);
```

어색하고 오류를 내기 쉬우며 멀티스레드 환경에서는 쓸 수 없다.

## 의존 객체 주입 🔸

이 방법은 인스턴스를 생성할 때 생성자에 필요한 자원을 넘겨준느 방식이다.

```java
public class SpellChecker {
	private final Lexicon dictionary;
	
	//여기서 의존성 주입!!
	public SpellChecker(Lexicon dictionary) {
		this.dictionary = Objects.requireNonNull(dictionary);
	}

	public boolean isValid(String word) {...}
	public List<String> suggestions(String typo) {...}

}

//인터페이스
interface Lexicon {}

//Lexicon 상속 받아 구현
public class myDictionary implements Lexicon {...}

//사용은 이렇게
Lexicon dic = new myDictionary();
SpellChecker check = new SpellChecker(dic);

check.isValid(word);
```

위 코드에선 클래스(SpellChecker)가 여러 자원 인스턴스를 지원할 수 있고 클라이언트가 원하는 자원(dictionary)을 사용할 수 있다.

의존객체 주입은 **유연성과 테스트 용이성을 높여준다.** 불변을 보장하여 (같은 자원을 사용하려는 ) 여러 클라이언트가 의존 객체들을 안심하고 공유할 수 있기도 하다.

이를 응용해서 생성자에 자원팩토리를 넘겨주는 `팩터리 메서드 패턴`방식을 사용할 수 있다. 팩터리란 호출할 때마다 특정 타입의 인스턴스를 반복해서 만들어주는 객체이다.(ex. `Supplier<T>`인터페이스)

의존성이 너무 많아지면 코드가 장황해질 수 있다. 스프링 같은 의존 객체 주입 프레임워크를 사용하여 이를 해소할 수 있다. 프레임워크는 의존 객체를 직접 주입하도록 설계된 API를 알맞게 응용해 사용하고 있기 때문이다.
