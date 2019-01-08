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
