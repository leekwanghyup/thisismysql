# 6.1 SELECT 문

## 6.1.1 원하는 데이터를 가져와 주는 기본적인 (SELECT ... FROM)

<br>

### USE 구문
```SQL
use employees;
```
- 이제부터는 employees데이터 베이스가 기본적으로 사용된다. 

<br>

### SELECT와 FROM

- 다음 쿼리를 실행하자. 
- 현재 선택된 데이터베이스가 employees이라면 두 쿼리는 동일하다.
```sql
select * from titles;
select * from employees.titles;
```

- 해당 테이블에서 전체 열이 아닌 필요로하는 열만 가져오자. 
```sql
select first_name from employees; 
```
- 여러개의 열을 가져오고싶으면 콤마로 구분한다.
```sql
select first_name, last_name from employees; 
```

> 주석
```sql
-- 한줄 주석
/*
	여러줄 주석 line 1
    여러줄 주석 line 2
*/
```

<br>

> 데이터베이스의 이름, 테이블 이름, 필드 이름이 정확하게 기억나지 않을 때, 이를 찾아서 조회하는 방법을 실습하자.

- 서버에 어떤 데이터 베이스가 있는지 조회한다. 
```sql
show databases;
```
- 우리가 찾던 데이터베이스가 employees라고 가정하자.
```sql
use employees;
```
- 현재 데이터베이스에 있는 테이블 정보를 조회하자.
```sql
show table status;
```
- 우리가 찾던 테이블이 employees이라고 가정하자.
- 이제 employees 테이블에 어떤 열이 존재하는지 조회해보자.
```sql
describe employees; 
```
- 최종적으로 데이터를 조회한다. 
```sql
select first_name, gender from employees; 
```

<br> 

- 열이름에 별칭을 사용하면 결과를 보기가 한결 편해진다.
```sql
select first_name as '이름', gender as '성별' from employees;
```

<br>


- 실습의 전과정에서 사용할 데이터베이스와 테이블을 생성할 것이다. 
- 이번 실습에서 입력한 쿼리를 저장해놓자.
```SQL
drop database if exists sqldb;
create database sqldb; 

use sqldb;

create table usertbl(
	userId char(8) not null primary key, -- 사용자아이디 
    `name` varchar(10) not null, -- 이름 
    birthYear int not null,  -- 출생년도
    addr char(2) not null, -- 주소 
    mobile1 char(3), -- 휴대폰 국번
    mobile2 char(8), -- 나머지 번호 
    height smallint, -- 키 
    mDate date -- 회원가입일
);

create table buytbl (
	`num` int auto_increment not null primary key, 
    userId char(8) not null,
    prodName char(6) not null, 
    groupName char(4), 
    price smallint not null,
    amount smallint not null, 
    foreign key(userId) references usertbl(userID)
); 

select * from usertbl;
select * from buytbl;

INSERT INTO usertbl VALUES ('LSG', '이승기', '1987', '서울', '011', '11111111', '182', '2008-8-8');
INSERT INTO usertbl VALUES ('KBS', '김범수', '1979', '경남', '011', '22222222', '173', '2012-4-4');
INSERT INTO usertbl VALUES ('KKH', '김경호', '1971', '전남', '019', '33333333', '177', '2009-4-4');
INSERT INTO usertbl VALUES ('JYP', '조용필', '1950', '경기', '011', '44444444', '166', '2013-12-12');
INSERT INTO usertbl VALUES ('SSK', '성시경', '1979', '서울',  NULL, NULL, '186', '2009-9-9');
INSERT INTO usertbl VALUES ('LJB', '임재범', '1963', '서울', '016', '66666666', '182', '2005-5-5');
INSERT INTO usertbl VALUES ('YJS', '윤종신', '1969', '경남',  NULL, NULL, '170', '2014-3-3');
INSERT INTO usertbl VALUES ('EJW', '은지원', '1972', '경북', '011', '88888888', '174', '2014-3-3');
INSERT INTO usertbl VALUES ('JKW', '조관우', '1965', '경기', '018', '99999999', '172', '2010-10-10');
INSERT INTO usertbl VALUES ('BBK', '바비킴', '1973', '서울', '010', '00000000', '176', '2013-5-5');

INSERT INTO buytbl VALUES (NULL, 'KBS', '운동화', 'NULL', '30', '2');
INSERT INTO buytbl VALUES (NULL, 'KBS', '노트북', '전자', '1000', '1');
INSERT INTO buytbl VALUES (NULL, 'JYP', '모니터', '전자', '200', '1');
INSERT INTO buytbl VALUES (NULL, 'BBK', '모니터', '전자', '200', '5');
INSERT INTO buytbl VALUES (NULL, 'KBS', '청바지', '의류', '50', '1');
INSERT INTO buytbl VALUES (NULL, 'BBK', '메모리', '전자', '80', '10');
INSERT INTO buytbl VALUES (NULL, 'SSK', '책', '서적', '15', '5');
INSERT INTO buytbl VALUES (NULL, 'EJW', '책', '서적', '15', '2');
INSERT INTO buytbl VALUES (NULL, 'EJW', '청바지', '의류', '50', '1');
INSERT INTO buytbl VALUES (NULL, 'BBK', '운동화', NULL,'30', '2');
INSERT INTO buytbl VALUES (NULL, 'EJW', '책', '서적', '15', '1');
INSERT INTO buytbl VALUES (NULL, 'BBK', '운동화', 'NULL', '30', '2');
```

<br>

### 6.1.2 특정 조건의 데이터만 조회하는 SELECT ... FROM ... WHERE

> WHERE
- 이름이 김경호인 회원 조회
```SQL
select * from usertbl where `name`= '김경호';
```

<br>

> 관계 연산자의 사용 : AND OR NOT
- 1970년 이후 출생했으며 신장이 182이상 회원 조회 
```sql
select * from usertbl where birthYear >= 1970 and height >= 182; 
```
- 1970년 이후에 출생했거나 신장이 182 이상인 사람의 아이디와 이름 조회
```sql
select userId, `name` from usertbl where birthYear >= 1970 or height >= 182 
```
- 조건연산자와 관계연산자를 잘 조합함녀 다양한 조건의 쿼리문을 생성할 수 있다.
    + 조건연산자 : =, <, >, <=, >=, !=
    + 관계연산자 : NOT, AND, OR

<br>

> BETWEEN ... AND 와 IN() 그리고 LIKE
- 키가 180 ~ 183인 사람 조회 
```sql
select * from usertbl where height >=180 and height <= 183; 

-- BETWEEN ... AND 구문은 동일한 결과를 나타낸다.
select * from usertbl where height between 180 and 183; 
```

- 180과 183도 포함하는지 여부 
- 다음 데이터를 삽입한 후 다시 조회해보자.
```SQL
INSERT INTO usertbl VALUES ('LMB', '이명박', '1951', '경북', '011', '58584848', '183', '2013-02-01');
INSERT INTO usertbl VALUES ('KYS', '김영삼', '1929', '경남', '010', '78786363', '180', '2022-01-05');
```
- 180과 183도 포함한다는 것을 알수 있다. 
- 삽입한 데이터를 삭제하자. 
```sql
delete from usertbl where userId = 'KYS';
delete from usertbl where userId = 'LMB';
```

<BR>

- 지역이 '전북'이거나 '경남'인 회원의 정보를 조회하자.
```sql
select * from usertbl where addr = '경남' or addr = '전북' or addr = '경북'; 

-- IN() 구문은 위와 동일한 결과를 나타낸다. 
select * from usertbl where addr in('경남','전북','경북');
```

- 문자열의 내용을 검색하기 위해 LIKE를 사용할 수 있다. 
```sql
select * from usertbl where name like '김%'; 
```
- 맨 앞글자가 한 글자이고 그 다음이 '종신'인 사람을 검색해보자.
```sql
select * from usertbl where name like '_종신'; 
```
- 참고 : %, _ 가 검색할 문자열의 제일 앞에 들어가면 Mysql 성능에 나쁜 영향을 끼칠 수 있다.
- name열에 인덱스가 있더라도 인덱스를 사용하지 않고 데이터 전체를 검색하기 때문이다.

<br>

> ANY / ALL / SOME / 그리고 서브쿼리 

- 김경호보다 키가 큰 회원을 조회해보자.
- 가장 단순한 방법은 다음과 같다.
```SQL
select * from usertbl where name = '김경호'; 
select * from usertbl where height > 177; 
```
- 이를 서브쿼리를 이용해서 나타내면 다음과 같다. 
```sql
select * from usertbl where height > (
    select height from usertbl where name = '김경호'
); 
```

<br> 

- 서브쿼리의 반환값이 둘 이상인 경우에 ANY, IN, SOME, ALL을 사용한다. 
- ANY, IN, SOME은 서브쿼리의 반환값 중 어느만 만족하면된다.
- ALL은 서브쿼리의 반환값을 모두 만족해야한다. 
- 다음 SQL문을 실행하면 오류가 발생한다. 서브쿼리의 반환값이 둘 이상이기 때문이다. 
```SQL
select * from usertbl where height >= (
    select height from usertbl where addr = '경남'
);
```
- ANY를 사용하여 결과값을 확인하자.
```SQL
select * from usertbl where height >= any (
    select height from usertbl where addr = '경남'
);
```
- 서브쿼리의 반환값은 173,170 이다. 
- 173이상 또는 170이상이므로 170이상을 만족하는 회원정보가 검색된다. 
- some을 사용하여 동일한 결과를 나타낼 수 있다 .
```SQL
select * from usertbl where height >= some (
    select height from usertbl where addr = '경남'
);
```






