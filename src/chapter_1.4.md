# 实验二：Qemu 模拟器启动 ArceOS,打印Hello,world

1. 在ArceOS目录下，输入：

```shell
make A=apps/helloworld ARCH=aarch64 PLATFORM=aarch64-raspi4 SMP=4 
```

编译出ArceOS在raspi4 上的一个可执行文件。

2. 在qemu模拟器上运行该文件：

```shell
./qemu-system-aarch64 -m 2G -smp 4 -cpu cortex-a72 -machine raspi4b2g -nographic -kernel {yourpath}/helloworld_aarch64-raspi4.bin
```

3、最后，看到ArceOS在qemu模拟器中成功运行：

```shell
root@DESKTOP-KO8A4KB:~/qemu-patch-raspberry4/build/arceos# make A=apps/helloworld ARCH=aarch64 PLATFORM=aarch64-raspi4 SMP=4
    Building App: helloworld, Arch: aarch64, Platform: aarch64-raspi4, App type: rust
cargo build --target aarch64-unknown-none-softfloat --target-dir /root/qemu-patch-raspberry4/build/arceos/target --release  --manifest-path apps/helloworld/Cargo.toml --features "axstd/log-level-warn axstd/smp"
    Finished release [optimized] target(s) in 0.09s
rust-objcopy --binary-architecture=aarch64 apps/helloworld/helloworld_aarch64-raspi4.elf --strip-all -O binary apps/helloworld/helloworld_aarch64-raspi4.bin
root@DESKTOP-KO8A4KB:~/qemu-patch-raspberry4/build/arceos# cd ..
root@DESKTOP-KO8A4KB:~/qemu-patch-raspberry4/build# ./qemu-system-aarch64 -m 2G -smp 4 -cpu cortex-a72 -machine raspi4b2g -nographic -kernel arceos/apps/helloworld/helloworld_aarch64-raspi4.bin

       d8888                            .d88888b.   .d8888b.
      d88888                           d88P" "Y88b d88P  Y88b
     d88P888                           888     888 Y88b.
    d88P 888 888d888  .d8888b  .d88b.  888     888  "Y888b.
   d88P  888 888P"   d88P"    d8P  Y8b 888     888     "Y88b.
  d88P   888 888     888      88888888 888     888       "888
 d8888888888 888     Y88b.    Y8b.     Y88b. .d88P Y88b  d88P
d88P     888 888      "Y8888P  "Y8888   "Y88888P"   "Y8888P"

arch = aarch64
platform = aarch64-raspi4
target = aarch64-unknown-none-softfloat
smp = 4
build_mode = release
log_level = warn

Hello, world!
```

至此，实验二结束，最终提交ArceOS成功运行，打印Hello，world的结果。



