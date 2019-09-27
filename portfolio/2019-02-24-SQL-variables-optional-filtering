~~~sql




/* 
See ClassSchedulesTables.sql for these tables
*/

use db

select * from db.dbo.buildings;
select * from db.dbo.building_material;
select * from db.dbo.classes;
select * from db.dbo.students;
select * from db.dbo.schedules;

/*
A reference table
*/

select * from db.dbo.schedules s 
full outer join db.dbo.students st on s.student_id=st.id
full outer join db.dbo.classes c on s.class_id=c.id
full outer join db.dbo.buildings b on c.building_id=b.id
full outer join db.dbo.building_material m on b.building_material_id=m.id



/*
Say you want a query that can be customized for different uses.
For example, you would like to have a query that can tell you which students have at least one class
in a limestone building or a brick building (depending on input), and that can also 
filter down to a subset of students if needed (but not always).
*/


/*
First, create a query that can filter on building material:
Which students have at least one class in a limestone building?
(Compare the results to the reference table above)
*/

declare @building_material varchar(50)='limestone'

select st.name [student_name], c.name [class_name], b.name [building_name], m.name [material_name] from schedules s 
join students st on s.student_id=st.id -- a left join is not necessary as we know the student_id field in dbo.schedules is non-null and always has matches in dbo.students
left join classes c on s.class_id=c.id --
left join buildings b on c.building_id=b.id
left join building_material m on b.building_material_id=m.id
where m.name=@building_material
go -- this batch separator allows us to use the same name for the variable @building_material in the next query without mssql throwing an error.

/*
Which students have at least one class in a brick building?
*/


declare @building_material varchar(50)='brick'

select st.name [student_name], c.name [class_name], b.name [building_name], m.name [material_name] from schedules s 
join students st on s.student_id=st.id -- a left join is not necessary as we know the student_id field in dbo.schedules is non-null and always has matches in dbo.students
left join classes c on s.class_id=c.id --
left join buildings b on c.building_id=b.id
left join building_material m on b.building_material_id=m.id
where m.name=@building_material
go -- this batch separator allows us to use the same name for the variable @building_material in the next query without mssql throwing an error.


/*
Wildcard searching: say you just want to know which students take at least one class in a stone building
*/

declare @building_material varchar(50)='stone'

select st.name [student_name], c.name [class_name], b.name [building_name], m.name [material_name],* from schedules s 
join students st on s.student_id=st.id -- a left join is not necessary as we know the student_id field in dbo.schedules is non-null and always has matches in dbo.students
left join classes c on s.class_id=c.id --
left join buildings b on c.building_id=b.id
left join building_material m on b.building_material_id=m.id
where m.name like '%'+@building_material+'%'
go

/*
Or to get just the list of students, use select distinct in the query above.
*/
declare @building_material varchar(50)='stone'

select distinct st.name [student_name]--, c.name [class_name], b.name [building_name], m.name [material_name],* 
from schedules s 
join students st on s.student_id=st.id -- a left join is not necessary as we know the student_id field in dbo.schedules is non-null and always has matches in dbo.students
left join classes c on s.class_id=c.id --
left join buildings b on c.building_id=b.id
left join building_material m on b.building_material_id=m.id
where m.name like '%'+@building_material+'%'
go

/*
Finally, optional filtering of the above query to a subset of students:
The expected behavior is that we can input a list of students and see results just for that list, or leave blank and see results for all students.
Compare the results to the reference table above
*/

declare @building_material varchar(50)='stone'
declare @students varchar(max) = 'herman,sarah,andrew'
--
drop table if exists #students_filter; create table #students_filter (students varchar(max));
if ltrim(rtrim(@students))<>'' insert into #students_filter select value from string_split(@students,',')
if ltrim(rtrim(@students))='' insert into #students_filter select distinct name from dbo.students
--
select distinct st.name [student_name]--, c.name [class_name], b.name [building_name], m.name [material_name],* 
from schedules s 
join students st on s.student_id=st.id -- a left join is not necessary as we know the student_id field in dbo.schedules is non-null and always has matches in dbo.students
left join classes c on s.class_id=c.id --
left join buildings b on c.building_id=b.id
left join building_material m on b.building_material_id=m.id
where m.name like '%'+@building_material+'%'
and st.name in (select students from #students_filter)

~~~
