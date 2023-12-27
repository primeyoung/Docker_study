# update 2023/12/26
## 镜像地址
```
https://hub.docker.com/r/linuxserver/qbittorrent
```

## 先建一个映射的日志日志
```
mkdir -p /usr/share/docker-data/qbittorrent
```

## 创建qbittorrent的docker-compose.yml
```
mkdir -p /usr/yang/docker/qbittorrent
```

```
cd /usr/yang/docker/qbittorrent
```

```
nano docker-compose.yml
```

```
version: "2.1"
services:
  qbittorrent:
    image:  lscr.io/linuxserver/qbittorrent:latest
    container_name: qbittorrent
    environment:
      - PUID=0
      - PGID=0
      - TZ=Asia/Shanghai
      - UMASK_SET=022
      - WEBUI_PORT=9600   # 9600是管理页面端口
    volumes:
      - /usr/share/docker-data/qbittorrent:/config
      - /usr/share/Downloads:/downloads
    ports:
      # 映射下载端口与内部下载端口,后面可改。
      - 6881:6881
      - 6881:6881/udp
      - 9600:8080
    restart: unless-stopped
```

```
docker-compose up -d
```

## 备注：账号还是默认admin，但是密码需要从日志里面看一下随机生成的密码
* <img width="210" alt="qb1" src="https://github.com/primeyoung/Docker_study/assets/48234143/c5eea276-1134-4288-a2aa-c3ed30fb8a0f">  

* <img width="493" alt="qb2" src="https://github.com/primeyoung/Docker_study/assets/48234143/e96d0e89-48b7-47d8-97e2-3d2737d744ba">  

* <img width="605" alt="qb3" src="https://github.com/primeyoung/Docker_study/assets/48234143/045ebada-0fee-4e15-a94d-56dc2c7f20af">

