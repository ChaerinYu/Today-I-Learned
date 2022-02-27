
# Dependency Injection (DI)

Spring Framework는 Java 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크로 IoC, DI, AOP 특징을 갖고 있습니다.  
그 중 DI(Dependency Injection)에 대해서 설명해보려 합니다.  



Dependency Injection을 우리나라 말로 풀어쓰면 `의존 주입`이라는 의미입니다.  

### 여기서의 `의존`이라는 건 무슨 의미일까요? 🤔  
한 클래스가 다른 클래스의 메서드를 실행할 때 이를 `의존`한다고 표현합니다.  
예를 들어, Service 클래스에서 DB 처리를 위해 Dao 클래스의 메서드를 사용한다고 하면,  
"Service 클래스가 Dao 클래스에 의존한다"고 표현할 수 있습니다.  

의존하는 대상이 있으면 그 대상을 구하는 방법이 필요합니다.  
가장 쉬운 방법으로는 의존대상 *객체를 직접 생성하는 것*입니다.

``` java
public class XXXService {
    // 의존 객체 직접 생성
    private XXXDao xxxDao = new XXXDao();
    ...
}
```

이런 경우, `XXXService` 객체를 생성하는 순간에 `XXXDao` 객체도 함께 생성됩니다.  
클래스 내부에서 직접 의존 객체 생성하는 것이 쉽긴 하지만 유지보수 관점에서 문제점을 유발할 수 있습니다.  
*(의존하는 객체가 변경되거나 혹은 기능을 추가하게 된다면 사용하는 코드를 모두 변경해야 하는 문제 등 자잘한 문제들이 있습니다.)*

이렇게 직접 생성하는 방식 이외 의존 객체를 구하는 또 다른 방법이 있습니다.  
이 방법이 `DI` 와 `서비스 로케이터` 입니다.  

이 중 스프링과 관련된 것은 DI로서 아래에서 DI를 이용하여 의존 객체를 구하는 방법에 관해 설명해보겠습니다.


<br/><br/>


## DI를 통한 의존처리
DI를 통해서 우리는 Bean을 생성하는 시점에 필요한 객체를 주입해줘야 합니다.  
의존 주입 방식으로는 4가지 정도가 있습니다.  

<br/>

### 1. 필드 주입 (Field Injection)
- 필드에 의존 관계를 바로 주입하는 방식
``` java
@Service
public class UserServiceImpl implements UserService {
    @Autowired
    private UserRepository userRepository;
}
```
- 외부에서 변경이 불가능하고 순수 JAVA 테스트에서는 불가능합니다. (순수 자바에서 `@Autowired` 동작하지 않는다.)
- 테스트 코드도 순수 자바 테스트이지만 `@SpringBootTest` 어노테이션으로 스프링 컨테이너를 테스트에 통합한 경우에만 가능합니다.

<br/>

### 2. 생성자 주입 (Constructor Injection)
- 생성자를 통해 의존 관계를 주입하는 방식
``` java
@Service
public class UserServiceImpl implements UserService {
	private UserRepository userRepository;
	
	@Autowired
	public UserServiceImpl(UserRepository userRepository) {
		this.userRepository = userRepository;
	}
}
```
- 생성자 호출 시점에 딱 1 번만 호출되는 것이 보장됩니다.
- 때문에 주입받은 객체가 변하지 않거나, 반드시 객체의 주입이 필요한 경우에 강제하기 위해 사용할 수 있습니다. 👉 **불변**, **필수**
- 생성자가 1개만 있을 경우 `@Autowired`를 생략해도 자동 주입이 됩니다.
``` java
@Service
public class UserServiceImpl implements UserService {
	private UserRepository userRepository;
	
	// @Autowired 생략
	public UserServiceImpl(UserRepository userRepository) {
		this.userRepository = userRepository;
	}
}
```
- Spring Framework 에서도 생성자 주입을 추천하고 있습니다.   
👉 `The Spring team generally advocates constructor injection, as it lets you implement application components as immutable objects and ensures that required dependencies are not null.`  
`(Spring 팀은 일반적으로 애플리케이션 구성요소를 불변 객체로 구현하고 필요한 종속성이 무효가 되지 않도록 보장하기 때문에 생성자 주입을 지지합니다.)`

<br/>


### 3. 수정자 주입 (Setter Injection)
- 필드 값을 변경하는 수정자 메서드인 setter를 이용해 의존 관계를 주입하는 방식
``` java
@Service
public class UserServiceImpl implements UserService {
	private UserRepository userRepository;
	
	@Autowired
	public setUserRepository(UserRepository userRepository) {
		this.userRepository = userRepository;
	}
}
```
- 주입받는 객체가 변경될 가능성이 있는 경우에만 사용합니다.
- 수정이 가능하기에 **선택적 종속성**에만 사용해야 한다. 👉 **변경**, **선택**
- `@Autowired`의 기본 동작은 주입할 대상이 없으면 오류가 발생합니다.
- 주입할 대상이 없어도 동작하게 하려면 `@Autowired(required=fasle)`로 지정합니다.

<br/>


### 4. 일반 메서드 주입 (Method Injection)
- 일반 메서드를 통해 주입하는 방식
``` java 
@Service
public class UserServiceImpl implements UserService {
	private UserRepository userRepository;
	private AAAA aaaa;
	
	@Autowired
	public void init(UserRepository userRepository, AAAA aaaa) {
		this.userRepository = userRepository;
		this.aaaa = aaaa;
	}
}
```
- 한 번에 여러 필드를 주입할 수 있습니다.


<br/><br/>


## 왜 생성자 주입을 추천할까요? 🤔
### 1. 객체 불변성 확보
- 대부분 의존 관계 주입은 어플리케이션 종료시점까지 변경되면 안됩니다. setter의 경우 누군가의 실수로 변경될 가능성이 큽니다.  
  👉 OCP 위배
  - *OCP(Open/closed principle) : 개방-폐쇄 원칙, 확장에 대해 열려 있어야 하고 수정에 대해서는 닫혀 있어야 한다.*
- 생성자 주입은 생성할 때 딱 1번만 호출되기 때문에 불변성이 확보됩니다.
- 불변성이므로 `final` 키워드 설정이 가능합니다.

<br/>

### 2. 테스트 코드 작성
- 필드 주입의 경우, 테스트 환경은 DI 프레임워크에서 동작되지 않고 순수 자바 코드로 동작하게 됩니다. 그렇기 때문에 NPE 에러가 발생하여 테스트가 불가능합니다.  
👉 `@SpringBootTest` 어노테이션으로 스프링 컨테이너를 테스트에 통합한 경우에는 가능합니다.
- 수정자 주입의 경우, 싱글톤 패턴을 해칠 수 있고 OCP 위배 문제가 있습니다.
- 생성자 주입의 경우, 의존 관계 주입 누락 시 컴파일 오류가 발생합니다.
  - 어떤 값을 필수로 주입해야 하는 지 확실하게 알 수 있습니다.
  
  
<br/>

### 3. `final` 키워드 작성 및 Lombok 과의 결합
- 생성자 주입의 경우, `final` 키워드 사용이 가능합니다. 이는 값 설정 누락 시 컴파일 시점에 오류를 발생하여 누락 확인이 빠릅니다.
  - `final` 키워드: 상수를 만드는 키워드로 한 번 주입한 이후 다른 객체로 변경이 불가능하다.
- `@RequiredArgsConstructor`: Lombok에서 제공하는 어노테이션으로 `final`이 붙은 필드들을 모아 생성자를 자동으로 생성해줍니다. 
``` java
@Service
@RequiredArgsConstructor
public class UserServiceImpl implements UserService {
	private final UserRepository userRepository;
}
```

<br/>


### 4. 순환참조 에러 방지
- 필드/수정자 주입의 경우, 서로 순환하며 호출하다가 StackOverFlowError가 발생할 수 있습니다.
- 예시로는 닭은 알을 낳고 알은 닭으로 부화되는 circle이 있는 코드입니다.
``` java
// 닭
@Component
public class Chicken {
	@Autowired Egg egg;
	
	public void layEgg() {
		egg.hatch();
	}
}
```
``` java
// 알
@Component
public class Egg {
	@Autowired Chicken chicken;
	
	public void hatch() {
		chicken.layEgg;
	}
}
```
- 생성자 주입의 경우, 어플리케이션 구동 시점(객체 생성 시점)에 컴파일 오류를 발생시켜 순환참조에러가 방지됩니다. 👉 `BeanCreationException`


<br/><br/>


## ✔ 결론
> 생성자 주입의 경우
> - Bean 주입이 되지 않는 경우, 컴파일 시 알려주어 NPE를 예방할 수 있습니다.
> - Bean이 순환 참조되는 경우, StackOverFlowError가 발생하게 되는데 Spring Container가 미리 알려주어 순환참조에 대한 에러를 방지할 수 있습니다.
> - final로 `immutable` 객체를 만들어버리기 때문에 불변하는 bean을 사용할 수 있습니다.
> 
> 필드/수정자 주입의 경우
> - 주입되지 않은 Bean으로 인한 NPE 발생 가능성이 있습니다.
> - Runtime에 순한참조로 인한 StackOverFlowError 발생 가능성이 있습니다.



<br/>
<br/>
<br/>



- [참고1](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-dependencies)
- [참고2](https://interconnection.tistory.com/124)
- [참고3](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard)