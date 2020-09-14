# R  프로그래밍_데이터 수집



## 1. R 패키지

> R을 가지고 할 수 있는 통계, 분석 그리고 시각화와 관련하여 기능을 정의한 함수들의 묶음이다.
>
> R을 설치할 때 함께 설치되는 기본 패키지가 있고 만약 찾는 기능이 없다면 원하는 기능을 처리해주는 패키지를 찾아서 추가로 설치한 후 사용하면 된다.
>
> R 패키지는 CRAN(https://cran.r-project.org/) 사이트에서 모두 검색 가능하고 다운로드 받을 수 있다.

```R
library()   #설치된 패키지 리스트 확인
installed.packages()   #설치된 패키지 리스트(콘솔창에 출력)
search()   #load된 패키지 점검
install.packages("패키지명")   #새로운 R 패키지 설치
remove.packages("패키지명")   #설치된 패키지 삭제
packageVersion("패키지명")   #설치된 패키지의 버전 확인
update.packages("패키지명")   #설치된 패키지 업데이트
library(패키지명)   #설치된 패키지 로드
require(패키지명)   #설치된 패키지 로드
detach("package:패키지명")   #로드된 패키지 언로드
```

```R
excel_data_ex <- read_excel("data/data_ex.xlsx")   #엑셀문서 읽어오는 함수
getwd()
View(excel_data_ex)   #V를 대문자로 써야함
search()
str(excel_data_ex)
```



## 2. 정적 웹 크롤링과 스크래핑

> 웹 스크래핑(web scraping) : 웹 사이트 상에서 원하는 부분에 위치한 정보를 컴퓨터로 하여금 자동으로 추출하여 수집하는 기술

> 웹 크롤링(web crawling) : 자동화 봇(bot)인 웹 크롤러가 정해진 규칙에 따라 복수 개의 웹 페이지를 브라우징 하는 행위

![scrape](C:\Rexam\이미지\scrape.JPG)

- rvest 패키지의 주요 함수

```R
html_nodes(id, css, xpath)   #매칭되는 모든 요소를 반환 
html_node(id, css, xpath)   #매칭되는 한 요소만 반환
html_text(x)   #찾아온 요소의 텍스트를 추출
html_text(x, trim=TRUE)   #Trim:앞뒤에 있는 공백, 탭 등을 없애라
html_attrs(x)   #속성값(attribute)을 추출
html_attrs(x, name, default="")
html_name()   #attribute의 이름을 추출
html_children()   #해당 요소의 하위 요소를 추출
html_tag()   #tag 이름 추출
```

- example1

```R
library(rvest)

url <- "http://unico2013.dothome.co.kr/crawling/tagstyle.html"
text <- read_html(url)   #url에서 html 파일을 읽어와 저장
> text
{html_document}
<html>
[1] <head>\n<meta http-equiv="Content-Type" content="text/html;  ...
[2] <body>\r\n<h1>블럭스타일 태그</h1>\r\n<div style="background-color: ...

nodes <- html_nodes(text, "div")   #태그 선택자; 모든 div태크를 찾아 추출
> nodes
{xml_nodeset (3)}
[1] <div style="background-color:yellow">테스트입니다1</div>
[2] <div>테스트입니다2</div>
[3] <div>테스트입니다3</div>

title <- html_text(nodes)   #찾아온 갯수만큼 그 안의 content만 추출
> title
[1] "테스트입니다1" "테스트입니다2" "테스트입니다3"

node1 <- html_nodes(text, "div:nth-of-type(1)")   #첫번째 div를 꺼내라
> node1
{xml_nodeset (1)}
[1] <div style="background-color:yellow">테스트입니다1</div>

> html_text(node1)
[1] "테스트입니다1"
> html_attr(node1, "style")   #원하는 속성의 값을 꺼내라
[1] "background-color:yellow"

node2 <- html_nodes(text, "div:nth-of-type(2)")
> node2
{xml_nodeset (1)}
[1] <div>테스트입니다2</div>

> html_text(node2)
[1] "테스트입니다2"

> html_attr(node2, "style")  #style이라는 속성이 없어서 NA 출력
[1] NA
```

- example2

```R
# 단일 페이지(rvest 패키지 사용)
url<- "http://movie.naver.com/movie/point/af/list.nhn?page=1"
text <- read_html(url,  encoding="CP949")
> text
{html_document}
<html lang="ko">
[1] <head>\n<meta http-equiv="Content-Type" content="text/html;  ...
[2] <body>\r\n\r\n\r\n\t\r\n\t\r\n\t\r\n\t\r\n\t\r\n\t\t\r\n\t\r ...

# 영화제목
nodes <- html_nodes(text, ".movie")
> html_text(nodes)
 [1] "#살아있다"                             
 [2] "길버트 그레이프"                       
 [3] "오! 문희"                              
 ...

# 영화평점
nodes <- html_nodes(text, ".title em")
> html_text(nodes)
[1] "6"  "10" "10" "2"  "10" "8"  "10" "10" "6"  "1" 

# 영화리뷰
nodes <- html_nodes(text, xpath="//*[@id='old_content']/table/tbody/tr/td[2]/text()")
nodes <- html_text(nodes, trim=TRUE)   #Trim:앞뒤에 있는 공백, 탭 등을 없애라
> nodes[nchar(nodes) > 0]   #null 다 없애기
[1] "유아인의 연기는 훌륭 하지만 이 배역은 안재홍 같은 배우가 연기했으면 더 어울렸을 듯. 약간 모자란 듯 하면서.."                                                                                                                     
[2] "27년이나 된 영화인데 전혀 촌스럽지 않았다. 레오의 연기는 최고였고 조니뎁의 얼굴도 최고였다.  울었다  너무 감동적이었다. 90년대 미국 시골마을 배경도 이뻤고 그냥 다 좋았다."  

[3] "오랜만에 엄마랑 영화보러 갔는데 엄마가 엄청 좋아하시고 웃으셔서 기쁘네여ㅎㅎ 뭉클하기도 하고 웃기기도 하고 눈물흘리며 넘나 재밌게 봤습니다!! ><" 
...

```

