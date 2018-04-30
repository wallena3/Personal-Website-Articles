## 终端搜索用过的命令
记不住某个命令时，可以记住这个命令的某个关键字，然后在终端下按ctrl+r进入搜索模式，然后输入你刚才记住的关键字，
这时系统会提示你以前输入过的相关命令。如果不是你想要的，继续按Ctrl-r提示下一条命令，直至找到你想要的命令，按回车即运行，如果最终都没有找到按Ctrl-c退出搜索模式即可。
## find 命令文件名搜索
find 命令可以按许多搜索条件来进行搜索文件，最常用的就是按文件名搜索：
```shell
$ find -name readme
```
上述命令指在当前目录下查找文件名是readme的文件，如果文件名过长你无法完全记住，可以选加通配符减小记忆负担例如：
```
$ find -name read*
```
## grep 文件内容搜索
grep 是很多有经验的开发者最常用的一个命令，如果你不知道文件在哪里，但是你知道文件中的几个关键字，你就可以把它找出来：
```
$ grep 记不住某个 -r
```
## 自动连接蓝牙4.0鼠标
编辑/etc/bluetooth/main.conf
去掉行[Policy]和AutoEnable前的注释。
将AutoEnable=false改为AutoEnable=true
搜索到蓝牙鼠标后配对，开机或唤醒后都可以自动连接。
## 解压缩乱码
1：unzip -O GBK you_zip_file.zip
2：这个最简单省力，默认debian已经安装了额unar，这个工具会自动检测文件的编码，也可以通过-e来指定：
unar file.zip
addition:这个可能不好使 还是方法一好
## sudo快捷方式
之前在ubuntu中安装了wireshark, 这个程序必须用root权限才能对某个接口抓包, 就一直是在终端"sudo wireshark"来运行. 最近在网上看到一种方法,在面板处添加一个快捷方式, 然后将其属性修改一下, 以后每次点击该图标就会提示输入sudo密码, 就可以直接以root方式打开了.
实际上就是Ubuntu GUI中以root方式打开某个软件的方式...
属性修改具体如下(主要是gksu):
```
 Type:	 Application
 Name:
 Wireshark
 Command:	 gksu "wireshark"
 Comment:	 Network traffic analyzer
```
PS: 如果系统没有gksu命令, 那就先安装之"apt-get install gksu".
## Debian添加普通用户为sudoer
需要注意，Debian默认没有安装sudo  
apt-get install sudo  
安装一下。debian下默认新建的用户都是普通用户，不是sudoer，这个和ubuntu有很大区别。这意味着你无法使用sudo来暂时提升权限  
所以你在debian上用普通用户执行sudo命令时，会报如下错误：
xxx is not in the sudoers file. This incident will be reported。  
出现这个问题，是因为执行sudo命令的用户不在sudoers文件的列表中。可以通过编辑sudoers文件，来解决这个问题。  
切换到root，修改/etc/sudoers。  
你会看到  
User privilege specification  
 root ALL=(ALL:ALL) ALL  
比如你的用户名是walle，就再写一行，把root变成walle就可以了  
当然如果你理解上面的原理后，可以直接输入如下命令解决此问题  
su -echo 'xxx ALL=(ALL:ALL) ALL' >> /etc/sudoers  (其中xxx代表用户名)  

## Debian语言问题  
如果一开始使用的是英语或者汉语，现在想切换，那就运行  
sudo dpkg-reconfigure locales  
然后移动光标到想要使用的主语言，tab键切到ok。  
然后还要在设置中的"区域与语言"中把语言改成英文或中文。注销即可。  

## firefox安装flash
很多网站以及在线播放视频都需要flash插件，但是ubuntu下并没有安装包，我们只能去adobe的官网下载flash的tar版本手动安装。下面给出地址。
在firefox中打开  
https://get.adobe.com/flashplayer/?promoid=KLXMF  
在这里我们要选择tar.gz for linux。firefox使用的是npapi，不要下载错了。可以看到libflashplayer.so这个文件以及/usr文件夹。  
下面进行下面两部：  
* 将libflashplayer.so复制到你系统下firefox的插件目录。我现在用的是ubuntu16.04。位置是/usr/lib/firefox-addons/plugins  
DEEPIN 15.4的目录是  
/usr/lib/mozilla/plugins    
cp libflashplayer.so /usr/lib/firefox-addons/plugins  
* 将/usr文件夹下内容全部复制到系统的/usr文件夹下即可  
sudo cp -r usr/* /usr  

## String、StringBuffer、StringBuilder区别
StringBuffer、StringBuilder和String一样，也用来代表字符串。String类是不可变类，任何对String的改变都 会引发新的String对象的生成  
StringBuffer则是可变类，任何对它所指代的字符串的改变都不会产生新的对象。既然可变和不可变都有了，为何还有一个StringBuilder呢？  
先说一下集合的故事，HashTable是线程安全的，很多方法都是synchronized方法，而HashMap不是线程安全的，但其在单线程程序中的性能比HashTable要高。  
StringBuffer和StringBuilder类的区别也是如此，他们的原理和操作基本相同，区别在于StringBufferd支持并发操作，线性安全的，适合多线程中使用。  
StringBuilder不支持并发操作，线性不安全的，不适合多线程中使用。新引入的StringBuilder类不是线程安全的，但其在单线程中的性能比StringBuffer高。
若不考虑多线程，采用String对象时，执行时间比其他两个都要高，而采用StringBuffer对象和采用StringBuilder对象的差别也比较明显。  
由此可见，如果我们的程序是在单线程下运行，或者是不必考虑到线程同步问题，我们应该优先使用StringBuilder类；如果要保证线程安全，自然是StringBuffer。  
除了对多线程的支持不一样外，这两个类的使用方式和结果几乎没有任何差别，  
## CMCC EDU 在Ubuntu16.04下连不上的原因:  
因为dns没有设置成cmcc-edu的dns，当时没有设置成dns自动获取.正常/etc/resolv.conf中的dns会随网络的改变自动更新(由Networkmanage接管)
所以要在windows/别的linux机器下连上cmccedu，使用ipconfig -all命令查看dns地址。之后在将dns地址加入/etc/resolv.conf 中即可
## LinuxGNOME拨号上网:
GNOME网络设置里没有dsl上网，在命令行启动nm-connection-editor即可出现上网界面
在dsl选项里面加入用户名密码即可。   有可能也和运营商dns有关 原理同cmcc-edu  
## 解决walle-x450jf不能无线上网的问题
要在/etc/modprobe.d/blacklist.conf文件中加入相应的黑名单：blacklist acer-wmi
  

## Ubuntu 快捷图标
目录位置为/usr/share/applications/下。但是.desktop文件其中应用程序的名称不能包含空格(如 Cmd MarkDown)。包含空格将无法识别快捷方式




## linux下阅读内核源码
windows下有一个很好的软件,source insight.linux下用的很常见的是一个github项目：OpenGrok

## 获取Bing每日背景图片
bing中文网一直在提供每日更新背景图片壁纸的json数据，网址为
http://cn.bing.com/HPImageArchive.aspx?format=js&idx=0&n=1
根据上面地址的结构，暂时研究到就三项属性有效，他们分别是
1、format，非必要。我理解为输出格式，不存在或者不等于js，即为xml格式，等于js时，输出json格式；
2、idx，非必要。不存在或者等于0时，输出当天的图片，-1为已经预备用于明天显示的信息，1则为昨天的图片，idx最多获取到之前16天的图片信息；*
3、n，必要。这是输出信息的数量，比如n=1，即为1条，以此类推，至多输出8条；*
此处我们要注意的时，是否正常的输出信息，与n和idx有关，通过idx的值，我们就可以获得之前bing所使用的背景图片的信息了。
最后根据需要就可以解析出图片了
reference: https://oss.so/article/55

## linux命令行工具
两个目录来回切换，当cd的参数为'-' 时，它将被环境变量OLDPWD替代。OLDPWD存储的是前一个操作目录的路径
```
cd -
```

## tomcat部署war
tomcat默认目录是webapps
将项目导出为war包然后直接上传到webapps根目录下，随着tomcat的启动,war包可以自动被解压。
例如我的war包是web.war，上传好后重启tomcat在webapps目录就多出一个对应的web目录

## 三月储存网站
美食天下:美食     http://www.meishichina.com/
Hi现场:微信上墙等 http://www.hixianchang.com/
阮一峰:个人博客    http://www.ruanyifeng.com/
Blender:三维动画制作 https://www.blender.org
APUE:书的网站       http://www.apuebook.com/
苏州旅游局       http://www.visitsz.com/

## 不同Linux间拷贝文件命令:scp
从本机拷贝到服务器:
```
scp /home/daisy/full.tar.gz root@172.19.2.75:/home/root
```
从服务器拷贝到本机:
```
scp root@www.cumt.edu.cn:/home/root/others/music /home/space/music/1.mp3
```

## 阿里云香港部署 springboot项目
现在阿里云的主机安全组必须要设置，常见的80端口 8080端口以及ss要用的端口都要给上。

## ss在linux下的全局代理模式----解决中科大网不能连接ss的问题
因为科大封锁了高端口的出站链接，为了不让玩游戏，游戏端口都是好几万，所以把ss的端口降到1024以内即可。
接下来又研究了下全局ss，proxychains不错，但是不能链接steam还存在bug。静观版本更新.还有一种解决方案是iptable+redsock。这种方式可以实现全局代理。
首先安装redsocks
> sudo apt install redsocks

接下来需要编辑/etc/redsocks.conf文件，将redsocks{} 内的ip以及port两个参数改为你的ss设置的参数。我的为127.0.0.1和1080
接下来配置iptables转发规则，创建一个文件夹
> sudo mkdir /etc/redsocks

在里面分别创建两个文件，start.sh以及stop.sh
start.sh:
```
#!/bin/bash
#不重定向目的地址为服务器的包
#请用你的shadowsocks服务器的地址替换$SERVER_IP
sudo iptables -t nat -A OUTPUT -d $SERVER_IP -j RETURN
#不重定向私有地址的流量
sudo iptables -t nat -A OUTPUT -d 10.0.0.0/8 -j RETURN
sudo iptables -t nat -A OUTPUT -d 172.16.0.0/16 -j RETURN
sudo iptables -t nat -A OUTPUT -d 192.168.0.0/16 -j RETURN
#不重定向保留地址的流量,这一步很重要
sudo iptables -t nat -A OUTPUT -d 127.0.0.0/8 -j RETURN
#重定向所有不满足以上条件的流量到redsocks监听的12345端口
#12345是你的redsocks运行的端口,请根据你的情况替换它
sudo iptables -t nat -A OUTPUT -p tcp -j REDIRECT --to-ports 12345
```
stop.sh
```
#!/bin/bash
#不重定向目的地址为服务器的包
#请用你的shadowsocks服务器的地址替换$SERVER_IP
sudo iptables -t nat -D OUTPUT -d $SERVER_IP -j RETURN
#不重定向私有地址的流量
sudo iptables -t nat -D OUTPUT -d 10.0.0.0/8 -j RETURN
sudo iptables -t nat -D OUTPUT -d 172.16.0.0/16 -j RETURN
sudo iptables -t nat -D OUTPUT -d 192.168.0.0/16 -j RETURN
#不重定向保留地址的流量,这一步很重要
sudo iptables -t nat -D OUTPUT -d 127.0.0.0/8 -j RETURN
#重定向所有不满足以上条件的流量到redsocks监听的12345端口
#12345是你的redsocks运行的端口,请根据你的情况替换它
sudo iptables -t nat -D OUTPUT -p tcp -j REDIRECT --to-ports 12345
```
最后赋予这两个脚本执行权限
```
>sudo chmod +x /etc/redsocks/*.sh
```

接下来把redsocks的启动脚本/lib/systemd/system/redsocks.service修改如下
```
[Unit]
Description=Redsocks transparent SOCKS proxy redirector
After=network.target

[Service]
Type=forking
EnvironmentFile=/etc/default/redsocks
ExecStartPre=/etc/redsocks/start.sh
ExecStartPre=/usr/sbin/redsocks -t -c \${CONFFILE}
ExecStart=/usr/sbin/redsocks -c \${CONFFILE}
ExecStopPost=/etc/redsocks/stop.sh

[Install]
WantedBy=multi-user.target
```
最后关闭redsocks的开机自启就可以了，每次可以通过
> sudo service redsocks stop

来关闭redsocks.
如果想要关闭开机自动启动，在ubuntu16.04下执行如下命令即可
> sudo update-rc.d redsocks disable

到此为止，全局都已经在代理下了，但是steam仍然登录不了。我不知道是因为steam锁区还是什么原因。使用手机开启热点让电脑登陆steam后，进入steam中下载/浏览就可以正常的使用代理网络了
类似的软件还有一个tsock。下面这个网站也可以作为参考
http://wonderkun.cc/index.html/?p=394

## 图床的使用
原来不清楚图床是什么概念。简单里说就是可以让你上传一张图片，然后图床为你生成一个外链，让你可以在别处通过这个外链使用这张图片，这对博客网站来说还是很有用处的。查看了几个产品后选择使用Droplr。使用方法很简单，上传图片后就会生成一个网址，此网址会显示图片。然后在这个网址上的图片上右键->copy image address   就可以得到图片的地址，在网站中使用了

## 重构企业日报系统历程
数据库表名和程序中变量名的大小要相同，因为原来在windows下数据库对大小写不敏感，linux下敏感

#### 在eclipse下使用gradle构建整个项目
首先在Help->Markerplace中搜索插件BuildShip，这是一个eclipse官方的gradle插件。安装好后可以创建gradle项目。
但是此时项目为java项目，右键工程名Properties->Project Facets ->choise -> Dynamic Web Module 即可。
接下来还要在Properties-> Deployment Assembly中点击Add,加入Project and EXternal Denpendeccies。才可以使jar包正确导入。
每次改完最好要Project->clean一下再运行tomcat，否则会出现奇怪的错，现在也不知道怎么弄那个错，大概就是the main resource ~/wokspace/.seetings/plugins/tmp0/webapps/DailyReport in not vaild.按照网上的方法尝试无果。

## 搭建caffe深度学习框架
* Ubuntu 16.04.2 LTS 64-bit
* Intel® Core™ i7-4700HQ CPU @ 2.40GHz × 8
* GeForce GT 745M/PCIe/SSE2
* CUDA 8.0.61
* cudnn 5.1
* opencv 3.2.0
* caffe release candidate 5
首先要安装CUDA以及cudnn。之前装过TensorFlow，已经装好了。按照TensorFlow的github指南安装即可。写的比caffe的详细多了。
接下来安装CUDA的Sample。进入/usr/local/cuda/samples目录下，输入
```
$ sudo make all -j8
```
-j8表示用八个核编译，会快很多。编译成功后进入/usr/local/cuda/samples/bin/x86_64/linux/release目录执行
```
$ ./deviceQuery
```
如果能够打印处显卡相关的信息，则说明cuda安装成功了。在上述make过程中，可能会报错，类似下面这些代码
```
make[1]: Entering directory /usr/local/cuda-7.0/samples/_Imaging/cudaDecodeGL
/usr/local/cuda-7.0/bin/nvcc -ccbing++   -m64      -gencodearch=compute_20,
code=compute_20 -o cudaDecodeGL FrameQueue.o ImageGL.o VideoDecoder.o
VideoParser.o VideoSource.o cudaModuleMgr.o cudaProcessFrame.o
videoDecodeGL.o  -L../../common/lib/linux/x86_64 -L/usr/lib/"nvidia-367"
-lGL -lGLU -lX11 -lXi -lXmu -lglut -lGLEW -lcuda -lcudart -lnvcuvid
/usr/bin/ld: cannot find -lnvcuvid
collect2: error: ld returned 1 exit status
make[1]: *** [cudaDecodeGL] Error 1
make[1]: Leaving directory /usr/local/cuda-7.0/samples/3_Imaging/cudaDecodeGL'
make: *** [3_Imaging/cudaDecodeGL/Makefile.ph_build] Error 2
```
上述的错误中有一段为'-L/usr/lib/"nvidia-367"'。这个错误是因为这里的驱动版本367和系统正在使用的375版本不一样，修改文件NVIDIA_CUDA-7.0_Samples/3_Imaging/cudaDecodeGL/findgllib.mk ，将其中的UBUNTU_PKG_NAME = "nvidia-367" 改成nvidia-375即可。
接下来编译安装opencv。在官网下载解压。先按照官网说的安装那一堆依赖，然后根据官网上的步骤来即可。简洁版如下。
```
cd ~/opencv
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local ..
make -j7
sudo make install
```
安装完opencv之后就可以安装caffe了。先按照官网上说的安装那一大堆依赖。然后按照接下来的Compilation进行。记得把Makefile.config中 USE_CUDNN := 1 之前的注释去掉。这里还需要把# OPENCV_VERSION := 3的注释去掉，因为我们用的opencv版本是3.否则会出如下的错
```
CXX examples/mnist/convert_mnist_data.cpp
AR -o .build_release/lib/libcaffe.a
LD -o .build_release/lib/libcaffe.so.1.0.0-rc3
/usr/bin/ld: cannot find -lcudnn
collect2: error: ld returned 1 exit status
Makefile:554: recipe for target '.build_release/lib/libcaffe.so.1.0.0-rc3' failed
```
在make all时可能会出错
```
./include/caffe/util/hdf5.hpp:6:18: fatal error: hdf5.h: No such file or directory
```
解决方法
在Makefile.config文件的第85行左右，添加 /usr/include/hdf5/serial/ 到 INCLUDE_DIRS，也就是把下面第一行代码改为第二行代码。
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/
接下来make test，再最后make runtest。最后一步可能会出现以下错误
```
 libraries: libopencv_core.so.3.2: cannot open shared object file: No such file or directory
```
解决方法
新建文件
/etc/ld.so.config.d/opencv.conf
然后再把libopencv_core.so.3.2所在的位置加入其中，我的位置是/usr/local/lib
最后i ran :
sudo ldconfig -v
and problem resolved .

## Qt creator4.3.1 (QT 5.9.0) 搜狗输入法不能输入中文
根本原因：缺乏动态共享库libfcitxplatforminputcontextplugin.so，因为是动态库，而且仓库中的版本太旧了，需要自己编译
* sudo apt install cmake fcitx-libs-dev libgl1-mesa-dev libglu1-mesa-dev libxkbcommon-dev bison
* 设置环境变量 export PATH="/home/walle/Qt5.9.0/5.9/gcc_64/bin":$PATH (这种export只在当前终端下生效，重启一个终端 echo $PATH 可发现没有qt的目录，这只是为下面编译而做)
* 下载编译cmake的扩展模块包，否则编译fcitx-qt5时会报  没有找到"ECM"包配置文件的错误
wget https://launchpadlibrarian.net/189487929/extra-cmake-modules_1.4.0.orig.tar.xz
tar -xJf extra-cmake-modules_1.4.0.orig.tar.xz
进入其中，然后执行cmake .来生成Makefile文件
生成了Makefile之后，直接make, sudo make install来安装就可以了。
* git clone https://github.com/fcitx/fcitx-qt5 下载项目源码，准备编译
```
cd fcitx-qt5
cmake .
make
sudo make install
```
* 最后把编译得到 libfcitxplatforminputcontextplugin.so 拷贝到两个指定目录,分别为
~/Qt5.9.0/5.9/gcc_64/plugins/platforminputcontexts
~/Qt5.9.0/Tools/QtCreator/lib/Qt/plugins/platforminputcontexts/
最后给这两个文件加上x权限就行了
第一个目录可以解决生成的qt程序不能输入中文，第二个目录可以解决QtCreator中不能输入中文。
reference:
http://www.cnblogs.com/oloroso/p/5114041.html
http://blog.csdn.net/liang101x/article/details/51956436

## 腾讯云香港shadowsocks libev的配置
阿里云香港的价格太贵了，腾讯云香港只要45，，故迁移，效果还不错
* ss安装方式有变，按照官方文档构建deb包即可。构建完毕后安装所有deb包，缺依赖则补依赖。
* 有一个包叫debhelper，要求10.0版本。默认源中最高版本才是9，解决办法:
```
sudo apt install debhelper/xenial-backports
```
* 腾讯安全组和阿里略有不同，入站规则要加上TCP,UDP的ss端口，以及SSH的22端口，以及常用的80,8080端口。
出站规则加上All traffic
* 与阿里云不同，ss中的/etc/shadowsocks-libev/config.json文件中，server一行中必须填写腾讯云的内网ip！！不能填外网ip，否则ss不能成功运行！！！具体原因不明。
* 如果在阿里云开过发票，而且发票抬头和支付宝/银行账户名不符，则必须把发票寄回才能修改抬头名称，以及提现！非常坑爹！
当时我就是为了报销学校的服务器填了学校的抬头，导致我现在无法提现，以后除非必要，不要把钱存入阿里云余额。没办法，幸好余额还可以在万网续费域名。

## ASUS450jf(X450JF)笔记本更新BIOS版本&修复Deepin EFI引导
#### 烧写BIOS
* 格式化一个U盘为FAT32格式，将下载好的BIOS复制进入U盘
* 将U盘插入电脑，重启进入BIOS，选择asus easy flash
* 进入后选择相应的文件回车即可刷新
#### 烧写后出现的问题
我的电脑是双系统，UEFI，由于更新了BIOS会导致Windows重新将linux的grub引导覆盖掉了(相当于先装linux后装windows的效果)。所以需要修复引导。
在windows下一个easyuefi的软件，新增引导，点击Windows下的EFI分区，点击浏览，选择/EFI/deepin/grubx64.efi,并将它放置在最上面。
因为更新了BIOS版本，BIOS参数都被还原了，所以还需要将BIOS设置中的Secure BOOT 关闭才能进入linux。
#### 番外，Secure Boot是什么
* Secure Boot一般会出现在OEM大厂的品牌电脑的2011年以后的UEFI BIOS中，想要改装预装Win8/10的电脑，必须要了解下这个概念
Secure Boot是UEFI BIOS的一个子规则，位于传统(Legacy)BIOS的BOOT选项下
微软规定，所有预装Win8/10操作系统的厂商(即OEM厂商)都必须打开Secure Boot(在主板里面内置Win8/10的公钥)。
部分主板该选项是Secure Boot Contrl，位于Security选项下。预装Win8/10系统电脑，一旦关闭这个功能(将其设置为“Disabled")，将导致无法进入系统。
* Secure BOOT设计之初作用是防止恶意软件侵入。事实上它能够做到的仅仅是，当电脑引导器被病毒修改之后，它会给出提醒并拒绝启动，避免可能带来的进一步损失。
更多的人认为，这是微软为了防止安装Windows操作系统的电脑改装linux。客观的讲，微软设计Secure Boot的原本用意可能是出于保证系统安全，但结果似乎成了PC厂商保护市场垄断、阻碍竞争的一种手段。

## ATOM&VIM设置tab为四个space
* ATOM:Packages->settings view->open
点击左侧Editor，在右侧勾选Soft Tabs. Tab Length 填入4. Tab Type选择soft
* VIM:在当前用户~目录下新建.vimrc文件，填入下面三行
```
:set ts=4
:set expandtab
:set autoindent
```




## linux下字体
复制windows中c:/windows/fonts 下所有文件到linux中HOME目录下的.fonts文件夹中(无则新建)
安装wps后，接着在官网下载wps fonts安装包安装。这样基本论文格式就是正确的了

## 任何一条java命令出现 Picked up \_JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=gasp 
系统原有的OpenJDK设置干扰了手动安装的JDK。干扰的文件是：/etc/profile.d/java-awt-font-gasp.sh
删除即可
## Deepin 15.4 笔记本安装显卡驱动(I卡+N卡)
linux下有两种N+I双卡的方案，一个是ubuntu下的nvidia-prime，另一个就是debian下的bumblebee。目前deepin基于debian，而debian只打包了第三种bumblebee，所以只能用后者。
ubuntu的nvidia-prime如果要切换显卡，必须要重启X session，因为在X启动的时候nvidia的驱动模块就已经加载了，也就是说独显是一直在工作的。而debian采用bumblebee，开机加载的是intel的驱动，程序默认也都是用intel的集显，如果需要用独显要用optirun运行程序，这样能做到最大程度的提高电池续航能力。目前debian的nvidia-driver，nvidia-legacy-driver都是默认bumblebee解决双显卡，X启动时，驱动是intel，glx是mesa的glx，但是有些硬件可能会出现驱动是intel，glx却是nvidia的情况，这就会导致opengl的程序跑不起来，需要手动执行sudo update-alternatives --config glx来选择。
两种实现其实各有利弊，debian当前也没有打包prime的打算打包方式不同，debian这边没有打包适配prime的驱动，加prime支持要改东西太多，所以就只用大黄蜂了。
* 删除以往所有安装的驱动/大黄蜂
```
sudo apt remove --purge nvidia*
sudo apt remove --purge bumblebee*
```
* 打开驱动管理器，点击驱动进行安装
* 再确保安装一些相关软件
```
sudo apt-get install bumblebee-nvidia bumblebee primus
sudo apt-get install nvidia-driver nvidia-settings
```
* 检查驱动是否安装成功,安装mesa-utils这个包，用来显示系统的glx相关信息。用optirun调用独显输出系统的glxinfo来查看驱动是否安装成功。
```
sudo apt-get install mesa-utils
optirun glxinfo|grep NVIDIA
```
* 使用独显启动程序
```
optirun command #使用独显运行command程序
primusrun command #另外一种使用独显运行程序方法
optirun -b primus command #使用独显运行command程序,提升性能
```
* 可以安装 nvidia-smi，sudo nvidia-smi执行之，可以看到驱动已经安装，设备正常运转
* 测试 Bumblebee 是否支持你的 Optimus 系统:如果在终端中看到一个关于你的 Nvidia 的提示和齿轮，恭喜你，Bumblebee 和 Optimus 已经开始工作了。
```
optirun glxgears -info
```
* 执行 sudo lspci -v 可以查看到详细信息
* 在使用optirun命令开启独显前，可以使用lspci命令查看设备，可以看到独显最后的信息为(rev ff).ff代表设备关闭
使用optirun命令后，再次lspci，可以看到独显最后的信息为(rev a2)，这代表独显已经正常使用了
* bumblebee配置下所有需要独显的操作都需要调用optirun或者primusrun.如果打开nvidia-settings时提示“You do not appear to be using the NVIDIA X driver”,在terminal中运行如下命令
```
optirun -b none nvidia-settings -c :8
```
这里解释一下上述命令.man optirun可以看到如下关于-b的信息
```
-b, --bridge METHOD
              acceleration/displaying  bridge to use. Valid values are auto, virtualgl and primus. The --vgl-* options only make sense
              when using the virtualgl bridge, while the --primus-* options apply only when using the  primus  bridge.   Additionally,
              value  none is recognized, and its effect is to add paths to driver libraries to LD_LIBRARY_PATH (useful for nvidia-set‐
              tings and CUDA applications)
```
man nvidia-settings可以看到如下信息
```
-c CTRL-DISPLAY, --ctrl-display=CTRL-DISPLAY
              Control the specified X display.  If this option is not given, then nvidia-settings will control the display specified by '--dis‐
              play' ; if that is not given, then the $DISPLAY environment variable is used.
```
由此可见-c后面跟的:8是屏幕显示的代码。查看bumblebee文件
```
sudo gedit /etc/bumblebee/bumblebee.conf
```
可以发现如下一段，由此可以解释上述命令了。
```
[bumblebeed]
# The secondary Xorg server DISPLAY number
VirtualDisplay=:8
```
* 在steam中使用独显来运行游戏，在游戏上右键，set launch options中加入
```
optirun -b primus %command%
```
* 开机黑屏拯救方法
```
sudo apt remove --purge nvidia*  //适用于安装了不正确的闭源驱动
sudo rm /etc/X11/xorg.conf  //适用于生成了不正确的xorg.conf文件
sudo apt install xorg  //适用于删除nvidia时误删了xorg
```
* reference:
1: https://wiki.deepin.org/index.php?title=%E6%98%BE%E5%8D%A1
2：https://wiki.archlinux.org/index.php/Bumblebee
##
dpkg -S （察看某个文件/软件属于哪个软件包）
dpkg -S driver-manager
apt-cache policy mintdrivers
二，apt-cache search package_name
搜索软件包，可以按关键字查找软件包,通常用于查询的关键字会使用软件包的名字或软件包的一部分.

三，apt-cache showpkg package_name
显示软件包的依赖关系信息.

四，apt-cache stats
显示当前系统所使用的数据源的统计信息,用户可以使用该命令查看数据源的相关统计信息.

五，apt-cache policy package_name
显示软件包的安装状态和版本信息.


sudo docker run -d -i -t debian /bin/bash



go import _ 操作其实是引入该包，而不直接使用包里面的函数，而是调用了该包里面的init函数
一般在导入数据库驱动时常见，是为了不直接使用驱动中的函数，保持驱动独立性

## 第一次操作gerrit
2001  ls
2002  git remote  -v
2003  git remote add gerrit https://your/repository/address
reference : http://developerworks.github.io/2014/08/22/gerrit-workflow/

2004  git status
2005  git log
2006  git status
2007  git pull
2008  git branch -a
2009  git review master
2011  apt-cache search ssh-keygen
//生成ssh秘钥
2012  ssh-keygen -t rsa -C "wallena3@gmail.com"
2013  cat ~/.ssh/id_rsa.pub
2014  git review master
2015  git remote -v
2016  git remote set-url gerrit ssh://wallena3@cr.deepin.io:29418/deepin-build-service
2017  git review master
2018  history


 修补上次commit
 2033  git commit  --amend

创建新分支
  2002  git checkout -b database


sudo docker  build -t edwardsbean/centos6-jdk1.7  .

必须先启动服务
sudo mongod --auth --dbpath /var/lib/mongodb
db.repository.find()
db.repository.insert({name:"first",description:"k",creator:"kkkk"})
db.collection_name.drop()


ssh 允许root登录

vi /etc/ssh/sshd_config

将PermitRootLogin值改yes


当存在冲突,首先进回收站,再pull,再pop解决冲突
git stash
 2006  git pull origin master
 2007  git stash pop



用HEAD表示当前版本，，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
简单总结一下，其实就是--soft 、--mixed以及--hard是三个恢复等级。使用--soft就仅仅将头指针恢复，已经add的缓存以及工作空间的所有东西都不变。如果使用--mixed，就将头恢复掉，已经add的缓存也会丢失掉，工作空间的代码什么的是不变的。如果使用--hard，那么一切就全都恢复了，头变，aad的缓存消失，代码什么的也恢复到以前状态。

现在，我们要把当前版本A ，回退到上一个版本“B”，就可以使用git reset命令：
2032  git reset --hard HEAD~


从远程仓库master合并代码到本地分支
git checkout master
git pull
git checkout react
git rebase master

< a href="url 302到下载地址" download >下载< / a >
url里统计，然后重定向的下载链接
添加 download 属性，这个属性可以指定下载文件名。
## 07.20代码
### 在每行加入链接
const priceFormatter = (cell, row) => (
    \`<a href=http://baidu.com>\${cell}</a>\`
)
dataFormat = { priceFormatter }


const RepoList = ({ repoList, error, loading, onAfterInsertRownew}) => {

    const options = {
        afterInsertRow: onAfterInsertRownew
    }

    const cellEditProp = {
      mode: 'click',
      blurToSave: true
    }


    if (loading) {
        return <h>loading...Please wait</h>
    }
    if (error) {
        return <h>ERROR!  {error}</h>
    }
    if (!repoList || repoList.length <= 0) {
        return <h>Empty</h>
    }
    return (
        <BootstrapTable data={repoList} striped hover bordered condensed pagination cellEdit={ cellEditProp } insertRow={ true }
        options={ options }>
            <TableHeaderColumn dataField='name' isKey>Repo name</TableHeaderColumn>
            <TableHeaderColumn dataField='creator' dataFormat = { priceFormatter }>Creator</TableHeaderColumn>
            <TableHeaderColumn dataField='description'>Description</TableHeaderColumn>
        </BootstrapTable>

    )
}
更新表格:
afterSaveCell

```

            //this will add(update) after state.reopList.repoIndex
            //also will ease the top level of state
            // return state.repoList.map(repoIndex =>
            //     (repoIndex._id === action._id)
            //         ? {...repoIndex, error: action.error, distributions: action.distributions}
            //         : repoIndex
            //     )
            //        relist[someid]  disna[codename]
            //state.first.second[someId].fourth
                // return {
                //     ....state,
                //     first : {
                //         ...state.first,
                //         second : {
                //             ...state.first.second,
                //             [action.someId] : {
                //                 ...state.first.second[action.someId],
                //                 fourth : action.someValue
                //             }
                //         }
                //     },
                //     error: state.error,
                //     loading: state.loading
                // }
                //

                //{}返回的是对象形式,[]为数组
```
# 提交到greeit的代码又回老版本变了
如果提交review的代码不是最新的,提交上去会重新覆盖服务器上部署的代码
此时需要在greeit上把这次的代码点rebase到master,变成最新的代码就行了
每次提交前,都切换到master,pull, rebase到最新的代码再提交

```
目前deepin的仓库中，安装docker.io是最新的版本，docker-engine为老版本  
但docker.io也已经过时，是1.13的版本，最新的docker-ce为1.17，deepin还没有加入仓库。
systemctl unmask docker.service
systemctl unmask docker.socket
systemctl start docker.service
```

## 各位是怎么解决React App中Store数据不能持久化的？页面不能被F5刷新，否则所有对Store中状态的修改都还原？
数据的持久化简单可以分为两级，当前页面的状态和当前用户的状态
当前页面的状态，典型的例子就是筛选条件，刷新之后需要保持之前的筛选状态，也是常见的需求.
我目前的做法是处理筛选动作的函数中，除了 dispatch 对应的 action，再去 dispatch(push('xxx'))，
(可以把重要数据比如repoid,加入地址栏中,这样在页面里就可以根据地址栏网址取到参数
yield put(push(\`/repos/\${action.repoid}/CertainRepoDetailsPage/\`))
)
把筛选状态附加到 queryString 中，也可以理解为存储到了 store 的 route 状态中，然后每次页面渲染前都会优先检查下 queryString 中的条件。
当前用户的状态，例子就是 access_token，老老实实存在 localStorage 就好了，需要的时候去取。

## Linux TCP拥塞技术 BBR
kernel 4.9版本后的内核新增了一款TCP拥塞控制技术：BBR  
sudo gedit /etc/sysctl.conf  
在 /etc/sysctl.conf  最后面加入下面两行，开启内核BBR的配置  

net.core.default_qdisc=fq  
net.ipv4.tcp_congestion_control=bbr  
保存退出。  

让刚才配置的 sysctl.conf 现在生效  
sudo  sysctl -p  

检查bbr是否生效：  
sudo sysctl net.ipv4.tcp_available_congestion_control  
如果输出  
net.ipv4.tcp_available_congestion_control = bbr cubic reno  
而且  
sudo lsmod | grep bbr  
有 tcp_bbr 进程 号 ，说明开启成功了。  

## Deepin15.4.1安装cuda，cudnn
sudo apt install nvidia-cuda-toolkit nvidia-cuda-dev  
参考官网，使用pip3方式安装tensorflow-with-gpu  
此时运行官网示例，会发现提示缺少相应版本的cudnn  
下载官网的cudnn  
使用 dpkg -L nvidia-cuda-dev, 可以看到头文件都在/usr/include, .so和.a文件都在/usr/lib/x86_64-linux-gnu/  
将cudnn的对应文件复制到这两个文件夹下即可  
ps:如此操作后，在执行软件的删除安装等操作时可以看到如下错误  
libcudnn.so.6 is not a symbolic link  
因为把cudnn的文件直接复制到上述两个文件夹会丢失符号链接  
libcudnn.so.6正常情况下应该是一个符号链接,而不是实体文集件,修改其为符号链接即可  
重新覆盖 添加软链接  
sudo ln -sf libcudnn.so.6.0.21 libcudnn.so.6  
sudo ln -sf libcudnn.so.6 libcudnn.so  

## Deepin15.4.1安装docker报错  
```
Job for docker.service failed because the control process exited with error code.  
See "systemctl status docker.service" and "journalctl -xe" for details.
```
解决方法：  
修改docker.service内容：  
```
sudo vim /lib/systemd/system/docker.service
```
将  
ExecStart=/usr/bin/dockerd -H fd:// 改成  
ExecStart=/usr/bin/dockerd -H fd:// -s overlay2   
然后执行  
```
sudo systemctl daemon-reload  
sudo systemctl restart docker  
```

## 删除github仓库中不需要的文件夹  
```
git rm -r --cached some-directory
git commit -m "Remove the now ignored directory some-directory"
最后push~
```

## 构建deb包
* 安装debhelper dh-make  
* 创建GPG key
* dh_make -f ../linux-magic-box-0.1.tar.gz  
在源码文件夹中执行，初始化debian文件夹  
* 修改debian文件夹下相应文件control，用于构建deb包  
* sudo dpkg-buildpackage  
ref: https://www.debian.org/doc/manuals/maint-guide/  
http://blog.csdn.net/mountzf/article/details/51863859  
http://blog.csdn.net/sky_qing/article/details/8477728  

#### git使用
git clean -fd  删除未跟踪文件
git add .
git reset --hard HEAD 回退版本信息
切换host，在linux下一定要加入127.0.0.1对应你主机名的一行，否则在工程启动的时候，
会报错暂时无法解析域名，导致工程起不来 
#### atom总是自动去除md文件中最后的行空格  
包管理中找到whitespace，去掉最后的remove space的勾选
## JavaScript 继承的几种方式  
### ES6中extends关键字
### ES6之前:原型链
\`\`\`
function SuperType(){
    this.prototype=true;
}
SuperType.prototype.getSuperValue=function(){
    return this.property;
}
function SubType(){
    this.subproperty=false;
}
//通过创建SuperType的实例继承了SuperType
SubType.prototype=new SuperType();

SubType.prototype.getSubValue=function(){
    return this.subproperty;
}
var instance=new SubType();
alert(instance.getSuperValue());  //true
\`\`\`
原型链存在两个问题  
* 包含引用类型值的原型属性会被所有实例共享，这会导致对一个实例的修改会影响另一个实例。
* 在创建子类型的实例时，不能向超类型的构造函数中传递参数。由于这两个问题的存在，实践中很少单独使用原型链。
### ES6之前:借助构造函数
借用构造函数的基本思想，即在子类型构造函数的内部调用超类型构造函数。
函数只不过是在特定环境中执行代码的对象，因此通过使用apply()和call()方法可以在新创建的对象上执行构造函数。如下例子
\`\`\`
function SuperType(){
    this.colors=["red", "blue", "green"];
}
function SubType(){
    //继承SuperType
    SuperType.call(this);
}
var instance1=new SubType();
instance1.colors.push("black");
alert(instance1.colors);  //red,bllue,green,black

var instance2=new SubType();
alert(instance2.colors);  //red,blue,green
\`\`\`
上面例子中，通过使用call()方法（或者apply()方法），在新创建的SubType实例的环境下调用了SuperType构造函数。  
这样一来就会在新的SubType对象上执行SuperType()函数中定义的所有对象初始化代码。结果，SubType的每个实例都会有自己的colors属性副本。  
相对于原型链而言，借用构造函数可以在子类型构造函数中向超类型构造函数传递参数。如下例子
\`\`\`
function SuperType(name){
    this.name=name;
}
function SubType(){
    //继承了SuperType,同时还传递了参数
    SuperType.call(this,"mary");
    //实例属性
    this.age=22;
}
var instance=new SubType();
alert(instance.name);  //mary
alert(instance.age);  //29
\`\`\`
借用构造函数存在两个问题：  
* 无法避免构造函数模式存在的问题，方法都在构造函数中定义，因此无法复用函数。  
* 在超类型的原型中定义的方法，对子类型而言是不可见的。因此这种技术很少单独使用。  
### ES6之前:组合继承
组合继承，指的是将原型链和借用构造函数的技术组合到一起。思路是使用原型链实现对原型方法的继承，而通过借用构造函数来实现对实例属性的继承。  
这样，既通过在原型上定义方法实现了函数的复用，又能够保证每个实例都有它自己的属性。以下例子充分说明了这一点
\`\`\`
function SuperType(name){
    this.name=name;
    this.colors=["red", "blue", "green"];
}
SuperType.prototype.sayName=function(){
    alert(this.name);
};
function SubType(name, age){
    //继承属性    使用借用构造函数实现对实例属性的继承
    SuperType.call(this,name);
    this.age=age;
}
//继承方法     使用原型链实现
SubType.prototype=new SuperType();
SubType.prototype.constructor=SubType;
subType.prototype.sayAge=function(){
    alert(this.age);
};
var instance1=new SubType("mary", 22);
instance1.colors.push("black");
alert(instance1.colors);   //red,blue,green,black
instance1.sayName();  //mary
instance1.sayAge();  //22

var instance2=new SubType("greg", 25);
alert(instance2.colors);   //red,blue,green
instance2.sayName();  //greg
instance2.sayAge();  //25
\`\`\`
这个例子中，两个实例既分别拥有自己的属性，包括colors属性，又可以使用相同的方法。  
组合继承避免了原型链和借用构造函数的缺点，融合了他们的优点，是JavaScript中最常用的继承模式。

## asmlinkage在linux内核中的应用
asmlinkage作用就是告诉编译器，函数参数不是用用寄存器来传递，而是用堆栈来传递的；  
原因是因为用户态寄存器在系统调用进入内核态时，会把用户态的寄存器全部压栈，通过合理的构造。正好满足用户态通过寄存器传递参数，  
内核态通过栈取参数的标准要求。这是很巧妙的安排，是高效的体现！其实还可以发现，内核只有在系统调用时才用asmlinkage，其它函数都没有。这是有意而为之的。  
\`\`\`
<<https://kernelnewbies.org/FAQ/asmlinkage
The asmlinkage tag is one other thing that we should observe about this simple function.   
This is a #define for some gcc magic that tells the compiler that the function should not expect to find any of its arguments in registers (a common optimization),   
but only on the CPU's stack. Recall our earlier assertion that system_call consumes its first argument, the system call number,  
and allows up to four more arguments that are passed along to the real system call.   
system_call achieves this feat simply by leaving its other arguments (which were passed to it in registers) on the stack.   
All system calls are marked with the asmlinkage tag, so they all look to the stack for arguments.   
Of course, in sys_ni_syscall's case, this doesn't make any difference, because sys_ni_syscall doesn't take any arguments,   
but it's an issue for most other system calls. And, because you'll be seeing asmlinkage in front of many other functions,   
I thought you should know what it was about.  

It is also used to allow calling a function from assembly files.  
\`\`\`


x86架构的定义位置如下，架构不同目录不同
./arch/x86/include/asm/linkage.h  
定义如下
\`\`\`
#define asmlinkage CPP_ASMLINKAGE __attribute__((regparm(0)))
\`\`\`
__attribute__是关键字，是gcc的C语言扩展，regparm(0)表示不从寄存器传递参数
如果是__attribute__((regparm(3)))，那么调用函数的时候参数不是通过栈传递，  
而是直接放到寄存器里，被调用函数直接从寄存器取参数

* "asmlinkage" 是在 i386 system call 实作中相当重要的一个 gcc 标签（tag）。
当 system call handler 要调用相对应的 system call routine 时，便将一般用途缓存器的值 push 到 stack 里，  
因此 system call routine 就要由 stack 来读取 system call handler 传递的参数。这就是 asmlinkage 标签的用意。所以一般可以看到，系统调用的一些函数都标记有asmlinkage标记  
 
* system call handler 是 assembly code，system call routine（例如：sys_nice）是 C code，当 assembly code 调用 C function，  
并且是以 stack 方式传参数（parameter）时，在 Cfunction 的 prototype 前面就要加上 "asmlinkage"。  
加上 "asmlinkage" 后，C function 就会由 stack 取参数，而不是从 register 取参数（可能发生在程序代码最佳化后）。

## 阿里云内核组面试
### 快表和高速缓存区别
快表更靠近cpu  
快表是单独的寄存器，页表是存在于主存。TLB又称页表缓存，为了加速页表查询的。根据执行步骤：  
当CPU执行机构收到应用程序发来的虚拟地址后，首先到TLB中查找相应的页表数据，如果TLB中正好  
存放着所需的页表，则称为TLB命中（TLB Hit）,接下来CPU再依次看TLB中页表所对应的物理内存地址  
中的数据是不是已经在一级、二级缓存里了，若没有则到内存中取相应地址所存放的数据。可以看出TLB是单独的寄存器。  
ref:https://blog.csdn.net/u014609236/article/details/39472115  讲述了tlb/cache原理  
### 为什么要有虚拟地址和物理地址转化?说说分页机制  
虚拟地址->线性地址->物理地址  
           分段机制    分页机制  
线性地址= 虚拟地址对应段基地址+偏移，linux中段基地址都为0，所以线性地址就是虚拟地址，是一个偏移量  

虚拟就是虚拟的，不是实际真是的物理地址。你可以认为，这两个地址之间没关系。  
这个虚拟是通过系统和硬件的双重工作，做的一种点对点的映射（当然实际内存分配是按照页来处理）。  
也就是软件不需要考虑内存数据的物理地址，只需要用虚拟地址做数据存储处理就行了。  

这样一个好处是，软件不需要自己做内存分配，也不需要考虑别的软件的内存占用问题。  
操作系统会根据当前的内存使用情况，动态的分配内存空间。虚拟内存地址还一个好处是因为是虚拟的，  
所以内存并不一定非要在物理内存中。可以存放在任何位置，比如把暂时不用的数据放进硬盘上的虚拟内存，  
腾出真实的物理内存交给程序运行而提高多程序时运行的效率。而且因为每个软件的虚拟内存地址都是从 0 开始，    
每个软件的寻址都是独立而且顺序的。程序编写和运行时，都好像是机器里面只有自己一个程序在运行，  
程序开发起来也很容易。软件不需要考虑内存分配的问题，也不需要担心内存不足和两个程序抢同一片内存导致系统整个崩溃的情况。

关于分页的ref:  
https://blog.csdn.net/gatieme/article/details/52403013  
https://www.2cto.com/kf/201609/545015.html  
https://www.2cto.com/kf/201708/670791.html  
http://blog.163.com/huawei_d/blog/static/211610257201321054753140/  
### linux系统可用内存基本快没有了，你会认为是系统有什么问题吗？  
不一定是  
Linux与Windows不同，会存在缓存内存，通常叫做Cache Memory。有些时候你会发现没有什么程序在运行，但是使用top或free命令看到可用内存会很少。  
什么是Cache Memory(缓存内存)：  
当你读写文件的时候，Linux内核为了提高读写效率与速度，会将文件在内存中进行缓存，这部分内存就是Cache Memory(缓存内存)。  
即使你的程序运行结束后，Cache Memory也不会自动释放。这就会导致你在Linux系统中程序频繁读写文件后，你会发现可用物理内存会很少。  
其实这缓存内存(Cache Memory)在你需要使用内存的时候会自动释放，所以你不必担心没有内存可用。  
如果你希望手动去释放Cache Memory(缓存内存)的话也是有办法的。    

ref: https://blog.csdn.net/wangcg123/article/details/52625335  

### linux 查看内存，处理器的方法？
top, free, cat /proc/meminfo, ps aux, vmstat  

### c语言内存分配是怎样的？（堆/栈）  
代码段， 堆， 栈， 数据段（已初始化， 未初始化（BSS）, 常量区(只读)）  
ref:  
https://blog.csdn.net/zorelemn/article/details/52566600   
https://blog.csdn.net/gzg1500521074/article/details/50444845  
https://www.cnblogs.com/tuhooo/p/7221136.html  

### C++ STL中stack queue等容器的常用方法？  
### ELF文件是什么（目标文件）?  
1）可重定向文件：文件保存着代码和适当的数据，用来和其他的目标文件一起来创建一个可执行文件或者是一个共享目标文件。（目标文件或者静态库文件，即linux通常后缀为.a和.o的文件）  
2）可执行文件：文件保存着一个用来执行的程序。（例如bash，gcc等）  
3）共享目标文件：共享库。文件保存着代码和合适的数据，用来被下连接编辑器和动态链接器链接。（linux下后缀为.so的文件。）  
http://blog.csdn.net/gatieme/article/details/51615799
https://www.cnblogs.com/20135223heweiqin/p/5554922.html

### c语言原子操作，linux锁机制？  内存屏障？？
volatile   
前面有人说volatile可以保证对内存操作的原子性，这种说法不大准确，  
其一，x86需要LOCK前缀才能在SMP下保证原子性，其二，RISC根本不能对内存直接运算，要保证原子性得用别的方法，如atomic_inc。  
linux 锁：  
spinlock  信号量。

### c语言怎么实现面向对象操作？  
结构体 封装      结构体嵌套  继承  
在c语言中我们可以用万能指针void*来实现多态。  

比如，在Ｌinux下大名鼎鼎的图形界面GNOME，就是通过Ｃ语言模拟的面向对象特性来实现的。   
其实大名顶顶的GObject系统，  
也就是Linux主流桌面系统之一gnome的基石。  
关于如何用C语言实现访问权限，可以参考 gobject的手册  
私有成员是通过公开头文件中指向私有数据的指针，和把私有数据设置为static存储类型来实现的。  
ref :  
http://blog.csdn.net/learning_zhang/article/details/52474704  
http://blog.csdn.net/mormont/article/details/52973514  

### c语言中静态全局变量、静态局部变量、全局变量、局部变量、宏?  
http://blog.csdn.net/q626992497q/article/details/45644791  
https://www.cnblogs.com/lanjianhappy/p/6035433.html   