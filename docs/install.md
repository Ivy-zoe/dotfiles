   * [Lenovo ThinkPad W520](#lenovo-thinkpad-w520)
      * [1. 硬件信息](#1-硬件信息)
         * [1.1 lscpu](#11-lscpu)
         * [1.2 lspci](#12-lspci)
         * [1.3 lsusb](#13-lsusb)
      * [2. 安装](#2-安装)
         * [2.1 分区设置](#21-分区设置)
         * [2.2 stage3文件](#22-stage3文件)
         * [2.3 make.conf和portage配置](#23-makeconf和portage配置)
         * [2.4 进入chroot环境](#24-进入chroot环境)
         * [2.5 同步portage](#25-同步portage)
         * [2.6 配置文件选择](#26-配置文件选择)
         * [2.7 更新系统](#27-更新系统)
         * [2.8 系统的基本配置](#28-系统的基本配置)
         * [2.9 固件](#29-固件)
         * [2.10 fstab](#210-fstab)
         * [2.11 系统扩展工具](#211-系统扩展工具)
         * [2.12 添加用户](#212-添加用户)
         * [2.13 内核配置](#213-内核配置)
         * [2.14 grub配置](#214-grub配置)
         * [2.15 重启](#215-重启)
      * [3. 应用列表](#3-应用列表)
         * [3.1 桌面环境](#31-桌面环境)
         * [3.2 音频](#32-音频)
         * [3.3 教育](#33-教育)
         * [3.4 邮箱](#34-邮箱)
         * [3.5 图形](#35-图形)
         * [3.6 脑图](#36-脑图)
         * [3.7 文档](#37-文档)
         * [3.8 终端](#38-终端)
         * [3.9 视频](#39-视频)
         * [3.10 KVM](#310-kvm)
      * [4. 参考链接](#4-参考链接)

# Lenovo ThinkPad W520

## 1. 硬件信息

| 型号 | cpu       | 内存     | 硬盘 | 显卡   | 其他 |
| ---- | --------- | -------- | ---- | ------ | ---- |
| W520 | i7 2820QM | 32G(8*4) | 480G | Q1000M | 指纹 |



### 1.1 lscpu

```sh
chaos@chaos-core ~ $ lscpu
Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
Address sizes:       36 bits physical, 48 bits virtual
CPU(s):              8
On-line CPU(s) list: 0-7
Thread(s) per core:  2
Core(s) per socket:  4
Socket(s):           1
NUMA node(s):        1
Vendor ID:           GenuineIntel
CPU family:          6
Model:               42
Model name:          Intel(R) Core(TM) i7-2820QM CPU @ 2.30GHz
Stepping:            7
CPU MHz:             797.332
CPU max MHz:         3400.0000
CPU min MHz:         800.0000
BogoMIPS:            4584.66
Virtualization:      VT-x
L1d cache:           32K
L1i cache:           32K
L2 cache:            256K
L3 cache:            8192K
NUMA node0 CPU(s):   0-7
Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx rdtscp lm constant_tsc arch_perfmon pebs bts nopl xtopology nonstop_tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 cx16 xtpr pdcm pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx lahf_lm epb pti tpr_shadow vnmi flexpriority ept vpid xsaveopt dtherm ida arat pln pts
chaos@chaos-core ~ $
```



### 1.2 lspci

```sh
chaos@chaos-core ~ $ lspci -nnk
00:00.0 Host bridge [0600]: Intel Corporation 2nd Generation Core Processor Family DRAM Controller [8086:0104] (rev 09)
	Subsystem: Lenovo ThinkPad T520 [17aa:21cf]
00:01.0 PCI bridge [0604]: Intel Corporation Xeon E3-1200/2nd Generation Core Processor Family PCI Express Root Port [8086:0101] (rev 09)
	Kernel driver in use: pcieport
00:02.0 VGA compatible controller [0300]: Intel Corporation 2nd Generation Core Processor Family Integrated Graphics Controller [8086:0126] (rev 09)
	Subsystem: Lenovo 2nd Generation Core Processor Family Integrated Graphics Controller [17aa:21d1]
	Kernel driver in use: i915
00:16.0 Communication controller [0780]: Intel Corporation 6 Series/C200 Series Chipset Family MEI Controller #1 [8086:1c3a] (rev 04)
	Subsystem: Lenovo ThinkPad T520 [17aa:21cf]
00:16.3 Serial controller [0700]: Intel Corporation 6 Series/C200 Series Chipset Family KT Controller [8086:1c3d] (rev 04)
	Subsystem: Lenovo 6 Series/C200 Series Chipset Family KT Controller [17aa:21cf]
	Kernel driver in use: serial
00:19.0 Ethernet controller [0200]: Intel Corporation 82579LM Gigabit Network Connection (Lewisville) [8086:1502] (rev 04)
	Subsystem: Lenovo ThinkPad T520 [17aa:21ce]
	Kernel driver in use: e1000e
00:1a.0 USB controller [0c03]: Intel Corporation 6 Series/C200 Series Chipset Family USB Enhanced Host Controller #2 [8086:1c2d] (rev 04)
	Subsystem: Lenovo ThinkPad T520 [17aa:21cf]
	Kernel driver in use: ehci-pci
00:1b.0 Audio device [0403]: Intel Corporation 6 Series/C200 Series Chipset Family High Definition Audio Controller [8086:1c20] (rev 04)
	Subsystem: Lenovo ThinkPad T520 [17aa:21cf]
	Kernel driver in use: snd_hda_intel
00:1c.0 PCI bridge [0604]: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 1 [8086:1c10] (rev b4)
	Kernel driver in use: pcieport
00:1c.1 PCI bridge [0604]: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 2 [8086:1c12] (rev b4)
	Kernel driver in use: pcieport
00:1c.3 PCI bridge [0604]: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 4 [8086:1c16] (rev b4)
	Kernel driver in use: pcieport
00:1c.4 PCI bridge [0604]: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 5 [8086:1c18] (rev b4)
	Kernel driver in use: pcieport
00:1c.6 PCI bridge [0604]: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 7 [8086:1c1c] (rev b4)
	Kernel driver in use: pcieport
00:1d.0 USB controller [0c03]: Intel Corporation 6 Series/C200 Series Chipset Family USB Enhanced Host Controller #1 [8086:1c26] (rev 04)
	Subsystem: Lenovo ThinkPad T520 [17aa:21cf]
	Kernel driver in use: ehci-pci
00:1f.0 ISA bridge [0601]: Intel Corporation QM67 Express Chipset LPC Controller [8086:1c4f] (rev 04)
	Subsystem: Lenovo ThinkPad T520 [17aa:21cf]
00:1f.2 SATA controller [0106]: Intel Corporation 6 Series/C200 Series Chipset Family 6 port Mobile SATA AHCI Controller [8086:1c03] (rev 04)
	Subsystem: Lenovo ThinkPad T520 [17aa:21cf]
	Kernel driver in use: ahci
00:1f.3 SMBus [0c05]: Intel Corporation 6 Series/C200 Series Chipset Family SMBus Controller [8086:1c22] (rev 04)
	Subsystem: Lenovo ThinkPad T520 [17aa:21cf]
	Kernel driver in use: i801_smbus
01:00.0 VGA compatible controller [0300]: NVIDIA Corporation GF108GLM [Quadro 1000M] [10de:0dfa] (rev ff)
	Kernel modules: nvidia_drm, nvidia
03:00.0 Network controller [0280]: Intel Corporation Centrino Advanced-N 6205 [Taylor Peak] [8086:0085] (rev 34)
	Subsystem: Intel Corporation Centrino Advanced-N 6205 (802.11a/b/g/n) [8086:1311]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
0d:00.0 System peripheral [0880]: Ricoh Co Ltd PCIe SDXC/MMC Host Controller [1180:e823] (rev 05)
	Subsystem: Lenovo ThinkPad T520 [17aa:21cf]
0d:00.3 FireWire (IEEE 1394) [0c00]: Ricoh Co Ltd R5C832 PCIe IEEE 1394 Controller [1180:e832] (rev 04)
	Subsystem: Lenovo ThinkPad T520 [17aa:21cf]
0e:00.0 USB controller [0c03]: NEC Corporation uPD720200 USB 3.0 Host Controller [1033:0194] (rev 04)
	Subsystem: Lenovo uPD720200 USB 3.0 Host Controller [17aa:21cf]
	Kernel driver in use: xhci_hcd
chaos@chaos-core ~ $
```



### 1.3 lsusb

```sh
chaos@chaos-core ~ $ lsusb
Bus 001 Device 004: ID 0a5c:217f Broadcom Corp. BCM2045B (BDC-2.1)
Bus 001 Device 005: ID 04f2:b217 Chicony Electronics Co., Ltd Lenovo Integrated Camera (0.3MP)
Bus 004 Device 002: ID 152d:9561 JMicron Technology Corp. / JMicron USA Technology Corp.
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 002: ID 05e3:0745 Genesys Logic, Inc. Logilink CR0012
Bus 001 Device 003: ID 147e:2016 Upek Biometric Touchchip/Touchstrip Fingerprint Sensor
Bus 001 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
chaos@chaos-core ~ $
```

## 2. 安装

### 2.1 分区设置

这次分区使用UEFI+GPT的安装方案，如下表

| 挂载点    | 物理分区  | 大小 | 文件系统 |
| --------- | --------- | ---- | -------- |
| /boot     | /dev/sda1 | 200M | Ext2     |
| /boot/efi | /dev/sda2 | 200M | Fat32    |
| /         | /dev/sda3 | 60G  | Ext4     |
| /data/    | /dev/sda4 | 100G | Ext4     |
| /portage  | /dev/sda5 | 60G  | Ext4     |
| /data/kvm | /dev/sda6 | all  | Ext4     |

分区命令

```sh
fdisk -l# 查看分区
gfdisk /dev/sda

Number  Start (sector)    End (sector)  Size       Code  Name
   1            2048          411647   200.0 MiB   8300  Linux filesystem
   2          411648          821247   200.0 MiB   8300  Linux filesystem
   3          821248       126650367   60.0 GiB    8300  Linux filesystem
   4       126650368       336365567   100.0 GiB   8300  Linux filesystem
   5       336365568       462194687   60.0 GiB    8300  Linux filesystem
   6       462194688       937703054   226.7 GiB   8300  Linux filesystem

```

> 初始化分区

```sh
mkfs.ext2 /dev/sda1
mkfs.fat -F32 /dev/sda2
mkfs.ext4 /dev/sda3
mkfs.ext4 /dev/sda4
mkfs.ext4 /dev/sda5
mkfs.ext4 /dev/sda6
```

> 挂载分区

```sh
 mount /dev/sda3  /mnt/gentoo/
 mkdir /mnt/gentoo/boot
 mount /dev/sda1 /mnt/gentoo/boot/
 mkdir /mnt/gentoo/boot/efi
 mount /dev/sda2 /mnt/gentoo/boot/efi/
 mkdir /mnt/gentoo/data
 mount /dev/sda4 /mnt/gentoo/data/
 mkdir /mnt/gentoo/portage
 mount /dev/sda5 /mnt/gentoo/portage/
 mkdir /mnt/gentoo/data/kvm
 mount /dev/sda6 /mnt/gentoo/data/kvm/
```



### 2.2 stage3文件

在这里你需要选择一个镜像站,在这里列出几个速度比较快的镜像站,请亲自测试选择镜像站:

[USTC](https://mirrors.ustc.edu.cn/)
[TUNA](https://mirrors.tuna.tsinghua.edu.cn/)
[163](http://mirrors.163.com/)

进入镜像站的`/gentoo/releases/amd64/autobuilds/`目录

如果你对systemd没有刚需则进入`current-stage3-amd64/`目录选择最新的`stage3`下载到本地的`/mnt/gentoo`目录,例如:stage3-amd64-20171019.tar.bz2

如果你需要systemd,则进入`current-stage3-amd64-systemd/`目录选择最新的`stage3`下载到本地的`/mnt/gentoo`目录,例如:stage3-amd64-systemd-20171018.tar.bz2

下载完成之后进入gentoo的根目录并解压文件:

```bash
cd /mnt/gentoo
tar vxpf stage3-*.tar.bz2或xz --xattrs-include='*.*' --numeric-owner
```



### 2.3 make.conf和portage配置

以下参数在经过自己调整或选择之后加入到 `/mnt/gentoo/etc/portage/make.conf`

- USE: 首先,你可以删掉默认的USE标记，加上`-bindist` (不了解USE的情况下建议如此)
- CFLAGS: 将CFLAGS修改为`CFLAGS="-march=native -O2 -pipe"` 或者你也可以指定.例如我的Intel CPU是haswell,将native换成haswell就行(不确定就不要指定).你也可以在[这里](https://www.funtoo.org/Subarches)看到所有可以设置的值
- MAKEOPTS: 根据你的CPU核心数设置MAKEOPTS例如双四线程设置为`MAKEOPTS="-j5"`
- GENTOO_MIRRORS: 设置为`GENTOO_MIRRORS="https://mirrors.ustc.edu.cn/gentoo/"` 请自行选择速度最快的Mirror
- EMERGE_DEFAULT_OPTS: 设置为`EMERGE_DEFAULT_OPTS="--keep-going --with-bdeps=y"`是个不错的选择,keep going意为安装一堆软件时遇到编译错误自动跳过这个软件继续编译安装
- FEATURES: 在这里最好写成`# FEATURES="${FEATURES} -userpriv -usersandbox -sandbox"`,最好在前面加上#注释掉,在你编译软件遇到权限不足时去掉注释即可解决问题（但请务必注意是不是因为`rm -rf /*`等命令权限不足，因为说不定你的ebuild文件被篡改了）
- ACCEPT_KEYWORDS: 如果你想用作桌面/学习/开发系统那就务必加上`ACCEPT_KEYWORDS="~amd64"`，服务器/工作/家/娱乐用可以忽略
- ACCEPT_LICENSE: 加上`ACCEPT_LICENSE="*"`表示此系统接受所有软件许可证,即不论非自由还是自由软件都接受,非商业用户基本不需要考虑
- L10N: 设置为`L10N="en-US zh-CN en zh"`
- LINGUAS: 设置为`LINGUAS="en_US zh_CN en zh"`
- VIDEO_CARDS: 根据你的显卡类型设置假如你是NVIDIA单显卡则设置为`VIDEO_CARDS="nvidia"`(闭源驱动)`VIDEO_CARDS="nouveau"`(开源驱动).还有radeon和intel,但如果你是双显卡例如Intel+NVIDIA则设置为`VIDEO_CARDS="intel i965 nvidia"`(只要不是远古的集成显卡都是用i965)
- GRUB_PLATFORMS: 如果你使用GRUB且使用UEFI启动则添加`GRUB_PLATFORMS="efi-64"`
- Portage Mirror: 这个不是make.conf的选项.`mkdir /mnt/gentoo/etc/portage/repos.conf`创建repos.conf目录并添加如下到/mnt/gentoo/etc/portage/repos.conf/gentoo.conf文件里面(自行选择速度最快的镜像站):

> Portage-mirrors

```bash
mkdir /mnt/gentoo/etc/portage/repos.conf
cat > /mnt/gentoo/etc/portage/repos.conf/gentoo.conf << EOF
[gentoo]
location = /portage
sync-type = rsync
sync-uri = rsync://mirrors.tuna.tsinghua.edu.cn/gentoo-portage/
#sync-uri = rsync://rsync.mirrors.ustc.edu.cn/gentoo-portage/
auto-sync = yes
EOF
```

> make.conf 

```sh
cat /mnt/gentoo/etc/portage/make.conf << EOF
# GCC
CHOST="x86_64-pc-linux-gnu"
CFLAGS="-march=sandybridge -O2 -pipe"
CXXFLAGS="${CFLAGS}"
MAKEOPTS="-j9"
CPU_FLAGS_X86="aes avx mmx mmxext popcnt sse sse2 sse3 sse4_1 sse4_2 ssse3"
# PORTAGE
PORTDIR="/portage"
DISTDIR="/portage/distfiles"
PKGDIR="/portage/packages"
GENTOO_MIRRORS="http://mirrors.tuna.tsinghua.edu.cn/gentoo/"
EMERGE_DEFAULT_OPTS="--ask --verbose=y --keep-going --with-bdeps=y --load-average"
# FEATURES="${FEATURES} -userpriv -usersandbox -sandbox"
PORTAGE_REPO_DUPLICATE_WARN="0"
# PORTAGE_TMPDIR="/var/tmp/notmpfs"
# USE
SUPPORT="pulseaudio mtp git chromium"
DESKTOP="infinality emoji cjk"
FUCK="-bindist -grub -plymouth -systemd consolekit -modemmanager -gnome-shell -gnome -gnome-keyring -nautilus -modules"
ELSE="client icu sudo python"
USE="${SUPPORT} ${DESKTOP} ${FUCK} ${ELSE}"
# LICENSED
ACCEPT_KEYWORDS="amd64"
ACCEPT_LICENSE="*"
# LANGUAGE
L10N="en-US zh-CN en zh"
LINGUAS="en_US zh_CN en zh"
# VIDEO_CARDS
VIDEO_CARDS="intel i965 nvidia"
# ELSE
LLVM_TARGETS="X86"
QEMU_SOFTMMU_TARGETS="alpha aarch64 arm i386 mips mips64 mips64el mipsel ppc ppc64 s390x sh4 sh4eb sparc sparc64 x86_64"
QEMU_USER_TARGETS="alpha aarch64 arm armeb i386 mips mipsel ppc ppc64 ppc64abi32 s390x sh4 sh4eb sparc sparc32plus sparc64"
# ABI_X86="64 32"
GRUB_PLATFORMS="efi-64"
# source /var/lib/layman/make.conf
EOF
```



### 2.4 进入chroot环境

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

### 2.5 同步portage

```sh
emerge --sync
```

 ### 2.6 配置文件选择

```sh
eselect profile list# 列出profile
```

如果你使用systemd则需要选上带有systemd字样的选项

如果你不使用systemd则不建议使用GNOME桌面,因为GNOME桌面依赖systemd(辣鸡)

```sh
eselect profile set 12
```

这里选择的是12的profile

### 2.7 更新系统

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

### 2.8 系统的基本配置



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

### 2.9 固件

```sh
emerge -av sys-kernel/linux-firmware
```

这里使用这个包所包含的固件就足够了

### 2.10 fstab

```sh
wget https://raw.githubusercontent.com/YangMame/Gentoo-Installer/master/genfstab
chmod +x genfstab
mv genfstab /usr/bin/
genfstab -U  / > /etc/fstab
more /etc/fstab #最好检查下此文件,删掉无用挂载点
```

是的就是这么的懒

如果你使用非ext4文件系统则在编译内核前需要另外安装相应的工具:

```
btrfs: emerge sys-fs/btrfs-progs
xfs: emerge sys-fs/xfsprogs
jfs: emerge sys-fs/jfsutils
```

### 2.11 系统扩展工具

```sh
emerge -av app-admin/sysklogd sys-process/cronie sudo layman vim
```

### 2.12 添加用户

```sh
useradd -m -G users,wheel,portage,usb,video,audio chaos #chaos是我的用户名称你可以设置你的
```



### 2.13 内核配置

有以下推荐内核可供选择:

```
gentoo-sources
ck-sources
git-sources
```

这里使用的是`gentoo-sources`

```sh
emerge -av gentoo-sources
```

如果你不会配置内核或者时间不允许可以先用`genkernel`:

```
emerge -av genkernel
genkernel --menuconfig all
genkernel --install initramfs
```

你可以使用我的配置

```sh
cd /usr/src/linux
wget -c https://raw.githubusercontent.com/slmoby/dotfiles/master/usr/src/linux/.config
make oldconfig
make modules_install
make install
```

### 2.14 grub配置

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



### 2.15 重启

卸载挂载点并重启

```sh
umount -R /mnt/gentoo
reboot
```



## 3. 应用列表

### 3.1 桌面环境

这次安装的桌面环境使用的是i3-gaps，启动管理器用的rofi巴拉巴拉一大堆直接看命令吧

```sh
emerge -av x11-wm/i3-gaps x11-misc/rofi  x11-misc/compton x11-misc/arandr x11-misc/i3blocks x11-misc/i3status
```

软件功能列表

| 包名              | 功能                   | ～   |
| ----------------- | ---------------------- | ---- |
| x11-wm/i3-gaps    | Windows Manager        | None |
| x11-misc/rofi     | 启动器挺好看的         | None |
| x11-misc/compton  | 终端透明支持           | None |
| x11-misc/arandr   | 方便设置分辨率         | None |
| x11-misc/i3blocks | 状态栏                 | None |
| x11-misc/i3status | 状态栏                 | None |
| media-gfx/feh     | 照片浏览，可以设置壁纸 | None |



### 3.2 音频

看视频用的是mpv，听音乐是用的ncmpcpp+mpd

安装命令

```sh
emerge -av media-sound/ncmpcpp  media-video/mpv  media-sound/mpd media-libs/alsa-lib media-plugins/alsa-plugins media-sound/alsa-utils
```



### 3.3 教育

平时也会用到Matlab 但是因为我穷还好有一个代替的软件看这里的[官网](<https://www.gnu.org/software/octave/>)

安装命令

```sh
emerge -av sci-mathematics/octave
```

### 3.4 邮箱

可以试试看thunderbird

安装命令

```sh
emerge -av mail-client/thunderbird
```

### 3.5 图形

有好的GPU为啥不用呢？`/etc/portage/package.mask/gpu`

加内容如下

```sh
>=x11-drivers/nvidia-drivers-391.0.0
```

这个是因为这个Q1000M比较古老了 GPU驱动最高支持到390版本

然后检查内核配置如果你使用的是我配置过的内核就不用做这步

```sh
cd /usr/src/linux
cat .config | grep CONFIG_MODULES=
cat .config | grep CONFIG_MODULE_UNLOAD=
```

确保`/etc/portage/make.conf`显卡部分配置为

```sh
VIDEO_CARDS=”intel i965 nvidia”
```

更新：

emerge –-sync && emerge -avuDN @world

添加如下内容到`/etc/portage/package.accept_keywords`：

```bash
=sys-power/bbswitch-9999 **
=x11-misc/bumblebee-9999 **
=x11-misc/primus-0.2 ~amd64
```

安装：

```bash
emerge -av bbswitch primus bumblebee x11-drivers/xf86-video-intel
```

用户组：

```bash
gpasswd -a chaos video && gpasswd -a chaos bumblebee # chaos是我的用户名你可以填你的
```

编辑`/etc/init.d/bumblebee` 删除以下内容：

```
depend() {
need xdm
need vgl
after sshd
} # 可能稍有不同 但放心的删掉depend(){…}就行了 因为根本不需要
```

编辑`/etc/bumblebee/bumblebee.conf` 修改为以下参数：

```
KeepUnusedXServer=false
Driver=nvidia
Bridge=primus
VGLTransport=rgb (如果你使用vgl则修改此参数)
KernelDriver=nvidia
PMMethod=bbswitch
```

开机服务：

```
rc-update add bumblebee default
```

测试：

```
optirun neofetch # neofetch可替换
```

PS：如果你还没有桌面则使用primusrun

### 3.6 脑图

有的时候也会画一些简单的思维导图这里我使用的是xmind

安装命令

```sh
emerge -av app-misc/xmind
```



### 3.7 文档

文档我这里主要使用三种方式记录文档

- LaTeX
- Org Mode
- Markdown

LaTeX安装不建议使用Portage里面所提供的安装可以参考我的这个项目里面的[脚本](<https://github.com/slmoby/script/tree/master/texlive>)

预览文档我使用的是`zathura`

安装命令

```sh
emerge -av app-text/zathura app-text/zathura-cb app-text/zathura-djvu app-text/zathura-meta app-text zathura-pdf-poppler app-text/zathura-ps
```

Org-mode 和 markdown 我比较懒也懒得配置emacs然后直接用了2333

### 3.8 终端

这里用的是`konsole`

安装命令

```sh
emerge -av kde-apps/konsole
```

### 3.9 视频

mpv用起来感觉很棒可以用这个

安装命令

```sh
emerge -av media-video/mpv
```

### 3.10 KVM

平时跑虚拟机干嘛的场景也是比较大的一个场景

内核配置部分你可以抄我的内核配置

安装软件

```sh
emerge -av app-emulation/libvirt app-emulation/qemu app-emulation/virt-manager
```





## 4. 参考链接



[YangMame](https://blog.yangmame.org)

[Gentoo Wiki]([https://wiki.gentoo.org)

[推荐软件列表](<https://github.com/alim0x/Awesome-Linux-Software-zh_CN>)

[系统主题参考](<https://github.com/Linux-Theme-Collection/Windows-Manager>)

