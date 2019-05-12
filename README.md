# Databases
<br/>


## Create database (DDL - Data Definition Language)
```sql
CREATE DATABASE register
DEFAULT CHARACTER SET utf8
DEFAULT COLLATE utf8_general_ci;
```
<br/>


## Delete database
```sql
DROP DATABASE register;
```
<br/>


## Create table (DDL - Data Definition Language)
#### Constraints
* `NOT NULL` Don't allow be null
* `PRIMARY KEY` Define the primary key to indentify the registries
* `AUTO_INCREMENT` It's used to the MySQL goind incrementing 1 in de id
* `DEFAULT` Case wasn't defined, go to default value
* `UNSIGNED` Remove the signal of the numbers
* `UNIQUE` Unique regitry with that column value

```sql
use register;

create table people (
	`id` int not null auto_increment primary key,
    `name` varchar(50),
    `height` decimal(3, 2),
    `weight` decimal(5, 2),
    `birth_date` date,
    `gender` enum("M", "F"),
    `nationality` varchar(5) default 'BR'
) default charset utf8;


create table if not exists courses (
	`name` varchar(20) not null unique,
    `description` text,
    `duration_time` int unsigned,
    `total_classes` tinyint unsigned,
    `year` year default '2019'
) default charset utf8;
```
<br/>


## Alter Table (DDL - Data Definition Language)
```sql
-- Add in the final
alter table people
add column `occupation` varchar(10);

-- Add in the start
alter table people
add column `occupation` varchar(10) first;

-- Add after some column
alter table people
add column `occupation` varchar(10) after `name`;

-- Remove a column
alter table people
drop column `occupation`;

-- Modify column
alter table people
modify `occupation` varchar(50) not null default '';

-- Change column name
alter table people
change column `occupation` `work` varchar(30) default '';

-- Change table name
alter table people
RENAME TO users;
```
<br />


## Drop Table (DDL - Data Definition Language)
```sql
drop table if exists users
```
<br/>


## Insert into (DML - Data Manipulation Language)
```sql
use register;

insert into people
(`name`, `height`, `weight`, `birth_date`, `gender`, `nationality`)
values
('Anderson', '1.65', '51.46', '2001-07-06', 'M', 'BR');

insert into people values
(DEFAULT, 'Marie', '1.56', '55.00', '1989-11-05', 'F', 'IRL'),
(DEFAULT, 'Liskov', '1.78', '75.00', '1939-03-29', 'F', 'USA');
```
<br/>


## Update (DML - Data Manipulation Language)
```sql
-- Update multiplies columns
update courses
set `name` = 'YouTube', `description` = "O curso que te ensina como ganhar muitos inscritos!"
where `id` = '5';


-- Update multiplies rows with limit
update courses
set `year` = "2045"
where `year` = '2016'
limit 2;
```
<br/>


## Delete row (DML - Data Manipulation Language)
```sql
-- Simple
delete from courses
where `id` = '98';

-- Or
delete from courses
where `id` = '98' or `id` = '98';

-- Grander than
delete from courses
where `year` > '2010';

-- Delete everthing
truncate table courses;
```


## Select (DQL - Data Query Language)
```sql
-- Select all
select * from people;

-- Select the rows and filter by nationality
select `name`,`nationality` from people where `nationality` = 'USA';

-- Order
select * from courses
order by `name`;

-- Order descent
select * from courses
order by `name` desc;

-- Grander | Minor than
select `name` from courses
where `year` > '2016'
order by `year` desc, `name`;

-- Grander and Minor than
select `name` from courses
where `year` <> '2016'
order by `year` desc, `name`;

-- Between
select `name` from courses
where `year` between '2016' and '2018'
order by `year` desc, `name`;

-- In
select `name` from courses
where `year` in ('2010', '2016', '2018')
order by `year` desc, `name`;

-- Logics NOT | AND | OR | = | !=
select `name` from courses
where `year` > '2010' and `year` < '2018' and `year` != '2017'
order by `year` desc, `name`;

-- Wildcards
-- 'a' -> it's equivalent to 'a' and 'A'
-- '%' -> represents any things, include nothing
-- '_' -> represents any character
-- result: words that start with 'a' and end with 's'
select * from courses
where `nome` like 'a%s';
```
<br/>


## Joins (DRL)
```sql
-- Define foreign key
alter table people
add foreign key (`course`)
references courses(`id`);

-- Make a inner joins - Intrisics relations
select p.`name`, c.`name`
from people as p inner join courses as c
on g.`course` != c.`id`;

-- Make a outer joins - Extrinsic relations with left | right priority
-- left -> Preference to list all rows of the left
-- right -> Preference to list all rows of the right
select p.`name`, c.`name`
from people as p left outer join courses as c
on g.`course` != c.`id`;



-- Multiplies tables
create table people_study_course (
	`id` int auto_increment primary key,
    `data` date,
    `idpeople` int,
    `idcourse` int,
    foreign key (idpeople) references people(id),
    foreign key (idcourse) references courses(id)
) default charset = utf8;

select p.`name`, c.`name`
from people as p join people_study_course as ass
on p.`id` = ass.`idpeople`
join courses as c
on ass.`idcourse` = c.`id`;
```