# MySQL

## MySQL 物理结构

### 查看数据库数据目录

> show variables like 'datadir';

### MySQL innodb 存储引擎数据文件

当开启 `innodb_file_per_table=on` 时，每个表一个文件。物理表文件有：

- `<tableName>.frm`: 表结构文件，存储表结构元信息, 参考: https://dev.mysql.com/doc/internals/en/frm-file-format.html
- `<tableName>.idb`: 独占表空间文件, 存储表数据。
- `ibdata1`: 共享表空间(Tablespace)文件。


## 参考资料

1. [MySQL References](https://dev.mysql.com/doc/refman/8.0/en/innodb-table-import.html)
2. [MySQL internals](https://dev.mysql.com/doc/internals/en/)
