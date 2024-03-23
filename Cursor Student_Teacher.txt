Subject–Teacher database

create table Teacher(t_no int primary key, t_name varchar(20), age int, yr_experience int); 
insert into teacher values(1,'Joshi',26,5);
insert into teacher values(2,'Kale',30,20);
insert into teacher values(3,'Shinde',35,15);
insert into teacher values(4,'Auti',22,3);

create table Subject(s_no int primary key, s_name varchar(15));

insert into subject values(101,'Math');
insert into subject values(102,'RDBMS');
insert into subject values(103,'JAVA');
insert into subject values(104,'PYTHON');

create table TS (t_no int references teacher,s_no int references subject);

insert into ts values(1,104);
insert into ts values(1,103);
insert into ts values(2,103);
insert into ts values(2,102);
insert into ts values(3,102);
insert into ts values(3,101);
insert into ts values(4,101);

a. Write a stored function using cursor which will accept the subject name and print the names of all teachers teaching that subject.

create or replace function f101(sname teacher.t_name%type)returns void as $$

declare
 t1 teacher%rowtype;

 c1 cursor for select t.* from teacher as t,subject as s,ts where  t.t_no=ts.t_no and s.s_no=ts.s_no and s_name=sname;

begin
 open c1;
  loop
  fetch c1 into t1;
   exit when not found;
    raise notice'Teacher name=%',t1.t_name;        
  end loop;
 close c1;
 end;
$$
 language plpgsql;
select f101('Math');
  

