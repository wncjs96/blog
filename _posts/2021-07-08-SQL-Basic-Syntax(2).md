---
layout: post
title: SQL Basic Syntax(1)
date: 2021-07-08 20:52:00
---

## SELECT를 공부하기 전.

### 사고의 확장
한번 생각해봅시다.  

실제로 서버에서 데이터를 저장해야 할 때를 말이죠.  
기본적인 구조이지만 저번 포스트 떄 배웠던 운영자, 즉 사용자를 일단 만들겠죠.  
사용자를 만들고 나서.. SQL의 테이블을 저장할 주소를 또 정해줘야 되니까. 테이블 스페이스를 정해줄 것이고요.  
그리고나서, 이제 생각을 해볼 겁니다. 어떠한 정보를 저장해야하나..?  

예시로 만약 저희가 회사원의 정보를 저장해본다고 생각하겠습니다.

Employee라는 테이블을 통해서,  
employee ID라는 유니크한 아이디를 만들어 줄 수 있겠구요.  
employee Name으로 이름을 지정해 줄 수도 있겠죠.  
department Name같은 일하는 곳에 대한 정보를 적어 줄 수도 있겠네요.  
salary 또한 정해줄 수 있습니다.  

이 외에도, department의 대한 정보, 즉 위치라던가 아이디 등등, salary에 따라 들어가는 band 영역까지 정해줄 수 있겠는데요.  
이 모든 내용을 하나의 테이블에 저장한다면 어떻게 보일까요..  

```
Employee Table
E_Id | E_Name | D_Name | Salary | D_loc | D_ID | band
1	James	IT	 1000	  MD	  1	 1
2	Jane	IT	 1500	  MD	  1	 1
```
한눈에 들어올지는 모르겠지만, D_Name, D_loc, D_ID과 Salary, band 들은 이 중 하나만 들어가있어도 무방할 정도로 서로 이어져있습니다.

이럴 경우, 같은 데이터가 반복적으로 들어가는걸 막기위해서는 테이블을 나눠주는 편이 더 효율적입니다.  

```
Employee Table
E_Id | E_Name |  Salary | D_ID 
1	James	 1000	   1	
2	Jane	 1500	   1

Department Table
D_ID	|D_Name	|	D_loc
1	 IT		MD
2	 Sales		WA

Band Table
salary	|band
1000	|1
2000	|2
```
이런식으로 말이죠.

자 그럼 효율적인 테이블의 분배는 알겠다 이겁니다.  
근데 왜 한겁니까?  

예를들어서 James의 department location을 알아내지도 못할거면 왜 했냐는 말입니다.
Employee 테이블에서 얻을 수 있는 내용은 D_ID밖에 없는데 어떻게 알아내냐구요!!

### D_ID에서 D_Loc을 얻어낼 방법이 있습니까?
-----------------------------
같은 칼럼을 쓰는 테이블을 합치면 됩니다.  
그리고 이 합치는 행위를 "JOIN" 이라고 합니다.  

필요한 정보가 들어있는 하나의 테이블을 원래의 기준 테이블 옆에 붙이는 형태로서 맨 처음 만들었던 모든 정보가 하나의 테이블에 들어있던 구조를 만들어 주는 것이지요.  

### JOIN ON
```
SELECT [<column-name>] FROM <TABLE1> JOIN <TABLE2> ON <TABLE1.column-name> = <TABLE2.column-name>;

SELECT * FROM EMPLOYEE JOIN DEPARTMENT ON EMPLOYEE.D_ID = DEPARTMENT.D_ID;
```
이렇게 만들어진 테이블에서는 평소 SELECT를 진행하면 됩니다.

그 평소 SELECT는 어떻게 진행하냐고요?  

## SELECT FROM TABLE

여기서부터는 진짜 SELECT에 대해서 설명합니다.

SELECT의 문법은 다음과 같습니다.

```
SELECT [<column-name>] FROM <table-name> [WHERE condition] [GROUP BY <column-name>] [ORDER BY <column-name>];
```

처음 괄호부터 몇가지 팁을 드리자면, column-name에는 aggregate functions라는 녀석들이 쓰여질 수 있다는 것입니다.  
그 중에는
- min() : 최솟값을 값으로한 row만 남습니다
- max() : 최댓값을 값으로한 row만 남습니다
- count() : 몇개의 rows들이 있는지를 나타낸 값의 row만 남습니다.
- sum() : integer 값들의 합을 나타낸 값의 row만 남습니다.
- avg() : integer 값들의 average 값의 row만 남습니다.
라는 것들이 있으며, 더 찾아보실려면 (functions-aggregate)[https://www.postgresql.org/docs/9.5/functions-aggregate.html]에서 확인하시길 바랍니다.  

그리고 추가적으로, aggregate 을 썼을경우, select는 뒤에 GROUP BY <column-name> 이라는 녀석을 추가시켜줘야 합니다.  
이를 추가시켜줌으로서, 어떤 column으로 aggregate을 하는지 알아야하니까요.

자 두번째 괄호는 table-name이라고 되어있는데요. 이것 또한 팁이 존재합니다.  

Select의 return 값은 Table입니다. 그 말은 즉슨, <table-name> 안에 또 다른 SELECT가 들어갈 수 있다는것이죠.

```
SELECT gender, SUM(qty) FROM (
SELECT gender, qty FROM orders JOIN users ON orders.user_id = users.user_id
)
GROUP BY gender;
```
이런것이 가능해지는 것이죠.

그게 답니다.

WITH라는 녀석도 있는데 그 녀석에 대한 것도 다음 시간에 다뤄보죠.
