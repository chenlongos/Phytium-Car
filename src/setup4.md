# 其它工具安装

本节主要介绍 arceos 编译所需的依赖工具的安装。

为了使用 objdump、objcopy 工具，我们首先需要安装名为 cargo-binutils 的命令行工具集：

```bash
cargo install cargo-binutils
```

为了编译运行 C 应用程序，安装 `libclang-dev`：

```bash
sudo apt install libclang-dev
```

下载并安装 `cross-musl-based`工具链：

```bash
# download
wget https://musl.cc/aarch64-linux-musl-cross.tgz
wget https://musl.cc/riscv64-linux-musl-cross.tgz
wget https://musl.cc/x86_64-linux-musl-cross.tgz
# install
tar zxf aarch64-linux-musl-cross.tgz
tar zxf riscv64-linux-musl-cross.tgz
tar zxf x86_64-linux-musl-cross.tgz
# exec below command in bash OR add below info in ~/.bashrc
export PATH=`pwd`/x86_64-linux-musl-cross/bin:`pwd`/aarch64-linux-musl-cross/bin:`pwd`/riscv64-linux-musl-cross/bin:$PATH
```
