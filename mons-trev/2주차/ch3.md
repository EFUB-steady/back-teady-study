## Chapter 3 스프링 부트에서 jpa로 데이터베이스 다뤄보자


## jpa 개념 정리 (자바 표준 orm)
- 객체를 관계형 데이터베이스에서 관리

### jpa 가 없다면? 
자바 클래스를 아름답게 설계하더라도 sql 문을 통해서만 데이터베이스를 조회할 수 있다.
객체를 데이터베이스에 저장하려고 하니 패러다임 불일치 발생
여기서 패러다임을 일치시켜주는 것이 jpa이다.


즉, 개발자는 객체지향적으로 프로그래밍을 하고, jpa가 이를 관계형 데이터베이스에 맞게 sql을 대신 생성해서 실행한다.


### 관계도
jpa<-hibernate<-spring data jpa


### 어노테이션

@Entity : jpa 어노테이션
@id : 해당 테이블의 pk 필드
@GeneratedValue : pk의 생성 규칙 나타냄 (strategy= GenerationType.IDENTITY) 를 뒤에 붙이면 auto increment 된다.
@Column : 테이블의 컬럼을 나타낸다. 굳이 이 어노테이션을 쓰지 않아도 자동으로 필드에 있는 변수들은 column이 된다.
@NoArgsConstructor : 기보 생성자 자동 추가
@Getter : 클래스 내 모든 필드의 Getter메소드를 자동생성
@Builder : 해당 클래스의 빌더 패턴 클래스를 생성, 생성자 상단에 선언 시 생성자에 포함된 필드만 빌더에 포함한다.

** setter 메소드가 없다. (자바빈 규약- entity 클래스에서는 절대 setter 메소드를 쓰지 않는다.)
-> 생성자를 통해 최종값을 채운 후 db에 삽입한다. 생성자 대신에 @Builder로 제공되는 빌더 클래스를 사용할 수 있다.

### jpa Repository
jparepository는 인터페이스를 생성한다. 인터페이스를 상속한 후, JpaRepository<Entity 클래스, PK 타입>를 상속하면 기본적인 CRUD 메소드가 자동으로 생성된다.
@Repository 어노테이션을 추가할 필요 없다. 단, entity 클래스와 기본 entity repository는 함께 위치해야한다. 


