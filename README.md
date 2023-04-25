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
