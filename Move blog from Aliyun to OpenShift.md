迁移博客从阿里云到OpenShift





再过半个月左右，阿里云的空间就到期了，为了省点费用，就搬到了RedHat旗下的Openshift下。一直听说质量还是不错的，但是和一个真正的服务器（阿里云）相比还是有很多差别的。下面记一些注意事项。

1：首先要有一个红帽的账号，用于OpenShift的登陆，在官网登陆完后就可以申请空间了。选择Add application。在出现的界面中可以看到很多选项，点进去就可以说是傻瓜的操作方式了，按照指示进行操作就行了。这点来说比自己搭建服务器环境方便很多，建站非常快速。再页面的右下角有一个DIY，点击去可以进行自己想要的配置。没有尝试过这种配置。

2：现在一个红帽的二级域名已经自动的绑定了你刚刚创建的application。但是我有自己一直在用的域名，openshift提供免费的顶级域名绑定服务，为你刚刚建立的application添加一个alias即可进行绑定。一般都添加两项，就本站来说一个为www.wallen.top，另一项为wallen.top。

3：现在还需要在域名DNS管理处--即万网处进行解析的修改。这里要将原来的A记录改为CNAME记录。CNAME记录是指：如果将域名指向一个域名，实现与被指向域名相同的访问效果，需要增加CNAME记录。同样，这里一般也要加入两条记录，记录类型都为CNAME。主机记录就是域名的前缀，一个为空（在阿里云中体现为符号@），另一个为www。解析线路皆为默认。最后的记录值填入红帽提供的二级域名即可。

4:进入wordpress后台，在设置中的Wordpress地址（后台地址），以及站点地址（前台地址）中都加入www.wallen.top即可。这里会有一个问题。OpenShift空间会给Wordpress后台登录地址强制使用Https。此时会显示

wallen.top uses an invalid security certificate.
The certificate is only valid for the following names: *.rhcloud.com, rhcloud.com
Error code: SSL_ERROR_BAD_CERT_DOMAIN
这里有两个解决方案，一是添加安全例外即可。二是将Wordpress地址（后台地址）改为OpenShift提供的二级域名即可（推荐），但是这种方法会造成后台登陆密码无法存储。

4.5：在上述步骤中将站点地址设为http://www.wallen.top后，输入www.wallen.top可以正常访问，但是wallen.top则不能访问，提示‘此网页包含重定向循环’。这个可以通过修改以下文件中即可解决首页包含过多重定向的问题。文件位于wp_includes文件夹下。首先，对于canonical.php文件的修改，利用文本编辑器打开该文件，开头注释下面就可以找到如下语句：
function redirect_canonical( $requested_url = null, $do_redirect = true ) {
把true修改为false即可。

注：在OpenShift的主机目录中，wordpress的默认安装根目录为 /app-root/repo/php

5:现在进行备份。在旧站的后台中的工具->导出中进行数据的导出，为一个xml格式的文件。然后在新站中在工具->导入中使用WordPress 导入工具 插件进行导入即可。但是图片要在多媒体中挨个保存，很麻烦，不知道有没有更好的办法。

6:在OpenShift中进行SSH连接的配置工作。在你的application中添加秘钥。此时OPenshift会要求你填入rsa秘钥。首先要再本机生成一个rsa秘钥。在terminal中输入

ssh-keygen –t rsa
然后一路回车即可。生成的秘钥在当前用户主目录下的.ssh文件夹中。其中id_rsa.pub为将要输入的秘钥。添加后即可。在应用的右侧的Source Code下的字符串即为可以进行ssh链接的地址。除去一些需要root权限的命令无法执行，其余命令基本都可以执行，基本等同VPS主机。这是就可进行ssh链接了。

7：OpenShift的目录结构与服务器主机不同，还需要进一步学习。

至此站点迁移已经完毕。

/****************************************************************************************************/

折腾过后，事实证明这东西没有毛用

接下来进行CDN加速的尝试，由于过去是国内阿里云的主机，所以还好，现在变成了OpenShift，CDN加速变得就较为必要了。首先尝试百度云加速这一CDN加速服务。百度云加速提供了NS以及Cname两种方式的加速方式。NS方式为更改域名的dns，Cname方式为更改域名的解析设置。和添加域名解析的步骤一样。

1：先尝试修改NS的方法，在输入域名后添加子域，其中指向字段填写Openshift提供的二级域名，其他按照指示添加即可。最后会给出需要更改的dns地址，在万网将DNS服务器的地址修改即可。但是当DNS服务器修改生效时，网站无法访问，咨询后得到结果：目前一些域名商对我们部分NS无法识别，这个问题还在协商处理中，您可以暂时修改切入方式，选择Cname方式切入。故尝试选择Cname方式接入。这种方式要重新改写域名的解析设置。

2：第一步选择Cname方式，还是要添加两项，一个子域名（前缀）为@，一个为www。指向字段为OpenShift提供的二级域名。最后会给出两个别名。在万网中设置域名解析的地方要把原来的两条解析设置修改，其中记录值字段填写百度云加速提供的别名字段，最后存储，稍等片刻即可发现加速已经生效。

3：在网站中，右键打开审查元素，打开网络选项卡，之后随意访问一个网址，在响应头部分查看X-cache字段，若显示为HIT，则加速已经成功，若为MISS则失败。

4：之前说过，在配置wordpress后台地址时，推荐将Wordpress地址（后台地址）改为OpenShift提供的二级域名即可，这是因为OpenShift将Wordpress后台强制为https。但是百度云加速的NS专业版才支持https服务，这就冲突了，所以把后台网址设为OpenShift的地址就能巧妙的避开这些冲突。





Reference:

1:https://www.freehao123.com/new-openshift/

2:Aliyun&RedHat_OpenShift&Baidu

3:http://blog.csdn.net/hbhhww/article/details/6800200
