首先更改网卡实现通网
ens33（网卡号）
![[Pasted image 20240826105047.png]]

![[Pasted image 20240826105137.png]]
nginx官网下载最新的ngnix tar包
tar zxvf 包
更新镜像源
./执行解压包内的configure
根据报错安装依赖
![[Pasted image 20240826105322.png]]
在 /usr/local/nginx/ 启动nginx
![[Pasted image 20240826110858.png]]
关闭防火墙
![[Pasted image 20240826111122.png]]
![[Pasted image 20240826111136.png]]
![[Pasted image 20240826111228.png]]
![[Pasted image 20240826111458.png]]
##### 创建开机自启的nginx服务
vi /usr/lib/systemd/system/nginx.service
[Unit]
Description=nginx - web server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/usr/local/nginx/logs/nginx.pid
ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s -stop
ExecQuit=/usr/local/nginx/sbin/nginx -s quit
PrivateTmp=true


[Install]
WantedBy=multi-user.target


重新加载系统服务
systemctl daemon-reload
启动服务
systemctl start nginx.service
设置开机
systemctl enable nginx.service
![[Pasted image 20240826120320.png]]

