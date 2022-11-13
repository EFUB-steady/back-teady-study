# 컬렉션 프레임웍(Collections Framework)

데이터 군을 저장하는 클래스들을 표준화한 설계

<br>

## 1.1 컬렉션 프레임웍의 핵심 인터페이스
![](https://velog.velcdn.com/images/chlqls/post/f910228d-f8c2-46d6-aa68-3e3f809e3968/image.png)

| 인터페이스 | 특징 
| :---: | --- 
| List | 순서가 있는 데이터의 집합, 데이터의 중복을 허용한다. 예) 대기자 명단 
||구현클래스: `ArrayList`, `LinkedList`, `Stack`, `Vector` 등
| Set | 순서를 유지하지 않는 데이터의 집합. 데이터의 중복을 허용하지 않는다. 예) 양의 정수집합, 소수의 집합
||구현클래스: `HashSet`, `TreeSet` 등
| Map | 키(key)와 값(value)의 쌍(pair)으로 이루어진 데이터의 집합, 순서는 유지되지 않으며, 키는 중복을 허용하지 않고, 값은 중복을 허용한다. 예) 우편번호, 지역번호(전화번호)
||구현클래스: `HashMap`, `TreeMap`, `HashTable`, `Properties` 등

<br>

### - Collection인터페이스
> `List`와 `Set`의 조상인 `Collection인터페이스`는 컬렉션 클래스에 저장된 데이터를 읽고, 추가하고 삭제하는 등 컬렉션을 다루는데 가장 기본적인 메서드를 정의하고 있다.

<br>

### - Map인터페이스
> `Map인터페이스`는 `키(key)`와 `값(value)`을 하나의 쌍으로 묶어서 저장하는 컬렉션 클래스를 구현하는 데 사용된다. 

- 키는 중복될 수 없지만 값은 중복을 허용한다.
- 기존에 저장된 데이터와 중복된 키와 값을 저장하면 기존의 값은 없어지고 마지막에 저장된 값이 남게 된다.

<br>


|메서드|설명
|:--|:--
|`Collection values()`|Map에 저장된 모든 value객체를 반환한다.
|`Set keySet()`|Map에 저장된 모든 key객체를 반환한다.

<br>

---
## 1.2 ArrayList
- 컬렉션 프레임웍에서 가장 많이 사용되는 컬렉션 클래스
- `List인터페이스` 구현하기 때문에 데이터의 저장순서가 유지되고 중복을 허용
- 배열에 더 이상 저장할 공간이 없으면 보다 큰 새로운 배열을 생성해서 기존의 배열에 저장된 내용을 새로운 배열로 복사한 다음에 저장

[ArrayList의 대표 메서드들]

|메서드|설명
|:--|:--
|`void add(int index, Object element)`|지정된 위치(index)에 객체를 저장
|`Object get(int index)`|지정된 위치(index)에 저장된 객체를 반환
|`Object remove(int index)`|지정된 위치(index)에 있는 객체를 제거
|`int size()`|`ArrayLiest`에 저장된 객체의 개수를 반환

<br>

---
## 1.3 LinkedList
>배열의 단점
>1. 크기를 변경할 수 없다.
>2. 비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다.

![](https://velog.velcdn.com/images/chlqls/post/08a71b1b-23f9-4ad0-9b01-82deb739a0b7/image.png)


- 링크드 리스트는 배열의 단점을 보완하기 위해 고안된 자료구조
- 불연속적으로 존재하는 데이터를 서로 연결(link)한 형태로 구성됨
- 링크드 리스트의 각 요소(node)들은 자신과 연결된 다음 요소에 대한 참조(주소값)와 데이터로 구성되어 있음
- 단방향이라는 단점을 보완한 것이 `더블 링크드 리스트(이중 연결리스트, doubly linked list)`
- 제공하는 메서드의 종류와 기능은 ArrayList와 거의 동일

[ArrayList와 LinkedList의 비교]

|컬렉션|읽기(접근시간)|추가 / 삭제|비고 
|:--:|:--:|:--:|:--
|`ArrayList`|빠르다|느리다|순차적인 추가/삭제는 더 빠름. 비효율적인 메모리 사용
|`LinkedList`|느리다|빠르다|데이터가 많을수록 접근성이 떨어짐


<br>

---
## 1.4 Stack과 Queue
### Stack
- 마지막에 저장한 테이더를 가장 먼저 꺼내게 되는 `LIFO(Last In First Out)`구조로 되어있음
- `ArrayList`와 같은 배열기반의 컬렌션 클래스가 적합
- 스택의 활용 예 : 수식계산, 수식괄호검사, 워드프로세서의 indo/redo, 웹브라우저의 뒤로/앞으로

[Stack의 메서드]

|메서드|설명
|:--|:--
|`boolean empty()`|Stack이 비어있는지 알려준다.
|`Object peek()`|Stack의 맨 위에 저장된 객체를 반환. pop()과 달리 Stack에서 객체를 꺼내지는 않음(비어있을 때는 `EmptyStackException`발생)
|`Object pop()`|Stack의 맨 위에 저장된 객체를 꺼낸다. (비어있을 때는 `EmptyStackException`발생)
|`Object push(Object item)`|Stack에 객체(item)를 저장한다.
|`int search(Object o)`|Stack에서 주어진 객체(o)를 찾아서 그 위치를 반환. 못찾으면 -1을 반환.(배열과 달리 위치는 0이 아닌 1부터 시작)

### Queue
- 처음에 저장한 데이터를 가장 먼저 꺼내게 되는 `FIFO(First In First Out)`구조로 되어있음
- 데이터의 추가 삭제가 쉬운 `LinkedList`로 구현하는 것이 적합
- 큐의 활용 예 : 최근사용문서, 인쇄작업 대기목록, 버퍼(buffer)

[Queue의 메서드]

|메서드|설명
|:--|:--
|`boolean add(Object o)`|지정된 객체를 Queue에 추가한다. 성공하면 true를 반환. 저장공간이 부족하면 IllegalStateException발생
|`Object remove()`|Queue에서 객체를 꺼내 반환. 비어있으면 NoSuchElementException발생
|`Object element()`|삭제없이 요소를 읽어온다. peek와 달리 Queue가 비어있을 때 NoSuchElementException발생
|`boolean offer(Object o)`|Queue에 객체를 저장 성공하면 true, 실패하면 false를 반환
|`Object poll()`| Queue에서 객체를 꺼내서 반환. 비어있으면 null을 반환
|`Object peek()`|삭제없이 요소를 읽어 온다. Queue가 비어있으면 null을 반환

<br>

---
## 1.5 Iterator, ListIterator, Enumeration
### Iterator
- 컬렉션에 저장된 각 요소에 접근하는 기능을 가진 인터페이스
- `Collection인터페이스`에는 ‘`Iterator`(Iterator를 구현한 클래스의 인스턴스)‘를 반환하는 `iterator()`를 정의

```java
public interface Iterator {
	boolean hasNext();
    Object next();
    void remove();
}

public interface Collection {
	...
    public Iterator iterator();
    ...
}
```
<br>
[Iterator인터페이스의 메서드]

|메서드|설명
|:--|:--
|`boolean hasNext()`|읽어 올 요소가 남아있는지 확인한다. 있으면 true, 없으면 false를 반환한다.
|`Objext next()`|다음 요소를 읽어 온다. `next()`를 호출하기 전에 `hasNext()`를 호출해서 읽어 올 요소가 있는지 확인하는 것이 안전하다.
|`void remove()`|`next()`로 읽어 온 요소를 삭제한다. `next()`를 호출한 다음에 `remove()`를 호출해야한다.(선택적 기능)


- `iterator()`를 호출하여 `Iterator`를 얻은 다음 반복문, 주로 `while문`을 사용해서 컬렉션 클래스의 요소들 읽을 수 있음
```java
Collection c = new ArrayList();
Iterator it = c.iterator();

while(it.hasNext()) {
	System.out.println(it.next());
}
```

- `Map인터페이스`을 구현한 컬렉션 클래스는 `iterator()`를 직접 호출할 수 없고, 그 대신 `keySet()`이나 `entrySet()`과 같은 메서드를 통해 키와 값을 각각 따로 Set의 형태로 얻어 온 후이 다시 `iterator()`를 호출해야 Iterator를 얻을 수 있음

### Listlterator와 Enumeration
> - `Listlterator` : Iterator에 양방향 조회기능추가(List를 구현한 경우만 사용가능)
> - `Enumeration` : Iterator의 구버전

<br>

---
## 1.6 Arrays
- Array클래스에는 배열을 다루는데 유용한 메서드가 정의되어 있음

### 배열의 복사 - `copyOf()`, `copyOfRange`
>- `copyOf()` : **배열 전체**를 복사해서  새로운 배열 만들어 반환
>- copyOfRange` : **배열의 일부**를 복사해서 새로운 배열을 만들어 반환

### 배열 채우기 - `fill()`, `setAll()`
>- `fill()` :  배열의 모든 요소를 지정된 값으로 채운다
>- `setAll()` : 배열을 채우는데 사용할 함수형 인터페이스를 매개변수로 받는다

```java
int[] arr = new int[5];
Arrays.fill(arr, 9); //arr = [9, 9, 9, 9, 9]
Arrays.setAll(arr, () -> (int)(Math.random() * 5) + 1); //arr = [1, 5, 2, 1, 1]
```

### 배열의 정렬과 검색 - `sort()`, `binarySearch()`
>- `sort()` :  배열을 정렬할 때 사용
>- `binarySearch()` : 배열에서 지정된 값이 저장된 위치(index)를 찾아서 반환(배열이 정렬된 상태이어야함)

### 배열의 비교와 출력 - `equals()`, `toString()`
>- `equals()` :  두 배열에 저장된 모든 요소를 비교해서 같으면 true, 다르면 false를 반환 (일차원 배열에만 사용가능, 다차원 배열의 비교에는 `deepEquals()`사용)
>- `toString()` : 배열의 모든 요소를 문자열로 출력(일차원 배열에만 사용가능, 다차원 배열의 비교에는 `deepToString()`사용)

<br>

---
## 1.7 Comparator와 Comparable
- `Comparator`와 `Comparable`은 모두 인터페이스로 컬렉션을 정렬하는데 필요한 메서드를 정의

>- `Comparator` : 기본 정렬기준을 구현하는데 사용(오름차순)
>- `Comparable` : 기본 정렬기준 외에 다른 기준으로 정렬하고자할 때 사용

<br>

---
## 1.8 HashSet
- `Set인터페이스`를 구현한 가장 대표적인 컬렉션
- 중복된 요소를 저장하지 않음
- `add()`나 `addAll()`로 새로운 요소를 추가하는데 이미 저장되어 있는 요소와 중복된 값이면 이 메서드들이 false를 반환

[HashSet 대표 메서드들]

|메서드|설명
|:--|:--
|`boolean add(Object o)`|새로운 객체를 저장
|boolean isEmpty()|HashSet이 비어있는지 알려줌
|`Object remove(Object o)`|지정된 객체를 HashSet에서 삭제(성공하면 true, 실패하면 false)
|`int size()`|저장된 객체의 개수를 반환

<br>

---
## 1.9 TreeSet
- 이진 검색 트리(binary search tree)라는 자료구조의 형태로 데이터를 저장하는 컬렉션 클래스
- 정렬, 검색, 범위검색에 높은 성능을 보이는 자료구조인 이진 검색 트리의 성능을 향상시킨 '`레드-블랙 트리(Red-Black tree)`'로 구현되어 있음
- 중복된 데이터의 저장을 허용하지 않으며 정렬된 위치에 저장하므로 저장순서를 유지하지 않음

<br>

>`이진 검색 트리(binary search tree)`는
>- 모든 노드는 최대 두 개의 자식노드를 가질 수 있음
>- 왼쪽 자식노드의 값은 부모노드의 값보다 작고 오른쪽 자식노드의 값은 부모노드의 값보다 커야함
>- 노드의 추가, 삭제에 시간이 걸림
>- 검색(범위검색)과 정렬에 유리
>- 중복된 값을 저장하지 못함

<br>

---
## 1.10 HashMap과 Hashtable
- `Hashtable`과 `HashMap`의 관계는 `Vector`와 `ArrayList`의 관계와 같아서 Hashtable보다 새로운 버전인 HashMap사용을 추천

<br>

>- 키(key) : 컬렉션 내의 키(key) 중에서 유일해야 함
>- 값(value) : 키(key)와 달리 데이터의 중복을 허용


<br>

---
## 1.11 TreeMap
- 이진 검색 트리의 형태로 키와 값의 쌍으로 이루어진 데이터를 저장
- 검색과 정렬에 적합한 컬렉션 클래스
<br>

---
## 1.12 Properties
- HashMap의 구버전인 `HashTable`을 상속받아 구현
- `(String, String)`의 형태로 데이터를 저장하는 단순화된 컬렉션 클래스
- 주로 애플리케이션의 환경설정과 관련된 속성(property)을 저장하는데 사용되며 데이터를 파일로부터 읽고 쓰는 편리한 기능 제공
<br>

---
## 1.13 Collections
- 컬렉션과 관련된 메서드를 제공
- `java.util.Collection`은 인터페이스이고 `java.util.Collections`는 클래스이다.
- 다음과 같은 기능을 제공하는 메서드들이 정의되어 있음
  - 컬렉션의 동기화
  - 변경불가 컬렉션 만들기
  - 싱글톤 컬렉션 만들기
  - 힌 종류의 객체만 저장하는 컬렉션 만들기


