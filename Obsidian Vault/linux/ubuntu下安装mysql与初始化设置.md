22.04版本下先sudo apt update更新
sudo apt install mysql-server
service mysql status检查是否开启
sudo mysql_secure_installation 设置安全策略

密码设置
![[Pasted image 20240823090915.png]]
密码强度尽量选1和2
![[Pasted image 20240823090940.png]]

移除匿名用户 Y
禁止root远程登录（无论选什么实际都是禁止）
移除测试数据库Y
![[Pasted image 20240823091309.png]]

使用默认的用户和密码或者忘记登录解决方法
![[Pasted image 20240823101211.png]]
使用这里默认的账号密码登录（如果不是ubuntu或debian系统无法使用）

sudo mysql -u root或sudo mysql
修改root密码：
USE mysql;
UPDATE user SET authentication_string=PASSWORD('hcwl.com') WHERE User='root';
FLUSH PRIVILEGES;

mysql默认下库存储目录在 /var/lib/mysql
      配置文件： /etc/my.cnf