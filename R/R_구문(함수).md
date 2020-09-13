# R 프로그래밍 언어 _구문

> **R**은 통계 계산과 그래픽을 위한 프로그래밍 언어이자 소프트웨어 환경이자 프리웨어이다.



## 1. 데이터 구조

![image](https://user-images.githubusercontent.com/69948774/93010908-3cc68c80-f5cc-11ea-9354-88e5a0b59706.png)

![image](https://user-images.githubusercontent.com/69948774/93010938-a34baa80-f5cc-11ea-87da-fe6fb927d7aa.png)

	### 1)벡터(Vector)

> 1차원 집합, 하나의 값(스칼라)도 벡터로 취급된다.
>
> 동일 타입의 데이터만으로 구성된다. (문자형(character)>수치형(numeric)>논리형(logical))

- **벡터 생성**

```R
v1 <- 1:10; v1 = 1:10; 1:10 -> v1   #':연산자' 1씩 증가하는 수열
v2 <- c(10, 20, 30)
v3 <- seq(1, 10, 2)   #1~10까지 2씩 증가  >1 3 5 7 9
v4 <- seq(0, 100, length.out=5)   #수열의 길이를 지정  >0 25 50 75 100
v5 <- rep(1:3, 3); rep(1:3, times=3)   #123을 3번 반복  >123123123 
v6 <- rep(1:3, each=3)   #각 원소마다 5번씩 반복  >111222333
```

- **주요 함수**

```R
class()   #데이터 타입
str()   #데이터의 유형 및 구조
summary()   #데이터의 요약 정보
length()   #벡터의 길이
min();max();mean();sum()   #최소;최대;평균;합
rev()   #거꾸로 정렬
range()  #최소값과 최대값을 보여줌
sort()   #오름차순 정렬
sort(x, decreasing = TRUE)   #내림차순 정렬
order()   #sort 결과를 인덱스로 표출
x[x>5 & x<15]   #원소마다 체크
x[x>5 && x<15]   #첫번째 원소만 체크
names(x) <- LETTERS[1:5]   #각각의 원소마다 고유의 이름을 부여할 수 있음
ls()   #생성된 벡터들을 보여줌
rm()   #삭제
which()   #해당값의 인덱스를 알려줌
sample(1:45, 6)   #랜덤;1~45까지 값 중에 6개를 꺼냄
sample(1:45, 6, replace=T)   #중복 가능
paste("I'm", "Duli", "!!")   #아규먼트로 주어진 문자를 하나로 결합
paste("I'm", "Duli", "!!", sep="")   #공백 제거
paste0("I'm", "Duli", "!!")   #결합할 때 중간에 공백을 두지마라
paste(v1, v2)   #각각의 원소끼리 붙여서 결합
paste(v1, v2, sep="", collapse="-")  #각각의 원소 사이에 공백제거, 벡터사이에 - 삽입
nchar()   #각 문자열의 길이
plot()   #산전도를 그려줌
```



### 2) 행렬(Matrix)

> 2차원 벡터이다.
>
> 동일 타입의 데이터만 저장 가능하다.

- **행렬 생성**

```R
matrix(data=1:12, nrow=3, ncol=4)   #1~12까지의 데이터로 3행4열의 행렬을 만듦
matrix(v1, 3, 4, byrow=TRUE)   #행의 방향으로 데이터를 채움
rbind(v1, v2, v3)   #행의 방향으로 결합하여 행렬을 생성
cbind(v1, v2, v3)   #열의 방향으로 결합하여 행렬을 생성
cbind(1:3, 4:6, matrix(7:12, 3, 2))   #벡터와 행렬의 결합도 가능
rbind(matrix(1:6, 2, 3), matrix(7:12, 2, 3))   #행렬과 행렬의 결합도 가능
```

- **주요 함수**

```R
str()   #행렬의 내부 구조 확인
summary()   #데이터에 대한 요약정보를 열단위로 보여줌
dim()   #행과 열의 개수 확인
nrow(); dim()[1]   #행의 개수 확인 
ncol(); dim()[2]   #열의 개수 확인
length()   #벡터의 길이 반환
min();max();mean();sum()   #최소;최대;평균;합
mat[ , 3, drop=FALSE]   #매트릭스 구조를 그대로 유지
rownames <- c("row1","row2","row3")   #행 이름 지정
colnames <- c("col1","col2","col3")   #열 이름 지정
matrix(v, 3, 4, dimnames=list(rownames, colnames))   #행과 열 이름 지정
mat[1:2, 2:3]; mat[ , -1]   #두 개 이상의 행 또는 열을 선택할 때(1열 제외하고 선택)
t()   #전치행렬로의 변환
apply(mat, 1, sum); rowSums()   #3번째 아규먼트로 또다른 함수를 전달할 수 있음
apply(mat, 2, sum); colSums()   #열단위로 합을 구함 (1:행단위, 2:열단위)
apply(mat, 1, mean); rowMeans()   #행단위로 평균
apply(mat, 1, max)   #행단위로 최대값
apply(mat, 2, min)   #열단위로 최소값
```



### 3) 배열(Array)

> 3차원 벡터이다.
>
> 동일 타입의 데이터만 저장 가능하다.

- **행렬 생성**

```R
array(1:30, dim=c(2, 3, 5))   #2행3열5층의 배열을 만듦
```

- **주요 함수**

```R
a1[, 1,2, drop=FALSE]   #차원을 그대로 유지해라
```

- example

```R
a1 <- array(1:12, dim=c(2,3,2))  #2행3열2층
> a1
, , 1

     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6

, , 2

     [,1] [,2] [,3]
[1,]    7    9   11
[2,]    8   10   12

> a1[,,1]
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6

> a1[,2,]
     [,1] [,2]
[1,]    3    9
[2,]    4   10
```



### 4) 팩터(Factor)

> 가능한 범주값(level)만으로 구성되는 벡터

- **팩터 생성**

```R
factor(v)
factor(v, levels=lv)
factor(v, levels=lv, oedered=TRUE)
```

- **주요 함수**

```R
class()   #데이터 타입
summary()   #범주형 데이터로 간주/ 각 데이터의 개수를 출력
levels()   #값의 종류
plot()   #막대그래프를 그려줌
```

- **example**

```R
data <- factor(c("월","수","토","월","목","화"),
               levels=c("일","월","화","수","목","금","토"))
>data
[1] 월 수 토 월 목 화
Levels: 일 월 화 수 목 금 토
>summary(data)
일 월 화 수 목 금 토 
 0  2  1  1  1  0  1 
>levels(data)
[1] "일" "월" "화" "수" "목" "금" "토"
```



### 5) 데이터프레임(Data Frame)

> 행과 열로 구성된 2차원 구조이다.
>
> 각 열이 서로 다른 데이터 유형으로 구성 가능하다.
>
> 모든 열의 데이터 개수(행의 개수)는 동일해야 한다.

- **데이터프레임 생성**

```R
data.frame(v1, v2, v3)   #데이터 프레임 생성
data.frame(row.names=v1, v2, v3)   #행 이름으로 사용할 벡터 지정
data.frame(id=v1, name=v2, price=v3)   #직접 열의 이름을 인수로 지정
as.data.frame(mat or list)   #행렬이나 리스트 형식의 데이터로부터 데이터프레임 생성
```

- **주요 함수**

```R
str()   #데이터프레임의 내부 구조
head()   #열 이름부터 첫 6행까지만 출력
[[ ]]   #열 하나만을 원래의 데이터 형식에 따라 벡터 또는 팩터 형식으로 추출
[  ]   #하나 또는 여러 개의 열을 추출, 선택된 열로 구성된 데이터프레임이 반환
df[,3]; df$price; df[[3]]; df[['price']]   #3(price)열 출력
df[, 3, drop=FALSE]; df[3]   #데이터프레임 형식을 그대로 유지
df[c('name', 'price')]; df[, c('name', 'price')]   #선택열들을 데이터프레임 형식으로 반환
df$var_sum <- df$var1+df$var2   #df 데이터프레임에 sum 열 추가
rbind(df, c('A123', 'USB', 3000)); rbind(df, new.rows)   #새로운 행 추가
cbind(df, madein=c('korea', 'china', 'USA')); df$madein; cbind(df, new.cols)   #새로운 열 추가
do.call(rbind, list)   #리스트 내의 각 원소를 행으로 결합하여 하나의 데이터프레임을 생성
lapply( )   #첫 번째 인수로 주어진 리스트의 각 원소에 대해 두 번째 인수로 주어진 함수를 반복 적용
do.call(rbind, lapply(list, as.data.frame))   #리스트의 각 원소를 데이터프레임으로 변환하고, 행으로 결합
merge(df1, df2)   #두 데이터프레임에 공통의 열이 존재할 때 공통의 열을 기준으로 두 데이터프레임을 결합(공통 열이 일치하는 행만이 선택, 교집합)
merge(df1, df2, all=TRUE)   #두 데이터프레임으로부터 모든 행이 추출(합집합)
subset(df, select=컬럼명들, subset=조건)   #원하는 행과 열 추출
with(iris, sepal.Length / sepal.width)   #데이터프레임 이름과 $ 기호 없이 데이터프레임 내의 열을 직접 참조 가능. but, 함수 내에서 이루어진 변수 할당은 함수 밖에서는 사용 x
```

- **example1**

```R
english <- c(90, 80, 60, 70)
math <- c(50, 60, 100, 20)
classnum <- c(1,1,2,2)
df_midterm <- data.frame(
  english, math, classnum)

> str(df_midterm)
'data.frame':	4 obs. of  3 variables:
 $ english : num  90 80 60 70
 $ math    : num  50 60 100 20
 $ classnum: num  1 1 2 2

> rownames(df_midterm)
[1] "1" "2" "3" "4"

> colnames(df_midterm); names(df_midterm)
[1] "english"  "math"     "classnum"

> mean(df_midterm$english)
[1] 75
```

- **example2**

```R
emp <- read.csv("data/emp.csv")

> emp[emp$ename=="KING",];subset(emp,subset=emp$ename=="KING");subset(emp,emp$ename=="KING")
  empno ename       job mgr   hiredate  sal comm deptno
9  7839  KING PRESIDENT  NA 1981-11-17 5000   NA     10

> emp[!is.na(emp$comm),]; subset(emp,!is.na(emp$comm))
  empno  ename      job  mgr   hiredate  sal comm deptno
2  7499  ALLEN SALESMAN 7698 1981-02-20 1600  300     30
3  7521   WARD SALESMAN 7698 1981-02-03 1250  500     30
5  7654 MARTIN SALESMAN 7698 1981-10-22 1250 1400     30

> subset(emp, select=c("ename","sal"), subset= emp$sal>= 2000)
   ename  sal
4  JONES 2975
6  BLAKE 2850
7  CLARK 2450
8  SCOTT 3000
9   KING 5000
13  FORD 3000
```



### 6) 리스트(List)

> 저장 가능한 데이터의 타입, 데이터 셋의 종류에 제한이 없다.(벡터, 행렬, 배열, 데이터프레임,...)
>
> 서로 다른 구조의 데이터를 하나로 묶을 수 있는 자료구조이다.

- **리스트 생성**

```R
list()   #리스트 생성
```

- **주요 함수**

```R
[ ]   #리스트를 포함한 하위 리스트를 추출
[[ ]]; $   #하위 리스트가 포함한 원소를 추출하고, 계층구조 수준을 한단계 제거  
unlist( )   #리스트를 해제하고 벡터로 반환 
l[1] + 100   #에러:list이기 때문에 연산x
l[[1]] + 100   #원소이기 때문에 연산 가능
names(list(1,2,3)) <- LETTERS[1:3]   #각 데이터 셋마다 이름 부여
```

​         ![img](file:///C:/Users/LimSH/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)  

## 2. 연산자



![image-20200910003110507](C:\Users\LimSH\AppData\Roaming\Typora\typora-user-images\image-20200910003110507.png)



## 3. 데이터 출력

```R
print()   #print 함수는 데이터를 벡터로 인식하기 때문에 출력시 앞에 인덱스를 같이 출력함
print(data, quote=FALSE)   #인용부호 없이 출력
print(data, print.gap=10)   #데이터 간 너비 지정
cat()   #여러개 데이터 값 출력 가능(하나의 문자열로 묶어서 출력), 인용부호 없음
cat(100, 200, "\n")   #개행처리 필요
print(paste("R", "은 통계분석", "전용 언어입니다."))   #"R은 통계분석 전용 언어입니다."
cat("R","은 통계분석","전용 언어입니다.", "\n")   #R은 통계분석 전용 언어입니다.(=paste+print)
```



##  4. 만들어진 데이터셋과 함수 저장

```R
save(list=ls(), file="all.rda")   #all.rda파일에 모두 저장
save(변수명, file="one.rda")   #한 개 저장
rm(list=ls())   #삭제 (지금까지 만들어진 객체 다 사라짐)
load("all.rda")   #읽어오기 (save로 저장한 파일은 반드시 load로 읽기)
```



## 5. 파일에서 데이터 읽어오기

```R
getwd( )   #현재의 작업 디렉토리 위치를 확인 (경로표시 기호: /)
setwd( )   #작업 디렉토리를 변경 ("C:/xxxxx")
read.csv("csv파일 또는 csv를 응답하는 url")   #csv 파일을 읽는데 특화됨, 첫 줄을 header로 읽음
read.csv(file.choose( ))   #원하는 파일을 선택해 옴
scan("data/sample_num.txt")   #숫자를 읽는데 특화됨, word 단위로 읽어줌
scan("data/sample_ansi.txt",what="")   #숫자가 아닌걸 읽을 때
scan("data/sample_utf8.txt", what="",encoding="UTF-8")   #UTF-8로 만들어진 파일을 읽을 때
readLines("data/sample_ansi.txt")   #행 단위로 읽어줌
read.table("일정한 단위(공백또는탭등)로 구성되어 있는 텍스트 파일 또는 URL")
read.table("data/product_click.log", stringsAsFactors = T)   #필요에 따라서 stringsAsFactors 속성 사용/ 문자열로 판단되는 데이터를 TRUE로..
```



## 6. 제어문

> 제어문을 적용하여 수행하려는 명령이 여러 개이면 블록({ })으로 구성한다.

- if 문

```R
if(조건)
	수행명령문장

if(조건){
	수행명령문장1
  }else{
	수행명령문장2
  }

if(조건1){
	수행명령문장1
  }else if(조건2){
	수행명령문장2

  }else if(조건3){
    수행명령문장3
  }else{
    수행명령문장4
  }

ifelse(조건, 조건이 참일 때 명령문1, 조건이 거짓일 때 명령문2)
```

- 반복문
  - [x] 반복문 내에서는 화면에 결과 출력시 출력함수(print, cat)를 사용해야 한다.

```R
for(변수 in 데이터셋)
    수행명령문장

for(변수 in 데이터셋){수행명령문장1; 수행명령문장2}

while(조건)
    수행명령문장

while(조건){
    수행명령문장1
    수행명령문장2
	}

repeat {명령문}(while(TRUE)와 동일) 
  #적어도 한 번 이상 명령문을 실행, 무한 루프에서 벗어나기 위해 분기문을 반드시 포함
```

- 분기문

```R
break   #해당 루프(반복문)를 종료
next   #현재 반복을 종료하고 실행 위치를 다음 반복문으로 이동(continue와 비슷)
```

- switch 함수

```R
switch(EXPR=수치데이터, 식1, 식2, 식3, ...)
switch(EXPR=문자열데이터, 비교값1=식1, 비교값2=식2, 비교값3=, 비교값4=식3,...,식4)

ex1)
num <- sample(1:10,1)
> num
[1] 4
> switch(EXPR = num,"A","B","C","D")
[1] "D"

ex2)
month <- sample(1:12,1)
month <- paste(month,"월",sep="")
> month
[1] "7월"
result <- switch(EXPR=month,
                 "12월"=,"1월"=,"2월"="겨울",
                 "3월"=,"4월"=,"5월"="봄",
                 "6월"=,"7월"=,"8월"="여름",
                 "가을")
> cat(month,"은 ",result,"입니다.\n",sep="")
7월은 여름입니다.
```



## 7. 함수

> 1. 호출 시 함수가 정의하고 있는 매개변수(기본값이 없는) 사양에 맞춰서 아규먼트를 전달해야 한다.
> 2. 리턴값이 없는 함수는 NULL이 리턴된다.
> 3. 리턴값은 return( ) 이라는 함수를 호출하여 처리하며 return( ) 문이 생략된 경우에는 마지막으로 출력된 데이터값이 자동으로 리턴된다. 가급적 리턴함수를 사용하여 명확한 구현 필요
> 4. 아규먼트의 타입을 제한하려는 경우에는 is.xxx() 함수를 활용한다.
> 5. 기본값을 갖는 매개변수를 선언하여 선택적으로 전달되는 아규먼트를 처리할 수 있다.
> 6. 아규먼트의 개수와 타입을 가변적으로 처리 가능하며 리턴값의 경우에도 선택적으로 처리 가능하다.
> 7. 함수 안에서 만들어진 변수는 지역변수이며, 지역변수는 함수내에서만 사용 가능하다.
> 8. 함수 안에서 만들어지지 않은 변수를 사용할 때는 전역 변수를 사용하는 결과가 된다. (전역변수에도 존재하지 않으면 오류 발생)
> 9. 함수 내에서 전역변수에 값을 저장하려는 경우 대입연산자로 '<<-'을 사용한다.

- 함수의 처리 과정

  - 시작(입력) :  매개변수를 통해 아규먼트 값을 받아옴
  - 실행(연산) :  연산, 변환 등..
  - 종료(출력) :  수행결과를 데이터셋으로 반환, 출력 등.. 
- example1

```R
func1 <- function() {    #함수 정의
  x <- 10   
  y <- 20
  return(x*y)
}
> func1()   #함수 호출  / 변수의 값을 출력해주는 기능
[1] 200   
```

- example2

```R
f5<- function(...) {   #매개변수 타입에 제한이 없다
  print("TEST"); 
  data <- c(...); 
  print(length(data))
  }
> f5(10, 20, 30)
[1] "TEST"
[1] 3
> f5("abc", T, 10, 20)
[1] "TEST"
[1] 4
```

- example3

```R
#invisible()함수 
ft.1 <- function(x) return()
ft.2 <- function(x) return(x+10)
ft.3 <- function(x) invisible(x+10)   #호출만 해줄때는 표출 안해줌

> ft.1(100)
NULL
> ft.2(100)
[1] 110
> ft.3(100)   #호출만 해줄때는 표출 안해줌
> print(ft.3(100))
[1] 110
> r3 <- ft.3(100);r3   #변수에 대입하면 표출해줌
[1] 110
```

-  example4

```R
stop("에러 메시지")   #중단하고 에러메시지 띄움
warning("경고 메시지")   #중단하지 않고 경고메시지 띄움
try()   #에러가 나더라도 그 다음을 수행함

testAll <-function(p){   #warning과 error에는 function, finally에는 코드블럭
  tryCatch({
    if(조건1){
        수행명령문장1
    }else if (조건2){
        수행명령문장2
    }else{
        수행명령문장3
    }
  },warning = function(w){
    수행명령문장4
  },error = function(e){
    수행명령문장5
  },finally ={      #오류, 경고 발생 여부에 관계없이 반드시 수행됨
    수행명령문장6
  })
}
```

- 그 외 함수

```R
is.na(x)   #하나의 원소만 체크
any(is.na(x))   #복수형을 처리 가능->하나라도 TRUE이면 조건 만족
all(is.na(x))   #복수형을 처리 가능->모두 다 TRUE일때 조건 만족

sys.sleep(1)   #1초간 쉬어라

```
