- 同一MSTP域设备的特点
    - 都启动MSTP
    - 具有相同的域名
    - 具有相同的VLAN到生成树实例映射配置
    - 具有相同的MSTP修订级别配置
# 配置命令及功能
stp region-configuration\\进入 MST 域视图
region-name x \\设mst域名
instance instance-id vlan vlan-list\\配置 VLAN 映射表
revision-level x\\配置 MST 域的 MSTP 修订级别
只有两台交换机上配置的 MST 域的域名相同、MST 域内配置的所有生成树实例对应的 VLAN 映射表完全相同、MST 域的修订级别相同，这两台交换机才属于同一个 MST 域
在满足下列条件之一的情况下，这些域的配置才会真正的生效：
用户使用命令 active region-configuration 手工激活配置的 MST 域相关参数
用户使用命令 stp enable 使能 MSTP
vlan-mapping modulo x\\快速地将所有的 VLAN 列表根据求模运算均匀的映射到
指定的生成树实例上
check region-configuration\\显示用户下发的最新域配置信息，不论其是否已经激活
![[Pasted image 20240720114137.png]]
指定为根桥或备份根桥
![[Pasted image 20240720114307.png]]
用户可以将当前交换机指定为生成树实例（由参数 instance instance-id 确定）的
根桥或备份根桥。如果 instance-id 取值为 0，当前交换机将被指定为 CIST 的根桥
或备份根桥。
<sys>stp mode mstp
stp priority x 配置指定交换机的 Bridge 优先级
stp max-hops x 配置 MST 域的最大跳数(20)
stp timer forward-delay\hello\max-age 配置交换机的时间参数
stp timer-factor x配置特定交换机的超时时间因子
stp pathcost-standard { dot1d-1998 |dot1t | legacy } 设定交换机在计算与交换机相连的链路的缺省 Path Cost 时采用的标准
stp mcheck  假设在一个交换网络中，运行 MSTP 的交换机的端口连接着运行 STP 的交换机，该端口会自动迁移到 STP 兼容模式下工作；但是此时如果运行 STP 协议的交换机
被拆离，该端口不能自动迁移到 MSTP 模式下运行，仍然会工作在 STP 兼容模式下。此时可以通过执行 mCheck 操作迫使其迁移到 MSTP 模式下运行。
stp bpdu-protection 配置交换机的 BPDU 保护功能
<interface>stp transmit-limit x 配置端口的最大发送速率
stp edged-port en\dis 配置端口为边缘端口或者非边缘端口
stp cost x 配置端口的 Path Cost
stp port priority x 端口优先级（128）
stp point-to-point  force-true\force-false\auto 配置端口是否与点对点链路相连
stp root-protection 
stp loop-protection
缺省情况下，交换机只启动防止 TC-BPDU 报文攻击的保护功能，不启动 BPDU
保护功能、Root 保护功能和环路保护功能。
交换机上启动了 BPDU 保护功能以后，如果边缘端口收到了配置消息，系统就将
这些端口关闭，同时通知网管这些端口被 MSTP 关闭。被关闭的端口只能由网络
管理人员恢复。
对于设置了 Root 保护功能的端口，其在所有实例上的端口角色只能保持为指定端
口。一旦这种端口上收到了优先级高的配置消息，即其将被选择为非指定端口时，
这些端口的状态将被设置为侦听状态，不再转发报文（相当于将此端口相连的链路
断开）。当在足够长的时间内没有收到更优的配置消息时，端口会恢复原来的正常
状态。
在对一个端口进行配置的时候，在 Loop 保护功能，Root 保护功能或者边缘端口设
置三个配置中，同一时刻只能有一个配置生效。 
stp non-flooding 缺省情况下，设备在未使能 STP 的端口上广播 BPDU 报文
stp config-digest-snooping 交换机实现了和其他厂商交换机在MSTP 域内互通
stp enable


stp mode mstp
stp re区域
stp rename区域名字
ins 1 vlan 10
ins 2 vlan 20
act re区域
q
stp enable

