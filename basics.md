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

```sql
SELECT <값> FROM member WHERE email = 'taehos@hanmail.net';
```

- 하단의 코드처럼 date타입의 column도 조건을 통해서 값을 가져올 수 있음

```sql
SELECT * FROM member WHERE age >= 27;
SELECT * FROM member WHERE sign_up_day > '2019-01-01';
```

- Between and는 A부터 B까지를 말한것 밑의 코드를 예로 나이가 30살 부터 39살까지의 데이터를 조회한것
- NOT을 추가하면 반대의 의미로써 밑의 코드를 예로 30대 나이는 조회하지말아라 라는 뜻을가짐
- Date타입도 조회가 가능하다.

```sql
SELECT * FROM member WHERE age BETWEEN 30 AND 39;
SELECT * FROM member WHERE age NOT BETWEEN 30 AND 39;
SELECT * FROM member WHERE sign_up_day BETWEEN '2018-01-01' AND '2018-12-31';
```

#### 문자열 패턴매칭

```sql

```

```sql

```
