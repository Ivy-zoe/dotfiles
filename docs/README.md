   * [MacbookAir](#macbookair)
      * [1. 描述](#1-描述)
      * [2. 安装](#2-安装)
         * [2.1 环境](#21-环境)
         * [2.2 分区列表](#22-分区列表)
         * [2.3 分区](#23-分区)
         * [2.4 stage3](#24-stage3)
         * [2.5 portage和make.conf](#25-portage和makeconf)
            * [2.5.1 make.conf](#251-makeconf)
            * [2.5.2 portage](#252-portage)
         * [2.6 更新系统](#26-更新系统)
            * [2.6.1 进入chroot环境](#261-进入chroot环境)
            * [2.6.2 同步portage](#262-同步portage)
            * [2.6.3 选择配置文件](#263-选择配置文件)
            * [2.6.4 更新系统](#264-更新系统)
         * [2.7 安装内核](#27-安装内核)
         * [2.8 安装必要软件](#28-安装必要软件)
            * [2.8.1 网络管理](#281-网络管理)
            * [2.8.2 系统扩展](#282-系统扩展)
         * [2.9 配置基本系统](#29-配置基本系统)
            * [2.9.2 主机名](#292-主机名)
            * [2.9.3 acpi](#293-acpi)
         * [2.10 fstab](#210-fstab)
         * [2.11 grub](#211-grub)
         * [2.12 显卡驱动](#212-显卡驱动)
      * [3. 软件列表](#3-软件列表)
            * [3.1 桌面环境](#31-桌面环境)
            * [3.2 输入法](#32-输入法)
            * [3.3 字体](#33-字体)
            * [3.4 浏览器](#34-浏览器)
            * [3.5 系统主题](#35-系统主题)
            * [3.6 ssh客户端](#36-ssh客户端)
            * [3.7 文档](#37-文档)
            * [3.8 思维导图](#38-思维导图)
            * [3.9 KVM](#39-kvm)
            * [3.10 VPN](#310-vpn)
            * [3.11 音频](#311-音频)
            * [3.12 截屏](#312-截屏)
            * [3.13 建模](#313-建模)
            * [3.14 背光](#314-背光)
            * [3.15 文件系统](#315-文件系统)
# MacbookAir

Gentoo on Macbook Air 2017 13-inch

## 1. 描述

因为Mac奇怪的温控然后把我的手指低温烫伤了，所以再见Mac OS（其实是又整了黑苹果

那么这个Mac就来安装一下Gentoo吧23333毕竟Gentoo大法好啊



## 2. 安装

### 2.1 环境



### 2.2 分区列表

| 挂载点    | 文件系统 | 大小 | 磁盘位置(假定) |
| --------- | -------- | ---- | -------------- |
| /boot     | ext2     | 100M | /dev/sda1      |
| /boot/efi | fat32    | 30M  | /dev/sda2      |
| /         | btrfs    | 80G  | /dev/sda3      |
| swap      | swap     | 16G  | /dev/sda4      |
| /data     | btrfs    | all  | /dev/sda5      |





### 2.3 分区



### 2.4 stage3



### 2.5 portage和make.conf



#### 2.5.1 make.conf



example

```sh
# GCC
CHOST="x86_64-pc-linux-gnu"
CFLAGS="-march=skylake -O2 -pipe"
CXXFLAGS="${CFLAGS}"
MAKEOPTS="-j5"
#CPU_FLAGS_X86
# PORTAGE
PORTDIR="/data/portage"
DISTDIR="/data/portage/distfiles"
PKGDIR="/data/portage/packages"
GENTOO_MIRRORS="http://mirrors.tuna.tsinghua.edu.cn/gentoo/"
EMERGE_DEFAULT_OPTS="--ask --verbose=y --keep-going --with-bdeps=y --load-average"
# FEATURES="${FEATURES} -userpriv -usersandbox -sandbox"
PORTAGE_REPO_DUPLICATE_WARN="0"
# PORTAGE_TMPDIR="/var/tmp/notmpfs"
# USE
SUPPORT="pulseaudio bluetooth mtp git chromium"
DESKTOP="infinality emoji cjk"
FUCK="-systemd -modemmanager"
ELSE="client icu sudo python"
USE="${SUPPORT} ${DESKTOP} ${FUCK} ${ELSE}"
# Font
FUCK_FONTS="/usr/share/fonts/arphicfonts"
INSTALL_MASK="${FUCK_FONTS}"

# LICENSED
ACCEPT_KEYWORDS="~amd64"
ACCEPT_LICENSE="*"
# LANGUAGE
L10N="en-US zh-CN en zh"
LINGUAS="en_US zh_CN en zh"
# VIDEO_CARDS
VIDEO_CARDS="intel i965"
# INPUT
INPUT_DEVICES="wacom"
# ELSE
LLVM_TARGETS="X86"
QEMU_SOFTMMU_TARGETS="alpha aarch64 arm i386 mips mips64 mips64el mipsel ppc ppc64 s390x sh4 sh4eb sparc sparc64 x86_64"
QEMU_USER_TARGETS="alpha aarch64 arm armeb i386 mips mipsel ppc ppc64 ppc64abi32 s390x sh4 sh4eb sparc sparc32plus sparc64"
# ABI_X86="64 32"
GRUB_PLATFORMS="efi-64"
# source /var/lib/layman/make.conf

```

#### 2.5.2 portage

```sh
mkdir /mnt/gentoo/etc/portage/repos.conf
cat > /mnt/gentoo/etc/portage/repos.conf/gentoo.conf << EOF
[gentoo]
location = /data/portage
sync-type = rsync
sync-uri = rsync://mirrors.tuna.tsinghua.edu.cn/gentoo-portage/
#sync-uri = rsync://rsync.mirrors.ustc.edu.cn/gentoo-portage/
auto-sync = yes
EOF
```



### 2.6 更新系统

#### 2.6.1 进入chroot环境



```sh
cp --dereference /etc/resolv.conf /mnt/gentoo/etc/
mount -t proc /proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --make-rslave /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev
mount --make-rslave /mnt/gentoo/dev
chroot /mnt/gentoo /bin/bash
source /etc/profile
export PS1="(chroot) ${PS1}"
```



#### 2.6.2 同步portage

```sh
emerge --sync
```

#### 2.6.3 选择配置文件

```sh
eselect profile list# 列出profile
eselect profile set 16 # 桌面64 
```





#### 2.6.4 更新系统



```sh
emerge -auvDN --with-bdeps=y @world
```

如果碰到未满足的xxx或者其它提示:

```
emerge -auvDN --with-bdeps=y --autounmask-write @world
etc-update # 然后输入-3就能更新配置,确保再次运行时没有可更新的文件
emerge -auvDN --with-bdeps=y @world
```

等它跑完了,先别急
运行下这几个命令:

```bash
emerge @preserved-rebuild
perl-cleaner --all
emerge -auvDN --with-bdeps=y @world
```

确定没有更新之后再继续，否则查看输出尝试重复运行

如果你在`emerge -auvDN --with-bdeps=y @world`时提示带有`bindist`字样且你已启用`ACCEPT_KEYWORDS="~amd64"`的话
运行如下命令之后再次重试：

```bash
cd /usr/portage/dev-libs/openssl/
ebuild openssl-1.0.2o-r6.ebuild merge # 这里openssl的版本可能和你的不一样，运行ls命令查看可用版本，替换为
```



### 2.7 安装内核

先安装一下固件



```sh
emerge -av sys-kernel/linux-firmware
```



这次安装的内核是gentoo-sources

```sh
emerge -av gentoo-sources
```





### 2.8 安装必要软件

#### 2.8.1 网络管理

```sh
emerge -av net-misc/connman
```

#### 2.8.2 系统扩展

```sh
emerge -av app-admin/sysklogd sys-process/cronie sudo layman vim
```

启动服务

```sh
rc-update add sysklogd default && rc-update add cronie default
```



### 2.9 配置基本系统



```sh
# 时间
echo "Asia/Shanghai" > /etc/timezone 
emerge --config sys-libs/timezone-data
# 本地化
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
echo "zh_CN.UTF-8 UTF-8" >> /etc/locale.gen
locale-gen
eselect locale list
eselect locale set "en_US.utf8" #命令行一定要用英文！！中文可能乱码。
env-update && source /etc/profile && export PS1="(chroot) $PS1"
```

#### 2.9.2 主机名

```sh
echo hostname=\"chaos-core\" > /etc/conf.d/hostname
```

#### 2.9.3 acpi



```sh
emerge -av sys-power/acpi
```





### 2.10 fstab

```sh
wget https://raw.githubusercontent.com/YangMame/Gentoo-Installer/master/genfstab
chmod +x genfstab
mv genfstab /usr/bin/
genfstab -U  / > /etc/fstab
more /etc/fstab #最好检查下此文件,删掉无用挂载点
```

### 2.11 grub

这里使用UEFI的模式去引导

确保`make.conf`里面包含`GRUB_PLATFORMS`

```sh
echo 'GRUB_PLATFORMS="efi-64"' >> /etc/portage/make.conf
emerge --ask sys-boot/grub:2 # 安装grub
```

安装grub

```sh
mount -o remount,rw /sys/firmware/efi/efivars
grub-install --target=x86_64-efi --efi-directory=/boot/efi --removable
```

生成配置文件

```sh
grub-mkconfig -o /boot/grub/grub.cfg
```



### 2.12 显卡驱动

```sh
emerge -av x11-drivers/xf86-video-intel
```



## 3. 软件列表



#### 3.1 桌面环境

这次桌面环境使用的i3 因为i3 配置起来简单很爽啊！

```sh
emerge -av i3-gaps app-misc/ranger media-gfx/feh x11-misc/i3blocks x11-misc/i3lock x11-misc/i3lock-color x11-misc/i3status x11-misc/compton media-gfx/imagemagick x11-misc/rofi
```



#### 3.2 输入法

````sh
emerge -av app-i18n/fcitx app-i18n/fcitx-configtool app-i18n/fcitx-qt5 app-i18n/fcitx-rime
````

#### 3.3 字体

```sh
emerge -av media-fonts/noto-emoji media-fonts/ubuntu-font-family  media-fonts/fontawesome media-fonts/wqy-zenhei
```

#### 3.4 浏览器

```sh
emerge -av www-client/chromium
```

#### 3.5 系统主题

```sh
emerge -av x11-themes/papirus-icon-theme
```

#### 3.6 ssh客户端

```sh
emerge -av net-misc/remmina
```

#### 3.7 文档

```sh
emerge -av app-doc/zeal app-text/zathura app-text/zathura-cb app-text/zathura-djvu app-text/zathura-meta app-text/zathura-pdf-poppler app-text/zathura-ps
```

#### 3.8 思维导图

```sh
emerge -av app-misc/xmind
```

#### 3.9 KVM

```sh
emerge -av app-emulation/libvirt app-emulation/qemu app-emulation/virt-manager net-misc/spice-gtk 
```

#### 3.10 VPN

```sh
emerge -av net-vpn/openvpn
```

#### 3.11 音频

```sh
emerge -av media-sound/alsa-utils media-sound/mpdmedia-sound/ncmpcpp media-video/mpv
```

#### 3.12 截屏

```sh
emerge -av media-gfx/flameshot
```

#### 3.13 建模



```sh
emerge -av media-gfx/blender
```

#### 3.14 背光

显示器的背光

```sh
emerge -av x11-apps/xbacklight
```



#### 3.15 文件系统

```sh
emerge  -av sys-fs/dosfstools sys-fs/exfat-utils 
```

