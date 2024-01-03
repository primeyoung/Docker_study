# 1.准备
```
mkdir -p /usr/share/docker-data/homeassistant
```

```
mkdir -p /usr/share/docker-data/homeassistant/etc/locatime
```

```
mkdir -p /usr/share/docker-data/homeassistant/run/dbus
```

# 2.安装
```
cd mkdir -p /usr/share/docker-data/homeassistant
```

```
nano docker-compose.yml
```

```
version: '3'
services:
  homeassistant:
    container_name: homeassistant
    image: "ghcr.io/home-assistant/home-assistant:stable"
    volumes:
      - /usr/share/docker-data/homeassistant:/config
      - /usr/share/docker-data/homeassistant/etc/locatime:/etc/localtime:ro
      - /usr/share/docker-data/homeassistant/run/dbus:/run/dbus:ro
    ports:
      - "18123:8123"  # 8123前面的端口号自己改
    restart: unless-stopped
    privileged: true
```

```
docker-compose up -d
```
