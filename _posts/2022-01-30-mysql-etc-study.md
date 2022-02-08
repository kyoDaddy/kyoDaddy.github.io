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


## 인덱스 사용 조건
```text
- group by 
    - gorup by 절에 명시된 칼럼이 인덱스 칼럼의 순거와 위치가 같아야 함
- 칼럼이 아니라 상수를 변형하는 형태로 쿼리를 작성해야 한다.
- 저장하고자 하는 값의 타입에 맞춰 칼럼의 타입을 선정하고, SQL을 작성할 때는 데이터의 타입에 맞춰서 비교 조건을 사용하길 권장
- datetime 과 date 타입의 비교에서 타입 변환은 인덱스 사용 여부에 영향을 미치지 않는다.  
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


## mysql-8.0 사용 예시
### recursive 사용 예시
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

### group concat 사용 예시
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


### rownum, partition by 사용 예시
중복건이 존재하는 유형별 최신 시간 2번째 건까지에서, 최신과 그전의 값을 뺀 값을 도출 
```mysql
SELECT 
    X.EVENT_TYPE, SUM(CASE X.ROW_NUM WHEN 1 THEN X.VALUE ELSE -1 * X.VALUE END) AS value 
FROM
(
	SELECT 
		ROW_NUMBER() OVER( PARTITION BY event_type ORDER BY e.time desc) AS ROW_NUM
		,e.*
	FROM 
    events e 
	where 
	  e.event_type IN (
		    SELECT event_type FROM events group by event_type having count(1) > 1
	  ) 
) X
where
    X.ROW_NUM IN (1, 2)
GROUP BY
    X.EVENT_TYPE
```


### union 사용 예시
```mysql
SELECT 
    DATE_FORMAT(trans_date, '%Y-%m') AS month, country, 
    SUM(state = 'approved') AS approved_count, 
    SUM(IF(state = 'approved', amount, 0)) AS approved_amount, 
    SUM(state = 'chargeback') AS chargeback_count, 
    SUM(IF(state = 'chargeback', amount, 0)) AS chargeback_amount
FROM (
	SELECT id, country, 'chargeback' AS state, amount, Chargebacks.trans_date
	  FROM Chargebacks LEFT JOIN Transactions ON trans_id = id
  UNION
	SELECT *
	FROM Transactions
	WHERE state = 'approved'
) AS t1
GROUP BY 1,2
```


### union all 사용 예시
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
	group by 
	  host_team 
	
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
	group by 
	  guest_team 

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

중복된 숫자가 없는 숫자중 최고값 구하기 (없으면 null로 표현) -> having 사용
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
	having 
    count(*) = 1
) temp
```



### function, dense_rank 사용 예시
```text
- RANK 공동 순위만큼 건너뜀 (ex: 1,2,2,4 ...)
- DENSE_RANK 공동 순위를 뛰어넘지 않음 (ex: 1,2,2,3 ...)
- ROW_NUMBER 공동 순위를 무시함 (ex: 1,2,3,4 ...)
```
DENSE_RANK 사용
```mysql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
    RETURN (
      WITH salary_order AS (
        SELECT DISTINCT salary, DENSE_RANK() OVER(ORDER BY salary DESC) AS salary_rank
        FROM Employee
      )
      SELECT salary FROM salary_order WHERE salary_rank = N LIMIT 1
    );
END
```

학생별 과목 중 가장 큰 점수 구하기 (ROW_NUMBER 사용)
```mysql
select
  X.student_id, X.course_id, X.grade
from
  (
    select
      student_id,
      course_id,
      grade,
      row_number() over (partition by student_id order by grade desc, course_id) as rn
    from
      Enrollments
  )
    X
where
  X.rn = 1
```


### LEAD() over (), LAG() over() 사용 예시
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
) X
where 
    X.num = X.nxt and X.nxt = X.nxt2
```


### self join 사용 예시
x,y 좌표 가장 짧은 거리 구하기 (x, y가 겹치지 않는 경우의 수를 구해서 최단거리 공식-sqrt((x2 - x1)2 +(y2 - y1)2) )
```mysql
select 
	ROUND(SQRT(min((p1.x-p2.x)*(p1.x-p2.x)+(p1.y-p2.y)*(p1.y-p2.y))),2) as shortest
from Point2D p1,Point2D p2
where p1.x <> p2.x or p1.y <> p2.y
```

세번 연달아 동일한 숫자가 존재할 경우의 숫자 출력
```mysql
SELECT 
    DISTINCT a.num as ConsecutiveNums
from 
    Logs a, Logs b, Logs c
WHERE 
    (a.id = b.id+1 and a.num = b.num)
    and (b.id = c.id+1 and b.num = c.num)
```

x 좌표 가장 최소 거리 값 구하기 (거리값 구하기-sqrt(pow((p.x-p1.x),2)))
```mysql
select min(sqrt(pow((p.x-p1.x),2))) as shortest
from `Point` p, `Point` p1
where p.x != p1.x
```

연속 번호의 시작 및 끝 번호를 찾아라.
```mysql
SELECT min(log_id) as start_id, max(log_id) as end_id
FROM
(
    SELECT 
           log_id, ROW_NUMBER() OVER(ORDER BY log_id) as num 
    FROM Logs
) a
GROUP BY log_id - num
```


### cross join 사용 예시
```mysql
with t as (
  select
    *
  from
    Students s cross join Subjects b
)
select
  t.student_id,
  t.student_name,
  t.subject_name,
  SUM(if(e.subject_name is null, 0, 1)) as attended_exams
from
  t left join Examinations e using(student_id, subject_name)
group by
  1,	2,	3
order by
  1,	3
```


### dense_rank() over (poartion by ... order by) 사용 예시
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
		DENSE_RANK() OVER (PARTITION by e.departmentId ORDER BY e.departmentId, e.salary DESC) as `rank`,
		e.salary,
		e.name,
		e.departmentId 
	FROM 
		Employee e
) X, Department D
WHERE 
    X.departmentId = D.id 
    AND X.rank < 4
```


### 조건에 따라서 결과를 만들어 내기 (질문의 파악을 잘해야함..)
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
    group by 
        t.request_at
) X
```

사용자 첫 로그인시점 바로 다음날 재접속한 사람들을 파악해서 전체 접속률 을 소수점 2번째 자리로 구해 (count(distinct) 사용)
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
) X
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
    case when T.p_id is null then 'Root'
        when (select count(1) from Tree sub where sub.p_id = T.id) > 0 then 'Inner'
        else 'Leaf' end as `type`
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
    case 
        when (x+y>z) and (x+z>y) and (z+y>x) then 'Yes'
        else 'No'
        end as triangle    
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
HAVING 
    COUNT(DISTINCT(product_key)) = (select count(DISTINCT product_key) from Customer)
```

총판매금액이 제일 높은 판매자 전부 찾아라
```mysql
WITH cte as (
    select seller_id, sum(price) as total_sell_price
    from Sales 
    group by seller_id
)
select seller_id  from cte where total_sell_price = (select max(total_sell_price) from cte)
```

S8은 샀지만 아이폰은 사지 않은 구매자 목록 찾아라
```mysql
SELECT s.buyer_id
FROM Sales AS s INNER JOIN Product AS p using (product_id)
GROUP BY s.buyer_id
HAVING SUM(CASE WHEN p.product_name = 'S8' THEN 1 ELSE 0 END) > 0
    AND SUM(CASE WHEN p.product_name = 'iPhone' THEN 1 ELSE 0 END) = 0
```

2019-01-01 ~ 2019-03-30 사이에만 팔린 상품을 찾아라
```mysql
-- 해당 상품이 2019-04-1 이후 팔린 적이 있는지 조회
with t as (
  select
    s.product_id
  from
    Sales s
  where
    (s.sale_date >= '2019-04-01' or s.sale_date <= '2018-12-31')
)
-- sale-date 2019-01-01 ~ 2019-03-30 내 팔린 품목 조회
select
  DISTINCT s.product_id, p.product_name
from Sales s inner join Product p using (product_id)
where s.sale_date >= '2019-01-01' and s.sale_date <= '2019-03-30'
  and s.product_id not in (select t.product_id from t)
;

-- 이렇게 생각을 못함..
select p.product_id, p.product_name
from Product p
join Sales s
on p.product_id = s.product_id
group by s.product_id
having min(s.sale_date)>="2019-01-01" and max(s.sale_date)<="2019-03-31"
```

일자별 첫설치 횟수 및 다음날 재로그인률 추출
```mysql
-- player 별 instal_dt, count 추출
with t as (
	select 
		player_id, min(event_date) as install_dt, count(event_date) as totalCnt
	FROM 
		Activity
	group by
		player_id
)
select 
    t.install_dt,
    t.totalCnt,
    round(avg(case when a.player_id is not null then 1 else 0 end), 2) as day1
from t left join Activity a on t.player_id = a.player_id and DATE_ADD(t.install_dt, INTERVAL 1 day) = a.event_date 
group by 1
```

일자별 유형별 총금액, 총인원 구하기 - 일자별 유형 테이블 만들어서 조인
```mysql
SELECT 
    p.spend_date,
    p.platform,
    IFNULL(SUM(amount), 0) total_amount,
    COUNT(user_id) total_users
FROM 
(
    SELECT DISTINCT(spend_date), 'desktop' platform FROM Spending
    UNION
    SELECT DISTINCT(spend_date), 'mobile' platform FROM Spending
    UNION
    SELECT DISTINCT(spend_date), 'both' platform FROM Spending
) p 
LEFT JOIN (
    SELECT
        spend_date,
        user_id,
        IF(mobile_amount > 0, IF(desktop_amount > 0, 'both', 'mobile'), 'desktop') platform,
        (mobile_amount + desktop_amount) amount
    FROM (
        SELECT
          spend_date,
          user_id,
          SUM(CASE platform WHEN 'mobile' THEN amount ELSE 0 END) mobile_amount,
          SUM(CASE platform WHEN 'desktop' THEN amount ELSE 0 END) desktop_amount
        FROM Spending
        GROUP BY spend_date, user_id
    ) o
) t
ON p.platform=t.platform AND p.spend_date=t.spend_date
GROUP BY spend_date, platform
```

스팸 삭제율을 구하라 (count(distinct ..) 사용)
```mysql
SELECT 
    ROUND(avg(remove_count) * 100, 2) as average_daily_percent 
FROM 
(
    SELECT 
        a.action_date, 
        COUNT(DISTINCT r.post_id) / COUNT(DISTINCT a.post_id) as remove_count
    FROM 
        Actions a LEFT JOIN Removals r ON a.post_id = r.post_id 
    WHERE 
        extra = 'spam'
    GROUP BY 
        a.action_date 
) X
```

같은 날짜에 둘 이상의 기사를 본 모든 사용자
```mysql
select
    distinct viewer_id as id
from
    Views
group by
    viewer_id, view_date
having
    count(distinct article_id) > 1
```

즉시 주문 비율 구하기 (true=1, false=0 사용)
```mysql
select 
     round(100*avg(order_date = customer_pref_delivery_date),2) as immediate_percentage 
from 
     Delivery
```

--  상품의 일자기준 단가표 기준 평균 판매가격 추출
```mysql
select
  u.product_id,
  round( (sum(u.units * p.price) / sum(u.units)), 2) as average_price
from
  UnitsSold u
  inner join Prices p on p.product_id = u.product_id and u.purchase_date between p.start_date and p.end_date
group by
  u.product_id
```


### exist 사용 예시
2015 보험 투자액(모두에게 동일) 조건을 만족하는, 위/경도가 고유한 2016 총 보험 투자액을 구하라.
```mysql
select 
    ROUND(sum(tiv_2016), 2) as tiv_2016
from 
    Insurance a
where 
	  EXISTS (select * from Insurance where pid <> a.pid and a.tiv_2015 = tiv_2015)
	  AND NOT EXISTS (select * from Insurance where pid <> a.pid and (lat, lon) = (a.lat, a.lon))
```


### multiple in 사용 예시
사용자별 첫 로그인 시점의 기기아이디를 구해라.
```mysql
select player_id, device_id 
from activity 
where (player_id, event_date) in (select player_id, min(event_date) from activity group by player_id) 
```


### sum() over(partition by .. order by ..) 사용 예시
사용자 행별 누적 플레이 포인트 값
```mysql
select 
    player_id, 
    event_date, 
    sum(games_played) over (partition by player_id order by event_date) as games_played_so_far
from Activity
```

7일연속의 금액의 합 - sum() over( ... rows between .. ) 사용
```mysql
SELECT   visited_on, 
         SUM(SUM(amount)) OVER(ORDER BY visited_on ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)   AS'amount',
         ROUND(CAST(SUM(SUM(amount)) OVER(ORDER BY visited_on ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS FLOAT)/7.0 ,2) AS'average_amount' 
FROM     Customer 
GROUP BY visited_on
ORDER BY visited_on
OFFSET 6 ROWS 
```


### count() over(partition by ..) 사용 예시
회사별 보고서를 5명이상에게 받는 매니저 추출
```mysql
select name from employee
where id in
      (select managerId from Employee
       group by managerId
       having count(managerId)>=5)
```


### using vs on 차이
using 은 두테이블간 필드이름이 같은 경우에 사용
```mysql
select 
    P.product_name, S.year, S.price
from
    Product P inner join Sales S using(product_id)
```


### datediff 사용 예시
```mysql
with t as (
  SELECT
    visited_on,
    SUM(amount) AS day_sum
  FROM
    Customer
  GROUP BY
    visited_on
)
select
  t1.visited_on,
  SUM(t2.day_sum) AS amount,
  ROUND(AVG(t2.day_sum), 2) AS average_amount
from
  t as t1 join t as t2
where
  datediff(t1.visited_on, t2.visited_on) between 0 and 6
GROUP BY
  t1.visited_on
HAVING
  COUNT(t2.visited_on) = 7
```

## mysql-5.7 사용 예시
### group_concat 사용 예시
```mysql
create table if not exists Customers (customer_id int, customer_name varchar(255));
truncate table Customers;
insert into Customers (customer_id, customer_name) values (1, 'Daniel');
insert into Customers (customer_id, customer_name) values (2, 'Diana');
insert into Customers (customer_id, customer_name) values (3, 'Elizabeth');
insert into Customers (customer_id, customer_name) values (4, 'Jhon');

create table if not exists Orders (order_id int, customer_id int, product_name varchar(255));
truncate table Orders;
insert into Orders (order_id, customer_id, product_name) values (10, 1, 'A');
insert into Orders (order_id, customer_id, product_name) values (20, 1, 'B');
insert into Orders (order_id, customer_id, product_name) values (30, 1, 'D');
insert into Orders (order_id, customer_id, product_name) values (40, 1, 'C');
insert into Orders (order_id, customer_id, product_name) values (50, 2, 'A');
insert into Orders (order_id, customer_id, product_name) values (60, 3, 'A');
insert into Orders (order_id, customer_id, product_name) values (70, 3, 'B');
insert into Orders (order_id, customer_id, product_name) values (80, 3, 'D');
insert into Orders (order_id, customer_id, product_name) values (90, 4, 'C');

select
    X.customer_id, c.customer_name
from
(
  select
    customer_id,
    group_concat(distinct product_name order by product_name) as total
  from
    Orders
  group by
    customer_id
) X inner join Customers c using(customer_id)
where
    X.total like "A,B%" and X.total not like "%C%"

```

### union 사용 예시 (union all 은 중복 제거 안함 - 속도가 더빠름)
```mysql
create table Sessions (session_id int, duration int);
truncate table Sessions;
insert into Sessions (session_id, duration) values (1, 30);
insert into Sessions (session_id, duration) values (2, 199);
insert into Sessions (session_id, duration) values (3, 299);
insert into Sessions (session_id, duration) values (4, 580);
insert into Sessions (session_id, duration) values (5, 1000);

select '[0-5>' as bin, sum(if(duration < 300, 1, 0)) as total from Sessions
union
select '[5-10>' as bin, sum(if(duration >= 300 and duration <600, 1, 0)) as total from Sessions
union
select '[10-15>' as bin, sum(if(duration >= 600 and duration <900, 1, 0)) as total from Sessions
union
select '15 or more' as bin, sum(if(duration >= 900, 1, 0)) as total from Sessions
```


### if, case 문 사용 예시
```mysql
create table if not exists Variables(name varchar(3), `value` int);
truncate table Variables;
insert into Variables (name, `value`) values ('x', 66);
insert into Variables (name, `value`) values ('y', 77);

create table if not exists Expressions(left_operand varchar(3), operator varchar(3), right_operand varchar(3));
truncate table Expressions;
insert into Expressions(left_operand, operator, right_operand) values ('x', '>', 'y');
insert into Expressions(left_operand, operator, right_operand) values ('x', '<', 'y');
insert into Expressions(left_operand, operator, right_operand) values ('x', '=', 'y');
insert into Expressions(left_operand, operator, right_operand) values ('y', '>', 'x');
insert into Expressions(left_operand, operator, right_operand) values ('y', '<', 'x');
insert into Expressions(left_operand, operator, right_operand) values ('x', '=', 'y');

select
	e.left_operand, e.operator, e.right_operand,
	if( (case 
		when operator = '<' then x.value < y.value
    	when operator = '>' then x.value > y.value
		else x.value = y.value end) = 1, 'true', 'false') as `value`
from
    Expressions e
    left join `Variables` x on e.left_operand = x.name
    left join `Variables` y on e.right_operand = y.name
```


### enum 사용 예시
```mysql
Create table If Not Exists Sales (sale_date date, fruit ENUM('apples', 'oranges'), sold_num int);
truncate table Sales;
insert into Sales (sale_date, fruit, sold_num) values ('2020-05-01', 'apples', 10);
insert into Sales (sale_date, fruit, sold_num) values ('2020-05-01', 'oranges', 8);
insert into Sales (sale_date, fruit, sold_num) values ('2020-05-02', 'apples', 15);
insert into Sales (sale_date, fruit, sold_num) values ('2020-05-02', 'oranges', 15);
insert into Sales (sale_date, fruit, sold_num) values ('2020-05-03', 'apples', 20);
insert into Sales (sale_date, fruit, sold_num) values ('2020-05-03', 'oranges', 0);
insert into Sales (sale_date, fruit, sold_num) values ('2020-05-04', 'apples', 15);
insert into Sales (sale_date, fruit, sold_num) values ('2020-05-04', 'oranges', 16);

select
  sale_date,
  sum(case when fruit = 'apples' then sold_num else -1 * sold_num end) as diff
from
    Sales
group by
    sale_date
```



### rank 대체 예시
```mysql
create table if not exists Scores (id int, score float);
truncate table Scores;
insert into Scores (id, score) values (1, '3.50');
insert into Scores (id, score) values (2, '3.65');
insert into Scores (id, score) values (3, '4.00');
insert into Scores (id, score) values (4, '3.85');
insert into Scores (id, score) values (5, '4.00');
insert into Scores (id, score) values (6, '3.65');

-- 5.7 dense_rank
SELECT
    Score,
    (SELECT count(distinct Score) FROM Scores WHERE Score >= s.Score) Rank
FROM 
    Scores s
ORDER BY 
    Score desc
;
select
  score, rank
from
  (
    SELECT
        id, score,
        ( @real_rank := IF ( @last > score, @real_rank:=@real_rank+1, @real_rank ) ) AS rank,
        ( @last := score )
    FROM
        Scores AS a ,
        ( SELECT @last := 0, @real_rank := 1 ) AS b
    ORDER BY
        a.score DESC
  ) X
;

-- 5.7 rank
SELECT
  id, score,
  ( SELECT COUNT(*) + 1 FROM Scores WHERE score > b.score ) AS rank
FROM
  Scores AS b
ORDER BY
  rank ASC
;

SELECT
  id, score, @curRank := @curRank + 1 AS rank
FROM Scores p, (SELECT @curRank := 0) r
ORDER BY  score;
;
```


### 기타 문제 풀이
연속 5일 로그인 사용자 추출
```mysql
create table if not exists Accounts(id int, name varchar(255));
truncate table Accounts;
insert into Accounts (id, name) values (1, 'Winston');
insert into Accounts (id, name) values (7, 'Jonathan');

create table if not exists Logins(id int, login_date date);
truncate table Logins;
insert into Logins (id, login_date) values (7, '2020-05-30');
insert into Logins (id, login_date) values (1, '2020-05-30');
insert into Logins (id, login_date) values (7, '2020-05-31');
insert into Logins (id, login_date) values (7, '2020-06-01');
insert into Logins (id, login_date) values (7, '2020-06-02');
insert into Logins (id, login_date) values (7, '2020-06-02');
insert into Logins (id, login_date) values (7, '2020-06-03');
insert into Logins (id, login_date) values (1, '2020-06-07');
insert into Logins (id, login_date) values (7, '2020-06-10');


select
  DISTINCT l1.id,
  (SELECT name FROM Accounts WHERE id = l1.id) AS name
FROM
    Logins l1
    JOIN Logins l2 ON l1.id = l2.id AND DATEDIFF(l2.login_date, l1.login_date) BETWEEN 1 AND 4
GROUP BY
    l1.id, l1.login_date    
HAVING
    COUNT(DISTINCT l2.login_date) >= 4
```

조건에 맞는 보험금 합계
```mysql
create table Insuarance (pid int, tiv_2015 float, tiv_2016 float, lat float, lon float);
truncate table Insuarance;
insert into Insuarance (pid, tiv_2015, tiv_2016, lat, lon) values (1, 10, 5, 10, 10);
insert into Insuarance (pid, tiv_2015, tiv_2016, lat, lon) values (2, 20, 20, 20, 20);
insert into Insuarance (pid, tiv_2015, tiv_2016, lat, lon) values (3, 10, 30, 20, 20);
insert into Insuarance (pid, tiv_2015, tiv_2016, lat, lon) values (4, 10, 40, 40, 40);

-- 조건에 맞는 보험금 합계
select *
from Insuarance a
where 1 = (select count(*) from Insuarance b where a.LAT=b.LAT and a.LON=b.LON)
and 1 < (select count(*) from Insuarance c where a.TIV_2015=c.TIV_2015)
;
SELECT ROUND(SUM(TIV_2016),2) AS TIV_2016 FROM Insuarance a
WHERE EXISTS (SELECT * FROM Insuarance WHERE PID <> a.PID AND TIV_2015 = a.TIV_2015)
AND NOT EXISTS (SELECT * FROM Insuarance WHERE PID <> a.PID AND (LAT, LON) = (a.LAT, a.LON))
```

두번째 큰값 추출
```mysql
create table if not exists Employee(id int, salary int);
truncate table Employee;

insert into Employee (id, salary) values (1, 100);
insert into Employee (id, salary) values (2, 200);
insert into Employee (id, salary) values (3, 300);

select
    max(salary) as SecondHighestSalary
from
    Employee
where
    salary < (select max(salary) from Employee)
```

최소 3회 이상 연속적으로 나타나는 모든 숫자
```mysql
create table if not exists `Logs` (id int, num varchar(255));
truncate table `Logs`;
insert into `Logs` (id, num) values (1, 1);
insert into `Logs` (id, num) values (2, 1);
insert into `Logs` (id, num) values (3, 1);
insert into `Logs` (id, num) values (4, 2);
insert into `Logs` (id, num) values (5, 1);
insert into `Logs` (id, num) values (6, 2);
insert into `Logs` (id, num) values (7, 2);

select
    distinct a1.num as ConsecutiveNums
from
    `Logs` a1
    join `Logs` a2 on a1.id = a2.id+1 and a1.num = a2.num
    join `Logs` a3 on a2.id = a3.id+1 and a2.num = a3.num
```

각 부서별 최고 연봉 수령자 추출
```mysql
create table if not exists Employee (id int, name varchar(255), salary int, departmentId int);
truncate table Employee;
insert into Employee(id, name, salary, departmentId) values (1, 'Joe', 70000, 1);
insert into Employee(id, name, salary, departmentId) values (2, 'Jim', 90000, 1);
insert into Employee(id, name, salary, departmentId) values (3, 'Henry', 80000, 2);
insert into Employee(id, name, salary, departmentId) values (4, 'Sam', 60000, 2);
insert into Employee(id, name, salary, departmentId) values (5, 'Max', 90000, 1);

create table if not exists Department (id int, name varchar(255));
truncate table Department;
insert into Department (id, name) values (1, 'IT');
insert into Department (id, name) values (2, 'Sales');

select
    (select d.name from Department d where e.departmentId = d.id) as Department,
    e.name as Employee,
    e.salary as Salary
from
    Employee e
where
    (e.departmentId, e.salary) in (
        select
            departmentId, max(salary) as salary
        from
            Employee
        group by
            departmentId
)
```

사용자별 일별 누적 플레이 게임수 추출
```mysql
create table if not exists Activity (player_id int, device_id int, event_date date, games_played int);
truncate table Activity;
insert into Activity (player_id, device_id, event_date, games_played) values (1, 2, '2016-03-01', 5);
insert into Activity (player_id, device_id, event_date, games_played) values (1, 2, '2016-05-02', 6);
insert into Activity (player_id, device_id, event_date, games_played) values (1, 3, '2017-06-25', 1);
insert into Activity (player_id, device_id, event_date, games_played) values (3, 1, '2016-03-02', 0);
insert into Activity (player_id, device_id, event_date, games_played) values (3, 4, '2018-07-03', 5);

select
    a.player_id,
    a.event_date,
    (select sum(games_played)
      from Activity a1
      where a1.player_id = a.player_id and a1.event_date <= a.event_date
    ) as games_played_so_far
from
Activity a
```

처음 로그인 후 다음날 다시 로그인한 플레이어의 비율 추출
```mysql
create table if not exists Activity (player_id int, device_id int, event_date date, games_played int);
truncate table Activity;
insert into Activity (player_id, device_id, event_date, games_played) values (1, 2, '2016-03-01', 5);
insert into Activity (player_id, device_id, event_date, games_played) values (1, 2, '2016-03-02', 6);
insert into Activity (player_id, device_id, event_date, games_played) values (2, 3, '2017-06-25', 1);
insert into Activity (player_id, device_id, event_date, games_played) values (3, 1, '2016-03-02', 0);
insert into Activity (player_id, device_id, event_date, games_played) values (3, 4, '2018-07-03', 5);

select
    round((count(*) / (select count(distinct b.player_id) from Activity b)),2) as fraction
from
    Activity a
where
    (a.player_id, a.event_date) in (
      select player_id, date_add(min(event_date), interval 1 day) as event_date
      from Activity
      group by player_id
    )
```

최소 5개의 리포트를 받는 관리자 추출
```mysql
create table if not exists Employee (id int, name varchar(255), department varchar(255), managerId int);
truncate table Employee;
insert into Employee (id, name, department, managerId) values (101, 'John', 'A', null);
insert into Employee (id, name, department, managerId) values (102, 'Dan', 'A', 101);
insert into Employee (id, name, department, managerId) values (103, 'James', 'A', 101);
insert into Employee (id, name, department, managerId) values (104, 'Amy', 'A', 101);
insert into Employee (id, name, department, managerId) values (105, 'Anne', 'A', 101);
insert into Employee (id, name, department, managerId) values (106, 'Ron', 'B', 101);

select e.name from Employee e where e.id in (  
    select managerId from Employee group by managerId having count(managerId) >= 5
);
```

당선자 추출
```mysql
create table if not exists Candidate (id int, name varchar(255));
truncate table Candidate;
insert into Candidate (id, name) values (1, 'A');
insert into Candidate (id, name) values (2, 'B');
insert into Candidate (id, name) values (3, 'C');
insert into Candidate (id, name) values (4, 'D');
insert into Candidate (id, name) values (5, 'E');

create table if not exists Vote (id int, candidateId int);
truncate table Vote;
insert into Vote (id, candidateId) values (1, 2);
insert into Vote (id, candidateId) values (2, 4);
insert into Vote (id, candidateId) values (3, 3);
insert into Vote (id, candidateId) values (4, 2);
insert into Vote (id, candidateId) values (5, 5);

select a.name
from Candidate a
where a.id = (
        select sub.candidateId from (
            select candidateId, count(*) as cnt from Vote group by candidateId order by cnt desc limit 0, 1
        ) sub
)
```

질문에 대한 답변률이 가장 높은 질문을 보고해라
```mysql
create table if not exists SurveyLog(id int, `action` Enum("show", "answer", "skip"), question_id int, answer_id int, q_num int, `timestamp` int);
truncate table SurveyLog;
insert into SurveyLog (id, `action`, question_id, answer_id, q_num, `timestamp`) values (5, 'show', 285, null, 1, 123);
insert into SurveyLog (id, `action`, question_id, answer_id, q_num, `timestamp`) values (5, 'answer', 285, 124124, 1, 124);
insert into SurveyLog (id, `action`, question_id, answer_id, q_num, `timestamp`) values (5, 'show', 369, null, 2, 125);
insert into SurveyLog (id, `action`, question_id, answer_id, q_num, `timestamp`) values (5, 'skip', 369, null, 2, 126);

select
    X.question_id as survey_log
from
(
  select
    question_id,
    round((sum(if(`action` = 'answer', 1, 0)) / count(distinct question_id)), 2) as rate
  from
    SurveyLog
  group by
    question_id
  order by
    rate desc, question_id
  limit
    0, 1
) X
```



## 참고
1. [sql join](https://t1.daumcdn.net/cfile/tistory/99219C345BE91A7E32)
