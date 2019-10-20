---
title: 20 update､deleteを使おう
author: pg1965
type: post
date: 2018-03-08T06:08:47+00:00
url: /2018/03/08/20-update､deleteを使おう/
page_layout:
  - def
sub:
  - def
side:
  - def
index:
  - index
follow:
  - follow
pvc_views:
  - 1
categories:
  - Sqlite

---
<pre class="lang:tsql decode:true ">drop table if exists users;
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

-- update
update users set score = 0, name = '* ' || name where score &lt; 60;
select * from users;

-- delete
-- delete from users;
delete from users where score = 0;
select * from users;</pre>

&nbsp;