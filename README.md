
## 1. 安装Shdowsocks服务端


#安装pip
yum install python-pip

#使用pip安装shadowsocks1234

pip install shadowsocks


## 2.配置Shdowsocks服务,并启动
vim /etc/shadowsocks.json
[root@f3s ~]# cat /etc/shadowsocks.json 

{

    "server":"0.0.0.0",   //<--是私有ip,而非公网ip
    
    "server_port":9999,
    
    "local_address":"127.0.0.1",
    
    "local_port":9999,
    
    "password":"c1cccc",
    
    "timeout":300,
    
    "method":"aes-256-cfb",
    
    "workers":5
    
}


## 注意修改 password 
workers 表示启动的进程数量 
 ssserver -c /etc/shadowsocks.json -d start   # 启动
 
 ssserver -c /etc/shadowsocks.json -d stop   # 停止
 
 ssserver -c /etc/shadowsocks.json -d restart  # 重启
 
## 运行带log的方式：
# ssserver -c /etc/shadowsocks.json --log-file /tmp/ss.log -d start
 ssserver -p PORT -k PASSWORD -m rc4-md5 --log-file /tmp/ss.log -d start


3.使用本机Shdowsocks客户端, 连接服务端上网

## 下载客户端 
windows版本 https://github.com/shadowsocks/shadowsocks-windows/releases
安卓版本    https://github.com/shadowsocks/shadowsocks-android/releases
            shadowsocks-arm64-v8a-4.6.1.apk

下载后将服务器ip、端口、密码依次填好就行了

windows 客户端需要 dotnet4.6的支持，
下载地址　//www.microsoft.com/zh-cn/download/details.aspx?id=53344
https://github.com/shadowsocks/shadowsocks-windows/releases

https://github.com/shadowsocks/shadowsocks-windows

Shadowsocks-4.1.2.zip


https://github.com/shadowsocks/libQtShadowsocks/releases

但其中有几个坑要注意：
1. 如果服务器是专有网络，/etc/shadowsocks.json 中的server ip 是私有ip,而非公网ip

2. /etc/shadowsocks.json中开放的端口需要 
  运行命令开放，iptables -A INPUT -p tcp --dport 9999 -j ACCEPT (iptables -L 服务没有启动，不需要加)

3. 服务器实例的安全组规则需要增加 自定义 TCP 在 相关端口（9999） 的访问 ,（允许所有ip访问，设置为0.0.0.0/0）
最后用telnet your_public_ip 8388验证, 只有telnet能访问端口了，才能正常使用shadowsocks!


## Shadowsocks for Android

[![CircleCI](https://circleci.com/gh/shadowsocks/shadowsocks-android.svg?style=svg)](https://circleci.com/gh/shadowsocks/shadowsocks-android)
[![API](https://img.shields.io/badge/API-21%2B-brightgreen.svg?style=flat)](https://android-arsenal.com/api?level=21)
[![Releases](https://img.shields.io/github/downloads/shadowsocks/shadowsocks-android/total.svg)](https://github.com/shadowsocks/shadowsocks-android/releases)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/1a21d48d466644cdbcb57a1889abea5b)](https://www.codacy.com/app/shadowsocks/shadowsocks-android?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=shadowsocks/shadowsocks-android&amp;utm_campaign=Badge_Grade)
[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

A [shadowsocks](http://shadowsocks.org) client for Android, written in Kotlin.  
<a href="https://play.google.com/store/apps/details?id=com.github.shadowsocks"><img src="https://play.google.com/intl/en_us/badges/images/generic/en-play-badge.png" height="48"></a>


### PREREQUISITES

* JDK 1.8
* Go 1.11+
* Android SDK
  - Android NDK r16+

### BUILD

You can check whether the latest commit builds under UNIX environment by checking Travis status.
Building on Windows is also possible since [#1570](https://github.com/shadowsocks/shadowsocks-android/pull/1570),
but probably painful. Further contributions regarding building on Windows are also welcome.

* Set environment variable `ANDROID_HOME` to `/path/to/android-sdk`
* (optional) Set environment variable `ANDROID_NDK_HOME` to `/path/to/android-ndk` (default: `$ANDROID_HOME/ndk-bundle`)
* Clone the repo using `git clone --recurse-submodules <repo>` or update submodules using `git submodule update --init --recursive`
* Build it using Android Studio or gradle script

### [TRANSLATE](https://discourse.shadowsocks.org/t/poeditor-translation-main-thread/30)

## OPEN SOURCE LICENSES

<ul>
    <li>redsocks: <a href="https://github.com/shadowsocks/redsocks/blob/shadowsocks-android/README">APL 2.0</a></li>
    <li>mbed TLS: <a href="https://github.com/ARMmbed/mbedtls/blob/development/LICENSE">APL 2.0</a></li>
    <li>libevent: <a href="https://github.com/shadowsocks/libevent/blob/master/LICENSE">BSD</a></li>
    <li>tun2socks: <a href="https://github.com/shadowsocks/badvpn/blob/shadowsocks-android/COPYING">BSD</a></li>
    <li>pcre: <a href="https://android.googlesource.com/platform/external/pcre/+/master/dist2/LICENCE">BSD</a></li>
    <li>libancillary: <a href="https://github.com/shadowsocks/libancillary/blob/shadowsocks-android/COPYING">BSD</a></li>
    <li>shadowsocks-libev: <a href="https://github.com/shadowsocks/shadowsocks-libev/blob/master/LICENSE">GPLv3</a></li>
    <li>overture: <a href="https://github.com/shawn1m/overture/blob/master/LICENSE">MIT</a></li>
    <li>libev: <a href="https://github.com/shadowsocks/libev/blob/master/LICENSE">GPLv2</a></li>
    <li>libsodium: <a href="https://github.com/jedisct1/libsodium/blob/master/LICENSE">ISC</a></li>
</ul>

### LICENSE

Copyright (C) 2017 by Max Lv <<max.c.lv@gmail.com>>  
Copyright (C) 2017 by Mygod Studio <<contact-shadowsocks-android@mygod.be>>

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <http://www.gnu.org/licenses/>.
