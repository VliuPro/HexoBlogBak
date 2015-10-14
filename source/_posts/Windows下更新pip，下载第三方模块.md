title: Windows下更新pip，下载第三方模块
date: 2015-08-08 09:44:43
categories: Python
tags: Python
description:
---
##### 这阵子一直在看Python，刚好看到了模块这一方面，但是在使用pip安装模块的时候出现了一点问题，这里记录一下。
---
#### 1. 安装和更新pip
    在Windows下安装Python时，记得勾选安装pip就可以，还有别忘了添加进path
    更新pip：
        使用script目录下的easy_install:
            easy_install pip


#### 2. 下载第三方模块报ReadTimeoutError
    这个就只能自己搭梯子了，我挂了全局下分分钟下好了。


#### 3. 还有吐槽一下
    不搭梯子不仅会read time out，而且网速只有几十kb/s，我用的digital ocean+ss，do里面用wget下载然后通过sftp传输到自己电脑上，分分钟就好了。。  do里面下载速度惊人的30+m/s。。再看看自己的30kb/s，我就简直了。。
