Linus于二月十七日发布了linux kernel 4.4系列的第三个稳定版本4.4.2。建议用户进行升级

此次更新对 PA-RISC 和 x86 架构方面的问题进行了修补，同时还对 EXT4、NFS 及 OCFS2 文件系统支持进行了改进。

除了对架构和内核的核心改进外，Linux kernel 4.4.2 LTS 还主要对驱动进行了更新，主要有： ATA、block、crypto、HID、IOMMU、MD, MTD、PCI、网络（主要是无线方面）、TTY 和 USB 方面的驱动更新。

下面的包对于Ubuntu，Linux Mint，Elementary OS或者其他所有的衍生版本系统均适用。

网盘：内核deb包下载

下载以上三个deb包至一个文件夹中，然后在当前目录下执行命令：

sudo dpkg -i linux-headers-4.4*.deb linux-image-4.4*.deb
安装完成
