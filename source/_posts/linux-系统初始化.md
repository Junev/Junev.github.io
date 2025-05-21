---
title: linux 系统初始化
date: 2025-05-19 09:23:58
categories: 
    - linux
tags: 
    - linux
---

# Ubuntu server

## installation

1. 磁盘划分: 注意调整磁盘空间到最大
2. 安装 openssh

## 配置网络，使用静态IP

### ubuntu 16 及之前版本 ubuntu
```bash
vim /etc/network/interfaces
```

### ubuntu 18 开始使用 netplan 配置网络

```bash
vim /etc/netplan/00-installer-config.yml
```

```yaml
# vim /etc/netplan/00-netcfg.yml

network:
  ethernets:
    ens33:
      dhcp4: false
      addresses: 
        - 172.16.107.245/23
      gateway4: 172.16.106.1
      nameserver: 
        - 114.114.114.114
```

```bash
netplan apply
```

### centos 7
vim /etc/sysconfig/network-scripts/ifcfg-ens33
```
BOOTPROTO=static
ONBOOT=yes

IPADDR=192.168.199.128
GATEWAY=192.168.199.1
NETMASK=255.255.255.0
```

vim /etc/resolv.conf
```
nameserver 114.114.114.114
```

```bash
systemctl restart network
```

## ssh
1. 把公钥加到~/.ssh/authorized_keys里
2. vim /etc/ssh/sshd_config
``` bash
PubkeyAuthentication yes

UseDNS no
```

问题1： Host key verification failed.
解决方法：
```bash
ssh-keygen -R 192.168.199.130
```

sftp

```bash
sudo vim /etc/ssh/sshd_config

Subsystem sftp internal-sftp
```

## 添加用户

```bash
sudo adduser {username}
sudo adduser {username} sudo
```

