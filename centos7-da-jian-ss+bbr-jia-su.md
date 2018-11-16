---
description: Centos7搭建ss+bbr加速Centos7+Shadowsocks+bbr
---

# Centos7搭建ss+bbr加速

#### 搭建 bbr 服务

*  使用root用户登录，运行以下命令：

```bash
# wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
# chmod +x bbr.sh
# ./bbr.sh
```

* 安装完成后，脚本会提示需要重启 VPS，输入 y 并回车后重启。

  重启完成后，进入 VPS，验证一下是否成功安装最新内核并开启 TCP [BBR](https://mrhee.com/tag/bbr)，输入以下命令：

```bash
# uname -r
```

*  查看BBR是否启用，输入以下命令：

```bash
# lsmod | grep bbr
```

#### 搭建 Shadowsocks 服务

* 安装组件

```bash
# yum install m2crypto python-setuptools
# easy_install pip
# pip install shadowsocks
1
2
3
# yum install m2crypto python-setuptools
# easy_install pip
# pip install shadowsocks
```

* 安装完成后配置服务器参数

```bash
# vi /etc/shadowsocks.json
```

```text
{
"server":"0.0.0.0",
"local_address": "127.0.0.1",
"local_port":1080,
"port_password": {
"8381":"xinmin1",
"8382":"xinmin2",
"8383":"xinmin3",
"8384":"xinmin4"
},
"timeout":300,
"method":"aes-256-cfb",
"fast_open": false
}
```

* 配置防火墙

```bash
# 安装防火墙
yum install firewalld
# 启动防火墙
systemctl start firewalld
# 安装防火墙
yum install firewalld
# 启动防火墙
systemctl start firewalld
```

* 开启防火墙相应端口

```bash
# 端口号是你自己设置的端口
# firewall-cmd --permanent --zone=public --add-port=443/tcp
# firewall-cmd --permanent --zone=public --add-port=80/tcp
# firewall-cmd --permanent --zone=public --add-port=1080/tcp
# firewall-cmd --permanent --zone=public --add-port=22/tcp
# firewall-cmd --permanent --zone=public --add-port=8381/tcp
# firewall-cmd --permanent --zone=public --add-port=8382/tcp
# firewall-cmd --reload
```

* 启动Shadowsocks服务器

```bash
# 后台运行    
ssserver -c /etc/shadowsocks.json -d start
```



