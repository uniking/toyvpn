原理:
1, 通过socket连接服务器
2, 发送0开头的控制数据, 获取客户端配置
3, 通过客户端配置建立vpn的接口ParcelFileDescriptor iface
4, 读取iface获得ip包, 通过socket直接发送到服务器, 服务器转发这个ip包
5, 通过socket读取服务器, 将服务器返回的ip包直接写入iface

