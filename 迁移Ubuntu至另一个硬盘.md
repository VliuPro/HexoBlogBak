title: 迁移Ubuntu至另一个硬盘
date: 2016-02-02 23:33:53
updated:
tags: 
- Ubuntu
categories: 
- Ubuntu
permalink: move-ubuntu
---
> 我一直认为，Windows就是用来玩游戏的系统。

> 自从喜欢上了Ubuntu，就一发不可收拾了。虚拟机玩着特别不爽，然后正好要戒游戏，咬咬牙直接装上了Ubuntu（当然，是双系统）。但是固态只有250G左右，两个系统分下来，Ubuntu只有70G左右，win的C盘不知不觉就会占很大的，所以win占大头。其实这本来也是够用了的，不过自从开始学Android之后，就有点吃紧了。原因无他，虚拟机没地方放了。谁让Ubuntu下没有官方的QQ呢，而且wine的QQ真的是特别难用。趁着过年，而且笔记本有个msata的接口，就买了个128G的建兴的。
    
> 头比较大的是家里网时4M小水管，如果重装系统的话很麻烦，而且开发环境也要重新配置，就想着能不能直接把原来的系统备份过来，还好，google是可以查到资料的！

---

1. 分区
    我之前的系统是分了4个区的，分别是： `/` ， `/boot` ， `交换分区` ， `/home`
    本来在新的硬盘上面也是这么分四个区，但是有个问题！
    我之前的硬盘是`GPT`的，系统都是用`UEFI`装的，在迁移系统的时候就要注意，新硬盘也要是`GPT`格式的。所以在分区的时候要分出ESP分区，我给了`200M`，然后其他的四个分区就跟以前一样的了。
    `/`我给了40G，`/boot`我给了600M（系统更新的时候有时候会显示boot空间不足，干脆分的大一点），`交换分区`我给了4G，但是应该没什么用，毕竟我也是8G`大`内存！（手动斜眼），剩下的就都是给了`/home`。

2. 拷贝
    分完区之后就是把原系统的文件拷贝到新硬盘中。
    ``` bash
        sudo cp -ax / /media/vliupro/_/
        sudo cp -ax /boot/* /media/vliupro/boot
        sudo cp -ax /home/* /media/vliupro/home
        
    ```

3. 修改uuid
    在拷贝结束之后，需要修改配置文件中的uuid，否则将会出现错误。
    - 查看uuid： `sudo blkid`
    - 修改配置文件：
        `sudo gedit /media/vliupro/_/etc/fstab`
        将其中对应的uuid一一替换。

4. 修复grub
    修改完uuid之后就可以去修复引导了。
    - 查看各分区情况： `sudo fdisk -l`
    - 挂载各分区，交换分区是不用挂载的！
        + sudo mount /dev/sda2 /mnt
        + sudo mount /dev/sda3 /mnt/boot
        + sudo mount /dev/sda1 /mnt/boot/efi
        + sudo mount /dev/sda5 /mnt/home
        + mount -t proc proc /mnt/proc
        + mount --rbind /sys /mnt/sys
        + mount --rbind /dev /mnt/dev
    - 挂载完成之后就可以chroot到新硬盘上的系统了：`sudo chroot /mnt /bin/bash`
    - 重新生成`/boot/grub/grub.cfg`：`grub-mkconfig -o /boot/grub/grub.cfg`
    - 最后安装grub即可：`sudo grub-install --boot-directory=/mnt/boot /dev/sda`
    
5. 重启
    如果没什么问题现在重启选择新硬盘启动就可以进入新硬盘中的系统
    ```bash
        sudo update-grub  
        sudo grub-install /dev/sda
    ```
    
---

> 现在可以愉快的撸Ubuntu了～