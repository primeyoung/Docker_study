# 安装命令
```
docker run --name gopeed -d -p 10001:9999 -v /usr/share/Downloads:/root/Downloads -v /usr/share/docker-data/gopeed:/app/storage liwei2633/gopeed -u admin -p 123456
```
-v 后面一个是下载地址，一个是数据地址，先创对应的映射文件夹

## 百度网盘拓展：
```
https://github.com/monkeyWie/gopeed-extension-baiduwp
```
