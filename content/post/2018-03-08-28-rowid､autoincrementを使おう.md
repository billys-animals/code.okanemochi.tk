---
title: 28 rowid､autoincrementを使おう
author: pg1965
type: post
date: 2018-03-08T06:00:50+00:00
url: /2018/03/08/28-rowid､autoincrementを使おう/
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
categories:
  - Sqlite

---
<pre class="lang:tsql decode:true ">-- rowid

drop table if exists users;
create table users (
  -- id integer primary key, -- =&gt; rowid
  id integer primary key autoincrement,
  name
);
insert into users (name) values ('a');
insert into users (name) values ('b');
insert into users (name) values ('c'); -- 3
select rowid, * from users;
delete from users where id = 3;
insert into users (name) values ('d'); -- 3
select rowid, * from users;</pre>

&nbsp;