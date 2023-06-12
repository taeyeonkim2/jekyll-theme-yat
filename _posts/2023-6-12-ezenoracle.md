---
layout: post
title: 2023-6-12-ezen
categories: 수업-오라클
---

## Theater 연습문제

```
    -- theater 계정 삭제
drop user theater;

-- 계정만들기 sql 문
CREATE USER THEATER IDENTIFIED BY 1234;
alter user THEATER default tablespace users;
alter user THEATER temporary tablespace temp;

CREATE USER THEATER IDENTIFIED BY 1234
default tablespace users
temporary tablespace temp;
-------------------------------------
-- 용량설정
alter user THEATER quota unlimited on users;
-- 권한부여 : DCL(데이터 제어어)
grant connect to THEATER;
grant resource to THEATER;

--테이블 만들기
create table theaterTBL(
    t_no number(3) not null,
    t_name nvarchar2(10) not null,
    t_addr nvarchar2(10) not null
);

create table cinemaTBL(
     t_no number(3) not null,
     c_no number(3) not null,
     title nvarchar2(50) not null,
     price number(5) not null,
     seatcount number(4) not null
);

create table reservationTBL(
    t_no number(3) not null,
    c_no number(3) not null,
    cust_no number(10) not null,
    seat_no number(4) not null,
    vdate date default sysdate --sysdate
);

select sysdate from dual; --23/06/12

create table customer(
    cust_no number(10) not null,
    cust_name nvarchar2(10) not null,
    cust_addr nvarchar2(50) --null 허용
);

--pk 재정의
alter table theaterTBL add constraint theater_pk primary key(t_no); 
--theaterTBL을 바꿀거야 제약조건 추가할건데 제약조건의 이름은 theater_pk
alter table cinemaTBL add constraint cinema_pk primary key(t_no,c_no);
--cinemaTBL의 기본키를 t_no,c_no로 할거야
alter table reservationTBL add constraint reservation_pk primary key(t_no,c_no,cust_no);

alter table customer  add constraint customer_pk primary key(cust_no);

--fk정의
alter table cinemaTBL add constraint theater_cinema_fk foreign key(t_no) references theaterTBL(t_no);
--테이블을 수정할거야 cinema 거기에 제약 조건을 추가할 거야 theater_cinema_fk이름으로.
alter table reservationTBL add constraint cinema_reservation_fk foreign key(t_no,c_no) references cinemaTBL(t_no,c_no);
alter table reservationTBL add constraint customer_reservation_fk foreign key(cust_no) references customer(cust_no);
--그 외 제약조건
alter table cinemaTBL add constraint price_ck check(price<20000);
alter table cinemaTBL add constraint c_no_ck check(c_no between 1 and 10);

-----모든 테이블 삭제------외래키가 있으면 삭제가 안될 수 있다. 순서가 중요하다.
drop table theaterTBL ; --참조당하고 있어서 삭제가 안돼 4
drop table reservationtbl; --가장 많이 참조하고 있는 테이블 먼저 삭제 1
drop table customer; --2
drop table cinematbl; --3
---------테이블 만들기 2(create에서 제약조건 모두 설정)---------
--외래키 제약조건때문에 만드는 순서가 중요하다. 삭제의 역순 !참조되어지는 테이블부터 생성)
```
