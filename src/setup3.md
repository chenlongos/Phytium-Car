# QEMU 模拟器安装

我们需要使用 QEMU 7.0 版本进行实验，低版本的 QEMU 可能导致框架代码不能正常运行。而很多 Linux 发行版的软件包管理器默认软件源中的 QEMU 版本过低，因此我们需要从源码手动编译安装 QEMU 模拟器软件。下面以 Ubuntu 22.04 上的安装流程为例进行说明：

首先我们安装依赖包，获取 QEMU 源代码并手动编译：

```bash
# 安装编译所需的依赖包
sudo apt install autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev \
              gawk build-essential bison flex texinfo gperf libtool patchutils bc \
              zlib1g-dev libexpat-dev pkg-config  libglib2.0-dev libpixman-1-dev libsdl2-dev \
              git tmux python3 python3-pip ninja-build
# 下载源码包
# 如果下载速度过慢可以使用我们提供的百度网盘链接：https://pan.baidu.com/s/1dykndFzY73nqkPL2QXs32Q
# 提取码：jimc
wget https://download.qemu.org/qemu-7.0.0.tar.xz
# 解压
tar xvJf qemu-7.0.0.tar.xz
# 编译安装并配置 RISC-V 支持
cd qemu-7.0.0
./configure --target-list=riscv64-softmmu,riscv64-linux-user  # 如果要支持图形界面，可添加 " --enable-sdl" 参数
make -j$(nproc)
```

> <font color=blue>**注解👉**</font>  
>
> 注意，上面的依赖包可能并不完全，比如在 Ubuntu 18.04 上：
>
> - 出现 `ERROR: pkg-config binary 'pkg-config' not found` 时，可以安装 `pkg-config` 包；
> - 出现 `ERROR: glib-2.48 gthread-2.0 is required to compile QEMU` 时，可以安装 `libglib2.0-dev` 包；
> - 出现 `ERROR: pixman >= 0.21.8 not present` 时，可以安装 `libpixman-1-dev` 包。

另外一些 Linux 发行版编译 QEMU 的依赖包可以从 [这里](https://risc-v-getting-started-guide.readthedocs.io/en/latest/linux-qemu.html#prerequisites) 找到。

之后我们可以在同目录下 `sudo make install` 将 QEMU 安装到 `/usr/local/bin` 目录下，但这样经常会引起冲突。个人来说更习惯的做法是，编辑 `~/.bashrc` 文件（如果使用的是默认的 `bash` 终端），在文件的末尾加入几行：

```bash
# 请注意，/path/to是qemu-7.0.0 的父目录，应调整为实际的安装位置
export PATH=$PATH:/path/to/qemu-7.0.0/build
```

随后即可在当前终端 `source ~/.bashrc` 更新系统路径，或者直接重启一个新的终端。

此时我们可以确认 QEMU 的版本：

```bash
qemu-system-riscv64 --version
qemu-riscv64 --version
```

> <font color=red>**警告⚠️**</font>  
>
> 请尽量不要安装 `qemu-kvm` ，这可能会导致我们的框架无法正常运行。另外，我们仅在 Qemu 7.0.0 版本上进行了测试，请尽量不要切换到其他版本。

### Q & A

当安装编译所需的依赖包出现Package xxx is not available, but is referred to by another package 错误时，可以尝试更新系统软件源，执行如下命令：

```bash
sudo apt-get update
sudo apt-get upgrade
```
