## 安装输入法

输入法软件分为输入引擎框架和具体输入法实现：

引擎框架，选择[fcitx](https://wiki.archlinux.org/index.php/Fcitx)

`# pacman -S fcitx`

具体输入法，选择[fcitx-googlepinyin](https://github.com/fcitx/fcitx-googlepinyin)

`# pacman -S fcitx-googlepinyin`

本来实现想选择搜狗拼音，但好像有bug，用着用着就卡死了，所以卸载，装回了googlepinyin。

安装输入法管理软件

`# pacman -S fcitx-configtool`

安装完管理软件后，输入下面命令即可按需对输入法进行配置了，比如快捷键什么的。

`# fcitx-configtool`
