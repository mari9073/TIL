# 데이터 전처리

> 데이터 분석에 사용할 수 있는 적합한 데이터를 만들기 위한 사전 작업을 데이터 전처리(data preprocessing)라고 한다.



## 1. apply 계열 함수

> R에는 벡터, 행렬 또는 데이터 프레임에 임의의 함수를 적용한 결과를 얻기 위한 apply 계열 함수가 있다. 이 함수들은 데이터 전체에 함수를 한번에 적용하는 벡터 연산을 수행하므로 속도도 빠르고 구현도 간단하다.

| 함수     | 설명                                                         |
| -------- | ------------------------------------------------------------ |
| apply()  | **배열** 또는 **행렬**에 주어진 함수를 적용한 뒤 그 결과를 벡터, 배열 또는 리스트로 반환 |
| lapply() | 벡터, 리스트 또는 표현식에 함수를 적용하여 그 **결과를 리스트로 반환** |
| sapply() | lapply와 유사하나 결과를 가능한 **심플한 데이터셋으로 반환** |
| tapply() | 벡터에 있는 데이터를 특정 기준에 따라 그룹으로 묶은 뒤 각 **그룹마다 주어진 함수를 적용**하고 그 결과를 반환 |
| mapply() | sapply의 확장된 버전으로 **여러 개의 벡터 또는 리스트를 인자로 받아** 함수에 각 데이터의 첫째 요소들을 적용한 결과, 둘째 요소들을 적용한 결과, 셋째 요소들을 적용한 결과 등을 반환 |

```R
# 데이터 전처리(1) - apply 계열의 함수를 알아보자

weight <- c(65.4, 55, 380, 72.2, 51, NA)
height <- c(170, 155, NA, 173, 161, 166)
gender <- c("M", "F","M","M","F","F")

df <- data.frame(w=weight, h=height)
df

apply(df, 1, sum, na.rm=TRUE)   #행단위로 sum 수행 / na.rm=TRUE:na값 무시
apply(df, 2, sum, na.rm=TRUE)   #열단위로 sum 수행
lapply(df, sum, na.rm=TRUE)   #열의 합 결과를 리스트로 반환
sapply(df, sum, na.rm=TRUE)   #심플한 데이터셋인 벡터로 반환
tapply(1:6, gender, sum, na.rm=TRUE)   #M/F 그룹으로 묶어 sum 수행
tapply(df$w, gender, mean, na.rm=TRUE)
mapply(paste, 1:5, LETTERS[1:5], month.abb[1:5]) #각 요소의 순서별로 묶어서 적용


v<-c("abc", "DEF", "TwT")
sapply(v, function(d) paste("-",d,"-", sep=""))

l<-list("abc", "DEF", "TwT")
sapply(l, function(d) paste("-",d,"-", sep=""))   #벡터로 반환
lapply(l, function(d) paste("-",d,"-", sep=""))   #리스트로 반환

flower <- c("rose", "iris", "sunflower", "anemone", "tulip")
length(flower)
nchar(flower)
sapply(flower, function(d) if(nchar(d) > 5) return(d))  #null리턴 때문에 리스트로 반환
sapply(flower, function(d) if(nchar(d) > 5) d)
sapply(flower, function(d) if(nchar(d) > 5) return(d) else return(NA))  #na리턴 때문에 벡터로 반환
sapply(flower, function(d) paste("-",d,"-", sep=""))

sapply(flower, function(d, n=5) if(nchar(d) > n) return(d)) #리스트로 반환
sapply(flower, function(d, n=5) if(nchar(d) > n) return(d), 3) #벡터로 반환
sapply(flower, function(d, n=5) if(nchar(d) > n) return(d), n=4) #리스트로 반환

count <- 1
myf <- function(x, wt=T){
  print(paste(x,"(",count,")"))
  Sys.sleep(1)
  if(wt) 
    r <- paste("*", x, "*")
  else
    r <- paste("#", x, "#")
  count <<- count + 1;
  return(r)
}
result <- sapply(df$w, myf)
length(result)
result
sapply(df$w, myf, F)
sapply(df$w, myf, wt=F)
rr1 <- sapply(df$w, myf, wt=F)
str(rr1)

count <- 1
sapply(df, myf)
rr2 <- sapply(df, myf)
str(rr2)
rr2[1,1]
rr2[1,"w"]
```



## 2. 날짜와 시간 관련 함수

```R
# 데이터 전처리(2) - 날짜와 시간 관련 기능을 지원하는 함수들

(today <- Sys.Date())   #현재 시스템 날짜 정보 "2020-09-23"
format(today, "%Y년 %m월 %d일")   #2020년 09월 23일
format(today, "%d일 %B %Y년")   #23일 9월 2020년
format(today, "%y")   #20
format(today, "%Y")   #2020
format(today, "%B")   #9월
format(today, "%a")   #수
format(today, "%A")   #수요일
weekdays(today)   #수요일
months(today)   #9월 
quarters(today)   #분기 : Q3
unclass(today)   # 1970-01-01을 기준으로 얼마나 날짜가 지났지는 지의 값을 가지고 있다.
Sys.time()   #현재 날짜 및 시간 "2020-09-23 22:00:43 KST"
Sys.timezone()   #"Asia/Seoul"

as.Date('1/15/2018',format='%m/%d/%Y')   #format 은 생략 가능/ 2018-01-15
as.Date('4월 26, 2018',format='%B %d, %Y')   #2018-04-26
as.Date('110228',format='%d%b%y')   #NA
as.Date('110228',format='%d%m%y')   #2028-02-11

x1 <- "2019-01-10 13:30:41"
# 문자열을 날짜형으로
as.Date(x1, "%Y-%m-%d %H:%M:%S")   #2019-01-10
# 문자열을 날짜+시간형으로
strptime(x1, "%Y-%m-%d %H:%M:%S")   #2019-01-10 13:30:41 KST

x2 <- "20200601"
as.Date(x2, "%Y%m%d")
strptime(x2, "%Y%m%d")

#날짜 데이터끼리 연산 가능(하루를 1로 간주, 소수점 생략)
as.Date("2020/01/01 08:00:00") - as.Date("2020/01/01 05:00:00") #Time difference of 0 days
#소수점을 표현하고자 하는 경우
as.POSIXct("2020/01/01 08:00:00") - as.POSIXct("2020/01/01 05:00:00") #Time difference of 3 hours
as.POSIXlt("2020/01/01 08:00:00") - as.POSIXlt("2020/01/01 05:00:00") #Time difference of 3 hours

t<-Sys.time()
ct<-as.POSIXct(t)   #2020-09-23 22:15:46 KST
lt<-as.POSIXlt(t)   #2020-09-23 22:15:46 KST
str(ct)   #POSIXct[1:1], format: "2020-09-23 22:15:46"
str(lt)   #POSIXlt[1:1], format: "2020-09-23 22:15:46" 
unclass(ct)   #1600866947
unclass(lt)
> $sec
[1] 46.92114
$min
[1] 15
$hour
[1] 22
$mday
[1] 23
$mon
[1] 8
$year
[1] 120
$wday
[1] 3
$yday
[1] 266
$isdst
[1] 0
$zone
[1] "KST"
$gmtoff
[1] 32400
attr(,"tzone")
[1] ""    "KST" "KDT"

lt$year+1900   #2020
as.POSIXct(1449894438,origin="1970-01-01")   #2015-12-12 13:27:18 KST
as.POSIXlt(1449894438,origin="1970-01-01")   #2015-12-12 13:27:18 KST

as.POSIXlt("2020/12/25")$wday   #한주 중 몇번째 요일이 되는 날인지 / 5
as.POSIXlt("2020/09/23")$wday   #3

#올해의 크리스마스 요일 2가지방법(요일명,숫자)
christmas2<-as.POSIXlt("2020-12-25")
weekdays(christmas2)
christmas2$wday

#오늘은 xxxx년x월xx일x요일입니다 형식으로 출력
tmp<-Sys.Date()
year<-format(tmp,'%Y')
month<-format(tmp,'%m')
day<-format(tmp,'%d')
weekday<-format(tmp,'%A')
paste("오늘은 ",year,"년 ",month,"월 ",day,"일 ",weekday," 입니다.",sep="")
#오늘은 2020년 09월 23일 수요일 입니다.
format(tmp,'오늘은 %Y년 %B %d일 %A입니다.')
#오늘은 2020년 9월 23일 수요일입니다.
```



## 3. 문자열 처리 관련 주요 함수

```R
# 데이터 전처리(3) - 문자열 처리 관련 주요 함수들 

x <- "We have a dream"
length(x)   #1
nchar(x)    #15

y <- c("We", "have", "a", "dream")
length(y)   #4
nchar(y)    #2 4 1 5

fox.says <- "It is only with the HEART that one can See Rightly"
tolower(fox.says)
toupper(fox.says)

substr("Data Analytics", start=1, stop=4)   #1~4번째 데이터만 추출 / Data
substr("Data Analytics", 6, 14)   #Analytics
substring("Data Analytics", 6)   #Analytics

classname <- c("Data Analytics", "Data Mining", 
               "Data Visualization")
substr(classname, 1, 4)   #각 원소마다 1~4번째 문자 추출 / "Data" "Data" "Data"

countries <- c("Korea, KR", "United States, US", 
               "China, CN")
substr(countries, nchar(countries)-1, nchar(countries))   #"KR" "US" "CN"

head(islands)
landmesses <- names(islands)
landmesses
grep(pattern="New", x=landmesses)   #각각의 원소에서 문자열(New)을 찾아 인덱스로 추출

index <- grep("New", landmesses)   #30 31 32 33 34
landmesses[index]   #"New Britain"  "New Guinea"  "New Zealand (N)" ....   
# 동일
grep("New", landmesses, value=T)  #해당 데이터값을 추출


txt <- "Data Analytics is useful. Data Analytics is also interesting."
sub(pattern="Data", replacement="Business", x=txt)   #첫번째 Data만 바뀜
gsub(pattern="Data", replacement="Business", x=txt)   #모든 Data가 다 바뀜


gsub("[ABC]", "@", "123AunicoBC98ABC")   #123@unico@@98@@@
gsub("ABC", "@", "123AunicoBC98ABC")   #123AunicoBC98@
gsub("(AB)|C", "@", "123AunicoBC98ABC")   #123AunicoB@98@@
gsub("A|(BC)", "@", "123AunicoBC98ABC")   #123@unico@98@@
gsub("A|B|C", "@", "123AunicoBC98ABC")   #123@unico@@98@@@


words <- c("ct", "at", "bat", "chick", "chae", "cat", 
           "cheanomeles", "chase", "chasse", "mychasse", 
           "cheap", "check", "cheese", "hat", "mycat")

grep("che", words, value=T)
grep("at", words, value=T)
grep("[ch]", words, value=T)
grep("[at]", words, value=T)
grep("ch|at", words, value=T)
grep("ch(e|i)ck", words, value=T) #ch로 시작해 ck로 끝나는데 중간에 e또는 i가 들어가는거 / "chick" "check"
grep("chase", words, value=T) 
grep("chas?e", words, value=T)  #? 앞 문자가 0개 or 1개 / "chae"  "chase"
grep("chas*e", words, value=T)  #* 앞 문자가 0개 이상 / "chae" "chase" "chasse" "mychasse"
grep("chas+e", words, value=T)  #+ 앞 문자가 1개 이상/ "chase" "chasse" "mychasse"
grep("ch(a*|e*)se", words, value=T)  #"chase"  "cheese"
grep("^c", words, value=T)  #^ 뒤 문자로 시작하는 문자
grep("t$", words, value=T)  #$ 앞 문자로 끝나는 문자
grep("^c.*t$", words, value=T)  #c로 시작하면서 t로 끝나고 .이 0개 이상인 문자

words2 <- c("12 Dec", "OK", "http//", 
            "<TITLE>Time?</TITLE>", 
            "12345", "Hi there")

grep("[[:alnum:]]", words2, value=TRUE)
grep("[[:alpha:]]", words2, value=TRUE)
grep("[[:digit:]]", words2, value=TRUE)
grep("[[:punct:]]", words2, value=TRUE)
grep("[[:space:]]", words2, value=TRUE)
grep("\\w", words2, value=TRUE)   #alnum과 같음
grep("\\d", words2, value=TRUE)   #digit과 같음 / \\D:숫자 제외하고 꺼내라
grep("\\s", words2, value=TRUE)   #space와 같음



fox.said <- "What is essential is invisible to the eye"
fox.said
strsplit(x= fox.said, split= " ")   #리스트로 리턴 / 공백을 기준으로 쪼개라
strsplit(x= fox.said, split="")   #리스트로 리턴 / 문자를 다 쪼갬

fox.said.words <- unlist(strsplit(fox.said, " "))
fox.said.words
fox.said.words <- strsplit(fox.said, " ")[[1]]
fox.said.words
fox.said.words[3]

p1 <- "You come at four in the afternoon, than at there I shall begin to the  happy"
p2 <- "One runs the risk of weeping a little, if one lets himself be tamed"
p3 <- "What makes the desert beautiful is that somewhere it hides a well"
littleprince <- c(p1, p2, p3)
strsplit(littleprince, " ")
strsplit(littleprince, " ")[[3]] 
strsplit(littleprince, " ")[[3]][5]
```

