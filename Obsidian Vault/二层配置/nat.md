# 静态 ：
nat static global 100.1.1.3 （转换成公网的地址）inside 192.168.1.1 （原ip）
# NAPT
（也称为PAT，Port Address Translation）是一种更灵活的NAT形式，它允许多个内网主机共享一个公网IP地址，通过不同的端口号来区分不同的连接。配置NAPT通常涉及以下几个步骤：
acl number
rule permit ip source <内网IP地址范围>
nat address-group <地址组编号> <起始公网IP地址> <结束公网IP地址>//定义一个或多个公网IP地址作为地址池，供NAPT使用。
接口nat outbound <acl编号> address-group <地址组编号>


定义外网ip地址池
使用ACL策略指定内网IP
在连接外网的接口上使用转换

easy ip
接口有出口ip
nat outbound acl

![[61b667c70b53065cc4af960f3700f84.png]]

## 源NAT esay-ip

nterface GigabitEthernet1/0/0
 undo shutdown
 ip address 10.10.34.1 255.255.255.252
 service-manage ping permit     /端口可以被PING通
#
interface GigabitEthernet1/0/1
 undo shutdown
 ip address 200.1.1.2 255.255.255.252
 service-manage ping permit

接口加入安全组

firewall zone trust
 set priority 85
 add interface GigabitEthernet0/0/0
 add interface GigabitEthernet1/0/0
#
firewall zone untrust
 set priority 5
 add interface GigabitEthernet1/0/1

配置安全策略

security-policy
 rule name in_out
  source-zone trust
  destination-zone untrust
  service icmp
  action permit
 rule name local_trust     //本地能与trust和untrust区域相通
  source-zone local
  destination-zone trust
  action permit
 rule name local_untr       //本地能与trust和untrust区域相通
  source-zone local
  destination-zone untrust   
  service icmp
  action permit

配置到公网的静态路由

ip route-static 0.0.0.0 0.0.0.0 200.1.1.1


nat-policy
 rule name Easy_ip
  source-zone trust
  interface GigabitEthernet1/0/1
  source-address 192.168.2.0 mask 255.255.255.0
  source-address 192.168.3.0 mask 255.255.255.0
  source-address 192.168.4.0 mask 255.255.255.0
  source-address 192.168.5.0 mask 255.255.255.0
  action source-nat easy-ip

# 双向地址转换
# 配置IP地址

[USG6000V1]interface g1/0/0
[USG6000V1-GigabitEthernet1/0/0]ip address 1.1.1.1 24
[USG6000V1-GigabitEthernet1/0/0]q
[USG6000V1]interface g1/0/1
[USG6000V1-GigabitEthernet1/0/1]ip address 10.1.1.1 24

# 加入安全区域

[USG6000V1]firewall zone untrust 
[USG6000V1-zone-untrust]add interface g1/0/0

[USG6000V1]firewall zone dmz 
[USG6000V1-zone-dmz]add interface g1/0/1

# 配置安全策略

[USG6000V1]security-policy
[USG6000V1-policy-security]rule name untust_dmz
[USG6000V1-policy-security-rule-untust_dmz]source-zone untrust 
[USG6000V1-policy-security-rule-untust_dmz]destination-zone dmz
[USG6000V1-policy-security-rule-untust_dmz]destination-address 10.1.1.2 32
[USG6000V1-policy-security-rule-untust_dmz]action permit 

NAT Server

[USG6000V1]nat server global 1.1.1.3（指定端口 ：8080） inside 10.1.1.2（：80）

域内NAT

# 配置地址池

[USG6000V1]nat address-group 1
[USG6000V1-address-group-1]mode pat 
[USG6000V1-address-group-1]section 10.1.1.100 10.1.1.100

# 配置NAT策略，源NAT

[USG6000V1]nat-policy 
[USG6000V1-policy-nat]rule name policy1
[USG6000V1-policy-nat-rule-policy1]destination-address 10.1.1.2 32
[USG6000V1-policy-nat-rule-policy1]action source-nat address-group 1

验证结果
外网客户端client ping 内网服务器Server

查看Server-map表

[USG6000V1]display firewall server-map
2024-07-01 13:43:25.560 
 Current Total Server-map : 2
 Type: Nat Server,  ANY -> 1.1.1.3[10.1.1.2],  Zone:---,  protocol:---
 Vpn: public -> public

 Type: Nat Server Reverse,  10.1.1.2[1.1.1.3] -> ANY,  Zone:---,  protocol:---
 Vpn: public -> public,  counter: 1

查看会话表

[USG6000V1]display firewall session table 
2024-07-01 13:59:20.170 
 Current Total Sessions : 1
 icmp  VPN: public --> public  1.1.1.2:256[10.1.1.100:2048] --> 1.1.1.3:2048[10.1.1.2:2048]