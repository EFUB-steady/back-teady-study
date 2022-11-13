# 다 쓴 객체 참조를 해제하라

## 메모리 누수 해결

자바 가비지 컬렉터가 다 쓴 객체를 알아서 회수하기 때문에 개발자가 메모리관리에 신경 쓰지 않아도 된다 생각할 수 있지만 이것은 사실이 아니다.

`메모리 누수` 문제가 있는 코드의 경우 메모리 누수가 발생해 오래 실행하다 보면 점차 가비지 컬렉션 활동과 메모리 사용량이 늘어나 결국 성능이 저하될 것이다.

### 문제가 있는 코드

```java
class Stack {
    private Object[] elements;
    private int size = 0;
    private static final int DEFAULT_INITIAL_CAPACITY = 16;

    public Stack() {
        elements = new Object[DEFAULT_INITIAL_CAPACITY];
    }

    public void push(Object object) {
        ensureCapacity();
        elements[size++] = object;
    }

    public Object pop() {
        if (size == 0) {
            throw new EmptyStackException();
        }
        return elements[--size];
    }

    /**
     * 원소를 위한 공간을 적어도 하나 이상 확보한다.
     * 배열 크기를 늘려야 할 때 마다 대략 2 배씩 늘린다.
     */
    private void ensureCapacity() {
        if (elements.length == size) {
            elements = Arrays.copyOf(elements, 2 * size + 1);
        }
    }
}
```

위 코드를 보기에 문제가 없어보이지만 메모리 누수의 문제를 가지고있다.

스택이 커졌다가 줄어들었을 때 스택에서 꺼내진 객체(pop() 으로 꺼내진 객체 )는 더 이상 사용하지 않는다고 하더라도 GC 가 회수 하지 않는다.

결국 이 스택이 객체들의 다 쓴 참조(obsolete reference)를 계속 갖고 있게 되고, 가비지 컬렉션 활동과 메모리 사용량이 늘어나 성능이 저하되게 된다.

이에 대한 해법은 아주 간단하다. pop() 실행 시 꺼내진 객체의 경우 해당 참조를 다 쓴 것이므로 null 처리 해주어서 참조 해제를 하면 된다.

```java
public Object pop(){
	if(size == 0){
		throw new EmptyStackException();
	}
	Object result = elements[--size];
	elements[size] = null;
	return result;
}
```

하지만 매번 객체를 null처리해주는 것은 바람직하지 않다.

왜냐하면 **객체 참조를 null 처리하는 일은 예외적인 경우여야 하기 때문이다.** 다 쓴 참조를 해제하는 가장 좋은 방법은 그 참조를 담은 변수를 유효 범위 (scope) 밖으로 밀어내는 것이다.

그럼 null처리는 언제 해야 할까? 

메모리를 직접 관리할 때, 원소를 다 사용한 즉시 그 원소가 참조한 객체들을 다 null 처리해줘서 해당 객체를 더는 쓰지 않을 것임을 가비지 컬렉터에 알려야 한다.

## 메모리 누수에 주의해야 하는 경우

### 자기 메모리를 직접 관리하는 클래스

원소를 다 사용한 즉시 그 원소가 참조한 객체들을 다 Null 처리 해주자

### 캐시

객체 참조를 캐시에 넣고 그 객체를 다 쓴 뒤로도 한참 캐시에 그냥 놔두는 일이 흔하다.

- 해결책
    1.  `WeakHash Map`을 사용해 캐시를 만드는 방법
    2. 특정 시간이 지나면 캐시 값이 의미가 없어지는 경우, `백그라운드 스레드`사용해 쓰지 않는 엔트리 청소(ex. Scheduled ThreadPoolExecutor)
    3. 새 엔트리를 추가할 때 부수 작업으로 쓰지 않는 엔트리 청소(LinkedHashMap 클래스는 removeEldestEntry라는 메서드 제공)

### 콜백

클라이언트가 콜백을 등록만 하고 명확히 해지하지 않는다면 ㅗㄹ백이 계속 쌓인다.

- 해결책
    - 콜백을 약한 참조(weak reference)로 저장해 가비지 컬렉터가 즉시 수거해가도록 한다.
        
        ex. WeakHashMap
