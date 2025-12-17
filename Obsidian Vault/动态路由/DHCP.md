# 接口型
接口地址池就是给端口配置IP然后这个IP作为网关使用，这个端口下面的PC都可以DHCP分到IP地址。
![[Pasted image 20240729101034.png]]
开局自爆我要启动dhcp
进入到接口分配IP
dhcp模式选择接口
分配DNS服务器
可以选择不自动分配哪些范围的ip（excluded）
# 全局模式
老样子开局自爆
先给接口分配网关
选择dhcp模式
在sys界面里创建ip池，ip pool name
network 接口处的网段 mask 子网掩码
gateway-list 接口的ip，配置dhcp客户端的网关地址
dns-list 114
可选excluded
# 中继模式
用于dhcp服务器的ip不和下面终端Ip同网段，所以可能是最常用的情况
![[Pasted image 20240729112829.png]]

首先AR1为主服务器，接口0的ip为3.3.3.1，开局自爆dhcp，模式选择global,为下面会用到的ip创建池子，一个池子对应一个网段，gateway网关为中继器上的vlanip，network网段

LSW3为三层交换机中继器，g2口做一个pla实现通路，1口做plv，自身开局爆
dhcp，创建vlan,一般vlan对应池子，进入vlanif界面为vlan添加那块区域的网关，然后为每个vlan选择dhcp模式relay,dhcp relay sever ip 为AR1的0口

LS1就正常做vlan和trunk、access口

ospf实现通信，lsw3通告上行1个和下行的两个vlanip，ar1通告下行的接口ip





