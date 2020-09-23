# MariaDB를 활용한 정형 데이터 처리

> R - DBMS 연동 : R이라는 언어로 데이터베이스에 테이블 생성, 데이터 삽입, 데이터 수정, 데이터 삭제, 테이블 삭제, 데이터 추출 등의 작업을 프로그래밍(구현)하는 것을 말한다.

### 1. 데이터베이스와 테이블 생성

> MariaDB 서버에 자료를 저장하기 위해서는 먼저 데이터베이스를 생성하고, 생성된 데이터베이스 안에 테이블을 생성한다. 생성된 테이블에는 자료를 저장, 조회, 수정, 삭제가 가능하다.

- MariaDB에 접속
  - MySQL Client 실행 -> password : bigdata
  - Command Prompt 실행 -> mysql -u root -p -> password : bigdata

```mariadb
MariaDB [(none)]> show databases;   #기본 데이터베이스 보기
MariaDB [(none)]> use test;   #test 데이터 베이스 사용
MariaDB [(test)]> show tables;   #test 데이터베이스 내의 테이블 목록 보기
MariaDB [(test)]> create database work;   #데이터베이스 생성
MariaDB [(test)]> show databases;   #데이터베이스 목록 보기
MariaDB [(test)]> use work;   #work 데이터베이스 사용
MariaDB [(work)]> create table goods (   #테이블 만들기
	-> code int primary key,
	-> name varchar(20) not null,
	-> su int,
	-> dan int);
MariaDB [(work)]> show tales;   #테이블 목록 보기
MariaDB [(work)]> insert into goods values(1, '냉장고', 2, 850000);   #레코드 추가
MariaDB [(work)]> insert into goods values(2, '세탁기', 3, 550000);
MariaDB [(work)]> insert into goods values(3, '전자레인지', 2, 350000);
MariaDB [(work)]> insert into goods values(4, 'HDTV', 3, 1500000);
MariaDB [(work)]> select * from goods;   #테이블에서 레코드 조회
MariaDB [(work)]> create user 'scott'@'localhost'identified by 'tiger';  #사용자 계정 만들기
MariaDB [(work)]> grant all privileges on work.* to 'scott'@'localhost';  #사용자에게 권한 주기
MariaDB [(work)]> flush privileges;
MariaDB [(work)]> quit   #MariaDB와의 연결을 종료하고, 시스템 프롬프트를 표시
```



### 2. R 스크립트에서 MariaDB 데이터 조작

**(1) 드라이버 프로그램 로딩**

- 드라이버 프로그램이랑 R, Java, Python 등의 언어로 DB를 연동할 때 여러가지 세부적인 작업을 대신 수행해주는 프로그램
- `JDBC()`

**(2) MariaDB 데이터베이스에 접속(연결)**

- `dbConnect(Driver, URL, 사용자 아이디, 사용자 비밀번호)`

**(3) 수행하려는 CRUD에 알맞는 SQL 명령을 처리하거나 API에서 제공하는 함수를 이용해서 처리한다.**

- `dbGetQuery()`         #레코드 조회
- `dbSendUpdate()`     #레코드 추가, 수정, 삭제
- `dbWriteTable()`     #데이터프레임을 테이블에 저장
- `dbReadTable()`       #테이블의 내용 전체를 가져옴
- `dbRemoveTable()`   #테이블 삭제

**(4) MariaDB 데이터베이스에 접속 해제**

- `dbDisconnect()`     #MariaDB 연결 종료



**[ example ]**

```R
#1. 데이터베이스로부터 레코드 검색, 추가, 수정, 삭제하기

#MariaDB 연동을 위한 패키지 설치 및 로딩 
install.packages("rJava")
install.packages("RJDBC")
install.packages("DBI")
library(rJava)
library(RJDBC)
library(DBI)

#드라이버 로딩과 MariaDB 데이터베이스 연동
drv <- JDBC(driverClass = 'org.mariadb.jdbc.Driver', 'mariadb-java-client-2.6.2.jar')
conn <- dbConnect(drv, 'jdbc:mariadb://127.0.0.1:3306/work', 'scott', 'tiger')   
#모든 레코드 조회
query <- "select * from goods"
goodsAll <- dbGetQuery(conn, query)   #테이블의 레코드를 조회
dbReadTable(conn, 'goods')   #모든 행과 열을 꺼낼 때 적합
> goodsAll
  code       name su     dan
1    1     냉장고  2  850000
2    2     세탁기  3  550000
3    3 전자레인지  2  350000
4    4       HDTV  3 1500000

#조건검색-수량(su)이 3 이상인 데이터
query <- "select * from goods where su >= 3"   #원하는 요구사항에 맞춰 꺼낼 수 있음
goodsOne <- dbGetQuery(conn, query)   
> goodsOne
  code   name su     dan
1    2 세탁기  3  550000
2    4   HDTV  3 1500000

#정렬검색-단가(dan)를 내림차순으로 정렬
query <- "select * from goods order by dan desc"
> dbGetQuery(conn, query)
  code       name su     dan
1    4       HDTV  3 1500000
2    1     냉장고  2  850000
3    2     세탁기  3  550000
4    3 전자레인지  2  350000

#데이터프레임 자료를 테이블에 저장
insert.db <- data.frame(code=5, name='식기 세척기', su=1, dan=250000)
dbWriteTable(conn, "goods", insert.db)   #에러:이미 존재하는 테이블에 저장할 수 없음
dbWriteTable(conn, "goods1", insert.db)   #goods1 테이블에 데이터프레임 기록
#테이블 조회
query <- "select * from goods1"
goodsAll <- dbGetQuery(conn, query)
> goodsAll
  code        name su    dan
1    5 식기 세척기  1 250000
> dbReadTable(conn, 'goods1')
  code        name su    dan
1    5 식기 세척기  1 250000
```

```R
#2. csv 파일의 자료를 테이블에 저장하기

#파일 자료를 테이블에 저장하기
recode <- read.csv("data/recode.csv")
dbWriteTable(conn, "goods2", recode)
#테이블 조회
query <- "select * from goods2"
goodsAll <- dbGetQuery(conn, query)
> goodsAll
  code        name su     dan
1    1      냉장고  2  850000
2    2      세탁기  3  550000
3    3  전자레인지  2  350000
4    4        HDTV  2 1500000
5    5 식기세척기   1  250000
```

```R
#3. 테이블에 자료 추가, 수정, 삭제하기

#테이블에 레코드 추가
query <- "insert into goods2 values(6, 'test', 1, 1000)"
dbSendUpdate(conn, query)
#테이블 조회
query <- "select * from goods2"
goodsAll <- dbGetQuery(conn, query)
> goodsAll
  code        name su     dan
1    1      냉장고  2  850000
2    2      세탁기  3  550000
3    3  전자레인지  2  350000
4    4        HDTV  2 1500000
5    5 식기세척기   1  250000
6    6        test  1    1000

#테이블의 레코드 수정
query <- "update goods2 set name = '테스트' where code = 6"
dbSendUpdate(conn, query)
#테이블 조회
query <- "select * from goods2"
goodsAll <- dbGetQuery(conn, query)
> goodsAll
  code        name su     dan
1    1      냉장고  2  850000
2    2      세탁기  3  550000
3    3  전자레인지  2  350000
4    4        HDTV  2 1500000
5    5 식기세척기   1  250000
6    6      테스트  1    1000

#테이블의 레코드 삭제
query <- "delete from goods2 where code = 6"
dbSendUpdate(conn, query)
#테이블 조회
query <- "select * from goods2"
goodsAll <- dbGetQuery(conn, query)
> goodsAll
  code        name su     dan
1    1      냉장고  2  850000
2    2      세탁기  3  550000
3    3  전자레인지  2  350000
4    4        HDTV  2 1500000
5    5 식기세척기   1  250000

#테이블 삭제
dbSendUpdate(conn, "drop table goods2")

#MariaDB 연결 종료
dbDisconnect(conn)   #테이블 관련 모든 쿼리문은 수행되지 않음
```

```R
dbWriteTable(conn,"book",data.frame(bookname=c("파이썬 정복","하둡 완벽 입문","R 프로그래밍"),
                                    price=c(25000,25000,28000)))
dbGetQuery(conn, "SELECT * FROM book")


head(mtcars)
str(mtcars)
#dbSendUpdate(conn, "drop table mtcars")
dbWriteTable(conn, "mtcars", mtcars[1:5, ])
dbReadTable(conn, "mtcars")

dbWriteTable(conn, "mtcars", mtcars[6:10, ], append = TRUE)   #존재하는 테이블에추가
dbReadTable(conn, "mtcars")

dbWriteTable(conn, "mtcars", mtcars[1:2, ], overwrite = TRUE)   #수정
dbReadTable(conn, "mtcars")

head(cars)
str(cars)
dbWriteTable(conn,"cars",head(cars,3))
dbGetQuery(conn, "SELECT * FROM cars")

# 데이터 수정
dbSendUpdate(conn,"INSERT INTO cars(speed, dist) VALUES(1,1)")
dbSendUpdate(conn,"INSERT INTO cars(speed, dist) VALUES(2,2)")
dbReadTable(conn,"cars")
dbSendUpdate(conn,"UPDATE CARS SET DIST=DIST*100 WHERE SPEED =1")
dbReadTable(conn,"cars")
dbSendUpdate(conn,"UPDATE CARS SET DIST=DIST*3")
dbReadTable(conn,"cars")

# 테이블 삭제
dbRemoveTable(conn,"cars")
```

