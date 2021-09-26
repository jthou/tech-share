## 自制Linux系统

### Ubuntu桌面版

- 下载

  > https://ubuntu.com/download/desktop/thank-you?version=20.04.2.0&architecture=amd64

- 制作USB启动工具

  > https://rufus.ie/

- 详细安装过程可参考：
  
  > https://www.jianshu.com/p/117ac3c3951a

### 更改开机动画

Plymouth是为Ubuntu系统提开机和关机画面的应该程序。plymouth整体分两个主要部分，服务端和客户端，典型的C/S模型。服务端和客户端直接通过socket通信。

- 服务端。是一个后台守护进程plymouthd，用于处理请求，请求种类有很多，比如典型的update、quit等。服务端通过epoll监控相关socket(也有管道），监听来自客户端的信息。
- 客户端。客户端可以多种多样，典型的客户端有：plymouth程序、systemd。客户端通过socket(也有管道）与服务端建立连接，并通过socket(也有管道）发送具体的请求。

```bash
jthou@ubuntu:~/Desktop$ sudo apt install plymouth-themes plymouth plymouth-x11
jthou@ubuntu:~/Desktop$ sudo apt install git
jthou@ubuntu:~/Desktop$ git clone https://github.com/adi1090x/plymouth-themes
jthou@ubuntu:~/Desktop$ sudo cp -r plymouth-themes/pack_1/angular/ /usr/share/plymouth/themes 
jthou@ubuntu:~/Desktop$ sudo update-alternatives --install /usr/share/plymouth/themes/default.plymouth default.plymouth /usr/share/plymouth/themes/angular/angular.plymouth 100
jthou@ubuntu:~/Desktop$ sudo update-alternatives --config default.plymouth 
There are 15 choices for the alternative default.plymouth (providing /usr/share/plymouth/themes/default.plymouth).

  Selection    Path                                                                               Priority   Status
------------------------------------------------------------
  0            /usr/share/plymouth/themes/edubuntu-logo/edubuntu-logo.plymouth                     150       auto mode
  1            /usr/share/plymouth/themes/bgrt/bgrt.plymouth                                       110       manual mode
  2            /usr/share/plymouth/themes/edubuntu-logo/edubuntu-logo.plymouth                     150       manual mode
  3            /usr/share/plymouth/themes/kubuntu-logo/kubuntu-logo.plymouth                       150       manual mode
  4            /usr/share/plymouth/themes/lubuntu-logo/lubuntu-logo.plymouth                       150       manual mode
  5            /usr/share/plymouth/themes/angular/angular.plymouth                                 100       manual mode
* 6            /usr/share/plymouth/themes/sabily/sabily.plymouth                                   60        manual mode
  7            /usr/share/plymouth/themes/spinner/spinner.plymouth                                 70        manual mode
.....

jthou@ubuntu:~/Desktop$ sudo update-initramfs -u
update-initramfs: Generating /boot/initrd.img-5.11.0-27-generic
jthou@ubuntu:~/Desktop$ reboot
```

### 修改Grub

### 独占式应用

### 自制Linux安装镜像



## 参考文献

[1] [CSDN: 解压vmlinuz和解压initrd（initramfs)](https://blog.csdn.net/junmuzi/article/details/22185003)

[2] [HappySeeker: 开机动画plymouth相关原理](https://happyseeker.github.io/graphic/2016/05/26/plymouth.html)

[3] [LinuxCool: How to Install Plymouth System Boot Theme on Ubuntu 20.04](https://linuxcool.ru/en/how-to-install-plymouth-system-boot-theme-on-ubuntu-20-04/)

