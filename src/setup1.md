# OS 环境配置

目前实验内容可支持在 [Ubuntu操作系统](https://cdimage.ubuntu.com/releases/) 上进行操作。

对于 Windows10/11 的用户可以通过系统内置的 WSL2 虚拟机（请不要使用 WSL1）来安装 Ubuntu 22.04，步骤如下：

- 升级 Windows 10/11 到最新版（Windows 10 版本 18917 或以后的内部版本）。注意，如果不是 Windows 10/11 专业版，可能需要手动更新，在微软官网上下载。升级之后， 可以在 PowerShell 中输入 `winver` 命令来查看内部版本号。

- 「Windows 设置 > 更新和安全 > Windows 预览体验计划」处选择加入 “Dev 开发者模式”。

- 以管理员身份打开 PowerShell 终端并输入以下命令：

  ```powershell
  # 启用 Windows 功能：“适用于 Linux 的 Windows 子系统”
  >> dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
  
  # 启用 Windows 功能：“已安装的系统虚拟机平台”
  >> dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
  
  # <Distro> 改为对应从微软应用商店安装的 Linux 版本名，比如：`wsl --set-version Ubuntu 2`
  # 如果你没有提前从微软应用商店安装任何 Linux 版本，请跳过此步骤
  >> wsl --set-version <Distro> 2
  
  # 设置默认为 WSL 2，如果 Windows 版本不够，这条命令会出错
  >> wsl --set-default-version 2
  
  # 启用运行 WSL 并安装 Linux 的 Ubuntu 发行版所需的功能，根据向导设置用户名和密码
  >> wsl --install
  ```

> <font color=blue>**注解👉**</font>  
>
> 由于时间问题我们主要在 Ubuntu22.04  on x86-64 上进行了测试，后面的配置也是基于此环境进行。你也可以在其他 Linux 发行版上进行实验，基本上不会出现太大的问题。如果遇到了问题的话，请在本节的讨论区中留言，我们会尽量帮助解决。

参考：[使用 WSL 在 Windows 上安装 Linux](https://docs.microsoft.com/zh-cn/windows/wsl/install-win10#step-4---download-the-linux-kernel-update-package)
