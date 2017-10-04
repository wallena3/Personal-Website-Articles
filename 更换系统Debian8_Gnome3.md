## 添加普通用户为sudoer
需要注意，Debian默认没有安装sudo ，apt-get install sudo  
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

语言问题  

如果一开始使用的是英语或者汉语，现在想切换，那就运行  
sudo dpkg-reconfigure locales  
找到想要的语言，空格键选中，tab键切换到ok，然后移动光标到想要使用的主语言，tab键切到ok。  
然后还要在设置中的"区域与语言"中把语言改成英文或中文。注销即可。  
