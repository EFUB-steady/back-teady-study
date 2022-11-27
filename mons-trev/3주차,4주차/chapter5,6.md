## chapter5,6
### 스프링 시큐리티와 OAuth 2.0으로 로그인 기능 구현하기

스프링 시큐리티 : 막강한 인증과 인가 기능을 가진 프레임워크

OAuth 로그인 구현 시, 로그인 시 보안, 비밀번호 찾기, 회원가입 시 이메일 혹은 전화번호 인증, 비번 변경 회원정보 변경을 구글, 네이버 등에 맡기면 되니 서비스 개발에 집중할 수 있다.

spring-security-oauth2-autoconfigure 라이브러리 사용

1.5 버전과 달리 2.0 버전에서는 직접 입력했던 값을 enum 으로 대체한다. 


프로퍼티스에 추기
```
spring.security.oauth2.client.registration.google.client-id = 클라이언트ID
spring.security.oauth2.client.registration.google.client-secret=클라이언트 보안 비밀
spring.secutiry.oauth2.client.registration.google.scope=profile,email
spring.profiles.include=oauth
```

+ .gitignore 에 추가하여 깃허브 업로드 막기
```
application-oauth.properties
```

USER 도메인 클래스를 만든 후, 사용자의 권한을 관리할 Enum 클래스 Role을 생성한다.

스프링 시큐리티 설정을 위해 build.gradle에 스프링 시ㅠ리티 관련 의존성 하나를 추가한다.

config.auth 패키지를 생성한 뒤, 시큐리티 관련 클래스는 모두 이곳에 담는다.
WebSecurityConfigureAdapter 을 상속 방는 SecurityConfig 클래스 추가해서 설정을 담는다.
OAuth2UserService<OAuth2UserRequest,OAuth2User> 인터페이스를 구현하는 CustomOauth2UserService 클래스를 생성하여 로그인 후 가져온 사용자의 정보, 가입, 정보수정, 세션 저장 등의 기능을
지원받는다. update 기능도 같이 구현되어있다.

dto 패키지에 OAuthAttributes 클래스와 SessionUser 클래스를 추가한다. SessionUser에는 인증된 사용자 정보만이 필요하다.

위의 방법 외에 중복된 코드를 줄이기 위해 config.oauth 패키지에 @LoginUser 어노테이션을 추가하는 방법 또한 있다.
애플리케이션을 재시작하면 재로그인해야되는 필요를 없애기 위해 세션 저장소로 db를 사용해보자.
세션을 저장하는 방법에는 톰캣 세션을 사용하는 경우, 데이터베이스를 세션 저장소로 사용하는 경우, Redis, Memcached와 같은 메모리 데이터베이스를 세션 저장소로 3가지 경우가 있다.
build.gradle 에 추가
```
compile('org.springframework.session:spring-session-jdbc')
```
프로퍼티스에 추가
```
spring.session.store-type=jdbc
````
### aws 서버 환경을 만들어보자-aws ec2

클라우드를 통해 서버, 스토리지, 데이터베이스, 네트워크, 소프트웨어, 모니터링 등의 서비스를 제공받을 수 있다.

aws 서버 환경 세팅 설명은 실습으로 생략
