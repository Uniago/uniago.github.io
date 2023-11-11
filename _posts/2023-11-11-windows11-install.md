---
title: Windows11安装
date: 2023-11-11 21:21:45 +0800
categories: ['Tools', 'Windows']
tags: ['Windows']
---


## 下载镜像

 在[https://next.itellyou.cn/](https://link.zhihu.com/?target=https%3A//next.itellyou.cn/)网站中复制链接到迅雷下载镜像
 以下是23H2的下载链接:
```text
ed2k://|file|zh-cn_windows_11_business_editions_version_23h2_x64_dvd_2a79e0f1.iso|6613571584|1AFADEBC5966E9E689F242549FFB5562|/
```

## 启动盘
镜像复制到ventoy启动盘的ISO目录下

## 跳过限制

 [参考](https://zhuanlan.zhihu.com/p/482466161)

由于win11安装有很多限制条件,例如:
- 需要开启TPM2.0
- 需要UEFI启动
- 内存4GB
- 磁盘64GB
- CPU英特尔8代酷睿/AMD锐龙2代

### 方案一

打开下面这个文件夹,删除`appraiserres.dll`文件
```text
C:\\$WINDOWS.~BT\Sources
```

### 方案二
#### 修改注册表

`shift+f10`进入到终端管理器,输入`regedit`进到注册表,定位到这个文件夹
```text
HKEY_LOCAL_MACHINE\SYSTEM\Setup
```
在此文件夹下创建项,命名为`LabConfig`,追加以下键值内容
```text
[HKEY_LOCAL_MACHINE\SYSTEM\Setup\LabConfig]
"BypassTPMCheck"=dword:00000001
"BypassSecureBootCheck"=dword:00000001
"BypassRAMCheck"=dword:00000001
"BypassStorageCheck"=dword:00000001
"BypassCPUCheck"=dword:00000001
```

## 跳过联网
[参考](https://zhuanlan.zhihu.com/p/627268754)
### 方案一
```text
oobe\bypassnro.cmd
```
### 方案二
#### 修改注册表
定位文件夹
```text
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\OOBE
```
新建DWORD（32位）值，名称为：BypassNRO，将值设置为1

## 激活码

```text
J8WVF-9X3GM-4WVYC-VDHQG-42CXT
7Y64F-88DCY-Y6WTC-H33D2-64QHF
BCQNW-3VWYB-4V7QD-M6R2B-7MH26
3JMNH-HBQ2K-C7VYP-4KGFR-DJ3GY
```