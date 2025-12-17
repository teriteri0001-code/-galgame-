//配置感兴趣流（需要通过IPSec加密的数据）
[VR1]acl 3000
[VR1-acl-adv-3000]rule 10 permit ip source 192.168.10.0 0.0.0.255 destination 192.168.20.0 0.0.0.255
[VR1-acl-adv-3000]rule 100 deny ip 
[VR1-acl-adv-3000]quit
 
//配置IPSec提议（工作模式、协议[ah/esp]、认证算法、加密算法[esp下]）
[VR1]ipsec proposal test    //test为提议名称，用于绑定ipsec安全策略
[VR1-ipsec-proposal-test]encapsulation-mode tunnel    //配置工作模式为隧道（默认为隧道）
[VR1-ipsec-proposal-test]transform esp    //配置协议为esp（可选ah、esp、ah-esp，默认为esp）
[VR1-ipsec-proposal-test]esp authentication-algorithm sha1    //配置认证算法为sha1
[VR1-ipsec-proposal-test]esp encryption-algorithm 3des    //配置加密算法为3des
 
//配置IKE提议（认证方式、认证算法、加密算法、DH组长度、SA密钥生存时间等）
[VR1]ike proposal 1    //创建IKE提议，编号为1
[VR1-ike-proposal-1]authentication-method pre-share    //认证方式为预共享密钥
[VR1-ike-proposal-1]authentication-algorithm md5    //认证算法为md5
[VR1-ike-proposal-1]dh group2    //配置dh组长度为2（1024bit）
[VR1-ike-proposal-1]sa duration 86400    //配置密钥生存时间为86400秒（默认值）可配可不配
 
//配置IKE对等体（预共享密钥、对端地址等）
[VR1]ike peer test v2        //创建IKE对等体，版本为v2，名称为test（用于关联IPSec安全策略）
[VR1-ike-peer-test]pre-shared-key cipher gdeie       //配置加密的预共享密钥为gdeie
[VR1-ike-peer-test]remote-address 23.1.1.2    //配置vpn对端地址为23.1.1.2
 
//配置IPSec安全策略（关联IKE对等体、IPSec提议、感兴趣流等）
[VR1]ipsec policy R1-R3-IPSecVPN 1 isakmp    //IPSec安全策略，名称为R1-R3-IPSecVPN，编号为1，采用ike自动协商
[VR1-ipsec-policy-isakmp-R1-R3-IPSecVPN-1]ike-peer test    //关联IKE对等体
[VR1-ipsec-policy-isakmp-R1-R3-IPSecVPN-1]proposal test    //关联IPSec提议
[VR1-ipsec-policy-isakmp-R1-R3-IPSecVPN-1]security acl 3000    //配置IPSec感兴趣流
 
//接口应用IPSec安全策略
[VR1]int g0/0/0
[VR1-GigabitEthernet0/0/0]ipsec policy R1-R3-IPSecVPN
 
 