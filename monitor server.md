# 分为单机版的Ward和多服务器版本的ServerStatus

## 一、Ward

### 1.准备工作
```
mkdir -p /usr/share/docker-data/Ward
```

切换目录
```
cd /usr/share/docker-data/Ward
```

下载并生成镜像
```
git clone https://github.com/AntonyLeons/Ward.git
```

```
cd Ward
```

```
docker build . --tag ward
```

### 2.运行
```
docker run -d --name ward -p 4000:4000 \
-p 自定义端口号:自定义端口号 \
--privileged=true \
--restart always \
ward:latest
```
Server Name 随便填，这个就是搭建完后的浏览器地址栏信息；然后可以选择命令主题或者黑暗主题；Application Port 要填写成刚刚创建 Docker 容器时的自定义端口号。然后点击 LAUNCH 按钮即可完成基本设置
