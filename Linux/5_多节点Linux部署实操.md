# 多节点Linux部署实操-虚拟机版

### 1.软件准备

操作系统镜像-推荐CentOS

虚拟机软件-VMware

SSH软件-SecureCRT或Xshell

FTP工具-Transmit或Xftp



### 2.系统安装

1. 使用VMware安装Linux操作系统镜像，调整所需CPU核心数、内存和存储

2. 安装中-软件选择：按需勾选

3. 安装中-安装位置：自动配置分区即可

4. 设置ROOT密码

5. 创建用户【可跳过】

6. 网络设置：

   * VMware中选择网络连接为桥接模式；

   * 切换到root用户，使用dhclient命令分配IP地址，使用ipconfig查到分配的IP地址；

   * 进入/etc/sysconfig/network-scripts/下虚拟机的网卡配置文件，将BOOTPROTO的值改为static，ONBOOT改为yes，增加IPADDR=刚才查到的,NETMASK=255.255.255.0,GATEWAY=192.168.xxx.1,DNS1=119.29.29.29;

   * 使用命令systemctl restart network.service重启网卡

