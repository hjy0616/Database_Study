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

## CRUD

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
