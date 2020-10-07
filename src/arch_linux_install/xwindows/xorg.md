## 安装Xorg

Xorg是X Window System的一个开源实现，目前大部分的窗口管理器或者桌面系统都是基于其实现的。

其中，Xorg大部分组件也用不上，安装窗口服务组件即可，后续其他GUI软件依赖的，可以按需通过Pacman安装即可，所以安装就很简单了。

`# pacman -S xorg-server xorg-xinit`

其中，xorg-xinit为Xorg的启动程序，后面会用到。
