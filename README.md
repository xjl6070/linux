# linux

## vim使用

#### 1. 三种运行模式：

1. 普通模式（normal）  
2. 插入模式（insert）  
3. 命令行模式（Command）

#### 2. 普通模式（normal）

1. G用于直接跳转到文件尾
2. ZZ用于存盘退出Vi
3. ZQ用于不存盘退出Vi
4. /和？用于查找字符串
5. n继续查找下一个
6. yy复制一行
7. p粘帖在下一行，P粘贴在前一行
8. dd删除一行文本
9. x删除光标所在的字符
10. u取消上一次编辑操作（undo）

#### 3. 插入模式（insert）

在 Normal 模式下输入插入命令 i、 a 、 o进入insert模式。用户输入的任何字符都被vim当做文件内容
保存起来，并将其显示在屏幕上。
在文本输入过程中，若想回到Normal模式下，按 Esc 键即可。

#### 4. 命令行模式（Command)

Normal 模式下，用户按冒号 :即可进入 Command 模式，此时 vim 会在显示窗口的最后一行 (屏幕的
最后一行) 显示一个 “:” 作为 Command 模式的提示符，等待输入命令。

1. :w 保存当前编辑文件，但并不退出
2. :w newfile 存为另外一个名为 “newfile” 的文件
3. :wq 用于存盘退出Vi
4. :q! 用于不存盘退出Vi
5. :q用于直接退出Vi （未做修改）

## usb权限

#### 1. 串口名

一般如果只连接一个串口设备，则设备名称为ttyUSB0；短时间内拔插串口设备名称可能会改变例如ttyUSB1等；所以需要设置成开机就赋予权限。解决方案：在终端中输入 

```Terminal
sudo gedit /etc/udev/rules.d/70-ttyusb.rules
```

这一步操作是修改udev规则，在打开的文件中添加以下字段 将ttyUSB0-9都赋予权限

```Terminal
KERNEL==“ttyUSB[0-9]*”, MODE=“0666”
```

#### 2. 串口延时

解决方案如下：查看当前系统的latency_time值，默认是16

```Terminal
cat /sys/bus/usb-serial/devices/ttyUSB0/latency_timer
```

使用以下命令可以修改latency_time值到1，修改以后重启串口使用程序即刻生效，但是重启电脑以后会恢复默认值

```Terminal
sudo chmod 0666 /sys/bus/usb-serial/devices/ttyUSB0/latency_timer
echo 1 > /sys/bus/usb-serial/devices/ttyUSB0/latency_timer
```

使其永久生效的方法还是修改udev的规则，同问题1操作

```
sudo gedit /etc/udev/rules.d/70-ttyusb.rules
```

加入以下语句 并保存，重启电脑可以使用上面的语句查看是否生效。

```Terminal
ACTION=="add", SUBSYSTEM=="usb-serial", DRIVER=="ftdi_sio", ATTR{latency_timer}="1"
```

## 更换内核

查看已安装内核

```Terminal
sudo dpkg --get-selections |grep linux
```

下载低版本内核

```Terminal
sudo apt-get install linux-image-4.15.0-22-generic -y
sudo apt install linux-headers-4.15.0-22-generic -y
sudo apt install linux-modules-4.15.0-22-generic -y
sudo apt-get install linux-modules-extra-4.15.0-22-generic -y
```

安装完后，需要修改默认内核，用下面的指令打开grub文档查看已安装内核

```
dpkg --list | grep linux-image
```

```Terminal
sudo gedit /etc/default/grub
```

此时，文档中应该会有 GRUB_DEFAULT=0  这一行，把它改成  GRUB_DEFAULT=‘Ubuntu，Linux 4.15.0-22-generic’  然后保存，执行下面这个指令

```Terminal
sudo update-grub
```

卸载内核

```Terminal
sudo apt-get remove linux-image-4.15.0-112-generic
sudo apt-get remove linux-headers-4.15.0-112-generic
sudo apt-get remove linux-modules-4.15.0-112-generic
sudo apt-get remove linux-modules-extra-4.15.0-112-generic
sudo apt-get purge linux-image-4.15.0-112-generic
sudo apt-get purge linux-headers-4.15.0-112-generic
sudo apt-get purge linux-modules-4.15.0-112-generic
sudo apt-get purge linux-modules-extra-4.15.0-112-generic
```

