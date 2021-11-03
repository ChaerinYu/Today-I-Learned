
## Transaction
DB의 상태를 변화시키는 일종의 **작업 단위**  
→ 논리적인 작업 set를 모두 처리하거나 또는 원상태로 복구하여 작업 일부만 적용되는 현상이 발생하지 않게 해줍니다.  
여러 클라이언트의 동시 접근 및 서비스 오류 시, 데이터 부정합을 방지하고자 사용됩니다.  
→ 트랜잭션의 _스케줄 관리_ 중요!


## Transaction의 특성 : ACID  
### 원자성 Atomicity  
트랜잭션의 연산은 모두 연산이 완벽히 수행되거나 모두 실패하거나 해야 합니다.  _흑백논리_
### 일관성 Consistency
트랜잭션은 **일관적인 DB 상태를 유지**합니다.  
_예를 들면, PRICE 데이터 타입이 정수형이었다가 문자열이 될 수 없습니다._
### 고립성 Isolation
동시에 실행될 경우, 다른 트랜잭션에 영향을 받지 않고 **독립적**으로 실행되어야 합니다.  
### 내구성 Durability  
트랜잭션이 정상적으로 종료된 이후에는 그 결과가 DB에 **유지**되어야 합니다.  


<br/>
<br/>

---

``` xml
-- table 선언
use testdb;
create table test_table (
    val varchar(20)
); 
```

``` xml
start transaction; -- COMMIT 또는 ROLLBACK이 나올 때 까지 실행되는 모든 SQL
insert into test_table values ('hello');
insert into test_table values ('world');
insert into test_table values ('challenger');
commit; -- COOMIT
```
COMMIT 후, `test_table` 테이블을 조회하면 `hello`, `world`, `challenger` 데이터가 들어있는 걸 확인할 수 있습니다.  
만약 COMMIT이 아닌 ROLLBACK을 했다면 `test_table`에는 데이터가 없는 빈 테이블이었겠죠?!  

<br/>

`test_table` 의 데이터를 다 날렸다 생각하고 다른 예시를 들어보겠습니다.  

``` xml
start transaction; 
insert into test_table values ('hello');
insert into test_table values ('world');
insert into test_table values ('challenger');
savepoint sp; -- point 세팅
insert into test_table values ('bye');
insert into test_table values ('see you');
rollback to sp; -- sp point까지 rollback
```
`savepoint`로 특정 저장점을 설정할 수 있습니다.  
`rollback to savepoint명` 을 수행할 경우, `savepoint` 이후 설정한 내용은 모두 무효 처리되어 DB에 반영되지 않습니다!  
