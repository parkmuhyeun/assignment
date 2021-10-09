# sql
#Database/sql

---

```sql
1
select sid, sname, deptno
from student
where birthdate between '96/01/01' and '96/12/31'
and addr = 'Busan';

2
select sname, deptno
from student s
where advisor = (select pid from professor where major = 'Novel');

3
select addr, avg(grade)
from student
group by addr
order by avg(grade) desc;

4
select s.sname, d.dname , p.pname
from student s, professor p, department d
where s.addr = 'Daegu' and s.gen = 'F'
and s.deptno = d.deptno
and s.advisor = p.pid;

5
select avg(s.grade) 평균학점
from student s, department d
where s.deptno = d.deptno and d.college = 'Engineering';

6
select dname, count(*) 학생수
from student s, department d
where s.deptno = d.deptno
group by dname
having count(*) = (select max(count(*)) from student group by deptno);

7
select 직원이름, 나이
from 직원
where 아이디 in ( select 직원아이디 from 근무 where 팀번호 = 10)

select 팀번호, 팀이름
from  팀
where 팀번호 in (select 팀번호 from 근무 group by 팀번호 having count(*) >= 5)

select 직원이름, 월급
from 직원
where 아이디 =  (select 직원아이디 from 근무 where 근무시간 = ( select max(근무시간) from 근무)   )

8
select 고객번호, 차량번호, 예약일자
from 예약
where 예약일자 between '15/11/01' and '15/11/30'

select res.차량번호, car.차종  , res.예약일자
from 예약 res, 렌트카 car
where res.고객번호 = ( select 고객번호 from 고객 where 이름 =  '홍길동') and res.차량번호 = car.차량번호


select 차종
from 렌트카 car, 예약 res
where 렌트카.차량번호 = res.차량번호
group by 차종
having count(*) = ( select max(count(*))  from 예약 group by 차량번호)
```
