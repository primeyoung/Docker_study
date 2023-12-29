# 项目地址
https://github.com/Kareadita/Kavita

## 1.准备安装目录
```
mkdir -p /usr/share/docker-data/kavita
```

```
mkdir -p /usr/share/Downloads/manga
```

切换目录
```
cd /usr/share/docker-data/kavita
```

```
nano docker-compose.yml
```

```
version: '3.9'
services:
    kavita:
        image: jvmilazz0/kavita:latest
        volumes:
            - /usr/share/Downloads/manga :/manga   #漫画目录
            - /usr/share/docker-data/kavita :/kavita/config   #应用日志
        ports:
            - "5000:5000"  #前面的5000修改成自己想要的端口号（端口的取值范围是：0-65535）
        restart: unless-stopped
```
## 2.安装
```
docker-compose up -d
```

以本文为例访问ip:5000,创建账号
