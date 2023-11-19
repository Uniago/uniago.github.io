---
title: IOS配置
date: 2023-11-19 12:33:16 +0800
categories: ['Tech', 'Network']
tags: ['网络', '路由器']
---

## 路由器

![](/assets/img/20231119123714.png)


### 路由器组成与功能

- CPU 执行操作系统的指令
- 随机访问存储器(RAM),RAM中内容断电丢失
	- 运行操作系统(IOS)
	- 运行配置文件
	- IP路由表
	- ARP缓存
	- 数据包缓冲区
- 只读存储器(ROM),保存开机自检,存储路由器的启动引导程序
	- bootstrap指令
	- 基本的自检软件(POST)
	- 迷你版IOS
- 非易失RAM(NVRAM),存储启动配置,包括IP地址、路由协议、主机名等
- 闪存Flash,运行操作系统(Cisco IOS)
- Interfaces,拥有多种物理接口,用于连接网络接口类型

1. 寻址
2. 路径选择(路由表)
3. 转发
4. 广播控制
5. 流量过滤
6. WAN

### 路由器启动步骤

1. 检测路由器硬件
	1. Power-On self Test(POST)
	2. 执行bootstrap
2. 定位加载Cisco IOS软件
	1. 定位IOS
	2. 加载IOS
3. 定位加载启动配置文件或进入配置模式
	1. 启动程序搜寻配置文件

## Cisco IOS

[Cisco IOS Download for GNS3 | SYSNETTECH Solutions](https://www.sysnettechsolutions.com/en/cisco-ios-download-for-gns3/)

> Cisco Internetwork Operation System(IOS),是为Cisco设备配置的软件系统,他是Cisco的一项核心技术,应用于路由器、局域网交换机、小型无线接入点、具有几十个接口的大型路由器以及许多其他设备

Cisco IOS可为设备提供网络服务

- 基本的路由和交换功能
- 安全可靠的访问网络资源
- 网络可伸缩性

### 访问CLI环境

- 控制台
- Telnet或SSH
- 辅助端口

### Cisco IOS模式

![[/assets/img/20231119160615.png]]

- 用户自行模式: `>`
- 特权执行模式: `#`
- 全局配置模式
- 其他特定模式


**基本命令**

- ?:帮助
- enable: 进入特权模式
- configure terminal: 进入全局配置模式
- exit: 返回到上个模式
- end: 返回到用户模式


### 常用命令

修改设备名称

```shell
# 特权模式下切换到全局配置模式
config ter
# 修改名称
hostname AS_R1
```

控制台密码:

```shell
# 全局模式下进入行模式
line console 0
# 增加 密码
password asdfasdf
# 让密码生效
login
# 让密码失效
no login
```

特权模式密码:

```shell
# 全局模式下
enable secret 0
# 直接设置密码
enable secret asdf
```

vty密码

```shell
# 进入行模式
line vty 0 4
# 设置密码
password asdfasdfasdf
# 生效密码
login
```

设置vty密码后,其他已连接的设备可以在特权模式下进行`telnet`连接

保存配置

```shell
# 查看已修改的配置
show running-config # 存放在RAM中,断电丢失
show start-up-config # 存储在NVRam/Flash中
# 保存配置
write
# 或 拷贝到其他配置文件中也能保存
copy running-config start-up-config
```

删除配置

```shell
# 释放
erase startup-config
# 或 删除配置文件
delete flash:config.text
```


接口配置

```shell
# 全局模式下,进入接口
interface ethernet 0/0

# 为接口配置ip地址
ip address 192.168.1.1 255.255.255.0

# 激活设备
no shutdown # Cisco设备接口默认shutdown状态

# 在全局模式下,测试一下
ping 192.168.1.1

# 串行链路需要同步时间
clock rate 64000 # 只需要在DCE端配置时钟信号

```

show命令

```shell
show ?

# 查看当前操作系统版本
show version

# 查看运行配置
show running-config

# 查看启动配置
show startup-config

# 查看flash
show flash

# 查看cpu利用率
show cpu

# 查看内存使用情况
show memory

# 查看接口信息
show interface

# 查看某个接口信息
show interface f 0/0

# 查看ip对不对
show ip interface brief
```