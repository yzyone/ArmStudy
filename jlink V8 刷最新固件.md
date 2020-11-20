jlink V8 刷最新固件

最近想玩玩ARM开发，由于是买的淘宝买的第三方jlink v8，在用MDK调试的时候被检测到不是官方的设备，出现 the connected emulator is a j-link clone 或者 the connected jlink is defective 信息。

网上有两种解决方法，比较简单的是替换驱动为老版的。可是我这里有个问题是替换后flash download algorithm找不带STM32片内资源，所以也就不能成功下载程度。另一种方法是对仿真器下手，通过刷固件让MDK不再能检测到是第三方设备。

网上很多jlink v8刷序列号或者固件掉了之后重新刷固件的教程。可是有个严重的问题是几乎所有网页所需要下载的工具都指向了csdn。我非常反感这个网站，很多开源免费或者用户自己分享的资源拿到这里下载就要被网站收费，太讨厌了。所以我这里重新写一份教程，一方面记录我的升级过程以便之后再次用到，其次给大家提供一种不需要csdn积分就能下载所有工具的资源。

其他帖子写得已经够详细了，这里我只记录重点以及要点。

1.下载安装SAM-BA。这是官网的地址：https://www.microchip.com/developmenttools/ProductDetails/atmel%20sam-ba%20in-system%20programmer 我下载的是最新版SAM-BA 2.18 for Windows亲测可用，不必向一些帖子所说比不下载指定版。这些烧写工具就是为了更方便可靠给芯片写程序，所以开发工程师只会让新版本更加可靠而不是说给用户添麻烦。

2.下载一个可更改序列号的原始固件： https://gronlier.fr/blog/wp-content/uploads/2015/07/V8_Firmware_NoSerial_crackn.zip这个地址来自一个国外的论坛，对应于一些用户转移到CSDN上边的是叫做 “JlinkV8出厂固件”或者“v8_ID-XXXXXXXX.bin”。 如果这个地址有设置防盗链而失效了，那就到这个网页点击一下下载：https://gronlier.fr/blog/2015/07/unbrick-and-update-an-j-link-v8-clone/

3.接着下载jlink驱动，依旧推荐官网： https://www.segger.com/downloads/jlink/

4.这里边分类很多，不想找就直接安装集成包：j-Link Software and Documentatin Pack. 点击clock for downloads安装就好了。记得安装时候提示usb driver不要取消掉（默认是安装的，也就是说直接下一步可以）

5.接下载步骤和其他教程差不多，首先是擦除jLink上flash程序 1）先通电。2）短接ERASE（JP12），保持短接状态一分钟。3）断电。4）移除短接

6.第二步 1）短接TST（JP13）。2）通电，并保持短接一分钟。3）断电。4）移除短接。其中时间一定要足够，一开始不成功我就是这里的错误。一些教程说20秒以上，国外教程写着一分钟，亲测时间久一点可以成功，如果时间不够错误现象是电脑不能识别usb设备，并且任何驱动都安装不上。

7.上边两步完成后，重新连接usb，设备会被识别成一个usb serial device并且配备一个com口。这时候打开SAM-BA 将芯片型号选成AT91SAM7S64-ek点击connect。


8.之后会弹出一个菜单如上边所示，再Flash栏下选择send File Name成为刚才下载的bin文件，然后点击send File。

9.之后会弹出两个框，第一个选yes解锁flash的写入，第二个选no可以保护不能自动升级，这对于第三方的jlink非常重要。

10.之后勇JlinkCommander改序列号和升级到最新固件。


11.连接usb后打开JlinkCommander，会提示固件升级，现在先别升，点no，然后在命令行中输入“exec setsn=XXXXXXXX”。其中XXXXXXXX是8个十进制数，可以随意设定，比如说是当天的日期。需要注意的是，写入序列号后将不能通过本命令更改序列号，除非重新写入固件恢复出厂设置。退出JlinkCommander软件。

12.断开jlinkv8重新连接，重新打开刚才的jlinkcommander，这次升级固件。成功后就得到了一个最新固件并且序列号自定义后的仿真器。

完成啦~

更新：

最新版5.26即使刷完固件后依旧会被检测到是克隆版的jlink。。。通过网上查资料可知道，上边的教程在MDK5.24a就不适用了，而我一些工程还是用新的版本创建的，用老版本Keil再编辑的话会出现很多未知问题。所以我要再解决一个新的问题，就是刷完固件依旧再Keil 5.24a不能使用的棘手问题。

首先我根据大多数的教程，将jlink的驱动dll换成了4.9版本的文件。可是programming algorithm 缺失片内资源的选项。。。

我之前用过5.11版本的Keil可是正常使用jlink，可惜就是我的工程问题，必须让我升级到最新版Keil，真实难为我。。。

根据一个帖子的指导，在5.24a之后可以使用一个之前的驱动，可是4.9亲测有bug，所以也就只能找一个介于4.9和5.24a之间的版本了。正好我手里有个5.11的，复制备份下载，然后重新安装了一个新版Keil，用5.11的dll覆盖了新的，问题解决。

不过最后提醒，值得注意的就是使用我提供的5.11的dll也需要刷新jlink的固件以得到一个新的SN，如果不刷固件的话，已经进入黑名单的SN序列号依旧会被检测到是clone版。

5.11的dll下载地址我放在我新开的帖子中：

https://shuspieler.com/blog/1292/

Reference：

https://gronlier.fr/blog/2015/07/unbrick-and-update-an-j-link-v8-clone/

https://gronlier.fr/blog/2015/07/unbrick-and-update-an-j-link-v8-clone/v8_firmware_noserial_crackn/

https://blog.csdn.net/chamiyouyan/article/details/41174907

http://www.sonsivri.to/forum/index.php?topic=41726.50

https://blog.csdn.net/zhyulo/article/details/78924069

https://blog.csdn.net/android_lover2014/article/details/75150327

https://blog.csdn.net/Android_Linux_Unix/article/details/72900551

https://www.segger.com/downloads/jlink/#J-LinkSoftwareAndDocumentationPack

http://bbs.21ic.com/icview-1858620-1-1.html?fromuid=0

https://pan.baidu.com/s/1ZilVJ9XmlpwV28KTDjzqPw#list/path=%2Fsharelink4279067727-58460714011757%2F10518&parentPath=%2Fsharelink4279067727-58460714011757

