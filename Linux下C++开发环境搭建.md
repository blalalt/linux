# Linux下C++开发环境搭建

以centos7为主机，[参考来源]( https://www.sylar.top/sblog/detail/42 )

## 0. 虚拟机环境配置

下载 VMware

下载 Centos 镜像，下载minimal版本即可

安装虚拟机

虚拟机网络配置

```shell
# 虚拟机使用NAT模式
# 查看虚拟机网络编辑器中 NAT 模式 分配的 IP 网关
# 修改 网络配置器 中的 vmnet-8，修改 IPV4 IP，网关，和虚拟机网络编辑器保持一致
# 修改 虚拟机系统配置
vim /etc/sysconfig/network-scripts/ifclf-eng33
# 加入
IPADDR=192.168.**.** # 分配的 IP 中同一网段的其他地址
DNS=114.114.114.114
ONBOOT=yes # 开机启动
GATEWAY= # 虚拟机网络编辑器中配置的网关
# 重启网卡
service network restart
```

## 1. 安装基础包

```shell
yum install glibc-static
yum install glibc-devel
yum install glibc-headers
yum install boost-devel
yum install mysql-devel
yum install openssl-devel
yum install git
yum install wget
yum install ncurses-devel
yum install gcc gcc-c++
yum install ctags
```

## 2. 安装Oh-my-zsh

```shell
yum install zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
# 常用插件
## 直接使用
(git extract z)
## 下载使用
(incr autojump auto-suggestions)
```

## 3. 安装VIM

系统所内置的vim版本较低，不支持C++新标准，手动安装最新版本的vim

```shell
wget ftp://ftp.vim.org/pub/vim/unix/vim-8.1.tar.bz2
tar xvf vim-8.1.tar.bz2
cd vim81
make install
# 配置(C++)
git clone https://github.com/sylar-yin/myvim.git
cp myvim/.vim ~/ -rf
cp myvim/.vimrc ~/

alias vctags="ctags -R --c++-kinds=+p --fields=+iaS --extra=+q"
#添加到/etc/profile末尾
```

## 4. 安装工具链

在安装的时候可以指定安装路径，建议安装到一个特定的文件夹下，把路径加入到环境变量中，防止和系统内置的冲突

```shell
# 例如
./configure --prefix=/home/tools/cpp

# 将路径加入环境变量
export PATH=/home/tools/cpp/bin:$PATH
```

#### 4.1 安装CMake

```shell
wget https://github.com/Kitware/CMake/releases/download/v3.15.0-rc1/cmake-3.15.0-rc1.tar.gz
tar xvf cmake-3.15.0-rc1.tar.gz
cd cmake-3.15.0-rc1
./configure --prefix=/home/tools/cpp
make -j
make install
```

#### 4.2 安装Boost

```shell
yum install boost-devel
```

#### 4.3 安装GDB

```shell
wget http://ftp.gnu.org/gnu/gdb/gdb-8.3.tar.xz
tar xvf gdb-8.3.tar.xz
cd gdb-8.3
./configure --prefix=/home/tools/cpp
make -j
make install
```

#### 4.4 安装GCC

新版本的gcc能够更好的支持C++的最新特性

###### 4.4.1 安装依赖

```shell
sudo yum install bison
sudo yum install texinfo
```

安装automake

```shell
# 安装automake
wget http://ftp.gnu.org/gnu/automake/automake-1.15.tar.gz
tar xvf automake-1.15.tar.gz
cd automake-1.15
./configure --prefix=/home/tools/cpp
#修改Makefile 查找 /doc\/automake\-$(APIVERSION)
#doc/automake-$(APIVERSION).1: $(automake_script) lib/Automake/Config.pm
#    $(update_mans) automake-$(APIVERSION) --no-discard-stderr
#(3686行，加上--no-discard-stderr)
make -j
make install
```

安装autoconf

```shell
wget http://ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.gz
tar xvf autoconf-2.69.tar.gz
cd autoconf-2.69
./configure --prefix=/home/tools/cpp
make -j
make install
```

###### 4.4.2 安装GCC

```shell
wget http://ftp.tsukuba.wide.ad.jp/software/gcc/releases/gcc-9.1.0/gcc-9.1.0.tar.xz
tar xvJf gcc-9.1.0.tar.xz
cd gcc-9.1.0
sh contrib/download_prerequisites
mkdir build
cd build
../configure --enable-checking=release --enable-languages=c,c++ --disable-multilib --prefix=/home/tools/cpp
make -j
make install
```

## 5. 安装 Man Page

``` shell
yum install man
yum -y install man-pages
```

## 6. 添加库的搜索路径

运行时动态库的搜索路径的先后顺序是：
 1.编译目标代码时指定的动态库搜索路径；
 2.环境变量`LD_LIBRARY_PATH`指定的动态库搜索路径；
 3.配置文件/etc/ld.so.conf中指定的动态库搜索路径；
 4.默认的动态库搜索路径/lib和/usr/lib；

```shell
vim /etc/profile
# 添加以下几句
export LD_LIBRARY_PATH=/home/tools/cpp/lib64:$LD_LIBRARY_PATH
source /etc/profile # 生效
```

## 7. 离线下载

不翻墙的话，一些库或者组件的下载速度很慢，所以放到了百度云上。

链接：https://pan.baidu.com/s/1ovQcsEh0SfLbCAawoXq-GA 
提取码：wmqr