huawei:
stp enable
display stp brief
![[Pasted image 20240716160833.png]]
选举方式通过优先级，网桥ID，cost


在生成树协议（STP）方面，Cisco和华为都遵循了IEEE的标准，但它们在实现和扩展上可能有所不同。针对你的问题，我们可以从以下几个方面进行解答：

# BPDU
BPDU的主要作用是帮助网络中的桥接设备发现网络拓扑结构，避免环路，并计算生成树以确保网络的可靠性和性能
### BPDU中的关键参数

- **协议ID**：总是为0，标识这是一个BPDU数据包。
- **版本**：STP的版本（如IEEE 802.1D版本是0）。
- **消息类型**：标识BPDU的类型（如配置BPDU=0，TCN BPDU=80）。
- **标志**：包含一些控制位，如TC标志和TCA标志。
- **根ID**：根网桥的网桥ID。
- **路径开销**：到达根网桥的STP开销。
- **网桥ID**：发送该BPDU的交换机的ID。
- **端口ID**：发送该BPDU的端口的ID。
- **消息寿命**：从根网桥发出BPDU之后的秒数，每经过一个网桥都递减1，表示到根网桥的跳数。
- **Hello时间**：根网桥连续发送BPDU之间的时间间隔。
- **最大寿命**：网桥在将根网桥看做不可用之前保留根网桥ID的最大时间。
- **转发延迟**：网桥在监听和学习状态所停留的时间间隔
- ![[Pasted image 20240717103806.png]]
- 
- 

### 一、Cisco的PVST

Cisco的PVST（Per-VLAN Spanning Tree）是Cisco专有的STP实现，它允许在每个VLAN上独立运行STP实例。这种机制提高了网络的灵活性和可扩展性，因为每个VLAN都可以有自己的生成树，从而减少了广播流量和潜在的环路问题。PVST是Cisco在标准STP（IEEE 802.1D）基础上进行的一种扩展。

### 二、华为的STP实现

华为交换机在STP方面的实现主要遵循IEEE 802.1D标准，但华为也提供了更先进的STP扩展协议，如RSTP（Rapid Spanning Tree Protocol，快速生成树协议）和MSTP（Multiple Spanning Tree Protocol，多生成树协议）。

1. **STP（IEEE 802.1D）**：
    - 华为交换机支持基本的STP协议，用于在包含冗余链路的网络中避免环路的形成，同时保持网络的冗余特性。
    - 默认情况下，华为交换机的STP优先级是32768，可以通过手动调整优先级来影响根桥的选举。
2. **RSTP（IEEE 802.1w）**：
    - RSTP是STP的快速版本，提供了更快的收敛时间和更少的网络带宽占用。
    - 华为交换机支持RSTP，可以在需要更快故障恢复时间的网络环境中使用。
3. **MSTP（IEEE 802.1s）**：
    - MSTP进一步扩展了STP的功能，允许在多个VLAN之间共享生成树实例，从而减少了生成树的数量和所需的资源。
    - 华为交换机也支持MSTP，适用于需要高可靠性和灵活性的大型网络。

### 三、总结

对于你的问题“Cisco是PVST，华为是什么”，可以回答如下：

- 华为交换机在STP方面主要遵循IEEE 802.1D标准，但提供了更先进的STP扩展协议，如RSTP和MSTP。
- 华为交换机默认可能使用STP（取决于具体型号和配置），但也可以配置为使用RSTP或MSTP以满足不同的网络需求。
- 与Cisco的PVST不同，华为没有专门的“PVST”模式，但MSTP可以看作是跨VLAN的STP实例共享机制，提供了类似PVST的灵活性和可扩展性。
请注意，以上信息可能因华为交换机的具体型号、软件版本和配置而异。在实际应用中，建议参考华为交换机的官方文档或联系华为技术支持以获取最准确的信息。


# RSTP
端口状态的重新划分

RSTP 的状态规范把原来的 5 种状态缩减为 3 种。根据端口是否转发用户流量和学习 MAC 地

址来划分:

 如果不转发用户流量也不学习 MAC 地址，那么端口状态就是 Discarding 状态。

 如果不转发用户流量但是学习 MAC 地址，那么端口状态就是 Learning 状态。

 如果既转发用户流量又学习 MAC 地址，那么端口状态就是 Forwarding 状态。

### RSTP中的端口角色

RSTP定义了四种端口角色，分别是：

1. **根端口（Root Port）**：连接根桥（即网络中选定的根交换机）的端口。
2. **指定端口（Designated Port）**：在生成树中，负责向特定网段转发帧的端口。
3. **Alternate端口（Alternate Port）**：根端口的备份端口，当根端口失效时，Alternate端口会切换为根端口。（因为根桥上的根端口被down掉了叫做alter）
4. **备份端口（Backup Port）**：指定端口的备份端口，当指定端口失效时，备份端口会切换为指定端口。（普通桥端口被down掉后的名字）

端口和状态没有强制绑定的状态，某些时候root和des状态也是discard
1. 通常有以下场景：
    
    1. 设备收到了自己发出的BPDU报文。
    2. 启用根保护功能，收到了更优的BPDU报文。
    3. 启用环路保护功能，端口在指定时间内收不到BPDU报文。
    
    可通过命令**display stp brief**的Protection字段或**display stp abnormal-interface**的Reason字段查看异常端口保护的类型。<H3C> **display stp abnormal-interface**  
    MSTID Interface Status Reason 0 10GE1/0/1 discarding root-protected 0 10GE1/0/3 discarding root-protected 0 10GE1/0/4 discarding loop-detected
    
    上述回显信息中的“Reason”字段标识端口异常的生成树保护类型：
    
    - root-protected：根保护生效。
    - loop-protected：环路保护生效。
    - loop-detected：环路检测生效（收到了自己发出的BPDU报文）。
    
    - 如果端口不是被生成树协议保护导致的异常阻塞，检查端口下是否配置了**bpdu bridge enable**，否则对于收到的STP报文不处理。<H3C> **system-view**  
        [H3C] **interface 10ge 1/0/1**  
        [H3C-10GE1/0/1] **bpdu bridge enable**  
        [H3C-10GE1/0/1] **commit**  
        
    - 如果端口被生成树协议保护导致的阻塞，需要根据不同情况做相应排查和处理。
        
        1. 如果端口由于环路检测（收到自己发出的BPDU）被阻塞，需要排查接口下的环路。
        2. 如果端口由于根保护被阻塞，说明从对端接收到的BPDU报文优先级更高，可通过诊断命令**debugging stp interface** _interface-type interface-number_ **packet all**采集详细的交互报文，排查端口接收报文中的CIST Root Identifier、CIST Bridge Identifier字段的桥MAC对应的设备，修改其优先级。<H3C> **debugging stp interface 10ge 1/0/3 packet all**  
            <H3C> **terminal monitor**  
            <H3C> **terminal debugging**  
            
            采集完信息后请尽快关闭Debug开关。<H3C> **undo debugging all**  
            <H3C> **undo terminal monitor**  
            <H3C> **undo terminal debugging**
            
        3. 如果端口由于环路保护被阻塞，说明本端没有收到对方的BPDU报文，可通过**debugging stp interface** _interface-type interface-number_ **packet all**诊断命令采集详细的交互报文，如未收到STP报文则需要向上排查网络问题。
        
         说明：
        
        ROOT保护（根保护）：使能根保护功能的指定端口收到优先级更高的BPDU报文时，端口状态将进入Discarding状态。
        
        环路保护：使能环路保护功能后，如果根端口或Alternate端口长时间收不到来自上游设备的BPDU报文时，根端口会进入Discarding状态，角色切换为指定端口；而Alternate端口则会一直保持在阻塞状态（角色也会切换为指定端口），从而不会在网络中形成环路。
        
2. 端口自动Error-Down
    
    当配置BPDU保护功能后，如果边缘端口收到BPDU报文，该边缘端口将会被Error-Down。可以通过命令**display error-down recovery**查看处于Error-Down状态的接口相关信息。
    
    如果该接口下连接的设备运行了生成树协议，则需要取消接口的边缘端口配置，再执行**shutdown**和**undo shutdown**重启接口。
