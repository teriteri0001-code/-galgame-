可跨越多跳建立连接
![[Pasted image 20240722154515.png]]
![[Pasted image 20240723151044.png]]
使用接口地址建立EBGP对等体
[ri]bgp 100
[r1-bgp]router-id 1.1.1.1
[r1-bgp]peer 12.0.0.2 as-number 200
[R2]bgp 200
[R2-bgp]router-id 2.2.2.2
[R2-bgp]peer 12.0.0.1 as-number 100
建立IBGP对等体
![[Pasted image 20240723151304.png]]
![[Pasted image 20240723151314.png]]
![[Pasted image 20240723151321.png]]
[R2]bgp 200
[R2-bgp]peer 3.3.3.3 as-number 200
peer 3.3.3.3 connect-interface LoopBack 0
[R3]bgp 200
[R3-bgp]router-id 3.3.3.3
[R3-bgp]peer 2.2.2.2 as-number 200
peer 2.2.2.2 connect-interface LoopBack 0
同理R3和R4
同理R2和R4
R2设置下一跳为本地 peer 4.4.4.4 next-hop-local，R4不用
使用本机环回地址建议EBGP
[R4]bgp 200
[r4-bgp]peer 5.5.5.5 as-number 300
[r4-bgp]peer 5.5.5.5 connect-interface LoopBack 0
ip route-static 5.5.5.5 32 45.0.0.5
peer 5.5.5.5 ebgp-max-hop 2
[r5]bgp 300
[r5-bgp]router-id 5.5.5 .5
[r5-bgp]peer 4.4.4.4 as-number 200
[r5-bgp]peer 4.4.4.4 connect-interface LoopBack 0
ip route-static 4.4.4.4 32 45.0.0.4
peer 5.5.5.5 ebgp-max-hop 2
始发路由器在给邻居传递BGP路由时，默认修改下一跳地址
从**外部邻居**哪里学来的路由，在传递给自己的**内部邻居时**，默认不修改下一跳地址
EBGP传路由的时候需要在边界路由处指定本地下一跳，