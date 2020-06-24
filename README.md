# Notes4Linux
## Linux(Ubuntu1804)使用中遇到的一些问题和解决办法

+ 安装报错：
  <pre>Errors were encountered while processing:
  electron-ssr</pre>
  + 安装本地包有时会因为缺少依赖报错，此时，执行<code> sudo apt --fix-broken install</code>可以下载缺少的依赖并自动继续安装
+ 新安装Ubuntu后要做些什么？
  + 1、更新内核（看个人情况，不一定有必要，但是更新内核会生成正确的grub引导，可以解决一些引导问题，亲测有效）
    + 先运行 <code> sudo uname -i </code> 查看当前内核版本
    + 去内核（kernel）网址找到自己平台的期望版本<a href="https://kernel.ubuntu.com/~kernel-ppa/mainline/"> https://kernel.ubuntu.com/~kernel-ppa/mainline/ </a>
    + 总共要下载四个(v4.17之前的内核没有modules.deb，只需下载三个)<code>.deb</code>包，一个<code>headers_all.deb</code>，一个<code>headers.deb</code>，一个<code>image.deb</code>，一个<code>modules.deb</code> 。在有些平台上，后三个包可能有两个版本，根据自己情况决定下载哪三个。一般是没有特殊名称的三个。
    + 对于Apache服务器上的镜像，右键复制可得到链接link，分别运行<code>wget link</code>下载所有的包
    + 命令行运行<code> sudo dpkg -i *.deb </code>
  + 2、软件（包）更新
    ```
    sudo apt update
    sudo apt upgrade
    ```
  + 3、安装媒体资源
    + VLC:Ubuntu Store
    + 
  + 4、安装Chrome浏览器
    + 去官网下载<code>.deb</code>安装包<a href="https://www.google.cn/chrome/index.html"> https://www.google.cn/chrome/index.html </a>
    + 右键点击，用Ubuntu的软件安装打开即可安装
  + 5、安装科学上网工具（Clash for Linux）
    + <a href="https://github.com/Dreamacro/clash/releases"> Dreamacro的clash仓库 </a>（Forked），下载clash-linux-amd64-vx.x.x.gz
    + 右键点击，用Ubuntu的归档管理器解压到指定文件夹（一般为<code> /home/user/App/clash/ </code>）,并重命名，可以改为<code> clash </code>
    + cd 到 上面的文件夹，执行<code> ./clash </code>，如果提示权限不足，执行<code> chmod +x clash </code>
    + 第一次执行会提示没有配置文件<code> config </code>和<code> MMDB </code>，会在目录<code> /home/user/.config/clash </code>中创建一个初始的配置文件 config.yaml 以及下载 MMDB 。
    + <code> ctrl + c </code>中止，添加自己的配置文件，重新运行<code> ./clash </code>
    + 登陆 <code> DashBoard </code> 选择节点配置策略，地址为 <code> http://clash.razord.top/ </code>
    + 最后开启系统代理，与配置文件一致即可    
  + 6、安装 <code> snap </code> (与Ubuntu Store搭配使用)
    + Snap是最初由Canonical设计和开发的软件软件包管理/软件部署工具，大多数的Ubuntu发行版都预装有Snap
    + 运行<code> snap </code>可以检测是否安装。如果没有，运行以下命令：
      ```
        sudo apt update
        sudo apt install -y snapd
      ```
    + 用snap搜索包：snap find \<search terms>
    + 安装Snaps: sudo snap install \<snapname>
    + 删除Snaps: sudo snap remove \<snapname>
    + 回滚到以前版本: sudo snap revert \<snap name>
    + 列出已安装的snaps: snap list
  + 7、安装VSCode
    + Ubuntu Store搜索
  + 8、安装N卡驱动（其中一种方法，可用）
    + 基本步骤：首先禁用初始驱动，接着从Nvidia下载相应系统的对应显卡的<code>xxx.run</code>文件，再配置<code>gcc、g++</code>环境，最后运行安装脚本；
    + 如果不知道自己的<code>gcc、g++</code>环境是否满足，可以先安装，如果提示版本太低，就<code>ctrl + c</code>先终止安装过程；
    + <code>gcc --version</code>查看版本；
    + 首先添加仓库
      ```
        sudo apt install software-properties-common
        sudo add-apt-repository ppa:ubuntu-toolchain-r/test
      ```
    + 接着安装想要的版本，<code>sudo apt install gcc-8 g++-8 gcc-9 g++-9</code>
    + gcc的安装不同与软件的更新。安装gcc后，系统中会保存有多个版本的gcc，此时需要调整不同版本的优先级，使得优先级最高的作为系统默认编译器。下面的例子中，gcc-9的优先级最高为90，gcc-8其次80，gcc-7最低70，系统默认为gcc-9。
      ```
        sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 90 --slave /usr/bin/g++ g++ /usr/bin/g++-9 --slave /usr/bin/gcov gcov /usr/bin/gcov-9
        sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 80 --slave /usr/bin/g++ g++ /usr/bin/g++-8 --slave /usr/bin/gcov gcov /usr/bin/gcov-8
        sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 70 --slave /usr/bin/g++ g++ /usr/bin/g++-7 --slave /usr/bin/gcov gcov /usr/bin/gcov-7
      ```
    + 至此，gcc的安装配置结束；执行<code>sudo update-alternatives --config gcc</code>查看系统中全部gcc。
    + 执行命令<code>sudo bash ./NVIDIA-Linux-x86_64-xxx.run</code>，按照指示进行安装。<a href="https://xungejiang.com/2019/10/08/ubuntu-gpu-driver/"> 详细教程 </a>
  + 8、创建程序/可执行文件的快捷方式
    + <code> sudo vi /usr/share/applications/Clash.desktop </code>
    + 编辑内容如下：
      ```
        [Desktop Entry]
          Version=0.20.0
          Name=Clash
          Comment=A rule-based tunnel in Go
          Exec=/home/user/Applications/clash/clash %U
          Icon=/home/user/Applications/clash/clash.png
          Terminal=true
          Type=Application
          Categories=Network
      ```
  + 9、手动安装<code> xxx.deb </code>包
    + cd 到所在目录，执行<code> sudo dpkg -i xxx.deb </code>
  + 10、<code> rm </code> 命令用法
    + 向下递归删除文件夹下所有文件且不作提示：rm -rf folder/，例如rm -rf QQ/
    + 删除文件：rm -f \<path to file>
  + 11、
  + 12、
  + 13、
  + 14、
  + 15、
  + 16、
  + 17、
  + 18、
