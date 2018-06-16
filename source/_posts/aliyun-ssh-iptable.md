---
title: aliyun ssh以及简易防火墙配置
date: 2018-05-22 15:11:39
tags: aliyun iptable ssh js fullstack
intro: 慢慢折腾吧...
---

> 接上一篇远程登录，添加用户等流程之后...

#### **修改ssh登录端口**
默认端口是22，我们可以更改为1024以上的端口。
1. 进入ssh配置文件`sudo vi /etc/ssh/sshd_config` 再打开的文件中找到 port 22 一行，将其改为新端口号。
2. 保存后执行`sudo service ssh restart`，重启ssh服务。
3. 到阿里云控制台安全组中，增加一组规则，设置为入方向，自定义tcp，把端口范围修改成新端口号。
> 尝试用新的端口号登录 ,在bash中输入"ssh manager@ip地址 -p 新端口号",如果顺利则可以登录，如果刚才折腾中出现了错误，现在不能登录，可以尝试到阿里云控制台里面的远程连接以root身份进行尝试。

#### **更新Ubuntu资源包和升级Ubuntu**
在命令行中输入`sudo apt-get update && sudo apt-get upgrade`

#### **设置简易防火墙aptables**
1. `sudo iptables -F` 清空目前iptables的所有规则
2. 采用编辑配置文件的方式设置iptables的规则，输入`sudo vi /etc/iptables.up.rules`
3. 在打开的文件中输入以下配置。注意，在 #号注释"ssh port login"中，我的登录端口是39999，这里填写之前ssh登录端口修改后的新端口即可。
```
*filter #allow all connections
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

#allow out traffic
-A OUTPUT -j ACCEPT

#allow http https
-A INPUT -p tcp --dport 443 -j ACCEPT

-A INPUT -p tcp --dport 80 -j ACCEPT

#allow ssh port login
-A INPUT -p tcp -m state --state NEW --dport 39999 -j ACCEPT

#allow ping
-A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT

#log denied calls
-A INPUT -m limit --limit 5/min -j LOG --log-prefix "iptables denied:" --log-level 7

#drop incoming sensitive connetions
-A INPUT -p tcp --dport 80 -i eth0 -m state --state NEW -m recent --set
-A INPUT -p tcp --dport 80 -i eth0 -m state --state NEW -m recent --update --seconds 60 --hitcount 150 -j DROP
#reject all other inbound
-A INPUT -j REJECT
-A FORWARD -j REJECT

COMMIT
```
这里面的设置含义以后再去看看，现在先copy再说。^_^

-------

上面的搞定之后在保存退出来，输入`sudo iptables-restore < /etc/iptables.up.rules`使设置生效。
可以同过`sudo ufw status`来监测防火墙是否打开。
使用`sudo ufw enable`打开防火墙


#### **设置防火墙开机自动开启**
输入```sudo vi /etc/network/if-up.d/iptables```，编写如下脚本
```

    #!/bin/sh
    iptables-restore /etc/iptables.up.rules

```
wq!保存退出后，在命令行中输入`chmod +x /etc/network/if-up.d/iptables`提升权限
