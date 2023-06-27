# oracle

1
create user aleksandre identified by chakhvadze
default TABLESPACE users QUOTA UNLIMITED on users;
grant create session, create table, create view to aleksandre;
grant select, update on hr.employees to aleksandre;
create role role_chakhvadze;
grant create sequence, create synonym to role_chakhvadze;
grant select, insert on hr.departments to role_chakhvadze;
grant role_chakhvadze to aleksandre;

3
create table exam (exam_id number(4) primary key, exam_name varchar2(20) unique);
create table students (id number(4) primary key, last_name varchar2(20) not null,
exam_id number(4) REFERENCES exam(exam_id));

4
create table workers as
select * from hr.employees where department_id &gt;50;

5
create view my_vu_aleksandre as
select * from workers where department_id =60
with check option;

6
create sequence chakhvadze_seq start with 12 increment by 6 maxvalue 450;

7
create index index_chakhvadze on students(last_name);

8
create synonym vu_chakhvadze for my_vu_aleksandre;

9
insert into exam values (chakhvadze_seq.nextval, &#39;excel&#39;);
insert into exam values (chakhvadze_seq.nextval, &#39;python&#39;);
insert into students (id, last_name)
select employee_id, last_name from workers;
commit;

10
update students set exam_id = (select min(exam_id) from exam) where length(last_name)&lt;7;

11
delete from students where exam_id is null or exam_id = (select max(exam_id) from exam);

12
with tab_aleksandre as
(select department_id department, count(*) stuff, sum(salary) full_salary
from workers
group by department_id)
select * from tab_aleksandre;

13
select employee_id, last_name, salary, hire_date
from hr.employees
where regexp_like(last_name, &#39;sa|ta|mo|ka|pi|lu|ro&#39;)

14
drop table workers;
drop sequence chakhvadze_seq;
drop view my_vu_aleksandre;
drop index index_chakhvadze;
drop synonym vu_chakhvadze;

