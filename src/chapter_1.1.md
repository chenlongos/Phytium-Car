# 实验一：支持树莓派4b的 Qemu 环境搭建

1. 克隆这个仓库来生成新的 qemu 用来支持树莓派4b

   ```
   git clone https://github.com/0xMirasio/qemu-patch-raspberry4.git
   ```
   ```shell
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
   mkdir build
   cd build/
   ../configure
   make
   ```
   ```shell
   root@uBuntu:~/Github/Chenlong/qemu-patch-raspberry4# mkdir build
   root@uBuntu:~/Github/Chenlong/qemu-patch-raspberry4# cd build/
   root@uBuntu:~/Github/Chenlong/qemu-patch-raspberry4/build# ../configure
   root@uBuntu:~/Github/Chenlong/qemu-patch-raspberry4/build# make
   ```

3. 编译时间较长，完成后生成的可执行文件就在当前 qemu 项目的 build 目录下，版本号为 QEMU emulator version 6.2.50 ：
   
   ```shell
   root@uBuntu:~/Github/Chenlong/qemu-patch-raspberry4/build# ls qemu-system-*
   qemu-system-aarch64  qemu-system-cris  qemu-system-microblaze    qemu-system-mips64el  qemu-system-ppc      qemu-system-rx     qemu-system-sparc    qemu-system-xtensa
   qemu-system-alpha    qemu-system-hppa  qemu-system-microblazeel  qemu-system-mipsel    qemu-system-ppc64    qemu-system-s390x  qemu-system-sparc64  qemu-system-xtensaeb
   qemu-system-arm      qemu-system-i386  qemu-system-mips          qemu-system-nios2     qemu-system-riscv32  qemu-system-sh4    qemu-system-tricore
   qemu-system-avr      qemu-system-m68k  qemu-system-mips64        qemu-system-or1k      qemu-system-riscv64  qemu-system-sh4eb  qemu-system-x86_64

   root@uBuntu:~/Github/Chenlong/qemu-patch-raspberry4/build# ./qemu-system-aarch64 --version
   QEMU emulator version 6.2.50 (v6.2.0-1433-g6213f46ca3)
   Copyright (c) 2003-2022 Fabrice Bellard and the QEMU Project developers
   ```
4. 最后，执行

   ```shell
   qemu-system-aarch64 -M help | grep ras
   ```

   可以看到 qemu 已经支持 树莓派4b 了

   ```shell
   root@iZ2ze7ajbjlxlzqf7yrk1zZ:~/Github/Chenlong/qemu-patch-raspberry4/build# ./qemu-system-aarch64 -M help | grep ras
   raspi0               Raspberry Pi Zero (revision 1.2)
   raspi1ap             Raspberry Pi A+ (revision 1.1)
   raspi2b              Raspberry Pi 2B (revision 1.1)
   raspi3ap             Raspberry Pi 3A+ (revision 1.0)
   raspi3b              Raspberry Pi 3B (revision 1.2)
   raspi4b1g            Raspberry Pi 4B (revision 1.1)
   raspi4b2g            Raspberry Pi 4B (revision 1.2)
   ```
至此，实验一结束，最终提交执行`qemu-system-aarch64 -M help | grep ras`命令后生成的结果，要求可以看到已经支持树莓派4b了。

