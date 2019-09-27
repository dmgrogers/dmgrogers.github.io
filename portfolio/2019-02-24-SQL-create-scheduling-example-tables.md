
~~~sql
/* 
Creating a relational database: some buildings at a university,
some classes, and some student class schedules.
Question: which students finish their day in a building made of limestone?
*/

select current_user;
-- create database db;
use db;

if object_id('db.dbo.buildings') is not null drop table db.dbo.buildings;
create table db.dbo.buildings (id int identity (1,1), name varchar(100), building_material_id int);
insert db.dbo.buildings values ('Wylie',1),('Maxwell',2),('Lindley',NULL);
select * from db.dbo.buildings

if object_id('db.dbo.building_material') is not null drop table db.dbo.building_material;
create table db.dbo.building_material (id int identity (1,1),name varchar(100));
insert db.dbo.building_material values ('brick'),('limestone');

/* 
View building material for each building.
(And, though it's better to be explicit, I'll omit the database 'db' and schema 'dbo'.)
*/

select b.name [building name], isnull(m.name,'Unknown') [building material]
from buildings b left join building_material m on b.building_material_id=m.id; -- an inner join would leave out Lindley

/* 
Adding two more tables: 
*/

if object_id('students') is not null drop table students;
create table students (id int identity (1,1),name varchar(100));
insert dbo.students values ('herman'),('andrew'),('ida'),('sarah')


if object_id('dbo.classes') is not null drop table dbo.classes
create table dbo.classes(id int identity(1,1),name varchar(50),building_id int);
insert classes values 
('exoplanetology',1)
,('history of the elephant',2)
,('applied musicology',3)
,('advanced pastry baking',2);
select * from classes;

/* Finally, one more table relating student id's, class start times, and class id's: */ 
drop table if exists schedules; -- this syntax actually works in both mssql and mysql
create table schedules(student_id int,start_time time,class_id int);
insert dbo.schedules values 
(1,'09:00',4),
(1,'12:00',3),
(2,'09:00',1),
(2,'12:00',3),
(3,'09:00',NULL),
(3,'12:00',2), -- notice the null value
(4,'09:00',2),
(4,'12:00',3);
select * from schedules;
~~~
