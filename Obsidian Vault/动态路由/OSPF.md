在不考虑区域划分的情况下，OSPF 协议的路由计算过程可简单描述如下：
	每个支持 OSPF 协议的路由器都维护着一份描述整个自治系统拓扑结构的链
路状态数据库 LSDB（Link State Database）。每台路由器根据自己周围的网
络拓扑结构生成链路状态广播 LSA（Link State Advertisement），通过相互
之间发送协议报文将 LSA 发送给网络中其它路由器。这样每台路由器都收到
了其它路由器的 LSA，所有的 LSA 放在一起便组成了链路状态数据库。
	由于 LSA 是对路由器周围网络拓扑结构的描述，那么 LSDB 则是对整个网络
的拓扑结构的描述。路由器很容易将 LSDB 转换成一张带权的有向图，这张图
便是对整个网络拓扑结构的真实反映。显然，各个路由器得到的是一张完全相
同的图。
	每台路由器都使用 SPF 算法计算出一棵以自己为根的最短路径树，这棵树给
出了到自治系统中各节点的路由，外部路由信息为叶子节点，外部路由可由广
播它的路由器进行标记以记录关于自治系统的额外信息。显然，各个路由器各
自得到的路由表是不同的。
![[Pasted image 20240722113515.png]]
![[Pasted image 20240722131650.png]]
# 五种包
hello：建议邻居
DBD：确定主从
ACK：应答主从
LSR：更新路由的请求
LSU：发送需更新的路由
# 五种状态
init:初始发送hellow包状态
two-way：进入邻居状态
exstart:协商主从状态
exchange：交换大量DBD和ACK更新链路状态
Full：完成LSDB同步状态

![[Pasted image 20240722113430.png]]
ospf 进程号 
area num
network网段+反掩码

# 多区域
末梢区域
在区域内开启stub，ABR也开启，如图给area1实现末梢
![[Pasted image 20240724163614.png]]
![[Pasted image 20240724163646.png]]
dis r1 ip rou table
完全末梢 现进入AR2开启stub no-summary
老样子dis r1 ip rou table
![[Pasted image 20240724163756.png]]


- **末梢区域**：禁止来自Area 0的Type-4（AS External）和Type-5（ASBR Summary）LSA（链路状态通告）进入。这意味着末梢区域内的路由器不会学习到自治系统外部的路由信息。
- **完全末梢区域**：除了禁止Type-4和Type-5 LSA外，还进一步禁止Type-3（Inter-Area）LSA（除了描述默认路由的Type-3 LSA外）进入。这意味着完全末梢区域内的路由器不仅不会学习到自治系统外部的路由信息，而且除了本区域内的路由信息外，其他区域的路由信息都会被聚合为默认路由。
- AS=公司 区域=部门 公司里当然有外交部门那个人就是ASBR和负责对接的人组成一个外交区域
源之结论，完全末梢就是我的领导只有一个杰哥，同事也有很多，我不知道公司和外面业务是什么，也不知道其他部门怎么样，但我不需要其他人沟通，屁大点事全找杰哥中转
我在一棵树的最低端，通过一根连到杰哥 
# LSA
- - **1类LSA（Router LSA）**：由每个OSPF路由器产生，描述了对应设备的物理接口所连接的链路和接口，并指明了各链路的状态开销等参数。
- **2类LSA（Network LSA）**：由DR（指定路由器）产生，用于描述广播型或NBMA（非广播多址）网络中的链路状态信息。
- **3类LSA（Summary LSA）**：由ABR（区域边界路由器）产生，用于描述区域间的路由信息。
- **4类LSA（ASBR Summary LSA）**：也由ABR产生，用于描述如何到达ASBR（自治系统边界路由器）的路由信息。
- **5类LSA（AS External LSA）**：由ASBR产生，用于描述自治系统外部的路由信息。
- **7类LSA（NSSA External LSA）**：在NSSA（Not-So-Stubby Area）区域中使用，与5类LSA类似，但用于描述NSSA区域外部的路由信息。
# 重分发
![[Pasted image 20240724170801.png]]
