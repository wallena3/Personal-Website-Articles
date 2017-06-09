本文章写一些wordpress使用时常见的问题。都是自己亲自实践过的，绝对有效，方便自己和大家查阅，长期更新。



1：更新翻译或安装模板提示登陆ftp，并且提示“无法创建目录”或无法定位wp-content

出现这个的问题就是执行身份非文件属主身份。sudo chmod -R 777 /var/www/ 增加权限。

还需要修改下Wordpress的配置文件,wp-config.php，加入这么一行：

define('FS_METHOD', "direct");

然后再进入后台，点击升级，发现升级成功了。

2：提示“在裁剪您的图像时发生了错误”

问题：在WordPress中使用裁剪图片功能时，出现:”在裁剪您的图像时发生了错误。”或者”There has been an error cropping your image.”

原因：缺少PHP GD库。

Ubuntu下运行：

sudo apt-get install php5-gd
CentOS下运行：

yum install php-gd
安装PHP-GD库后重启httpd（Apache）服务即可使用

参考链接：http://blog.moemiku.com/

3：首页中侧边栏文章分类的链接打不开，显示404

这是由于wordpress的编码造成的。在后台的文章->分类目录中，把别名，即URL中显示的名字改为英文即可。

4:wordpress注册用户时收不到邮件提醒

注册WordPress或别人评论时收不到邮件提醒的问题。使用WordPress 的WP SMTP插件来解决邮件接收问题，即使主机支持mail()函数也需要使用SMTP插件，因为mail()函数经常失效或者邮件进入垃圾箱的问题。安装完插件填写表格即可。

选区_007

网易各个邮箱 POP3 和 SMTP 服务器地址设置如下：

邮箱	POP3 服务器（端口110）	SMTP 服务器（端口25）
188.com	pop3.188.com	smtp.188.com
163.com	pop3.163.com	smtp.163.com
126.com	pop3.126.com	smtp.126.com
netease.com	pop.netease.com	smtp.netease.com
yeah.net	pop.yeah.net	smtp.yeah.net
所有的SMTP服务器都需要身份验证。
QQ邮箱 POP3 和 SMTP 服务器地址设置如下：

邮箱	POP3服务器（端口995）	SMTP服务器（端口465或587）
qq.com	pop.qq.com	smtp.qq.com
SMTP服务器需要身份验证。

5：紧随上个问题。WordPress“您的密码重设链接无效，请在下方请求新链接。”

当注册WordPress帐户时进行邮箱验证，邮箱成功收到邮件，但点击邮件链接后结果发现显示“您的密码重设链接无效，请在下方请求新链接。”。

选区_008

选区_009

其实是邮箱发送的地址后面多了个”>”号，本来是WordPress为了美观，前后加上了尖括号，结果适得其反，被邮箱解析到地址里面去了，点击后自然会是无效的了，去掉尖括号即可。

修改网站目录下wp-login.php文件。

找到327行

$message .= '<' . network_site_url("wp-login.php?action=rp&key=$key&login=" . rawurlencode($user_login), 'login') . ">rn";
改成

$message .= network_site_url("wp-login.php?action=rp&key=$key&login=" . rawurlencode($user_login), 'login') ;
注意：当wp升级后会失效，因为升级后wp-login.php会被替换，需要重新修改wp-login.php

经过测试，在4.4.2版本下这种修改只能使已注册用户在重置密码时所收到的邮件正常。而对于新注册的用户收到的邮件地址依旧不正常，qq邮箱会错误解析激活的地址，而新浪邮箱则可以正确访问。这对大量qq用户来说依旧不方便。故寻求另外的解决方法：DX Login Register 插件以及Pie Register插件。
