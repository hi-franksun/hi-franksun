# Views StoredProcedure Cursor

## Views

### What Views

Views 是 SELECT query 的封装，本质上还是个 SELECT query，相当于类似在代码层进行的 view model 一个封装，这一层可以写在代码里，也可以写在 MySQL 里，本质上是一个查询封装，形成一个虚拟的表，这个虚拟的表不会对底层的表格产生任何影响，只是有查询的作用；

比如查询 player 表中，所有身高大于 2m 的 player，可以写成 `SELECT player_id, player_name, player_height FROM player WHERE height > 2`，这个查询函数经常使用，那么就可以写成一个 View，`CREATE VIEW player_about_2m as SELECT player_id, player_name, player_height FROM player WHERE height > 2`；

以后直接查询 `player_about_2m` 即可：`SELECT * FROM player_about_2m`；

### Why Views

#### Advantage

1. 安全性：view 是基于底层的数据表的，不会对底层的数据进行修改。在一些权限控制系统中，view 能起到一部分权限的作用，比如对一般权限的 worker，不能从表中读取薪资，这个时候形成一个不带薪资 column view 的虚拟表，这些 worker 在查询的时候就只能有查询这个虚拟表的权限就可以了；
2. 清晰简单：view 比较适合很长的查询参数时候使用，调用者不需要知道底层的查询，只要根据 view name 的意思就能直接使用，这样子也能避免很多人为的错误，和大量的重复编写(当然也可以在代码里封装)，其实 MySQL 提供的这个功能和软件编写中封装、模块化编程的思想是一致的，开发人员常见了一个 view 其他人可以进行使用。

#### Disadvantage

1. 功能有限：view 基本上每次都会取查询 MySQL，且因为安全性不会就不能对 table 进行修改。

### How Use View

#### View Syntax

1. CREATE VIEW Statement
    ```sql
    CREATE VIEW view_name AS
    SELECT column1, column2
    FROM table
    WHERE condition
    ```
    可以看到除了 `CREATE VIEW view_name AS` 其他和 查询没有任何的区别；

2. Modify View Statement
    ```sql
    CREATE VIEW view_name AS
    SELECT column1, column2
    FROM table
    WHERE condition
    ```
    和 CREATE 语法是一样的，就可以理解为重新创建了 View 覆盖原来的 View
3. Delete View Statement
    ```sql
    DROP VIEW view_name
    ```
4. Use View
    ```sql
    SELECT * FROM view_name -- 直接使用就可以了，默认也就是一个虚拟的表
    ```

---

## Store Procedure

### What Store Procedure

是 SQL 语句的封装。一旦存储过程被创建出来，使用它就像使用函数一样简单，我们直接通过调用存储过程名即可。

### Why Use Store Procedure

优点：
1. 和视图一样，进行了封装，符合开发的封装原则，能被重用和代码清晰；
2. 存储过程是创建的时候会被编译一次，可以永久使用；
3. 封装了代码，减少一系列查询的次数，减少了连接次数，同样的也减少了网络传输的流量消耗；
4. 可以设置权限，相对安全；

缺点：
1. 可移植性差，存储过程不能跨数据库移植，比如在 MySQL、Oracle 和 SQL Server 里编写的存储过程，在换成其他数据库时都需要重新编写；
2. 调试困难，只有少数 DBMS 支持存储过程的调试。对于复杂的存储过程来说，开发和维护都不容易；
3. 存储过程的版本管理也很困难，比如数据表索引发生变化了，可能会导致存储过程失效。我们在开发软件的时候往往需要进行版本管理，但是存储过程本身没有版本控制，版本迭代更新的时候很麻烦；
4. 不适合高并发的场景，高并发的场景需要减少数据库的压力，有时数据库会采用分库分表的方式，而且对可扩展性要求很高，在这种情况下，存储过程会变得难以维护，增加数据库的压力，显然就不适用了；


### How Use Store Procedure

1. create procedure
    ```
    CREATE PROCEDURE 存储过程名称 ([参数列表])
    BEGIN
        需要执行的语句
    END
    ```

    > 关于流程控制语句，暂时忽略，现阶段我很少使用，后续使用到再会进行学习


2. delete procedure
    DROP PROCEDURE

3. modifity procedure
    ALTER ROCEDURE

---

## Cursor

游标是一个临时的的数据库对象，其实就是一个指针，用来标记数据对象中单个数据，进行操作；

创建游标的 5 步骤：

1. 定义游标：DECLARE cursor_name CURSOR FOR select_statement
2. 打开游标：OPEN cursor_name
3. 从游标中获取数据：FETCH cursor_name INTO var_name ...
4. 关闭游标：CLOSE cursor_name
5. 释放游标：DEALLOCATE PREPARE

一个游标的实例：

```sql
CREATE PROCEDURE `calc_hp_max`()
BEGIN
       -- 创建接收游标的变量
       DECLARE hp INT;  
       -- 创建总数变量 
       DECLARE hp_sum INT DEFAULT 0;
       -- 创建结束标志变量  
       DECLARE done INT DEFAULT false;
       -- 定义游标     
       DECLARE cur_hero CURSOR FOR SELECT hp_max FROM heros;
       
       OPEN cur_hero;
       read_loop:LOOP 
       FETCH cur_hero INTO hp;
       SET hp_sum = hp_sum + hp;
       END LOOP;
       CLOSE cur_hero;
       SELECT hp_sum;
       DEALLOCATE PREPARE cur_hero;
END

```

需要处理一些复杂的数据行计算的时候，游标就会起到作用了，但是在各个公司游标很少使用，或者说，很多游标的操作都被封装到了 orm 框架里；
