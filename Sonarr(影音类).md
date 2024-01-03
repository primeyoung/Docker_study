# 1.准备
```
mkdir -p /usr/share/docker-data/sonarr
```

```
mkdir -p /usr/share/Downloads/sonarr-downloads
```

# 2.安装
```
cd /usr/share/docker-data/sonarr
```

```
nano docker-compose.yml
```

```
---
version: "2.1"
services:
  sonarr:
    image: lscr.io/linuxserver/sonarr:latest
    container_name: sonarr
    environment:
      - PUID=0
      - PGID=0
      - TZ:=Asia/Shanghai
    volumes:
      - /usr/share/docker-data/sonarr:/config
      - /usr/share/Downloads/tv:/tv #optional
      - /usr/share/Downloads/sonarr-downloads:/downloads #optional
    ports:
      - 18989:8989
    restart: unless-stopped
```

```
docker-compose.yml up -d
```
