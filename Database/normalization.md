# 정규화 (Normalization)

### ✔ 요약
> 관계형 DB의 설계 단계에서 **데이터 중복을 최소화**하기 위하여 데이터 구조를 결정하는 작업  


* 중복 데이터를 허용하지 않기 때문에 `무결성(Integrity)`를 유지할 수 있으며, `DB의 저장 용량` 도 줄일 수 있다.  
* 테이블과 테이블 간의 관계를 분리하여 체계적으로 만드는 단계 👉 **정규화 (Normalization)**


* 정규화를 거치지 않은 경우 `이상(Anomaly) 현상`이 발생할 수 있다.
  * 데이터들이 중복되어 발생하는 문제
    * 삽입 이상 (Insertion Anomaly): 데이터 삽입 시 의도와 다른 값들도 삽입됨
    * 삭제 이상 (Delete Anomaly): 데이터 삭제 시 의도와 다른 값들도 삭제됨
    * 갱신 이상 (Update Anomaly): 속성값 갱싱 시 일부 튜플만 갱신되어 모순 발생

### 정규화 목적
* 중복을 제거하여 삽입, 삭제, 갱신 이상 발생 방지
* 각 릴레이션(table)에 중복된 종속성을 여러 릴레이션(table)으로 분할
* 어떠한 릴레이션이라도 DB로 표현 가능하게 함
* 데이터 삽입 시 릴레이션 재구성할 필요성 감소
* 효과적인 검색 알고리즘 생성 가능

😶💭 정리 필요

### 제 1 정규화 (First Normal Form: 1NF)
### 제 2 정규화 (Second Normal Form: 2NF)
### 제 3 정규화 (Thrid Normal Form: 3NF)
### BCNF (Boyce-Codd Normal Form)
### 제 4 정규화 (Fourth Normal Form: 4NF)
### 제 5 정규화 (Fifth Normal Form: 5NF)

<br/><br/><br/>

[참고1](https://itwiki.kr/w/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4_%EC%A0%95%EA%B7%9C%ED%99%94)  
[참고2](https://mangkyu.tistory.com/110)