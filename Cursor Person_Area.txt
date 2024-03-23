Person-Area Database

create table area(aid int primary key, aname varchar(10), area_type varchar(10));

create table person( pno int primary key, pname varchar(10), birthdate date, income int, aid int references area);

insert into area values(101,'pune','urban');
insert into area values(102,'khanapur', 'rural'); 
insert into area values(103,'belapur', 'rural');
insert into area values(104, 'mumbai','urban');

insert into person values(1, 'om','2000-10-15',30000,101);
insert into person values(2, 'sai', '2001-8-11', 40000,102);
insert into person values(3,'ram','2000-3-13',50000,102);
insert into person values(4, 'sham', '2001-11-15',34000,103);
insert into person values(5,'aman','2002-12-25',44000,103);
insert into person values(6,'aba','2002-10-11',35000,104);
insert into person values(7,'dada','2002-10-11',35000,104);

a. Write a cursor to accept a month as an input parameter from the user and display the names of persons whose birthday falls in that particular month.
create or replace function disp_person(m int)returns VOID
 as
 $$
 DECLARE
 t_pname person.pname%type;
 c1 cursor for select pname from person where extract (month from birthdate)=m;
BEGIN
 open c1;
 loop
 fetch c1 into t_pname;
 exit when not found;
 raise notice 'Person Name=%',t_pname;
end loop;
close c1;
END;
$$
 language plpgsql;
 select disp_person(10);


plpgsql=#  select disp_person(10);
NOTICE:  Person Name=aba
NOTICE:  Person Name=dada

b. Write a cursor to display the names of persons living in urban area.

do $$
declare
 p_name person.pname%type;
 c1 cursor for select pname from person, area where area.aid=person.aid and area_type='urban';
 BEGIN
 open c1;
 loop
  fetch c1 into p_name;
  exit when not found;
   raise notice 'Person Name=%',p_name;
 end loop;
 close c1;
 END;
 $$
language plpgsql;

c. Write a cursor to print names of all persons having income between 50000 and 100000.

DO $$
 DECLARE
t_pname person.pname%type;
 c1 cursor for select pname from person where income between 35000   and 45000;
 BEGIN
  open c1;
  loop
  fetch c1 into t_pname;
  exit when not found;
   raise notice 'Person Name=%',t_pname;
 end loop;
close c1;
 END;
 $$
 language plpgsql;

NOTICE:  Person Name=sai
NOTICE:  Person Name=aman
NOTICE:  Person Name=aba
NOTICE:  Person Name=dada