# PVE (Proxmox VE) 换源

## 适用场景
PVE 系统 `apt-get update` 报错，通常是默认源在国外访问不了。

## PVE 版本与 Debian 对应关系
| PVE 版本 | Debian 版本 | 代号 |
|----------|-------------|------|
| PVE 8.x  | Debian 12   | bookworm |
| PVE 9.x  | Debian 13   | trixie |

> ⚠️ 常见坑：源文件里写错 Debian 版本（比如 bookworm 系统配了 trixie 源），会导致更新失败。

## 换源步骤

### 1. 备份原有源
```bash
mkdir -p /etc/apt/sources.list.d.bak.$(date +%Y%m%d)
cp /etc/apt/sources.list /etc/apt/sources.list.d.bak.$(date +%Y%m%d)/
cp /etc/apt/sources.list.d/*.list /etc/apt/sources.list.d.bak.$(date +%Y%m%d)/ 2>/dev/null
cp /etc/apt/sources.list.d/*.sources /etc/apt/sources.list.d.bak.$(date +%Y%m%d)/ 2>/dev/null
```

### 2. 清空 sources.list（由 debian.sources 统管）
```bash
> /etc/apt/sources.list
```

### 3. 配置 debian.sources（清华源）
写入 `/etc/apt/sources.list.d/debian.sources`：
```
Types: deb
URIs: https://mirrors.tuna.tsinghua.edu.cn/debian
Suites: bookworm bookworm-updates bookworm-backports
Components: main contrib non-free non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

Types: deb
URIs: https://mirrors.tuna.tsinghua.edu.cn/debian-security
Suites: bookworm-security
Components: main contrib non-free non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
```

### 4. 配置 PVE 源（清华 no-subscription）
写入 `/etc/apt/sources.list.d/pve-no-subscription.list`：
```
deb https://mirrors.tuna.tsinghua.edu.cn/proxmox/debian/pve bookworm pve-no-subscription
```

### 5. 禁用企业版源（没有订阅许可证用不了）
```bash
mv /etc/apt/sources.list.d/pve-enterprise.list /etc/apt/sources.list.d/pve-enterprise.list.disabled 2>/dev/null
mv /etc/apt/sources.list.d/pve-enterprise.sources /etc/apt/sources.list.d/pve-enterprise.sources.disabled 2>/dev/null
```

### 6. 配置 Ceph 源（清华源）
写入 `/etc/apt/sources.list.d/ceph.sources`：
```
Types: deb
URIs: https://mirrors.tuna.tsinghua.edu.cn/proxmox/debian/ceph-squid
Suites: bookworm
Components: main
Signed-By: /usr/share/keyrings/proxmox-archive-keyring.gpg
```

> 注意：Ceph 源组件名是 `main`，如果报 "component misspelt" 警告，检查 Ceph 镜像路径是否正确。

### 7. 验证
```bash
apt-get update
```
全部 `Hit` 就是成功。

## 可用镜像源
| 镜像 | 地址 |
|------|------|
| 清华大学 | `https://mirrors.tuna.tsinghua.edu.cn/` |
| 中科大 | `https://mirrors.ustc.edu.cn/` |
| 阿里云 | `https://mirrors.aliyun.com/` |

## 踩坑记录
1. **sources.list 和 debian.sources 重复** → 两个文件都配了同样的源，会报 "configured multiple times" 警告。解决：清空 sources.list，只用 debian.sources。
2. **trixie 和 bookworm 混用** → 系统是 bookworm 但源写了 trixie，导致包版本不匹配。一定要确认 PVE 版本对应正确的 Debian 代号。
3. **企业版源报错** → pve-enterprise 源需要订阅许可证，没有的话直接禁用。
4. **PVE Web 界面显示旧错误** → Web 界面的任务列表会缓存之前的错误记录，时间戳是旧的不代表现在还有问题。命令行 `apt-get update` 成功就行。
