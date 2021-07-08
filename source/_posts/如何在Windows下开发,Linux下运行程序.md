---

title: 如何在Windows下开发,Linux下运行程序
date: 2015-12-31 16:07:43
categories: [Windows, Linux]
tags: [Windows, Linux]

---

```
As we know, 服务端程序一般都是运行在Linux系统中,而Linux下的一些日常软件,却没办法做到像Windows下那样的精美,so,问题来了:有没有办法做到在Windows上进行代码的编写,而运行是在linux下呢???
```

## 环境需求:
* 虚拟机(vmware,vbox...)
* Windows 操作系统


## 虚拟机安装linux
 在Windows下想要同时使用到linux系统,果断是必须安装虚拟机的,如何安装虚拟机在这里就略过了,
一般服务器都是采用Centos系统作为服务器运行的Linux系统,所以我们选择一个centos的镜像网站
下载centos镜像[浙江大学镜像](http://mirrors.zju.edu.cn/centos/),一般选择6.*版本的,这个是centos6.7版本的镜像路径:http://mirrors.zju.edu.cn/centos/6.7/isos/x86_64/  ,选择minimal版本iso镜像下载,不需要带桌面的,可以减少虚拟机的资源占用.
减下来就是安装了,略过.

##   设置虚拟机网络.
安装的Linux虚拟机我们会通过ssh进行连接,所以虚拟机和本机之间必须通过桥接模式进行连接.设置虚拟机网络桥接不同虚拟机有不同的设置方法,略过.

<!-- more -->
## 通过ssh连接到Linux
在虚拟机中ifconfig得到虚拟机的IP地址,
在Windows下通过`SecureCRT`或其他的ssh连接工具连接到Linux中.(一般Linux默认开启了ssh支持,用户密码为你的系统用户密码.)

## 将Windows下的文件实时同步到Linux中
成功通过ssh连接到虚拟机的Linux后,我们要把在Windows下编辑的代码"复制"到Linux中,在Linux中运行,
so,what to do?? 这里我们需要用到Linux下的[mount](http://www.baidu.com/s?wd=mount)命令,将Windows下的文件Mount到Linux下,当Windows下文件改变时,Linux文件也会改变.

命令如下:
```
mount -t cifs -o username=administrator,password=123456 //你windows的ip地址/git /mnt/git

```

命令讲解:
.-t vfstype 指定文件系统的类型为 [cifs](https://www.baidu.com/s?wd=cifs)类型
.-o 指定用户名和密码(windows系统用户密码)
如果不想输入用户密码,可以参照[这里](http://www.windows7en.com/Win7/20234.html)


### 准备工作
1. 在Linux下创建挂载文件存放路径

	```
	cd /mnt && mkdir git
	```

	* 必须创建,不然无法挂载.

2.  [设置windows下文件夹为共享](http://jingyan.baidu.com/article/295430f13cc4e60c7e005095.html)

3. 清空Linux的iptable
` iptables -F `

4. 修改~/.bashrc 加入
`/sbin/iptables -F`这样启动的时候,会自动清除iptable限制

### 在ssh终端输入命令
```
mount -t cifs -o username=administrator,password=123456 //你windows的ip地址/git /mnt/git

```

现在可以在windows下的git文件新建任意文件,Linux都会实时同步.











