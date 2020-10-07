## 安装声音相关控件

声音问题就比较简单了，声卡驱动一般自带，就只需要装个[管理软件](https://wiki.archlinux.org/index.php/Advanced_Linux_Sound_Architecture)。管理声道和控制音量就行。

`# pacman -S alsa-utils alsa-plugins`

然后通过下面的命令打开控制面板进行管理即可。

`# alsamixer`
