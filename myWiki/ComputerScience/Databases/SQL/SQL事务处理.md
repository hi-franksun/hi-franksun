# 事务

## 事务处理
我们知道在 MySQL 5.5 版本之前，默认的存储引擎是 MyISAM，在 5.5 版本之后默认存储引擎是 InnoDB。InnoDB 和 MyISAM 区别之一就是 InnoDB 支持事务，也可以说这是 InnoDB 取代 MyISAM 的重要原因。那么什么是事务呢？事务的英文是 transaction，它是进行一次处理的基本单元，要么完全执行，要么都不执行。

可以查看 mysql 的引擎：

```sql
MariaDB [(none)]> show engines;
+--------------------+---------+-------------------------------------------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                                                         | Transactions | XA   | Savepoints |
+--------------------+---------+-------------------------------------------------------------------------------------------------+--------------+------+------------+
| CSV                | YES     | Stores tables as CSV files                                                                      | NO           | NO   | NO         |
| MRG_MyISAM         | YES     | Collection of identical MyISAM tables                                                           | NO           | NO   | NO         |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables                                       | NO           | NO   | NO         |
| Aria               | YES     | Crash-safe tables with MyISAM heritage. Used for internal temporary tables and privilege tables | NO           | NO   | NO         |
| MyISAM             | YES     | Non-transactional engine with good performance and small data footprint                         | NO           | NO   | NO         |
| SEQUENCE           | YES     | Generated tables filled with sequential values                                                  | YES          | NO   | YES        |
| InnoDB             | DEFAULT | Supports transactions, row-level locking, foreign keys and encryption for tables                | YES          | YES  | YES        |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                                                              | NO           | NO   | NO         |
+--------------------+---------+-------------------------------------------------------------------------------------------------+--------------+------+------------+
8 rows in set (0.001 sec)
```

事务的常用控制语句都有哪些。

1. START TRANSACTION 或者 BEGIN，作用是显式开启一个事务。
2. COMMIT：提交事务。当提交事务后，对数据库的修改是永久性的。
3. ROLLBACK 或者 ROLLBACK TO [SAVEPOINT]，意为回滚事务。意思是撤销正在进行的所有没有提交的修改，或者将事务回滚到某个保存点。
4. SAVEPOINT：在事务中创建保存点，方便后续针对保存点进行回滚。一个事务中可以存在多个保存点。
5. RELEASE SAVEPOINT：删除某个保存点。
6. SET TRANSACTION，设置事务的隔离级别。


MySQL 默认自动提交，当然我们可以配置 MySQL 的参数，`set autocommit = 0; Query OK, 0 rows affected (0.001 sec)`


### 事务的简单定义

1. 定义
这种把多条语句作为一个整体进行操作的功能，被称为数据库事务。数据库事务可以确保该事务范围内的所有操作都可以全部成功或者全部失败。如果事务失败，那么效果就和没有执行这些SQL一样，不会对数据库数据有任何改动。

2. 事务的特性：ACID

- A：Atomicity，原子性，将所有SQL作为原子工作单元执行，要么全部执行，要么全部不执行，可以把它理解为组成物质的基本单位，也是我们进行数据处理操作的基本单位；
- C：Consistency，一致性，事务完成后，所有数据的状态都是一致的，即A账户只要减去了100，B账户则必定加上了10，数据库在进行事务操作后，会由原来的一致状态，变成另一种一致的状态。也就是说当事务提交后，或者当事务发生回滚后，数据库的完整性约束不能被破坏。；
- I：Isolation，隔离性，如果有多个事务并发执行，每个事务作出的修改必须与其他事务隔离，也就是说一个事务在提交之前，对其他事务都是不可见的；
- D：Durability，持久性，即事务完成后，对数据库数据的修改被持久化存储，即使在系统出故障的情况下，比如系统崩溃或者存储介质发生故障，数据的修改依然是有效的。因为当事务完成，数据库的日志就会被更新，这时可以通过日志，让系统恢复到最后一次成功的更新状态。

ACID 可以说是事务的四大特性，在这四个特性中，原子性是基础，隔离性是手段，一致性是约束条件，而持久性是我们的目的。原子性和隔离性比较好理解，这里我讲下对一致性的理解（国内很多网站上对一致性的阐述有误，具体你可以参考 Wikipedia 对 [Consistency](https://en.wikipedia.org/wiki/ACID) 的阐述）。


## 事务隔离异常
SQL-92 标准中已经对 3 种异常情况进行了定义，这些异常情况级别分别为脏读（Dirty Read）、不可重复读（Nnrepeatable Read）和幻读（Phantom Read）。


dirty read：读取了还未 commit 的数据/事务
Nnrepeatable read：同一条记录，两次读取的结果不同，第二次读取时候被别人修改了。
Phantom read：事务 A 根据条件查询得到了 N 条数据，但此时事务 B 更改或者增加了 M 条符合事务 A 查询条件的数据，这样当事务 A 再次进行查询的时候发现会有 N+M 条数据，产生了幻读。


## 事物隔离级别

四种隔离级别从低到高分别是：读未提交（READ UNCOMMITTED）、读已提交（READ COMMITTED）、可重复读（REPEATABLE READ）和可串行化（SERIALIZABLE）。这些隔离级别能解决的异常情况如下表所示：

| 隔离级别 | 脏读 | 不可重复读 | 幻读 |
| -- | -- | -- | -- |
| 读未提交（READ UNCOMMITTED） | 允许 | 允许 | 允许 |
| 读已提交（READ COMMITTED） | 禁止 | 允许 | 允许 |
| 可重复读（REPEATABLE READ） | 禁止 | 禁止 | 允许 |
| 可串行化（SERIALIZABLE） | 禁止 | 禁止 | 禁止 |

> 可串行化能避免所有的异常情况，而读未提交则允许异常情况发生

1. 读未提交，也就是允许读到未提交的数据，这种情况下查询是不会使用锁的，可能会产生脏读、不可重复读、幻读等情况。
2. 读已提交就是只能读到已经提交的内容，可以避免脏读的产生，属于 RDBMS 中常见的默认隔离级别（比如说 Oracle 和 SQL Server），但如果想要避免不可重复读或者幻读，就需要我们在 SQL 查询的时候编写带加锁的 SQL 语句（我会在进阶篇里讲加锁）。
3. 可重复读，保证一个事务在相同查询条件下两次查询得到的数据结果是一致的，可以避免不可重复读和脏读，但无法避免幻读。MySQL 默认的隔离级别就是可重复读。
4. 可串行化，将事务进行串行化，也就是在一个队列中按照顺序执行，可串行化是最高级别的隔离等级，可以解决事务读取中所有可能出现的异常情况，但是它牺牲了系统的并发性。

