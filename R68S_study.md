# 开源地址：https://github.com/istoreos

## 编译
```
固件地址：https://github.com/immortalwrt/immortalwrt
```


### 第一阶段：搭建编译环境
```
sudo apt update -y && sudo apt upgrade -y
sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
git gperf haveged lzma lcmfg ncurses-dev libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
libmpc-dev libmpfr-dev libmsvcrt-dev libncurses5-dev libncursesw5-dev libpython3-dev libreadline-dev \
libssl-dev libtool lrzsz mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python3 \
python3-pyelftools python3-setuptools qemu-utils rsync scons squashfs-tools subversion swig \
texinfo u-boot-tools upx-ucl unzip vim wget xmlto xz-utils zlib1g-dev apktool
```

### 第二阶段：获取 ImmortalWrt 源码与分支切换

#### 克隆源码
```
sudo git clone https://github.com/immortalwrt/immortalwrt.git
sudo cd immortalwrt
```

#### 分支切换
```
sudo git checkout openwrt-25.12
```

#### 更新
```
sudo ./scripts/feeds update -a
sudo ./scripts/feeds install -a
```

### 第三阶段：配置电犀牛 R68S 专属固件参数
```
make menuconfig
```

在图形化界面中，针对电犀牛 R68S 芯片与型号进行精准勾选（使用键盘空格键选中）：
1. Target System (目标系统) → 选择 Rockchip
2. Subtarget (子目标机器架构) → 选择 ARMv8 64-bit machines
3. Target Profile (设备型号目标) → 寻找并选择 FastRhino R68S（电犀牛官方英文名称为 FastRhino）
4. Target Images (固件镜像格式) → 建议确保勾选了 squashfs（如果你需要安装到 eMMC 或外置存储，也可以顺便勾选 ext4）
可选配置（LuCI 插件）：
• 进入 LuCI → Applications。
• ImmortalWrt 25.12 拥有针对国内环境优化的全新插件（如 luci-app-mihomo / luci-app-homeproxy 等现代科学上网核心组件）。由于 R68S 拥有 4GB DDR4 内存和强劲的 RK3568 CPU，你可以放心勾选各类重度插件。
配置完成后，按左右方向键选择 Save，保存为 .config 退出

### 第四阶段：下载依赖与首次单线程编译
#### 预下载源码包 (DL库)：
```
sudo make download -j$(nproc)
```

#### 首次编译（由于 25.12 采用了全新内核和 apk 包管理器机制，强烈建议首次使用单线程）：
```
sudo make -j1 V=s
```
时间会很久，可能几小时

后续如果进行二次修改编译，可以使用多线程提速：
```
make -j$(nproc)
```

### 编译后
编译完成后，你的电犀牛 R68S 固件会存放在：
📂 bin/targets/rockchip/armv8/









