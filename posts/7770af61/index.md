# Centos7安装mysql 5.7


## 开放3306端口

```
firewall-cmd --state
firewall-cmd --zone=public --add-port=3306/tcp --permanent
firewall-cmd --reload
```

## 下载mysql官方repo源并安装

```
wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm

yum localinstall mysql57-community-release-el7-8.noarch.rpm -y
```

## 导入mysql repo源的GPG-KEY

```
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
```

## 安装mysql

```
yum install mysql-community-server -y
```

## 修改`/etc/my.cnf`

修改默认字符编码、数据库引擎并禁用密码复杂度验证

添加下面内容：

```
[mysql]
default-character-set=utf8
 
[mysqld]
default-storage-engine=INNODB
character_set_server=utf8

validate_password = off
```

重启mysql

## 获取root临时密码

```
 grep 'temporary password' /var/log/mysqld.log
```

## 登录mysql

```
mysql -uroot -p
```

使用临时密码登录后，不能进行其他的操作，否则会报错

## 修改root密码

```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'yourpassword';
```

退出mysql并使用新密码重新登录

## 授权其它机器远程登录

```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'yourpassword' WITH GRANT OPTION;
 
FLUSH PRIVILEGES;
```

## 开机自启动

```
systemctl enable mysqld
systemctl daemon-reload
```

## 其它设置

### 忽略大小写

mysql在Windows上lower_case_table_names默认值为1（不敏感），在macOS上默认值为2（不敏感）。在Linux上不支持值2，服务器强制该值为0（敏感）。

MySQL官方也提示说：如果在数据目录驻留在不区分大小写的文件系统（例如Windows或macOS）上，则不应将lower_case_table_names设置为0，否则将出现MySQL服务无法启动的问题。

由于操作系统不同导致大小写敏感的默认设置不一致，因此在开发时一定要养成严格的意识，SQL语句**一律采用小写字母**，避免无意义的踩坑。

登陆mysql查看：

```

mysql> show variables like "%case%";
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_file_system | OFF   |
| lower_case_table_names | 0     |  ##0区分 1 不区分
+------------------------+-------+
2 rows in set (0.00 sec)

```

修改配置文件 /etc/my.cnf 添加：

```
# 0：区分大小写，1：不区分大小写

lower_case_table_names =1

```

重启后生效：

```
systemctl restart mysqld

```

