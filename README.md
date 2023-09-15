# TouristDbms

EMAIL
+-----------+----------------------+
| TouristId | Email                |
+-----------+----------------------+
| TR001     | anki@gmail.com       |
| TR002     | sharath@gmail.com    |
| TR003     | jattigahan@gmail.com |
| TR004     | chidu@gmail.com      |
| TR005     | kuladeep@gmail.com   |
| TR006     | ganesha@gmail.com    |
| TR007     | shodu@gmail.com      |
+-----------+----------------------+

--------------------------------------------------------------------------------------------------------------------------------
PLACE
+---------+-----------+-----------+------+-------------------------------+
| PlaceId | PlaceName | State     | Km   | History                       |
+---------+-----------+-----------+------+-------------------------------+
| PI01    | Coorg     | Karnataka |  200 | Coorg is beautifull place     |
| PI02    | Noida     | Delhi     |  500 | Delhi is Marvelous place      |
| PI03    | Chennai   | TamilNadu |  250 | Chennai is a beautifull place |
| PI04    | Trishur   | Kerala    |  270 | Trishur Pooram is fantastic   |
| PI05    | Kudla     | Karnataka |   50 | Kudla is Jigga                |
| PI06    | Yallapura | Karnataka |  300 | Yallapura is cold             |
| PI07    | Goa       | Goa       |  260 | Goa is party place            |
+---------+-----------+-----------+------+-------------------------------+


-----------------------------------------------------------------------------------------------------------------------------
TOURIST
+-----------+-------------+------+-----------+
| TouristId | TouristName | Age  | Country   |
+-----------+-------------+------+-----------+
| TR001     | Ankith      |   40 | Bharat    |
| TR002     | Sharath     |   42 | Bharat    |
| TR003     | Gahan       |   35 | Germany   |
| TR004     | Chida       |   40 | Nepala    |
| TR005     | Kuldeep     |   41 | Australia |
| TR006     | Ganesha     |   40 | Indonasia |
| TR007     | Shodhu      |   40 | Pakistan  |
+-----------+-------------+------+-----------+
INSERT into TOURIST values('',);
--------------------------------------------------------------------------------------------------------------------------------
VISIT
+-----------+---------+------------+
| TouristId | PlaceId | VisitDate  |
+-----------+---------+------------+
| TR001     | PI01    | 2023-06-23 |
| TR002     | PI02    | 2023-06-23 |
| TR003     | PI03    | 2023-06-23 |
| TR004     | PI04    | 2023-06-23 |
| TR005     | PI05    | 2023-06-23 |
| TR006     | PI06    | 2023-06-23 |
| TR007     | PI07    | 2023-06-23 |
| TR001     | PI02    | 2023-05-23 |
| TR001     | PI04    | 2023-05-23 |
| TR001     | PI07    | 2023-02-25 |
| TR001     | PI02    | 2023-02-09 |
| TR001     | PI04    | 2023-10-09 |
| TR001     | PI06    | 2023-02-09 |
+-----------+---------+------------+

INSERT into VISIT values('TR001','PI04','2023-05-23');
INSERT into VISIT values('TR003','PI01','2023-05-13');

INSERT into VISIT values('TR002','PI01','2023-05-13');

---------------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------------

i. List the state name which is having maximum number of tourist places.

SELECT STATE ,COUNT(*) AS NumOfPlace FROM PLACE GROUP BY STATE ORDER BY NumOfPlace  desc limit 1; 
+-----------+------------+
| STATE     | NumOfPlace |
+-----------+------------+
| Karnataka |          3 |
+-----------+------------+
----------------------------------------------------------------------------------------------------------------------------------

ii. List details of Tourist place where maximum number of tourists visited.

SELECT P.PlaceName ,P.State,COUNT(*) AS Total_Visited FROM PLACE P JOIN VISIT V ON P.PlaceId =V.PlaceId  GROUP BY P.PlaceName,P.State ORDER BY Total_Visited desc limit 1;
+-----------+-----------+---------------+
| PlaceName | State     | Total_Visited |
+-----------+-----------+---------------+
| Coorg     | Karnataka |             1 |
+-----------+-----------+---------------+
------------------------------------------------------------------------------------------------------------------------------

iii. List the details of tourists visited all tourist places of the state “KARNATAKA”.


SELECT * from TOURIST where TouristId in(SELECT v.TouristId from VISIT v,PLACE p WHERE v.PlaceId=p.PlaceId AND p.State="Karnataka" GROUP BY v.TouristId HAVING count(distinct p.PlaceId)=(select count(*) from PLACE WHERE State="karnataka"));
+-----------+-------------+------+---------+
| TouristId | TouristName | Age  | Country |
+-----------+-------------+------+---------+
| TR003     | Gahan       |   35 | Germany |
+-----------+-------------+------+---------+

---------------------------------------------------------------------------------------------------------------------------------
iv. Display the details of the tourists visited at least one tourist place of the state, but visited all states tourist places.

SELECT v.TouristId,t.TouristName,t.Age,t.Country,count(distinct PlaceId) as Total FROM VISIT v,TOURIST t WHERE v.TouristId=t.TouristId group by TouristId having count(distinct PlaceId)>=(select count(distinct State) As TotalState from PLACE);
+-----------+-------------+------+---------+-------+
| TouristId | TouristName | Age  | Country | Total |
+-----------+-------------+------+---------+-------+
| TR001     | Ankith      |   40 | Bharat  |     5 |
+-----------+-------------+------+---------+-------+
------------------------------------------------------------------------------------------------------------------------
v. Display the details of the tourist place visited by the tourists of all country.

SELECT p.PlaceId,p.PlaceName,p.State,p.Km,p.History FROM PLACE AS p,VISIT AS v,TOURIST as t 
WHERE p.PlaceId=v.PlaceId and v.TouristId=t.TouristId group by p.PlaceId HAVING count(distinct t.country)=(SELECT count(distinct country) from TOURIST);

+---------+-----------+-----------+------+---------------------------+
| PlaceId | PlaceName | State     | Km   | History                   |
+---------+-----------+-----------+------+---------------------------+
| PI01    | Coorg     | Karnataka |  200 | Coorg is beautifull place |
+---------+-----------+-----------+------+---------------------------+
-------------------------------------------------------------------------------------------------------------------------------


