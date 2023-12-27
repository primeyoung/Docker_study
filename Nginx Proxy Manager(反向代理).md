# docker安装Nginx Proxy Manager

## 1、创建安装目录
①创建目录
```
mkdir -p /usr/share/docker-data/npm
```
②切换到对应目录
```
cd /usr/share/docker-data/npm
```
③创建docker-compose.yml文件
```
nano docker-compose.yml
```
④复制以下内容
```
version: '3'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'  # 保持默认即可，不建议修改左侧的80
      - '81:81'  # 冒号左边可以改成自己服务器未被占用的端口
      - '443:443' # 保持默认即可，不建议修改左侧的443
    volumes:
      - ./data:/data # 冒号左边可以改路径，现在是表示把数据存放在在当前文件夹下的 data 文件夹中
      - ./letsencrypt:/etc/letsencrypt  # 冒号左边可以改路径，现在是表示把数据存放在在当前文件夹下的 letsencrypt 文件夹中
```

### 如出现端口问题可以验证一下
```
lsof -i:81  #查看 81 端口是否被占用，如果被占用，重新自定义一个端口
```
上方命令要是反馈-bash: lsof: command not found，请执行下方命令
```
apt install lsof  #安装 lsof
```

## 2、创建并登录
```
docker-compose up -d
```
运行完没报错，就可以使用示例中本机ip:81端口访问了
```
Email:    admin@example.com
Password: changeme
```

## 3、更新NPM
```
cd /usr/share/docker-data/npm

docker-compose down 

cp -r /usr/share/docker-data/npm /usr/share/docker-data/npm.archive  # 万事先备份，以防万一

docker-compose pull

docker-compose up -d    # 请不要使用 docker-compose stop 来停止容器，因为这么做需要额外的时间等待容器停止；docker-compose up -d 直接升级容器时会自动停止并立刻重建新的容器，完全没有必要浪费那些时间。

docker image prune  # prune 命令用来删除不再使用的 docker 对象。删除所有未被 tag 标记和未被容器使用的镜像
```

提示以下内容输入y：
```
WARNING! This will remove all dangling images.
Are you sure you want to continue? [y/N] 
```

## 4、卸载npm
```
cd /usr/share/docker-data/npm

docker-compose down 

rm -rf /usr/share/docker-data/npm  # 完全删除映射到本地的数据
```
