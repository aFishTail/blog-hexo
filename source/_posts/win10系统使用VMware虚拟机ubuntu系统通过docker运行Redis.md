---
title: win10系统使用VMware虚拟机ubuntu系统通过docker运行Redis
date: 2020-05-06 20:02:47
tags: docker
---
五一期间花了一两天时间学了下docker，虽然仅仅学会了几个命令，但是仍然体会到其强大。
之前在本地win10系统运行Redis需要先下载安装包，然后进行安装。安装好之后再通过命令行启动服务。具体可见[菜鸟教程安装Redis](https://www.runoob.com/redis/redis-install.html)
但是通过docker的话就只需要几个命令就可以
```
docker pull redis:latest // 安装最新的redis
docker run -itd --name redis-test -p 6379:6379 redis
```
同样，菜鸟教程也有完整的[docker搭建Redis示例](https://www.runoob.com/docker/docker-install-redis.html)
起初我是直接看电子书学习docker,有不少问题，比如版本不对，命令不生效，报错不知道如何解决。后来偶然发现开源的[docker从入门到实战](https://yeasy.gitbook.io/docker_practice/)在线教程，真的比啃书舒服多了，毕竟技术时效性太强了。**感谢开源！**
> 好了扯了这么多开始进入正题，本地怎么访问到虚拟机里面docker运行的服务？
上面已经说了Docker如何运行服务并映射到本地端口，那么剩下问题就是**获取虚拟机的ip地址**，然后本地通过**虚拟机的ip地址+docker容器映射的端口**就可以访问对应的服务了
那怎么获取虚拟机的IP地址呢，google了下，虚拟机网络模式，无论是vmware,virtual box,virtual pc等虚拟机软件，一般来说，虚拟机有三种网络模式:
- 桥接
- NAT
- Host-Only

> **重点**： 由于NAT的网络在vmware提供的一个虚拟网络里，所以局域网其他主机是无法访问虚拟机的，而宿主机可以访问虚拟机，虚拟机可以访问局域网的所有主机，具体可见（[实例讲解虚拟机3种网络模式(桥接、nat、Host-only)](https://www.cnblogs.com/ggjucheng/archive/2012/08/19/2646007.html)）

由于只需要本地访问虚拟机ip地址，我果断地选择了默认的NAT模式

然后在ubuntu中获取IP地址，可以通过`ifconfig -a`命令
![Ubuntu查看ip地址](http://yanxuan.nosdn.127.net/1b690ab3282a866a94612f33e6ba6db0.png)
箭头所指地就是虚拟机地ip地址了
然后我们本地Ping一下试试
![本地Ping虚拟机的IP地址](http://yanxuan.nosdn.127.net/437bd3249b9bdabceff927646e91bf0c.png)
GOOD！没有任何问题！接下来可以验证下Redis服务能不能使用了。
首先试一下通过本地redis-cli连接docker服务，set几个key
![本地redis-cli连接docker redis服务](http://yanxuan.nosdn.127.net/549ded049e119d39daf4e7db493e36a9.png)
可以看出，目前redis服务里有两个key值，分别是test, testvalue
再到虚拟机的docker看一下，完全一样！
![docker 容器内查看redis服务](http://yanxuan.nosdn.127.net/e743037486e0919b2bf6a722a4ed8903.png)
到此也就结束了，其实大部分都是google来的，照着做了一遍解决了问题。给我的启示就是多搜索，多想多动手才能解决问题。同时积累基础知识（linux,docker），可能一时没法精通，但是接触、积累多了总会熟悉、精通的，这是个必须且正向的过程。
