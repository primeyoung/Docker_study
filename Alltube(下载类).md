# 安装步骤

## 1.创建目录
```
mkdir -p /usr/share/docker-data/alltube
```

```
cd /usr/share/docker-data/alltube
```

```
nano docker-compose.yml
```

```
version: '3.3'
services:
    alltube:
        container_name: alltube
        ports:
            - '5993:80'   # 5993可以改成任意vps上未使用过的端口，80不要改
        environment:
            - PUID=0    # 稍后在终端输入id可以查看当前用户的id
            - PGID=0    # 同上
            - TZ=Asia/Shanghai
        restart: always
        image: rudloff/alltube
```

```
docker-compose up -d 
```
