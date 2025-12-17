# 中继的两种封装类型
![[Pasted image 20240715212432.png]]
![[Pasted image 20240715212452.png]]
发到access剥离tag,
发到trunk保留tag
# VTP，vcmp
![[Pasted image 20240715222717.png]]

vcmp:
[Huawei]vcmp role server
[Huawei]vcmp doamin domain1		//设备域名 对server进行设备域名指定
[Huawei]vcmp device-id server1	//设备ID
...//选择想加入同一vcmp管理域的交换机，指定域名，默认为client
[Huawei]int GE 1/0/1
[Huawei-GigabitEthernet1/0/1]
[Huawei-GigabitEthernet1/0/1]vcmp disable//选择与直连交换机相连的接口打开vcmp
GVRP:
![[Pasted image 20240716135818.png]]
改名后开启功能，设置端口并开启功能
![[Pasted image 20240716144154.png]]
fixed模式能接受normal模式交换机的动态，Fixed模式的动态会传播给normal
fixed模式不能修改normal传过来的动态，normal模式也不能修改fixed的动态
forbidden模式只传播vlan1，不管vlan1是不是默认vlan




vtp：
Switch(config)# vtp domain domain_name
Switch(config)# vtp mode { server | client | transparent }
Switch(config)# vtp password password
Switch(config)# vtp pruning//配置VTP修剪
Switch(config)# vtp version 2//配置VTP版本
若server配置口令，则其域内交换机也需配置口令才能学习和转发，且client会更新




ensp中，

trunk实现相同vlan的跨交换机通信
![[Pasted image 20240716101804.png]]
PC456同属vlan50
PC1vlan10
LSW1 GE5vlan50 GE1vlan10 GE10trunk
LSW2 trunk
PC 4-5-6, 5,6， 1-5不通，因为不同vlan也没配接口vlan
