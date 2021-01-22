# mysql部署


## docker部署
### my.cnf设置
* vi my.cnf
``` 
[client]
default-character-set=utf8mb4

[mysql]
default-character-set=utf8mb4

[mysqld]
character-set-server=utf8mb4
max_connections=1000
```
### 部署mysql5.7
* docker pull mysql:5.7 
* docker run --restart always --name mysql -p 3306:3306 -v /etc/mysql/my.cnf:/etc/mysql/my.cnf -v /opt/data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7 （引用外部my.cnf文件设置编码）   
* docker run --restart always --name mysql -p 3306:3306 -v /opt/data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci （直接设置编码）

### 部署mysql8.0
* docker pull mysql
* docker run --restart always --name mysql -p 3306:3306 -v /etc/mysql/my.cnf:/etc/mysql/my.cnf -v /opt/data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql （引用外部my.cnf文件设置编码）   
* docker run --restart always --name mysql -p 3306:3306 -v /opt/data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci （直接设置编码）

## centos部署

### 部署mysql8.0

* wget -i -c https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
* yum -y install mysql80-community-release-el7-3.noarch.rpm
* yum -y install mysql-community-server
* vi /etc/my.cnf
``` 
[client]
default-character-set=utf8mb4

[mysql]
default-character-set=utf8mb4

[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock

log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid

character-set-server=utf8mb4
max_connections=2000

```
* systemctl start mysqld.service
* systemctl enable mysqld.service
* grep "A temporary password" /var/log/mysqld.log
* mysql -uroot -p
* ALTER USER 'root'@'localhost' IDENTIFIED BY 'Aa123456@';  (密码规则：大小写字母+数据+符号)
* use mysql;
* update user set host='%' where user='root';
* FLUSH PRIVILEGES;


### centos 系统时间改成cst（国内）
date查看

在CentOS Linux中终端命令中执行以下命令：

1).vi /etc/sysconfig/clock   #编辑时间配置文件

    ZONE="Asia/Shanghai"

    UTC=false                   #设置为false，硬件时钟不于utc时间一致

    ARC=false

2).ln -sf /usr/share/zoneinfo/Asia/Shanghai    /etc/localtime    #linux的时区设置为上海时区

3).ntpdate cn.pool.ntp.org    #对准时间

4)./sbin/hwclock --systohc   #设置硬件时间和系统时间一致并校准
