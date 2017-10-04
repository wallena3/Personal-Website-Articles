* edit profile  
```
sudo gedit /etc/profile
```
add the following code in the end of this file
```
#JAVA ENVIROMNET PATH
export PATH=/home/walle/JDK/jdk1.8.0_92/bin:$PATH
```
ATTENTION:  
It is different to configure PATH on LInux from Windows.Windows use ';' to  separate each path,but unix use ':'  

The corrent version  of JDK only need to configure ONLY ONE parameter--PATH.CLASSPATH and JAVAHOME are never be needed.  

* Now let's configure the real path of jdk when you type java/javac in terminal.  

Input the following command,change the JDK directory to the corrent directory.  

sudo update-alternatives --install /usr/bin/java java /home/walle/JDK/jdk1.8.0_92/bin/java 300  
The command above is to add the JDK directory to command java's alternative list.300 is the priority.The smaller priority is ,the power is bigger.Terminal will decide which directory to use according to the alternative list.If the highest power directory is not exits,it will try to use the secondly one.
Like above,command javac need the similar command.

sudo update-alternatives --install /usr/bin/javac javac /home/walle/JDK/jdk1.8.0_92/bin/javac 300
5:When there are many version of jdk on your PC,you should type the following command to change 'java' command to the highest priority one.

sudo update-alternatives --config java
Type the number you want to use.

By the same way,'javac' should also use these commands to configure.

sudo update-alternatives --config javac
If you only have one version of JAVA,then you needn't to configure it.

6:If you want the profile take effect immediately,you can type the commands below.

source /etc/profile
java -version
javac -version
Finally,check the Java version.


* 编辑profile
```
sudo gedit /etc/profile
```
在最后加入:
```
#JAVA ENVIRONMENT PATH
export PATH=/home/walle/JDK/jdk1.8.0_92/bin:$PATH
```
注意:  
linux和windows平台在PATH设置上的不同：windows是以分号;来分隔path,而unix是以冒号:来分隔path。  
当前版本的JDK只需设置一个PATH参数即可，JAVAHOME以及Classpath都可以不设置了。

* 现在设定在终端输入java/javac时，实际对应的java档案。输入以下指令，修改JDK目录名称为要使用的目录。
```
sudo update-alternatives --install /usr/bin/java java /home/walle/JDK/jdk1.8.0_92/bin/java 300  
```
这段命令是将刚才手动复制到/home/walle/JDK中的JDK目录位置加入'java'这个指令的候选名单中，300为优先级，数字越小权限越高。终端会根据这个候选名单决定'java'这个指令实际对应哪个目录。如果较高优先权的路径不存在则尝试次高优先级路径。  
同样，javac也要使用类似的指令指定。  
```
sudo update-alternatives --install /usr/bin/javac javac /home/walle/JDK/jdk1.8.0_92/bin/javac 300
```
* 当机器中存在多个版本的java时需要输入下面的命令来更改。继续输入以下指令，指定'java'指令最优先对应的路径
```
sudo update-alternatives --config java
```
输入要优先自动使用的目录编号,同样，javac也要使用类似的指令指定。
```
sudo update-alternatives --config javac
```
若候选名单只有一个路径则不需要选择编号。
* 使环境变量立刻生效，输入以下指令查看java版本。
```
source /etc/profile
java -version
javac -version
```
