
# 新版MDK提示the connected jlink is defective 的解决办法 #

在使用新版MDK调试arm芯片时候，市场上常见的jlink v8会被检测到是克隆版
并弹出the connected jlink is defective 错误。网上一般是用4.9的dll替换最新的驱动，这种方法我在5.26中测试已经失效，由于版本跨度太大而出现
flash download algorithm 找不到的兼容性问题。

另外一个方法是使用出厂固件刷jlink，这样对于介于4.9和5.24a之间的MDK管用，再新版比如我用的5.26就又失效了，依旧能被检测到。

所以如果想用最新的MDK的话比如5.26，我经过尝试，将两种方法发融合找到新的解决办法并且亲测可用。

首先将jlink固件刷新更换SN，具体教程推荐我记录下来的这篇：

https://shuspieler.com/blog/1242/

这样jlink能在5.24a之前版本使用，如果用的高于这个版本的话，用我提供的5.11 提取出来的dll驱动覆盖到最新版的安装路径中（比如D:\Keil_v5\ARM\Segger），亲测效果很完美。

注：

我的博客由于安全设置原因不让我上传zip文件，我还没来得及修理，如果有需要的同学可以给我留言我发到你的邮箱。我讨厌CSDN积分下载所以不会让自己成为讨厌的那种人。等之后我解决一下这个资源上传的问题以保证无障碍下载。

更新：

我将所需的固件上传到Github，需要用到的同学可以到这里下载：[https://github.com/shuspieler/DIY-RTOS-Learning-tinyOS-/tree/master/Jlink-Fix](https://github.com/shuspieler/DIY-RTOS-Learning-tinyOS-/tree/master/Jlink-Fix)

读到这里的朋友，记得github给我点一个星星呀，感激不尽。