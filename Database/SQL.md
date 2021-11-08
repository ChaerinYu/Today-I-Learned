
# SQL (Structured Query Language)
DB 정보를 사용할 수 있도록 지원하는 언어  
모든 DBMS에서 사용가능합니다.  
대소문자 구별하지 않습니다. _(데이터의 대소문자는 구별합니다.)_  

SQL 구문은 DCL, DDL, DML로 구분된다. 

### DML (Data Manipulation Language): 조작 (CRUD)
- DB 테이블에서 새로운 행을 입력하거나 기존의 행을 변경하고 제거합니다.  
  - INSERT, UPDATE, DELETE
- DB 테이블에서 데이터를 검색합니다. 
  - SELECT  
  
<BR/>

#### TCL (Transaction Control Language)
- **COMMIT, ROLLBACK**: DML 명령문으로 수행한 변경을 관리합니다.
- transaction: DB의 논리적 연산 단위

### DDL (Data Definition Language): 정의
- 데이터베이스 객체(table, view, index, ..)의 구조를 정의합니다.  
- 테이블 생성, 컬럼추가, 타입변경, 제약조건 지정/수정 등 ..
  - CREATE, ALTER, DROP, RENAME

### DCL (Data Control Language): 제어
- DB와 그 구조에 대한 접근권한을 제공하거나 제거합니다.  
  - GRANT, REVOKE





