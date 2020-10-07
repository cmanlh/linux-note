## 安装显卡驱动

现在Linux的生态比以前好太多了，以前使用Linux系统的两大困难就是少驱动和少软件。少驱动，系统起不来；少软件，很多事干不了。而现在，甚至连N卡都已经有官方驱动了。幸福。

确定显卡类型：

`# lspci | grep -e VGA -e 3D`

如果有多块显卡，又都需要用到的话，就都安装，否则一般选择主板集成的显卡即可。T430有一块Intel的集成显卡、一块Nvidia的独立显卡。独立显卡用不上，所以直接通过主板BIOS禁用了。如果BIOS没有提供相应选项，也可执行以下操作停用对应卡。

* 系统启动时，不加载内核自带的对应显卡驱动

`# vim /etc/modprobe.d/blacklist.conf // 文件名任意`

`blacklist nouveau // N卡的开源驱动组件模块为nouveau，禁用即可`

* 移除加载的显卡设备

`# echo 1 > /sys/devices/pci0000:00/显卡设备路径/remove`

所以，仅需安装集成的Intel显卡驱动

`# pacman -S xf86-video-intel`
