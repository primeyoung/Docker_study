# 以p3terx大佬制作的为参考

## 1.下载镜像
```
sudo docker pull p3terx/aria2-pro
```

## 2.准备工作
### 2.1 创建映射的日志
```
mkdir -p /usr/share/docker-data/aria2
```
### 2.2 创建映射的下载目录
```
mkdir -p /usr/share/Downloads
```

## 3.制作docker-compose
### 3.1 先决条件
```
mkdir -p /usr/yang/docker/aria2
```
### 3.2 切换到目录
```
cd /usr/yang/docker/aria2
```

### 3.3 创建aria2的docker-compose(默认你安装了nano,没有自己装一下)
```
nano docker-compose.yml
```

### 3.4 将下面内容复制进去
```
version: "3"
services:
  aria2-pro:
    image: p3terx/aria2-pro:latest
    container_name: aria2-pro
    environment:
      - PUID=0   # id查看
      - PGID=0
      - TZ=Asia/Shanghai  # 时区，自己看着设置
      - UMASK_SET=022
      - RPC_SECRET=yy1993     # 密码自己设置6位
      - RPC_PORT=6800
      - LISTEN_PORT=6888
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
    volumes:
      - /usr/share/docker-data/aria2:/config   # 需要映射的日志目录
      - /usr/share/Downloads:/downloads   # 需要映射的下载目录
    ports:
      - 6800:6800
      - 6888:6888
      - 6888:6888/udp
    restart: unless-stopped
```

### 3.5 运行docker-compose
```
docker-compose up -d
```


# Aria2ng
镜像网址
```
https://hub.docker.com/r/iuboy/aria2ng
```

```
docker run -d \--restart=always \--name aria2ng \-p 6801:8080 iuboy/aria2ng:latest
# 直接执行
```
