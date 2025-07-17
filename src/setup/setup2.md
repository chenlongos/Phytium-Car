# Rust 开发环境配置

首先安装 Rust 版本管理器 rustup 和 Rust 包管理器 cargo，这里我们用官方的安装脚本来安装：

`curl https://sh.rustup.rs -sSf | sh`

如果通过官方的脚本下载失败了，可以在浏览器的地址栏中输入 [https://sh.rustup.rs](https://sh.rustup.rs/) 来下载脚本，在本地运行即可。

如果官方的脚本在运行时出现了网络速度较慢的问题，可选地可以通过修改 rustup 的镜像地址（修改为中国科学技术大学的镜像服务器）来加速：

```bash
export RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static
export RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup
curl https://sh.rustup.rs -sSf | sh
```

或者使用 tuna 源来加速 [参见 rustup 帮助](https://mirrors.tuna.tsinghua.edu.cn/help/rustup/)：

```bash
export RUSTUP_DIST_SERVER=https://mirrors.tuna.tsinghua.edu.cn/rustup
export RUSTUP_UPDATE_ROOT=https://mirrors.tuna.tsinghua.edu.cn/rustup/rustup
curl https://sh.rustup.rs -sSf | sh
```

或者也可以通过在运行前设置命令行中的科学上网代理来实现：

```bash
# e.g. Shadowsocks 代理，请根据自身配置灵活调整下面的链接
export https_proxy=http://127.0.0.1:1080
export http_proxy=http://127.0.0.1:1080
export ftp_proxy=http://127.0.0.1:1080
```

安装完成后，我们可以重新打开一个终端来让之前设置的环境变量生效。我们也可以手动将环境变量设置应用到当前终端，只需要输入以下命令：

`source $HOME/.cargo/env`

接下来，我们可以确认一下我们正确安装了 Rust 工具链：

`rustc --version`

可以看到当前安装的工具链的版本。

`rustc 1.70.0 (90c541806 2023-05-31)`



可通过如下命令安装 rustc 的 nightly 版本，并把该版本设置为 rustc 的默认版本。

```bash
rustup install nightly
rustup default nightly
```


我们最好把软件包管理器 cargo 所用的软件包镜像地址 crates.io 也换成中国科学技术大学的镜像服务器来加速三方库的下载。我们打开（如果没有就新建） `~/.cargo/config` 文件，并把内容修改为：

```
[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"
replace-with = 'ustc'
[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"
```

同样，也可以使用 tuna 源 [参见 crates.io 帮助](https://mirrors.tuna.tsinghua.edu.cn/help/crates.io-index.git/)：

```
[source.crates-io]
replace-with = 'tuna'

[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"
```

接下来安装一些Rust相关的软件包

```bash
rustup target add riscv64gc-unknown-none-elf
rustup component add llvm-tools-preview
rustup component add rust-src
```

> <font color=red>**警告⚠️**</font>  
>
> 如果你更换了另外一个 rustc 编译器（必须是 nightly 版的），**需要重新安装上述 rustc 所需软件包**。ArceOS 仓库中的 `Makefile` 包含了这些工具的安装，如果你使用 `make run` 也可以不手动安装。

至于 Rust 开发环境，推荐 JetBrains Clion + Rust插件 或者 Visual Studio Code 搭配 rust-analyzer 和 RISC-V Support 插件。

> <font color=blue>**注解👉**</font>  
>
> JetBrains Clion 付费商业软件，但对于学生和教师，只要在 JetBrains 网站注册账号，可以享受一定期限（半年左右）的免费使用的福利。
>
> - Visual Studio Code 是开源软件，不用付费就可使用。
> - 当然，采用 VIM，Emacs 等传统的编辑器也是没有问题的。
