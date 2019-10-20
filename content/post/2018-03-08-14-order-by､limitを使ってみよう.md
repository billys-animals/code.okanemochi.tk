---
title: 14 order by､limitを使ってみよう
author: pg1965
type: post
date: 2018-03-08T06:11:55+00:00
url: /2018/03/08/14-order-by､limitを使ってみよう/
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
  score integer
);
insert into users (name, score) values ('taguchi', 43);
insert into users (name, score) values ('fkoji',   80);
insert into users (name, score) values ('tashiro', 65);
insert into users (name, score) values ('hayashi', 54);
insert into users (name, score) values ('sato',    74);
insert into users (name, score) values ('ohashi',  null);

-- order by
-- select * from users where score is not null order by score;
-- select * from users where score is not null order by score desc;

-- limit
-- select * from users where score is not null order by score desc limit 3;
-- select * from users where score is not null order by score desc limit 3 offset 2;
select * from users where score is not null order by score desc limit 2, 3;</pre>

&nbsp;