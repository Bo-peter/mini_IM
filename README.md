# MINI_IM
> 这是一个基于muduo网络库，Nginx 和Redis 的及时通讯系统。使用之前需要安装muduo网络库，Nginx(Tcp负载均衡模块) 和 Redis以及Redis的开发库。

### 使用方法
```shell
#自动化脚本编译
./autobuild.sh
```
在bin目录下生成 ChatClient 和 ChatServer 
>单机可以运行，当然你也可以配置Nginx的Tcp负载均衡,部署为集群服务

```shell
#单机执行
./ChatServer 127.0.0.1 6000

./ChatClient 127.0.0.1 6000

#集群需要配置Nginx配置文件
#nginx.conf文件内容需要添加:
stream{
        upstream MyServer{
                server 127.0.0.1:6000 weight=1 max_fails=3 fail_timeout=30s;
                server 127.0.0.1:6001 weight=1 max_fails=3 fail_timeout=30s;
        }
        server{
                proxy_connect_timeout 1s;
                #proxy_timeout 3s;
                listen 8000;
                proxy_pass MyServer;
                tcp_nodelay on;
        }
}
#nginx 默认端口为8000
#集群执行 

./ChatServer 127.0.0.1 6000
./ChatServer 127.0.0.1 6001

./ChatClient 127.0.0.1 8000
./ChatClient 127.0.0.1 8000
```

