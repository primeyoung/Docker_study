# 1.准备安装目录
```
mkdir -p /usr/share/docker-data/frps
```

```
cd /usr/share/docker-data/frps
```

```
touch frps.ini
```

```
nano docker-compose.yml
```

```
version: '3.3'
services:
    frps:
        restart: always
        network_mode: host
        volumes:
            - './frps.ini:/etc/frp/frps.ini'
        container_name: frps
        image: snowdreamtech/frps
```

# 2.建立服务
```
docker-compose up -d
```

# 3.之前创建的frps.ini编辑
```
nano frps.ini
```
下面内容复制进去
```
[common]

#frp 监听端口，与客户端绑定端口

bind_port= 5443
kcp_bind_port = 5443


#dashboard用户名

dashboard_user= admin

#dashboard密码

dashboard_pwd= saynihao

#dashboard端口，启动成功后可通过浏览器访问如http://ip:9527

dashboard_port= 9527

#设置客户端token，对应客户端有页需要配置一定要记住，如果客户端不填写你连不上服务端

token = 8ad3d1x429a2d
```

重启服务
```
docker-compose restart
```
