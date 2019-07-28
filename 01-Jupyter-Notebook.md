目录链接：https://www.jianshu.com/p/e1e201bea601

## 什么是 Jupyter Notebook？

按照 Jupyter 创始人 Fernando Pérez 的说法，他最初的梦想是做一个综合 Ju （Julia）、Py （Python）和 R 三种科学运算语言的计算工具平台，所以将其命名为 Ju-Py-te-R。发展到现在，Jupyter 已经成为一个几乎支持所有语言，能够把软件代码、计算输出、解释文档、多媒体资源整合在一起的多功能科学运算平台。

你在一个框框中直接输入代码，运行，它立马就在下面给你输出。

## 安装 Jupyter notebook

目前，安装 Jupyter 的最简单方法是使用 Anaconda。该发行版附带了 Jupyter notebook。你能够在默认环境下使用notebook。
系统： Ubuntu 16.0.4 LTS

## Anacond下载与安装

链接 https://www.anaconda.com/distribution/#download-section 下载所需的对应版本

![image.png](https://upload-images.jianshu.io/upload_images/16846478-670ac29db89adf72.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我这里选着的如图，Linux 的 Python3.7 version
如何安装 可以参看官方文档 [Installing on Linux]( https://docs.anaconda.com/anaconda/install/linux/)

### 1、要在Linux中使用GUI包，您需要为Qt安装以下扩展依赖项：
>apt-get install libgl1-mesa-glx libegl1-mesa libxrandr2 libxrandr2 libxss1 libxcursor1 libxcomposite1 libasound2 libxi6 libxtst6

### 2、输入以下内容以安装Anaconda for Python 3.7：
在你下载目录中使用如下命令安装，然后依据提示安装即可
>bash Anaconda3-2019.03-Linux-x86_64.sh

安装过程中，看到提示“In order to continue the installation process, please review the license agreement.”（“请浏览许可证协议以便继续安装。”），点击“Enter”查看“许可证协议”。

在“许可证协议”界面将屏幕滚动至底，输入“yes”表示同意许可证协议内容。然后进行下一步。

安装过程中，提示“Press Enter to accept the default install location, CTRL-C to cancel the installation or specify an alternate installation directory.”（“按回车键确认安装路径，按'CTRL-C'取消安装或者指定安装目录。”）如果接受默认安装路径，则会显示 PREFIX=/home/<user>/anaconda<2 or 3> 并且继续安装。安装过程大约需要几分钟的时间。

 安装器若提示“Do you wish the installer to prepend the Anaconda<2 or 3> install location to PATH in your /home/<user>/.bashrc ?”（“你希望安装器添加Anaconda安装路径在 /home/<user>/.bashrc 文件中吗？”），建议输入“yes”。

当看到“Thank you for installing Anaconda<2 or 3>!”则说明已经成功完成安装。

###  3、验证安装结果。可选用以下任意一种方法：
① 在终端中输入命令 conda list ，如果Anaconda被成功安装，则会显示已经安装的包名和版本号。

② 在终端中输入 python 。这条命令将会启动Python交互界面，如果Anaconda被成功安装并且可以运行，则将会在Python版本号的右边显示“Anaconda custom (64-bit)”。退出Python交互界面则输入 exit() 或 quit() 即可。
![image.png](https://upload-images.jianshu.io/upload_images/16846478-cc0e4241133ed0f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

③ 在终端中输入 anaconda-navigator 。如果Anaconda被成功安装，则Anaconda Navigator将会被启动。
![image.png](https://upload-images.jianshu.io/upload_images/16846478-e5c54697fde69460.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如果有问题 重复1 2 3操作

###   4、更新

>conda update conda


## 参考资料：

*极客时间 Python核心技术与实战学习*

Python核心技术与实战（极客时间）链接：
http://gk.link/a/103Sv

Anaconda详细安装及使用教程（带图文）：
[https://blog.csdn.net/ITLearnHall/article/details/81708148](https://blog.csdn.net/ITLearnHall/article/details/81708148)

Anaconda Distribution:
[https://docs.anaconda.com/anaconda/](https://docs.anaconda.com/anaconda/)

anaconda安装与认识：
[https://blog.csdn.net/weixin_44055272/article/details/89076680](https://blog.csdn.net/weixin_44055272/article/details/89076680)


----
>*GitHub链接：*
>*[https://github.com/lichangke/LeetCode](https://github.com/lichangke/LeetCode)*

>*知乎个人首页：*
>*[https://www.zhihu.com/people/lichangke/](https://www.zhihu.com/people/lichangke/)*

>*简书个人首页：*
>*[https://www.jianshu.com/u/3e95c7555dc7](https://www.jianshu.com/u/3e95c7555dc7)*

>*个人Blog:*
>*[https://lichangke.github.io/](https://lichangke.github.io/)*

>*欢迎大家来一起交流学习*
