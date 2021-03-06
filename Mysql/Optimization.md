# Optimization 优化
<!-- TOC -->

- [Optimization 优化](#optimization-%E4%BC%98%E5%8C%96)
    - [优化数据大小 Data Size](#%E4%BC%98%E5%8C%96%E6%95%B0%E6%8D%AE%E5%A4%A7%E5%B0%8F-data-size)
        - [Table Columns](#table-columns)
        - [Row Format](#row-format)
        - [Index](#index)
        - [join](#join)
        - [Normalization](#normalization)
    - [Optimizing SQL Statement](#optimizing-sql-statement)
        - [Optimizing SELECT Statements](#optimizing-select-statements)
        - [Index Merge Optimization](#index-merge-optimization)

<!-- /TOC -->

## 优化数据大小 Data Size
### Table Columns
    * 尽量使用小的数据类型，比如MEDIUMINT要比INT类型要好，因为节省了25%空间
    * 尽量设置NOT NULL数据，如果可能的话。这样可以让索引更加有效，同时也节约了判断是否为NULL的情况，节约了空间，使用一个bit就能满足。

### Row Format
tbd
### Index
* 主键越小越好。二级索引的时候，都会使用到这个主键，节约空间
* 只有在需要的优化查询的时候，才创建索引。 
* 尽量给String列，创建前缀索引

### join
* In some circumstances, it can be beneficial to split into two a table that is scanned very often. This is especially true if it is a dynamic-format table and it is possible to use a smaller static format table that can be used to find the relevant rows when scanning the table.
* 尽量使用同一样的列类型去定义，在不同的表中。
* 保持列名的简洁。尽量小于18 characters。

### Normalization
* Normally, try to keep all data nonredundant (observing what is referred to in database theory as third normal form). Instead of repeating lengthy values such as names and addresses, assign them unique IDs, repeat these IDs as needed across multiple smaller tables, and join the tables in queries by referencing the IDs in the join clause. 尽量保持数据不重复，不冗余。第三范式。

* If speed is more important than disk space and the maintenance costs of keeping multiple copies of data, for example in a business intelligence scenario where you analyze all the data from large tables, you can relax the normalization rules, duplicating information or creating summary tables to gain more speed. 如果存储廉价的情况下，可以通过大表的形式或者创建终结表的形式进行加速。


## Optimizing SQL Statement
* 慢查询select ... where 是否可以增加索引。
* 减少全表扫描
* 执行Analyze Table进行表的优化，换句话说，修复index。即索引的散列。Mysql优化器会在进行优化，产出更加有效的执行计划。因为优化器会使用一些统计信息，而 analyze table 会更新这些信息。
* InoDB的只读事务。 Read-Only Transaction
* 相应的调整Mysql的内存大小。

### Optimizing SELECT Statements
https://dev.mysql.com/doc/refman/5.7/en/where-optimization.html

### Index Merge Optimization
https://www.jianshu.com/p/1c00dc8be14a
