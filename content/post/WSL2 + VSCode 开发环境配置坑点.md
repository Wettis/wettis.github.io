---
title: "WSL2 + VSCode 开发环境配置坑点"
date: 2023-10-13T15:51:37+08:00
categories: []
---

# WSL2 编程环境配置

系统版本： Ubuntu 22.04

## 超级管理员密码错误

使用 su 命令并且输入预设的密码，发现su：Authentication failure

解决办法 ( 重设 root 密码 ， 需要键入预设的密码 )

```bash
sudo passwd root
```

## WSL2 网络防火墙

WSL 1 与 WSL 2 的网络情况并不相同

如果你是WSL2则需要新建一条防火墙规则

```powershell
New-NetFirewallRule -DisplayName "WSL" -Direction Inbound  -InterfaceAlias "vEthernet (WSL)"  -Action Allow
```

如果新建成功 在 `设置` -> `防火墙` -> `高级设置` -> `入站规则` 能看见 WSL

## 配置网络代理 （Clash For Windows）

1.  编写一段脚本 保存 [.proxyrc]

```bash
#!/bin/bash
host_ip=$(cat /etc/resolv.conf |grep "nameserver" |cut -f 2 -d " ")
echo $host_ip
export ALL_PROXY="http://$host_ip:7890"
```

2. 运行脚本 .  /.proxyrc

## 配置WSL2 的DNS解析地址

```bash
sudo rm /etc/resolv.conf
sudo bash -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'
sudo bash -c 'echo "[network]" > /etc/wsl.conf'
sudo bash -c 'echo "generateResolvConf = false" >> /etc/wsl.conf'
```

## WSL2 路由配置不正确

主机与WSL互相能够ping通但是不在一个网段

```bash
ip route # 查看当前 路由信息
# $ ip route
# default via 172.28.224.1 dev eth0
# 172.28.224.0/20 dev eth0 proto kernel scope link src 172.28.226.213
ip route del default # 删除默认网关
ip route add default via 172.28.224.1 dev eth0
```

## 关闭自动生成 resolv.conf 文件 指定DNS

```bash
vim /etc/wsl.conf # 创建 wsl.conf 文件
```

```
# wsl.conf 需要添加以下内容
[network] 
generateResolvConf = false

# 修改默认生成文件内容
/etc/resolvconf/resolv.conf.d/base
```

## 目录映射错误

用作为当前目录的以上路径启动了 CMD.EXE。
UNC 路径不受支持。默认值设为 Windows 目录。

```bash
reg add "HKEY_CURRENT_USER\Software\Microsoft\Command Processor" /v "DisableUNCCheck" /t "REG_DWORD" /d "1" /f
```

## 权限不足

npx create-react-app my-app 出现权限不足问题

su root账户登录

## 一些命令

| 命令                 |                 释义                 |
| -------------------- | :----------------------------------: |
| cat /etc/resolv.conf | 查看 WSL 主机 IP 地址 (`nameserver`) |
| wsl -t [name]        |               关闭WSL                |
| wsl                  |               启动WSL                |
| wsl --list           |            查看子系统列表            |

