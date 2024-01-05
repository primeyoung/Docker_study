#  密钥环
```
apt install --yes gnupg
wget -O "/etc/apt/trusted.gpg.d/openmediavault-archive-keyring.asc" https://packages.openmediavault.org/public/archive.key
apt-key add "/etc/apt/trusted.gpg.d/openmediavault-archive-keyring.asc"
```

# 清华源加速
```
cat <<EOF > /etc/apt/sources.list.d/openmediavault.list
deb https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/public shaitan main
deb https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/packages shaitan main
## Uncomment the following line to add software from the proposed repository.
# deb https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/public shaitan-proposed main
# deb https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/packages shaitan-proposed main
## This software is not part of OpenMediaVault, but is offered by third-party
## developers as a service to OpenMediaVault users.
# deb https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/public shaitan partner
# deb https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/packages shaitan partner
EOF
```

# 设置语言以及API等默认环境
```
export LANG=C.UTF-8
export DEBIAN_FRONTEND=noninteractive
export APT_LISTCHANGES_FRONTEND=none
timedatectl set-timezone Asia/Shanghai
```

# 添加php库源并安装旧版php
```
apt install -y apt-transport-https lsb-release ca-certificates wget 
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php.list
apt update
apt install -y php7.4 libapache2-mod-php7.4 libphp7.4-embed php7.4-cgi php7.4-cli php7.4-fpm php7.4-phpdbg postfix=3.7.6-0+deb12u2 mysql* 
apt purge apache2* -y  && apt autoremove -y && apt install -y nginx
```

# 更新软件包源并安装omv
```
apt update
apt-get --yes --auto-remove --show-upgraded \
    --allow-downgrades --allow-change-held-packages \
    --no-install-recommends \
    --option DPkg::Options::="--force-confdef" \
    --option DPkg::Options::="--force-confold" \
    install openmediavault-keyring openmediavault
```

# 更新
```
omv-confdbadm populate
```

## 最后在浏览器输入您的IP地址即可，omv的默认端口是80   默认账号：admin,默认密码：openmediavault
