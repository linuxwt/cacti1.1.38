# cacti1.1.38
# 此项目为docker部署cacti，基本架构为LNMP,nginx镜像与php镜像通过Dockerfile自动构建，mysql拉取的官方镜像
# 部署之后请进行cacti数据库初始化
mysql>create database cacti; 
mysql>use cacti; 
mysql>source /usr/local/Nginx/html/cacti/cacti.sql 
mysql>grant all on cacti.* to root; 
mysql>grant all on cacti.* to root@localhost; 
mysql>grant all on cacti.* to cactiuser; 
mysql>grant all on cacti.* to cactiusr@localhost identified by "密码" with grant option; 
mysql>flush privileges; 
vi /usr/local/Nginx/html/cacti/include/config.php 
设置数据库相关配置 
$database_type = 'mysql'; $database_default = 'cacti'; $database_hostname = 'localhost'; $database_username = 'cactiuser'; $database_password = '密码'; $database_port = '3306'; $database_ssl = false; 
