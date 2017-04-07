#配置开发环境
## 开发环境说明
* 操作系统版本：Ubuntu 14.04.5(Kernel:4.4.0-31)
* 主机环境： Dell XPS 8920（预装Win10） 

##安装操作系统
1. 关闭Secure UEFI
   * 开机按F2键进入BIOS -> Boot选项 -> Secure UEFI(Disabled)
   * 保存修改 -> 退出重启
2. Fix GPT分区表问题，否则安装程序会看不到原有磁盘分区表，造成无法安装
   * 插入Ubuntu 14引导盘 ->F12键进入启动选项->U盘启动
   * 选中Try Ubuntu -> 按e键进入编辑菜单，在quiet splash的后边加上" monodeset"->按F10引导启动。
     如果不进行这步操作，引导程序会卡住，黑屏。 
   * 进入系统中，按下菜单键，选择GPartion程序。进入后会提示"Fix GPT分区存在问题的窗口"。-> 选择Fix
3. 对SSD盘进行分区
   * 这款主机的SSD盘默认没有使用，需要我们手动分区
   * 进入GParption程序，选中/dev/sda设备；在Device菜单中，选择新建分区表。
   * 然后再在硬盘区域上，右键New，分出1G和31G两个区。
4. 开始安装Ubuntu
   * 选择桌面上的Install Ubuntu 
   * 一路默认选项安装, 完成后重启(在中间选择分区方案时，强烈建议使用自定义分区方
     案，将/boot和/分别挂载到/dev/sda的1G和31G分区上，SWAP和/home挂载到/dev/sdb上，分别使用16G和2T，
     这样我们就可以使用SSD，另外系统如果意外崩溃，我们的数据是可以被保留的）
5. 重启进入系统，进行软件更新 
   * 系统启动时，按住Esc键，进入Ubuntu的启动引导菜单
   * 在第一行（Ubuntu）上按下e，进入编辑菜单
   * 在第三行中，将其中的quiet splash替换为monodeset。请一定完成这一步，否则系统是无法正常启动的。
   * 在System Setting（左侧边栏螺丝扳手图标）程序中，选择Softwafe&Updates，关闭一切自动更新。如果你
     确定要保留自动更新的话，请将Updates中`UnSupport updates`选项一定去掉，然后保留N卡的驱动程序，预
     备每次自动更新后自行重装一遍驱动，否则很可能自动更新完，出现循环登录的情况。
   * 进入系统后执行如下命令
```
   cp -rf /media/`whoami`/Ubuntu\ 14.0/Drivers\&Libs/* $HOME/Downloads
   sudo apt-get update 
   sudo apt-get upgrade
   sudo sed -i "s/quiet splash/quiet splash nomodeset/" /etc/default/grub
   sudo update-grub2
   sudo apt-get install nvidia-375
   sudo reboot
```
5. 再次进入系统，安装Cuda
   * 验证能否正常登录
   * 使用nvidia-smi 查看N卡驱动是否正常安装
   * 如果一切正常，我们开始安装Cuda 
   * 打开命令行后执行如下程序
```
   cd $HOME/Downloads
   sudo bash cuda_8.0.61_375.26_linux.run #注意在中间选择不要安装N卡驱动，此外, 按照提示默认安装ok。
```
   * 安装完成后， 在$HOME目录中会有一个NVIDIA_CUDA-8.0_Samples的目录，进入后make可以编译出实例程序，
     用来验证安装是否成功。





