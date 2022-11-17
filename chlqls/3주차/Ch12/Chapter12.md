# 1. 지네릭스(Generics)
## 1.1 지네릭스란?
지네릭스는 다양한 타입의 객체들을 다루는 메서드나 컬렉셔 클래스에 컴파일 시의 타입 체크(compile-time type check)를 해주는 기능

>지네릭스의 장점
>- 타입 안정성을 제공한다.
>- 타입체크와 령변환을 생략할 수 있으므로 코드가 간결해 진다.

<br>

## 1.2 지네릭 클래스의 선언

### - 클래스에 선언하는 지네릭 타입

```java
class Box<T> { //T는 타입 변수(type variable)
    T item;
    
    void setItem(T item) { this.item = item; }
    T getItem() { return item; }
}
```

### - 지네릭스의 용어

```java
class Box<T> {}
```
- `Box<T>` : 지네릭 클래스. 'T의 Box' 또는 'T Box'라고 읽는다
- `T` : 타입 변수 또는 타입 매개변수(T는 타입 문자)
- `Box` : 원시 타입(raw type)

### - 지네릭스의 제한
- 모든 객체에 동일하게 동작해야하는 static멤버에 타입 변수 T를 사용할 수 없다
- T는 인스턴스변수로 간주되기 때문

```java
    static T item; //error
    static int compare(T t1, T t2); //error
```

<br>

## 1.3 지네릭 클래스의 객체 생성과 사용
- 참조변수와 생성자에 대입된 타입(매개변수화된 타입)이 일치해야 함
- 두 타입이 상속관계에 있어도 마찬가지다. (Apple이 Fruit의 자손이라고 가정)
- 단, 두 지네릭 클래스의 타입이 상속관계에 있고, 대입된 타입이 같은 것은 괜찮음(FruitBox는 Box의 자손이라고 가정)
```java
Box<Apple> appleBox = new Box<Apple>; //OK
Box<Apple> appleBox = new Box<Grape>; //error
Box<Fruit> appleBox = new Box<Apple>; //error, 대입된 타입이 다름
Box<Apple> appleBox = new FruitBox<Apple>; //OK, 다형성

```

<br>

## 1.4 제한된 지네릭 클래스
- 지네릭 타입에 `extends`를 사용하면, 특정 타입의 자손들만 대입할 수 있게 제한할 수 있다.
```java
class TruitBox<T extends Fruit> {
	...
}
```

- 클래스가 아니라  인터페이스를 구현해야 한다는 제약이 필요하다면, 이때도 `implements`가 아닌 `extends`를 사용한다

```java
interface Eatable {}
class FruitBox<T extends Eatable> { ...}
```
- 특정 타입의 자손이자 특정 인터페이스를 구현해야 한다면 `&`를 사용하여 나타낸다
```java
class FruitBox<extends Fruit & Eatable> {...}
```

<br>

## 1.5 와일드 카드
- `<? extends T>` : 와일드 카드의 상한(upper bound) 제한. T와 그 자손들만 가능

- `<? super T>` : 와일드 카드의 하한(lower bound) 제한. T와 그 조상들만 가능

- `<?>` : 제한 없음. 모든 타입이 가능. `<? extends Object>`와 동일

<br>

## 1.6 지네릭 메서드
- 메서드의 선언부에 지네릭 타입이 선언된 메서드를 지네릭 메서드라 한다.
- 지네릭 타입의 선언 위치는 반환 타입 바로 앞
```java
static <T> void sort(List<T> list, Comparator<? super T> c)
```

<br>

## 1.7 지네릭 타입의 형변환
- 지네릭 타입과 넌지네릭(non-generic) 타입 간의 형병환은 항상 가능하지만 경고가 발생할 뿐
- 대입된 타입이 Object일지라도 대입된 타입이 다른 지네릭 타입 간에는 형변환이 불가능하다

<br>

## 1.8 지네릭 타입의 제거
1. 지네릭 타입의 경계(bound)를 제거한다.
	- 예를 들어, 지네릭 타입이 `<T extends Fruit>`라면 `T`는 `Fruit`로 치환됨
	- `<T>`인 경우는 `T`는 `Object`로 치환됨
	- 클래스 옆의 선언은 제거됨
2. 지네릭 타입을 제거한 후에 타입이 일치하지 않으면, 형변환을 추가한다.

<br>
<br>

# 2. 열거형(enums)
## 2.1 열거형이란?
- 열거형은 서로 관련된 상수를 편리하게 선언하기 위한 것으로 여러 상수를 정의할 때 사용하면 유용함
- 열거형이 갖는 값뿐만 아니라 타입도 관리하기 때문에 보다 논리적인 오류 줄일 수 있음

<br>

## 2.2 열거형의 정의와 사용
- 열거형 정의하는 방법 : `enum 열거형이름 { 상수명1, 상수명2, ... }`

- 열거형에 정의된 상수를 사용하는 방법 : `열거형이름.상수명`

- 열거형의 대소비교
  - 열거형 상수간의 비교에는 `==`사용
  - `<`,`>`와 같은 비교 연산자는 사용 불가
  - `copareTo()`는 사용 가능

<br>

[Enum클래스에 정의된 메서드]

|메서드|설명
|:--|:--
|`Class<E> getDeclaringClass()`|열거형의 Class객체를 반환
|`String name()`|열거형 상수의 이름을 문자열로 반환
|`int ordinal()`|열거형 상수가 정의된 순서를 반환(0부터 시작)
|`T valueOf(Class<T> enumType, String name)`|지정된 열거형에서 name과 일치하는 열거형 상수를 반환

<br>

## 2.3 열거형에 멤버 추가하기
- 열거형 상수의 값이 불연속적인 경우에는 열거형 상수의 이름 옃에 원하는 값을 괄호()와 함께 적는다
- 열거형에 멤버 추가할 때 주의할 점
  - 열거형 상수를 모두 정의한 다음에 다른 멤버들을 추가해야 함
  - 열거형 상수의 마지막에 `;`을 잊지 말아야 함

```java
enum Direction {
    EAST(1), SOUTH(5), WEST(-1), NORTH(10);
    
    private final int value; //정수를 저장할 필드(인스턴스 변수) 추가
    Direction(int value) { this.value = value; } //생성자 추가
    
    public int getValue() { return value; } 
}
```

<br><br>

# 3. 애너테이션(annotation)
## 3.1 애너테이션이란?
- 애너테이션은 프로그램의 소스코드 안에 다른 프로그램을 위한 정보를 미리 약속된 형식으로 포함시킨 것
- 애너테이션은 주석처럼 프로그래밍 언어에 영향을 미치지 않으면서도 다른 프로그램에게 유용한 정보 제공
- 미리 정의된 태그들을 이용해서 주석 안에 정보를 저장하고, `javadoc.exe`라는 프로그램이 이 정보를 읽어서 문저를 작성하는데 사용한다

## 3.2 표준 애너테이션 & 3.3 메타 애너테이션
[자바에서 기본적으로 제공하는 표준 애너테이션(*가 붙은 것은 메타 애너테이션)]

|애너테이션|설명
|:--|:--
|`@Override`|컴파일러에게 오버라이딩하는 메서드라는 것을 알린다.
|`@Deprecated`|앞으로 사용하지 않을 것을 권장하는 대상에 붙인다.
|`@SuppressWarnings`|컴파일러의 특정 경고메시지가 나타나지 않게 해준다.
|`@SafeVarargs`|지네릭스 타입의 가변인자에 사용한다.
|`@FunctionalInterface`|함수형 인터페이스라는 것을 알린다.
|`@Navtive`|native메서드에서 참조되는 상수 앞에 붙인다.
|`@Target*`|애너테이션이 적용가능한 대상을 지정하는데 사용한다.
|`@Documented*`|애너테이션 정보가 javadoc으로 작성된 문서에 포함되게 한다.
|`@Inherited*`|애너테이션이 자손 클래스에 상속되도록 한다.
|`@Retention*`|애너테이션이 유지되는 범위를 지정하는데 사용한다.
|`@Repeatable`|애너테이션을 반복해서 적용할 수 있게 한다.

- 메타 애너테이션(meta annotation)은 애너테이션을 정의하는데 사용되는 애너테이션의 애너테이션

<br>

## 3.4 애너테이션 타입 정의하기

```java
@interface 애너테이션이름 {
	타입 요소이름(); //애너테이션의 요소를 선언
    ...
}
```

### 애너테이션의 요소(element)
- 애너테이션 내에 선언된 메서드
- 애너테이션 요소의 규칙
   - 요소의 타입은 기본형, String, enum, 애너테이션, Class만 허용된다
   - () 안에 매개변수를 선언할 수 없다
   - 예외를 선언할 수 없다
   - 요소를 타입 매개변수로 정의할 수 없다
