---
layout: post
title: SQL Basic Syntax(3)
date: 2021-07-08 23:52:00
---

## WITH clause

저번 시간에
```
SELECT c1, c2 .. from (SELECT c1, c2 from T1) where 1=1;

```
이런식으로 SELECT안에 "Sub-query", 즉 또 다른 SELECT를 쓸 수 있다는걸 알았죠?  
WITH는 그 sub-query의 부분을 가독성을 위해 대체해줍니다.  

```
WITH [<TEMP-NAME>] [<temp-column-name>] AS (SUB-QUERY) [, <TEMP-NAME2> AS ... ]
SELECT <temp-column-name> from <TEMP-NAME> ...

```
이런 식으로 말이죠.  

좋은 점은 뒤에 ,를 통해 with를 계속 이어나갈수도 있다는겁니다.

Sub-query가 multiple-level 이 되는것이죠.

예시를 보면 더 빨리 이해될 것이므로, 예시 하나를 툭 던지겠습니다.
```
WITH dept_emp (emp_id, dept_id, salary) AS (SELECT e.emp_id, d.dept_id, e.salary from EMPLOYMENT AS e JOIN DEPARTMENT AS d ON e.dept_id = d.dept_id) 
SELECT emp_id, dept_id, AVG(salary) FROM dept_emp GROUP BY salary ORDERS BY dept_id;
```
이런식이네요.

그게 답니다.
