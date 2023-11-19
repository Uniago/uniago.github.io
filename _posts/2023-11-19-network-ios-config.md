---
title: IOS配置
date: 2023-11-19 12:33:16 +0800
categories: ['Tech', 'Network']
tags: ['网络', '路由器']
---

## 路由器

![[assets/img/Pasted image 20231119123714.png]]


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

![[assets/img/Pasted image 20231119160615.png]]

- 用户自行模式: `>`
- 特权执行模式: `#`
- 全局配置模式
- 其他特定模式