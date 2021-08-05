---
layout: post
title: SQL Basic Syntax(4)
date: 2021-07-25 01:52:00
---

## Join 개념 정리

흔히 테이블 두개의 중복한 값을 통해 두 테이블의 정보를 둘 다 가지고 있는 테이블 하나로 조립되어 쓰이는 것으로 우리는 Join을 썼습니다.

이 Join은 implicit하게 Inner Join을 쓰고 있는데요.

사실 실무 상황에서는 left join, right join, full outer join 등등을 다 필요로 할 때가 많습니다.

예를 들어서, 두 테이블이 있다고 칩시다.
Table 1은 Users 테이블입니다. 유저의 이름과 티어 정보를 갖고 있습니다.
Table 2는 Sessions 테이블입니다. 유저의 이름과 마지막으로 통신한 시간에 대한 정보를 가지고 있습니다.

유저가 온라인 오프라인인지의 상태를 알기 위해 Table 2를 이용하는데요.  

만약 방금 회원가입을 끝마친 유저가 온라인으로 돌아 서기 전에,  

### 오프라인 유저의 티어 정보를 얻으려 한다면 어떡할까요?

Outer join의 존재를 모르는 우리로써는..  
오프라인인지 알기 위해서 보통은 Users과 Sessions의 join을 실행하겠죠.  
그렇게 나온 어떤 유저의 테이블에서, 유저 A는 회원가입만하고 request를 아무것도 보내지 않은 상태로, Sessions의 테이블이 비어있습니다. 즉, join을 했을 시 User A는 result table에 나타나지 않게 됩니다.  
(아 물론 회원가입때를 첫번째 request로 만드는 방법도 있긴합니다만.. 여기선 개념 설명을 위해 그냥 넘어갑니다.)

그럼 sessions 테이블에는 안나오는 그 유저의 정보도 얻는 방법이 무엇이냐?  
그게 바로 Outer Join을 쓸 수 있는 때입니다.  
Table1 left join Table2 on Table1.name = Table2.name
이런식으로 하게 되면, Users 테이블의 모든 데이터는 등장하게 되며, Sessions에 존재하지 않는 유저에 대한 session 데이터는 Null로써 등장합니다.  
right join도 같은 개념이지만 Table2의 데이터를 다 등장시킵니다.  

Full outer join은 Table1과 Table2 모든 정보를 다 등장시킵니다.  
개념적으로는 Left join과 Right join의 Union입니다.  
(Union에 중복값을 없애주는 결과가 있으므로 가능)  

MySQL같이 Full outer join을 지원하지 않는 경우에는 left join UNION ALL (<- 중복값도 포함시킴, 더 효율적) right join where name is NULL;의 식으로 쓰입니다.  
right join에서 혹은 left join에서 중복으로 나오는 값을 where name is null의 구문을 사용, exclude 시켜주는 것이죠.  

끝
