# CRUD의 저장, 삭제, 업데이트

> mysql 기준

## Create

- database 생성

```sql
CREATE DATABASE [생성할 데이터베이스 이름];
```

- 생성할때 있는지 확인 후 없다면 생성해줄때

```sql
CREATE DATABASE IF NOT EXISTS [생성할 데이터베이스 이름];
```

### Type

1. Numeric types(숫자형 타입)
   - 숫자를 나타내기 위해서 사용하는 데이터 타입, 숫자형 타입은 **정수**와 **실수**로 나눌 수 있음
   1. 정수형 타입
      - **'TINYINT'**, 최소 -128 ~ 최대 127 까지의 정수를 저장할 수 있는 타입 **SIGNED**는 ‘양수, 0, 음수’를 나타내고, **UNSIGNED**는 ‘0과 양수’를 나타냄
        1. TINYINT SIGNED : -128 ~ 127
        2. TINYINT UNSIGNED : 0 ~ 255
      - **'SMALLINT'**, TINYINT 보다 좀더 큰 범위의 정수를 나타낼 때 쓰는 데이터 타입,
        1. SMALLINT SIGNED : -32768 ~ 32767
        2. SMALLINT UNSIGNED : 0 ~ 65535
      - **'MEDIUMINT'**, 더 넓은 범위를 나타내는 데이터 타입
        1. MEDIUMINT SIGNED : -8388608 ~ 8388607
        2. MEDIUMINT UNSIGNED : 0 ~ 16777215
      - **'INT'**,
        1. INT SIGNED : -2147483648 ~ 2147483647
        2. INT UNSIGNED : 0 ~ 4294967295
      - **'BIGINT'**
        - 진짜 제일 큼
        1. BIGINT SIGNED : -9223372036854775808 ~ 9223372036854775807
        2. BIGINT UNSIGNED : 0 ~ 18446744073709551615
   2. 실수형 타입
      - 소수점이 붙어있는 수를 사용하기도 한다. 이런 수를 저장하기 위한 타입을 실수형 타입
      1. **'DECIMAL'**, 일반적으로 자주 쓰이는 실수형 타입 중 하나로 보통 DECIMAL(M, D)의 형식으로 나타냄
         - M은 최대로 쓸 수 있는 전체 숫자의 자리수이고, D는 최대로 쓸 수 있는 소수점 뒤에 있는 자리의 수를 의미
         - M은 최대 65, D는 30까지의 값을 가질 수 있다, DECIMAL이라는 단어 대신 DEC, NUMERIC, FIXED를 써도 됨
      2. **'FLOAT'**
         - -3.402823466E+38 ~ -1.175494351E-38,
         - 0,
         - 1.175494351E-38 ~ 3.402823466E+38
         - 범위의 실수들을 나타낼 수 있는 데이터 타입입니다. 참고로
         - -3.402823466E+38 은 (-3.402823466) X (10의 38제곱) 을 의미하고
         - -1.175494351E-38 은 (-1.175494351) X (10의 38제곱 분의 1) 을 의미합니다.
      3. **'DOUBLE'**
         - -1.7976931348623157E+308 ~ -2.2250738585072014E-308,
         - 0,
         - 2.2250738585072014E-308 ~ 1.7976931348623157E+308
         - 범위의 실수들을 나타낼 수 있는 데이터 타입입니다. FLOAT에 비해 더 넓은 범위의 수를 나타낼 수 있을 뿐만 아니라, 그 정밀도 또한 더 높은 타입입니다.(소수점 뒤에 최대로 허용가능한 자리수가 더 많음)
           <br>
2. Date and Time types(날짜 및 시간 타입)

   1. DATE
      - 날짜를 저장하는 데이터 타입입니다. 날짜는 ’**2020-03-26**’ 이런 형식의 연, 월, 일 순으로 값을 나타냅니다.
   2. DATETIME
      - 날짜와 시간을 저장하는 데이터 타입입니다. ’**2020-03-26 09:30:27**’ 이런 식으로 연, 월, 일, 시, 분, 초를 나타냅니다.
   3. TIMESTAMP
      - 날짜와 시간을 저장하는 데이터 타입입니다. ’**2020-03-26 09:30:27**’ 이런 식으로 연, 월, 일, 시, 분, 초를 나타냄
      - DATETIME과 차이점은 나라별 타임존을 저장할 필요가 있는지 여부에 따라서 다르다 필요하다면 TIMESTAMP 없다면 DATETIME
   4. TIME
      - 시간을 나타내는 데이터 타입입니다. ’09:27:31’ 형식으로 ‘시:분:초’를 나타냅니다.
        <br>

3. String types(문자열 타입)
   - 문자열을 저장하기 위한 타입입니다. 이름, 댓글, 구매후기 등 문자열 형태의 데이터는 정말 다양하죠? 아래와 같은 타입이 있다.
   1. **'CHAR'**
      - 문자열을 나타내는 기본 타입으로 Character의 줄임말입니다. CHAR(30), 이런 형식으로 나타내는데요. 괄호 안의 숫자는 문자를 최대 몇 자까지 저장할 수 있는지를 나타냅니다. 30이라고 써있으면 최대 30자의 문자열을 저장할 수 있다는 뜻입니다. CHAR 타입의 괄호 안에는 0부터 255까지의 숫자를 적을 수 있습니다.
   2. **'VARCHAR'**
      - 괄호 안에 최소 0부터 최대 65,535 (216 − 1)를 쓸 수 있습니다.
      - 차이점은 CHAR보다 긴거 저장이 가능함
      - CHAR는 고정 길이 타입이고, VARCHAR는 가변 길이 타입
   3. **'TEXT'**
      - 문자열을 저장하는 데이터 타입으로 최대 65535 자까지 저장할 수 있습니다. 이외에도 16,777,215 (224 − 1) 자까지 저장할 수 있는 **MEDIUMTEXT**, 4,294,967,295(232 − 1) 자까지 저장할 수 있는 **LONGTEXT** 타입이 있습니다.

#### Table 생성

- 하단처럼 생성 가능
- 첫 번째, 백틱을 쓰면 어느 단어가 사용자가 직접 이름을 지은 부분인지를 보다 확실하게 나타내줄 수 있기 때문입니다.
- 두 번째, 이미 SQL 문법에 정해진 키워드로 이름을 짓고 싶을 때는 백틱을 쓰는 것이 필수입니다.

```sql
USE course_rating;
CREATE TABLE `student` (
	`id` INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
	`name` VARCHAR(20) NULL,
	`student_number` INT NULL,
	`major` VARCHAR(15) NULL,
	`email` VARCHAR(50) NULL,
	`phone` VARCHAR(15) NULL,
	`admission_date` DATE NULL,
	 );
```

##### row 생성하기

- INSERT INTO 는 ~에 삽입한다 라는 뜻
- ()에 테이블의 column이름 넣어야함
- 모든 값이 들어갈때는 column을 안넣어도 됨

```sql
INSERT INTO student
    VALUES (1, '성태후', 20142947, '컴퓨터 공학과', 'taehos@naver.com', '010-1234-1234', '2014-03-12');
```

- 특정 column에만 row 추가하기
- id는 auto_increment 때문에 자동으로 숫자가 올라가서 제외해도 됨
- 데이터가 없는 부분은 NULL로 처리됨

```sql
INSERT INTO student
        (name, student_number, major, admission_date)
    VALUES ('정유진', 20160843, '빅데이터과', '2013-03-07');
```

###### Unknown column '컬럼명' in 'field list' 에러 해결 방법

- 문자열로 넣었는지 '이게 들어가야할곳에 `(백틱)이 들어간게 아닌지 등등 타입을 잘 확인해야함

```sql
INSERT INTO student
    (id, name, student_number, major, email, phone, admission_date)
    VALUES (1, '성태후', 20142947, '컴퓨터 공학과', 'taehos@naver.com', '010-1234-1234', '2014-03-12');

```

## UPDATE

- update를 사용함 set 설정한다 major column을
- where절을 통해서 특정 id 값을 업데이트 시켜준다고 보면됨
- 여러게 데이터를 수정하려고 해도 하단의 코드처럼 여러가지 column을 넣으면 됨

```sql
UPDATE student SET major = '멀티미디어학과', name = '차소원' WHERE id = 2;
```

- 기존의 값을 활용해서 업데이트 하는 방식이 있다고 함

```sql
UPDATE final_exam_result SET score = 100 WHERE id = 1;  -- 기존의 사용방식
UPDATE final_exam_result SET score = score + 3;
```

## DELETE

- 데이터를 삭제하는 방법
- Delete (삭제한다.) FROM (~로 부터) id = 4인 값을

```sql
DELETE FROM student WHERE id = 4;
```

### row를 삭제하는 2가지 방식

- '물리 삭제'와 '논리 삭제'

1. 물리 삭제
   - 기존 처럼 데이터를 삭제해야할 때 그냥 row를 바로 삭제해버리는 것을 '물리 삭제'라고 함
2. 논리 삭제
   - 삭제해야할 row를 삭제하지 않고, **'삭제 여부'를 나타내는 별도의 컬럼을 두고, 거기에 '삭제되었음'을 나타내는 값을 넣는 것**
   - 논리 삭제는 뒷 column에 삭제되었다는 "표시만" 해둔다는 것.
   - 하단의 코드처럼 is_cancelled라는 column명에 삭제되었다고 Y를 통해서 체크만 해두는 것
   - 이미 데이터 분석에 활용되었거나
   - 고객이 동의한 데이터 보유기간이 지난 row들은
   - 삭제하는 방향으로 간다고함, 하나하나가 중요한 데이터이기 때문임

```sql
UPDATE order SET is_cancelled = 'Y';
```

##### 주의사항이 있음

- **delete는 where절을 꼭 써서 사용해야한다는 점**

## 테이블 다루기 (응용)

#### 테이블의 컬럼 구조, 각 컬럼의 데이터 타입 및 속성을 수정하는 법

- DESCRIBE를 사용하면 테이블의 column 정보를 한눈에 볼 수 있다고함.

```sql
DESCRIBE [Table명]
DESC [Table명] -- 이렇게 줄여서 사용도 가능함
```

### column 추가 및 이름 변경

- ALTER는 테이블을 변경하기 위해서 사용하는 것
- ADD는 추가한다는 의미 하단의 코드는 gender라는 column을 추가한다 라는 뜻

```sql
ALTER TABLE student ADD gender CHAR(1) NULL;
```

- RENAME column은 컬럼의 이름을 변경한다는 의미
- TO를 사용해서 이전의 이름과 변경할 이름을 추가하면 됨 하단처럼

```sql
ALTER TABLE student
    RENAME COLUMN student_number TO registration_number;
```

### column 삭제 및 데이터 타입 변경

- DROP이 SQL에서는 삭제를 의미함

```sql
ALTER TABLE student DROP COLUMN admission_date;
```

- MODIFY 수정하다 라는 의미를 가짐 major column의 데이터 타입을 INT로 수정한다는 의미;
- 기존값이 CHAR 타입이라서 강제로 만약 수정을 한다면 데이터를 전부다 잃게됨 기존 코드들을
- 변경할 타입에 맞춰서 미리 수정 후 타입을 변경하면 잃게되지 않음

- 하단의 수정 코드는 컴퓨터 공학인 데이터들을 10으로 변경한 것

```sql
UPDATE student SET major = 10 WHERE major = '컴퓨터 공학과'; -- 이렇게 수정하고 진행하면 됨
ALTER TABLE student MODIFY major INT;
```

> 위처럼 데이터를 변경하려고 할때 생기는 오류 중 하나가
>
> > Error Code: 1175. You are using safe update mode and you tried to update a table without a WHERE that uses a KEY column. To disable safe mode, toggle the option in Preferences -> SQL Editor and reconnect.
> >
> > > 이런 오류가 존재하는데 이런 건 **safe update** mode 때문임, 해제를 하고 진행을 하면됨

- 속성을 줄때도 modify를 사용해서 기존과 동일하게 진행을 하면됨.

- default 속성을 통해서 기본적인 속성을 넣을 수 있다.

```sql
ALTER TABLE student MODIFY major INT NOT NULL DEFAULT 101;
```

- 날짜는 NOW()를 default에 넣으면 지금 시각의 데이터로 들어간다.

### unique (고유한)

- unique 속성은 중복된 값이 들어가면 안될때 사용된다.

```sql
ALTER TABLE student MODIFY registration_number INT NOT NULL UNIQUE;
```

#### 차이점

- **Primary Key**는 테이블당 오직 하나만 존재할 수 있습니다. 이에 반해 **Unique** 속성은 각각의 컬럼들이 가질 수 있는 속성이기 때문에 한 테이블에 여러 개의 **Unique** 속성들이 존재할 수
- 중요한 차이점 하나는 Primary Key는 NULL을 가질 수 없지만, Unique는 NULL을 허용한다는 점입니다. Primary Key는 애초에 그 목적이 테이블에서 하나의 row를 식별하기 위해 사용되는 컬럼입니다. 하지만 Primary Key에 NULL이 있어버리면, 특정 row를 검색해야할 때 등호(=) 연산을 수행할 수 없기 때문에 NULL을 허용하지 않는 것으로 추측 됨

#### CONSTRAINT (제약사항)

- rule 처럼 데이터를 넣는데 제약을 걸어서 필터링 처리하는 것 하단처럼 사용

```sql
ALTER TABLE student
    ADD CONSTRAINT st_rule CHECK (registration_number < 30000000);
```

- column 순서를 변경하고 싶을때 하단의 코드의 뒷자리처럼 사용함

```sql
ALTER TABLE player_info
    MODIFY id INT NOT NULL AUTO_INCREMENT FIRST;
```

- role 칼럼이 AFTER로 name column 뒤에 나오게 순서 변경하는 것

```sql
ALTER TABLE player_info
    MODIFY role CHAR(5) NULL AFTER name;
```

##### 컬럼의 이름과 컬럼의 데이터 타입 및 속성 동시에 수정하기

- CHANGE로 변경이 가능하다.

1. 이름을 position으로 변경
2. 동시에 그 데이터 타입을 CHAR(5)에서 VARCHAR(2)로, 그 속성도 NULL에서 NOT NULL로 변경.

```sql
ALTER TABLE player_info
    CHANGE role position VARCHAR(2) NOT NULL;
```

##### 한번에 변경하기

- 한번에 변경이 가능함
- 수정하는 것들은 CHANGE로 가능함

```sql
ALTER TABLE player_info
    RENAME COLUMN id TO registration_number,
    MODIFY name VARCHAR(20) NOT NULL,
    DROP COLUMN position,
    ADD height DOUBLE NOT NULL,
    ADD weight DOUBLE NOT NULL;
```

## 테이블 자체를 변경하는 방법

```sql
RENAME TABLE student TO undergraduate;
```

- 복사본 만들기

```sql
CREATE TABLE copy_of_undergraduate AS SELECT * FROM undergraduate;
```

- 테이블 삭제

```sql
DROP TABLE test;
```
