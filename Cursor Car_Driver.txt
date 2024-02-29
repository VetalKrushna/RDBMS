create table CAR(c_no int primary key,owner varchar(20),
model text,colour text);

insert into CAR values(101,'OM','Ertiga','White');
insert into CAR values(102,'Sai','SUV700','White');
insert into CAR values(103,'Ram','Ertiga','RED');

create table driver(driver_no int primary key,
driver_name text,lic_no int,address text,age int,sal int);

insert into driver values(1,'aman',1111,'pune',30,9000);
insert into driver values(2,'sham',2222,'mumbai',40,12000);
insert into driver values(3,'aba',3333,'shirdi',35,15000);

create table CD(c_no int references CAR,driver_no int references driver);

insert into CD values(101,1);
insert into cD values(102,2);
insert into cD values(103,3);
insert into cD values(103,1);

a. Write a stored function with cursor which accepts the color and prints the names of all 
owners who own a car of that color.
=
 create or replace function disp_ow(car_color car.colour%type)returns void as
 $$ 
 declare
 O_name car.owner%type; 
 c1 cursor for select owner from car where colour=car_color;
 begin
 open c1;
 loop
 fetch c1 into O_name;
  exit when not found; 
 raise notice'Owner name=%',O_name;
end loop;  
close c1;
end;
$$
language plpgsql;
 select disp_ow('black');

b. Write a cursor which accepts the driver name and prints the details of all cars that 
this driver has driven, if the driver name is invalid,print an appropriate message.

create or replace function disp_car(dri_name driver.driver_name%type)returns void as $$
declare
  r1 car%rowtype;
  c1 cursor for select car.* from car,driver,cd where car.c_no=cd.c_no and
   driver.driver_no=cd.driver_no and driver_name=dri_name;
begin
  open c1;
   loop
    fetch c1 into r1;
    exit when not found;
     raise notice'Car no=%',r1.c_no;   
     raise notice'Owner=%',r1.owner;   
     raise notice'Model=%',r1.model;   
     raise notice'Car colour=%',r1.colour; 
    end loop;
   close c1;
  
if not found then
   raise exception'Invalid driver';
  end if;  
 end;
$$
language plpgsql;

select disp_car();