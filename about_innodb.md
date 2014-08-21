# innodb与性能


## 常用操作

### show engine innodb status

展示的信息包括

1. BACKGROUND THREAD
2. SEMAPHORES
3. FILE I/O
4. INSERT BUFFER AND ADAPTIVE HASH INDEX
5. LOG
6. BUFFER POOL AND MEMORY
7. ROW OPERATIONS
8. TRANSACTIONS


### 改变Innodb的日志文件数量及大小

[参考来源](http://dev.mysql.com/doc/refman/5.0/en/innodb-data-log-reconfiguration.html)

```
If innodb_fast_shutdown is set to 2, set innodb_fast_shutdown to 1:

mysql> SET GLOBAL innodb_fast_shutdown = 1;

After ensuring that innodb_fast_shutdown is not set to 2, stop the MySQL server and make sure that it shuts down without errors (to ensure that there is no information for outstanding transactions in the log).

Copy the old log files into a safe place in case something went wrong during the shutdown and you need them to recover the tablespace.

Delete the old log files from the log file directory.

Edit my.cnf to change the log file configuration.

Start the MySQL server again. mysqld sees that no InnoDB log files exist at startup and creates new ones.
```

## 关键参数

### innodb\_buffer\_pool\_size

用来缓冲数据页，缓冲区数据页中数据类型包括：

1. 索引页
2. 数据页
3. undo页
4. 插入缓冲(insert buffer)
5. 自适应哈希索引(adaptive hash index)
6. innodb存储的锁信息(lock info)
7. 数据字典信息(data dictionary)

一般缓冲区的页大小为**16KB**，采用LRU算法对缓冲池进行管理。

同时，Innodb对LRU管理不是直接把最新访问的页直接插入到LRU的首部，而是插入到midpoint的位置（默认是LRU尾端37%的位置），这样就避免了索引扫描等类似的操作，把LRU的真正热数据淘汰掉。

### innodb\_flush\_log\_at\_trx\_commit

如果这个值为0，那么log buffer的数据会每秒钟统一一次刷到log file，然后把log file的数据刷新到磁盘。

如果值为1(默认)，那么log buffer的数据会在每次事务提交的时候，把log file刷到磁盘。

如果值为2，那么log buffer的数据会每次commit都写入到log file，每秒钟刷一次log file到磁盘。

注意：

1. 如果要完全ACID，那么需要把值配置到1。


[参考来源](http://dev.mysql.com/doc/refman/5.1/en/innodb-parameters.html#sysvar_innodb_flush_log_at_trx_commit)

