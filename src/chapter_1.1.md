# 任务一：Qemu 开发环境搭建

- Qemu GitHub 网址：<https://github.com/qemu/qemu>
- Qemu 使用手册：<https://www.qemu.org/docs/master/system/index.html>

1. 克隆这个仓库来生成新的qemu支持树莓派4

   ```
   git clone https://github.com/0xMirasio/qemu-patch-raspberry4.git
   ```
   ```
   root@uBuntu:~/Github/Chenlong# git clone https://github.com/0xMirasio/qemu-patch-raspberry4
   Cloning into 'qemu-patch-raspberry4'...
   remote: Enumerating objects: 605399, done.
   remote: Counting objects: 100% (1/1), done.
   remote: Total 605399 (delta 0), reused 0 (delta 0), pack-reused 605398
   Receiving objects: 100% (605399/605399), 360.98 MiB | 11.84 MiB/s, done.
   Resolving deltas: 100% (489320/489320), done.
   root@uBuntu:~/Github/Chenlong# cd qemu-patch-raspberry4/
   root@uBuntu:~/Github/Chenlong/qemu-patch-raspberry4# ls
   accel           common-user    dtc                   io             memory_ldst.c.inc  page-vary-common.c    qemu-keymap.c    scripts         trace
   audio           configs        dump                  iothread.c     meson              pc-bios               qemu-nbd.c       scsi            trace-events
   authz           configure      ebpf                  job.c          meson.build        plugins               qemu.nsi         semihosting     ui
   backends        contrib        fpu                   job-qmp.c      meson_options.txt  po                    qemu-options.hx  slirp           util
   block           COPYING        fsdev                 Kconfig        migration          python                qemu.sasl        softmmu         VERSION
   block.c         COPYING.LIB    gdbstub.c             Kconfig.host   module-common.c    qapi                  qga              storage-daemon  version.rc
   blockdev.c      cpu.c          gdb-xml               libdecnumber   monitor            qemu-bridge-helper.c  qobject          stubs
   blockdev-nbd.c  cpus-common.c  gitdm.config          LICENSE        nbd                qemu-edid.c           qom              subprojects
   blockjob.c      crypto         hmp-commands.hx       linux-headers  net                qemu-img.c            README.rst       target
   bsd-user        disas          hmp-commands-info.hx  linux-user     os-posix.c         qemu-img-cmds.hx      replay           tcg
   capstone        disas.c        hw                    MAINTAINERS    os-win32.c         qemu-io.c             replication.c    tests
   chardev         docs           include               Makefile       page-vary.c        qemu-io-cmds.c        roms             tools
   ```

2. 然后，执行以下操作进行编译：

   ```shell
   root@uBuntu:~/Github/Chenlong/qemu-patch-raspberry4# mkdir build
   root@uBuntu:~/Github/Chenlong/qemu-patch-raspberry4# cd build/
   root@uBuntu:~/Github/Chenlong/qemu-patch-raspberry4/build# ../configure
   root@uBuntu:~/Github/Chenlong/qemu-patch-raspberry4/build# make
   ```

3. 将 Qemu 目录下 build 目录加入进`PATH`环境变量

   ```shell
   export PATH=(your path/qemu/build):$PATH
   ```

4. 设置 Qemu 编译生成的目标为 RISC-V，AArch64，X86

   ```shell
   ./configure --target-list=aarch64-softmmu
   ```

   - 如果需要配置多个目标，可在 target-list=后面添加，并以逗号分隔，例如：

   ```shell
   ./configure --target-list=aarch64-softmmu,riscv64-softmmu,x86_64-softmmu
   ```

   ![result](assert/task1.1.1.png)

   - 其他可生成的目标：

     ```shell
     set target list (default: build all)
     Available targets: aarch64-softmmu alpha-softmmu
     arm-softmmu avr-softmmu cris-softmmu hppa-softmmu
     loongarch64-softmmu m68k-softmmu
     microblaze-softmmu microblazeel-softmmu mips-softmmu
     mips64-softmmu mips64el-softmmu mipsel-softmmu
     nios2-softmmu or1k-softmmu ppc-softmmu ppc64-softmmu
     riscv32-softmmu riscv64-softmmu rx-softmmu
     s390x-softmmu sh4-softmmu sh4eb-softmmu
     sparc-softmmu sparc64-softmmu tricore-softmmu
     x86_64-softmmu xtensa-softmmu xtensaeb-softmmu
     ```

5. 编译

   ```shell
   make
   ```

![result](assert/task1.1.2.png)

- Qemu for RISC-V
- Qemu for AArch64
- Qemu for X86
