# IPV4

网络中计算机的唯一标识
组成：网络段号+主机段号，4字节32位整数，分成4段8位的二进制数，圆点隔开，表示时每8位二进制数转换成十进制整数表示。

### 1）号段分类

##### 1.1）A类：政府机构【约占1/2】

```
* 1.0.0.1~126.255.255.254
```

##### 1.2）B类：中型企业【占1/4】

```
* 128.0.0.1~191.255.255.254
```

##### 1.3）C类：个人用户【占1/8】

```
* 192.0.0.1~223.255.255.254
```

##### 1.4）D类：组播【占1/16】

```
* 224.0.0.1~239.255.255.254
```

##### 1.5）E类：实验【占1/16】

```
* 240.0.0.1~255.255.255.254
```



### 2) 常见特殊号段

##### 	2.1）私有地址：在互联网上不使用，用在局域网上的

	* A类：10.X.X.X	// 即10.0.0.0/8
	* B类：172.16.0.0 ~172.31.255.255	// 即172.16.0.0/12
	* C类：192.168.X.X	//	即192.168.0.0/16
	* 169.254.X.X	// 即169.254.0.0/16  【Windows操作系统在DHCP信息租用失败时自动给客户机分配的IP地址】

##### 	2.2）回环地址：表示本机

	* 127.0.0.1

##### 	2.3）广播地址：与所有人通信

	* X.X.X.255

##### 	2.4）网络地址：

	* X.X.X.0

### 	2）常用命令：

```
查看本机IP：					
ipconfig

查看本机与指定IP间的通信是否有问题：	
ping  IP地址
```

### 	3）JAVA常用类：

```
InetAddress类：表示互联网协议（IP）地址
该类无构造方法
可由getByName方法，获取对象
```

