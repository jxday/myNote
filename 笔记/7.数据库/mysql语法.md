要查看一下表的定义，可以使用如下命令：DESC tablename;



查看创建表的SQL语句，可以使用如下命令查看：mysql> show create table tablename;



表的删除命令如下：DROP TABLE tablename; 



修改表：alter table



```
coalesce(a,b,c);
参数说明：如果a==null,则选择b；如果b==null,则选择c；如果a!=null,则选择a；如果a b c 都为null ，则返回为null（没意义）。
```





with rollup 通常和group by 语句一起使用，是根据维度在分组的结果集中进行聚合操作。——对group by的分组进行汇总。注意：

1）ORDER BY不能在rollup中使用，两者为互斥关键字；

2）如果分组的列包含NULL值，那么rollup的结果可能不正确，因为在rollup中进行的分组统计时，null具有特殊意义。因此在进行rollup时可以先将

null转换成一个不可能存在的值，或者没有特别含义的值，比如：IFNULL(xxx,0)

3）mysql中没有像oracle那样的grouping()函数；



拼接字符串CONCAT	日期的年/月/日	year/month/day

CONCAT	(	YEAR(createTime)	,	'-'	,	MONTH(createTime)	) month



now()函数获取当前时间



子查询的关键字主要包括 in、not in、=、!=、exists、not exists



记录联合：union/union all	

UNION和UNION ALL的主要区别是UNION ALL是把结果集直接合并在一起，而UNION是将UNIONALL后的结果进行一次DISTINCT，去除重复记录后的结果



bin()（显示为二进制格式）或者hex()（显示为十六进制格式）函数进行读取。



```
exec sp_databases; --查看数据库
exec sp_tables;        --查看表
exec sp_columns student;--查看列
exec sp_helpIndex student;--查看索引
exec sp_helpConstraint student;--约束
exec sp_stored_procedures;
exec sp_helptext 'sp_stored_procedures';--查看存储过程创建、定义语句 经常用到这句话来查看存储过程，like sp_helptext sp_getLoginInfo.
exec sp_rename student, stuInfo;--修改表、索引、列的名称
exec sp_renamedb myTempDB, myDB;--更改数据库名称
exec sp_defaultdb 'master', 'myDB';--更改登录名的默认数据库
exec sp_helpdb;--数据库帮助，查询数据库信息
exec sp_helpdb master;
```





查询sql优化过程：

explain select * from ct_order;

show warnings;



优化追踪：可以看到整个优化器的优化过程。

SET optimizer_trace="enabled=on";

select * from ct_order;

select * from information_schema.optimizer_trace;



设置系统变量：

set global optimizer_prune_level = 0;