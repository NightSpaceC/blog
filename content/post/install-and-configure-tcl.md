---
author: "Night Space"
title: 安装与配置 Tiny Core Linux
date: 2023-04-09T12:25:20+08:00
lastmod: 2023-04-09T21:25:20+08:00
tags:
    - 笔记
categories:
    - 笔记
---
# 安装

配置DNS服务器

```
sudo printf "nameserver <DNS服务器IP>" > /etc/resolv.conf
```

安装安装程序

```bash
tce-load -wi tc-install
```

安装

```
sudo tc-install.sh
```

# 配置系统

设置密码

```bash
passwd
sudo vi /opt/.filetool.lst
# 添加：
# /etc/shadow
filetool.sh -b # 保存更改，下同
```

配置网络

```bash
sudo vi /opt/init-network.sh
# 内容：
# ifconfig eth0 <IP地址> netmask <子网掩码>
# route add default gw <默认网关> dev eth0
# printf "nameserver <DNS服务器IP>" > /etc/resolv.conf
sudo chmod +x /opt/init-network.sh
sudo vi /opt/bootloader.sh # /opt/bootloader.sh 会在开机时自动执行
# 添加：
# /opt/init-network.sh
filetool.sh -b
```

## 安装 OpenSSH

安装

```bash
tce-load -wi openssh
sudo cp /usr/local/etc/ssh/sshd_config.orig /usr/local/etc/sshsshd_config
sudo vi /usr/local/etc/sshsshd_config
# 编辑配置
```

修复SFTP

```patch
--- /usr/local/etc/ssh/sshd_config.orig
+++ /usr/local/etc/ssh/sshd_config
@@ 0,7 0,7 @@
 #Banner none
 
 # override default of no subsystems
-Subsystem      sftp    /usr/libexec/sftp-server
+Subsystem      sftp    /usr/local/lib/openssh/sftp-server
 
 # Example of overriding settings on a per-user basis
 #Match User anoncvs
```

初始化

```bash
sudo /usr/local/etc/init.d/openssh start
```

自动启动
```bash
sudo vi /opt/bootloader.sh
# 添加：
# /usr/local/etc/init.d/openssh start
```

保存

```bash
sudo vi /opt/.filetool.lst
# 添加：
# /usr/local/etc/ssh
filetool.sh -b
```