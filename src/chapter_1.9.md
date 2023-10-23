# 补充知识：树莓派如何与PC相连、串口软件的使用


## 树莓派如何与PC相连

将三根串口连接线分别了解到编号为6,8,10的三个引脚处。

![](assert/引脚.PNG)

![](https://github.com/chenlongos/raspi4-with-arceos-doc/blob/master/src/assert/%E6%8E%A5%E7%BA%BF.jpg?raw=true)



再将一根USB串口转换线与连接到树莓派的线相连，其中TXD对应RXD，RXD对应TXD，GND对应GND。（默认情况下，USB串口转换线中黑色代表GND，白色代表RXD，绿色代表TXD，红色不连接）

## 串口软件的使用

以putty为例：

连接类型选择Serial，再将Serial line改为COM3,Speed(波特率)设为115200，最后点击Open即可。

![](https://raw.githubusercontent.com/chenlongos/raspi4-with-arceos-doc/master/src/assert/putty.png)

