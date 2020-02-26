---
title: Ubuntu 使用配置
date: 2020-02-25 23:40:27
tags: [Ubuntu, Linux, 教程]
---
## <b>【系统】</b>

## 1. ubuntu无法识别固态硬盘上的windows操作系统

```bash
sudo apt-get install ntfs-3g
sudo ntfsfix /dev/sdb2		#无法识别的盘的挂载位置
```

## 2. Windows和Ubuntu时间不一致

```bash
sudo timedatectl set-local-rtc 1
```

## 3. 编辑hosts文件

```bash
sudo gedit /etc/hosts
```

## 4. 更新字体缓存

```bash
#Ubuntu字体目录：/usr/share/fonts/	复制安装新字体最好新建立文件夹，便于chmod 755
sudo mkfontscale		#生成字体的索引信息
sudo mkfontdir
sudo fc-cache -f -v
#可以将WindowsFonts文件夹复制到/usr/share/fonts/ 文件目录下之后运行生成字体索引等，丰富字体
```

## 5. 误删除/var/lib/dpkg解决办法

```bash
sudo mkdir -p /var/lib/dpkg/{alternatives,info,parts,triggers,updates} 
#Recover some backups:
sudo cp /var/backups/dpkg.status.0 /var/lib/dpkg/status 
#Now, lets see if your dpkg is working (start praying):
apt-get download dpkg
sudo dpkg -i dpkg*.deb 
#If everything is "ok" then repair your base files too:
apt-get download base-files
sudo dpkg -i base-files*.deb 
#Now try to update your package list, etc.:
dpkg --audit
sudo apt-get update
sudo apt-get check 
```

## 6. NVIDIA显卡驱动方法（NVIDIA Graphic Drivers PPA）

```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
```

## 7. 多余无用内核卸载

```bash
sudo dpkg --get-selections|grep linux		#查看系统所有内核
uname -a	#查看当前系统内核，无法卸载当前系统内核，需在grub里选择option内其他内核启动
sudo apt-get purge linux-headers-4.13.0-37-generic 		#卸载指定内核，示例4.13.0-37
sudo apt-get remove linux-headers-4.13.0-37
sudo apt-get remove linux-image-4.13.0-37-generic
sudo update-grub		#更新grub缓存
```
## 8. Ubuntu设置环境变量的几种方法

1.  Linux的变量种类
    按变量的生存周期来划分，Linux变量可分为两类：
    1.1 永久的：需要修改配置文件，变量永久生效。
    1.2 临时的：使用export命令声明即可，变量在关闭shell时失效。
2.  设置变量的三种方法
    2.1 在/etc/profile文件中添加变量【对所有用户生效(永久的)】

```bash
#用gedit在文件/etc/profile文件中增加变量，该变量将会对Linux下所有用户有效，并且是“永久的”。
#例如：编辑/etc/profile文件，添加CLASSPATH变量
gedit /etc/profile
export CLASSPATH=./JAVA_HOME/lib; $JAVA_HOME/jre/lib
source /etc/profile		#注：修改文件后要想马上生效时运行，不然只能在下次重进此用户时生效。
```

​	2.2 在个人用户主目录下的~/.bashrc中增加变量【对单一用户生效(永久的)】

```bash
#用gedit在用户目录下的~/.bashrc文件中增加变量，改变量仅会对当前用户有效，并且是“永久的”。
#例如：编辑emos用户目录 (/home/emos)下的.bashrc
gedit ~/.bashrc
#添加如下内容：
export CLASSPATH=./JAVA_HOME/lib; $JAVA_HOME/jre/lib
#注：修改文件后要想马上生效还要运行 source ~/.bashrc,不然只能在下次重进此用户时生效。
```

​	2.3 直接运行export命令定义变量【只对当前shell(BASH)有效(临时的)】
​		在shell的命令行下直接使用**[export 变量名=变量值]** 定义变量，该变量只在当前的shell(BASH)或其子shell(BASH)下是有效的，shell关闭了，变量也就失效了，再打开新shell时就没有这个变量，需要使用的话还需要重新定义。

3.  环境变量的查看
    3.1 使用echo命令查看单个环境变量。例如：
    
    ```bash
    echo $PATH
    ```
    
    3.2 使用env查看所有环境变量。例如：
    
    ```bash
    env
    ```
    
    3.3 使用set查看所有本地定义的环境变量。
    
    ```bash
    unset		#可以删除指定的环境变量。
    ```
    
4.  常用的环境变量
    
    ```bash
    PATH		#决定了shell将到哪些目录中寻找命令或程序
    HOME		#当前用户主目录
    HISTSIZE	#历史记录数
    LOGNAME		#当前用户的登录名
    HOSTNAME	#指主机的名称
    SHELL		#当前用户Shell类型
    LANGUAGE	#语言相关的环境变量，多语言可以修改此环境变量
    MAIL		#当前用户的邮件存放目录
    PS1			#基本提示符，对于root用户是#，对于普通用户是$
    ```
    
## 9. Ubuntu菜单栏出现两个输入法图标的解决办法
```bash
#安装搜狗输入法更新系统后，菜单栏出现两个输入法图标。
#这两个图标一个是搜狗用的fcitx-ui-classic，另一个是fcitx的fcitx-ui-qimpanel。
#而删除 fcitx-ui-qimpanel，只保留 fcitx-ui-classic就可以解决问题了，打开终端执行：
sudo apt-get remove fcitx-ui-qimpanel
#注销或重启系统即可
```

## <b>【软件】</b>

## 1. 删除系统中不常用软件

```bash
sudo apt-get remove libreoffice* -y
sudo apt-get remove totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku landscape-client-ui-install -y
```

## 2. 安装常用软件（VLC, Shutter, WPS）

```bash
sudo apt-get install vlc -y			#VLC播放器
sudo apt-get install shutter -y		#shutter截图软件
#WPS 2019 官方下载地址：https://linux.wps.cn/#
sudo dpkg -i wps-office_11.1.0.9080_amd64.deb #wps软件包需在官网自行下载
```

## 3. 解决WPS中的中文字体显示奇怪，字的大小不一致，模糊

```bash
sudo apt-get install ttf-wqy-microhei 	#文泉驿-微米黑
sudo apt-get install ttf-wqy-zenhei 	#文泉驿-正黑
sudo apt-get install xfonts-wqy 		#文泉驿-点阵宋体
```

## 4. 安装微信

```yaml
   1. 下载链接：https://github.com/geeeeeeeeek/electronic-wechat/releases
   2. 解压linux-x64.tar.gz到指定目录，直接点击electronic-wechat运行即可
```

## 5. 安装sublime-text-3(stable)

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt-get update
sudo apt-get install sublime-text
```
## 6. 安装搜狗中文输入法

```bash
sudo dpkg -i sogoupinyin_2.3.1.0112_amd64.deb #软件需自行下载
#搜狗输入法for Linux官方下载地址：https://pinyin.sogou.com/linux/?r=pinyin
sudo apt-get install -f
sudo apt-get update		#注销后重新登录可配置输入法
```

## 7. 解决搜狗输入法显示中文乱码的问题

```bash
cd ~/.config
rm -rf SogouPY* sogou*
#注销重新登录
```

## <b>【其他】</b>

## 1. Ubuntu系统安装genymotion安卓模拟器

```bash
chmod +x genymotion-2.12.0-linux_x64.bin #软件需要注册登录后下载
./genymotion-2.12.0-linux_x64.bin -d /home/niuhuan/Programs/		#安装位置可自定义
sudo apt-get install virtualbox
sudo apt-get install virtualbox-dkms
```

## 2. Ubuntu系统安装Windows软件-CrossOver 17 for Linux

```bash
#安装CrossOver17，相关配置可以百度
sudo dpkg -i crossover17.deb
sudo apt-get install -f
sudo apt-get update
#破解方法：找到winewrapper.exe.so文件后执行命令（不提示试用相当于正常使用）
sudo cp winewrapper.exe.so /opt/cxoffice/lib/wine
#实测可以运行Office365ProPlus，Tim
```

## 3. CrossOver 17 安装Office 365

```yaml
1.  选择安装应用：Office 2016，Windows7
2.  选择Office365_ProPlus.exe (32位安装包)，等待联网安装
```

   