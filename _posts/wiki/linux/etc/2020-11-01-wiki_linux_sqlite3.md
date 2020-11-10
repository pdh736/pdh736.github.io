---
title: "sqlite3"
excerpt: "How to use sqlite"

categories:
  - linux
tags:
  - wiki
  - linux

toc: true
toc_sticky: true

date: 2020-11-01
last_modified_at: 2020-11-01
---

## sqlite3

### 파일 열기
```
sqlite3 [filename]
```


### 종료
```
.quit
```


### 테이블 이름 보기
```
.tables
```


### 스키마 보기
#### 모든 테이블
```
.schema
```

#### 특정 테이블
```
.schema [table?]
```


### 현재 설정 상태 확인
```
.show
```
```
sqlite> .show 
echo: off 
eqp: off 
explain: auto 
headers: off 
mode: list 
nullvalue: "" 
output: stdout 
colseparator: "|" 
rowseparator: "\n" 
stats: off 
width: 
filename: mydb.sqlite3 
sqlite>
```


### .header  (select 시 컬럼 이름 같이 조회)
```
.header on
.mode column
```
.show 로 확인 가능


### .mode  (데이터 구분)
.mode 명령으로  select문의 실행 결과에 표시되는 데이터 구분을 어떻게 할지 설정할수 있다.
```
.mode [mode?] [table?]
```
.mode 인수 
| 인수   | 설명                                             |
| ------ | ------------------------------------------------ |
| csv    | 쉼표로 구분하여 출력                             |
| column | 컴럼마다 왼쪽 정렬하여 출력                      |
| html   | HTML의 TABLE 형식으로 출력                       |
| insert | INSERT 문으로 출력                               |
| line   | 각 컬럼마다 행을 나누어 출력                     |
| list   | 구분 기호로 컴럼을 구분하여 행으로 출력(default) |
| quote  | SQL 리터럴로 출력                                |
| tabs   | 탭으로 구분하여 출력                             |
| tcl    | TCL list형식으로 출력                            |


#### list
 list 모드는 조회한 데이터를 각 행마다 1행으로 출력한다. 하나의 행에 저장된 데이터는 컬럼마다 현재 설정되어 있는 구분 기호로 구분하여 출력
```
sqlite> .mode list 
sqlite> select * from user; 
1|devkuma|Seoul 
2|kimkc|Busan 
3|araikuma|Seoul
4|happykuma|Seoul 
5|mykuma|Daejeon
```
여기서 구분자는 .setparator 명령으로 변경할 수 있음

#### column
 column 모드는 조회한 데이터를 각 행마다 1행으로 출력한다. 하나의 행에 저당된 데이터는 컬럼마다 왼쪽 정렬하여 출력
``` 
sqlite> .mode column 
sqlite> select * from user; 
1 	devkuma 	Seoul 
2 	kimkc 		Busan 
3 	araikuma 	Seoul 
4 	happykuma 	Seoul 
5 	mykuma 		Daejeon 
```
.width 명령어로 데이터 폭 설정 

#### csv
 csv 모드는 조회한 데이터를 각 행마다 1행으로 출력한다. 하나의 행에 저장된 데이터는 컬럼마다 쉼표(,)로 구분하여 출력
```
sqlite> .mode csv 
sqlite> select * from user; 
1,devkuma,Seoul 
2,kimkc,Busan 
3,araikuma,Seoul 
4,happykuma,Seoul 
5,mykuma,Daejeon 
```

#### tabs
 tabs 모드는 조회한 데이터를 각 행마다 1행으로 출력한다. 하나의 행에 저장된 데이터는 컬럼마다 탭으로 구분하여 출력
 
문자간격 8
```
sqlite> .mode tabs 
sqlite> select * from user; 
1 devkuma Seoul 
2 kimkc Busan 
3 araikuma Seoul 
4 happykuma Seoul 
5 mykuma Daejeon
```

#### line
 line 모드는 조회한 데이터를 한줄씩 처리하고, 컬럼마다 행(row)을 변경하여 표시
```
sqlite> select * from user; 
id = 1 
name = devkuma 
address = Seoul 

id = 2 
name = kimkc 
address = Busan 

id = 3 
name = araikuma 
address = Seoul 

id = 4 
name = happykuma 
address = Seoul 

id = 5 
name = mykuma 
address = Daejeon
```

#### html
 html 모드는 조회한 데이터를 HTML의 TABLE 형식으로 변환하여 표시

```
sqlite> .mode html
sqlite> select * from user; 
<TR><TD>1</TD> 
<TD>devkuma</TD>
<TD>Seoul</TD>
</TR>
<TR><TD>2</TD>
<TD>kimkc</TD>
<TD>Busan</TD>
</TR>
<TR><TD>3</TD>
<TD>araikuma</TD>
<TD>Seoul</TD>
</TR>
<TR><TD>4</TD>
<TD>happykuma</TD>
<TD>Seoul</TD>
</TR>
<TR><TD>5</TD>
<TD>mykuma</TD>
<TD>Daejeon</TD>
</TR>
```



 #### tcl

 tcl 모드는 조회한 데이터를 TCL 목록 형식으로 변환하여 표시

```
sqlite> select * from user;
"1" "devkuma" "Seoul"
"2" "kimkc" "Busan"
"3" "araikuma" "Seoul"
"4" "happykuma" "Seoul" 
"5" "mykuma" "Daejeon"
```



#### insert

insert 모드는 조회한 데이터를 각 행에 데이터를 추가할 때의 INSERT 문 형식으로 표시

```
sqlite> select * from user; 
INSERT INTO "table" VALUES(1,'devkuma','Seoul');
INSERT INTO "table" VALUES(2,'kimkc','Busan');
INSERT INTO "table" VALUES(3,'araikuma','Seoul'); 
INSERT INTO "table" VALUES(4,'happykuma','Seoul'); 
INSERT INTO "table" VALUES(5,'mykuma','Daejeon'); 
```



### .separator

**mode 값이 list 일때** 구분 기호로 사용되는 문자를 변경할 수 있다.

```
.separator [COL?] [ROW?]
```

구분자 문자열로 사용가능

```
.separator /-/
```

구분자 문자열에 공백 포함되어 있을경우 쌍따옴표로 묶어서 작성

```
.separator ", "
```

 

### .width

**mode 값이 column 일때** 컬럼마다 폭 설정

```
.width NUM NUM ....
```

인수는 컬럼마다 폭을 문자로 지정, 여러 컬럼의 너비를 설정하려공백을 두고 계속 작성



지정된 폭으로 데이터가 맞지 않는경우 폭 이상의 문자는 잘려서 표시된다.

#### 데이터 오른쪽 정렬

기본은 왼쪽정렬이지만 폭을 음수로 지정하면 오른쪽 정렬로 데이터를 표시 할 수 있다.

```
.width -4 -8
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI3NTY0MTY4OSwyMDQ2NTc0MTMsMTUzMj
IyNDMzNV19
-->