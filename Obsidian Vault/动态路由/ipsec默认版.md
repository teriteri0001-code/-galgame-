[VR1]acl 3000
[VR1-acl-adv-3000]rule 10 permit ip source 192.168.10.0 0.0.0.255 destination 192.168.20.0 0.0.0.255
[VR1-acl-adv-3000]rule 100 deny ip 
[VR1-acl-adv-3000]quit

配置IPSec提议
[VR1]ipsec proposal ipsecproposalname    //test为提议名称，用于绑定ipsec安全策略
![[Pasted image 20240816141027.png]]
配置IKE提议
[VR1]ike proposal num    //创建IKE提议，编号为1
[VR1-ike-proposal-1]dh group2    //配置dh组长度为2（1024bit）
![[Pasted image 20240816141324.png]]

//配置IKE对等体（预共享密钥、对端地址等）
[VR1]ike peer ikepeername v2        //创建IKE对等体，版本为v2，名称为test（用于关联IPSec安全策略）
[VR1-ike-peer-test]pre-shared-key cipher admin       //配置加密的预共享密钥为
[VR1-ike-peer-test]remote-address ip    //配置vpn对端地址为23.1.1.2
 
//配置IPSec安全策略（关联IKE对等体、IPSec提议、感兴趣流等）
[VR1]ipsec policy policyname num isakmp    //IPSec安全策略，名称为R1-R3-IPSecVPN，编号为1，采用ike自动协商
[VR1-ipsec-policy-isakmp-R1-R3-IPSecVPN-1]ike-peer ikepeername    //关联IKE对等体
[VR1-ipsec-policy-isakmp-R1-R3-IPSecVPN-1]proposal ipsecproposalname    //关联IPSec提议
[VR1-ipsec-policy-isakmp-R1-R3-IPSecVPN-1]security acl 3000    //配置IPSec感兴趣流
 
//接口应用IPSec安全策略
[VR1]int g0/0/0
[VR1-GigabitEthernet0/0/0]ipsec policyname