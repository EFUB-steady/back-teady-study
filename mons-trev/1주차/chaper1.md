# chapter1 인텔리제이로 스프링 부트 시작하기

### 인텔리제이: 자바 웹 개발의 대표적인 도구

얼티메이트 버전의 강점
- 자바 개발에 대한 모든 기능 지원
- Maven, Gradle 과 같은 빌드 도구 기능 지원
- 깃 & 깃허브와 같은 vcs(version control system) 기능 지원
- 스프링 부트의 경우 톰캣과 같은 별도의 외장 서버 없이 실행 가능

### 인텔리제이로 프로젝트 생성 및 설정
- 빌드 관리 도구로 gradle 선택 = gradle 프로젝트 생성
- gradle 프로젝트=>springboot project 로 변경

build.gradle 소스
```
buildscript {
  ext{                                          //전역변수 설정
    springBootVersion = '2.1.7.RELEASE'
    }
  repositories {
    mavenCentral()
    jcenter()           //mavenCentral() 보다 라이브러리를 업로드하는 과정이 간단하다.
  }
  dependencies {
    classpath ("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
  }
}
///이 프로젝트의 플러그인 의존성 관리를 위한 설정
/////////////


apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management'   //스프링 부트의 의존성들을 관리해주는 플러그인

//위 네개의 플러그인은 스프링부트에서 필수이다.

group 'com.jojoldu.book'
version '1.0-SNAPSHOT'
sourceCompatibility = 1.8

repositories {        //각종 의존성(라이브러리)들을 어떤 원격 저장소에서 받을지를 정한다.
  mavenCentral()
}

dependencies {        //개발에 필요한 의존성들을 선언하는 곳     *특정 버전을 명시하면 안된다.*
  compile('org.springframework.boot:spring-boot-starter-web') 
  testCompile('org.springframework.boot:spring-boot-starter-test')
}
```
