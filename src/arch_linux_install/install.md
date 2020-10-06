## 内核镜像安装
> 基本参照[官方指引](https://wiki.archlinux.org/index.php/Installation_guide)

#### 登录ArchLinux
通过BIOS设置从USB启动，或者直接通过GRUB引导界面选择USB启动进入Arch Linux系统。

#### 时间同步
`timedatectl set -ntp true`

#### 硬盘分区
执行命令`# ls /sys/firmware/efi/efivars`看看是否报错。如果不报错，就选择 _EFI with GPT_，否则就选择 _MBR with BIOS_。

现在的主板基本都支持EFI，小黑T430也不例外。按照官方指引，基本就分三个区，可以使用`# fdisk /dev/xxx`执行：
* EFI分区
  * 分区类型选择efi，`fdisk`过程中，执行`t`命令进行选择即可。
  * 分区大小为260M。
  * 必须选择FAT32文件系统格式，在分区完毕后执行`mkfs.fat -F32 /dev/sdxY`。
  * 承担系统启动引导作用。
* SWAP分区
  * 分区类型选择Linux Swap。
  * 分区大小大于512M，小于等于物理内存大小。
  * 文件系统格式通过`mkswap /dev/sdxY`赋予，格式化为Swap分区特有文件系统格式。
  * 当内存紧张时，系统可将闲置程序的内存数据存放到该分区，从而缓解内存困境。如果物理内存够大，可以不需要该区。
* root分区
  * 分区类型选择Linux Root(X86_64)，系统架构不同，可以根据实际架构进行选择。
  * 剩余的硬盘空间，当然也可以根据实际需要，按照系统目录功能特点，多划分几个分区。
  * 文件系统格式建议使用ext4，通过`mkfs.ext4 /dev/sdxY`进行格式化。
  * 系统分区，存放操作系统及所有其他后续文件。

#### Pacman镜像源选择
官方源下载速度非常慢，但Arch Linux官方非常贴心的已经将各国的源都已收集保存在了`/etc/pacman.d/mirrorlist`配置文件，找到中国的源，将其移到文件顶部即可。其中，清华、交大和中软的源比较好用，建议特别置顶。

#### 

#### 网络

