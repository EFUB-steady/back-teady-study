# 불필요한 객체 생성을 피하라

## 객체 재사용

똑같은 기능의 객체를 매번 생성하기 보단 **객체 하나를 재사용하는 편이 나을 때가 많다.**

특히 불변 객체는 언제든 재사용할 수 있다.

```java
//나쁜 코드
String s= new String("mimmu"); //새로운 인스턴스 매번 만듦
boolean b = new Boolean("mimmu");  // 호출할 때마다 새로운 객체 만듦

//좋은 코드
String s = "mimmu"; // 하나의 String 인스턴스 사용
boolean b = new Boolean.valueOf("mimmu"); //정적 팩터리 메소드를 사용해 불필요한 객체 생성을 피함
```

## 생성비용이 비싼 객체

비싼 객체가 반복해서 필요하다면 **캐싱해서 재사용**하자

```java
// (반복해서 사용할 경우에 효율이) 나쁘다
static boolean isRomanNumeralSlow(String s) {
        return s.matches("^(?=.)M*(C[MD]|D?C{0,3})" 
                + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");
}
```

```java
// (반복해서 사용할 경우에 효율이) 더 낫다
 private static final Pattern ROMAN = Pattern.compile(
            "^(?=.)M*(C[MD]|D?C{0,3})"
                    + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");

static boolean isRomanNumeralFast(String s) {
    return ROMAN.matcher(s).matches();
}
```

하지만 가벼운 객체의 경우 그냥 만드는 게 더 나을 수 있다.(오히려 코드가 헷갈리고, 메모리 사용량을 늘리고 성능을 떨어뜨리기도 한다.)

## 불필요한 객체 생성

### 어댑터

어댑터는 뒷단 객체만 관리하면 되므로, 뒷단 객체 외에는 관리할 상태가 없어 뒷단 객체 하나당 어댑터 하나씩만 만들어지면 충분하다.

Map인터페이스의 keySet 메서드는 Map객체 안의 키 전부를 담은 Set뷰를 반환한다. keySet이 뷰 객체를 여러개 만들어도 상관없지만 이득이 없다.

### 오토박싱

reference type과 primitive type을 섞어 쓸 때 자동으로 상호 변환해주는 기술

```java
private static long sum() {
    Long sum = 0L;
    for (long i = 0; i <= Integer.MAX_VALUE; i++)
        sum += i;
    return sum;
}
```

sum을 long이 아닌 Long으로 선언한 점이 문제가 된다. 불필요한 Long 인스턴스가 약 2^31개 만들어진다.

(long 으로 바꾸면 6.3초 → 0.59초)

따라서 박싱된 기본타입 보다는 기본타입을 사용하고 의도치 않은 오토박싱이 숨어들지 않도록 주의해야 한다.

> 하지만 방어적 복사(defensive copy/아이템50)에서는 이와 대조적으로 객체를 재사용해서는 안된다.
새로운 객체를 만들어야 한다면 기존 객체를 재사용하면 안된다. (버그와 보안 구멍이 발생한다.)
>
