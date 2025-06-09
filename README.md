#  🌈🌈linux内核安装

## 目录🌳
1. [介绍](#介绍)✈️
2. [前提条件](#前提条件)✈️
3. [下载内核源码](#下载内核源码)✈️
4. [配置内核](#配置内核)✈️
5. [编译内核](#编译内核)✈️
6. [安装内核](#安装内核)✈️
7. [验证安装](#验证安装)✈️
8. [常见问题](#常见问题)✈️
9. [参考资料](#参考资料)✈️

## 介绍🌳
该仓库旨在编译安装相关linux内核。

## 前提条件🌳
在开始之前，请确保你具备以下条件：
- 一台已经安装了 Linux 操作系统的虚拟机（本教程以 Ubuntu24版本 为例）。
- 具备 sudo 权限的用户帐户。
- 足够的硬盘空间用于存放编译后的内核文件。
- 安装了一些必要的依赖工具（如 `gcc`、`make`、`libncurses-dev` 等）。
- 例如：在 Ubuntu 上，可以使用以下命令安装所需的开发工具和库：
  ```bash
  sudo apt update
  sudo apt install build-essential libncurses-dev bison flex libssl-dev libelf-dev bc

## 下载内核源码🌳
- 下载内核源码前可以先在虚拟机上创建一个文件夹用来保存下载的安装包 方便以后查询。
    ```bash
    makedir /你的文件名
- 可以直接去[linux源码官网](https://www.kernel.org/)下载对应linux版本（这里采用linux6.15.1）
- 或者直接执行如下命令
    ```bash
    sudo apt update
    sudo apt install linux-image-6.15.0-1

## 配置内核🌳
-  使用`make menuconfig`对内核进行图形化配置
    ```bash
      sudo apt update
      sudo apt install linux-image-6.15.0-1

## 编译内核🌳
-  使用`make `对内核进行编译,其中x可以根据自己的cpu设置
      ```bash
     make  -jx
## 安装内核🌳 
-  复制如下指令
     ```bash
     sudo make modules_install
     sudo make install
-  可以检查自己安装的模块是否在`lib`下面
     ```bash
     cd /lib/modules
     ls

## 验证安装🌳
-  在上述过程无误后可以执行如下指令更新grub
-  ```bash
     cd /lib/modules
     ls

## 常见问题🌳

## 参考资料🌳








