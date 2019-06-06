# 记录Linux的安装与配置过程
安装为Ubuntu单系统
### 1.准备
1. 下载[Ubuntu](https://www.ubuntu.com/index_kylin)镜像, 建议下载LTS版本。
(官网下载速度较慢时考虑[清华镜像](https://mirrors.tuna.tsinghua.edu.cn/))
2. 制作启动盘, 建议使用[Rufus](https://rufus.ie/),操作简单，在制作时要格式化Ｕ盘，先做好Ｕ盘备份工作。
3. 若要安装双系统，还要清理出一块磁盘空间，参考[知乎文章](https://zhuanlan.zhihu.com/p/35970220)

### 2.安装(以DELL为例)
建议先阅读一些安装教程:  
> https://blog.csdn.net/baidu_36602427/article/details/86548203  
> https://www.jianshu.com/p/54d9a3a695cc  
> http://tonyzzx.github.io/2017/12/23/U%20%E7%9B%98%E5%AE%89%E8%A3%85%20Ubuntu%2016.04%20%EF%BC%88%E5%8D%95%E7%B3%BB%E7%BB%9F%EF%BC%89%E7%AE%80%E6%98%93%E6%95%99%E7%A8%8B/  
> https://blog.csdn.net/u010801439/article/details/80485251  
> https://www.glbwl.com/u-install-ubuntu.html  

1. 插入Ｕ盘，启动过程中出现DELL徽标时，快速按F12键。这将使您转至Boot Once（启动一次）菜单。通过键盘选择UEFI启动方式。同时，建议按F2进入BIOS，将 Security 选项中的 Secure Boot 设置为 Disabled 。
2. 选择语言，键盘布局，一路Next就好，在选择网络时*不建议联网，建议正常安装并且为图形或无线硬件以及其他媒体格式安装第三方软件*，在安装类型时！  
前两个选项会自动分区安装，省时省心，但这里推荐自己手动分区，这样可以熟悉自己的分区方便后续管理，因此这里我们选择*其他选项*  
3. 接下来我们需要手动分区(自动分区即可跳过次步骤)。以本人电脑为例，128G固态，500G机械。  
    * EFI分区　512M SSD 主分区　空间起始位置 (用作EFI启动，512M足够)
    * /　SSD剩余空间　主分区　空间起始位置　(根目录)
    * 交换分区　4G 机械硬盘　主分区　空间起始位置　(按照内存大小来设置，一般为内存大小的两倍，但也不至于过大，本机内存12G)
    * /home　机械硬盘剩余空间　逻辑分区　空间起始位置　(存放个人文件，越大越好)
    * **注意:** 安装启动引导器的设备选择为EFI分区对应的位置  
4. 等待安装完成 ... ... 
5. 享用全新的Ubuntu　Ｏ（∩＿∩）Ｏ～～

### 3.配置
1. 美化  
**一个赏心悦目的界面能给人工作的欲望**  
此处的美化强烈推荐知乎专栏[世上最良心***](https://zhuanlan.zhihu.com/p/63584709)，按照文章一步步来就能打造很美观的界面  
**注意:** 不要操作文章开头关于安装显卡驱动的部分，我们会在后续安装。
2. Deep Learning 配置  
    建议先阅读几篇教程，对流程有基本了解:  
    > https://blog.csdn.net/xierhacker/article/details/53035989  
    > https://blog.csdn.net/dudu815110/article/details/87167518  
    > http://www.pianshen.com/article/527837750/  
    > https://www.vicw.com/groups/water/topics/200  
    > https://blog.csdn.net/qq_32408773/article/details/84111244  
    > https://my.oschina.net/u/2306127/blog/2877804  

    Ubuntu18.04自带Python3.6，关于如何安装pip或者conda请自行百度  
    在安装pip之前，建议安装几个依赖包: `sudo apt install build-essential python3-dev python3-setuptools`
* 安装NVIDIA驱动  
    1. 可以使用命令 `lshw -c video` 查看机器的显卡型号，根据显卡型号去[NVIDIA官网](https://www.nvidia.cn/)下载最新的驱动程序  
    2. 由于系统自带了nouveau的开源驱动，在安装NVIDIA驱动前要现将其屏蔽
        * 编辑系统黑名单　`sudo vi /etc/modprobe.d/blacklist.conf`
        * 在末尾新增一行　`blacklist nouveau`
        * 保存，退出，然后更新initramfs　`sudo update-initramfs -u`
        * 重启电脑　`reboot`
        * 查看是否禁用成功　`lsmod | grep nouveau` 若没有输出，则表明禁用成功  
    3. 卸载原驱动　`sudo apt-get --purge remove nvidia-*` 无论是否安装过，执行此命令是没有坏处的  
    4. 停止可视化界面　`sudo telinit 3` 输入用户名和密码
    5. 运行下载的run文件　`sudo bash NVIDIA-Linux-x86_64*****.run`　，大部分选择默认提示即可，在GCC版本不匹配时，我选择了忽略，依然可以安装运行成功，目前还没有遇到问题  
    6. 安装完成后重启，运行 `nvidia-smi` 可以看到NVIDIA的输出即安装成功

* 安装cuda  
    1. 去[CUDA官网](https://developer.nvidia.com/cuda-downloads)下载所需要的版本，建议下载runfile，　Base 和 Patch 部分都建议下载
    2. 建议在安装前阅读官方的document，以cuda10.0为例，其中比较有建设性的为[这里](https://docs.nvidia.com/cuda/archive/10.0/cuda-quick-start-guide/index.html#ubuntu-x86_64)  
    3. 接下来先运行base installer的文件 `sudo bash cuda_10.0******_linux.run`  
    **注意:** 运行后在提示是否安装驱动时(Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 384.81?)，　选择**否！**，其他地方默认安装即可
    4. 安装完成后，将以下命令加入到`~/.bashrc`当中，以改变环境变量(摘抄自官方document)
    ``
    export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    ``
    重启shell
    5. 验证  
    在命令行输入 `nvcc -V`，会输出版本信息  
    进入




