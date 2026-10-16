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

## 方案3：添加仓库源
1.登录 iStoreOS 后台，进入 “系统” -> “软件包”。
2.点击 “配置 opkg”。
3.在“自定义软件源（Custom feeds）”的最下方，另起一行，把 Kenzok8 维护的国内加速流媒体/插件源粘贴进去（它会自动识别你的 R68S 是 ARM64 架构并匹配）：
```
src/gz kenzok8_packages https://mpdn.fun
```
4.点击保存。回到软件包页面，点击 “更新列表”。
5.在下面的搜索框里搜 luci-app-homeproxy 或 luci-app-openclash，点击安装，系统就会100%自动下载符合 R68S（ARM64）框架的核心和所有依赖，比你手动下载 .ipk 上传安全得多

上述是ai给出方案，也可以按照仓库给的一键命令
```
sed -i '1i src-git kenzo https://github.com/kenzok8/openwrt-packages' feeds.conf.default
sed -i '2i src-git small https://github.com/kenzok8/small' feeds.conf.default
./scripts/feeds update -a && rm -rf feeds/luci/applications/luci-app-mosdns
rm -rf feeds/packages/net/{alist,adguardhome,mosdns,xray*,v2ray*,sing*,smartdns} feeds/packages/utils/v2dat feeds/packages/lang/golang
git clone https://github.com/kenzok8/golang -b 1.26 feeds/packages/lang/golang
./scripts/feeds install -a
make menuconfig
```
