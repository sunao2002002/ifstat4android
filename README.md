# ifstat_4_android
## ifstat是什么
ifstat是一个查看网口统计数据的工具，类似于iostat与vmstat。可以用于查看一段时间的网口收发数据的统计信息。
目前Android系统中并没有集成该工具，所以将其移植到Android系统，添加编译脚本，解决编译问题。
## ifstat的编译
### android
Android系统下面可以直接运行mm进行编译
### Linux
Linux系统下面提供了CMAKE编译脚本，编译方式如下：
```
[david@ifstat4android]$ mkdir out
[david@ifstat4android]$ cd out
[david@out]$ cmake ..
[david@out]$ make

```
或者直接运行build.sh进行编译。
## ifstat怎么用
可以在Linux下行通过'man ifstat'查看ifstat的帮助文档。
```
usage: ifstat [-a] [-l] [-z] [-n] [-v] [-h] [-t] [-i if0,if1,...]
       [-d drv[:opt]] [-s [comm@][#]host[/nn]] [-T] [-A] [-w]
       [-W] [-S] [-b] [-q] [delay[/delay] [count]]
```
ifstat有很多选项，常用选项的含义如下：
```
-a：监控所有能查询到的网口
-l：监控回环loop-back网口，lo默认是不监控的。
-z：隐藏计数为0的网口，例如那些激活但不使用的网口。
-n：关闭周期性的显示表头
-v：显示ifstat的版本号以及编译进去的网卡驱动。
-t：在每一行增加一个时间戳
-i：监控网卡的列表，用，隔开，如果网卡名称中有，号，则用反斜杠\进行转义
-d:指定driver，默认是用ifstat自带driver的第一个，常用的driver有：proc、ifmib、kstat、ifdat、route、kvm等
-s：与-d sndp:[comm@][#]host[/nn]等价，用于通过SNMP获取远程主机上的网络信息。
-T：报告所有网卡的带宽之和。
-w：使用固定列宽
-W：截断超过终端宽度的行
-b：带宽按照kbps，千比特/秒 而不是kBps千字节/秒计算
-q：安静模式，不打印警告信息
delay:数据更新的时间间隔，单位为秒，默认为1秒。如果需要指定小于1秒的间隔，可以用小数，如0.1秒
count：数据更新的次数，如未制定，默认为无限次。
```
## 应用举例
```
#ifstat -a -n -t -T 1 5
Time            lo                 eth0               Total       
HH:MM:SS   KB/s in  KB/s out   KB/s in  KB/s out   KB/s in  KB/s out
15:06:51      0.00      0.00      0.46      0.23      0.46      0.23
15:06:52      0.00      0.00      1.66     16.50      1.66     16.50
15:06:53      0.00      0.00      0.12      0.17      0.12      0.17
15:06:54      0.00      0.00      0.56      0.62      0.56      0.62
15:06:55      0.00      0.00      0.13      0.17      0.13      0.17

```
-a：打印所有网卡，包括回环lo

-n：不周期性打印表头

-T：增加Total列，统计所有网卡带宽之和

-t：增加时间戳

1：间隔1S

5：打印5次
