标准ACL，离源越远越好
扩展ACL，离源越近越好
acl 2000(num越小越优先)
acl name blyat（豪堪）
rule 66 perimt/deny source ip des ip
rule deny tcp destination 12.1.1.2 0 destination-port eq 23 source 172.16.1.200 0
1. 意思是拒绝目的TCP端口为23号端口的数据包，（telnet使用的是//tcp23号端口），作用即如果有数据包是访问某IP的telnet端口的，则会被拒绝掉。
int 接口
traffic-filter 入或出 acl num
先小范围的再大范围的
比如说先允许PC1进入再拒绝PC2进入

blyat
