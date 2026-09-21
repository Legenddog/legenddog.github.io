---
title: "命令行界面"
description: "Command Line Interface"
date: 2021-04-09
lastmod: 
weight: 3
categories:
    - shell
tags:
    - shell

---


# 第一篇章：CLI - 命令行界面(Command Line Interface)


## 一、CMD 命令速查表

### 1. 系统管理命令

|  指令                               | 含义                                   |
|  -------                            | -------                               |
| `cmd`/`wt`                              | 命令行                                 |
| `compmgmt.msc`                        | 计算机管理                             |
| `gpedit.msc`                          | 组策略                                 |
| `cleanmgr`                            | 磁盘清理工具                           |
| `control`                             | 控制面板                               |
| `mstsc`                               | 远程桌面连接                           |
| `osk`                                 | 屏幕键盘                               |
| `regedit`                             | 注册表                                 |
| `netsh`                               | （winsock reset）重新初始化网络环境     |
| `msinfo32`                            | 系统信息                               |
| `ncpa.cpl`                            | 网络适配器                             |

### 2. 网络诊断命令


|  指令                               | 含义及作用                            |
|  -------                            | -------                              |
| `ping`                                | 测试连通性，如 `ping 192.168.1.1 -t`         |
| `netstat`                             | 查看端口状态，如 `netstat -an`              |
| `telnet`                              | 测试端口联通性，如 `telnet 192.168.1.1`           |
| `tracert`                             | 跟踪路由信息，用法：`tracert IP`       |
| `route`                               | 查看路由状态，如 `route print`              |
| `nbtstat`                             | 查看 NetBIOS 统计信息        |
| `net`                                 | 网络服务管理                        |
| `at`                                  | 计划任务                              |
| `ftp`                                 | 文件传输协议客户端                       |
| `nslookup`                            | DNS 解析查询，如 `nslookup baidu.com`      |
| `tcping`/`ipref3`                       |  (需要额外安装指令集)                  |
| `ipconfig`  | TCP/IP 网络配置值（参数：`/all`、`/displaydns`、`/flushdns`、`/release`、`/renew`）   |
| `shutdown`                | 关机：`shutdown -s -t 0`；重启：`shutdown -r -t 0`            |
| `wmic`                                | 内存信息查询，如 `wmic memorychip list brief`       |


---


## 二、Windows CMD 窗口常用命令


在 CMD 窗口中输入 `命令名 /?` 即可查看对应的帮助文件。

### 1. ping 命令：验证与远程计算机的连接

`ping` 是 Windows 自带的 DOS 命令，用于检查网络连通性、分析网络速度，可帮助快速判定网络故障。输入 `ping` 后按回车即可查看详细说明，默认响应 4 次后结束。

**语法：**


```bash
ping [选项] [主机名称或IP地址]
```

**示例：**

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

### 2. netstat 命令：查看进程和端口状态


`netstat` 命令用于显示协议统计信息和当前 TCP/IP 网络连接。

**语法：**

```bash
netstat [选项]
```

**常用参数：**

| 参数 | 说明 |
| --- | --- |
| `-a` | 显示所有连接和侦听端口 |
| `-n` | 以数字形式显示地址和端口号 |
| `-o` | 显示拥有的与每个连接关联的进程 ID |

**示例：** 查看监听端口及其对应的进程（PID）

```bash
D:\>netstat -ano | findstr 8000
  TCP    0.0.0.0:8000           0.0.0.0:0              LISTENING       29296
  TCP    [::]:8000              [::]:0                 LISTENING       29296
```

---


## 三、Bash Shell 命令（Linux Shell = Bash）

> [!TIP]
> Bash 是 Linux 系统默认的 Shell 环境，常用命令与 Windows CMD 有所差异，但核心逻辑相通。具体命令可参考附录中的在线资源。

---

## 附录：良心网站

- [Quick Reference](http://bbs.laoleng.vip/reference/index.html)
- [runoob（菜鸟教程）](https://www.runoob.com/)

---

# 第二篇章：计算机学习交流网址

- [羽翼城个人博客（dogfight360）](https://www.dogfight360.com/blog/)
- [HTTP响应状态码（MDN 文档）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/status)




