# database
数据库操作
#
# Sqlserver和Oracle批量处理表名和字段名的大写

#### sqlserver修改表名大写sql

---

```sql
declare @sql varchar(300)--,@rowcount varchar(10),@dyncnum int
declare @tablename varchar(100)
declare cursor1 cursor for
select name from sysobjects where xtype = 'u' order by name
open cursor1
fetch next from cursor1 into @tablename
while @@fetch_status=0
begin
set @sql='sp_rename '''+@tablename+''','''+upper(@tablename)+''''
print @sql
--exec(@sql)
fetch next from cursor1 into @tablename
end
close cursor1
deallocate cursor1
```

---

#### sqlserver修改字段名大写

---

```sql
declare @sql varchar(300)--,@rowcount varchar(10),@dyncnum int
declare @tablename varchar(100)
declare cursor1 cursor for
select name from sysobjects where xtype = 'u' order by name
open cursor1
fetch next from cursor1 into @tablename
while @@fetch_status=0
begin
set @sql='sp_rename '''+@tablename+''','''+upper(@tablename)+''''
print @sql
--exec(@sql)
fetch next from cursor1 into @tablename
end
close cursor1
deallocate cursor1
```

---

#### Oralce修改表名大写

###### 首先使用语句查询需要语句，然后执行就ok了

---

```sql
select 'alter table "'||table_name||'" rename to '||upper(table_name)||';' from user_tables where table_name<>upper(table_name);
```

---

##### Oracle修改字段名大写

---

```sql
select 'alter table '||table_name||' rename column "'|| column_name ||'" to '||upper(column_name)||';' from user_tab_columns where column_name<>upper(column_name);
```

---

##### 补充

---

```sql
--清除数据
select 'TURNCATE table '||table_name ||';' from user_tables;
--删除表
select 'drop table '||table_name ||';' from user_tables;
```

---

##### Oracle数据库备份还原不成功问题。由于11g新特性,增加了参数deferred\_segment\_creation，参数默认值是TRUE,这样,新建的表无记录时,是滞后分配段的,甚至连DDL定义也无法获取,所以EXP无法导出空表.

```sql
-- ######方式1：
select 'alter table '|| table_name ||' move;' from user_tables where segment_created='NO';
-- ######方式2：（暂时未尝试过）
-- 解决方法：用本用户登录,下面三个语句查看,结果是不是0行记录（通常第一个肯定不是0条）
(1)select 'alter table '||table_name||' allocate extent;'from user_tables WHERE SEGMENT_CREATED='NO';
(2)select * from user_indexes WHERE SEGMENT_CREATED='NO';
(3)select * from user_lobs where segment_created='NO';
```


