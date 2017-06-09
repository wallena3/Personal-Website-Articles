我知道Android Studio现在很好用，但是近期需要用到以前的Eclise下的安卓项目，对AS还不是很熟，所以还是用Eclipse。第一次在linux下配置。所以记录下来。

Genymotion与Virtualbox

genymotion下载下来是bin格式的  用以下命令来安装

./genymotion-2.6.0-linux_x64.bin
装好后提示没有virtualbox，但是我之前已经安装了，很费解。用dpkg -i安装virtualbox安装时根据提示查看目录，

得到信息

/usr/share/virtualbox/src/vboxhost/build_in_tmp: 62: /usr/share/virtualbox/src/vboxhost/build_in_tmp: make: not found
搞了半天原来是因为安装系统的时候使用的是最小化mini安装，系统没有安装make、vim等常用命令，直接apt-get安装下即可；
接下来又出毛病了，提示

 Makefile:185: *** Error: unable to find the sources of your current Linux kernel. Specify KERN_DIR=<directory> and run Make again.  Stop.
网上查了一番，说安着这配那个，反正都是cv的，没一个好使。自己估计是debian8的内核可能有点老？之后更新了4.4.1内核。再安装 搞定。

之后在Eclipse上安装插件即可。Genymotion官网上有教程故不赘述。

SDK与ADT

ADT(Android Development Tools)： 目前Android开发所用的开发工具是Eclipse，在Eclipse编译IDE环境中，安装ADT，为Android开发提供开发工具的升级或者变更，简单理解为在Eclipse下开发工具的升级下载工具。adt只是一个eclipse的插件，里面可以设置sdk路径
SDK(Software Development Kit)： 一般是一些被软件工程师用于为特定的软件包、软件框架、硬件平台、操作系统等建立应用软件的开发工具的集合。在Android中，他为开发者提供了库文件以及其他开发所用到的工具。简单理解为开发工具包集合，是整体开发中所用到的工具包，如果你不用Eclipse作为你的开发工具，你就不需要下载ADT，只下载SDK即可开发。

SDK

可以在谷歌开发者的网站上下载到，这里给个地址。进去之后拉到网页最下面，可以看到

Get just the command line tools

选相应版本即可    下载下来以后在你想要的地方解压出来来，文件夹名为android-sdk-linux。然后在里面的tools文件夹里运行

./android
即可打开SDK。

ADT

在Eclipse里安装插件，网址location是

http://dl-ssl.google.com/android/eclipse/
全选安装即可

安装完重启，把你的SDK目录地址加入，就会自动识别你的API神马的。



总结：还可以  比较顺利。和win下配置没有太大区别。
