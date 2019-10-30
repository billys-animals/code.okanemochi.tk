---
title: 21 transactionを使ってみよう
author: pg1965
type: post
date: 2018-03-08T06:08:12+00:00
url: /2018/03/08/21-transactionを使ってみよう/
categories:
  - Sqlite

---
```tsql
drop table if exists users;
CREATE table users (
  id integer primary key,
  name text,
  score integer,
  team text
);
insert into users (name, score, team) values ('taguchi', 43, 'team-A');
insert into users (name, score, team) values ('fkoji',   80, 'team-B');
insert into users (name, score, team) values ('tashiro', 65, 'team-B');
insert into users (name, score, team) values ('hayashi', 54, 'team-A');
insert into users (name, score, team) values ('sato',    74, 'team-C');

.headers on
.mode column

-- transaction

begin transaction;
update users set score = score - 10 where name = 'fkoji';
update users set score = score + 10 where name = 'taguchi';
-- commit;
rollback;

select * from users;
```
