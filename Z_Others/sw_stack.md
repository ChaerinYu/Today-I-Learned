[TOC]



## ModelMapper

- https://devwithpug.github.io/java/java-modelmapper/
- https://zara49.tistory.com/153
- 어떤 Object에 있는 필드 값들을 자동으로 원하는 Object로 Mapping시켜준다.
- getter, setter로 일일이 입력할 필요 없이, 일부만 매핑시켜 사용하고 싶을 때 사용한다.
- 직접 개발하면서 ... araboza





## ObjectMapper

- Jackson library 클래스
  - JSON 컨텐츠 👉 Java 객체 : `deserialization`
  - Java 객체 👉 JSON 컨텐츠 : `serialization`

- ObjectMapper는 생성비용이 비싸기 때문에 bean/static으로 처리하는 게 좋다.



#### Java 객체 👉 JSON : `Serialization`

- ObjectMapper의 `writeValue()` 메서드 사용
- Json 직렬화시킬 클래스에 getter가 존재해야 한다.

```java
ObjectMapper objectMapper = new ObjectMapper(); 
User user = new User("Ryan", 30); 
objectMapper.writeValue(new File("user.json"), user); 
// 파일 출력: user.json 
{"name":"Ryan","age":30} 
// 문자열 출력 
String userAsString = objectMapper.writeValueAsString(user); 
{"name":"Ryan","age":30}
```



#### JSON 👉 Java 객체: `Deserialization`

- ObjectMapper의 `readValue()` 메서드 사용
- 역직렬화시킬 클래스에 JSON을 파싱한 결과를 전달할 생성자가 있어야 한다.
  - ex) Jackson 라이브러리의 `@JsonCreator`, `DTO` 클래스

- `objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);`
  - JSON의 모든 데이터를 파싱하는 것이 아닌 내가 필요로 하는 데이터, 즉 내가 필드로 선언한 데이터들만 파싱할 수 있다.





- https://interconnection.tistory.com/137
- https://velog.io/@zooneon/Java-ObjectMapper%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-JSON-%ED%8C%8C%EC%8B%B1%ED%95%98%EA%B8%B0





## Lombok

어노테이션 기반으로 코드를 자동완성해주는 라이브러리

Lombok을 이용하면 `Getter`, `Setter`, `Equals`, `ToString` 등 다양한 코드를 자동완성시킬 수 있다.

#### 장점

- 어노테이션 기반의 코드 자동생성을 통한 생산성 향상
- 반복되는 코드 다이어트를 통한 가독성 및 유지보수성 향상
- getter, setter외에 빌더 패턴이나 로그 생성 등 다양한 방면으로 활용 가능



- https://mangkyu.tistory.com/78





## Log Back

### Logging?

시스템이 동작할 때 시스템의 상태 및 동작 정보를 시간 경과에 따라 기록하는 것

👉 개발과정 혹은 개발 후 발생할 수 있는 예상치 못한 애플리케이션 문제 진단 가능

👉 다양한 정보 수집 가능

👉 사용자 로그의 경우 분석 데이터로 활용 가능

요즘에는 대표적으로 **Log4j** 와 **Logback** 으로 스프링 부트의 로그 구현체를 사용한다.

(Log4j는 가장 오래된 프레임워크이며 Apache 의 Java 기반 Logging Framework 다.)

### LogBack

Logback 은 log4j 이후에 출시된 Java 기반 Logging Framework 중 하나로 가장 널리 사용되고 있다. 

SLF4j 의 구현체이며 **Spring Boot 환경이라면 별도의 dependency 추가 없이 기본적으로 포함**되어 있다.

Logback 은 log4j 에 비해 향상된 필터링 정책, 기능, 로그 레벨 변경 등에 대해 **서버를 재시작할 필요 없이 자동 리로딩을 지원한다는 장점**이 있다.

Logback 은 5단계의 로그 레벨을 가진다.
심각도 수준은 Error > Warn > Info > Debug > Trace 이다.

- ⛔️ Error : 예상하지 못한 심각한 문제가 발생하는 경우, 즉시 조취를 취해야 할 수준의 레벨
- ⚠ ️Warn : 로직 상 유효성 확인, 예상 가능한 문제로 인한 예외 처리, 당장 서비스 운영에는 영향이 없지만 주의해야 할 부분
- ✅ Info : 운영에 참고할만한 사항, 중요한 비즈니스 프로세스가 완료됨
- ⚙️ Debug : 개발 단계에서 사용하며, SQL 로깅을 할 수 있음
- 📝 Trace : 모든 레벨에 대한 로깅이 추적되므로 개발 단계에서 사용함







## Spring Data JPA

Spring Framework 에서 JPA(*Java Persistence API*)를 편리하게 사용할 수 있도록 지원하는 프로젝트

Spring Data JPA가 JPA를 추상화했다. (Spring Data JPA의 `Repository`의 구현체에서 JPA를 사용하고 있다. JPA는 `EntityManager` 사용)

- CRUD 처리를 위한 공통 인터페이스 제공
- https://docs.spring.io/spring-data/jpa/docs/1.8.0.RELEASE/reference/html/#jpa.repositories



### JPA (Java Persistence API)

자바 어플리케이션에서 관계형 DB를 사용하는 방식을 정의한 **인터페이스**

Hibernate는 JPA라는 명세의 구현체이다. JPA와 Hibernate는 마치 자바의 interface와 해당 interface를 구현한 class와 같은 관계

![JPA, Hibernate, Spring Data JPA의 전반적인 그림](https://suhwan.dev/images/jpa_hibernate_repository/overall_design.png)









## Spring LDAP Repo

**LDAP** - Lightweight Directory Access Protocol

- 네트워크 상에서 조직이나 개인정보 혹은 파일이나 디바이스 정보 등을 찾아보는 것을 가능하게 만든 소프트웨어 프로토콜이다.

- 네트워크 상의 디렉토리 서비스 표준인 X.500의 DAP(Directory Access Protocol)를 기반 경량화(Lightweight) 된 DAP 버전이다.

  - DAP는 OSI 전체 프로토콜 스택을 지원하며 운영에 매우 많은 컴퓨팅 자원을 필요로하는 아주 무거운 프로토콜

  - LDAP은 DAP의 복잡성을 줄이고 TCP/IP 레이어에서 더 적은 비용으로 DAP의 많은 기능적인 부분을 조작할 수 있도록 설계

- **Directory Service**: 이름을 기준으로 대상을 찾아 조회하거나 편집할 수 있는 서비스

  - DNS도 디렉터리 서비스의 일종 (DNS: 도메인 이름으로 IP 주소를 조회)

- Lightweight 하다.

  - 이 의미는 사용하기 간편하다는 의미가 아니라 통신 네트워크 대역폭 상의 가벼움을 의미

  - 인터넷 프로토콜로 데이터를 조금만 주고 받아도 되게끔 설계되었다고 함

  - LDAP의 요청의 99%는 검색에 대한 요청

  - 디렉토리 안에는 연락처, 사용자, 파일, code 등 무엇이든 넣을 수 있고, insert, update 보다는 검색 요청에 특화되어 있다.

  - 검색에 특화되다 보니 트랜잭션이나 롤백이 없고 복잡한 관계 등을 설정할 수 없다.

  - 신뢰성이나 가용성을 개선하기 위해 쉽게 복제될 수 있는 아키텍처로 이루어져 있다.

  - 기본적으로 바이너리 프로토콜이다.

  - ASN.1이라는 언어로 메시지를 표현

  - 메시지를 BER(Basic Encoding Rules)라는 포맷으로 인코딩하여 주고 받음 - BER 인코딩이 바이너리라서 내용을 알아볼 순 없음

- 비동기 프로토콜이다.

  - 세션을 하나만 열어서 여러 메시지 요청을 보낼 수 있고, 각각의 요청에 대한 응답이 다른 시점에 올 수도 있음

  - 응답마다 어떤 요청의 응답인지 식별할 수 있는 아이디가 부여됨

### 용도

- 사용자, 시스템, 네트워크, 서비스, 애플리케이션 등의 정보를 **트리 구조로 저장하여 조회하거나 관리**

- 회사에서 구성원의 조직도나 팀별 이메일 주소 등도 LDAP 서비스로 관리

- 특정 영역에서 이용자명과 패스워드를 확인하여 인증하는 용도로 쓰임









## Hikari CP

매우 가볍고 빠르고 안정적인 JDBC Connection Pool

DB Connection Pool을 관리해주는 역할

#### Connection Pool 관리가 중요한 이유

- Connection 맺는 과정이 성능에 큰 영향을 미치기 때문
- 상당히 복잡하고 컴퓨터의 자원을 많이 소모하는 작업이다.

HikariCP는 미리 정해놓은 만큼 connection을 pool에 담아 놓고 요청이 들어오면 Thread가 connection을 요청하고 HikariCP는 Pool내에 있는 connection을 연결해 준다.



## EhCache

Spring에서 주로 사용되는 로컬 캐시로 Spring에서 간단하게 사용할 수 있는 Java기반 오픈소스 캐시 라이브러리이다.

demon을 가지지 않고 Spring 내부적으로 동작하여 캐싱 처리를 한다.

일반적으로 동일한 리소스에 대해 빈번한 SELECT로 발생되는 DBMS 과부하를 줄이고자 사용한다.

Spring에서는 캐시 추상화(Cache Abstraction)을 통해 편리한 캐싱 기능을 지원하고 있다. 👉 `Spring Cache Abstraction`



> ㅇSpring Cache Abstraction
>
> 사용자는 캐시 구현에 대해 신경 쓸 필요 없이 public interface로 쉽게 캐싱기능을 사용할 수 있다.
>
> 캐싱이 필요한 비즈니스 로직에서 `EhCache`, `Redis`등 캐싱 인프라스트럭쳐에 의존하지 않고 추상화된 퍼블릭 인터페이스로 캐싱을 할 수 있다. 👉 `EhCache`를 사용하다가 중간에 `Redis`로 변경하더라도 비즈니스 로직에는 영향을 주지 않는다.





## JUnit5

Java 8부터 사용 가능한 테스팅 기반 프레임워크이다.

JUnit5 = `JUnit Platform` + `JUnit Jupiter` + `JUnit Vintage`

##### JUnit Platform

JVM에서 테스트 프레임워크를 실행하는데 기초를 제공한다.

##### JUnit Jupiter

테스트를 작성하고 확장하기 위한 새로운 프로그래밍 모델과 확장 모델의 조합이다.

##### JUnit Vintage

하위 호환성을 위해 JUnit3과 JUnit4를 기반으로 돌아가는 플랫폼에 테스트 엔진을 제공한다.



- https://donghyeon.dev/junit/2021/04/11/JUnit5-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C/
- https://steady-coding.tistory.com/349







## Spring Validation

스프링에서는 Validator 인터페이스를 지원하여 어플리케이션에서 사용하는 객체를 검증할 수 있는 기능을 제공한다.

이 Validator 인터페이스는 어떤 특정 계층에 사용하는 기능이 아닌 모든 계층에서 사용할 수 있다.



Validator는 Java EE Spec인 Bean Validation의 Annotation을 이용하여 객체가 제대로 요구사항에 맞추어 생성됐는지 검증할 수 있다.



- https://engkimbs.tistory.com/728





## Spring AOP (Aspect Oriented Programming)

- 관점 지향적 프로그래밍

- 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하는 것 
  - 모듈화? 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것
  - 핵심적인 관점은 우리가 적용하고자 하는 핵심 비즈니스 로직
  - 부가적인 관점은 핵심 로직을 실행하기 위해서 행해지는 DB 연결, 로깅, 파일 입출력
- 코드들을 부분적으로 나누어서 모듈화
- Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용한다.





