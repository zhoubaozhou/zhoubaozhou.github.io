# mysql数据备份方案

目前mysql主要有如下三种数据备份方案：

1. Hot Backup（热备，也称为Online Backup）
2. Cold Backup (冷备)
3. Warm Backup (温备)


按照备份的内容，可以分为：

1. 逻辑备份
2. 裸文件备份

## 逻辑备份

mysqldump

## 裸文件备份

直接拷贝mysql各个数据文件；或者是数据库运行中的复制，例如ibbackup, xtrabackup这类工具。

按照备份内容来区分，可以分为：

1. 完全备份
2. 增量备份
3. 日志备份

## 冷备

对于Innodb的冷备，方案很简单，直接备份MySQL的数据库frm文件，共享表空间文件，独立表空间文件(*.ibd)，重做日志文件。

另外建议定期备份MySQL的数据库配置文件my.cnf。

