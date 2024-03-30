### Лабораторная работа №3

## Структура базы 

Структура базы данных «Университет»:
-	Students(StudentId, StudentName, GroupId)
-	Groups(GroupId, GroupName)
-	Courses(CourseId, CourseName)
-	Lecturers(LecturerId, LecturerName)
-	Plan(GroupId, CourseId, LecturerId)
-	Marks(StudentId, CourseId, Mark)


## Создание таблиц 


``` sql
create table Students (
    StudentId serial primary key,
    StudentName varchar(50),
    GroupId int,
    foreign key (GroupId) references Groups(GroupId)
);

create table Groups (
    GroupId SERIAL primary key,
    GroupName varchar(50)
);

create table Courses (
    CourseId SERIAL primary key,
    CourseName varchar(50)
);

create table Lecturers (
    LecturerId SERIAL primary key,
    LecturerName varchar(50)
);

create table Plan (
    GroupId int,
    CourseId int,
    LecturerId int,
    primary key (GroupId, CourseId),
    foreign key (GroupId) references Groups(GroupId),
    foreign key (CourseId) references Courses(CourseId),
    foreign key (LecturerId) references Lecturers(LecturerId)
);

create table Marks (
    StudentId int,
    CourseId int,
    Mark int,
    primary key (StudentId, CourseId),
    foreign key (StudentId) references Students(StudentId),
    foreign key (CourseId) references Courses(CourseId)
);
```

## Вариант 2

## 1.2

```
π(studentid, studentname, groupid)(students ⨝_{studentid=groupid} groups)

```
``` sql
select s.studentid , s.studentname , s.groupid
from students s join groups g on s.studentid = g.groupid
```
## 2.1

```
π(studentid, studentname, groupid, groupname)(students ⨝_{studentid=groupid} groups)
```

``` sql
select s.studentid , s.studentname ,s.groupid, g.groupname  
from students s join groups g on s.studentid = g.groupid 
```

## 3.2

```
π(studentid, studentname, groupid)((Students ⨝_{studentid=studentid} Marks) ⨝_{courseid=courseid} Courses ⨝_{coursename='Mathematics' ∧ mark=90} π(courseid)(Courses))

```

```sql
select s.studentid, s.studentname, s.groupid
from Students s join Marks m on s.studentid = m.studentid
join courses c on m.CourseId = c.CourseId
where m.mark = 90 and c.coursename = 'Mathematics';
``` 

## 4.1

```
π(studentid, studentname, groupid)(students) - π(studentid)(σ(coursename='Physics' ∧ mark IS NULL)(Marks ⨝_{courseid=courseid} Courses))
```


``` sql
select s.studentid, s.studentname, s.groupid  
from students as s 
where s.studentid not in ( 
	 select m.studentid  
	 from marks m join courses c on m.courseid = c.courseid  
	 where c.coursename = 'Physics' and m.mark is null
);
```

## 5.2

```
π(studentid, studentname, groupid)(σ(mark IS NULL)(Students))
```

``` sql 
select s.studentid, s.studentname, s.groupid  
from students as s 
where s.studentid in ( 
	 select m.studentid  
	 from marks m join courses c on m.courseid = c.courseid  
	 where m.mark is null
);
```

## 6.2 

```
π(studentid, studentname, mark, courseid, lecturerid, lecturername)(students ⨝_{studentid=studentid} (π(studentid, mark, courseid)(Marks) ⨝_{courseid=courseid} π(courseid, lecturerid, lecturername)(Courses ⨝_{courseid=courseid} Lecturers)) ⨝_{mark IS NULL}(marks))
```

```sql 
select distinct s.StudentId, s.studentname , m.mark , c.courseid , l.lecturerid ,l.lecturername 
from students s
left join Marks m on s.StudentId = m.StudentId
left join Courses c on m.CourseId = c.CourseId
left join Lecturers l on c.LecturerId = l.LecturerId
where m.Mark is null;
```

## 7.2 
```
π(studentname, mark, coursename, groupname)((students ⨝_{studentid=groupid} groups) ⨝_{studentid=studentid} marks ⨝_{courseid=courseid} courses)
```

```sql
select s.studentname, m.mark , c.coursename , g.groupname 
from students s 
left join groups g on s.studentid = g.groupid 
inner join marks m on s.studentid = m.studentid 
inner join courses c on m.courseid = c.courseid 
where m.mark is not null
```

## 8.2 

```
π(studentname, SUM(mark), studentid)(students ⨝_{studentid=studentid} marks)
```
```sql
select s.studentname, sum(m.mark), s.studentid from students s 
left join marks m on s.studentid = m.studentid 
group by s.studentid 
```

## 9.4

```
π(groupname, SUM(mark))(groups ⨝_{groupid=groupid} students ⨝_{studentid=studentid} marks)
```
```sql
select g.groupname , sum(m.mark)
from groups g 
join students s ON g.groupid = s.groupid 
inner join marks m on s.studentid = m.studentid 
group by g.groupname 
```

## 10

```sql

select s.studentid,
count(distinct m.courseid) as Total,
count(distinct case when m.mark is not null then m.courseid end) as passed,
count(distinct case when m.mark is null then m.courseid end) as Failed
from students s 
left join marks m on s.studentid = m.studentid 
group by s.studentid 

```

```
π StudentId, Total, Passed, Failed (
    Students ⨝ σ Mark IS NOT NULL (π StudentId, COUNT(CourseId) AS Passed (Marks)) ⨝ 
    σ Mark IS NULL (π StudentId, COUNT(CourseId) AS Failed (Marks)) ⨝
    π StudentId, COUNT(CourseId) AS Total (Marks)
)
```
