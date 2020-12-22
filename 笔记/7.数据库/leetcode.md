

# leetcode

> > mysql官网文档地址：https://dev.mysql.com/doc/
> >
> > 

> 20200518

175:

![1.jpg](https://pic.leetcode-cn.com/ad3df1c4ecc7d2dbe85f92cdde8ec9a731fdd20dc4c5629ecb372b21de26c682-1.jpg)



在使用left jion时，on和where条件的区别如下：

1、on条件是在生成临时表时使用的条件，它不管on中的条件是否为真，都会返回左边表中的记录。

2、where条件是在临时表生成好后，再对临时表进行过滤的条件。这时已经没有left join的含义（必须返回左边表的记录）了，条件不为真的就全部过滤掉。



176

```mysql
SELECT
    [ALL | DISTINCT | DISTINCTROW ]
    [HIGH_PRIORITY]
    [STRAIGHT_JOIN]
    [SQL_SMALL_RESULT] [SQL_BIG_RESULT] [SQL_BUFFER_RESULT]
    [SQL_CACHE | SQL_NO_CACHE] [SQL_CALC_FOUND_ROWS]
    select_expr [, select_expr] ...
    [into_option]
    [FROM table_references
      [PARTITION partition_list]]
    [WHERE where_condition]
    [GROUP BY {col_name | expr | position}
      [ASC | DESC], ... [WITH ROLLUP]]
    [HAVING where_condition]
    [ORDER BY {col_name | expr | position}
      [ASC | DESC], ...]
    [LIMIT {[offset,] row_count | row_count OFFSET offset}]
    [PROCEDURE procedure_name(argument_list)]
    [into_option]
    [FOR UPDATE | LOCK IN SHARE MODE]

into_option: {
    INTO OUTFILE 'file_name'
        [CHARACTER SET charset_name]
        export_options
  | INTO DUMPFILE 'file_name'
  | INTO var_name [, var_name] ...
}
```



177.

where后面是对前表的限制，不需要拘泥于 a .price > xxx的形式，

```mysql
#例如
SELECT 
  DISTINCT e.salary
FROM 
  employee e
WHERE 
  (SELECT count(DISTINCT salary) FROM employee WHERE salary>e.salary) = N-1;


SELECT 
  DISTINCT e1.salary
FROM 
  employee e1 JOIN employee e2 ON e1.salary <= e2.salary
GROUP BY 
  e1.salary
HAVING 
  count(DISTINCT e2.salary) = N;
 
```

[排序函数.md](/Users/cty/Documents/myData/笔记/7.数据库/排序函数.md)



> 20200519

178 

别名与关键字不能重复，由于关键字增加，以后别名需要加上\` 别名\` 

```mysql
select article, dense_rank() over (order by article desc)  as `Rank` from shop;
```



> 20200520

180

### :=和=的区别

- =
    - 只有在set和update时才是和:=一样，**赋值**的作用，其它都是**等于**的作用。鉴于此，用变量实现行号时，必须用:=
- :=
    - 不只在set和update时时赋值的作用，在select也是赋值的作用。



181

If more than one *CTE_query_definition* is defined, the query definitions must be joined by one of these set operators: UNION ALL, UNION, EXCEPT, or INTERSECT.



182

group by .. having...



183

where exists...

select查询的结果是否[exists/not exists]在exists后的子表中存在



184

分解问题



185

排序函数语法：

 DENSE_RANK() OVER(partition by xxx order by xxx DESC)