很多网站以及在线播放视频都需要flash插件，但是ubuntu下并没有安装包，我们只能去adobe的官网下载flash的tar版本手动安装。下面给出地址。adobe声明为linux开发的版本11.2将是最后一个，但将来会持续提供安全更新。

https://get.adobe.com/flashplayer/?promoid=KLXMF
在这里我们要选择tar.gz for linux。注意 下面ubuntu版本的已经太老不能用了。下载下来后解压。可以看到libflashplayer.so这个文件以及/usr文件夹。下面进行下面两部：

1：将libflashplayer.so复制到你系统下firefox的插件目录。我现在用的是ubuntu16.04。位置是/usr/lib/firefox-addons/plugins

cp libflashplayer.so /usr/lib/firefox-addons/plugins
2：将/usr文件夹下内容全部复制到系统的/usr文件夹下

sudo cp -r usr/* /usr
大功告成！
