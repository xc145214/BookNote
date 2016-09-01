# mysql lock read

> mysql version :5.7

## 读锁

如果查询数据，然后在同一个事务中插入或更新相关的数据，常规SELECT 语句不能给予足够的保护，其他事务可以更新或删除刚刚查询相同的行。
为了提供额外的安全， InnoDB支持两种类型的 锁定读取 ：

+ `SELECT ... LOCK IN SHARE MODE` 在所读取行上设置共享模式锁。其他的会话可以读取行，但是不能修改它们。如果这些行被其他的事务改变但是没有提交，
你的查询将会等待其他的事务结束并读取最新的值。
+  `SELECT ... FOR UPDATE` 对于使用这个语句影响的行，会锁定的这些数据行和相关的索引。其他更新这些行的事务会被阻塞，或者读取事务隔离级别定义的隔离的数据。
一致性读将会忽略任何读记录上的设置任何锁。（记录旧版本不能被锁定;他们运用重建撤销日志上记录的内存副本。）

> `SELECT ... FOR UPDATE` 只有当自动提交被禁用（以transaction开始或者设置为0）时才会锁定这些需要被update的数据行。如果自动提交启用，符合规范的数据将不会被锁定。

### Usage Examples

Suppose that you want to insert a new row into a table child, and make sure that the child row has a parent row in table parent. Your application code can ensure referential integrity throughout this sequence of operations.

First, use a consistent read to query the table PARENT and verify that the parent row exists. Can you safely insert the child row to table CHILD? No, because some other session could delete the parent row in the moment between your SELECT and your INSERT, without you being aware of it.

To avoid this potential issue, perform the SELECT using LOCK IN SHARE MODE:
```
SELECT * FROM parent WHERE NAME = 'Jones' LOCK IN SHARE MODE;
```
After the LOCK IN SHARE MODE query returns the parent 'Jones', you can safely add the child record to the CHILD table and commit the transaction. Any transaction that tries to read or write to the applicable row in the PARENT table waits until you are finished, that is, the data in all tables is in a consistent state.

For another example, consider an integer counter field in a table CHILD_CODES, used to assign a unique identifier to each child added to table CHILD. Do not use either consistent read or a shared mode read to read the present value of the counter, because two users of the database could see the same value for the counter, and a duplicate-key error occurs if two transactions attempt to add rows with the same identifier to the CHILD table.

Here, LOCK IN SHARE MODE is not a good solution because if two users read the counter at the same time, at least one of them ends up in deadlock when it attempts to update the counter.

To implement reading and incrementing the counter, first perform a locking read of the counter using FOR UPDATE, and then increment the counter. For example:
```
SELECT counter_field FROM child_codes FOR UPDATE;
UPDATE child_codes SET counter_field = counter_field + 1;
```
A SELECT ... FOR UPDATE reads the latest available data, setting exclusive locks on each row it reads. Thus, it sets the same locks a searched SQL UPDATE would set on the rows.

The preceding description is merely an example of how SELECT ... FOR UPDATE works. In MySQL, the specific task of generating a unique identifier actually can be accomplished using only a single access to the table:
```
UPDATE child_codes SET counter_field = LAST_INSERT_ID(counter_field + 1);
SELECT LAST_INSERT_ID();
```
The SELECT statement merely retrieves the identifier information (specific to the current connection). It does not access any table.



























## 参考
[mysql 官方读锁文档](http://dev.mysql.com/doc/refman/5.7/en/innodb-locking-reads.html)