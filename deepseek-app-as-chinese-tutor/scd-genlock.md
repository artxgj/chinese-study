# Genlock（发生器锁定 / Generator Locking）—— 完整中英对照详解

---

## 一、核心定义 | Core Definition

#### 中文：
**Genlock**（全称 **Generator Locking**，中文译作“发生器锁定”或“同步锁相”）是一种将多个视频源同步到同一个参考信号源的技术。当多个设备通过这种方式实现同步时，这些设备被称为“发生器锁定”（generator-locked）或“已Genlock”。

#### English：
**Genlock** (short for **Generator Locking**) is a technique for synchronizing multiple video sources to a single reference source or to a reference signal from a signal generator. When sources are synchronized in this way, they are said to be **generator-locked**, or **genlocked**.

---

## 二、通俗理解 | An Intuitive Analogy

#### 中文：
可以把 Genlock 想象成一位 “绝对指挥家” ：

- **Timecode（时间码）** 像墙上的日历——告诉你“今天是几号”；
- **Genlock 像节拍器**——告诉你“此时此刻必须踏出哪一步”。

Timecode 给每一帧起一个名字（元数据），而 Genlock 强迫每一台设备在**同一个微秒**开始捕捉和输出帧。

#### English：
Think of Genlock as an “absolute conductor” :

- Timecode is like a calendar on the wall — it tells you "what date it is";
- Genlock is like a metronome — it tells you the exact heartbeat rhythm at which you must tap your foot.

Timecode gives every frame a unique name (metadata), while Genlock forces every device to capture and output frames at **the exact same microsecond**.

---

## 三、为什么需要 Genlock？| Why Do We Need Genlock?

#### 中文：
视频不是连续信号，而是由一帧一帧的离散画面快速播放而成的。在**多机位直播**（如体育赛事、音乐会）中，如果多台摄像机的画面输出时间不一致，导播切换时就会出**现画面跳跃、撕裂（tearing）或不同步的切换**。

Genlock 确保所有摄像机和视频源不仅帧率相同，而且相位也完全一致。

#### English：
Video is not a continuous signal — it consists of discrete frames (rows of pixels) displayed at high speed. In **multi-camera live broadcasts** (e.g., sports events, concerts), if multiple cameras output frames at slightly different times, switching between them causes **frame jumps, tearing, or asynchronous cuts**.

Genlock ensures that all cameras and video sources run at **not only the same frame rate, but also the exact same phase**.

---

## 四、工作原理 | How Genlock Works

### 4.1 基本流程 | Basic Flow

#### 中文：

1. **主同步信号发生器（Master Sync Generator）** 通过极其稳定的晶体振荡器产生一个基准同步信号。
2. 这个参考信号通过 **SDI 同轴电缆**或网络分发到片场/演播室的每一个设备。
3. 每个设备内部的**锁相环（PLL — Phase-Locked Loop）** 电路将自己的时钟锁定到参考信号上。
4. 所有设备从此拥有**同一个“心跳”** ——在完全相同的时刻开始每一帧。

#### English:

1. A **Master Sync Generator** produces a reference sync signal via an extremely stable crystal oscillator.
2. This reference signal is distributed to every device in the studio/studio via **SDI coaxial cables** or networks.
3. Each device's internal **Phase-Locked Loop (PLL)** circuit locks its own clock to the reference signal.
4. All devices now share the **same "heartbeat"** — they begin each frame at the exact same moment.

---

### 4.2 文字流程图 | Text-Based Flowchart

```
┌─────────────────────────────────────────────────────────────────┐
│                    主同步信号发生器                              │
│               (Master Sync Generator)                          │
│            ┌───────────────────────┐                          │
│            │  恒温晶体振荡器 (OCXO) │                          │
│            └───────────────────────┘                          │
│                         │                                      │
│                         ▼                                      │
│            ┌───────────────────────┐                          │
│            │  产生 Genlock 参考信号  │                          │
│            │  (Blackburst / Tri-Level Sync)                   │
│            └───────────────────────┘                          │
└─────────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              │               │               │
              ▼               ▼               ▼
      ┌───────────┐   ┌───────────┐   ┌───────────┐
      │  摄像机A   │   │  摄像机B   │   │  LED处理器 │
      │ ┌───────┐ │   │ ┌───────┐ │   │ ┌───────┐ │
      │ │  PLL  │ │   │ │  PLL  │ │   │ │  PLL  │ │
      │ └───────┘ │   │ └───────┘ │   │ └───────┘ │
      │     │     │   │     │     │   │     │     │
      │     ▼     │   │     ▼     │   │     ▼     │
      │ 锁定参考  │   │ 锁定参考  │   │ 锁定参考  │
      │ 同步输出  │   │ 同步输出  │   │ 同步输出  │
      └───────────┘   └───────────┘   └───────────┘
              │               │               │
              └───────────────┼───────────────┘
                              ▼
                    ┌─────────────────┐
                    │  所有设备相位一致 │
                    │  (完美同步)      │
                    └─────────────────┘
```

---

### 4.3 锁相环 (PLL) 的核心作用 | The Core Role of PLL

#### 中文：
Genlock 的核心技术是 **锁相环（Phase-Locked Loop, PLL）** 。PLL 包含以下几个关键模块：

| 模块                      | 功能                  |
| ----------------------- | ------------------- |
| **相位检测器 (Phase Detector)**  | 比较本地信号与参考信号的相位差     |
| **环路滤波器 (Loop Filter)**     | 平滑相位误差信号            |
| **压控振荡器 (VCO)**             | 根据误差信号调整输出频率        |
| **分频器 (Frequency Divider)** | 将 VCO 输出分频后反馈给相位检测器 |

PLL 通过负反馈机制不断调整，直到本地信号与参考信号频率相同、相位锁定。

#### English:
The core technology of Genlock is the **Phase-Locked Loop (PLL)** . A PLL consists of the following key modules:

| Module                              | Function                                                          |
| ----------------------------------- | ----------------------------------------------------------------- |
| **Phase Detector**                      | Compares the phase difference between local and reference signals |
| **Loop Filter**                         | Smooths the phase error signal                                    |
| **Voltage-Controlled Oscillator (VCO)** | Adjusts output frequency based on the error signal                |
| **Frequency Divider**                   | Divides VCO output and feeds back to the phase detector           |

The PLL continuously adjusts via negative feedback until the local signal and the reference signal are frequency-locked and phase-locked.

---

### 4.4 PLL 文字流程图 | PLL Text-Based Flowchart

```
                    ┌─────────────────────────────────────────┐
                    │             参考信号输入                 │
                    │        (Reference Signal Input)         │
                    └───────────────────┬─────────────────────┘
                                        │
                                        ▼
                    ┌─────────────────────────────────────────┐
                    │         相位检测器 (Phase Detector)      │
                    │     比较相位差 → 输出误差信号             │
                    │   Compare phases → Output error signal  │
                    └───────────────────┬─────────────────────┘
                                        │
                                        ▼
                    ┌─────────────────────────────────────────┐
                    │        环路滤波器 (Loop Filter)          │
                    │       平滑误差信号 (Smooth error)        │
                    └───────────────────┬─────────────────────┘
                                        │
                                        ▼
                    ┌─────────────────────────────────────────┐
                    │     压控振荡器 (VCO)                    │
                    │   根据电压调整频率 (Adjust frequency)    │
                    └───────────────────┬─────────────────────┘
                                        │
                    ┌───────────────────┴─────────────────────┐
                    │                                         │
                    ▼                                         ▼
        ┌───────────────────────┐             ┌───────────────────────┐
        │    输出信号 (Output)   │             │   分频器 (Divider)     │
        │   (锁定后的时钟信号)   │             │    (反馈路径)          │
        └───────────────────────┘             └───────────┬───────────┘
                                                          │
                                                          ▼
                                              (反馈回相位检测器)
                                            (Feedback to Phase Detector)
```

---

## 五、Genlock 信号的类型 | Types of Genlock Signals

#### 中文：
根据视频制式的不同，Genlock 参考信号主要有两种形式：

| 信号类型                   | 适用格式             | 说明                   |
| ---------------------- | ---------------- | -------------------- |
| **黑场信号 (Black Burst)**     | 标清 SD / 部分 1080i | 传统复合视频信号，包含色同步和垂直同步  |
| **三电平同步 (Tri-Level Sync)** | 高清 HD / 4K       | 更高频率、更低抖动，是 HD 环境的首选 |

此外，在现代 IP 化制作中，**PTP（精确时间协议，IEEE 1588）** 正逐渐成为网络化 Genlock 的替代方案。

#### English:
Depending on the video format, Genlock reference signals come in two main forms:

| Signal Type    | Supported Formats | Description                                                      |
| -------------- | ----------------- | ---------------------------------------------------------------- |
| Black Burst    | SD / some 1080i   | Legacy composite video signal with color burst and vertical sync |
| Tri-Level Sync | HD / 4K           | Higher frequency, lower jitter — preferred for HD environments   |

In addition, in modern IP-based production, **PTP (Precision Time Protocol, IEEE 1588)** is increasingly becoming an alternative to Genlock in networked environments.

---

## 六、主要应用场景 | Main Applications

#### 中文：

| 应用场景                           | 说明                          |
| ------------------------------ | --------------------------- |
| **多机位直播**                          | 体育赛事、音乐会等需要多机位快速切换的场景       |
| **虚拟制片 (XR / Virtual Production)** | 同步摄像机快门与 LED 屏幕刷新，避免扫描条纹和撕裂 |
| **立体 3D 拍摄**                       | 同步两台摄像机的帧起始时间               |
| **广播演播室**                          | 所有摄像机和播放设备锁定到同一个“室内基准”      |
| **多显示器同步**                         | 确保电影中出现的多个 CRT 显示器无闪烁       |

#### English:

| Application                    | Description                                                                          |
| ------------------------------ | ------------------------------------------------------------------------------------ |
| **Multi-camera live broadcasting** | Sports, concerts — scenarios requiring fast switching between multiple cameras       |
| **XR / Virtual Production**        | Synchronizing camera shutters with LED screen refresh to avoid scanlines and tearing |
| **Stereoscopic 3D shooting**       | Synchronizing frame start times of two cameras                                       |
| **Broadcast studios**              | All cameras and playback devices locked to the same "house reference"                |
| **Multi-display sync**             | Ensuring multiple CRT monitors appearing in a movie are flicker-free                 |

---

## 七、Genlock vs Timecode vs Word Clock

#### 中文：
这三者常被混淆，但功能完全不同：

| 特性     | Genlock       | Timecode（时间码） | Word Clock（字时钟） |
| ------ | ------------- | ------------- | --------------- |
| **作用**     | 同步视频的**帧相位和帧率**   | 给每一帧**贴标签/编地址**   | 同步**音频**设备的采样时钟     |
| **信号类型**   | 模拟电脉冲（黑场/三电平） | 元数据 / 时间戳     | 数字时钟信号          |
| **解决什么问题** | “何时开始画这一帧？”   | “这是第几帧？”      | “以多快速度采样？”      |
| **典型用途**   | 直播切换          | 后期剪辑对齐        | 音频系统同步          |

#### English:
These three are often confused, but they serve completely different functions:

| Feature        | Genlock                                         | Timecode                    | Word Clock                         |
| -------------- | ----------------------------------------------- | --------------------------- | ---------------------------------- |
| **Function**       | Syncs video **frame phase and frame rate**          | **Labels/addresses** each frame | Syncs **audio** device sampling clocks |
| **Signal Type**    | Analog electrical pulse (Black Burst/Tri-Level) | Metadata / timestamp        | Digital clock signal               |
| **Problem Solved** | "When to start drawing this frame?"             | "Which frame is this?"      | "How fast to sample?"              |
| **Typical Use**    | Live switching                                  | Post-production alignment   | Audio system synchronization       |

---

## 八、Genlock vs Frame Lock（帧锁定）

#### 中文：

- **Genlock（发生器锁定）**：设备的扫描频率与**外部频率源**保持一致。
- **Frame Lock（帧锁定）**：多台设备的显示器扫描频率**彼此保持一致**。

简单说：Genlock 是“大家都听同一个指挥”；Frame Lock 是“大家互相看齐”。

#### English:

- **Genlock (Generator Locking)**：A device's scan frequency is locked to an **external frequency source**.
- **Frame Lock**：The display scan frequencies of multiple devices are locked **to each other**.

Simply put: Genlock means "everyone follows the same conductor"; Frame Lock means "everyone aligns with each other."

---

## 九、总结 | Summary

#### 中文：

| 要点   | 说明                                       |
| ---- | ---------------------------------------- |
| **全称**   | Generator Locking（发生器锁定）                 |
| **本质**   | 用统一的参考信号同步多个视频设备的时钟                      |
| **核心技术** | 锁相环 (PLL)                                |
| **信号形式** | 黑场 (Black Burst) 或三电平同步 (Tri-Level Sync) |
| **主要目的** | 避免多机位切换时的画面撕裂、跳跃和不同步                     |
| **关键区别** | Timecode 管“命名”，Genlock 管“节奏”             |

#### English:

| Key Point       | Description                                                                       |
| --------------- | --------------------------------------------------------------------------------- |
| **Full Name**       | Generator Locking                                                                 |
| **Essence**         | Synchronizing multiple video devices' clocks to a unified reference signal        |
| **Core Technology** | Phase-Locked Loop (PLL)                                                           |
| **Signal Forms**    | Black Burst or Tri-Level Sync                                                     |
| **Main Purpose**    | Avoiding tearing, jumps, and async issues when switching between multiple cameras |
| **Key Distinction** | Timecode handles "naming," Genlock handles "rhythm"                               |

---

## 十、术语对照表 | Terminology Glossary

| 中文           | English                             |
| ------------ | ----------------------------------- |
| 发生器锁定 / 同步锁相 | Generator Locking / Genlock         |
| 锁相环          | Phase-Locked Loop (PLL)             |
| 压控振荡器        | Voltage-Controlled Oscillator (VCO) |
| 相位检测器        | Phase Detector                      |
| 环路滤波器        | Loop Filter                         |
| 黑场信号         | Black Burst                         |
| 三电平同步        | Tri-Level Sync                      |
| 帧锁定          | Frame Lock                          |
| 时间码          | Timecode                            |
| 字时钟          | Word Clock                          |
| 精确时间协议       | Precision Time Protocol (PTP)       |
| 主同步信号发生器     | Master Sync Generator               |
| 画面撕裂         | Tearing                             |
| 虚拟制片         | Virtual Production (XR)             |
