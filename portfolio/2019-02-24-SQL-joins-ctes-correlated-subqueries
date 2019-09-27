
~~~sql
/* 
Question: which students finish their day in a building made of limestone?
*/
use db

/* 
See ClassSchedulesTables.sql for these tables
*/

select * from db.dbo.buildings;
select * from db.dbo.building_material;
select * from db.dbo.classes;
select * from db.dbo.students;
select * from db.dbo.schedules;

/* 
Which buildings are not brick?  
Beware of null values 
*/

select * from dbo.buildings where building_material_id <> 1 -- we don't know whether Lindley hall is brick or not, so it's omitted.
select * from dbo.buildings where building_material_id <> 1 or building_material_id is null; -- this may be preferable behavior
select * from dbo.buildings where isnull(building_material_id,0)<>1; -- similar behavior 
select * from dbo.buildings where coalesce(building_material_id,0)<>1 -- equivalent, though coalesce() can take >2 arguments, returning the first that is non-null

/* 
View building material for each building.
(And, though it's better to be explicit, I'll omit the database 'db' and schema 'dbo'.)
*/

select b.name [building name], isnull(m.name,'Unknown') [building material]
from buildings b left join building_material m on b.building_material_id=m.id; -- an inner join would leave out Lindley


/* Which students attend each class at each time? */
select s.start_time,c.name [classname], string_agg(st.name,',') [students] 
from schedules s join students st on s.student_id=st.id left join classes c on s.class_id=c.id 
group by c.name,s.start_time
order by s.start_time,c.name;


/* Our burning question: which students end the day in a building made of limestone? */

/* One strategy: creating a grand reference temp table */
drop table if exists dbo.#schedule_reference;
--
select st.name [student],s.start_time,c.name as class_name,b.name as building,m.name as building_material_name -- since select into is used, no need to create the table in a separate step
into #schedule_reference
from schedules s
join students st on s.student_id = st.id 
left join classes c on s.class_id = c.id
left join buildings b on c.building_id = b.id -- because one of the class ID's is null, anything joined with a class field needs to be a left join
left join building_material m on b.building_material_id = m.id -- and remember that one of the buildings has null building_material_id, or you'll lose data
order by st.name;
--
select * from #schedule_reference;


/* Then filter down to the results of interest, using a correlated subquery to get the last class of the day per student */
select string_agg(student,',') as students_ending_day_in_a_limestone_building 
from #schedule_reference sr 
where building_material_name='limestone'
and start_time=(select max(start_time) from #schedule_reference sr1 where sr.student=sr1.student group by student) 
group by building_material_name,start_time;
-- correctly returns just ida (we don't know whether Lindley is limestone or not)


/*
Equivalently, using a CTE, which may be more performant for large data sets and which allows easier aliasing
*/


;with schedule_reference AS (
select s.start_time,c.name [class_name],st.name [student],b.name [building],m.name [building_material_name]
from schedules s
join students st on s.student_id = st.id 
left join classes c on s.class_id = c.id
left join buildings b on c.building_id = b.id
left join building_material m on b.building_material_id = m.id 
)
select string_agg(student,',') as students_ending_day_in_a_limestone_building 
from schedule_reference sr 
where building_material_name='limestone' -- note the column name now needs to match the one in the cte
and start_time=(select max(start_time) from schedule_reference sr1 where sr.student=sr1.student group by student) 
group by building_material_name,start_time; 
-- correctly returns just ida

~~~
