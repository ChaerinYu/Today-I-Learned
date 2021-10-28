## Spring DI
애플리케이션 실행 시점(런타임)에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 _클라이언트와 서버의 실제 의존관계가 연결되는 것_  
객체간의 의존관계 연결을 외부에서 수행한다!

### Spring Bean 설정 방법

1. XML로 설정하기
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- root element는 무조건 beans -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!-- 1. 직접 bean 등록하기 -->
    <!-- id: 주인받을 곳에서 호출할 설정 값. 식별을 위한 이름 - 보통 첫글자를 소문자 바꾼 값을 이용한다. 
         class: 주입할 객체의 클래스 패키지경로+클래스이름 -->
    <bean id="memberService" class="com.spring.test.service.MemberServiceImpl">
        <!-- 생성자를 이용한 설정
        <constructor-arg ref="mDao"></constructor-arg>
         -->
        <!-- setter(property)를 이용한 설정
        <property name="memberDao">
            <ref bean="mDao"/>
        </property>
         -->
        <property name="memberDao" ref="mDao"/>
    </bean>
	
    <!-- 2. component-scan + annotation com.spring.test 패키지 아래있는 모든 파일을 체크 -->	
    <context:component-scan base-package="com.spring.test"></context:component-scan>
	
    <!-- 트랜잭션관리, 예외처리, 다국어 처리 등 따로 생성한 설정파일 통합하기 위해 사용 -->
    <import resource="xxxx">
	
	
</beans>
```
* XML
  * Application에서 사용할 spring 자원들을 설정한다. 파일명은 상관없다.
  * `<bean>`: 스프링 컨테이너가 관리할 bean 객체를 지정한다.
  * **생성자**이용한 bean 등록 `<constructor-arg>`
    1. 하위 태그 이용하여 주입 가능
      * 객체 주입: `<ref bean="bean이름"/>`
      * 문자열(or primitive type data) 주입: `<value>값</value>` 
    2. 속성 이용하여 주입 가능
      * 객체 주입: `<constructor-arg ref="bean이름"/>`
      * 문자열(or primitive type data) 주입: `<constructor-arg value="값"/>`
  * **Setter(프로퍼티)** 이용한 bean 등록 `<property>`
      1. 하위 태그 이용하여 주입 가능
      * 객체 주입: `<ref bean="bean이름"/>`
      * 문자열(or primitive type data) 주입: `<value>값</value>`
      2. 속성 이용하여 주입 가능
      * 객체 주입: `<property name="property이름" ref="bean이름"/>`
      * 문자열(or primitive type data) 주입: `<property name="property이름" value="값"/>`
      3. xml namespace 이용
      * `<beans>`태그의 스키마 설정에 p namespace 등록
      * 기본데이터 주입: `p:propertyname="value"`
      * bean 주입: `p:propertyname-ref="bean아이디"`
      * ``` xml
        <!--예시-->
        <bean id="memberService" class="com.spring.test.service.MemberServiceImpl"
            p:name="Challenger"
            p:memberDao-ref="memberDao"
        />
        ```
* 컴포넌트 스캔
  * 해당 패키지 하위 파일 중 `@Component` annotation 붙어있는 얘들을 읽어와 자동으로 빈을 등록해준다.  
  * `@Controller`, `@Service`, `@Repository` ..
  * `@Autowired`
    * 특정 Bean의 기능 수행을 위해 다른 Bean을 참조해야 하는 경우 사용한다. 
    * SpringFramework에 종속적이지만 정밀한 DI에 유용하다.
    * 변수에 직접 적용 가능하다. 멤버변수에 직접 정의하는 경우 setter method 만들 필요가 없다.  
    * 멤버변수, setter, constructor, 일반 method 사용 가능
    * 동일한 타입의 bean이 여러 개일 경우, `@Qualifier("name")`으로 식별한다.
    * ``` java
      // 예시
      @Service
      public class MemberServiceImpl implements MemberService {
      
        @Autowired
        @Qualifier("mdao")
        MemberDao memberDao;
      
        @Autowired
        @Qualifier("adao")
        AdminDao adminDao;
      
        
        ...
      }
      
      ```
  * 

<br/>
<br/>

2. Java 클래스로 설정하기
``` java
@Configuration // XML 대신
@ComponentScan(basePackages = {"com.spring.test"})
public class ApplicationConfig {
	
	@Bean
	public DataSource getDataSource() {
		SimpleDriverDataSource dataSource = new SimpleDriverDataSource();
		dataSource.setDriverClass(com.mysql.cj.jdbc.Driver.class);
		dataSource.setUrl("jdbc:mysql:URL");
		dataSource.setUsername("계정이름");
		dataSource.setPassword("계정비밀번호");
		
		return dataSource;
	}
}
```


