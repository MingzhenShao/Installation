# Installation

## Ubuntu Swap
```
$ free
$ sudo gedit /etc/sysctl.conf
#################################################
# Set Swap usable availability
#        Swap <-------> physical memory
# default: 60%                40%
vm.swappiness=20
```
## Hide mounted Drives logo infiles sidebar
Use the mount option `x-gvfs-hide` in `/etc/fstab` to hide it in nautilus, for example.
```
For example, a line in /etc/fstab would become:

/dev/sda1 /mnt/sda1 ext4   defaults,x-gvfs-hide       0     2
```

## Install Nvidia Driver, cuda, cuDnn, Anaconda, Tensorflow  
Nvidia Driver can be installed by CUDA (Some times doesn't work)  
```
$ sudo apt-get purge nvidia*
# Note this might remove your cuda installation as well
$ sudo apt-get autoremove 
```
For a new PC you still need `$ sudo apt-get install build-essential gcc-multilib dkms` to install the depends.  

* In this condiction, When "X server is running"  
```
$ sudo service lightdm stop
$ rm /tmp X*-lock
```
* Add to path to `~/.bashrc`
```
export PATH=/usr/local/cuda/bin:${PATH}
export LD_LIBRARY_PATH=/usr/local/cuda/bin:${LD_LIBRARY_PATH}
```
* When you update your Ubuntu system, it may cause a login loop. 
> 1. check the authority of the .Xauthority `$ ls -lah` if `root` then `$ sudo chown username:username .Xauthority`.  
> 2. If after this the login loop keeps,`reinstall the Nvidia driver`  
* Anaconda couldn't install `check the owner of anaconda`


### Independent install Nvidia Driver, follow [this](https://gist.github.com/wangruohui/df039f0dc434d6486f5d4d098aa52d07)  

`ATTENTION`      
* If you can not solve the nouveau problem, try -no-x-check -no-nouveau-check, the check is not necessary, in [this](https://blog.csdn.net/wangsidadehao/article/details/70255754)  

multiple CUDA    
```
$ cd /usr/local   #cuda is a soft link
$ stat cuda

File: 'cuda' -> '/usr/local/cuda-9.0'
  Size: 19        	Blocks: 0          IO Block: 4096   symbolic link
Device: 801h/2049d	Inode: 15337567    Links: 1
Access: (0777/lrwxrwxrwx)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2018-12-27 22:38:59.461332996 +0900
Modify: 2018-12-27 22:38:51.621298141 +0900
Change: 2018-12-27 22:38:51.621298141 +0900
 Birth: -
```  
cuDNN  
```
$ sudo cp cuda/include/cudnn.h /usr/local/cuda/include
$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

## openCV with videoCapture
```
$ pip uninstall opencv-python
$ conda remove opencv. 

$ conda install -c anaconda opencv
```
With the method mentioned above, we meet the problem  
```
The function is not implemented. Rebuild the library with Windows, GTK+ 2.x or
Carbon support. If you are on Ubuntu or Debian, install libgtk2.0-dev and
pkg-config, then re-run cmake or configure script
```
which solved by below
```
$ conda install -c menpo opencv=2.4.11
```

## VS Code 
* VS Code cannot open
```
$ code --verbose
[main 20:19:26] Startup error: 
Error: EACCES: permission denied, mkdir '/home/<user>/.config/Code/CachedData'

# sure enough the folder ~/.config/Code had root access permissions for some reason. Deleted the folder using sudo.

$ rm -rf /home/<user>/.config/Code 
```
* VS Code could not install extensions  
The owner is root.  
`$ sudo chown username:username VSCode/`

## exFAT in Ubuntu
exFAT works fine in Win and MacOS, for Ubuntu,   
`$ sudo apt install exfat-fuse exfat-utils`
reboot to make it work. (In my case, without logout, the disk can be mount, but fail to write.)

## Reproduce Keras in Tensorflow for 'Deep Image Homographt Estimation'
When we turn the `Keras` model into normal tf Graph, with totally same parameters and setting, the Loss does NOT constringe. The prediction of our tf network is arrange in a very narrow range around 0 after thousands of training rounds, while the Keras model grows into a reasonable range(single digit).  
When we decrease the learning rate, `Adam(lr=1e-4)`, the tf model also constringe. BUT the rate of the tf model is slow than the Keras model and the Keras also have a better monotonicity. Still waiting for the final constringency loss difference.  
By compareing the two models, the same model, same optimizer & learning_rate. The difference may caused by the initializer or the `model.compile`

**A small problem about how to update sublime `$ sudo apt-get install sublime-text`**

## VPN & SSR Setting
A Google cloud platform(GCP) based method is published here. Two types (VPN/SSR) have been tested. 
### SSR
[here]() is a reference for SSR. But there is a problem that the connection will be blocked after several hours and changing port number can bring the connection back for several hours. 
### VPN
The Windows 10 firewall bring some troubles on both these two types. [Here](https://www.qnap.com/en/how-to/knowledge-base/article/how-to-fix-the-issue-of-windows-10-not-connecting-to-ipsecl2tp-vpn-servers/) is the setting of Registry and [here] is the setting of the firewall rule.
*PPTP 
[Here](http://www.zhoujianhui.com/2019/01/16/centos7搭建pptpvpn一键安装脚本/) is a reference for PPTP. 
*L2TP
[Here](https://www.dazhuanlan.com/2019/12/19/5dfad25c9ac0f/) is a reference for L2PT.
