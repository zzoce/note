在Ubuntu中，MySQL默认有一个导入文件的大小限制（通常为2MB）。你可以通过修改MySQL的配置文件来增加这个限制。具体操作如下：

### 步骤1：找到MySQL配置文件

MySQL的配置文件通常位于 `/etc/mysql/` 目录下，文件名为 `my.cnf` 或 `mysqld.cnf`。

```ubuntu
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

### 步骤2：修改配置文件

在配置文件中添加或修改以下参数：

```ubuntu
max_allowed_packet = 64M
```

`max_allowed_packet` 设置的是MySQL单个包的最大大小。你可以将其设置为合适的值，比如64M或更大。

如果你找不到 `max_allowed_packet` 参数，可以直接在 `[mysqld]` 部分添加。

另外，还有一个参数 `innodb_log_file_size` 也可能影响大文件的导入。你可以根据需要修改这个参数。

```ubuntu
innodb_log_file_size = 256M
```

### 步骤3：重启MySQL服务

修改配置文件后，需要重启MySQL服务使更改生效。

```ubuntu
sudo systemctl restart mysql
```

### 步骤4：验证更改

你可以登录到MySQL并检查设置是否生效。

```sql
SHOW VARIABLES LIKE 'max_allowed_packet';
SHOW VARIABLES LIKE 'innodb_log_file_size';
```
确保输出结果显示你设置的值。


### 通过命令行导入数据库

调整完配置后，可以通过命令行导入大的SQL文件：

```sql
mysql -u root -p emp < /path/to/employees.sql
```
这样，你就可以成功导入更大的SQL文件到MySQL数据库中。