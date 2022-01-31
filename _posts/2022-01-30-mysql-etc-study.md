---
layout: post
title:  "mysql 관련 자주 참고하는 정보등.."
date:   2022-01-30 08:51
categories: archives
tags: mysql etc
---


## DBMS 기능
```text
- DDL (Data Definition Language)
  - 데이터베이스가 어떤 용도이먀, 어떤 식으로 이용될것이라는 것에 대한 정의가 필요함
  - CREATE, ALTER, DROP, RENAME
- DML (Data Manipulation Language)
  - 데이터베이스를 만들었을 때 그 정보를 수정하거나 삭제, 추가, 검색할 수 있어야함
  - SELECT, INSERT, UPDATE, DELETE
- DCL (Data Control Language)
  - 데이터베이스에 접근하고 객체들을 사용하도록 권한을 주고 회수하는 명령
  - GRANT, REVOKE
```


## UML
```text
프로그램 설계를 표현하기 위해 사용하는 그림으로 된 표기법
이해하기 힘든 복잡한 시스템을 의사소통하기 위해 만듬
난 주로 draw.io 사용..
```


## VIEW
```text
허용된 데이터를 제한적으로 보여주기 위한 것
하나의 이상의 테이블에서 유도딘 가상 테이블이다.
- 사용자가 view에 접근했을 때 해당하는 데이터를 원본에서 가져온다.
- view에 나타나지 않은 데이터를 간편히 보호할 수 있는 장점 존재
```


## 정규화
```text
중복을 최대한 줄여 데이터를 주조화하고, 불필요한 데이터를 제거해 데이터를 논리적으로 저장하는 것
```


## 이상현상
```text
릴레이션에서 일부 속성들의 종속으로 인해 데이터 중복이 발생하는 것 (insert, update, delete)
```


## 데이터베이스 무결성
```text
테이블에 있는 모든 행들이 유일한 식별자를 가질 것을 요구함 (같은 값 아님)
외래키 값은 NULL or 참조 테이블의 PK 값이어야 함

- 무결성 보장 (데이터 설계시 가장 중요한 것 중 하나)
데이터를 조작하는 프로그램 내에서 데이터 생성, 수정, 삭제 시 무결성 조건을 검증한다.
```

  
## 트랜잭션
```text
데이터 베이스의 상태를 변화시키키 위해 수행하는 작업 단위
- 원자성 (Atomicity) : 트랜잭션이 db에 모두 반영되거나, 혹은 전혀 반영되지 않아야 된다.
- 일관성 (Consistency) : 트랜잭션의 작업 처리 결과는 항상 일관성 있어야 한다.
- 독립성 (Isolation) : 둘 이상의 트랜잭션이 동시에 병행 실행되고 있을 때, 어떤 트랜잭션도 다른 트랜잭션 연산에 끼어들 수 없다.
- 지속성 (Durability) : 트랜잭션이 성공적으로 완료되었으면, 결과는 영구적으로 반영되어야 한다.
```

## Commit
```text
하나의 논리적 단위(트랜잭션)에 대한 작업이 성공적으로 끝났을때,
이 트랜잭션이 수행한 갱신 연산이 완료된 것을 트랜잭션 관리자에게 알려주는 연산
```


## Rollback
```text
하나의 트랜잭션 처리가 비정상적으로 종료되어 DB의 일관서을 깨뜨렸을 떄, 모든 연산을 취소시키는 연산
- 트랜잭션이 정상적으로 종료되지 않았을때, last consistent state 로 rollback 할 수 있음
```


## 데이터베이스 인덱스
```text
- DBMS에서 저장 성능을 의생하여 데이터 읽기 속도를 높이는 기능
- 테이터가 정렬되어 들어간다
- 종류
  - B+- 트리 인덱스 : 원래의 값을 이용하여 인덱싱
  - Hash 인덱스 : 칼럼 값으로 해시 값 계산하여 인덱싱, 메모리 기반 DB에서 많이 사용
  - B > Hash
- 생성시 고려해야 할 점
  - 테이블 전체 로우 수 15% 이하 데이터 조회시 생성
  - 테이블 건수가 적으면 풀스캔으로 전환해서 조회함
  - 자주 쓰는 컬럼을 앞으로 지정
  - DML 시 인덱스에도 수정 작업이 동시에 발생하므로 DML이 많은 테이블은 인덱스 생성 하지 않음
```


## 기타
```text
- 기본적으로 FK는 지양을 많이 함 (관계는 알지만, 너무 커질수록 복잡해지고 느려짐)
  - ERD 유지를 위해 개발 DB는 FK 를 사용하여 반영하는 방식은 어떨까?
- 카운트성 컬럼에 대해 cacheDB로 관리
- 모든 업데이트 가능성이 있는 테이블은 생성일시, 수정일시 컬럼 필수
- 이미지 경로, description 같은 경우는 text 로 변경 필요
- type 류에 대해 number or string 할지
  - 종류가 많다면 별도 코드테이블 분리? 단순하면 enum string 으로 저장??
```


## recursive 사용 예시
```mysql
WITH RECURSIVE TIMETABLE(HOUR) AS (
  SELECT 0
  UNION
  SELECT TIMETABLE.HOUR + 1 FROM TIMETABLE WHERE TIMETABLE.HOUR < 23
)

SELECT
  HOUR, COUNT(A.ANIMAL_ID)
FROM
  TIMETABLE AS T LEFT JOIN ANIMAL_OUTS AS A ON T.HOUR = HOUR(A.DATETIME)
GROUP BY
  HOUR
ORDER BY
  HOUR
```


## group concat 사용 예시
학생 fullname 출력
```mysql
CREATE TABLE student (id INT PRIMARY KEY, fname VARCHAR(100), lname VARCHAR(100), age INT, dob DATE, department VARCHAR(100));

INSERT INTO student values (1,'Darren', 'Still', 32, '1988-05-20', 'ENGINEERING'), (2,'Abhishek', 'Kumar', 28, '1992-05-20', 'ACCOUNTING'), (3,'Amit', 'Singh', 30, '1990-09-20', 'ENGINEERING'), (4,'Steven', 'Johnson', 40, '1980-05-21', 'HUMAN RESOURCES'), (5,'Kartik', 'Shamungam', 20, '2000-05-12', 'TRAINEE');

SELECT 
    CONCAT(fname,lname) as fullName 
from 
    student
;
SELECT 
    department, 
    GROUP_CONCAT(CONCAT(fname, ' ', lname) order by fname asc SEPARATOR ' | ') as students 
from 
    student 
group by 
    department
;   
```

모든 상품을 구매한 고객 id 추출1(group_concat 사용)
```mysql
with t as (
select 
customer_id, GROUP_CONCAT(DISTINCT product_key order by product_key) as product_key_array
from 
	Customer 
group by 
	customer_id
)
select 
t.customer_id 
FROM 
t 
where 
product_key_array = (select GROUP_CONCAT(DISTINCT product_key order by product_key) as product_key_array from Product)
```



## rownum, partition by 사용 예시
중복건이 존재하는 유형별 최신 시간 2번째 건까지에서, 최신과 그전의 값을 뺀 값을 도출 
```postgresql
SELECT 
X.EVENT_TYPE, SUM(CASE X.ROW_NUM WHEN 1 THEN X.VALUE ELSE -1 * X.VALUE END) AS value 
from
(
	SELECT 
		ROW_NUMBER() OVER( PARTITION BY event_type ORDER BY e.time desc) AS ROW_NUM
		,e.*
	FROM 
	  events e 
	where 
	e.event_type IN (
	
		SELECT 
		event_type
		FROM
		events 
		group by
		event_type
		having count(1) > 1
		
	) 
) X
where
X.ROW_NUM IN (1, 2)
GROUP BY
X.EVENT_TYPE
```


## union all 사용 예시
매치업 결과 나타내기 
```mysql
select
T.team_id,
T.team_name,
IFNULL(SUM(X.points), 0) AS num_points
from 
teams T left outer join (

	select

	host_team as team_id, 
	SUM(
		case when host_goals > guest_goals then 3
		when host_goals = guest_goals then 1
		else 0 end
	) AS points
	
	FROM 
	matches 
	group by host_team 
	
	union ALL 
	
	select
	
	guest_team as team_id, 
	SUM(
		case when guest_goals > host_goals then 3
		when guest_goals = host_goals then 1
		else 0 end
	) AS points
	
	FROM 
	matches 
	group by guest_team 

) X on X.team_id = T.team_id 

group BY 
	X.team_id 
ORDER BY 
	num_points desc, X.team_id 
```

친구 제일 많은 id 찾기
```mysql
select id,
       count(*) num
  from (
        select requester_id id
          from RequestAccepted
         union all
        select accepter_id  id
          from RequestAccepted
       ) r
 group by id
 order by num desc
 limit 1
```

팔로우 1명이상이고, 팔로잉 1명이상 인 사람 구하기
```mysql
with tab1 as (
select followee as follower, count(followee) as num from Follow group by followee
)
select * from tab1 where follower in (select distinct(follower) from Follow) order by follower
```

중복된 숫자가 없는 숫자중 최고값 구하기 (없으면 null로 표현)
```mysql
select
max(num) as num 
from (
	select 
		num, count(*) 
	from 
		MyNumbers
	group by 
		num 
	having count(*) = 1
) temp
```


## function, dense_rank 사용 예시
```text
- RANK 공동 순위만큼 건너뜀 (ex: 1,2,2,4 ...)
- DENSE_RANK 공동 순위를 뛰어넘지 않음 (ex: 1,2,2,3 ...)
- ROW_NUMBER 공동 순위를 무시함 (ex: 1,2,3,4 ...)
```
```mysql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
    RETURN (
      WITH salary_order AS (
        SELECT 
            DISTINCT salary, DENSE_RANK() OVER(ORDER BY salary DESC) AS salary_rank
          FROM Employee
      )
      SELECT salary 
          FROM salary_order 
              WHERE salary_rank = N
      LIMIT 1
  );
END
/*
SELECT
DISTINCT X.Salary 
from 
(
SELECT 
DENSE_RANK() OVER (ORDER BY Salary DESC) as rankNo,
Salary 
FROM 
employee ep 
) X
where X.rankNo = 3
*/
```


## LEAD() over (), LAG() over() 사용 예시
세번 연달아 동일한 숫자가 존재할 경우의 숫자 출력 
```mysql
select
distinct X.num AS ConsecutiveNums
from 
(
    select
        num,
        lead(num, 1) over (order by id) nxt,
        lead(num, 2) over (order by id) nxt2,
        lag(num, 1) over (order by id) pre,
        lag(num, 2) over (order by id) pre2
    from
        Logs
)
X
where X.num = X.nxt and X.nxt = X.nxt2
```


## self join 사용 예시
x,y 좌표 가장 짧은 거리 구하기 (x, y가 겹치지 않는 경우의 수를 구해서 최단거리 공식-sqrt((x2 - x1)2 +(y2 - y1)2) )
```mysql
select 
	ROUND(SQRT(min((p1.x-p2.x)*(p1.x-p2.x)+(p1.y-p2.y)*(p1.y-p2.y))),2) as shortest
from Point2D p1,Point2D p2
where p1.x <> p2.x or p1.y <> p2.y;
```

세번 연달아 동일한 숫자가 존재할 경우의 숫자 출력
```mysql
SELECT 
DISTINCT a.num as ConsecutiveNums
from Logs a, Logs b, Logs c
WHERE 
(a.id = b.id+1 and a.num = b.num)
and (b.id = c.id+1 and b.num = c.num)
```

x 좌표 가장 최소 거리 값 구하기 (거리값 구하기-sqrt(pow((p.x-p1.x),2)))
```mysql
select 
min(abs(abs(a.x) - abs(b.x))) as shortest
FROM 
`Point` a, `Point` b
where
a.x <> b.x
and abs(abs(a.x) - abs(b.x)) <> 0
;
select min(sqrt(pow((p.x-p1.x),2))) as shortest
from `Point` p, `Point` p1
where p.x != p1.x
```



## dense_rank() over (poartion by ... order by) 사용 예시
그룹별 최상의 인원 뽑기
dese_rank 중복된 랭크 이후 넘버링을 안띄어넘고 붙여서 감
```mysql
SELECT 
D.name AS Department,
X.name AS Employee,
X.salary  AS Salary 
FROM 
(
	select
		DENSE_RANK() OVER (PARTITION by e.departmentId ORDER BY e.departmentId, e.salary  DESC) as `rank`,
		e.salary,
		e.name,
		e.departmentId 
	FROM 
		Employee e
)
X, Department D
WHERE 
X.departmentId = D.id 
AND X.rank < 4
;
```


## 조건에 따라서 결과를 만들어 내기 (질문의 파악을 잘해야함..)
주어진 일자 내 블랙리스트 회원,강사가 아닌 예약 건 중 취소율을 구해라.
```mysql
select
    X.request_at AS 'Day',
    round(X.cancelCnt / X.totalCnt, 2) AS 'Cancellation Rate'
FROM 
(
    SELECT 
        t.request_at 
        ,sum(CASE t.status when 'completed' then 0 else 1 end) as  cancelCnt
        ,count(1) as totalCnt

    FROM 
        Trips t 
        left outer join Users cu on t.client_id = cu.users_id 
        left outer join Users du on t.driver_id = du.users_id 
    WHERE 
    cu.banned = 'No'
    and du.banned = 'No'
    and t.request_at between '2013-01-01' and '2013-10-03'
    group by t.request_at
)
X
```

사용자 첫 로그인시점 바로 다음날 재접속한 사람들을 파악해서 전체 접속률 을 소수점 2번째 자리로 구해
```mysql
SELECT
  round(
      count(1) / (select COUNT(DISTINCT B.player_id) from Activity B)
    , 2) as fraction

FROM
  Activity A
WHERE
    (A.player_id, A.event_date) in
    (
      SELECT
        C.player_id, DATE_ADD(min(event_date), INTERVAL 1 day)
      FROM
        Activity C
      group by
        C.player_id
    )
```

회사별 급여의 중간 값을 구하라. (totalCnt/2 and totalCnt/2+1)
```mysql
SELECT 
*
FROM 
(
select
 ROW_NUMBER() over (PARTITION by E.company order by E.salary) as rankNoAsc,
 ROW_NUMBER() over (PARTITION by E.company order by E.salary desc) as rankNoDesc,
 E.*
from 
Employee E
)
X
WHERE abs(rankNoAsc-rankNoDesc) BETWEEN 0 AND 1
;

SELECT 
	company, rankNo, salary 

FROM 
(
select
 E.company,
 E.salary,
 count(1) over (PARTITION by E.company) as totalCnt,
 ROW_NUMBER() over (PARTITION by E.company order by E.salary) as rankNo
from 
Employee E
) X
where X.rankNo BETWEEN totalCnt/2 and totalCnt/2+1
```

숫자와 빈도수를 가지고 중위값을 구하라
```mysql
with t as (
	select 
		*, 
		sum(frequency) over(order by num) freq, 
		(sum(frequency) over())/2 median_num
	from Numbers
)
           
select avg(num) as median
from t
where median_num between (freq-frequency) and freq
```

트리노드별 유형을 구하라
```mysql
select 
T.id,
(case when T.p_id is null then 'Root'
	when (select count(1) from Tree sub where sub.p_id = T.id) > 0 then 'Inner'
	else 'Leaf' end 
) as `type`
from
Tree T
;

select
  id,
  case
    when p_id is NULL then 'Root'
    when id in (select distinct p_id from Tree) then 'Inner'
    else 'Leaf'
    end as `type`
from Tree
```

삼각형이 되기 위한 조건 (길이를 알면 2변을 더한 값이 나머지 1변보다 무조건 큼)
```mysql
select 
x, y, z,
(case 
    when (x+y>z) and (x+z>y) and (z+y>x) then 'Yes'
    else 'No'
 end) as triangle    
from
Triangle
```

모든 상품을 구매한 고객의 아이디 추출2(having 사용)
```mysql
select 
customer_id 
from 
Customer 
Group by 
customer_id 
HAVING COUNT(DISTINCT(product_key)) = (select count(DISTINCT product_key) from Customer)
```



## exist 사용 예시
2015 보험 투자액(모두에게 동일) 조건을 만족하는, 위/경도가 고유한 2016 총 보험 투자액을 구하라.
```mysql
select 
ROUND(sum(tiv_2016), 2) as tiv_2016
from 
Insurance a
where 
	EXISTS (select * from Insurance where pid <> a.pid and a.tiv_2015 = tiv_2015)
	and NOT EXISTS (select * from Insurance where pid <> a.pid and (lat, lon) = (a.lat, a.lon))
```


## multiple in 사용 예시
사용자별 첫 로그인 시점의 기기아이디를 구해라.
```mysql
select player_id, device_id 
from activity 
where (player_id, event_date) in (
                                select player_id, min(event_date)
                                from activity 
                                group by player_id
                                 ) 
```


## sum() over(partition by .. order by ..) 사용 예시
사용자 행별 누적 플레이 포인트 값
```mysql
select 
       player_id, event_date, 
       sum(games_played) over (partition by player_id order by event_date) 
games_played_so_far
from Activity;
```


## count() over(partition by ..) 사용 예시
회사별 보고서를 5명이상에게 받는 매니저 추출
```mysql
SELECT 
    *
FROM 
    Employee e 
where 
    id in (

        select 
            managerId 
        from (
            select
                DISTINCT managerId ,
                count(1) over (partition by managerId) as cnt
            from
                Employee
        ) X 
        where 
        X.cnt > 4
    )
;
select name from employee
where id in
      (select managerId from Employee
       group by managerId
       having count(managerId)>=5)
```


## PIVOT 사용 예시
```mysql
SELECT 
       name, K AS Kakao , N AS Naver , F AS FaceBook 
FROM ( 
    SELECT M.name, M.snsType FROM memberSns AS M 
) AS MemberInfo 
PIVOT ( 
    COUNT(snsType) FOR snsType IN ([K],[N],[F]) 
) AS pivot_result
```

