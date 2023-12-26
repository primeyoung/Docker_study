![image](https://github.com/primeyoung/Docker_study/assets/48234143/7c848c0f-04a8-4ab5-ac13-ad80b127c518)# update 2023/12/26
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
      - WEBUI_PORT=9600    #9600是管理页面端口
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
