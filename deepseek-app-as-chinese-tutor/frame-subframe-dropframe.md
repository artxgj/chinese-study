## Frame、Subframe 與 SMPTE Drop Frame 中英雙語詳解
## Frame, Subframe, and SMPTE Drop Frame — Bilingual Deep Dive

---

### 一、Frame（影格／幀）

#### 中文

**Frame（影格／幀）** 是影片或電影中最基本的單一影像單元。在 SMPTE 時間碼（Timecode）中，frame 是時間碼的最小可計數單位，時間碼格式表現為 「**時：分：秒：幀」(HH:MM:SS:FF)** 。

每一個 frame 代表一幅靜態圖像。當一系列 frame 以足夠快的速度連續播放時，人眼便會感知為連續動態影像。Frame 也是剪輯作業中可以切割的最小增量單位。

幀率（Frame Rate）以 fps（frames per second，每秒幀數）表示。常見的幀率包括：

- **24 fps**：電影標準
- **25 fps**：PAL 電視標準（歐洲、澳洲等地）
- **29.97 fps**：NTSC 彩色電視標準（美國、加拿大、墨西哥等）
- **30 fps**：部分 ATSC 廣播標準

在 SMPTE 時間碼中，每一幀都被賦予一個獨特的地址標籤，格式為 `HH:MM:SS:FF`（小時:分鐘:秒:幀）。這個標籤讓剪輯師、音效師和視覺特效團隊能夠在多台攝影機和錄音設備之間保持同步。

---

#### English

A **frame** is the most fundamental single image unit in video or film. In SMPTE timecode, the frame is the smallest countable unit of timecode, formatted as **"Hours:Minutes:Seconds:Frames" (HH:MM:SS:FF)** .

Each frame represents a still image. When a sequence of frames is played back at a sufficiently fast rate, the human eye perceives continuous motion. A frame is also the smallest increment that can be cut in editing.

Frame rate is expressed in fps (frames per second). Common frame rates include:

- **24 fps**: Film standard
- **25 fps**: PAL television standard (Europe, Australia, etc.)
- **29.97 fps**: NTSC color television standard (US, Canada, Mexico, etc.)
- **30 fps**: Some ATSC broadcast standards.

In SMPTE timecode, every frame is assigned a unique address label in the format `HH:MM:SS:FF` (hours:minutes:seconds:frames). This label allows editors, sound mixers, and VFX teams to stay synchronized across multiple cameras and audio recorders.

---

### 二、Subframe（子影格／子幀）

#### 中文

**Subframe（子影格／子幀）** 是比 frame 更精細的時間單位，用於將單一 video frame 的顯示時段進一步細分。

Subframe 的主要用途與特性如下：

1. 高幀率系統的精確同步

當動態捕捉（motion capture）系統的幀率超過標準 SMPTE 時間碼幀率時，系統會在時間碼戳記末端添加一個 subframe 值。這個 subframe 值從 0 開始編號，表示在每個 SMPTE 時間碼 frame 之間所擷取的動態捕捉幀數。

例如：一個 120 FPS 的動態捕捉 session 同步於 30 FPS 的 SMPTE 時間碼訊號源，則每個 SMPTE frame 之間有 4 個動態捕捉幀，這些額外的幀便由 subframe 欄位來表示。其時間碼格式為：**HH:MM:SS:FF.Y**（小時:分鐘:秒:幀.子影格）。

2. 精確定位時間標記

Subframe 可用於將時間標記定位在單一 video frame 所代表的時間區間內的任意位置。在 Apple 的開發者文件中，每個 video frame 通常被細分為 **80 或 100 個 subframes**。

3. 音訊與影像的次幀級同步

在專業影音後製中，subframe 層級的解析度可用於實現音訊與影像的精確同步。

---

#### English

A **subframe** is a finer temporal unit than a frame, used to subdivide the time period during which a single video frame is displayed.

The main uses and characteristics of subframes are as follows:

1. Precise Synchronization for High-Frame-Rate Systems

When a motion capture system's frame rate exceeds standard SMPTE timecode frame rates, the system adds a subframe value at the end of the timecode stamp. This subframe value is 0-based and indicates the increments of captured mocap frames in between every SMPTE timecode frame.

For example: a 120 FPS motion capture session synchronized to a 30 FPS SMPTE timecode source has 4 mocap frames per SMPTE frame — these extra frames are represented by the subframe field. The timecode format is: **HH:MM:SS:FF.Y** (hours:minutes:seconds:frames.subframe).

2. Precise Positioning of Time Markers

Subframes can be used to position a time marker anywhere within the time span represented by a single video frame. In Apple's developer documentation, each video frame is typically subdivided into **80 or 100 subframes**.

3. Subframe-Level Audio/Video Synchronization

In professional post-production, subframe-level resolution enables precise synchronization between audio and video.

---

### 三、SMPTE Drop Frame（丟格時間碼）

#### 中文

**SMPTE Drop Frame（丟格時間碼，簡稱 DF）** 是 SMPTE 時間碼的一種特殊計數方式，專門用於解決 NTSC 彩色電視標準中 29.97 fps 幀率與真實時鐘時間之間的偏差問題。

#### 問題的根源

當 NTSC 彩色電視導入時，為了避免與色彩副載波（color subcarrier）產生干擾，幀率從精確的 30 fps 降為 29.97 fps。

- **30 fps Non-Drop Frame（不丟格）** 以 1:1 的比例為每一幀計數。一小時共有：30 × 60 × 60 = 108,000 幀。
- 但 **29.97 fps** 的實際影片在一小時內只有：29.97 × 60 × 60 = 107,892 幀。

這導致了 **108 幀的差異**，相當於 Non-Drop Frame 時間碼在運行了 1 小時後，會比真實時鐘快 **3.6 秒**（108 ÷ 30 = 3.6 秒）。對於需要精確計時（如廣播節目排程）的應用來說，這是一個嚴重的問題。

#### Drop Frame 的解決方案

Drop Frame **並非真的丟棄任何影像幀**，而是**跳過某些 frame 編號**（frame numbers），讓時間碼的計數與真實時鐘保持同步。

丟格規則：

- 每分鐘的開頭跳過 frame 編號 0 和 1
- 但每第 10 分鐘（即 00、10、20、30、40、50 分）不跳過

這樣的設計使得 Drop Frame 時間碼在節目播出的時間長度內能與真實時鐘保持同步。

如何辨識 Drop Frame vs. Non-Drop Frame

| 時間碼類型                 | 格式                     | 範例          |
|-----------------------| ---------------------- | ----------- |
| *Non-Drop Frame（不丟格）* | 全部使用冒號（:）              | 01:00:00:00 |
| *Drop Frame（丟格）*      | frame 編號前使用分號（;）或句點（.） | 01:00:00;00 |

記憶口訣：分號（;）表示有些數字被跳過了。

使用時機

| 幀率           | 建議使用的時間碼類型     | 原因                   |
|--------------| -------------- | -------------------- |
| *23.976 fps* | Non-Drop Frame | 接近 24 fps，影片來源，偏差不明顯 |
| *29.97 fps*  | Drop Frame     | 廣播標準，需要與真實時間對齊       |
| *59.94 fps*  | Drop Frame     | 29.97 的倍數，相同原理       |
| *30 fps*     | Non-Drop Frame | 與真實時間完全對應            |

---

#### English

SMPTE Drop Frame (DF) is a special counting method of SMPTE timecode designed to solve the discrepancy between the 29.97 fps frame rate of the NTSC color television standard and real wall-clock time.

The Root of the Problem

When NTSC color television was introduced, the frame rate was slowed from exactly 30 fps to 29.97 fps to avoid interference with the color subcarrier.

- 30 fps Non-Drop Frame counts every frame in a 1:1 ratio. One hour contains: 30 × 60 × 60 = 108,000 frames.
- But actual video at 29.97 fps contains only: 29.97 × 60 × 60 = 107,892 frames in one hour.

This results in a difference of 108 frames, meaning that after 1 hour of running, Non-Drop Frame timecode is 3.6 seconds ahead of real clock time (108 ÷ 30 = 3.6 seconds). For applications requiring precise timing (such as broadcast scheduling), this is a serious problem.

The Drop Frame Solution

Drop Frame does not actually drop any video frames — it skips certain frame numbers in the counting process to keep the timecode synchronized with real clock time.

Drop Rules:

- Skip frame numbers 0 and 1 at the start of every minute
- Except every 10th minute (i.e., minutes 00, 10, 20, 30, 40, 50) — do not skip

This design keeps Drop Frame timecode synchronized with real clock time over the duration of a program.

How to Identify Drop Frame vs. Non-Drop Frame

| Timecode Type    | Format                                             | Example     |
|------------------| -------------------------------------------------- | ----------- |
| *Non-Drop Frame* | Colons (:) throughout                              | 01:00:00:00 |
| *Drop Frame*     | Semicolon (;) or period (.) before the frame count | 01:00:00;00 |

Memory aid: the semicolon means some numbers were skipped.

When to Use Which

| Frame Rate   | Recommended Timecode Type | Reason                                             |
|--------------| ------------------------- | -------------------------------------------------- |
| *23.976 fps* | Non-Drop Frame            | Near 24 fps, film-origin — drift is minimal        |
| *29.97 fps*  | Drop Frame                | Broadcast standard — needs to align with real time |
| *59.94 fps*  | Drop Frame                | Multiple of 29.97 — same principle                 |
| *30 fps*     | Non-Drop Frame            | Exactly matches real time                          |

---

#### 總結 Summary

| 概念 Concept | 中文     | 英文         | 核心說明 Core Explanation                                                                                                              |
|------------| ------ | ---------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| *Frame*    | 影格／幀   | Frame      | 影片的最小影像單元，時間碼的基本計數單位。The smallest image unit of video; the basic counting unit of timecode.                                        |
| *Subframe* | 子影格／子幀 | Subframe   | Frame 的進一步細分，用於高精度同步與定位。A finer subdivision of a frame, used for high-precision synchronization and positioning.                   |
| *Drop Frame* | 丟格時間碼  | Drop Frame | 為解決 29.97 fps 與真實時間偏差而設計的跳號計數方式。A skip-number counting method designed to resolve the discrepancy between 29.97 fps and real time. |

這三個概念共同構成了 SMPTE 時間碼系統的完整層級結構：Frame 是基本單位，Subframe 提供更精細的解析度，而 Drop Frame 則是一種特殊的計數規則，確保時間碼在特定幀率下能與真實時間保持一致。

---

These three concepts together form the complete hierarchical structure of the SMPTE timecode system: Frame is the basic unit, Subframe provides finer resolution, and Drop Frame is a special counting rule that ensures timecode remains aligned with real time at specific frame rates.