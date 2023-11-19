---
title: OSI网络参考模型
date: 2023-11-18 20:41:10 +0800
categories: ['Tech', 'Network']
tags: ['网络', '协议']
---

## OSI参考模型体系结构

*上三层定义为应用程序,下四层定义为数据流*

| 名称 | Name | 说明 | 功能定义 |
| ---| --- | --- | --- |
| 应用层 | Application | 应用程序及接口| HTTP,POP3,FTP |
| 表示层 | Presentation | 对数据进行转换,加密/压缩 | jpg,gif,gzip,ascii |
| 会话层 | Session | 建立/管理和终止会话 |对话控制,同步,PDU |
| 传输层 | Transport Layer | 提供可靠的端到端的保温传输和差错控制 | 建立连接,分段(Segment),TCP/UPD,端口 |
| 网络层 | Network Layer| 将分组从源端传送到目的端;提供网络互联 | WAN寻址,路由,转发,打包(Packet),IP |
| 数据链路层 | Data Link | 将分组数据封装成帧;提供节点到节点方式的传输 | 成帧(Frame),LAN寻址,MAC地址,(IEEE 802.3协议) |
| 物理层 | Physical | 在媒体上传输Bit;提供机械的电气的规约 | 物理设备搬运报文,位(Bit) |

*路由器在网络层*

*交换机在数据链路层*

*集线器在物理层*

特点:
1. OSI模型每层都有自己的功能集
2. 层与层之间互相独立又互相依赖
3. 上层依赖下层,下层为上层提供服务

## DoD模型

| DoD模型分层 | OSI模型分层 | 说明 |
| -- | -- | -- |
| 应用层 | 应用层/表示成/会话层 | Telent/HTTP, FTP/SMTP,TFTP/NFS,SNMP/DHCP |
| 主机到主机层 | 传输层 | TCP,UDP,Port |
| 因特网层 | 网络层 | ICMP,ARP,RARP,IP |
| 网络接入层 | 数据链路层/物理层 | Ethernet,Fast Eth,TokenRing,FDDI |


### 主机到主机层

#### TCP

传输控制协议,属于面向连接的网络协议(20Bytes)

- 面向连接
- 可靠传输
- 流控

应用:Web应用程序、SSH、文件传输(SMB)、远程访问、数据库访问、电子邮件

##### TCP三次握手

*Acknowledgement number(32)*

1. SYN：客户端发送同步请求
2. SYN-ACK：服务器确认并发送同步回应
3. ACK：客户端确认号,ACK = SEQ + 1

##### 滑动窗口机制

服务端窗口size会根据ack对客户端size进行调整
#### UDP

用户保文协议,属于无链接的网络协议(8Bytes)

- 无连接
- 不可靠传输
- 尽力传递

应用:实时通信应用程序、流媒体、DNS、广播、语音通话、视频聊天、实时游戏

#### 端口

- 源端口随机分配,目标端口使用知名端口
- 应用客户端使用的源端口号一般是系统中未使用的且大于1023的端口(1024 ~ 65535)
- 目的端口号为服务器端应用服务的进程端口号

##### 常见的端口号

> [可参考IANA](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)

- 公认端口(0~1023)

| 服务 | 默认端口 |
| -- | -- |
| FTP | 21 |
| SSH | 22 |
| Telnet | 23 |
| SMTP | 25 |
| TFTP | 69 |
| HTTP | 80 |
| POP2 | 109 |
| POP3 | 110 |
| SFTP | 115 |
| NTP | 123 |
| IMAP | 143 |
| SNMP | 161 |
| BGP | 179 |
| LDAP | 389 |
| HTTPS | 443 |
| *SMTPS* | 465 |
| RTSP | 554 |
| *IMAPS* | 993 |
| *POP3S* | 995 |

- 注册端口(1024~49151)

| 服务 | 默认端口 |
| -- | -- |
| socks | 1080 |
| sqlServer | 1433 |
| oracle | 1521 |
| zookeeper | 2181 |
| mysql | 3306 |
| db2 | 5000 |
| postgresql | 5432 |
| redis | 6379 |
| tomcat | 8080 |
| elasticsearch | 9200 |
| dubbo | 20800 |
| mongoDB | 27017 |

 - 动态/私有端口(49152~65535)

### 因特网层
#### ARP

ARP基本功能
- 将ipv4地址解析为MAC地址
- 维护映射的缓存

查看arp
```bash
# windows
show arp
# linux/mac
arp -a
```

#### 其他工具

`ping (ICME)`检测网络是否通畅

```shell
ping google.com
```

`Traceroute/Tracert`追踪网络中目标经过每一跳的设备

```shell
# windows
tracert google.com
# linux/mac
traceroute google.com
```

