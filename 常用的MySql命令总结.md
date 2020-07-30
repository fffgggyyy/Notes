### 1.建立外部连接

由于mysql的安全机制，默认不允许外部连接。可以user表中host进行更改以便支持外部连接。

```
use mysql;
update user set host = “%” where user = “root”; 
flush privileges;
```

### 2.创建数据库并制定字符集

首先指出utf8和utf8mb4的区别。MySQL在5.5.3之后增bai加了这个utf8mb4的du编码，mb4就是zhimost bytes 4的意思，专门用来兼容四字节的unicode。好在daoutf8mb4是utf8的超集，除了将编码改为utf8mb4外不需要做其他转换。

```
 CREATE database testdb DEFAULT CHARACTER SET utf8mb4;
```

