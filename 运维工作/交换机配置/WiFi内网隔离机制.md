---
created: '2026-08-26T15:54:26.542893'
tags:
- wifi
- aruba
- 网络
- 隔离
---

# WiFi 内网隔离机制

> **发现时间：** 2026-08-26
> **状态：** 已验证

---

## 核心发现

**WiFi 内网隔离不在防火墙，不在核心交换机 ACL，而在 Aruba AC 的 Role-Based Access Control。**

---

## 机制详解

### Aruba 角色系统

| 角色 | ACL | 效果 |
|------|-----|------|
| logon | 内置受限 | 只允许 DHCP/DNS/HTTP 到控制器，不能访问内网 |
| guest | ACL 为空 | 默认放行所有流量，能访问内网 |
| authenticated | allowall | 全部放行（RADIUS 下发） |

### SSID 角色分配

| SSID | initial-role | 认证方式 | 实际角色 | 能否访问内网 |
|------|-------------|----------|----------|-------------|
| BTBX | logon | 802.1X (RADIUS) | guest（RADIUS下发） | 能 |
| WMS | guest | PSK | guest | 能 |
| WCS | guest | PSK | guest | 能 |
| BTB | logon | PSK | logon | 不能 |
| WAP | logon | PSK | logon | 不能 |

### 关键结论

- **PSK 的 SSID：** initial-role 决定最终角色
- **802.1X 的 SSID：** RADIUS 服务器可以下发角色，覆盖 initial-role

---

## 内网访问的两个条件

1. **Aruba AAA Profile 的 initial-role = guest**（或 RADIUS 下发 guest）
2. **VLAN 需要有 DHCP helper 指向内网 DNS 服务器**

缺少任何一个都不行：
- 只有 guest role 没有 DHCP helper → DNS 解析到公网 IP，内网域名打不开
- 有 DHCP helper 但 role 是 logon → Aruba 层面拦截流量

---

## 修改方法

### 方法一：改 initial-role（简单）

```
SSH 登录 Aruba AC (10.12.20.50)
configure terminal
aaa profile <profile名>
initial-role guest
exit
exit
write memory
```

### 方法二：走 RADIUS（灵活）

在 RADIUS 服务器 (10.11.10.51) 上配置用户的角色下发。

---

## 排查过程

1. 最初怀疑核心交换机 ACL → 查了 VLAN 45 没有 ACL
2. 登录 Aruba AC 查 SSID 配置和用户角色
3. 发现 logon vs guest 角色差异
4. 把 BTB 改成 guest → 用户拿到 guest role → 但还是不能访问 TMS
5. 发现 VLAN 45 没有 DHCP helper → DNS 是公网 → 域名解析失败
6. 用 WCS 测试（已有 guest role + 内网 DNS）→ 成功

---

## 相关设备

| 设备 | IP | 型号 |
|------|-----|------|
| Aruba AC | 10.12.20.50 | Aruba (WAPRO) |
| 核心交换机 | 10.12.20.1 | Cisco Catalyst 4500 |
| RADIUS | 10.11.10.51 | - |

---

## 参考

- [[AP无线网络拓扑汇总]]
- [[SW01_核心_配置整理]]
