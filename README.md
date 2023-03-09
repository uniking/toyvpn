# toyvpn
android官方提供的例子, 放到了android studio

原理:

1, 通过socket连接服务器

2, 发送0开头的控制数据, 获取客户端配置

3, 通过客户端配置建立vpn的接口ParcelFileDescriptor iface

4, 读取iface获得ip包, 通过socket直接发送到服务器, 服务器转发这个ip包

5, 通过socket读取服务器, 将服务器返回的ip包直接写入iface

服务端打开"/dev/net/tun"

创建tunnel的socket

在tun设备和tunnel socket间转发数据

服务端配置
#/bin/bash

iptables -t nat -F
ip tuntap del dev tun0 mode tun

# Enable IP forwarding
echo 1 > /proc/sys/net/ipv4/ip_forward

# Pick a range of private addresses and perform NAT over eth2.
iptables -t nat -A POSTROUTING -s 10.0.0.0/8 -o eth2 -j MASQUERADE

# Create a TUN interface.
ip tuntap add dev tun0 mode tun

# Set the addresses and bring up the interface.
ifconfig tun0 10.0.0.1 dstaddr 10.0.0.2 up

# Create a server on port 61195 with shared secret "mySecret".
./ToyVpnServer tun0 61195 mySecret -m 1400 -a 10.0.0.2 32  -r 192.168.1.8 32

2, 启动服务

