
# Spring Boot Data Jpa 프로젝트에서 Querydsl 적용하기



Querydsl 사용하기 전, 설정을 진행해봅시다.  

`build.gradle`은 준비되었다는 전제하에 진행하도록 하겠습니다.  

<br/><br/>

### QueryDSL Configuration 설정

프로젝트 어느 곳에서나 JPAQueryFactory를 주입받아 Querydsl을 사용할 수 있게 만듭니다.  

``` java
package study.querydsl.config;

import com.querydsl.jpa.impl.JPAQueryFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;

@Configuration
public class QuerydslConfiguration {

    @PersistenceContext
    private EntityManager entityManager;

    @Bean
    public JPAQueryFactory jpaQueryFactory() {
        return new JPAQueryFactory(entityManager);
    }
}
```

QuerydslConfiguration에서 등록한 jpaQueryFactory Bean을 생성자 인잭션으로 주입해 사용하면 됩니다.  

<br/><br/>


#### ✔ JPAQueryFactory 를 주입 받는 방식

1. JPAQueryFactory 를 Bean 으로 등록하여 사용하기
2. 생성자에서 주입 받는 EntityManager 를 넣어 생성하여 사용하기

JPAQueryFactory 를 Bean 으로 등록 시 동시성 문제에 대해 생각해볼 수 있겠지만, 이는 고려하지 않아도 괜찮습니다.  
스프링이 주입해주는 EntityManager 는 런타임 시에 필요한 EntityManager를 사용할 수 있도록 라우팅 해주는 Proxy EntityManager 입니다.  
해당 Proxy는 실제 사용 시점에 트랜잭션 단위로 실제 EntityManager(영속성 컨텍스트)를 할당해주기 때문에 Thread-Safe를 보장합니다.  


<br/><br/><br/>



### Entity 설정

```java
package study.querydsl.domain.foo;

import lombok.AccessLevel;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Entity
public class Foo {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String likes;

    @Builder
    public Foo(String name, String likes) {
        this.name = name;
        this.likes = likes;
    }
}
```

### Entity의 Repository 설정

``` java
package study.querydsl.domain.foo;

import org.springframework.data.jpa.repository.JpaRepository;

public interface FooRepository extends JpaRepository<Foo, Long> {
}
```

<br/><br/>





## ✅ Querydsl 사용방법 1. QuerydslRepositorySupport

QuerydslRepositorySupport를 상속받아 사용하는 방법입니다.  

- Querydsl Repository 생성하기: `FooRepositorySupport`  

``` java
package study.querydsl.domain.foo;

import com.querydsl.jpa.impl.JPAQueryFactory;
import org.springframework.data.jpa.repository.support.QuerydslRepositorySupport;
import org.springframework.stereotype.Repository;

import java.util.List;

import static study.querydsl.domain.foo.QFoo.foo;

@Repository
public class FooRepositorySupport extends QuerydslRepositorySupport {
    private final JPAQueryFactory queryFactory;

    public FooRepositorySupport(JPAQueryFactory queryFactory) {
        super(Foo.class);
        this.queryFactory = queryFactory;
    }

    public List<Foo> findByName(String name) {
        return queryFactory.select(foo)
                .from(foo)
                .where(foo.name.eq(name))
                .fetch();
    }
}
```

- Test 코드

``` java
package study.querydsl.domain.foo;

import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

import static org.assertj.core.api.Assertions.*;

@Slf4j
@SpringBootTest
class FooRepositorySupportTest {

    @Autowired
    private FooRepository fooRepository;

    @Autowired
    private FooRepositorySupport fooRepositorySupport;

    @Test
    @DisplayName("Querydsl 기본 설정 확인 (Configuration)")
    void findByName() {
        // given
        String name = "foo";
        String likes = "movies";
        fooRepository.save(new Foo(name, likes));

        // when
        List<Foo> result = fooRepositorySupport.findByName(name);

        // then
        assertThat(result.size()).isEqualTo(1);
        assertThat(result.get(0).getLikes()).isEqualTo(likes);

        fooRepository.deleteAllInBatch();
    }
}
```

- Querydsl의 Custom Repository와 JpaRepository를 상속한 Repository가 기능을 나눠 가졌기 때문에 항상 2개의 Repository를 의존성으로 받아야 합니다.  

  ``` java
      @Autowired
      private FooRepository fooRepository;  
  
      @Autowired
      private FooRepositorySupport fooRepositorySupport;  
  ```

<br/><br/>


## ✅ Querydsl 사용방법 2. Spring Data Jpa Custom Repository

위와 같은 문제로 Spring Data Jpa에서는 **Custom Repository를 JpaRepository 상속 클래스에서 사용**할 수 있도록 기능을 지원합니다.  

``` java
import org.springframework.data.domain.Page;

import java.util.List;

public interface FooRepositoryCustom {
    List<Foo> findByName(String name);
}
```

``` java
// .. import 생략

import static study.querydsl.domain.foo.QFoo.foo;

public class FooRepositoryImpl implements FooRepositoryCustom {
    // querydsl 쓰려면 필요
    private final JPAQueryFactory queryFactory;

    public FooRepositoryImpl(EntityManager em) {
        this.queryFactory = new JPAQueryFactory(em);
    }

    @Override
    public List<Foo> findByName(String name) {
        return queryFactory.select(foo)
                .from(foo)
                .where(foo.name.eq(name))
                .fetch();
    }

}
```

이 코드를 FooRepository에서 쓸 수 있게 상속 구조로 변경하겠습니다.  

``` java
public interface FooRepository extends JpaRepository<Foo, Long>, FooRepositoryCustom {
}
```

이렇게 하게 되면, FooRepository에서 FooRepositoryImpl의 코드도 사용 가능합니다.  

- Test 코드

``` java
@SpringBootTest
class FooRepositoryTest {

    @Autowired
    private FooRepository fooRepository;

    @Test
    @DisplayName("Querydsl Custom 설정 확인")
    void findByName() {
        // given
        String name = "foo";
        String likes = "movies";
        fooRepository.save(new Foo(name, likes));

        // when
        List<Foo> result = fooRepository.findByName(name);

        // then
        assertThat(result.size()).isEqualTo(1);
        assertThat(result.get(0).getLikes()).isEqualTo(likes);

        fooRepository.deleteAllInBatch();
    }
}
```

<br/><br/>


## ✅ Querydsl 사용방법 3. 상속/구현 없는 Repository

**QueryDSL만으로** Repository를 구성하는 방법입니다.  

- 별도의 extends / implements 없이 **JPAQueryFactory**만 있으면 됩니다.
- 최소한의 Bean 등록을 위해 @Repository를 선언합니다.
- 특정 Entity 만 사용해야 한다는 제약도 없습니다.

``` java
@Repository // Bean 등록
public class FooQueryRepository {
    private final JPAQueryFactory queryFactory;

    public FooQueryRepository(JPAQueryFactory queryFactory) {
        this.queryFactory = queryFactory;
    }
    
    public List<Foo> findByName(String name) {
        return queryFactory.select(foo)
                .from(foo)
                .where(foo.name.eq(name))
                .fetch();
    }
}
```

- Test 코드

``` java
@SpringBootTest
class FooQueryRepositoryTest {

    @Autowired
    private FooRepository fooRepository;

    @Autowired
    private FooQueryRepository fooQueryRepository;


    @Test
    @DisplayName("Querydsl 상속/구현 없는 Repository 확인")
    void findByName() {
        // given
        String name = "foo";
        String likes = "movies";
        fooRepository.save(new Foo(name, likes));

        // when
        List<Foo> result = fooQueryRepository.findByName(name);

        // then
        assertThat(result.size()).isEqualTo(1);
        assertThat(result.get(0).getLikes()).isEqualTo(likes);

        fooRepository.deleteAllInBatch();
    }
}
```

<br/><br/>

## 번외:: QuerydslRepositorySupport

- **장점**
    - getQuerydsl().applyPagination() 스프링 데이터가 제공하는 페이징을 Querydsl로 편리하게 변환 가능합니다. (단, Sort는 오류 발생)
    - from() 으로 시작 가능합니다.(최근에는 QueryFactory를 사용해서 select() 로 시작하는 것이 더 명시적)
    - EntityManager 를 제공합니다.
- **단점/한계**
    - Querydsl 3.x 버전을 대상으로 만들어졌습니다.
    - Querydsl 4.x에 나온 JPAQueryFactory로 시작할 수 없어서 select가 아닌 from으로 시작해서 작성해야 합니다.
    - QueryFactory를 제공하지 않습니다.
    - 스프링 데이터 Sort 기능이 정상 동작하지 않습니다.

<br/><br/>

**🚫 방법 2인 'Custom Repository를 사용'하거나 방법 3인 '상속/구현없는 Repository를 사용'하게 되는 경우, QuerydslSupport에서 제공해주는 페이징 처리는 사용할 수 없습니다.**  

``` java
public class FooRepositoryImpl extends QuerydslRepositorySupport implements FooRepositoryCustom {
    
    private final JPAQueryFactory queryFactory;

    public FooRepositoryImpl(EntityManager em) {
        super(Foo.class);
        this.queryFactory = new JPAQueryFactory(em);
    }
    
    // ...
    
	@Override
    public Page<Foo> findByNamePaging(String name) {
        Pageable pageable = PageRequest.of(0, 10);
        JPQLQuery<Foo> jpaQuery = from(foo)
                .where(foo.name.eq(name))
                .offset(pageable.getOffset())
                .limit(pageable.getPageSize())
                .select(foo);
        // getQuerydsl()이 offset, limit, sorting 자동으로 적용해준다.
        List<Foo> content = getQuerydsl().applyPagination(pageable, jpaQuery).fetch();

        return PageableExecutionUtils.getPage(content, pageable, jpaQuery::fetchCount);
    }
    
    // ...
    
}
```



<br/><br/><br/><br/>


- Ref.
    - [참고1](https://jojoldu.tistory.com/372)
    - [참고2](http://honeymon.io/tech/2020/07/09/gradle-annotation-processor-with-querydsl.html)