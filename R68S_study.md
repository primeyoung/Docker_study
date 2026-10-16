# 开源地址：https://github.com/istoreos

## 方案1：软件中心解锁

1.  登录 iStoreOS 后台，点击左侧导航栏的 “终端”（或者通过 SSH 工具连接软路由）。
2.  输入以下一键解锁命令并回车（此命令目前最常用，集成了热门的科学上网插件）：
```
sh -c "$(curl -ksS https://githubusercontent.com)"
```

```
sh -c "$(curl -ksS https://jsdelivr.net)"
```

## 方案2：手动上传
或则手动上传.ipk的安装包

### 1、OpenWrt-nikki
从软件源安装
#### 添加源
```
# 只需运行一次
wget -O - https://github.com/nikkinikki-org/OpenWrt-nikki/raw/refs/heads/main/feed.sh | ash
```

#### 安装
```
# 你可以从 shell 执行命令安装或者从 LuCI 的`软件包`菜单安装
# for opkg
opkg install nikki
opkg install luci-app-nikki
opkg install luci-i18n-nikki-zh-cn
# for apk
apk add nikki
apk add luci-app-nikki
apk add luci-i18n-nikki-zh-cn
```

#### 卸载并重置
```
wget -O - https://github.com/nikkinikki-org/OpenWrt-nikki/raw/refs/heads/main/uninstall.sh | ash
```

如果软件源安装方案不行，再试发行版
```
wget -O - https://github.com/nikkinikki-org/OpenWrt-nikki/raw/refs/heads/main/install.sh | ash
```


#### 卸载并重置
```
wget -O - https://github.com/nikkinikki-org/OpenWrt-nikki/raw/refs/heads/main/uninstall.sh | ash
```

