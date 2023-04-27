# SQL Questions:-

Q.1 <img width="639" alt="image" src="https://user-images.githubusercontent.com/60449352/233910871-ab60e13e-67db-4956-ad03-b25d8b653981.png">

SOLUTION:-
create table icc_world_cup
(
Team_1 Varchar(20),
Team_2 Varchar(20),
Winner Varchar(20)
);
INSERT INTO icc_world_cup values('India','SL','India');
INSERT INTO icc_world_cup values('SL','Aus','Aus');
INSERT INTO icc_world_cup values('SA','Eng','Eng');
INSERT INTO icc_world_cup values('Eng','NZ','NZ');
INSERT INTO icc_world_cup values('Aus','India','India');

select * from icc_world_cup;

SELECT TEAM_NAME,COUNT(1) AS Match_Played,SUM(WIN_FLAG) AS No_OF_Wins,COUNT(1)-SUM(WIN_FLAG)
AS No_Of_Loses FROM
(
SELECT Team_1 AS TEAM_NAME,CASE WHEN Team_1=Winner THEN 1 ELSE 0 END AS WIN_FLAG FROM icc_world_cup 
UNION ALL
SELECT Team_1 AS TEAM_NAME,CASE WHEN Team_2=Winner THEN 1 ELSE 0 END AS WIN_FLAG FROM icc_world_cup
) A
GROUP BY TEAM_NAME
ORDER BY Match_Played DESC;

Q.2 [COMBINE TWO TABLES LC QUESTION](https://leetcode.com/problems/combine-two-tables/)

SOLUTION:- SELECT P.firstName,P.lastName,A.city,A.state
FROM PERSON P LEFT JOIN ADDRESS A ON P.personId=A.personId

Q.3 [Employees-earning-more-than-their-managers](https://leetcode.com/problems/employees-earning-more-than-their-managers/)

Solution:-select E1.name as Employee from Employee as E1 JOIN Employee as E2 where E1.ManagerId = E2.Id and E1.salary > E2.salary

Q.4 [Duplicate Emails](https://leetcode.com/problems/duplicate-emails/)

Solution:- SELECT Email FROM PERSON GROUP BY EMAIL HAVING COUNT(*)>1;

Q.5 [Customers-who-never-order](https://leetcode.com/problems/customers-who-never-order/)

Solution:- SELECT C.NAME AS Customers FROM Customers C LEFT JOIN Orders O
ON C.ID=O.customerId WHERE o.customerId is NULL;
ALTERNATE SOLUTION:- SELECT NAME AS CUSTOMERS FROM Customers WHERE ID NOT IN (SELECT customerId FROM ORDERS);

Q.6 [Delete-duplicate-emails](https://leetcode.com/problems/delete-duplicate-emails/)

Solution:- delete a from person a inner join person b on a.email = b.email where a.id > b.id

Q.7 [Rising-temperature](https://leetcode.com/problems/rising-temperature/)

Solution:- SELECT a.id as id 
FROM Weather a
JOIN Weather b ON DATEDIFF(a.RecordDate,b.Recorddate)=1
WHERE a.Temperature > b.Temperature;

Q.8 Find new and Repeat customers?

Solution:- create table customer_orders (
order_id integer,
customer_id integer,
order_date date,
order_amount integer
);

insert into customer_orders values(1,100,cast('2022-01-01' as date),2000),(2,200,cast('2022-01-01' as date),2500),(3,300,cast('2022-01-01' as date),2100)
,(4,100,cast('2022-01-02' as date),2000),(5,400,cast('2022-01-02' as date),2200),(6,500,cast('2022-01-02' as date),2700)
,(7,100,cast('2022-01-03' as date),3000),(8,400,cast('2022-01-03' as date),1000),(9,600,cast('2022-01-03' as date),3000)
;

select * from customer_orders;

with new as (
SELECT customer_id,min(order_date) as new_order_date from customer_orders 
group by customer_id)
Select c.order_date,sum(c.order_amount),
sum(case when c.order_date=n.new_order_date then 1 else 0 end) as new_customers,
sum(case when c.order_date!=n.new_order_date then 1 else 0 end) as repeat_customers
from customer_orders c join new n on c.customer_id=n.customer_id
group by c.order_date;

Q.9 [Game-play-analysis](https://leetcode.com/problems/game-play-analysis-i/)

Solution:- SELECT player_id,min(event_date) as first_login from Activity group by player_id

Q.10 [Employee-bonus](https://leetcode.com/problems/employee-bonus/)

Solution:-SELECT E.name,B.bonus FROM Employee E LEFT JOIN Bonus B
ON E.empId=B.empId
WHERE B.bonus<1000 OR B.bonus is null;

Q.11 Number of Floor Visit

Solution:-
create table entries ( 
name varchar(20),
address varchar(20),
email varchar(20),
floor int,
resources varchar(10));

insert into entries 
values ('A','Bangalore','A@gmail.com',1,'CPU'),('A','Bangalore','A1@gmail.com',1,'CPU'),('A','Bangalore','A2@gmail.com',2,'DESKTOP')
,('B','Bangalore','B@gmail.com',2,'DESKTOP'),('B','Bangalore','B1@gmail.com',2,'DESKTOP'),('B','Bangalore','B2@gmail.com',1,'MONITOR')

SELECT * FROM entries;



with ct1 as(select name, max(Total_visits) as Total_visit,
STRING_AGG(resources,',') as resources_used from 
(select distinct name, resources, count(*) over(partition by name) Total_visits from entries) d
group by name)
,ct2 as (
select name, floor, row_number() over(partition by name order by c desc) as rn from
(select name, floor, count(floor) over(partition by name,resources) as c from entries) a
)
select c1.name,c1.Total_visit, c2.floor as m_v_f, c1.resources_used
from ct1 c1 join ct2 c2 on c1.name=c2.name where c2.rn=1

Q.12 [Find-customer-referee](https://leetcode.com/problems/find-customer-referee/)

Solution:-SELECT name FROM Customer
WHERE COALESCE(referee_id,'')!=2;

Q.13 [Customer-placing-the-largest-number-of-orders](https://leetcode.com/problems/customer-placing-the-largest-number-of-orders/)

Solution:-SELECT customer_number FROM 
(SELECT COUNT(order_number),customer_number FROM Orders GROUP BY customer_number
ORDER BY COUNT(order_number) DESC)
Orders LIMIT 1;

ALITER:-SELECT customer_number 
FROM Orders
GROUP BY customer_number
ORDER BY COUNT(order_number) DESC
LIMIT 1;

Q.14 [Big-countries](https://leetcode.com/problems/big-countries/description/)

Solution:-SELECT name, population, area from world
where area>=3000000 or population>=25000000
