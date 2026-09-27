# SQL_ADVANCED 4주차 정규 과제 

📌SQL_ADVANCED 정규과제는 매주 정해진 분량의 『*혼자 공부하는 SQL*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **SQL_ADVANCED_4th_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=DMNpkj_bZIs&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=13
https://www.youtube.com/watch?v=BUHj-behLyc&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=14
https://www.youtube.com/watch?v=JrXWxku7ZIM&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=15
-->

**교재 실습 예제 파일은 08_SQL_ADVANCED_Template 레포지토리의 src 폴더에 업로드되어 있습니다. market_db 파일도 해당 폴더에 함께 포함되어 있으니 참고하시기 바랍니다.**

**👀(수행 인증샷은 필수입니다.)** 

## SQL_ADVANCED_4th_TIL

### 5장 테이블과 뷰
#### 01. 테이블 만들기
#### 02. 제약조건으로 테이블을 견고하게
#### 03. SQL 가상의 테이블: 뷰 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~99    | ✅         |
| 2주차 | p.102~155   | ✅         |
| 3주차 | p.158~213  | ✅         |
| 4주차 | p.216~271 | ✅         |
| 5주차 | p.274~327 | 🍽️         |
| 6주차 | p.330~369 | 🍽️         |
| 7주차 | p.372~407 | 🍽️         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. 테이블 만들기 

<!-- 테이블 만들기에 관해 배우게 된 점을 적어주세요. -->

**테이블 기본 개념**
- 테이블 = 엑셀 시트 같은 2차원 구조
- 행 = 로우 / 레코드, 열 = 컬럼 / 필드

**만들기 전에 설계부터**
- 테이블 이름, 열 이름, 데이터 형식, NULL 허용 여부, PK/FK 먼저 정리
- ex) 평균 키 → `TINYINT UNSIGNED` (0~255면 충분)

**GUI vs SQL**
- GUI(워크벤치): PK / NN / UN / AI 체크박스로 설정
- 근데 [Apply] 누르면 결국 `CREATE TABLE` 문이 생성됨
- SQL 방식은 Oracle, SQL Server에서도 거의 비슷하게 쓰임 → SQL 먼저 익히는 게 맞음

**SQL로 만들 때 순서**
1. 열 이름 + 데이터 형식만 콤마로 나열
2. NULL / NOT NULL 추가 (안 쓰면 기본 NULL 허용 → 헷갈리니까 직접 써주기)
3. PK, FK 등 제약조건 추가

```sql
CREATE TABLE buy
( num       INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
  mem_id    CHAR(8) NOT NULL,
  prod_name CHAR(6) NOT NULL,
  FOREIGN KEY(mem_id) REFERENCES member(mem_id)
);
```

**기억할 것**
- `AUTO_INCREMENT` → 1부터 자동 증가, 반드시 PK or UNIQUE여야 함 / INSERT 시 `NULL` 넣으면 됨
- FK는 테이블 정의 **맨 마지막**에 작성
- 회원 테이블에 없는 APN으로 구매 입력 → `Error 1452` (회원가입 먼저 해야 구매 가능한 원리)
- 테이블 삭제는 FK 있는 쪽(buy) 먼저
- 주석 `--` 뒤에 한 칸 띄어야 함


## 2. 제약조건으로 테이블을 견고하게 

<!-- 제약조건에 관해 배우게 된 점을 적어주세요. -->

**제약조건 = 데이터 무결성(결함 없음)을 지키기 위한 제한**
- 종류: PK / FK / UNIQUE / CHECK / DEFAULT / NULL 허용

**① PRIMARY KEY (기본 키)**
- 행 구분하는 식별자 → 중복 X, NULL X
- 테이블당 **1개만**
- PK 지정 시 클러스터형 인덱스 자동 생성 (6장에서 자세히)
- 설정 방법 3가지
  - 열 뒤에 `PRIMARY KEY`
  - 맨 마지막에 `PRIMARY KEY(mem_id)`
  - `ALTER TABLE member ADD CONSTRAINT PRIMARY KEY(mem_id);`
- 이름 붙이기도 가능: `CONSTRAINT PRIMARY KEY PK_member_mem_id (mem_id)`

**② FOREIGN KEY (외래 키)**
- 두 테이블 관계 연결
- PK 있는 쪽 = 기준 테이블(member) / FK 있는 쪽 = 참조 테이블(buy)
- 기준 테이블 열은 무조건 PK or UNIQUE
- 열 이름은 달라도 OK (buy.user_id → member.mem_id)
- 관계 맺은 뒤엔 기준 테이블 값 수정·삭제 불가 → `Error 1451`
- 해결: `ON UPDATE CASCADE` / `ON DELETE CASCADE`
  - BLK → PINK로 바꾸면 buy도 자동으로 PINK
  - PINK 삭제하면 구매 기록도 같이 삭제

**③ UNIQUE (고유 키)**
- 중복 X는 PK랑 같음
- 차이점: **NULL 허용**(여러 개여도 OK) + 테이블에 여러 개 설정 가능
- ex) 이메일

**④ CHECK**
- 입력값 점검
- `CHECK (height >= 100)` → 99 입력 시 `Error 3819`
- `ALTER TABLE ... ADD CONSTRAINT CHECK (phone1 IN ('02','031',...))`로 나중에 추가 가능

**⑤ DEFAULT**
- 값 안 넣으면 자동으로 들어갈 값
- `height ... DEFAULT 160`
- ALTER로 할 땐 `ALTER COLUMN phone1 SET DEFAULT '02'`
- INSERT에서 `default`라고 쓰면 기본값 들어감

**⑥ NULL 허용**
- NULL = 허용 / NOT NULL = 필수 입력
- PK 열은 생략해도 자동 NOT NULL
- ⚠️ NULL ≠ 공백(' ') ≠ 0

> **확인문제: 다음 보기 중에서 각 문항이 설명하는 것을 고르세요.**

보기는 아래와 같습니다.
```
CHECK / DEFAULT / PRIMAY KEY / UNIQUE / NOT NULL / FOREIGN KEY
```

```
여기에 답과 그 이유를 적어주세요!
1. 입력되는 데이터가 조건에 맞는지 검사하는 기능: CHECK
   → 입력값 점검용 제약조건. CHECK (height >= 100) 걸면 조건 벗어난 값은 입력 안 되고 Error 3819 뜸
2. 값을 입력하지 않으면 자동으로 들어갈 값: DEFAULT
   → 값 생략하거나 default라고 쓰면 미리 정해둔 값이 자동 입력됨 (ex. DEFAULT 160)
3. 빈 값을 입력하는 것을 허용하지 않음: NOT NULL
   → NULL은 빈 값 허용, NOT NULL은 반드시 값 입력해야 함
```


## 3. 가상의 테이블: 뷰 

<!-- 뷰에 관해 배우게 된 점을 적어주세요. -->

**뷰 = 가상의 테이블**
- 데이터 저장 X, 실체는 **SELECT 문**
- 뷰 접근 순간 SELECT 실행 → 결과 출력
- 바탕화면 '바로 가기 아이콘' 같은 개념
- 이름 앞에 보통 `v_` 붙임

```sql
CREATE VIEW v_member
AS
SELECT mem_id, mem_name, addr FROM member;

SELECT * FROM v_member WHERE addr IN ('서울', '경기');
```

**단순 뷰 vs 복합 뷰**
- 단순 뷰: 테이블 1개로 만든 뷰
- 복합 뷰: 2개 이상(주로 조인) → **읽기 전용**, 입력·수정·삭제 불가

**뷰 쓰는 이유**
1. 보안
   - 알바생한테 이름·주소만 보여줘야 할 때
   - member 테이블 접근은 막고 v_member에만 권한 → 연락처 등 개인정보 노출 X
2. 복잡한 SQL 단순화
   - 긴 조인 쿼리를 뷰(v_memberbuy)로 저장
   - 이후엔 `SELECT * FROM v_memberbuy WHERE ...`로 끝

**뷰 생성·수정·삭제**
- 생성 `CREATE VIEW` / 수정 `ALTER VIEW` / 삭제 `DROP VIEW`
- `CREATE OR REPLACE VIEW` → 있으면 덮어쓰고 없으면 생성 (DROP + CREATE 효과)
- 별칭으로 열 이름 변경 가능, 띄어쓰기·한글도 됨
  - 단, 조회할 때 공백 있으면 **백틱(`)** 필수
  - 한글 열 이름은 비권장
- `DESCRIBE` → 구조 확인 (PK 정보는 안 나옴)
- `SHOW CREATE VIEW` → 소스 코드 확인

**뷰로 데이터 수정할 때 주의점**
- UPDATE는 됨
- INSERT 실패 케이스: 뷰에 없는 열이 NOT NULL이면 `Error 1423`
  - 해결: 뷰에 그 열 포함 / NULL 허용으로 변경 / DEFAULT 지정
- 키 167 이상 뷰에 159 입력 → 들어가긴 하는데 뷰에선 안 보임 (이상함)
  - `WITH CHECK OPTION` 붙이면 조건 밖 값 입력 차단 → `Error 1369`

**참조 테이블 삭제 시**
- 뷰가 있어도 테이블은 그냥 삭제됨
- 뷰 조회하면 `Error 1356`
- `CHECK TABLE 뷰이름;`으로 상태 확인 가능

> **확인문제: 다음은 뷰의 특징입니다. 거리가 먼 것을 하나 고르세요.**

보기는 아래와 같습니다.
```
1️⃣ 뷰에는 테이블의 모든 열을 포함시켜야 합니다.
2️⃣ 뷰는 복잡한 SQL을 단순하게 만드는 효과가 있습니다.
3️⃣ 뷰는 보안에 도움이 됩니다.
4️⃣ 일부 사용자가 테이블에는 접근하지 못하게 하고, 뷰에만 접근하도록 설정할 수 있습니다.
```

```
여기에 답과 그 이유를 적어주세요!
정답: 1️⃣
→ 뷰 실체는 SELECT 문이라 필요한 열만 골라서 만들 수 있음
→ 교재 v_member도 member 테이블에서 아이디·이름·주소 3개 열만 가져옴
→ 오히려 중요한 열을 빼고 보여줄 수 있어서 보안에 도움 되는 것(3, 4번)
→ 모든 열을 포함해야 한다는 건 틀린 설명
```


---

# 2️⃣ 실습과제

## 1. 데이터베이스 구축

아래 코드를 MySQL Workbench에 붙여넣은 후,  
**전체 드래그 → 실행 (Ctrl + Shift + Enter)** 하여 데이터베이스를 생성하세요.

```sql
CREATE DATABASE IF NOT EXISTS week4_db;
USE week4_db;
```

## 2. 실습문제

1. 다음 조건을 만족하는 `users` 테이블을 생성하시오.
```
- user_id는 INT이며 **기본키(Primary Key)**로 설정합니다.
- name은 VARCHAR(20)이며 NULL을 허용하지 않습니다.
- email은 VARCHAR(50)이며 중복을 허용하지 않습니다.
- signup_date는 DATE 타입으로 설정합니다.
- grade는 INT이며 기본값(Default)을 1로 설정합니다.
```

2. 다음 조건을 만족하는 `orders` 테이블을 생성하시오.
```
- order_id는 INT이며 기본키(Primary Key)로 설정합니다.
- user_id는 INT이며 NULL을 허용하지 않습니다.
- amount는 INT이며 0보다 커야 합니다.
- order_date는 DATE 타입으로 설정합니다.
```

3. 다음 조건을 만족하여 데이터를 삽입하시오.
```
- users 테이블에 3명 이상의 데이터를 직접 INSERT 하시오. (단, user 중 본인이 포함돼야 함)
- orders 테이블에 3건 이상의 데이터를 직접 INSERT 하시오.
```

4. users와 orders 테이블을 활용하여 다음 컬럼을 보여주는 뷰 user_order_view를 생성하시오.
```
- user_id
- name
- amount
```

5. 생성한 user_order_view를 조회하시오.

## 3. 제출 방법

1. 각 문제의 실행 결과가 보이도록 화면을 캡처합니다.
2. 테이블 생성 결과, 데이터 삽입 결과, 뷰 생성 및 조회 결과가 모두 보이도록 제출합니다.

<!-- 이 부분을 지우고 인증사진을 제출해주세요.-->

### 🎉 수고하셨습니다.







