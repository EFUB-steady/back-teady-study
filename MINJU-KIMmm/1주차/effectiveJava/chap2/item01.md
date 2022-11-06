# 아이템 1. 생성자 대신 정적 팩터리 메서드를 고려하라

> 클래스는 생성자와 별도로 `정적 팩터리 메서드` 를 제공할 수 있다
> 

## public 생성자 활용해 객체 생성

```java
public class Student {
	protected int strength, age;

	public Student(int strength, int age) {
		this.strength = strenth;
		this.age = age;
	}
}
```

```java
public class Freshman extends Student {
	//public 생성자
	public Freshman() {
		super(100, 20);
	}
}
```

```java
public class Mimmu extends Student {
	//public 생성자
	public Mimmu() {
		super(50, 23);
	}
}
```

## static factory method 활용해 객체 생성

**static factory method**라는 이름에서도 알 수 있듯이 `static method` 를 활용하여 객체(인스턴스)를 생성(`factory` : 무언가 만드는 공장)하는 것을 의미한다.

```java
public class Student {

	private int strength, age;

	private Student(int strength, int age) {
		this.strength = strength;
		this.age = age;
	}
	
	// 새내기 생성하는 정적 팩토리 메소드
	public static Student newFreshman() {
		return new Student(100, 20);
	}

	// 밈무 학생 생성하는 정적 팩토리 메소드
	public static Student newMimmu() {
		return new Student(50, 23);
	}
}
```

정적 팩토리 메소드를 사용해야 하는 이유는 뭘까? 정적 팩토리 메소드는 여러 장점들을 가진다.

## 정적 팩토리 메소드의 장점

### ▪️이름을 가질 수 있다.

- 생성자와 다르게 클래스의 이름과 동일하지 않아도 된다.
- 생성자와 다르게 반환될 **객체의 특성을 이름으로 묘사**할 수 있기 때문에 시그니처(*리턴타입, 파라미터의 데이터타입, 파라미터 개수*)가 같은 생성자가 여러 개 필요하다면 생성자 대신 각각의 차이를 잘 드러내는 이름을 가진 정적 팩터리 메서드가 유리하다.

**예시를 통해 알아보자.**

1. `public 생성자`를 활용해 객체 생성
    
    ```java
    Student freshman = new Student(100, 20);
    Student mimmu = new Student(50, 23);
    ```
    
    생성자에 넘기는 매개변수와 생성자 자체만으로는 반환될 객체의 특성을 제대로 설명하지 못한다. `*new Student(50, 23)*`의 코드를 보고 밈무 학생임을 직관적으로 알기 어렵다.
    
2. `static factory method`를 활용해 객체 생성
    
    ```java
    Student freshman = Student.newFreshman();
    Student mimmu = Student.newMimmu();
    ```
    
    하지만 정적 팩토리 메소드를 사용한다면 의미를 파악하기 쉽다. *`newFreshman(), newMimmu()`* 와 같이 이름을 가지게 되므로 새내기 학생, 밈무 학생임을 직관적으로 알 수 있다.
    
    이렇게 정적 팩토리 메소드 이름을 적절히 지어 메서드 이름만 보고 객체의 생성 목적을 알아낼 수 있다.
    
    ```java
    public class LottoFactory() {
        private static final int LOTTO_SIZE = 6;
        
        private static List<LottoNumber> allLottoNumbers = ...; // 1~45까지의 로또 넘버
    	    
        // 정적 팩토리 메서드
        public static Lotto createAutoLotto() {
            Collections.shuffle(allLottoNumbers);
            return new Lotto(allLottoNumbers.stream()
                    .limit(LOTTO_SIZE)
                    .collect(Collectors.toList()));
        }
    	
        // 정적 팩토리 메서드
        public static Lotto createManualLotto(List<LottoNumber> lottoNumbers) {
            return new Lotto(lottoNumbers);
        }
        ...
    }
    ```
    

### ▪️호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.

- 불변 클래스(immutable class)는 인스턴스를 미리 만들어 놓거나 새로 생성한 인스턴스를 캐싱하여 재활용하는 식으로 불필요한 객체 생성을 피할 수 있다.
- 같은 객체가 자주 요청되는 상황이라면 성능을 올려준다.

<aside>
💡 **인스턴스 통제(instance-controlled) 클래스 (아직 이해가 잘 안 된다)**
반복되는 요청에 같은 객체를 반환하는 식으로 정적 팩터리 박식의 클래스는 언제 어느 인스턴스를 살아 있게 할지를 통제할 수 있다.
**- 통제하는 이유**
1. 클래스를 싱글턴(오직 하나만 생성할 수 있는 클래스)으로 만들 수 있다.
2. 인스턴스화 불가(정적인 변수와 메소드로 구성)로 만들 수도 있다.
3. 불변값 클래스에서 동치인 인스턴스가 단 하나뿐임을 보장할 수 있다. (a==b일때만 a.equals(b)성립)

</aside>

### ▪️반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.

반환할 객체의 클래스를 자유롭게 선택할 수 있게하는 유연성을 제공한다. API를 만들 때 하위 타입의 객체를 반환하는 것을 응용하면 구현 클래스를 공개하지 않고도 그 객체를 반환할 수 있어 API*(각 클래스에 대해 설명해놓은 자료)*를 작게 유지할 수 있다. (다른 개발자들이 무언가를 만들 때 익혀야 할 API가 작아서 쉽게 배워서 사용할 수 있다)

**예시를 통해 알아보자**

1. 정적 팩토리 메소드가 활용되지 않은 경우

```java
public class Level {}

public class Advanced extends Level {}

public class Intermediate extends Level {}

public class Basic extends Level {}
```

API 문서에서 Level, Advanced, Intermediate, Basic 클래스를 모두 이해해야 하고 점수에 따라 레벨 인스턴스를 반환하기 위해 매번 점수 반환 로직을 반복해서 사용해야 한다. 이 경우 중복이 일어날 가능성이 높고, API 문서가 복잡해져 다른 개발자들이 사용하는 데에 어렵다.

1. 정적 팩토리 매소드가 활용된 경우

```java
public class Level {
	public static Level of(int score) {
		if(score < 50) {
			return new Basic();
		} else if(score < 80) {
			return new Intermediate();
		} else {
			return new Advanced();
		}
	}
}

class Advanced extends Level{}

class Intermediate extends Level{}

class Basic extends Level{}
```

API 문서에서 `Level` 클래스의 부분을 읽어보고, `of(int score)`라는 메서드를 사용하면 끝이다.즉, `Advanced`, `Intermediate`, `Basic` 클래스에 대해서는 캡슐화가 되어 객체 생성 인터페이스가 간단해졌다.

### ▪️입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.

```java
class Book {
	//정적 팩토리 메소드
	public static Book getBook(String name) {
		if("EffectiveJava".equals(name)) {
			return new EffectiveJava();
		} else if("CleanCode".equals(name)) {
			return new CleanCode();
		} else {
			return new SelfishGene();
		}
	}

	class EffectiveJava extends Book{}
	
	class CleanCode extends Book{}

	class SelfishGene extends Book{}

	public class Item1 {
		//입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
		Book book = Book.getBook("EffectiveJava");
		System.out.println(book.getClass().getName());
	}
}
```

반환 타입의 하위타입이라면 어떤 클래스의 객체를 반환하든 상관없다. 클라이언트는 팩토리가 건네주는 객체가 어느 클래스의 인스턴스인지 알 수도 없고, 알 필요도 없다.

### ▪️****************************************************************************************************************************************************************************************************************************정적 팩토리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다****************************************************************************************************************************************************************************************************************************

```java
public class Level {
    // 정적 팩토리 메서드
    public static Level of(int score) {
        return new Basic();
    }
}

class Basic extends Level {
}
```

위에서 `Level.of(int score)`라는 **메서드의 구현체 부분을 클라이언트가 직접 구현하지 않도록** 할 수 있게 함으로써 클라이언트를 구현체로부터 분리를 할 수 있다. 이것이 ******************서비스 제공자 프레임워크(ex. JDBC)를 만드는 근간이 된다.******************

예를 들면, 클라이언트가 레벨을 얻는 로직을 구현해야 된다고 가정하자. 레벨을 산출하는 시스템이 달라짐에 따라서 매번 클라이언트가 로직을 수정하면 불편할 것이다. 하지만 위와 같은 정적 팩토리 메서드를 사용함으로써 밑과 같이 `Level.of(int score)`의 **로직을 바꾸더라도 클라이언트는 그대로 수정 없이 동일한 메서드를 사용하면 된다**. 이것이 바로 클라이언트를 구현체로부터 분리한 것이다. 이 덕분에 메서드는 그대로 유지한 채로, 구현체만 바꿔끼울 수 있는 것이다.

```java
public class Level {
    // 정적 팩토리 메서드
    public static Level of(int score) {
        if (score < 50) {
            return new Basic();
        } else if (score < 80) {
            return new Intermediate();
        } else {
            return new Advanced();
        }
    }
}

class Basic extends Level {
}
```

만약 위 패턴을 사용하지 않는다면 프레임워크의 코드가 업데이트 되면 클라이언트도 자신이 구현한 코드를 일일이 다 수정해야 한다.

- 원래

```java
// 특정 프레임 워크에서의 클래스 구조
public class Level {
}

public class Basic extends Level {
}

///////

// 클라이언트가 프레임워크를 사용해서 구현한 로직
Level level = new Basic();
```

- 프레임워크 코드 업데이트되면?

```java
// 특정 프레임 워크에서의 클래스 구조
public class Level {
}

public class Basic extends Level {
}

public class Advanced extends Level {
}

public class Intermediate extends Level {
}

////////

//클라이언트는 원래 코드가 존재했던 부분의 코드를 다 일일이 이렇게 바꿔줘야 한다.
Level level;
if (score < 50) {
  level = new Basic();
} else if (score < 80) {
  level = new Intermediate();
} else {
  level = new Advanced();
}
```

## 단점

### ▪️****상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다****

오히려 장점으로 받아들일 수도 있는 부분(?)

### ▪️정적 팩터리 메서드는 프로그래머가 찾기 어렵다

생성자처럼 API 설명에 명확히 드러나지 않기 때문에 API 문서를 잘 써놓고 메서드 이름도 널리 알려진 규약을 따라 짓는 식으로 문제를 완화해줘야 한다.
