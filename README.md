
> 声明：本项目软硬件均有参考逐飞科技基于CH32V203的无感无刷驱动开源项目，感兴趣的朋友可以参考其gitee仓库(https://gitee.com/seekfree/CH32V203_BLDC_Project)

## 项目简介
本项目是一款专为小型多旋翼无人机或高性能机器人设计的十字异形四合一电子调速器（ESC）。板载4路独立的无刷电机驱动电路，主控采用国产高性能 RISC-V 内核单片机，实现了高集成度、低成本的动力驱动方案，适合作为自动化专业毕业设计、电子竞赛或 DIY 无人机爱好者的参考与实践平台。

## 项目功能
- 四路独立驱动：单板集成4路完整的电机驱动通道，可同时或独立控制4个无刷电机。
- 高频PWM控制：支持通过外部 PWM 信号或串口通信进行调速控制。
- 信号反馈与检测：硬件电路上预留了电流采样（BATADC）与比较器反馈接口（CMPO），为无感 FOC 控制或方波驱动算法的开发提供了硬件基础 。
- 异形十字布局：专为多旋翼无人机设计的十字形 PCB 外形，完美契合机架中心位置，最大限度节省布线空间。

## 项目参数
- 核心主控：采用4颗沁恒 CH32V203C8T6 芯片作为各路电机的独立逻辑主控，主频高达 144MHz，定时器资源丰富 。
- 驱动级方案：采用 FD6288Q 三相全桥栅极驱动器，搭配低内阻 MOSFET AON6354，保证大电流下的高效能输出 。
- 输入电压：支持 12V 级直流输入（适配 3S 锂电池） 。
- 电源管理：集成 SY8205FCC 同步降压芯片将 12V 降至 5V，配合 RT9013-33GB 线性稳压器输出纯净 3.3V，确保 MCU 和模拟采样电路稳定运行 。


## 原理解析（硬件说明）
本项目硬件主要由逻辑控制部分、电源转换部分、三相功率驱动部分及外围接口组成。
### 主控与逻辑电路
每路电调采用一颗独立的 CH32V203C8T6 单片机，负责接收上位机/飞控指令并生成控制电机的 6 路 PWM 信号。预留了 SWD 下载接口与串口引脚，方便后期代码调试。

![image.png](https://image.lceda.cn/oshwhub/pullImage/3c57f6f62dcc4c82a108491c84946785.png)

![image.png](https://image.lceda.cn/oshwhub/pullImage/6d59491111b54df08f0ecd3c3c39584a.png)

![image.png](https://image.lceda.cn/oshwhub/pullImage/36d8cf61e28d4a5caf6e523af4f61ce5.png)
### 三相全桥驱动电路
驱动部分使用了 FD6288Q。单路电机使用了 6 颗 AON6354 功率管组成三相半桥结构 。大电流走线部分进行了大面积露铜处理，以增加过流能力并辅助散热。

![image.png](https://image.lceda.cn/oshwhub/pullImage/9b9ef254e4544d0f981659ca1ca1bb14.png)

![image.png](https://image.lceda.cn/oshwhub/pullImage/07bc0959f8594272a03d6ed8d7f4074f.png)
### 电源转换电路
为了隔离动力电源的干扰，采用 SY8205FCC 方案进行一级降压（12V转5V），再通过 RT9013 LDO 进行二级降压得到 3.3V 供主控使用 。输入端预留了外部大电容的焊接孔，用于吸收电机刹车产生的反动势。


![image.png](https://image.lceda.cn/oshwhub/pullImage/6b0749b9b25f469186671f6dc47d9128.png)

![image.png](https://image.lceda.cn/oshwhub/pullImage/3582885697484dd4a0bcda6948accd8b.png)

## 软件代码

由于作者是新手小白，代码水平不高，所以这里只提供了固件，方便测试。需要的朋友可以在我github仓库中下载：https://github.com/ZGY313/CH32V203_ESC.git

## 注意事项
- 大电流焊接：PCB 上的大面积露铜处（如 VCCBAT 节点与电机线接口）吸热较快，手工焊接功率管 AON6354 时建议配合加热台使用，避免虚焊 。
- 绝缘与防护：由于板子底部有较多裸露焊点，安装至碳纤机架前，请务必做好底部的绝缘处理（如贴绝缘胶带或使用尼龙柱垫高），防止短路。

## 实物图

![板子.jpg](https://image.lceda.cn/oshwhub/pullImage/24d98f203e414abe88e21a8e521f4cd9.jpg)


![186f0r186f0r186f.jpg](https://image.lceda.cn/oshwhub/pullImage/962ec37842c6480d97e2c64c1c45bd49.jpg)


![868119f5a26698f6d7e7f43422820f5a.jpg](https://image.lceda.cn/oshwhub/pullImage/937e9a321e6e4c87880de91943696e25.jpg)



https://github.com/user-attachments/assets/b4f2f102-448a-438b-9e24-a001206e98ff



