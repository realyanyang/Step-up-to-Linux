# 记录Linux的安装与配置过程
安装为Ubuntu单系统
### 1.准备
1. 下载[Ubuntu](https://www.ubuntu.com/index_kylin)镜像, 建议下载LTS版本。
(官网下载速度较慢时考虑[清华镜像](https://mirrors.tuna.tsinghua.edu.cn/))
2. 制作启动盘, 建议使用[Rufus](https://rufus.ie/),操作简单，在制作时要格式化Ｕ盘，先做好Ｕ盘备份工作。
3. 若要安装双系统，还要清理出一块磁盘空间，参考[知乎文章](https://zhuanlan.zhihu.com/p/35970220)

### 2.安装(以DELL为例)
1. 插入Ｕ盘，启动过程中出现DELL徽标时，快速按F12键。这将使您转至Boot Once（启动一次）菜单。通过键盘选择UEFI启动方式。同时，建议按F2进入BIOS，将 Security 选项中的 Secure Boot 设置为 Disabled 。
2. 选择语言，键盘布局，一路Next就好，在安装类型时！  
前两个选项会自动分区安装，省时省心，但这里推荐自己手动分区，这样可以熟悉自己的分区方便后续管理，因此这里我们选择*其他选项*  
![image]()