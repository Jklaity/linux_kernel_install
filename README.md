#  🌈🌈linux内核编译安装

## 目录🌳
1. [介绍](#介绍)✈️
2. [前提条件](#前提条件)✈️
3. [下载内核源码](#下载内核源码)✈️
4. [配置内核](#配置内核)✈️
5. [编译内核](#编译内核)✈️
6. [安装内核](#安装内核)✈️
7. [验证安装](#验证安装)✈️
8. [常见问题](#常见问题)✈️
## 介绍🌳
该仓库旨在编译安装相关linux内核。

## 前提条件🌳
在开始之前，请确保你具备以下条件：
- 一台已经安装了 Linux 操作系统的虚拟机（本教程以 Ubuntu24版本 为例）。
- 具备 sudo 权限的用户帐户。
- 足够的硬盘空间用于存放编译后的内核文件。
- 需要切换国内的软件源🏃[参考该博主](https://blog.csdn.net/Zzp750/article/details/145771731?ops_request_misc=&request_id=&biz_id=102&utm_term=Ubuntu24%E6%8D%A2%E5%8E%9F&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-145771731.142^v102^control&spm=1018.2226.3001.4187)
- 安装了一些必要的依赖工具（如 `gcc`、`make`、`libncurses-dev` 等）。
- 例如：在 Ubuntu 上，可以使用以下命令安装所需的开发工具和库：
  ```bash
  sudo apt update
  sudo apt-get install gcc g++
  sudo apt-get install libncurses5-dev 
  sudo apt-get install build-essential
  sudo apt-get install kernel-package 
  sudo apt-get install libssl-dev
  sudo apt-get install libc6-dev 
  sudo apt-get install bin86
  sudo apt-get install flex 
  sudo apt-get install bison  
  sudo apt-get install qttools5-dev  
  sudo apt-get install libelf-dev
  
## 下载内核源码🌳
- 下载内核源码前可以先在虚拟机上创建一个文件夹用来保存下载的安装包 方便以后查询。
    ```bash
    makedir /你的文件名
- 可以直接去🏃[linux源码官网](https://www.kernel.org/)下载对应linux版本（这里采用linux5.15.134）
- 或者直接执行如下命令
    ```bash
    wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.15.134.tar.xz
- 下载后解压进入该代码目录
    ```bash
    tar -xvf linux-5.15.134.tar.xz
    cd  linux-5.15.134
## 配置内核🌳
-  1.建议使用`make Xconfig`进行默认配置
     ```bash
      make Xconfig
-  2.或者使用`make menuconfig`对内核进行图形化配置
    ```bash
      make menuconfig
## 编译内核🌳
-  为防止编译时出现常见的错误,建议编译前检查执行 [常见问题](#常见问题) 的第 2, 3 步
-  使用`make `对内核进行编译,其中x可以根据自己的cpu设置
      ```bash
     make  -jx
-  亦可直接复制如下指令
     ```bash
     make -j$(nproc) 
-  `-j$(nproc)` 会让编译使用系统上所有的 CPU 核心，这样会加快编译速度。如果你只想使用一个核心，移除 `-j$(nproc)` 即可。
## 安装内核🌳 
-  复制如下指令
     ```bash
     sudo make modules_install
     sudo make install
-  可以检查自己安装的模块是否在`lib`下面
     ```bash
     cd /lib/modules
     ls
-  安装启动文件
      ```bash
     sudo mkinitramfs /lib/modules/5.15.134 -o /boot/initrd.img-5.15.134
     sudo cp arch/x86/boot/bzImage /boot/vmlinuz-5.15.134
     sudo cp System.map /boot/System.map-5.15.134

## 验证安装🌳
-  在执行`sudo make install` 后会自动更新`grub`可以不做手动更新
-  重启系统默认会切换到最新的内核 ,也可以在启动界面手动进入高级选项选择内核，进入系统使用如下指令查看当前系统版本
    ```bash
     uname -a

## 常见问题🌳
-  1.在安装内核以后一般会有一个配置引导程序`grub `我并没有做该操作🏃[详情参考](https://blog.csdn.net/ustczwc/article/details/9053803?ops_request_misc=&request_id=&biz_id=102&utm_term=Ubuntu24%E7%BC%96%E8%AF%91linux%E5%86%85%E6%A0%B8&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-3-9053803.142^v102^control&spm=1018.2226.3001.4187)
    ```bash
     sudo update-grub
-  2.提示` “make[1]: *** No rule to make target ‘debian/certs/benh@debian.org.cert.pem’, needed by ”` ，将下面的内存修改为空[原文参考](https://blog.csdn.net/feihe0755/article/details/125424910?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522ef289f15aa3ccefedc9a2b4a83f94ebf%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=ef289f15aa3ccefedc9a2b4a83f94ebf&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-125424910-null-null.142^v102^control&utm_term=ubuntu20.04%E7%BC%96%E8%AF%91%E5%86%85%E6%A0%B8&spm=1018.2226.3001.4187)
      ` CONFIG_SYSTEM_TRUSTED_KEYS=“debian/canonical-certs.pem”` 
      ` CONFIG_SYSTEM_REVOCATION_KEYS=“debian/canonical-revoked-certs.pem”` 
      新值
      ` CONFIG_SYSTEM_TRUSTED_KEYS=“”` 
      ` CONFIG_SYSTEM_REVOCATION_KEYS=“”`
-  3.提示`“BTF: .tmp_vmlinux.btf: pahole (pahole) is not available”`时，修改
      `CONFIG_DEBUG_INFO_BTF=y`为 
      `CONFIG_DEBUG_INFO_BTF=n` [原文参考](https://blog.csdn.net/feihe0755/article/details/125424910?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522ef289f15aa3ccefedc9a2b4a83f94ebf%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=ef289f15aa3ccefedc9a2b4a83f94ebf&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-125424910-null-null.142^v102^control&utm_term=ubuntu20.04%E7%BC%96%E8%AF%91%E5%86%85%E6%A0%B8&spm=1018.2226.3001.4187)
-  4.报错`...No such file or directory`时，请自行安装相关依赖




























