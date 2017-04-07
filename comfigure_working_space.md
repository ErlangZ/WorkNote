#配置开发环境
## 开发环境说明
* 操作系统版本：Ubuntu 14.04.5(Kernel:4.4.0-31)
* 主机环境： Dell XPS 8920（预装Win10） 

##安装操作系统
1. 关闭Secure UEFI
   * 开机按F2键进入BIOS -> Boot选项 -> Secure UEFI(Disabled)
   * 保存修改 -> 退出重启
2. 清理掉Win10分区，否则Ubuntu无法看到已有的硬盘分区,无法正常安装
   * 插入Ubuntu 14引导盘 ->F12键进入启动选项->U盘启动
   * Try Ubuntu      -> 进入引导系统 -> 打开命令行(Ctrl+Alt+T)
   * $fdisk /dev/sda -> 连续敲d（一共2次），直到删除/dev/sda所有已有的分区表
   * $fdisk /dev/sdb -> 连续敲d（一共6次），直到删除/dev/sdb所有已有的分区表
   * 重新启动
3. 开始安装Ubuntu
   * 插入Ubuntu 14引导盘 ->F12键进入启动选项->U盘启动
   * Install Ubuntu -> 一路默认选项安装, 完成后重启(在中间选择分区方案时，强烈建议使用自定义分区方
     案，将/和/home分别挂载到两个独立分区上，这样系统如果意外崩溃，我们的数据是可以被保留的）
4. 重启进入系统，进行软件更新 
   * 系统启动时，按住Esc键，进入Ubuntu的启动引导菜单
   * 在第一行（Ubuntu）上按下e，进入编辑菜单
   * 在第三行中，将其中的quiet splash替换为monodeset。请一定完成这一步，否则系统是无法正常启动的。
   * 在System Setting（左侧边栏螺丝扳手图标）程序中，选择Softwafe&Updates，关闭一切自动更新。如果你
     确定要保留自动更新的话，请将Updates中`UnSupport updates`选项一定去掉，然后保留N卡的驱动程序，预
     备每次自动更新后自行重装一遍驱动，否则很可能自动更新完，出现循环登录的情况。
   * 进入系统后执行如下命令
```$ cp -rf /media/`whoami`/Ubuntu\ 14.0/Drivers\&Libs/* $HOME/Downloads
   $ sudo apt-get update 
   $ sudo apt-get upgrade
   $ sudo sed -i "s/quiet splash/quiet splash nomodeset/" /etc/default/grub
   $ sudo update-grub2
   $ sudo apt-get install nvidia-375
   $ sudo reboot
```
5. 再次进入系统，安装Cuda
   * 验证能否正常登录
   * 使用nvidia-smi 查看N卡驱动是否正常安装
   * 如果一切正常，我们开始安装Cuda 
   * 打开命令行后执行如下程序
```$cd $HOME/Downloads
   $sudo bash cuda_8.0.61_375.26_linux.run #注意在中间选择不要安装N卡驱动，此外, 按照提示默认安装ok。
```
   * 安装完成后， 在$HOME目录中会有一个NVIDIA_CUDA-8.0_Samples的目录，进入后make可以编译出实例程序，
     用来验证安装是否成功。
