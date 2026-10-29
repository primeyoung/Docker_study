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
git clone https://github.com/immortalwrt/immortalwrt.git
cd immortalwrt
```

#### 分支切换
```
git checkout openwrt-25.12
```

#### 更新
```
./scripts/feeds update -a
./scripts/feeds install -a
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
make download -j$(nproc)
```



### 编译后
编译完成后，你的电犀牛 R68S 固件会存放在：
📂 bin/targets/rockchip/armv8/


## 下载脚本加速
```
wget -O /usr/bin/openclash-menu https://ghproxy.net && chmod +x /usr/bin/openclash-menu && openclash-menu
```


还不行就输出下面内容
```
nano /usr/bin/openclash-menu
```

```
#!/bin/sh
set -eu

REPO="slobys/openclash-auto-installer"
BRANCH="main"
DEFAULT_BASE_URL="https://raw.githubusercontent.com/$REPO/$BRANCH"
BASE_URL="${OPENCLASH_AUTO_BASE_URL:-$DEFAULT_BASE_URL}"
RESOLVED_BASE_URL=""
TMP_SCRIPT="/tmp/openclash-menu-action.sh"
NONINTERACTIVE_ACTION=""

log() {
    printf '%s\n' "==> $*"
}

die() {
    printf '%s\n' "[ERROR] $*" >&2
    exit 1
}

need_cmd() {
    command -v "$1" >/dev/null 2>&1 || die "缺少命令: $1"
}

need_downloader() {
    command -v curl >/dev/null 2>&1 && return 0
    command -v wget >/dev/null 2>&1 && return 0
    die "缺少 curl 或 wget，无法下载脚本"
}

download_file() {
    URL="$1"
    OUT="$2"

    if command -v curl >/dev/null 2>&1; then
        curl -fsSL --retry 3 --connect-timeout 15 "$URL" -o "$OUT" && return 0
        curl -kfsSL --retry 2 --connect-timeout 15 "$URL" -o "$OUT" && return 0
    fi

    if command -v wget >/dev/null 2>&1; then
        wget -qO "$OUT" "$URL" && return 0
        wget --no-check-certificate -qO "$OUT" "$URL" && return 0
    fi

    return 1
}

usage() {
    cat <<'EOF_USAGE'
用法:
  sh menu.sh
  sh menu.sh --check-all-updates
  sh menu.sh --check-updates
  sh menu.sh --check-update-openclash
  sh menu.sh --check-update-passwall
  sh menu.sh --check-update-passwall2
  sh menu.sh --check-update-nikki
  sh menu.sh --check-update-smartdns
  sh menu.sh --check-update-mosdns
  sh menu.sh --check-update-daed
  sh menu.sh --openclash
  sh menu.sh --openclash-check-update
  sh menu.sh --openclash-plugin-only
  sh menu.sh --openclash-core-only
  sh menu.sh --openclash-meta-core
  sh menu.sh --openclash-smart-core
  sh menu.sh --passwall
  sh menu.sh --passwall2
  sh menu.sh --nikki
  sh menu.sh --smartdns
  sh menu.sh --mosdns
  sh menu.sh --daed
  sh menu.sh --uninstall-passwall
  sh menu.sh --uninstall-passwall2
  sh menu.sh --uninstall-nikki
  sh menu.sh --uninstall-smartdns
  sh menu.sh --uninstall-mosdns
  sh menu.sh --uninstall-daed
  sh menu.sh --uninstall-openclash

说明:
  不带参数时进入交互菜单
  带参数时直接执行对应动作，适合非交互环境
EOF_USAGE
}

parse_args() {
    while [ "$#" -gt 0 ]; do
        case "$1" in
            --openclash)
                NONINTERACTIVE_ACTION="openclash"
                ;;
            --check-all-updates)
                NONINTERACTIVE_ACTION="check-all-updates"
                ;;
            --check-updates)
                NONINTERACTIVE_ACTION="check-updates"
                ;;
            --check-update-openclash)
                NONINTERACTIVE_ACTION="check-update-openclash"
                ;;
            --check-update-passwall)
                NONINTERACTIVE_ACTION="check-update-passwall"
                ;;
            --check-update-passwall2)
                NONINTERACTIVE_ACTION="check-update-passwall2"
                ;;
            --check-update-nikki)
                NONINTERACTIVE_ACTION="check-update-nikki"
                ;;
            --check-update-smartdns)
                NONINTERACTIVE_ACTION="check-update-smartdns"
                ;;
            --check-update-mosdns)
                NONINTERACTIVE_ACTION="check-update-mosdns"
                ;;
            --check-update-daed)
                NONINTERACTIVE_ACTION="check-update-daed"
                ;;
            --openclash-check-update)
                NONINTERACTIVE_ACTION="openclash-check-update"
                ;;
            --openclash-plugin-only)
                NONINTERACTIVE_ACTION="openclash-plugin-only"
                ;;
            --openclash-core-only)
                NONINTERACTIVE_ACTION="openclash-core-only"
                ;;
            --openclash-meta-core)
                NONINTERACTIVE_ACTION="openclash-meta-core"
                ;;
            --openclash-smart-core)
                NONINTERACTIVE_ACTION="openclash-smart-core"
                ;;
            --passwall)
                NONINTERACTIVE_ACTION="passwall"
                ;;
            --passwall2)
                NONINTERACTIVE_ACTION="passwall2"
                ;;
            --nikki)
                NONINTERACTIVE_ACTION="nikki"
                ;;
            --smartdns)
                NONINTERACTIVE_ACTION="smartdns"
                ;;
            --mosdns)
                NONINTERACTIVE_ACTION="mosdns"
                ;;
            --daed)
                NONINTERACTIVE_ACTION="daed"
                ;;
            --uninstall-passwall)
                NONINTERACTIVE_ACTION="uninstall-passwall"
                ;;
            --uninstall-passwall2)
                NONINTERACTIVE_ACTION="uninstall-passwall2"
                ;;
            --uninstall-nikki)
                NONINTERACTIVE_ACTION="uninstall-nikki"
                ;;
            --uninstall-smartdns)
                NONINTERACTIVE_ACTION="uninstall-smartdns"
                ;;
            --uninstall-mosdns)
                NONINTERACTIVE_ACTION="uninstall-mosdns"
                ;;
            --uninstall-daed)
                NONINTERACTIVE_ACTION="uninstall-daed"
                ;;
            --uninstall-openclash)
                NONINTERACTIVE_ACTION="uninstall-openclash"
                ;;
            -h|--help)
                usage
                exit 0
                ;;
            *)
                die "未知参数: $1"
                ;;
        esac
        shift
    done
}

resolve_base_url() {
    if [ -n "$RESOLVED_BASE_URL" ]; then
        printf '%s' "$RESOLVED_BASE_URL"
        return 0
    fi

    if [ "${OPENCLASH_AUTO_BASE_URL:-}" != "" ]; then
        RESOLVED_BASE_URL="$BASE_URL"
        printf '%s' "$RESOLVED_BASE_URL"
        return 0
    fi

    COMMIT_JSON="/tmp/openclash-menu-commit.json"
    rm -f "$COMMIT_JSON"
    LATEST_SHA=""
    if download_file "https://api.github.com/repos/$REPO/commits/$BRANCH" "$COMMIT_JSON"; then
        LATEST_SHA="$(sed -n 's/.*"sha":[[:space:]]*"\([0-9a-f]\{40\}\)".*/\1/p' "$COMMIT_JSON" | head -n1 || true)"
        rm -f "$COMMIT_JSON"
    fi
    if [ -n "$LATEST_SHA" ]; then
        RESOLVED_BASE_URL="https://raw.githubusercontent.com/$REPO/$LATEST_SHA"
    else
        RESOLVED_BASE_URL="$BASE_URL"
    fi

    printf '%s' "$RESOLVED_BASE_URL"
}

download_and_run() {
    SCRIPT_NAME="$1"
    shift || true
    
    # 优先使用本地智能版本
    if [ -f "scripts/$SCRIPT_NAME" ]; then
        log "使用本地智能版本: scripts/$SCRIPT_NAME"
        sh "scripts/$SCRIPT_NAME" "$@"
        return
    elif [ -f "$SCRIPT_NAME-smart.sh" ]; then
        log "使用本地智能版本: $SCRIPT_NAME-smart.sh"
        sh "$SCRIPT_NAME-smart.sh" "$@"
        return
    fi
    
    URL="$(resolve_base_url)/$SCRIPT_NAME"

    log "下载脚本: $URL"
    download_file "$URL" "$TMP_SCRIPT" || die "下载脚本失败: $SCRIPT_NAME"
    [ -s "$TMP_SCRIPT" ] || die "下载的脚本为空: $SCRIPT_NAME"
    chmod +x "$TMP_SCRIPT"
    sh "$TMP_SCRIPT" "$@"
}

run_interactive_script() {
    # Keep a failed child action (including download errors) inside the menu.
    if (download_and_run "$@"); then
        return 0
    else
        ACTION_STATUS="$?"
        printf '%s\n' "[ERROR] 操作失败（退出码 $ACTION_STATUS），请保留上方日志；本次未确认完成，可返回菜单重试" >&2
    fi
}

show_menu() {
    cat <<'EOF_MENU'
================ 代理插件管理菜单 ================
1. 检查插件更新
2. 安装插件
3. 卸载插件
0. 退出
==================================================
EOF_MENU
}

show_install_menu() {
    cat <<'EOF_INSTALL_MENU'
================ 安装插件 ================
1. 安装 / 更新 OpenClash（自动识别 Meta / Smart）
2. 只更新 OpenClash 插件
3. 只安装 OpenClash 核心（自动识别 Meta / Smart）
4. 只安装 OpenClash 普通 Meta 内核
5. 只安装 OpenClash Smart Meta 内核
6. 安装 / 更新 PassWall
7. 安装 / 更新 PassWall2
8. 安装 / 更新 Nikki
9. 安装 / 更新 SmartDNS
10. 安装 / 更新 MosDNS
11. 安装 / 更新 daed
0. 返回上一级
==========================================
EOF_INSTALL_MENU
}

show_uninstall_menu() {
    cat <<'EOF_UNINSTALL_MENU'
================ 卸载插件 ================
1. 卸载 PassWall
2. 卸载 PassWall2
3. 卸载 Nikki
4. 卸载 SmartDNS
5. 卸载 MosDNS
6. 卸载 OpenClash
7. 卸载 daed
0. 返回上一级
==========================================
EOF_UNINSTALL_MENU
}

show_check_update_menu() {
    cat <<'EOF_CHECK_MENU'
================ 检查插件更新 ================
1. 检查所有插件
2. 检查 OpenClash
3. 检查 PassWall
4. 检查 PassWall2
5. 检查 Nikki
6. 检查 SmartDNS
7. 检查 MosDNS
8. 检查 daed
0. 返回上一级
==============================================
EOF_CHECK_MENU
}

read_from_tty() {
    if [ -r /dev/tty ]; then
        read -r "$1" </dev/tty
    else
        die "当前环境不可交互，请改用非交互参数模式"
    fi
}

run_action() {
    action="$1"
    case "$action" in
        1|check-updates)
            run_check_update_menu
            SKIP_MAIN_PAUSE="1"
            ;;
        check-all-updates)
            download_and_run check-updates.sh
            ;;
        check-update-openclash)
            download_and_run check-updates.sh --openclash
            ;;
        check-update-passwall)
            download_and_run check-updates.sh --passwall
            ;;
        check-update-passwall2)
            download_and_run check-updates.sh --passwall2
            ;;
        check-update-nikki)
            download_and_run check-updates.sh --nikki
            ;;
        check-update-smartdns)
            download_and_run check-updates.sh --smartdns
            ;;
        check-update-mosdns)
            download_and_run check-updates.sh --mosdns
            ;;
        check-update-daed)
            download_and_run check-updates.sh --daed
            ;;
        2|install-plugins)
            run_install_menu
            SKIP_MAIN_PAUSE="1"
            ;;
        3|uninstall-plugins)
            run_uninstall_menu
            SKIP_MAIN_PAUSE="1"
            ;;
        openclash)
            download_and_run install.sh
            ;;
        openclash-check-update)
            download_and_run install.sh --check-update --skip-pkg-update
            ;;
        openclash-plugin-only)
            download_and_run install.sh --plugin-only
            ;;
        openclash-core-only)
            download_and_run install.sh --core-only
            ;;
        openclash-meta-core)
            download_and_run install.sh --core-only --meta-core --skip-pkg-update
            ;;
        openclash-smart-core)
            download_and_run install.sh --core-only --smart-core --skip-pkg-update
            ;;
        passwall)
            download_and_run passwall.sh
            ;;
        passwall2)
            download_and_run passwall2.sh
            ;;
        nikki)
            download_and_run nikki.sh
            ;;
        smartdns)
            download_and_run smartdns.sh
            ;;
        mosdns)
            download_and_run mosdns.sh
            ;;
        daed)
            download_and_run daed.sh
            ;;
        uninstall-passwall)
            download_and_run uninstall.sh passwall --delete-config
            ;;
        uninstall-passwall2)
            download_and_run uninstall.sh passwall2 --delete-config
            ;;
        uninstall-nikki)
            download_and_run uninstall.sh nikki --delete-config
            ;;
        uninstall-smartdns)
            download_and_run uninstall.sh smartdns --delete-config
            ;;
        uninstall-mosdns)
            download_and_run uninstall.sh mosdns --delete-config
            ;;
        uninstall-daed)
            download_and_run uninstall.sh daed --delete-config
            ;;
        uninstall-openclash)
            download_and_run uninstall.sh openclash --delete-config
            ;;
        0)
            log "已退出"
            exit 0
            ;;
        *)
            printf '%s\n' '[WARN] 无效选项，请重新输入'
            ;;
    esac
}

run_check_update_menu() {
    subchoice=""
    while true; do
        show_check_update_menu
        printf '请输入选项 [0-8]: ' >/dev/tty
        read_from_tty subchoice
        case "$subchoice" in
            1)
                run_interactive_script check-updates.sh
                ;;
            2)
                run_interactive_script check-updates.sh --openclash
                ;;
            3)
                run_interactive_script check-updates.sh --passwall
                ;;
            4)
                run_interactive_script check-updates.sh --passwall2
                ;;
            5)
                run_interactive_script check-updates.sh --nikki
                ;;
            6)
                run_interactive_script check-updates.sh --smartdns
                ;;
            7)
                run_interactive_script check-updates.sh --mosdns
                ;;
            8)
                run_interactive_script check-updates.sh --daed
                ;;
            0)
                return 0
                ;;
            *)
                printf '%s\n' '[WARN] 无效选项，请重新输入'
                ;;
        esac
        printf '\n按回车键返回检查插件更新菜单...' >/dev/tty
        read_from_tty _subdummy
        printf '\n'
    done
}

run_install_menu() {
    while true; do
        show_install_menu
        printf '请输入选项 [0-11]: ' >/dev/tty
        read_from_tty subchoice
        case "$subchoice" in
            1)
                run_interactive_script install.sh
                ;;
            2)
                run_interactive_script install.sh --plugin-only
                ;;
            3)
                run_interactive_script install.sh --core-only
                ;;
            4)
                run_interactive_script install.sh --core-only --meta-core --skip-pkg-update
                ;;
            5)
                run_interactive_script install.sh --core-only --smart-core --skip-pkg-update
                ;;
            6)
                run_interactive_script passwall.sh
                ;;
            7)
                run_interactive_script passwall2.sh
                ;;
            8)
                run_interactive_script nikki.sh
                ;;
            9)
                run_interactive_script smartdns.sh
                ;;
            10)
                run_interactive_script mosdns.sh
                ;;
            11)
                run_interactive_script daed.sh
                ;;
            0)
                return 0
                ;;
            *)
                printf '%s\n' '[WARN] 无效选项，请重新输入'
                ;;
        esac
        printf '\n按回车键返回安装插件菜单...' >/dev/tty
        read_from_tty _subdummy
        printf '\n'
    done
}

run_uninstall_menu() {
    while true; do
        show_uninstall_menu
        printf '请输入选项 [0-7]: ' >/dev/tty
        read_from_tty subchoice
        case "$subchoice" in
            1)
                run_interactive_script uninstall.sh passwall --delete-config
                ;;
            2)
                run_interactive_script uninstall.sh passwall2 --delete-config
                ;;
            3)
                run_interactive_script uninstall.sh nikki --delete-config
                ;;
            4)
                run_interactive_script uninstall.sh smartdns --delete-config
                ;;
            5)
                run_interactive_script uninstall.sh mosdns --delete-config
                ;;
            6)
                run_interactive_script uninstall.sh openclash --delete-config
                ;;
            7)
                run_interactive_script uninstall.sh daed --delete-config
                ;;
            0)
                return 0
                ;;
            *)
                printf '%s\n' '[WARN] 无效选项，请重新输入'
                ;;
        esac
        printf '\n按回车键返回卸载插件菜单...' >/dev/tty
        read_from_tty _subdummy
        printf '\n'
    done
}

main() {
    parse_args "$@"
    need_downloader
    choice=""

    if [ -n "$NONINTERACTIVE_ACTION" ]; then
        run_action "$NONINTERACTIVE_ACTION"
        exit 0
    fi

    while true; do
        show_menu
        printf '请输入选项 [0-3]: ' >/dev/tty
        read_from_tty choice
        SKIP_MAIN_PAUSE="0"
        run_action "$choice"
        if [ "$SKIP_MAIN_PAUSE" != "1" ]; then
            printf '\n按回车键返回菜单...' >/dev/tty
            read_from_tty _dummy
            printf '\n'
        fi
    done
}

main "$@"
```

然后赋权
```
chmod +x /usr/bin/openclash-menu && openclash-menu
```


























