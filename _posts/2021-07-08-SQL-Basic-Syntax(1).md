---
layout: post
title: SQL Basic Syntax(1)
date: 2021-07-08 19:31:00
---

## RDBMS의 기능
SQL이든 NOSQL이든 목표는 하나, 기본 기능은 다섯가지로 볼 수 있습니다.
목표는 데이터의 취급이죠.  

제일 기본적인 기능은 다음과 같습니다.  
- Create
- Insert
- Update
- Delete
- Select

-----------------
## Create에는 사용자, 테이블 스페이스, 데이터 베이스, 테이블이 있습니다.

### Create User
--------------------------------------
```
CREATE USER <user-name> [PASSWORD 'password'];
\du 로 확인
```

### Create Tablespace
--------------------------------------
```
CREATE TABLESPACE <tablespace-name> [OWNER <owner-name>] LOCATION 'datafile-name';
\db 로 확인
```

### Create Database
--------------------------------------
```
CREATE DATABASE <database-name> [[WITH] [OWNER=<owner-name>] [ENCODING = <characterset-encoding>] [TABLESPACE = <tablespace-name>]];
\dl 로 확인
\c로 connect
```

### Create Table
--------------------------------------
```
CREATE TABLE <table-name> ([<column-name> <data-type>]);
\dt 로 확인
```

### Drop Table
------------------------
```
DROP <table-name>;
```

## Insert
사실, create에서 올바르게 세팅을 한 이후로부터는 올바르게 이용하기만 하면 된답니다.  
Insert로는 테이블에 데이터를 넣는거만 하면 되는거죠.  

### Insert values
------------------------------------------
```
INSERT INTO <table-name> ([<column-name>]) VALUES ([<data-type>]);
```

## Update
Update 또한 데이터셋을 취급하는데 이용합니다.

### Update set
-------------------------------
```
UPDATE <table-name> SET [<column-name> = <data-value>] [WHERE condition];
```

## Delete
Delete는 테이블로부터 몇몇 condition에 부합하는 데이터를 제거합니다.  

### Delete
---------------------------------
```
DELETE FROM <table-name> [WHERE condition];
```

### Truncate
--------------------------------
Delete의 where이 없이 테이블을 비우려고 할 때는 Truncate가 훨씬 효율적입니다. 컨디션을 스캔하지 않기 때문이죠.  
```
Truncate <table-name>
```

## SELECT ..
-------------------------------
SELECT는 여러가지 테이블의 관계를 설명하는것에 아주 유용하기에 다른 포스트에 정리해보도록 하겠습니다.  

끝 마치기 전, 마지막으로 테이블의 구조를 수정할수 있는 방법을 말해보겠습니다.  
테이블의 구조를 수정한다는 것은 쉽게 말하면 column을 수정한다는 것이지요?  

그 방법에는 column의 이름을 바꿀 수도 있을 것이고, column을 넣거나 뺄수도 있겠죠. 물론 데이터 타입과 테이블의 constraint들을 이해한다면 더 많은 기능도 행할 수 있겠습니다. 이는 다음에 다루도록 하고..  

### Alter Rename Column
```
ALTER TABLE <table-name> RENAME COLUMN <current-column-name> TO <new-name>;
```

### Alter Add Column
```
ALTER TABLE <table-name> ADD COLUMN <column-name> <data-type> [constraint];
```

### Alter Drop Column
```
ALTER TABLE <table-name> DROP COLUMN <column-name>;
```

Select는 바로 다음 포스트로 이어집니다.
