## 安装dwm

[dwm](https://dwm.suckless.org)全称其实就是动态窗口管理器，由一帮崇尚极简有效理念的[程序员](https://dwm.suckless.org)开发维护。无论是新增、切换窗口，还是在双显示器间切换，都深得我心，快捷键让一切都丝滑般顺畅。

dwm内置使用的terminal同是这帮程序员维护的[st](https://st.suckless.org)，顾名思义就是极简终端。可以修改配置换成其他终端，但我用下来挺好，所以直接沿用了。

dwm支持多显示需要[xrandr](https://www.x.org/wiki/Projects/XRandR)配合，所以还需[安装xrandr](https://wiki.archlinux.org/index.php/Xrandr)。

dwm和st的定制完全通过C代码的头文件来实现，所以程序员们推荐直接从源码自行编译，并没有加入到pacman的软件源内。因此，对于这两个程序，我都fork了，其中master分支保持和源master一致，custom分支维护自己的自定义，当源有必要更新时，就合并到custom分支。

dwm：[官方git](https://git.suckless.org/dwm)，[我的Fork](https://gitee.com/lifeonwalden/dwm)
```
# git clone --depth=1 https://gitee.com/lifeonwalden/dwm 	// 下载代码
# cd dwm
# git checkout custom 						// 切换到自定义分支
# make 								// 编译，可能会报错，主要是缺少一些依赖库，根据报错通过pcman -Ss xxx查找，然后通过pacman -S xxx安装即可
# make clean install						// 安装
```
 
st：[官方git](https://git.suckless.org/st)，[我的Fork](https://gitee.com/lifeonwalden/st)
```
# git clone --depth=1 https://gitee.com/lifeonwalden/st		// 下载代码
# cd st
# git checkout custom 						// 切换到自定义分支
# make 								// 编译，可能会报错，主要是缺少一些依赖库，根据报错通过pcman -Ss xxx查找，然后通过pacman -S xxx安装即可
# make clean install						// 安装
```

安装xrandr

`# pacman -S xorg-xrandr`

最后，配置窗口启动参数，然后进入窗口环境了。

`# cp /etc/X11/xinit/xinitrc ~/.xinitrc`

`# vim ~/.xinitrc`

移除 _.xinitrc_ 中下面的加载项：
```
twm &
xclock -geometry 50x50-1+1 &
xterm -geometry 80x50+494+51 &
xterm -geometry 80x20+494-0 &
exec xterm -geometry 80x66+0+0 -name login
```
在 _.xinitrc_ 中添加dwm的启动命令：

`exec dwm`

最后，轻敲命令即可进入窗口环境了。

`# startx`

进入窗口环境后，按 _Shirt+Alt+Enter_ 即可打开窗口，具体dwm的快捷键可以查看源码文件 _config.h_ 。

启用双显示器：

`# xrandr --output LVDS1 --auto --output VGA1 --auto --rotate inverted --right-of LVDS1`

在dwm状态栏显示一些系统信息，比如系统时间、内存使用情况等等。脚本名称为dwmstatus.zsh，可在dwm我的fork地址的custom分支获得。
