### [命令]()

| 命令                              | 备注                                          |
| --------------------------------- | --------------------------------------------- |
| pwd                               | 当前目录的绝对路径（print working directory） |
| env                               | 列出所有环境变量                              |
| echo $PATH                        | 列出环境变量中PATH的值                        |
| cat [file_path]                   | 打印一个文件的内容                            |
| xdg-open ./                       | 从bash打开文件窗口                            |
| top                               | 查系统状况                                    |
| cp -r [direction_a] [direction_b] | 循环复制                                      |

环境变量文件及加载顺序

https://www.cnblogs.com/youyoui/p/10680329.html

用了zsh之后，配置在~/.zshrc里

## 双系统Ubuntu22.04

https://blog.csdn.net/Flag_ing/article/details/121908340



## 换源

/etc/apt/source.list

deb http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse

## Nvidia driver

https://blog.csdn.net/Perfect886/article/details/119109380



## 安装nvidia驱动后 网卡丢失

```
Sudo lshw -c network

```

显示 *-network UNCLAIMED 



要用其他电脑下载了拷贝过来

https://askubuntu.com/questions/1437477/22-04-1-network-unclaimed

下载地址

http://mirrors.edge.kernel.org/ubuntu/ubuntu/ubuntu/ubuntu/ubuntu/pool/main/l/linux-hwe-5.19/

## 输入法

https://blog.csdn.net/qq_19331985/article/details/125998219



## VPN

### V2Ray gui

https://github.com/v2rayA/v2rayA

### Clash for windows



## github ssh key

```cpp
// check if configured before
git config user.name和git config user.email
  

git config --global user.name ‘xxxxx’ 
git config --global user.email ‘xxx@xx.xxx’
ssh-keygen -t rsa -C ‘上面的邮箱’
  
// 把rsa.pub的内容复制到github
```



## Oh my ZSH

https://blog.csdn.net/weixin_42326144/article/details/121957795

```cpp
cat /etc/shells
 
sudo apt-get install zsh
  
 sudo  curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh 

// 设置为默认shell
chsh -s $(which zsh) // 然后注销重新登录

  
```

如果要用到bash（比如配置source .bashrc）

切换shell  `exec bash`  `exec zsh`

配置

https://github.com/ohmyzsh/ohmyzsh/

乱码

搜索powerline 最后在bash窗口设置里换字体 如果配置没能成功 注销试试

## CLion

用了zsh之后配置在 ~/.zshrc里

```
# CLion 
CLION_PATH=/home/chenhui/App/clion-2023.1.2/bin 
PATH=$PATH:$CLION_PATH
```

## Houdini

linux版本官网下载说明

遇到qt问题

https://www.sidefx.com/forum/topic/81990/?page=1#post-376619

## OpenGL

https://crainyday.gitee.io/Ubuntu_004.html

```
sudo apt-get install build-essential libgl1-mesa-dev
sudo apt-get install libglew-dev libsdl2-dev libsdl2-image-dev libglm-dev libfreetype6-dev
sudo apt-get install libglfw3-dev libglfw3
```

## TBB

```
sudo apt-get install libtbb-dev
```



## cuda

在已经安装好nvdia driver的基础上，只需要安装cuda-toolkit

**查看driver版本**

```
nvidia-smi                                
Thu Jun 29 12:15:02 2023       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 525.116.04   Driver Version: 525.116.04   CUDA Version: 12.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  Off  | 00000000:01:00.0  On |                  N/A |
|  0%   51C    P8    19W / 200W |    337MiB /  8192MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A      1325      G   /usr/lib/xorg/Xorg                249MiB |
|    0   N/A  N/A      1607      G   /usr/bin/gnome-shell               28MiB |
|    0   N/A  N/A      2220      G   ...mviewer/tv_bin/TeamViewer       14MiB |
|    0   N/A  N/A    296705      G   ...182476128516229679,262144       41MiB |
+-----------------------------------------------------------------------------+

```

**google "cuda 12.0 download"**

https://developer.nvidia.com/cuda-12-0-0-download-archive?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=deb_network

注意最后不要`sudo apt-get -y install cuda`那会重新下载驱动，而应该`sudo apt-get -y install cuda-toolkit`

**修改env path**

```
# chenhui's env variables
# CLion 
CLION_PATH=/home/chenhui/App/clion-2023.1.2/bin
# Cuda
CUDA_PATH=/usr/local/cuda-12.0/bin
PATH=$PATH:$CLION_PATH:$CUDA_PATH


```



**安装错版本怎么办**

1. cuda 路径

```
$ ll /usr/local | grep cuda
lrwxrwxrwx  1 root    root      22  6月 29 10:43 cuda -> /etc/alternatives/cuda
lrwxrwxrwx  1 root    root      25  6月 29 10:43 cuda-12 -> /etc/alternatives/cuda-12
drwxr-xr-x 15 root    root    4.0K  6月 29 12:12 cuda-12.0
```

2. 把下错的版本先删掉

```mv cuda-12.2 /.trash```

3. 把软连接都改了

```
sudo ln -sf /usr/local/cuda-12.0 cuda

-sf 表示修改  
ln -sf <A> <B>
B指向A
```

4. 修改env path
5. `nvcc --version`

