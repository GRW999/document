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