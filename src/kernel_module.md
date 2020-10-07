## Linux内核模块

内核模块分为内置模块和动态模块，一般在系统启动时加载。其中，动态模块可以在运行时[动态的加载和卸载](https://wiki.archlinux.org/index.php/Kernel_module)。

模块文件存放在/usr/lib/modules/_kernel_release_目录下，其中_kernel_release_可以通过命令`# uname -r`获取。

`# lsmod`列出当前系统加载的模块。

`# modinfo module_name`查看模块信息

因为很多模块都提供了调用接口，特别是驱动程序，使得我们可以更方便地控制自己的系统。而怎么启用接口以及如何调用，就可以通过上面的查看了解。

T430风扇转速的自动管理一直不是很合心意，即使键盘面板摸着很烫手了，风扇转速也不会自动往上调整，所以偶尔想手动把转速提高。

T430的[ACPI](https://uefi.org/acpi/specs)有模块thinkpad_acpi实现，所以操作如下：

`# cat /proc/acpi/ibm/fan`

看到如下信息：
```
status:         enabled
speed:          0
level:          auto
```
以上内容逐行说明了以下信息：
- 风扇启用
- 风扇转速为0
- 风扇处于自动运行模式

没有任何接口调用的信息，这时就需要执行下面命令查看thinkpad_acpi的模块信息，看看能不能启用控制接口了。


`# modinfo thinkpad_acpi`

可以看到该模块提供了以下参数：
```
parm:           experimental:Enables experimental features when non-zero (int)
parm:           debug:Sets debug level bit-mask (uint)
parm:           force_load:Attempts to load the driver even on a mis-identified ThinkPad when true (bool)
parm:           fan_control:Enables setting fan parameters features when true (bool)
parm:           brightness_mode:Selects brightness control strategy: 0=auto, 1=EC, 2=UCMS, 3=EC+NVRAM (uint)
parm:           brightness_enable:Enables backlight control when 1, disables when 0 (uint)
parm:           volume_mode:Selects volume control strategy: 0=auto, 1=EC, 2=N/A, 3=EC+NVRAM (uint)
parm:           volume_capabilities:Selects the mixer capabilities: 0=auto, 1=volume and mute, 2=mute only (uint)
parm:           volume_control:Enables software override for the console audio control when true (bool)
parm:           software_mute:Request full software mute control (bool)
parm:           index:ALSA index for the ACPI EC Mixer (int)
parm:           id:ALSA id for the ACPI EC Mixer (charp)
parm:           enable:Enable the ALSA interface for the ACPI EC Mixer (bool)
parm:           hotkey:Simulates thinkpad-acpi procfs command at module load, see documentation
parm:           bluetooth:Simulates thinkpad-acpi procfs command at module load, see documentation
parm:           video:Simulates thinkpad-acpi procfs command at module load, see documentation
parm:           light:Simulates thinkpad-acpi procfs command at module load, see documentation
parm:           cmos:Simulates thinkpad-acpi procfs command at module load, see documentation
parm:           led:Simulates thinkpad-acpi procfs command at module load, see documentation
parm:           beep:Simulates thinkpad-acpi procfs command at module load, see documentation
parm:           brightness:Simulates thinkpad-acpi procfs command at module load, see documentation
parm:           volume:Simulates thinkpad-acpi procfs command at module load, see documentation
parm:           fan:Simulates thinkpad-acpi procfs command at module load, see documentation
```
其中如下的参数表明是可以开启风扇控制接口的：

`parm:           fan_control:Enables setting fan parameters features when true (bool)`

添加以下配置文件，并在其中添加相应配置项：

`vim /etc/modprobe.d/thinkpad_acpi.conf`

配置项内容为：

`options thinkpad_acpi fan_control=1`

重启系统或者执行以下命令重新加载模块：
```
rmmod thinkpad_acpi
modprobe thinkpad_acpi
```

这时再次查看风扇驱动程序：

`# cat /proc/acpi/ibm/fan`

即可看到更丰富的信息，多了commands相关的接口信息：
```
status:         enabled
speed:          2585
level:          auto
commands:       level <level> (<level> is 0-7, auto, disengaged, full-speed)
commands:       enable, disable
commands:       watchdog <timeout> (<timeout> is 0 (off), 1-120 (seconds))
```
当我想把风扇转速调高时，就可以执行以下命令：

`# echo 5 > /proc/acpi/ibm/fan`

再次查看，风扇转速上去了，声音也更大了...
```
status:         enabled
speed:          4587
level:          5
commands:       level <level> (<level> is 0-7, auto, disengaged, full-speed)
commands:       enable, disable
commands:       watchdog <timeout> (<timeout> is 0 (off), 1-120 (seconds))
```

跑一段时间后，记得把风扇调回自动档，降噪节能。

`# echo auto > /proc/acpi/ibm/fan`
