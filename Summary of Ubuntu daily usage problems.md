Ubuntu日常使用问题汇总

1：在非正常取消一次更新后显示

无法锁定管理目录(/var/lib/dpkg/)，是否有其他进程正占用它

sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock
2：在更新时提示：  

W: 仓库 “http://security.ubuntu.com/ubuntu xenial-security Release” 没有 Release 文件。
N: 无法认证来自该源的数据，所以使用它会带来潜在风险。
N: 参见 apt-secure(8) 手册以了解仓库创建和用户配置方面的细节。
W: GPG 错误：http://archive.ubuntukylin.com:10006/ubuntukylin xenial InRelease: 由于没有公钥，无法验证下列签名： NO_PUBKEY 8D5A09DC9B929006
W: 仓库 “http://archive.ubuntukylin.com:10006/ubuntukylin xenial InRelease” 没有数字签名。
N: 无法认证来自该源的数据，所以使用它会带来潜在风险。
N: 参见 apt-secure(8) 手册以了解仓库创建和用户配置方面的细节。
E: 无法下载 http://security.ubuntu.com/ubuntu/dists/xenial-security/restricted/binary-amd64/Packages 无法发起与 security.ubuntu.com:80 (2001:67c:1560:8001::14) 的连接 - connect (101: 网络不可达) [IP: 2001:67c:1560:8001::14 80]
E: 部分索引文件下载失败。如果忽略它们，那将转而使用旧的索引文件。
解决方法也很简单，缺少一个key的话，导入它就好了。

sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 8D5A09DC9B929006
上述命令最后跟着的key值为你自己的机器上报错时显示的key值
