# VMware安装CentOS纪实

### 一、软件版本

1. 宿主机`window10`
2. 虚拟机软件`VMware Workstation 15 player`
3. CentOS镜像`CentOS-7-x86_64-DVD-2009.iso`
4. SSH终端`Xshell6`
5. SFTP传输工具`Xftp`

### 二、基础安装

1. 打开VMware，选择新建虚拟机，选择Linux镜像，自定义虚拟机名称和存储位置
2. 按需分配核心数和内存大小、硬盘大小
3. 自定义硬件-网络选择桥接模式，点击完成
4. 配置语言、软件选择【选开发及生成工作站即可】，点击开始安装
5. 安装中设置root密码，可选创建用户
6. 完成安装，重启后点击同意lesence

### 三、调试网络

此时虚拟机无法与外界完成联通，也无法与其它虚拟机联通，需要进行网络设置

1. 使用`dhclient`为当前虚拟机分配IP地址，注意，需要为root用户，切换至root用户：`su root`

2. 分配完成后使用`ifconfig`查看分配的ip，记下来

3. 编辑网卡设置，使用命令`vim /etc/sysconfig/network-scripts/ifcfg-ens33`，配置如下内容

   ```
   ……
   
   BOOTPROTO=static
   
   ……
   
   ONBOOT=yes
   IPADDR=分配的IP
   NETMASK=255.255.255.0
   GATEWAY=XXX.XXX.XXX.1
   DNS1=119.29.29.29
   ```

   注意以下命令：

   * 编辑按`i`
   * 退出编辑按`Esc`
   * 保存并退出输入`:wq`

4. 验证是否能ping通`www.baidu.com`、宿主机、本机、其它虚拟机

### 四、其他问题

##### 1.宿主机能ping通虚拟机，虚拟机却ping不通虚拟机

上网查了下，可能是两种原因造成，第一是宿主机防火墙拦住了，但是我宿主机使用了火绒，没开window防火墙

另一个原因是网络与共享中心的高级共享设置中，文件和打印机共享处没有启用导致，开启后，问题解决。

