---
title: 常用命令及sublime笔记
date: 2017-01-21 23:07:49
tags:
---

## [HTTP/HTTPS部分]
HTTP协议指的是超文本传输协议。
#### HTTP/1.0和HTTP/1.1支持的方法
* GET    ---->获取资源
* POST   ---->传输实体主体

<!-- more -->
* PUT    ---->传输文件
* DELETE ---->删除文件(前面四个比较常用)
* HEAD   ---->获得报文首部
* OPTIONS---->询问支持的方法
* TRACE  ---->追踪路径
* CONNECT---->要求用隧道协议连接代理

## [其他部分]
#### sublime快捷键
        html:xt         x表示XHTML，t表示transitional
        ctrl+shift+d    复制当前行
        ctrl+shift+k    删除当前行
        ctrl+shift+↑    上移当前行
        ctrl+shift+↓    下移当前行
#### windows快捷键
        windows+E 	     打开资源管理器
        windows+D	     显示桌面
#### 控制台命令
        $cls/clear      清空控制台
        $netstat -a -o  去打印本机所正在使用的端口

## [nodejs]
    node已经支持ipv6了
    nodemon这个npm包可以监听某个文件变化从而重新对它启动服务(eg: nodemon test.js)

浏览器输入url地址按回撤后，浏览器会把url这些信息封装成有特定个格式的请求报文，然后将
这个报文传输到服务器端，这个方式叫做socket。

telnet 建立链接
 eg: telnet 127.0.0.1 2080(127.0.0.1 是本地回环地址)
