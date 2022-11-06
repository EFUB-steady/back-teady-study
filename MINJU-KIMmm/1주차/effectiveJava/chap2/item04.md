# 인스턴스화를 막으려거든 private 생성자를 사용하라

- 정적 멤버만 담은 유틸리티 클래스(인스턴스화 원치 않음)의 쓰임새
    - java.lang.Math, java.util.Arrays처럼 기본 타입값이나 배열 관련 메서드를 모아둠
    - java.util.Collections 처럼 특정 인터페이스를 구현하는 객체를 생성해주는 정적 메서드(혹은 팩터리)를 모아둠
    - final 클래스와 관련한 메서드들을 모아놓음

클래스에 생성자를 명시하지 않으면 자동으로 기본 생성자가 만들어지기 때문에 클래스의 인스턴스화를 막을 수 없다. 클래스의 인스턴스화를 막기위해서는 `private 생성자를 추가`해야한다.

> cf) 추상 클래스로 만드는 방식은 인스턴스화를 막을 수 없다.
하위클래스를 만들어 인스턴스화하면 되기 때문
> 

```java
public class MimmuUtilityClass {
		//기본 생성자가 만들어지는 것을 막는다.(인스턴스화 방지용)
		private MimmuUilityClass() {
			throw new AssertionError();
		}
}
```

private으로 선언하여 상속도 불가능해진다.
