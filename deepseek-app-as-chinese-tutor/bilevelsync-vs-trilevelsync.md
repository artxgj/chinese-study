# Bi-Level Sync vs Tri-Level Sync：完整详解
# Bi-Level Sync vs Tri-Level Sync: Complete Detailed Explanation

## 一、概述 | Overview

**Bi-Level Sync（双电平同步）**，也称为黑场信号（Black Burst），是一种用于标清（SD）视频系统的模拟同步信号。**Tri-Level Sync（三电平同步）** 是一种主要用于高清（HD）视频系统的模拟同步脉冲信号。

两者都是广播和视频制作领域中用于**同步锁相（Genlock）** 的参考信号，目的是让多台视频设备输出时序一致的信号，从而实现无缝切换和混合。

## 二、Bi-Level Sync（双电平同步）详解

### 2.1 定义 | Definition

**中文**： Bi-Level Sync（双电平同步），又称黑场信号（Black Burst），是一种模拟复合视频信号，画面内容为全黑画面，主要用于标清（SD）视频设备的同步。

**English**: Bi-level sync, also known as black burst, is an analog composite video signal with a black picture, used primarily for synchronizing standard-definition (SD) video equipment.

### 2.2 波形特征 | Waveform Characteristics

**中文**： Bi-Level Sync 的名称来源于其脉冲信号只有两个电平（一个高电平和一个低电平）。其典型波形为一个负向脉冲，电平为 -40 IRE，随后跟随10个周期的彩色副载波（Color Burst）。

**English**: Bi-level sync gets its name from having two voltage levels (a high level and a low level). Its typical waveform is a negative pulse at -40 IRE, followed by 10 cycles of the color subcarrier.

### 2.3 触发方式 | Trigger Method

**中文**： 使用 Bi-Level 同步的系统采用边沿触发（Edge Triggered） 。典型情况下，脉冲的负向下降沿（Negative-going Edge） 作为触发点，显示系统通过检测这个下降沿来重新同步扫描过程。

**English**: Systems using bi-level sync are edge triggered. Typically, the negative-going leading edge of the pulse triggers the synchronization process.

### 2.4 应用标准 | Application Standards

**中文**： Bi-Level Sync 适用于 NTSC、PAL、SECAM 等标清视频标准，以及计算机视频、复合视频、S-Video 和分量视频。

**English**: Bi-level sync is used for NTSC, PAL, SECAM and other standard-definition video standards, as well as computer video, composite video, S-Video, and component video.

### 2.5 主要缺点 | Main Disadvantages

**中文**：

1. 引入直流（DC）分量：作为视频信号的一部分，Bi-Level Sync 会引入不需要的直流分量
2. 同步残留问题：在 RGB 系统中，同步信号可能无法完全从视频通道中分离，残留的同步信号会导致色彩偏移
3. 高清环境下抖动较大：由于频率较低，用于高清视频时更容易产生抖动（Jitter）和撕裂（Tearing）

**English**:

1. Introduces DC component: As part of the video signal, bi-level sync introduces an unwanted DC component
2. Residual sync issue: In RGB systems, sync may not be completely removed from video channels, causing color shifts
3. Higher jitter in HD environments: Due to lower frequency, more prone to jitter and tearing when used for HD video

## 三、Tri-Level Sync（三电平同步）详解

### 3.1 定义 | Definition

**中文**： Tri-Level Sync（三电平同步）是一种模拟视频同步脉冲，主要用于锁定高清（HD）视频信号（同步锁相）。

**English**: Tri-level sync is an analog video synchronization pulse primarily used for the locking of high-definition video signals (genlock).

### 3.2 波形特征 | Waveform Characteristics

**中文**： Tri-Level Sync 的信号包含三个电平：从0V（黑电平）开始，先变为 -300 mV，然后跳变为 +300 mV，最后回到0V。负向脉冲和正向脉冲各持续 40个采样时钟（采样时钟为 74.25 MHz）。每个脉冲宽度为 44个点频周期，总宽度 88个周期。上升/下降时间为 4个采样时钟。

**English**: Tri-level sync consists of three voltage levels: starting from 0V (black level), going to -300 mV, then jumping to +300 mV, and finally returning to 0V. The negative and positive pulses each last 40 sample clocks (sample clock at 74.25 MHz). Each pulse width is 44 pixel clock periods, total 88 periods. Rise/fall time is 4 sample clocks.

### 3.3 触发方式 | Trigger Method

**中文**： 显示系统通过检测同步脉冲过零点（Crossing Zero Point） 来触发同步。

**English**: Display systems trigger synchronization by detecting the sync pulse crossing the zero point.

### 3.4 应用标准 | Application Standards

**中文**： Tri-Level Sync 适用于 HDTV（高清电视） 格式，包括 720p、1080i、1080p 等。由 SMPTE 240 模拟 HDTV 标准引入。

**English**: Tri-level sync is used for HDTV (High Definition Television) formats, including 720p, 1080i, 1080p, etc.. It was introduced with the SMPTE 240 analog HDTV standard.

### 3.5 主要优点 | Main Advantages

**中文**：

1. 零直流（DC）分量：由于脉冲具有正负两种极性，Tri-Level Sync 的 DC 值为 0，这是其最主要的优点
2. 更高频率，更低抖动：更高的频率特性使得时序抖动显著降低
3. 更精确的同步：为三个分量视频信号提供更精确的同步和相对定时
4. 易于数字系统识别：在数字系统中更容易被编码和识别

**English**:

1. Zero DC component: Since pulses have both positive and negative polarities, tri-level sync has a DC value of 0 - its main advantage
2. Higher frequency, lower jitter: Higher frequency characteristics significantly reduce timing jitter
3. More precise synchronization: Provides more accurate sync and relative timing for three component video signals
4. Easy digital recognition: Easier to encode and recognize in digital systems

## 四、核心区别对比 | Core Differences Comparison

| 对比项  | Bi-Level Sync（双电平同步） | Tri-Level Sync（三电平同步）  |
| ---- | -------------------- | ---------------------- |
| 电平数量 | 2个电平（高、低）            | 3个电平（0V、-300mV、+300mV） |
| 主要应用 | 标清（SD）视频             | 高清（HD）视频               |
| 频率   | ~16 MHz（480p）        | ~33 MHz（1080p@30fps）   |
| 触发方式 | 边沿触发（负向下降沿）          | 过零点触发                  |
| 直流分量 | 含有不需要的DC分量           | DC值为0                  |
| 兼容标准 | NTSC、PAL、SECAM       | 720p、1080i、1080p       |
| 引入标准 | RS-170 / RS-343      | SMPTE 240              |
| 抖动性能 | 较高抖动                 | 较低抖动                   |

| Comparison Item      | Bi-Level Sync                  | Tri-Level Sync                |
| -------------------- | ------------------------------ | ----------------------------- |
| Number of Levels     | 2 levels (high, low)           | 3 levels (0V, -300mV, +300mV) |
| Primary Application  | Standard Definition (SD) video | High Definition (HD) video    |
| Frequency            | ~16 MHz (480p)                 | ~33 MHz (1080p@30fps)         |
| Trigger Method       | Edge triggered (negative edge) | Zero-crossing triggered       |
| DC Component         | Contains unwanted DC component | DC value is 0                 |
| Compatible Standards | NTSC, PAL, SECAM               | 720p, 1080i, 1080p            |
| Introduced By        | RS-170 / RS-343                | SMPTE 240                     |
| Jitter Performance   | Higher jitter                  | Lower jitter                  |

## 五、波形图示 | Waveform Diagrams

### 5.1 Bi-Level Sync 波形 | Bi-Level Sync Waveform

```
电压
 ↑
 |   _____________ 视频信号（~0 IRE / 黑电平）
 |  |             |
 |  |   █████████████████████████████████████████████████
 |  |   █████████████████████████████████████████████████   ← 彩色副载波（Color Burst）
 |  |   █████████████████████████████████████████████████   10个周期
 |  |   █████████████████████████████████████████████████
 |  |             |
 |  |_____________|______________________________________→ 时间
 |  |             |
 |  |  负向同步脉冲  |
 |  |  (-40 IRE)   |
 |  |             |
 0 ─┴─────────────┴─────────────────────────────────────→
                    ↑
              下降沿触发点
         (Negative Edge Trigger)

【波形说明】两个电平：视频电平（高）和同步脉冲电平（低）
【Waveform Description】Two levels: video level (high) and sync pulse level (low)
```

### 5.2 Tri-Level Sync 波形 | Tri-Level Sync Waveform

```
电压
 ↑
+300mV ──────┐     ┌──────────
             │     │
   0V ───────┼─────┼─────●─────●──────────────────────→ 时间
             │     │     ↑     ↑
             │     │  过零点   过零点
-300mV ──────┴─────┴──────────
             │     │
          负脉冲   正脉冲
         (-300mV) (+300mV)
         40 clk    40 clk

【波形说明】三个电平：+300mV、0V、-300mV
触发方式：检测过零点（0V交叉点）
【Waveform Description】Three levels: +300mV, 0V, -300mV
Trigger method: zero-crossing detection
```

## 六、ASCII 示意图：系统连接 | ASCII Diagram: System Connection

```
                    ┌─────────────────────────────────────────┐
                    │          SYNC GENERATOR               │
                    │        （同步信号发生器）               │
                    │                                         │
                    │   ┌─────────┐    ┌─────────┐          │
                    │   │Bi-Level │    │Tri-Level│          │
                    │   │  Output │    │  Output │          │
                    │   └────┬────┘    └────┬────┘          │
                    └────────┼──────────────┼────────────────┘
                             │              │
              SD设备 ←───────┘              └───────→ HD设备
         (Bi-Level Sync)                    (Tri-Level Sync)
         NTSC/PAL 设备                      720p/1080p 设备
              │                                  │
              ▼                                  ▼
      ┌───────────────┐                  ┌───────────────┐
      │  SD Camera 1  │                  │  HD Camera 1  │
      ├───────────────┤                  ├───────────────┤
      │  SD Camera 2  │                  │  HD Camera 2  │
      ├───────────────┤                  ├───────────────┤
      │  SD Switcher  │                  │  HD Switcher  │
      └───────────────┘                  └───────────────┘

【说明】两种同步信号不直接兼容，需要转换器才能在异种设备间互连[reference:105]
【Note】The two sync signals are not directly compatible; converters are needed
for interconnection between different types of equipment[reference:106]
```

## 七、兼容性与转换 | Compatibility and Conversion

**中文**： Bi-Level Sync 和 Tri-Level Sync 不直接兼容。高清设备需要 Tri-Level Sync 作为参考信号，而标清设备需要 Bi-Level Sync。在实际应用中，经常需要将 Tri-Level 转换为 Bi-Level Sync。许多现代同步信号发生器（如 LYNX SPG 1708）可以同时输出两种信号。

**English**: Bi-level sync and tri-level sync are not directly compatible. HD equipment requires tri-level sync as reference, while SD equipment requires bi-level sync. In practice, it is often necessary to convert tri-level to bi-level sync. Many modern sync generators (such as LYNX SPG 1708) can output both signals simultaneously.

## 八、总结 | Summary

| 特征   | Bi-Level Sync               | Tri-Level Sync |
| ---- | --------------------------- | -------------- |
| 中文名称 | 双电平同步 / 黑场信号                | 三电平同步          |
| 英文名称 | Bi-Level Sync / Black Burst | Tri-Level Sync |
| 世代   | 传统（SD时代）                    | 现代（HD时代）       |
| 核心优势 | 兼容性广，设备普及                   | 无DC分量，低抖动，高精度  |
| 发展趋势 | 正被Tri-Level逐步取代             | 高清制作标准         |

## 最终结论 | Final Conclusion：

Bi-Level Sync 是标清时代的同步标准，虽然仍在广泛使用，但 Tri-Level Sync 凭借其**零直流分量、更低抖动和更高精度**的优势，已成为高清视频制作的同步标准。随着视频技术向更高分辨率发展，理解这两种同步信号的区别对于视频工程师和广播从业者至关重要。