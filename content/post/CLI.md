---
title: "Command Line Interface"
description: "cli"
date: 2021-04-09
lastmod: 
weight: 3
categories:
    - shell
tags:
    - shell

---


# 第一篇章：CLI - 命令行界面(Command Line Interface)

## CMD指令
|  指令                               | 含义                                   |
|  -------                            | -------                               |
| cmd\wt                              | 命令行                                 |
| compmgmt.msc                        | 计算机管理                             |
| gpedit.msc                          | 组策略                                 |
| cleanmgr                            | 磁盘清理工具                           |
| control                             | 控制面板                               |
| mstsc                               | 远程桌面连接                           |
| osk                                 | 屏幕键盘                               |
| regedit                             | 注册表                                 |
| netsh                               | （winsock reset）重新初始化网络环境     |
| msinfo32                            | 系统信息                               |
| ncpa.cpl                            | 网络适配器                             |

---------------------------------------------------------------------


|  指令                               | 含义及作用                            |
|  -------                            | -------                              |
| ping                                | （连通）ping 192.168.1.1 -t           |
| netstat                             | （端口状态）netstat -an               |
| telnet                              | （联通）telnet 192.168.1.1            |
| tracert                             | (跟踪路由信息) 用法：tracert IP        |
| route                               |  (路由状态) route printf              |
| nbtstat                             |                                      |
| net                                 |                                      |
| at                                  |                                      |
| ftp                                 |                                      |
| nslookup                            |  (dns解析) nslookup baidu.com         |
| tcping\ipref3                       |  (需要额外安装指令集)                  |
| ipconfig(/all /displaydns /flushdns /release /renew)  | TCP/IP 网络配置值   |
| shutdown                            | （关机） shutdown -s -t 0             |
|                                     | （重启） shutdown -r -t 0             |
| wmic                                | (memorychip list brief)内存信息       |


---------------------------------------------------------------------
## Windows cmd窗口常用命令
cmd中输入 命令名 /? ，就可查看其对应的帮助文件。

1、ping 命令：用来验证与远程计算机的连接。

ping 是Windows自带的一个DOS命令。利用它可以检查网络是否能够连通和分析网络速度，用好它可以很好地帮助我们分析判定网络故障。

输入ping按回车即可看到详细说明。默认响应4下结束

```bash
语法：ping   [选项]  [主机名称或IP地址]
```

```bash
PS C:\Users\panso> ping 192.168.1.1

正在 Ping 192.168.1.1 具有 32 字节的数据:
来自 192.168.1.1 的回复: 字节=32 时间=3ms TTL=64
来自 192.168.1.1 的回复: 字节=32 时间=2ms TTL=64
来自 192.168.1.1 的回复: 字节=32 时间=2ms TTL=64
来自 192.168.1.1 的回复: 字节=32 时间=2ms TTL=64

192.168.1.1 的 Ping 统计信息:
    数据包: 已发送 = 4，已接收 = 4，丢失 = 0 (0% 丢失)，
往返行程的估计时间(以毫秒为单位):
    最短 = 2ms，最长 = 3ms，平均 = 2ms
```

2、用命令查看和终止进程

netstat命令：显示协议统计信息和当前 TCP/IP 网络连接

```bash
语法：netstat   [选项] 
```

常用参数：

-a            显示所有连接和侦听端口。

-n            以数字形式显示地址和端口号。

-o            显示拥有的与每个连接关联的进程 ID。

比如：

查看监听端口以及监听对应的进程（PID）>netstat -ano | findstr 端口号

```bash
D:\>netstat -ano | findstr 8000
  TCP    0.0.0.0:8000           0.0.0.0:0              LISTENING       29296
  TCP    [::]:8000              [::]:0                 LISTENING       29296
```



## Bash shell指令(linux shell=bash)



### 附录：良心网站
[Quick Reference](http://bbs.laoleng.vip/reference/index.html)

[runoob](https://www.runoob.com/)



# 第二篇章：计算机学习交流网址

[羽翼城个人博客（dogfight360）](https://www.dogfight360.com/blog/)

[HTTP响应状态码](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/status)

