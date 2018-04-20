---
title: "Linode 上搭建 Shadowsocks"
comments: true
date: 2018-04-13 09:08:00
udpated: 2018-04-20 13:18:00
categories:
 - Technology
 - Shadowsocks
tags:
 - Linode
 - Shadowsocks
 - bbr
---

## VPS 选择

### [Linode](https://www.linode.com/?r=0a89daeb73aede849b760c66127f1bb8960c73dc)

老大哥Linode近年最大的变化是，倍感digitalocean和vultr的压力了。Linode近期最大的福利是，全面由原来的Xen架构改用最流行的KVM架构服务器，套餐内存全部翻倍！

例如，过去每月$10只能买1G内存套餐，现在你可购买Linode 2G套餐，目前最低$5可以买1G内存套餐，这也是最低套餐配置了，绝对具有性价比。

Linode也允许用户按月付费，仍然采用信用卡付款的方式。新增了Paypal付款方式，目前处于测试阶段。需要提醒中国用户，Paypal仍然会要求你绑定一张双币信用卡（支持国内发行的visa信用卡），所以你应当尽快申请一张属于自己的信用卡。

Linode日本机房长期缺货，无法购买。美国西海岸Fremont机房位于加州，是中国大陆用户最应当购买的机房位置。

### [Vultr](www.vultr.com)

Vultr这两年的增长速度惊人，中国站长把原来digitalocean的服务器搬到了vultr，原因是vultr日本机房速度很快、同样的1G内存套餐，每月只要$8，具有超高性价比。

日本机房速度一直很快，没有绕行美国。今年9月初，杭州开会期间，Vultr日本机房线路突然变得非常慢，ping丢包严重，但是此事件仅持续了一周左右时间，又恢复正常，怀疑是中国电信国际出口线路调整导致的。在该期间，用户可把vps搬家到Vultr美国SFO机房，速度仍然很棒。

### [digitalocean](www.digitalocean.com)

digitalocean新增SFO 2号机房，是对中国用户来说相当不错的选择。印度机房虽然距离中国近，但线路并未优化，速度不如美国西海岸机房。

价格方便，$10每月享受1G内存、2TB月流量、单核CPU、KVM架构，在当前竞争激烈的VPS市场，表现中庸。

digitalocean最大的变化是，过去三年间，中国买家疯狂涌入，导致线路表现不佳。大批用户搭建出国梯子，导致digitalocean机房很多IP被封。

不明就里的新用户兴奋地创建一个新账号，结果竟然ping不通服务器，无法连接ssh，其实是因为自动分配的IP被墙的原因。目前，digitalocean线路稳定，原因之一是竞争对手vultr把很多中国用户吸引了过去。
<!-- more -->

## 注册/申请服务器

### 注册账号

登陆 [Linode](https://www.linode.com/?r=0a89daeb73aede849b760c66127f1bb8960c73dc) 注册账号，需要绑定一张信用卡，注册时填写邀请码(**0a89daeb73aede849b760c66127f1bb8960c73dc**)可获得奖励。

### 申请服务器
* Add a Linode 创建服务器
* Select an instance type 因为这里只做流量转发，所以选最小的类型:1024
* Location 服务器位置选 **Fremont, CA**
* 点击 Add This Linode

### 配置操作系统镜像
* 进入 Linodes 界面，点击 **Deploy an Image**。
* 操作系统镜像选择 **Ubuntu 16.04 LTS**，输入 root 用户密码，其他保持默认。
* Boot 启动服务器。

### 更新系统
```sh
# 更新源
$ apt-get update
# 更新已安装的包
$ apt-get upgrade
# 升级系统
$ apt-get dist-upgrade
```
更新完成后 reboot 重新启动系统。

### 开启 bbr
BBR是Google开源的TCP拥堵控制算法，与2016年9月开源。BBR的目的是要尽量跑满带宽, 并且尽量不要有排队的情况。
linux 内核4.9版本以上已集成了bbr算法支持，使用 **uname -r**查看内核版本。
```sh
$ uname -r
4.15.12-x86_64-linode105
```
执行以下命令修改系统配置并保存：
```sh
$ echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
$ sysctl -p
```
检查系统配置：
```sh
$ sysctl net.ipv4.tcp_available_congestion_control
net.ipv4.tcp_available_congestion_control = reno bbr bic cubic westwood htcp
$ sysctl net.ipv4.tcp_congestion_control
net.ipv4.tcp_congestion_control = bbr
```

> Linux 4.13内核版本及以上，不再需要配置 `net.core.default_qdisc=fq`。

### 优化

#### Step 1

修改 `/etc/security/limits.conf` 文件，加入以下两行：
```
* soft nofile 51200
* hard nofile 51200
```

执行 **ulimit -n 51200** 命令。

#### Step 2

修改 `/etc/sysctl.conf` 文件，增加以下配置：
```
fs.file-max = 51200

net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.core.netdev_max_backlog = 250000
net.core.somaxconn = 4096

net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 0
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_keepalive_time = 1200
net.ipv4.ip_local_port_range = 10000 65000
net.ipv4.tcp_max_syn_backlog = 8192
net.ipv4.tcp_max_tw_buckets = 5000
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_mem = 25600 51200 102400
net.ipv4.tcp_rmem = 4096 87380 67108864
net.ipv4.tcp_wmem = 4096 65536 67108864
net.ipv4.tcp_mtu_probing = 1
net.ipv4.tcp_congestion_control = bbr
```

执行 **sysctl -p** 生效。

## 搭建 Shadowsocks

### libsodium 安装
```sh
$ apt-get install build-essential
$ wget -N --no-check-certificate https://github.com/jedisct1/libsodium/releases/download/1.0.16/libsodium-1.0.16.tar.gz
$ tar zxvf libsodium-1.0.16.tar.gz
$ pushd libsodium-1.0.16
$ ./configure --prefix=/usr && make
$ make install
$ popd
$ ldconfig
```

### Shadowsocks-python

#### 安装 Shadowsocks-python
```sh
$ apt-get update
$ apt-get install python-pip
$ pip install shadowsocks
```
或者是通过源码安装最新版 Shadowsocks-python
```sh
$ apt-get update
$ apt-get install python-pip
$ apt-get install git
$ pip install git+https://github.com/shadowsocks/shadowsocks.git@master
```

#### 配置 Shadowsocks
修改 /etc/shadowsocks-python/config.json 文件，填入以下内容：
```
{
    "server":"xxx.xxx.xxx.xxx",
    "server_port":8388,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"xxxxxxxx",
    "timeout":300,
    "method":"aes-256-gcm",
    "fast_open": false
}
```
多用户配置：
```
{
    "server": "xxx.xxx.xxx.xxx",
    "port_password": {
        "8381": "xxxxxxxx",
        "8382": "xxxxxxxx",
        "8383": "xxxxxxxx",
        "8384": "xxxxxxxx"
    },
    "timeout": 300,
    "method": "aes-256-gcm"
}
```

#### 开启服务
```sh
# 启动服务
$ ssserver -c /etc/shadowsocks-python/config.json -d start
# 关闭服务
$ ssserver -c /etc/shadowsocks-python/config.json -d stop
```

### Shadowsocks-libev

#### 安装 mbedtls
```sh
$ wget https://tls.mbed.org/download/mbedtls-2.8.0-gpl.tgz
$ tar xvf mbedtls-2.8.0-gpl.tgz
$ pushd mbedtls-2.8.0
$ make SHARED=1 CFLAGS=-fPIC
$ sudo make DESTDIR=/usr install
$ popd
$ sudo ldconfig
```

#### 安装 Shadowsocks-libev
Ubuntu 14.04 and 16.04:
```sh
$ sudo add-apt-repository ppa:max-c-lv/shadowsocks-libev -y
$ sudo apt-get update
$ sudo apt-get install shadowsocks-libev
```
Ubuntu 16.10 or higher:
```sh
$ sudo apt-get update
$ sudo apt-get install shadowsocks-libev
```

#### 开启服务
```sh
# 启动
$ /etc/init.d/shadowsocks-libev start
# 停止
$ /etc/init.d/shadowsocks-libev stop
# 重启
$ /etc/init.d/shadowsocks-libev restart
# 查看状态
$ /etc/init.d/shadowsocks-libev status
```

### Shadowsocks-go

#### 安装 Shadowsocks-go
```sh
$ apt-get install golang
```
编辑 `~/.bashrc`，加入以下内容:
```sh
export GOPATH=/usr/gopath
export PATH=$PATH:$GOPATH/bin
```
执行 `source ~/.bashrc`使设置生效。然后再执行以下命令安装 Shadowsocks-go：
```sh
$ go get github.com/shadowsocks/shadowsocks-go/cmd/shadowsocks-server
```

#### 启动服务
```sh
# 后台启动
$ shadowsocks-server -c /etc/shadowsocks-go/config.json > /usr/gopath/log/log &
```
