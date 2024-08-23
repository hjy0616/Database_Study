# Database 기초 공부

## MySql Docker로 설치

- 도커 이미지 설치

```bash
    docker pull <다운로드 할 명칭>
```

- 내려받은 이미지 확인

```bash
    docker images
```

- 도커 이미지 실행

```bash
    docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=<password> -d -p 3306:3306 mysql:latest
```

### DBeaver로 MySql 접근 시 allow 필요

- allowPublicKeyRetrieval true로 바꿔줘야함

## MySql 기준 Database

> database create
>
> > ```sql
> >     CREATE DATABASE <만들 데이터베이스 명칭>
> > ```

> database에서는 row와 column이 존재한다.
>
> > 테이블에서 행(row"옆")은 하나의 개체를 의미하고, 열(column"세로")은 각 개체가 갖는 속성(attribute)을 나타냅니다.
> >
> > > 행은 레코드(record) 열은 필드(field)라고도 부를때 많다고 함

- DBMS에서는 식별을 하기위해서 id row를 무조건 추가해야함
  > 테이블에서 하나의 row를 고유하게 식별할 수 있도록 해주는 column을 **primary Key**라고 한다.

> primary Key 종류
>
> > **Natural Key**
> > ex) 의미가 없는 id 같은 형식이 아닌 user를 예로 email이나 name같은 식별로 key를 갖는 것을 말함
> > **Surrogate Key**
> > ex) 직접적으로 나타내는 column으로 id를 사용해서 혹은 다른 명칭으로 지정해서 해당 row에 임의 번호를 넣어서 작업을 할 수 있음

- 각 상황마다 잘 사용하면 됨 장단점이 있어서 그럼
  <br>

### Not Null

- Null은 값이 아예 없다는 뜻 0도 아니고 ""이것도 아님 그냥 진짜 값이 없다는 뜻으로 쓰임
- NN(Not Null)이 체크되어있는 column 즉 **이 column에 null이 있으면 안된다** 라는 뜻

  - 그래서 **primaryKey는 Not Null** 이어야함

- **Auto Increment**
  - 자동증가를 뜻함 Mysql에서 AI라고 적혀있음
  - primaryKey 중 Surrogate Key는 id를 사용할때 보통 AI를 사용해서 id값을 임의적으로 넣지않고 자동증가를 시켜서 사용한다고 한다.
    - 혹시 테이블에서 Auto Increment, AI 이런 표시를 보게 된다면 이번 노트에서 배운대로 그것이 Surrogate Key이고 MySQL에 의해 자동으로 관리되고 있는 컬럼이구나라고 생각하시면 됩니다.

## R 조회

### 조회

- \*(asterisk)는 모든 값을 보여줘라 라는뜻

- select 조회하겠다. \* 모든 컬럼을, from 어디에서, copang_main이라는 데이터베이스의, member 테이블을 라는 뜻을 가진 문장을 하단 코드에 적음

```sql
SELECT <값을 넣어도됨> FROM copang_main.member;
```

#### 조회시 조건걸기

- **where**은 특정 조건을 만족하는 row들만 "조회"할때 사용한다고 한다. 하단의 코드는 저 문자열이 들어간 email만 조회한 것이다.
- "="는 같음을 의미 "!=", "<>"은 다름을 의미

```sql
SELECT <값> FROM member WHERE email = 'taehos@hanmail.net';
```

- 하단의 코드처럼 date타입의 column도 조건을 통해서 값을 가져올 수 있음

```sql
SELECT * FROM member WHERE age >= 27;
SELECT * FROM member WHERE sign_up_day > '2019-01-01';
```

- Between은 **특정 구간을 조건으로 걸고 싶을 때** 사용함
- Between and는 A부터 B까지를 말한것 밑의 코드를 예로 나이가 30살 부터 39살까지의 데이터를 조회한것
- NOT을 추가하면 반대의 의미로써 밑의 코드를 예로 30대 나이는 조회하지말아라 라는 뜻을가짐
- Date타입도 조회가 가능하다.

```sql
SELECT * FROM member WHERE age BETWEEN 30 AND 39;
SELECT * FROM member WHERE age NOT BETWEEN 30 AND 39;
SELECT * FROM member WHERE sign_up_day BETWEEN '2018-01-01' AND '2018-12-31';
```

#### 문자열 패턴매칭

- like = ~처럼 이라는 뜻, %는 임의 의 문자열이 들어간것
- *를 통해서 한칸을 의미함 c*라면 c의 뒷자리 1칸 이라는뜻

```sql
SELECT * FROM member WHERE address LIKE '서울%';
SELECT * FROM member WHERE email LIKE 'c_____@%';
```

- 이 중에 있는~ (in) 여러 값들 중에서 해당하는 값이 있는 row들만 추려야할 때 사용됨
- 나이 row 에서 20과 30 이란뜻

```sql
SELECT * FROM member WHERE age IN (20, 30);
```

#### 문자열 패턴매칭 주의 사항

1. 이스케이팅(escaping) 문제
   \를 통해서 %같은 SQL에 등록된 문자를 이스케이핑을 시켜 일반 문자열로 볼 수 있게 수정해줘야합니다. 그래야 상관없이 가능합니다.

## SQL을 배워도 잘 못하는 이유

- SQL에서 제공하는 함수를 잘 사용하지 못하기 때문임

### DATE 타입 값을 다루는 함수

#### 연도,월,일 추출하기

> YEAR() 함수를 사용하면 하단처럼 날짜에서 년도만 추출이 가능함

```sql
SELECT * FROM member WHERE YEAR(birthday) = '1992';
```

> MONTH()를 사용하면 날짜의 월값만 뽑는게 가능함

```sql
SELECT * FROM member WHERE MONTH(sign_up_day) IN (6,7,8);
```

> DAYOFMONTH() 함수는 날짜값에서 일만 뽑아낼 수 있

```sql
SELECT * FROM member WHERE DAYOFMONTH(sign_up_day) BETWEEN 15 AND 31;
```

#### 날짜간의 차이 구하기

> DATEDIFF() 함수를 사용하면 됨

```sql
SELECT email, sign_up_day, DATEDIFF(sign_up_day, '2019-01-01') FROM member;
```

> 오늘 기준으로 확인하는 함수 CURDATE()

```sql
SELECT email, sign_up_day, CURDATE(), DATEDIFF(sign_up_day, CURDATE()) FROM member;
```

> 몇살일때 가입했는지도 알 수 있음

- 가입한 날짜 그리고 저장되어 있던 생일을 365로 나눈값임

```sql
SELECT email, sign_up_day, DATEDIFF(sign_up_day, birthday) / 365 FROM member;
```

#### 날짜 더하기 빼기

- 더하기 DATE_ADD(), 빼기 DATE_SUB()
  ex) 가입일 기준 300일 이후의 날짜들을 구할때

```sql
SELECt email, sign_up_day, DATE_ADD(sign_up_day, INTERVAL 300 DAY) FROM member;
```

ex) 가입일(sign_up_day) 기준 250일 이전의 날짜를 구하고 싶으면 이렇게 쓰면 됩니다.

```sql
SELECt email, sign_up_day, DATE_SUB(sign_up_day, INTERVAL 250 DAY) FROM member;
```

**Sql문에서 AND가 OR보다 우선순위가 높다는것을 꼭 알고있어야함**

## 데이터 정렬해서 보기

- ORDER(순서) BY(~에 의해)라는 뜻으로 뒤에 정렬 기준을 적어주면됨

```sql
SELECT * FROM member
ORDER BY height;
```

- 기본적으로 오름차순으로 데이터가 출력이 되는데,
  - **ASC(ascending)** 때문임, 오름차순이라는 의미
  - **DESC(descending)**은 반대로, 내림차순이라는 의미

### 정렬시 주의사항

- int와 Text의 기준으로 정렬할때 주의사항이 있음

- **INT 타입의 값은 숫자의 대소(크고 작음)를 기준으로 정렬이 수행되지만, TEXT 타입의 값은 숫자의 대소가 아니라 한 문자, 한 문자씩 그 문자 순서를 비교해서 정렬이 수행**

- 문자열 타입으로 저장돼있지만, 정렬 기준으로 쓸 때는 숫자형으로 사용하고 싶다면 **CAST()** 함수를 사용하면 됨

```sql
SELECT * FROM ordering_test ORDER BY CAST(data AS signed) DESC;
```

- 저장된 숫자값에 소수점이 포함되어 있다면 **signed 대신 decimal**을 사용하면됨

## 데이터 일부만 추려서 보기

- limit을 통해서 10개만 추려서 볼 수 있음

```sql
SELECT * FROM member
ORDER BY sign_up_day DESC
LIMIT 10;
```

- 하단 처럼 사용하면 2개의 정보만 볼 수 있음

```sql
SELECT * FROM member
ORDER BY sign_up_day DESC
LIMIT 8, 2;
```

- LIMIT (row의 개수), LIMIT (첫 번째 row를 기준으로 한 시작 Offset, row의 개수)

- 하단 처럼 ,은 AND 대신 사용할 수 있음 근데 웃긴게 AND쓰면 에러가남 잘 생각해야함 "하단의 뜻은 review 테이블에 star은 오름차순으로 star가 같은경우 registration_date는 내림차순으로 5개만 보여주세요 라는뜻"

```sql
SELECT * FROM review
ORDER BY star ASC , registration_date desc
LIMIT 5;
```

<br/>

**문자열 비교를 할 때 대소문자 구분을 확실하게 하고 싶다면 문자열 패턴 표현식 앞에 'BINARY'라고 써주면 됩**

<br />

## 데이터 특성 분석

### 집계 함수(Aggregate Function)

- 여러가지 특성이 있음

- COUNT를 통해서 특정 row, column의 갯수를 구할 수 있음
- NULL의 갯수는 제외하고 갯수를 측정하기 때문에 "\*"를 사용하면 NULL값이 있든 없든 전체 row 수를 확인하기 때문에 사용하면 좋음

```sql
SELECT COUNT(*) FROM member;
```

- 최댓값과 최솟값
  - **MAX** 함수와 MIN을 사용해서 구할 수 있다.

```sql
SELECT MAX(height) FROM member;
```

- 평균을 구하는 함수 AVG(average)
  - Null이 있는 값은 제외하고 평균을 구하기 때문에 걱정할 필요없음

```sql
SELECT AVG(weight) FROM member;
```

- **SUM** 함수는 해당 column의 합계를 구해준다고 함.

```sql
SELECT SUM(age) FROM member;
```

- **STD** 함수는 표준 편차를 구해준다고 합니다.

```sql
SELECT STD(age) FROM member;
```

### 산술 함수(Mathematical Function)

- 산술 연산을 해주는 함수들

- **ABS** 함수는 절대값을 구하는 함수

```sql
SELECT ABS(height) FROM member;
```

- **SQRT** 함수는 제곱근을 구하는 함수

```sql
SELECT SQRT(height) FROM member;
```

- **CEIL** 함수는 올림 함수

```sql
SELECT CEIL(height) FROM member;
```

- **FLOOR** 함수는 내림 함수

```sql
SELECT FLOOR(height) FROM member;
```

- **ROUND** 함수는 반올림 함수

```sql
SELECT ROUND(height) FROM member;
```

#### 집계 함수와 산술 함수의 차이점!!!!

1. 집계 함수는 특정 column의 **여러 row의 값들을 동시에 고려**해서 실행되는 함수라고 함.
2. 산술 함수는 특정 column의 **각 row의 값마다 실행되는 함수**라고함.

#### NULL 값 다루기

- IS는 ~가 있는지를 묻는 조건, 하단의 코드는 NULL이 있는지 묻는 것
- 실제 NULL이 아닌 있는 데이터를 가져오는 방법은 **NOT**을 추가하면 됨

```sql
SELECT * FROM member WHERE address IS NULL;
SELECT * FROM member WHERE address IS NOT NULL;
```

- NULL값 대신 다른 값들을 넣어줄 때 사용하는 함수로
- "**COALESCE**"를 사용한다.
  - 첫번째 인자값은 column이 들어가는데, 값이 있다면 column의 일반 값을 **출력**해주고,
  - 값이 없다면 2번째 인자값인 문자열을 **출력**해준다.

```sql
SELECT COALESCE(height, '###'), COALESCE(weight, '___'), COALESCE(address, '@@@') FROM member;
```

#### \* IS NULL과 = NULL은 다르다.

- NULL 값은 어떤 값도 아니기 때문에 =를 사용해서 어떤 값들과 비교할 수 있는 대상이 아님
- 그래서 아무 row에 출력되지 않음

- **NULL에는 어떤 연산을 해도 결국 NULL이다.**

#### 이상한 값을 제외하고 싶을때

- 나이 평균을 구하는 코드,
- 5~100세까지의 데이터를 추려서 통계를 구하는 방법임

```sql
SELECT AVG(age) FROM copang_main.member WHERE age BETWEEN 5 AND 100;
```

- 주소 통계,
- 데이터를 거를때 데이터를 확인하고 처리하면됨
- 하단의 코드는 ~호로 끝나는 주소만 정상이고 나머지는 비정상 주소

```sql
SELECT * FROM copang_main.member WHERE address NOT LIKE '%호';
```

#### column끼리 계산 방법

- 하단은 BMI 계산하는 방법임
- NULL 값이 들어가면 무조건 값이 NULL로 나옴

```sql
SELECT email, height, weight, weight / ((height/100) * (height/100)) FROM copang_main.member;
```

- 이름이 너무 길어서 복잡할땐 별명을 넣을 수 있음
- 넣는 방법은 하단 코드의 AS를 넣어서 사용하면 됨
- alias라고 부르는데 리눅스 사용하면 다들 아시는 그 alias라고 보면 됨 별명이라는 뜻
- AS 없이 한칸띄워도 가능함

```sql
SELECT email, height AS 키, weight AS 몸무게, weight / ((height/100) * (height/100)) AS BMI FROM copang_main.member;
```

- CONCAT 함수를 사용해서도 별명을 넣는게 가능함

```sql
SELECT email, CONCAT(height, "cm", ", ", weight, 'kg') AS '키와 몸무게', weight / ((height/100) * (height/100)) AS BMI FROM copang_main.member;
```

#### 컬럼의 값 변환해서 보는 방법

- ex) 위의 BMI를 재는 과정에서 비만인지 아닌지 판별이 가능한 결과 값이 필요한데,
- 그 값을 보는 방법을 알려준다는 것

- CASE는 "사건/경우" 라는뜻
- WHEN의 줄이 하나의 "조건"라고 보면됨
- THEN은 "반환 값"이라고 보면됨

```sql
SELECT
	email, CONCAT(height, "cm", ", ", weight, 'kg') AS '키와 몸무게', weight / ((height/100) * (height/100)) AS BMI,
	CASE
		WHEN weight IS NULL OR height IS NULL THEN '비만 여부 알 수 없음'
		WHEN weight / ((height/100) * (height/100)) >= 25 THEN '과체중 또는 비만'
		WHEN weight / ((height/100) * (height/100)) >= 18.5
			AND weight / ((height/100) * (height/100)) < 25
			THEN '정상'
		ELSE '저체중'
	END AS obesity_check
FROM copang_main.member ORDER BY obesity_check ASC ;
```

#### 고유값만 보는 방법

- DISTINCT() 함수를 사용하면 고유한 값만 가져온다고 한다.
- 하단의 코드를 예시로 gender의 고유값 즉 남자와 여자에 대한 데이터만 가져오는 것이다.

```sql
SELECT DISTINCT(gender) FROM member;
```

- address를 예시로 들면 모든 row의 값이 고유한 값이기 때문에
- SUBSTRING 함수로 몇자리까지를 나타내는걸로 수정 후 고유값을 가져오는게 맞다.

```sql
SELECT DISTINCT(SUBSTRING(address, 1, 2)) FROM member;
```

#### 문자열 처리 함수들

1. LENGTH()

- 문자열의 길이를 구해줌

```sql
SELECT address, LENGTH(address) FROM member;
```

2. UPPER(), LOWER() 함수

- UPPER 함수는 대문자로 바꿔주는 함수, LOWER 함수는 소문자로 바꿔주는 함수

```sql
SELECT email, UPPER(email) FROM member;
```

```sql
SELECT email, LOWER(email) FROM member;
```

3. LPAD(), RPAD() 함수

- 오른쪽 혹은 왼쪽을 특정 문자열로 채워주는 기능이라고 함

4. TRIM(), LTRIM(), RTRIM() 함수

- 유용하게 쓰일 것 같음
- 왼쪽 오른쪽의 빈칸을 삭제해주는 기능임

```sql
SELECT TRIM(world) FROM test;
```

## GROUPING(그룹으로 여러개 나눈다는 이야기)

- 하단의 코드를 보면 DISTINCT과 비슷하지만 전체적으로 보면 다름
- gender의 기준으로 모든 row들이 원래는 보여주기 때문임

```sql
SELECT gender FROM member GROUP BY gender;
```

- GROUP BY를 함께 진행되면 좀 더 다양한 방식으로 분석이 가능함.

- **Address**같이 고유값이 있는 column은 SUBSTRING 함수로 조절이 필요함

```sql
SELECT SUBSTRING(address, 1, 2) as region, COUNT(*) FROM member GROUP BY SUBSTRING(address, 1, 2);
```

- 여러 column으로 그룹을 나눌 수 있음 하단 처럼

```sql
SELECT SUBSTRING(address, 1, 2) as region, gender, COUNT(*) FROM member GROUP BY SUBSTRING(address, 1, 2), gender;
```

- HAVING 함수를 사용하면 한 그룹의 **특정**그룹만 또 볼 수있게해줌
- 하단의 코드는 서울과 남자인 사람만 보여주는 코드라고 보면됨

```sql
SELECT SUBSTRING(address, 1, 2) as region, gender, COUNT(*) FROM member GROUP BY SUBSTRING(address, 1, 2), gender HAVING region = '서울' AND gender = "m";
```

- NULL 값 삭제 로직

```sql
SELECT SUBSTRING(address, 1, 2) as region, gender, COUNT(*) FROM member GROUP BY SUBSTRING(address, 1, 2), gender HAVING region IS NOT NULL;
```

- NULL 값을 지우고 출력하고, region이 오름차순으로 gender가 내림차순으로 보여주기

```sql
SELECT SUBSTRING(address, 1, 2) as region, gender, COUNT(*) FROM member GROUP BY SUBSTRING(address, 1, 2), gender HAVING region IS NOT NULL ORDER BY region ASC, gender DESC;
```

#### 규칙

- GROUP BY를 사용할 때는, SELECT 절에는

  1. GROUP BY 뒤에서 사용한 컬럼들 또는
  2. COUNT(), MAX() 등과 같은 집계 함수만
     쓸 수 있다는 규칙이 있음.

- 단순히 조회용으로 GROUP BY에 없는 column을 SELECT문 뒤에 사용하는건 안됨 ex) SELECT age
- 근데 집계함수 같은 MAX 혹은 COUNT같은 기능은 추가 가능함 ex) SELECT MAX(age)

#### 부분 총계 WITH ROLLUP

- 부분을 중간중간에 합쳐서 데이터를 알려줌

```sql
SELECT SUBSTRING(address, 1, 2) as region, gender, COUNT(*) FROM member GROUP BY SUBSTRING(address, 1, 2), gender WITH ROLLUP HAVING region IS NOT NULL ORDER BY region ASC, gender DESC;
```

- GROUP BY의 기준의 순서로 WITH ROLLUP의 총계를 알려줌 ex) GROUP BY age, year, gender라면 1. age 2. year 3.gender 마지막 집계 총합 이런식으로 알려준다는 이야기

- 실제로 NULL인지 부분총계를 위해서 나타낸건지 구별하기 힘들때는 GROUPING()함수를 사용하면 된다.
- 0은 부분총계를 위한 NULL 1은 실제 NULL이라는 뜻이다.
- 하단처럼 사용된다.

```sql
SELECT SUBSTRING(address, 1, 2) as region, gender, COUNT(*) FROM member GROUP BY GROUPING(SUBSTRING(address, 1, 2)), GROUPING(gender) WITH ROLLUP HAVING region IS NOT NULL ORDER BY region ASC, gender DESC;
```

#### SQL의 흐름

1. FROM: 어느 테이블을 대상으로 할 것인지를 먼저 결정합니다.
2. WHERE: 해당 테이블에서 특정 조건(들)을 만족하는 row들만 선별합니다.
3. GROUP BY: row들을 그루핑 기준대로 그루핑합니다. 하나의 그룹은 하나의 row로 표현됩니다.
4. HAVING: 그루핑 작업 후 생성된 여러 그룹들 중에서, 특정 조건(들)을 만족하는 그룹들만 선별합니다.
5. SELECT: 모든 컬럼 또는 특정 컬럼들을 조회합니다. SELECT 절에서 컬럼 이름에 alias를 붙인 게 있다면, 이 이후 단계(ORDER BY, LIMIT)부터는 해당 alias를 사용할 수 있습니다.
6. ORDER BY: 각 row를 특정 기준에 따라서 정렬합니다.
7. LIMIT: 이전 단계까지 조회된 row들 중 일부 row들만을 추립니다.

## JOIN

- **외래 키**를 **Foreign Key**라고 부른다.
- 위 설명으로는 **다른 테이블의 특정 row를 식별할 수 있게 해주는 컬럼**이라고 생각하면 된다.
  - ex) 참조를 하는 테이블인 stock 테이블을 '자식 테이블'
  - ex) 참조를 "당하는" 테이블인 item 테이블을 '부모 테이블'
- **Foreign Key**는 다른 테이블의 특정 row를 식별할 수 있어야 하기 때문에 주로 다른 테이블의 **Primary Key**를 참조할 때가 많다.

- join은 연결하다 합치다를 의미
- LEFT OUTER JOIN을 사용하면 item의 데이터와 stock의 데이터를 합쳐서 출력해줌
- RIGHT OUTER JOIN을 사용하면 오른쪽에 있는 stock 테이블의 기준으로 합쳐줌

```sql
SELECT
	item.id,
	item.name,
	stock.item_id,
	stock.inventory_count
FROM item LEFT OUTER JOIN stock
ON item.id = stock.item_id;
```

- NULL로 되어있는 데이터는 item의 데이터가 있지만 stock 테이블에는 없는 데이터를 NULL로 표현함

- join 할때 alias를 사용을 하는 방법은 하단의 코드와 같고
- 꼭 알아야하는건 as로 별명을 지어놨으면 sql의 모든 부분을 수정해줘야한다는 것

```sql
USE copang_main;
SELECT
	i.id,
	i.name,
	s.item_id,
	s.inventory_count
FROM item AS i LEFT OUTER JOIN stock AS s
ON i.id = s.item_id;
```

#### alias의 차이점

- Column의 alias는 조회 결과에서 컬럼 이름을 변경해주는것
- JOIN시 alias는 SQL 문의 전체 길이를 줄여서 가독성을 높이기 위해 사용됨

#### inner Join

- 각 테이블에서 조인 기준으로 기준 column에 같은 값인 것만 연결됨
- 수학으로 치면 교집합이라고 보면됨
- Right OUTER Join하고 결과가 같을 수 있다고함

```sql
USE copang_main;
SELECT
	i.id,
	i.name,
	s.item_id,
	s.inventory_count
FROM item AS i INNER JOIN stock AS s
ON i.id = s.item_id;
```

#### ON 대신에 USING도 사용 가능합니다!

- 하단의 방식대로 사용이 가능함

```sql
USE copang_main;
SELECT
	i.id,
	i.name,
	s.item_id,
	s.inventory_count
FROM item AS i INNER JOIN stock AS s
ON USING(id);
```

#### 합치는(join) 연산

- 테이블을 합치는 연산은 2가지로 나누는데 **결합 연산**과 **집합 연산**으로 나뉨

1. 결합 연산은 테이블을 가로 방향으로 합치는 것에 관한 연산
2. 집합 연산은 테이블을 세로 방향으로 합치는 것에 관한 연산

위에 까지 배운 연사들이 결합 연산에 해당된다고 함.

- 그렇다면 집합 연산이란?
  - c 를 A와 B의 교집합 : A ∩ B
  - a 를 A의 차집합 : A - B
  - b 를 B의 차집합 : B - A
  - (a+b+c) 를 A와 B의 합집합 : A U B

1. A ∩ B (INTERSECT 연산자 사용)

```sql
SELECT * FROM member_A
INTERSECT
SELECT * FROM member_B
```

2. A - B (MINUS 연산자 또는 EXCEPT 연산자 사용)

```sql
SELECT * FROM member_A
MINUS
SELECT * FROM member_B
```

3. B - A (MINUS 연산자 또는 EXCEPT 연산자 사용)

```sql
SELECT * FROM member_B
MINUS
SELECT * FROM member_A
```

4. A U B (UNION 연산자 사용)

```sql
SELECT * FROM member_A
UNION
SELECT * FROM member_B
```

### 활용

왼쪽 테이블을 확인하고싶을때 LEFT OUTER JOIN
오른쪽 테이블을 확인하고싶을때 RIGHT OUTER JOIN
둘다 공통으로 갖고있는걸 확인하기 위해서는 INNER JOIN
전체 합쳐서 전체 상품을 보기위해서 사용하는 UNION

- UNION 함수 사용 시 다른 종류의 테이블도 조회하는 column을 일치시키면 집합 연산이 가능함
- 하단의 코드 방식대로 가능함.

```sql
SELECT id, nation, count FROM Summer_Olympic_Medal
UNION
SELECT id, nation, count FROM Winter_Olympic_Medal
```

#### UNION과 UNION ALL

- UNION은 "교집합에 해당하는 영역의 row들은 중복을 제거하고, 그냥 딱 하나의 row만 보여"준다는 것
- UNION ALL은 두 테이블의 교집합에 해당하는 영역의 row들은 중복을 제거하고, 그냥 딱 하나의 row만 보여준다

- UNION 연산과 UNION ALL 연산은 둘다 합집합을 구하되, 전자는 중복을 제거해서 보여주고, 후자는 그런 작업없이 두 테이블을 합친 결과를 그대로 보여준다는 차이가 있다.
- 깔끔하게 보이는 것이 중요하면 UNION 중복을 제거하면 안된다면 UNION ALL을 사용하면 된다.

#### 3개의 테이블 JOIN하기

- 3개 조인 가능

### 별점 평균 높은것 찾기 (성별 포함)

```sql
SELECT
	i.name,
	i.id,
	r.item_id,
	r.star,
	r.comment,
	r.mem_id,
	m.id,
	m.email
FROM
	item AS i LEFT OUTER JOIN review AS r
		ON r.item_id = i.id LEFT OUTER JOIN member AS m
		ON r.mem_id = m.id;
```

- 여성의 column만 출여서 출력하기

```sql
SELECT
 *
FROM
	item AS i LEFT OUTER JOIN review AS r
		ON r.item_id = i.id LEFT OUTER JOIN member AS m
		ON r.mem_id = m.id
    WHERE m.gender= 'f';
```

- 별점만 조회하는데 내림차순으로 확인하기

```sql
SELECT
 i.id, i.name, AVG(star)
FROM
	item AS i LEFT OUTER JOIN review AS r
		ON r.item_id = i.id LEFT OUTER JOIN member AS m
		ON r.mem_id = m.id
WHERE m.gender= 'f'
GROUP BY i.id, i.name
ORDER BY AVG(star) DESC;
```

- 리뷰의 갯수도 확인하기
- 리뷰 수가 2개 이상인 것만 보여달라 HAVING을 통해서 조회 가능.

```sql
SELECT
 i.id, i.name, AVG(star),COUNT(*)
FROM
	item AS i LEFT OUTER JOIN review AS r
		ON r.item_id = i.id LEFT OUTER JOIN member AS m
		ON r.mem_id = m.id
WHERE m.gender= 'f'
GROUP BY i.id, i.name
HAVING COUNT(*) > 1
ORDER BY AVG(star) DESC;
```

## JOIN 정리 및 추가

- 결합 연산 중에서는

  - LEFT OUTER JOIN
  - RIGHT OUTER JOIN
  - INNER JOIN

- 집합 연산 중에서는
  - INTERSECT
  - MINUS
  - UNION
  - UNION ALL

### 잘 사용하지 않지만 알아두면 좋은 함수

1. NATURAL JOIN

- 두 테이블에서 같은 이름의 컬럼을 찾아서 **자동으로 그것들을 조인 조건**을 설정하고, INNER JOIN을 해주는 조인입니다. 우리말로는 자연 조인이라고 함

```sql
SELECT p.id, p.player_name, p.team_name, t.team_name, t.region FROM player AS NATURAL JOIN team AS t;
```

2. CROSS JOIN

- 한 테이블의 하나의 row에 다른 테이블의 모든 row들을 매칭하고, 그 다음 row에서도 또, 다른 테이블의 모든 row들을 매칭하는 것을 반복함으로써 두 테이블의 row들의 모든 조합을 보여주는 조인

```sql
SELECT * FROM member CROSS JOIN stock;
```

3. SELF JOIN

- 서로 별개인 두 테이블을 조인하는 것처럼 생각하면 됨
- 하나의 테이블 안에서 다양한 정보들을 추출이 가능

```sql
SELECT * FROM member AS m1 LEFT OUTER JOIN member AS m2
ON m1.age = m2.age;
```

4. FULL OUTER JOIN

- 두 테이블의 LEFT OUTER JOIN 결과와 RIGHT OUTER JOIN 결과를 합치는 조인
- 두 결과에 모두 존재하는 row들(두 테이블에 공통으로 존재하던 row들)은 한번만 표현함

5. Non-Equi JOIN

- **"Equi"**는 Equality Condition의 줄임말로 동등 조건을 의미
- 동등 조건이 아닌 다른 종류의 조건을 사용해서 조인을 할 수도 있다.

```sql
SELECT m.email, m.sign_up_day, i.name, i.registration_date FROM copang_main.member AS m LEFT OUTER JOIN copang_main.item AS i ON m.sign_up_day < i.registration_date ORDER BY m.sign_up_day ASC;
```

<br />

## SUBQUERY(서브쿼리)란?

- SQL문 안에 '부품' 처럼 들어가는 SELECT 문
- 전체 sql문 안에 쿼리를 넣는게 서브쿼리임 꼭 ()로 서브쿼리임을 나타내야함

```sql
SELECT i.id, i.name, AVG(star) AS avg_star
FROM item AS i LEFT OUTER JOIN review AS r
ON r.item_id = i.id
GROUP BY i.id, i.name
HAVING avg_star < (SELECT AVG(star) FROM review) // 이처럼 sql을 넣어도 됨 이걸 서브쿼리라고함
ORDER BY avg_star DESC;
```

### SELECT 절에 서브쿼리 사용법

- 원래의 테이블에 없던 새로운 테이블에 추가해서 본다는 의미로 보면됨

```sql
SELECT
  id,
  name,
  price,
  (SELECT MAX(price) FROM item) AS max_price
FROM copang_main.item;
```

### WHERE 절에 서브쿼리 사용법

```sql
SELECT
  id,
  name,
  price,
  (SELECT MAX(price) FROM item) AS max_price
FROM copang_main.item
WHERE price > (SELECT AVG(price) FROM item);
```

- Q ? 상품 중에서 리뷰가 최소 3개 이상 달린 상품들의 정보만 보고 싶으면 어떻게 해야할까?
- 조인을 사용할 수 있지만 서브 쿼리를 통해서도 해결이 가능하다.

```sql
SELECT * FROM item WHERE id IN (
  SELECT item_id
  FROM review
  GROUP BY item_id HAVING COUNT(*) >= 3
);
```

#### ANY와 ALL도 서브쿼리랑 함께 유용하게 쓰임

- ANY는 우리말로 '~중 하나라도'라는 뜻을 가지는 영어 단어
- SOME은 '어떤 하나의~' 라는 뜻을 가진 영어 단어
- 위 두개의 특징 서브쿼리의 결과에 있는 각 row의 값들 중 하나라도 조건을 만족하면 TRUE를 리턴
- ALL은 '모든~' 이라는 뜻
- 모든 경우에 대해서 해당 조건이 성립해야 TRUE를 리턴

```sql
WHERE view_count > ANY(서브쿼리)
WHERE view_count > SOME(서브쿼리)
WHERE view_count > ALL(서브쿼리)
```

#### FROM 절에 있는 서브쿼리

- derived table은 FROM 안의 서브쿼리를 나타냄 alias를 꼭 사용해야함

```sql
SELECT
  AVG(review_count),
  MAX(review_count),
  MIN(review_count),
FROM
  (SELECT
      SUBSTRING() AS region,
      COUNT(*) AS review_count
   FROM review AS r LEFT OUTER JOIN member AS m
   ON r.mem_id = m.id
   GROUP BY SUBSTRING(address, 1, 2)
   HAVING region IS NOT NULL
    AND region != '안드') AS review_count_summary;
```

#### 서브쿼리의 종류

- 단일값을 리턴하는 서브쿼리
- \*스칼라 : 하나의 수치만으로 완전히 표시되는 양으로 벡터 등과 같은 방향의 구별이 없는 수량이다. 예를 들면, 질량, 밀도 따위를 나타내는 수이다. 라고도 불린다.

```sql
SELECT MAX(age) FROM member;
```

- 하나의 column에 여러 row들이 있는 형태의 결과를 리턴하는 서브쿼리

```sql
SELECT SUBSTRING(address, 1, 2) FROM member;
```

- 하나의 테이블 형태의 결과(여러 column, 여러 row)를 리턴하는 서브쿼리
- 서브쿼리로 일시적으로 탄생한 테이블을 derived table

```sql
SELECT * FROM member;
```

### 비상관 서브쿼리와 상관 서브쿼리

- 서브쿼리가 outer query에 적힌 테이블 이름 등과 상관 관계를 갖고 있어서 그 단독으로는 실행되지 못하는 서브쿼리를 상관 서브쿼리라고 합니다.

1. NOT EXISTS
   한 테이블에서 어떤 특정 테이블에 연관된 row가 있는 것들만, 혹은 없는 것들만 추려낼 때는 상관 서브쿼리를 사용하고 그 앞에 EXISTS 또는 NOT EXISTS를 붙이면 됩니다.
2. EXISTS

## 뷰

- 결과 테이블을 **뷰**라는 테이블에 저장이 가능하다.

```sql
CREATE VIEW three_tables_joined AS <기존 서브쿼리>
```
