主要包含： 编译内核 ，编译busybox ，使用qemu启动到命令行

**一 、编译内核**

1.1 安装依赖

```bash
 sudo apt install build-essential libncurses-dev libssl-dev bison flex libelf-dev wget -y
```

下载内核代码

```bash
 cd ~/code
 wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.1.120.tar.xz
 tar xf linux-6.1.120.tar.xz
```

配置

```bash
 cd linux-6.1.120
 make ARCH=x86_64 x86_64_defconfig
 make ARCH=x86_64 menuconfig
 ## 以下是配置内容, 很重要!!! 否则会挂载rootfs失败.
     General setup  --->
        ----> [*] Initial RAM filesystem and RAM disk (initramfs/initrd) support
     Device Drivers  --->
        [*] Block devices  --->
                <*>   RAM block device support
                (65536) Default RAM disk size (kbytes)
## 以下配置是为了后面的vscode调试
    Kernel hacking  ---> 
        Compile-time checks and compiler options  --->
            Debug information : Rely on the toolchain's implicit default DWARF version 
            [*] Provide GDB scripts for kernel debugging
```

CONFIG_MEMCG 要开启 memory 写不下去

CONFIG_BPF_SYSCALL 要开启 runc 用

编译

编译期间：可能会报错 要删除 rm -rf networking/tc.c

```bash
 make ARCH=x86_64 bzImage -j8
```

编译完成, 即可以看到生成了内核, 路径: arch/x86_64/boot/bzImage.

后面用busybox制作一个最小根文件系统, 然后用qemu启动它.

# 二、编译busybox 制作rootfs

[Busybox](https://zhida.zhihu.com/search?content_id=251616084&content_type=Article&match_order=1&q=Busybox&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzIwOTA5ODgsInEiOiJCdXN5Ym94IiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjUxNjE2MDg0LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.YXI0rTn50_0YdUoRI6Pz_MB6B9d3BSvBWccV4V12Xn8&zhida_source=entity)是Linux界的瑞士军刀, 只有1-2M, 包含了大部分Linux常用命令.

[嵌入式Linux](https://zhida.zhihu.com/search?content_id=251616084&content_type=Article&match_order=1&q=%E5%B5%8C%E5%85%A5%E5%BC%8FLinux&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzIwOTA5ODgsInEiOiLltYzlhaXlvI9MaW51eCIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI1MTYxNjA4NCwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.PYfawEcmidwvWYrBkZIAWyaVklwzG1vqSp-OVDYBD5g&zhida_source=entity)/[Android](https://zhida.zhihu.com/search?content_id=251616084&content_type=Article&match_order=1&q=Android&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzIwOTA5ODgsInEiOiJBbmRyb2lkIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjUxNjE2MDg0LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.6yoEHptOf6ZkmRYb2DkIyvOYYlBG6XNw5piUJy1mA10&zhida_source=entity)一般都使用busybox构建最小的运行环境.

## **2.1安装依赖**

```bash
sudo apt install build-essential libncurses5-dev tree wget -y 
```

## **2.2 下载**

```bash
 cd ~/code
 wget https://busybox.net/downloads/busybox-1.36.1.tar.bz2
 tar xf busybox-1.36.1.tar.bz2
```

```bash
 cd busybox-1.36.1
 make menuconfig
     Settings
             -> Build static binary (no shared libs)
```

当前目录下生成`.config`配置文件

## **2.3 编译安装**

`make -j4 && make install`

编译完成后，`_install/` 目录就是 busybox 的根目录

## 2.4 补全目录结构

`cd _install/
mkdir dev etc/init.d/ proc sys tmp mnt/sysroot -p`

### 2.5 初始化设备

`sudo mknod dev/console c 5 1
sudo mknod dev/null c 1 3
sudo mknod dev/tty1 c 4 1`

- `mknod`：命令用于创建特殊文件，通常是设备文件。
- `dev/console`：指定设备节点的路径和名称。
- `c`：表示创建的是字符设备文件。
- `5`：主设备号（major number），用于标识设备类型。
- `1`：次设备号（minor number），用于标识同一类型中的特定设备。
- `/dev/console` 是系统控制台设备，通常用于内核消息和系统日志的输出。
- `/dev/null` 是一个特殊的设备文件，任何写入它的数据都会被丢弃，而读取它则总是返回EOF。
- `/dev/tty1` 是第一个虚拟控制台，通常在文本模式下可以通过按Ctrl+Alt+F1来访问。

### 2.6 初始化挂载点

vim etc/fstab

```
# proc /proc proc defaults             0    0
# sysfs   /sys     sysfs   defaults    0    0
tmpfs   /tmp     tmpfs   defaults    0    0
```

- `etc/fstab`：配置文件，用于指定启动时挂载的文件系统。
- 以`#`开头，表示被注释掉，所以它不会被执行。
- `proc`：文件系统类型，提供内核和进程的运行时信息。
- `sysfs`：文件系统类型，提供对系统硬件配置的视图。
- `tmpfs`：文件系统类型，tmpfs是一个基于内存的文件系统，通常用于存储临时文件。
- `/proc /sys /tmp`：挂载点，即挂载文件系统的位置。
- `proc`：文件系统的设备名，这里与类型同名，因为proc是一个伪文件系统。
- `defaults`：挂载选项，`defaults`通常包括`rw`（可读写）、`suid`（允许`setuid`位生效）、`dev`（解释字符或块特殊设备）、`exec`（允许执行二进制文件）等。
- `0`：是否需要dump备份，`0`表示不需要。
- `0`：是否需要fsck检查，`0`表示不需要。

### 2.7 初始化启动脚本，增加执行权限

```
vim etc/init.d/rcS

#!/bin/sh

echo -e "Welcome to busybox"
/bin/mount -a
/bin/mount -t proc proc /proc
/bin/mount -t sysfs sysfs /sys
echo -e "Remounting the rootfs..."
mount -o remount,rw /
mkdir -p /dev/pts
mount -t devpts devpts /dev/pts
echo /sbin/mdev > /proc/sys/kernel/hotplug
mdev -s

chmod 755 etc/init.d/rcS
```

- 告诉系统这个脚本应该使用哪个解释器来执行
- 输出欢迎信息到控制台。`-e`选项允许转义序列，例如`\n`（换行符）
- 尝试挂载`/etc/fstab`文件中列出的所有文件系统（对应上一步）
- 挂载`proc sysfs`文件系统到`/proc /sys`目录
- 输出信息到控制台，表示正在重新挂载根文件系统
- 重新挂载根文件系统为可读写模式。默认情况下，根文件系统可能以只读方式挂载
- 挂载`devpts`文件系统到`/dev/pts`目录，`devpts`用于提供伪终端
- 设置内核的热插拔事件处理程序为`/sbin/mdev`。`mdev`是BusyBox提供的一个轻量级设备管理器
- 扫描所有已知的设备文件，并在`/dev`目录下创建或删除相应的设备节点

### 2.8 初始化init进程，增加执行权限

```
vim etc/inittab

::sysinit:/etc/init.d/rcS
::askfirst:-/bin/ash
::ctrlaltdel:/sbin/reboot
::shutdown:/sbin/swapoff -a
::shutdown:/bin/umount ‐a ‐r
::restart:/sbin/init
```

chmod 755 etc/inittab

- 在传统的System V初始化系统中使用的配置文件，用于告诉init进程如何启动和运行系统。
- `::sysinit`: 是一个运行级别无关的操作，表示在系统初始化时执行。
- `/etc/init.d/rcS` 是一个脚本，通常在单用户模式或系统启动时运行，用于启动系统服务（对应上一步）。
- `::askfirst:` 表示在指定运行级别时，init将执行后面的命令，但在执行前会提示用户登录。
- `-` 表示忽略Ctrl+C中断信号。
- `/bin/ash` 是一个shell，通常是一个轻量级的Bourne shell兼容的shell。
- `::ctrlaltdel`: 指定当用户按下Ctrl+Alt+Delete组合键时执行的操作。
- `/sbin/reboot` 是重新启动系统的命令。
- `::shutdown:` 表示在系统关机时执行的操作。
- `/sbin/swapoff -a` 是停止所有交换分区的命令。
- `/bin/umount -a -r` 是卸载所有文件系统的命令，`-r` 选项表示如果无法正常卸载，则进行强制卸载。
- `::restart`: 指定当init进程重启时执行的操作。
- `/sbin/init` 是重新启动init进程的命令，这通常用于重启整个系统。

## 2.9 文件打包方式

`find . | cpio -R root:root -H newc -o | gzip > ../rootfs.gz`

- `find .`：在当前目录及其子目录中查找所有文件和目录。`.` 表示当前目录。
- `|`：管道符，将 `find .` 命令的输出传递给下一个命令。
- `cpio -R root:root -H newc -o`：`cpio` 是一个用于复制文件和目录的工具，这里使用的是 `cpio` 的写入模式（`-o` 选项）。`-R root:root` 指定文件所有者为 `root`，组为 `root`。`-H newc` 指定使用 `newc` 格式进行归档。
- `| gzip > ../rootfs.gz`：将 `cpio` 归档的输出通过管道传递给 `gzip` 命令进行压缩，并将压缩后的文件输出到 `../rootfs.gz`。

## 2.10 挂载

```
qemu-img create -f raw rootfs.img 32M
mkfs -t ext4 ./rootfs.img
mkdir img
sudo mount -o loop rootfs.img ./img
sudo cp -rf ./_install/* ./img
sudo umount ./img
```

- 创建了一个名为`rootfs.img`的32MB大小的原始格式磁盘映像文件，要小于内核配置中的`Default RAM disk size`
- 在`rootfs.img`映像文件上创建了一个ext4文件系统
- `-o loop`：指定使用环回（loopback）设备来挂载文件系统，这样可以将一个文件当作块设备来挂载
- 将`rootfs.img`映像文件挂载到`./img`目录
- 将BusyBox的安装目录（`_install`）中的所有文件和目录复制到挂载的映像文件系统中
- 卸载之前挂载的`rootfs.img`映像文件

2.11

### 2.10 配合Linux内核运行、调试

```text
qemu-system-x86_64 -nographic \
-m 1024m -smp cores=4 \
-kernel ./linux-6.1.120/arch/x86_64/boot/bzImage \
-initrd ./busybox-1.36.1/rootfs.cpio \
-append "console=ttyS0 nokaslr" \
-net nic -net user \
-drive file=./busybox_data.qcow2,format=qcow2,if=virtio,cache=none \
-s -S
```

- 设置网络
  
  ```
  ip link set eth0 up && \
  ip addr add 10.0.2.15/24 dev eth0 &&\
  ip route add default via 10.0.2.2 dev eth0 &&\
  echo "nameserver 8.8.8.8" >> /etc/resolv.conf
  ```
  

## 三 使用vscode连接并调试

0. 安装
  

- 安装vscode插件: 安装C++(Microsoft)插件. 这个是C开发基础插件.
  
- 安装[gdb](https://zhida.zhihu.com/search?content_id=251619853&content_type=Article&match_order=1&q=gdb&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzIwOTA2NDksInEiOiJnZGIiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNTE2MTk4NTMsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.DN1_vNK24L2W_WvN144q2Je2SPB_AswZi7q5CLLi02A&zhida_source=entity): sudo apt install gdb
  

1. 使用vscode的remote功能, 连接开发机, 打开文件夹 ~/code/linux-6.1.120
  
2. 创建文件~/code/linux-6.1.120/.vscode/launch.json, 并添加以下内容
  
  ```
  {
      "version": "0.2.0",
      "configurations": [
          {
              "name": "kernel-debug",
              "type": "cppdbg",
              "request": "launch",
              "miDebuggerServerAddress": "127.0.0.1:1234",
              "program": "${workspaceFolder}/vmlinux",
              "args": [],
              "stopAtEntry": true,
              "cwd": "${workspaceFolder}",
              "environment": [],
              "externalConsole": false,
              "logging": {
                  "engineLogging": false
              },
              "MIMode": "gdb",
          }
      ]
  }
  ```
  
  设置好后, 效果如下:
  
  - 运行qemu: 注意这里比上面的步骤多了两个参数(-s -S), 这是启动调试模式.
  - 设置断点: 在源文件某一行左侧点一下就可设置断点. 例如设置 driver/char/misc.c的misc_register函数. 185 行
  - 点Debug图标: 进入调试界面.
  - 点Run图标: qemu会继续运行, 并有相应输出. 然后源代码会停在第一个断点处.

```
qemu-system-x86_64 -nographic -m 1024m -smp cores=4 -kernel ./linux-6.1.120/arch/x86_64/boot/bzImage -initrd ./busybox-1.36.1/rootfs.img -drive file=disk.qcow2,format=qcow2,if=virtio,index=1,media=disk -append "root=/dev/ram rdinit=/linuxrc console=ttyS0 nokaslr" -net nic -net user
```

mkdir -p /mnt/qcow2_data  
mount /dev/vda /mnt/qcow2_data
