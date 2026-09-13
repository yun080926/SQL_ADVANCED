# SQL_ADVANCED 2주차 정규 과제 

📌SQL_ADVANCED 정규과제는 매주 정해진 분량의 『*혼자 공부하는 SQL*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **SQL_ADVANCED_2nd_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=_JURyg_KzHE&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=7
https://www.youtube.com/watch?v=6qkPy7RfLqQ&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=8
https://www.youtube.com/watch?v=WWAFAm9op2U&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=9
-->

**교재 실습 예제 파일은 08_SQL_ADVANCED_Template 레포지토리의 src 폴더에 업로드되어 있습니다. market_db 파일도 해당 폴더에 함께 포함되어 있으니 참고하시기 바랍니다.**

**👀(수행 인증샷은 필수입니다.)** 

## SQL_ADVANCED_2nd_TIL

### 3장 SQL 기본 문법
#### 01. 기본 중에 기본 SELECT ~ FROM ~ WHERE
#### 02. 좀 더 깊게 알아보는 SELECT문
#### 03. 데이터 변경을 위한 SQL문


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~99    | ✅         |
| 2주차 | p.102~155   | ✅         |
| 3주차 | p.158~213  | 🍽️         |
| 4주차 | p.216~271 | 🍽️         |
| 5주차 | p.274~327 | 🍽️         |
| 6주차 | p.330~369 | 🍽️         |
| 7주차 | p.372~407 | 🍽️         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. 기본 중에 기본 SELECT ~ FROM ~ WHERE

### SELECT 문의 전체 구조 (p.112)
SELECT 열_이름
    FROM 테이블_이름
    WHERE 조건식
    GROUP BY 열_이름
    HAVING 조건식
    ORDER BY 열_이름
    LIMIT 숫자

- 대괄호로 묶인 절(FROM 이하)은 전부 생략 가능하고, 3장에서는 이 중
  `SELECT ~ FROM ~ WHERE` 세 줄이 핵심이다.
- 세미콜론(;)이 나오기 전까지는 한 줄로 쓰든 여러 줄로 쓰든 동일하다.
  SQL이 길어지면 절 단위로 줄을 나눠 쓰는 게 읽기 편하다.

### USE 문 (p.111)
- `USE 데이터베이스_이름;` — 앞으로 실행할 SQL이 어느 DB에서 돌지 지정.
  워크벤치 [SCHEMAS] 패널에서 DB를 더블클릭하는 것과 같은 효과다.
- 한 번 지정하면 계속 유지되지만, 워크벤치를 재시작하거나 쿼리 창을 새로 열면
  다시 실행해야 한다.
- 지정을 빼먹으면 `Error Code: 1146. Table 'sys.member' doesn't exist` 발생.
- 테이블의 원래 전체 이름은 `데이터베이스_이름.테이블_이름`(market_db.member)인데,
  USE로 지정해두면 `member`만 써도 된다.

### SELECT ~ FROM (p.112~115)
- `SELECT * FROM member;` — `*`는 '모든 열'. 열 이름이 올 자리에 쓰였기 때문에
  모든 열을 의미한다.
- 필요한 열만: `SELECT mem_name FROM member;`
- 여러 열은 콤마로 구분하고, **테이블을 만들 때의 열 순서와 맞출 필요가 없다.**
  보고 싶은 순서대로 나열하면 된다.
- 별칭(alias): `SELECT addr 주소, debut_date "데뷔 일자", mem_name FROM member;`
  별칭에 공백이 있으면 큰따옴표로 묶는다.

### WHERE 절 (p.115~120)
- WHERE 없이 조회하면 테이블 전체 행이 나온다. 실무처럼 수백만 건이면
  성능에도 부담이므로, 학습용 소규모 데이터가 아닌 이상 WHERE와 함께 쓴다.
- 문자형(CHAR, VARCHAR, DATE)은 작은따옴표로 묶고, 숫자형(INT)은 그냥 쓴다.
  `WHERE mem_name = '블랙핑크'` / `WHERE mem_number = 4`
- 관계 연산자: `<`, `<=`, `>`, `>=`, `=`
- 논리 연산자: `AND`는 두 조건을 **모두** 만족, `OR`는 둘 중 **하나만** 만족해도 된다.
- `BETWEEN A AND B` — 숫자 범위. `height >= 163 AND height <= 165`와 완전히 동일.
- `IN('경기','전남','경남')` — 주소처럼 **범위로 표현할 수 없는 문자 데이터**에 사용.
  OR로 일일이 나열한 것과 결과가 같지만 훨씬 간결하다.
- `LIKE` — 문자열의 일부만 검색. `'우%'`는 '우'로 시작하는 모든 것(%는 여러 글자),
  `'__핑크'`는 앞 두 글자는 아무거나이고 '핑크'로 끝나는 것(_는 한 글자).

### 서브쿼리 (p.121)
- SELECT 안에 또 다른 SELECT를 넣는 것.
  `SELECT mem_name, height FROM member
     WHERE height > (SELECT height FROM member WHERE mem_name = '에이핑크');`
- 괄호 안 SELECT의 결과(164)가 그 자리에 값처럼 들어간다.
- SQL 2개를 하나로 합칠 수 있어 관리할 SQL이 하나로 줄어드는 게 장점.

### 그외..
- 주석은 하이픈 2개(`--`) 이후. **하이픈 2개 뒤에 한 칸 띄고** 설명을 써야 한다.
- - `AUTO_INCREMENT`: INSERT할 때 그 자리에 NULL을 넣으면 1, 2, 3… 자동 증가.

<img width="1420" height="981" alt="image" src="https://github.com/user-attachments/assets/2713b20a-c102-41a8-a61b-ebbae42b8eb9" />
<img width="1196" height="886" alt="image" src="https://github.com/user-attachments/assets/06b3ba01-23de-41d4-9538-678bf93889c0" />



> **확인문제: 주소의 지역이 서울, 경기인 회원을 추출하는 SQL 문입니다. 빈칸에 들어갈 수 있는 것을 모두 고르세요.**

```sql
SELECT *
FROM table
WHERE ________;
```

보기는 아래와 같습니다.
```
1. addr IN('서울', '경기')
2. addr BETWEEN '서울' AND '경기'
3. addr = '서울' OR addr = '경기'
4. addr = '서울' AND addr = '경기'
```

```답: 1,3

1. addr IN ('서울', '경기') — 괄호 안 값 중 하나라도 일치하면 참. 주소처럼 문자 데이터는 범위로 지정할 수 없어 IN()을 쓴다.
2. addr BETWEEN '서울' AND '경기' — BETWEEN은 키처럼 크기 비교가 가능한 숫자 데이터용(p.118~119). 게다가 문자열 순서상 '경기'가 앞이라 시작값 > 끝값이 되어 결과가 없다.
3. addr = '서울' OR addr = '경기' — 둘 중 하나만 만족하면 되므로 서울·경기 회원이 모두 나온다. 교재도 OR로 나열한 SQL과 IN()이 동일한 결과라고 명시
4. addr = '서울' AND addr = '경기' — 두 조건을 모두 만족해야 하는데(p.117), 한 행의 addr은 값이 하나뿐이라 0건임.
```


## 2. 좀 더 깊게 알아보는 SELECT문

<!-- ORDER BY절과 GROUP BY절 그리고 HAVING절에 관해 배우게 된 점을 적어주세요. -->

```
여기에 배우게 된 점을 적어주세요!
ORDER BY절: 결과가 출력되는 '순서'를 조절하는 절. 결과의 값이나 개수 자체에는
영향을 주지 않는다는 점이 GROUP BY/WHERE와 다르다.
  - `ORDER BY debut_date` 처럼 쓰며, 기본값은 ASC(Ascending, 오름차순).
    내림차순은 열 이름 뒤에 DESC(Descending)를 붙인다. 생략하면 ASC로 인식된다.
  - **SELECT 문의 절은 생략은 가능해도 순서는 지켜야 한다.** WHERE보다 ORDER BY를
    먼저 쓰면 `Error Code: 1064` 문법 오류가 난다. (p.126)
    → 올바른 순서: SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY → LIMIT
  - 정렬 기준은 여러 개 지정 가능하다. `ORDER BY height DESC, debut_date ASC`처럼
    쓰면 키가 큰 순으로 정렬하되, 키가 같으면 데뷔 일자가 빠른 순으로 정렬된다.
  - 함께 배운 LIMIT: 출력 개수를 제한하며 `LIMIT 시작, 개수` 형식이다.
    `LIMIT 3`은 `LIMIT 0, 3`과 같고(첫 데이터가 0번), `LIMIT 3, 2`는 3번째부터 2건.
    `LIMIT 개수 OFFSET 시작`으로 써도 동일하다. 아무 기준 없이 앞에서 몇 건만 뽑는
    경우는 드물고, 대부분 ORDER BY로 정렬한 뒤 상위 몇 건을 뽑는 용도로 쓴다.
  - 함께 배운 DISTINCT: 조회 결과에서 중복된 값을 1개만 남긴다.
    `SELECT DISTINCT addr FROM member;`처럼 **열 이름 앞**에 붙인다.
    ORDER BY로 정렬해 눈으로 세는 것보다 훨씬 확실하다.

GROUP BY절: 지정한 열의 값이 같은 행끼리 묶어주는 절. 여러 행을 하나로 묶기 때문에
단독으로는 의미가 적고, 묶은 결과를 합계·평균·개수 등으로 계산하는 **집계 함수와
함께 쓰는 것이 기본**이다.
  - `SELECT mem_id, SUM(amount) FROM buy GROUP BY mem_id;`
    → 회원별로 묶은 뒤 구매 개수를 합산. 그냥 조회하면 APN이 1,2,1,1로 4행이지만
      GROUP BY + SUM()을 쓰면 APN 5로 한 행이 된다.
  - 집계 함수: SUM(합계), AVG(평균), MIN(최솟값), MAX(최댓값),
    COUNT(행 개수), COUNT(DISTINCT)(중복 제외 행 개수)
  - 계산식도 넣을 수 있다. 구매 금액은 가격×수량이므로 `SUM(price*amount)`.
  - **COUNT(*)와 COUNT(열_이름)은 다르다.** COUNT(*)는 모든 행을 세지만,
    COUNT(phone1)은 그 열이 NULL인 행을 제외하고 센다.
    member 테이블은 COUNT(*)=10이지만 COUNT(phone1)=8이다. (오마이걸·잇지가 NULL)
  - 결과 열 이름에 함수명이 그대로 나오므로 별칭을 붙이면 보기 좋다.
    별칭에는 작은따옴표도 되지만, 작은따옴표는 INSERT에서 문자 입력에 쓰이므로
    **큰따옴표 사용을 권장**한다. (p.133 note)

HAVING절: GROUP BY로 묶은 결과에 조건을 거는 절. WHERE와 비슷해 보이지만,
**집계 함수에 대한 조건**이라는 점이 다르다.
  - 집계 함수는 WHERE 절에 쓸 수 없다. `WHERE SUM(price*amount) > 1000`을 실행하면
    `Error Code: 1111. Invalid use of group function` 오류가 발생한다. (p.135)
  - 그래서 WHERE 대신 HAVING을 쓰며, **반드시 GROUP BY 절 다음에 나와야 한다.**
    SELECT mem_id "회원 아이디", SUM(price*amount) "총 구매 금액"
        FROM buy
        GROUP BY mem_id
        HAVING SUM(price*amount) > 1000
        ORDER BY SUM(price*amount) DESC;
  --> WHERE는 '묶기 전 개별 행'을 거르고, HAVING은 '묶은 뒤 집계 결과'를 거른다.
```

> **확인문제: 다음 표는 주요 집계함수를 정리한 것입니다. 각 설명에 해당하는 올바른 함수명을 기호에 맞게 작성하세요.**

| 함수명 | 설명 |
|--------|------|
| SUM() | 합계를 구합니다. |
| (ㄱ) | 평균을 구합니다. |
| (ㄴ) | 최소값을 구합니다. |
| MAX() | 최대값을 구합니다. |
| (ㄷ) | 행의 개수를 셉니다. |
| (ㄹ) | 행의 개수를 셉니다 (중복은 1개만 인정). |

```
여기에 답을 적어주세요!
(ㄱ) AVG()
(ㄴ) MIN()
(ㄷ) COUNT()
(ㄹ) COUNT(DISTINCT)
```


## 3. 데이터 변경을 위한 SQL문

<!-- INSERT문, UPDATE문, DELETE문에 관해 배우게 된 점을 적어주세요. -->

```
여기에 배우게 된 점을 적어주세요!
INSERT문: 테이블에 새로운 행 데이터를 입력하는 명령.
  형식: INSERT INTO 테이블 [(열1, 열2, ...)] VALUES (값1, 값2, ...)
  - 테이블 이름 뒤의 **열 목록은 생략 가능**하다. 단, 생략하면 VALUES에 오는 값의
    순서와 개수가 테이블을 정의할 때의 열 순서·개수와 정확히 같아야 한다.
  - 일부 열만 입력하려면 열 이름을 명시하면 되고, 생략한 열에는 NULL이 들어간다.
    `INSERT INTO hongong1 (toy_id, toy_name) VALUES (2, '버즈');`
  - 열 이름과 값을 짝만 맞추면 **순서를 바꿔서도** 입력할 수 있다.
    `INSERT INTO hongong1 (toy_name, age, toy_id) VALUES ('제시', 20, 3);`
  - 여러 건을 한 줄로: VALUES 뒤에 괄호를 콤마로 이어 붙이면 된다.
    `INSERT INTO 테이블 VALUES (값1, 값2), (값3, 값4), (값5, 값6);`

  ▸ AUTO_INCREMENT
  - 열을 정의할 때 지정하면 1부터 자동으로 증가하는 값이 입력된다.
    INSERT할 때는 그 자리에 **NULL**을 써주면 된다.
  - 주의: AUTO_INCREMENT로 지정하는 열은 **반드시 PRIMARY KEY로 지정**해야 한다.
  - 현재 어디까지 증가했는지 확인: `SELECT LAST_INSERT_ID();`
  - 시작값 변경: `ALTER TABLE hongong2 AUTO_INCREMENT=100;`
  - 증가값 변경: 시스템 변수 `SET @@auto_increment_increment=3;`
    → 1000부터 시작 + 3씩 증가로 설정하면 1000, 1003, 1006, ... 으로 입력된다.
  - 시스템 변수는 앞에 @@가 붙는 게 특징이고, `SHOW GLOBAL VARIABLES`로 전체 목록을
    볼 수 있다(500개 이상).

  ▸ INSERT INTO ~ SELECT
  - 다른 테이블에 이미 있는 데이터를 한 번에 대량으로 가져와 입력하는 구문
    INSERT INTO 테이블_이름 (열1, 열2, ...)
        SELECT 문 ;
  - **SELECT가 반환하는 열 개수와 INSERT할 테이블의 열 개수가 같아야 한다.**
  - `INSERT INTO city_popul SELECT Name, Population FROM world.city;`
    -> 4079행이 한 번에 입력됨
  - 이때 `DESC 테이블_이름`(DESCribe)으로 가져올 테이블의 열 이름과 데이터 형식을
    미리 확인하면 CREATE TABLE을 어떻게 만들지 알 수 있다.
  - `데이터베이스_이름.테이블_이름` 형식으로 다른 DB의 테이블에도 접근할 수 있다.

UPDATE문: 이미 입력되어 있는 값을 수정하는 명령
  형식: UPDATE 테이블_이름
            SET 열1=값1, 열2=값2, ...
            WHERE 조건 ;
  - 콤마로 구분하면 여러 열을 한꺼번에 바꿀 수 있다.
    `SET city_name = '뉴욕', population = 0 WHERE city_name = 'New York';`
  - SET에 계산식도 쓸 수 있다. `SET population = population / 10000;`
    → 인구 단위를 1명 → 1만 명 단위로 한 번에 환산.
  - **WHERE는 문법상 생략 가능하지만, 생략하면 테이블의 모든 행이 변경된다.**
    `UPDATE city_popul SET city_name = '서울';`을 실행하면 4000개가 넘는 도시
    이름이 전부 '서울'이 되어버린다. WHERE가 없는 UPDATE는 꼭 다시 확인할 것.
  - MySQL 워크벤치는 기본적으로 UPDATE/DELETE를 막아있음
    [Edit] - [Preferences] - [SQL Editor]에서 **Safe Updates** 체크를 해제하고
    워크벤치를 재시작해야 실행된다. (p.147)

DELETE문: 행 단위로 데이터를 삭제하는 명령. UPDATE와 사용법이 거의 같다.
  형식: DELETE FROM 테이블이름 WHERE 조건 ;
  - `DELETE FROM city_popul WHERE city_name LIKE 'New%';` → 11건 삭제
  - LIMIT과 함께 쓰면 조건에 맞는 것 중 상위 몇 건만 지울 수 있다.
    `DELETE FROM city_popul WHERE city_name LIKE 'New%' LIMIT 5;`
  - UPDATE와 마찬가지로 **WHERE를 생략하면 전체 행이 삭제**되므로 주의.

  ▸ 대용량 테이블을 지울 때: DELETE / DROP / TRUNCATE 비교 (p.151~152)
  | 명령 | 남는 것 | 속도 | WHERE |
  |---|---|---|---|
  | DELETE | 빈 테이블 | 느림(44만 건에 3.4초) | 사용 가능 |
  | DROP | 아무것도 없음(테이블 자체 삭제) | 매우 빠름 | 불가 |
  | TRUNCATE | 빈 테이블 | 매우 빠름(0.03초) | **불가** |
  - 테이블 자체가 더 이상 필요 없으면 DROP,
    테이블 구조는 남기고 내용만 전부 비우려면 TRUNCATE가 효율적이다.
  - TRUNCATE는 WHERE를 못 쓰므로 '조건 없이 전체 행 삭제'일 때만 쓸 수 있다.
```


# 2️⃣ 실습과제

다음 SQL 문을 작성하고 실행 결과를 확인 후 인증 사진을 아래에 업로드하세요.(market_db를 그대로 사용합니다.)

1. 모든 그룹 멤버의 정보를 조회하시오.
2. 멤버의 수가 6명 이상인 그룹 정보를 조회하시오.
3. 현재 구매 테이블에 존재하는 서로 다른 상품(prod_name)이 어떤 것이 있는지 조회하시오.
4. 총 구매 금액이 1000미만인 prod_name 중 상위 2개만 조회하시오.

<img width="1027" height="762" alt="image" src="https://github.com/user-attachments/assets/378cb80b-712b-44e1-b9c4-e228d5053da1" />


### 🎉 수고하셨습니다.







