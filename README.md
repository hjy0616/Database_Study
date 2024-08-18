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

## Database

> database create
>
> > ```sql
> >     create database <만들 데이터베이스 명칭>
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
