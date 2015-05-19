# SQLのJOIN
- 概要
http://qiita.com/sgtn/items/aa51332f62ba96cc441e#comment-984c44d18e0103577748

- データの準備

```sql
create database study_mysql;
use study_mysql;

create table dogs (id int, name varchar(20), owner_id int);
insert into dogs values (1,'aka',1), (2,'ao',2), (3,'shiro',1), (4,'kuro',4);

create table owners (id int, name varchar(20));
insert into owners values (1,'ichiro'), (2,'jiro'), (3,'saburo');
```

## 内部結合 (INNER JOIN)
内部結合では左右それぞれのテーブルの指定したカラムの値が一致するレコードだけを取得します

```sql
select * from dogs inner join owners on owners.id = dogs.owner_id;
```

## 外部結合 (LEFT JOIN, RIGHT JOIN)
外部結合は左右それぞれのテーブルの指定したカラムの値が一致するレコードに加えてどちらかのテーブルにしか存在しないデータについても取得します

### LEFT JOIN

```sql
select * from dogs left join owners on dogs.owner_id = owners.id;

select * from dogs left join owners on dogs.owner_id = owners.id where owners.id is null;
```

### RIGHT JOIN

```sql
select * from dogs right join owners on dogs.owner_id = owners.id;

select * from dogs right join owners on dogs.owner_id = owners.id where dogs.id is null;
```

## 完全外部結合 (FULL OUTER JOIN)
MySQLではFULL OUTER JOINは使えない
代わりとして UNION を使う

```sql
select * from dogs left join owners on owners.id = dogs.owner_id
union
select * from dogs right join owners on owners.id = dogs.owner_id;
```

```sql
select * from dogs left join owners on owners.id = dogs.owner_id where dogs.id is null
union
select * from dogs right join owners on owners.id = dogs.owner_id where owners.id is null;
```

### UNION [DISTINCT | ALL]
DISTINCT は重複行を削除
ALL は重複行を削除しない

```sql
select dogs.name from dogs left join owners on owners.id = dogs.owner_id
union distinct
select owners.name from dogs right join owners on owners.id = dogs.owner_id;
```

```sql
select dogs.name from dogs left join owners on owners.id = dogs.owner_id
union all
select owners.name from dogs right join owners on owners.id = dogs.owner_id;
```

## source
- http://mathemathiko.hatenablog.com/entry/2012/06/08/201735
