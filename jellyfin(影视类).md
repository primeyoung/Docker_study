# 1.准备工作

## 1.1 工具准备


## 1.2 目录创建
docker目录
```
mkdir -p /usr/share/docker-data/jellyfin
```

电影目录
```
mkdir -p /usr/share/Downloads/mv
```

电视剧目录
```
mkdir -p /usr/share/Downloads/tv
```

```
mkdir -p /usr/share/Downloads/comics
```

# 2.安装
```
cd /usr/share/docker-data/jellyfin
```

```
nano docker-compose.yml
```

```
version: "2.1"
services:
  jellyfin:
    image: lscr.io/linuxserver/jellyfin:latest
    container_name: jellyfin
    environment:
      - PUID=0     #id查看
      - PGID=0     #id查看
      - TZ=Asia/Shanghai
    volumes:
      - /usr/share/docker-data/jellyfin:/config
      - /usr/share/Downloads/mv:/data/movies
      - /usr/share/comics:/data/comics
    ports:
      - 8096:8096
      - 7359:7359/udp #客户端在本地网络发现
    restart: unless-stopped
```

```
docker-compose up -d
```
