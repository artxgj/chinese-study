### 多機系統同步設定完整指南（中英雙語）

### Complete Bilingual Guide: Setting Up Genlock, Tri-level Sync & LTC for Multi-Camera Systems

---

#### 前言 / Introduction

本指南涵蓋從核心原理、實際接線、選單設定到最終驗收的完整流程。適用於電影級多機（如 Sony Venice、RED、ARRI）或廣播級 EFP 現場製作系統。

This comprehensive guide covers the entire workflow from core principles, physical cabling, and menu configurations to final verification. It is applicable to cinema-grade multi-camera rigs (e.g., Sony Venice, RED, ARRI) or broadcast EFP live production systems.

---

#### 第一部分：核心概念釐清 / Part 1: Core Concepts

**Q1：這三個訊號分別負責什麼？**<br>
**Q1: What do these three signals do respectively?**

· **Genlock (同步鎖相)**：確保所有攝影機的**畫格邊緣**完全對齊。它是「骨架」，讓視訊切換器（Switcher）能在無撕裂（Glitch-free）的情況下進行即時切換。<br>
· **Genlock**：Ensures the **frame edges** of all cameras are perfectly aligned. It acts as the "skeleton," allowing the video switcher to perform real-time cuts without glitches.

· **Tri-level Sync (三級同步)**：這是**高畫質（HD）與高速格率**專用的 Genlock 訊號類型（取代舊式 Black Burst）。它提供極精確的畫格起始點，適合 1080p/4K 及慢動作（HFR）系統。<br>
· **Tri-level Sync**：This is the specific **HD and high-frame-rate** Genlock signal type (replacing old Black Bur<br>st). It provides extremely precise frame-starting points, suitable for 1080p/4K and High Frame Rate (HFR) systems.

· **Linear Timecode (LTC / 線性時間碼)**：這是一條獨立的**音頻等級**類比訊號，負責讓所有攝影機的**時間戳（時：分：秒：格）**完全一致，確保後期剪輯時所有素材能「一秒對齊」。<br>
· **Linear Timecode (LTC)**：This is a separate **audio-level** analog signal responsible for making the **timestamps (Hour:Min:Sec:Frame)** identical across all cameras, ensuring all clips align perfectly in post-production.

---

#### 第二部分：實體接線架構 / Part 2: Physical Wiring Topology

建議採用「星型+並聯」混合拓撲（Star + Daisy-chain hybrid）。<br>
A "Star + Daisy-chain hybrid" topology is recommended.

訊號分配放大器（DA）是必備品。切勿直接將一條 BNC 線並聯分接給多台攝影機（會導致阻抗不匹配與訊號衰減）。務必使用 DA 將 1 路輸入轉為多路獨立輸出。<br>
A Distribution Amplifier (DA) is mandatory. **Never** passively split a single BNC cable to multiple cameras (this causes impedance mismatch and signal attenuation). Always use a DA to convert 1 input into multiple isolated outputs.

##### 接線順序（Wiring Sequence）：

· **Genlock (Tri-level)**：主時鐘產生器（Master Clock）輸出 → DA 輸入 → DA 輸出（x N 路）→ 各攝影機 **GENLOCK/REF IN**（BNC 埠）。<br>
· **Genlock (Tri-level)**：Master Clock Out → DA In → DA Out (x N) → Each camera's **GENLOCK/REF IN** (BNC port).

· **LTC (Timecode)**：主時鐘 LTC 輸出 → DA（或專用 TC 分配器）→ 各攝影機 **TC IN**（通常為 BNC 或 XLR，須確認阻抗設定為 Hi-Z）。<br>
· **LTC (Timecode)**：Master Clock LTC Out → DA (or dedicated TC distributor) → Each camera's **TC IN** (usually BNC or XLR, ensure impedance is set to Hi-Z).

· **進階技巧**：若攝影機支援，可將 LTC 嵌入 **Audio Input (Ch 1)**，但正規作業強烈建議走專用 TC BNC 以減少干擾。<br>
· **Advanced Tip**：If the camera supports it, you can embed LTC into the **Audio Input (Ch 1)**. However, for professional workflows, a dedicated TC BNC is highly recommended to minimize interference.

---

#### 第三部分：攝影機選單詳細設定 / Part 3: Camera Menu Settings

以下為通用邏輯（以 ARRI Alexa 或 Sony Venice 為範例）。請務必依序檢查。

The following is the general logic (using ARRI Alexa or Sony Venice as examples). Please check them sequentially.

| 設定項目 (Menu Item)    | 設定值 (Setting)                   | 中文說明 (Chinese)                                   | 英文備註 (English Notes)                                                                                                     |
| ------------------- | ------------------------------- | ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| **System / Genlock**    | **External / Tri-level**            | 強制設為外部同步，並指定訊號類型為三級同步。                           | Force to external sync and specify the signal type as Tri-level. Do NOT set to "Free Run" or "Internal".                 |
| **Frame Rate (FPS)**    | **23.98 / 25 / 29.97**              | **關鍵！** 必須與 Master Clock 的 Timecode Frame Rate 完全一致。 | **Critical!** Must exactly match the Master Clock's Timecode Frame Rate. Check pulldown settings if Genlock uses interlaced. |
| **Timecode Mode**       | **External / Continuously running** | 設為「外部時間碼」並開啟「持續讀取」。                              | Set to "Ext TC" or "Regen". Ensure "TOD" (Time of Day) mode is locked.                                                   |
| **Phase / Scan Offset** | **0 (Adjust if needed)**            | 調整畫素延遲。若多機監看有細微左右偏移，可微調此值。                       | Adjust pixel delay. Only tweak if there's a sub-frame horizontal shift between cameras on an oscilloscope.               |

---

#### 第四部分：實戰設定步驟（SOP）/ Part 4: Step-by-Step SOP

**步驟 1**：**暖機與基礎設定（Step 1: Power-Up & Base Config）**<br>
開啟所有設備，先將所有攝影機的 FPS、解析度、色彩空間（Color Space）調至完全統一。<br>
Power on all devices. First, unify the FPS, resolution, and Color Space across all cameras to be absolutely identical.

**步驟 2**：**發送 Genlock（Step 2: Inject Genlock Sync）**<br>
將 Master Clock 打開，確認 Tri-level 輸出指示燈亮起。觀察攝影機螢幕左上角，通常 **"Gen" 圖示會由紅轉綠**，代表已鎖定外部同步。
Turn on the Master Clock and confirm the Tri-level output indicator is lit. Check the camera monitor's top-left corner; the **"Gen" icon will typically turn from Red to Green**, indicating external sync is locked.

**步驟 3**：**發送 LTC（Step 3: Inject LTC Timecode）**<br>
送出 LTC 訊號（通常以 Jam-Sync 方式）。攝影機 TC 顯示器應開始跳動，且不會出現閃爍（即代表無中斷）。<br>
Send the LTC signal (usually via Jam-Sync). The camera's TC display should start running steadily without flickering, which means there are no interruptions.

**步驟 4**：**相位對齊驗證（Step 4: Phase Alignment Verification）**<br>
使用 **Vectorscope / Waveform Monitor** 觀看兩台攝影機的輸出。切換至「外部同步」模式，檢查垂直遮沒區（VBI）是否重疊。若無儀器，可拍攝高速閃光燈測試是否同時捕捉到完整光脈衝。<br>
Use a **Vectorscope / Waveform Monitor** to view the outputs of two cameras. Switch to "External Sync" mode and check if the Vertical Blanking Intervals (VBI) overlap. Without instruments, shoot a high-speed strobe light to test if both capture the full light pulse simultaneously.

**步驟 5**：**時間碼壓印（Step 5: Timecode Burn-in Verification）**<br>
開啟攝影機的「顯示輸出」功能，將 Timecode 壓印在 SDI 回傳畫面上。若兩台攝影機同時按下快門，讀數誤差必須 **0 格**（Frame-accurate）。<br>
Enable the camera's "Display Output" to burn the Timecode onto the SDI return feed. When shutter buttons are pressed simultaneously on two cameras, the reading difference must be **0 frames** (Frame-accurate).

---

### 第五部分：常見陷阱與除錯 / Part 5: Common Pitfalls & Troubleshooting

· **陷阱 1（Pitfall 1）：格率牴觸（Frame Rate Mismatch）**<br>
&nbsp;&nbsp;若 Genlock 設定為 59.94 Hz，但 LTC 設定為 23.98 fps，攝影機會拒絕同步。**解法**：確認 Genlock 的「分頻係數」對應 LTC。<br>
&nbsp;&nbsp;If Genlock is set to 59.94 Hz but LTC is set to 23.98 fps, the camera will reject synchronization. **Solution**：Ensure the Genlock's "division factor" corresponds to the LTC frame rate.

· **陷阱 2（Pitfall 2）：終端阻抗（Improper Termination）**<br>
&nbsp;&nbsp;若使用類比訊號串接（Daisy-chain）而非 DA，請務必將線路「末端」的攝影機打開 **75-ohm Termination** 開關，否則會產生回波反射導致畫面抖動。<br>
&nbsp;&nbsp;If using analog daisy-chaining instead of a DA, be sure to turn on the **75-ohm Termination** switch on the last camera in the chain. Otherwise, echo reflections will cause picture jitter.

· **陷阱 3（Pitfall 3）：LTC 音量過大（LTC Audio Level Too High）**<br>
&nbsp;&nbsp;若將 LTC 誤入 Audio In，且未衰減（Attenuation），會造成音頻破音與時間碼解碼失敗。LTC 標準電平為 **0 dBu ~ +4 dBu**。<br>
&nbsp;&nbsp;If LTC is accidentally fed into Audio In without attenuation, it will cause audio clipping and timecode decoding failure. The standard LTC level is **0 dBu ~ +4 dBu**.

---

### 第六部分：最終驗收清單 / Part 6: Final Checklist

□ 所有攝影機的 **Genlock LED** 顯示為鎖定（綠色／白色）。<br>
&nbsp;&nbsp;&nbsp;&nbsp;All cameras' Genlock LEDs show locked (Green / White).
 cameras' **Genlock LEDs** show locked (Green / White).

□ 所有攝影機的 **TC 讀數** 在靜止狀態下完全一致（包含 Drop-Frame 與 Non-Drop 設定）。<br>
&nbsp;&nbsp;&nbsp;&nbsp;All cameras' **TC readings** are perfectly identical in still state (including Drop-Frame vs. Non-Drop settings).

□ 將所有攝影機對準同一顆時鐘（或手機碼錶），錄製 1 分鐘。播放時，暫停任兩台畫面，格數差異為 **±0 格**。<br>
&nbsp;&nbsp;&nbsp;&nbsp;Point all cameras at the same clock (or phone stopwatch) and record for 1 minute. During playback, pause any two cameras; the frame count difference must be **±0 frames**.

□ 即時切換（Cut）畫面時，切換器螢幕無黑畫面或抖動（即同步成功）。<br>
&nbsp;&nbsp;&nbsp;&nbsp;When performing a live Cut, the switcher monitor shows no black screen or jitter (indicating successful sync).

---

### 總結 / Final Summary

把 **Tri-level Sync** 當作「心臟跳動的節拍器」，確保每台攝影機在同一毫秒睜開眼睛；把 **LTC** 當作「日記本的頁碼」，確保剪接師能找到同一頁。兩者缺一不可，且務必透過 DA 分配，嚴守 75 歐姆阻抗規範。

Treat **Tri-level Genlock** as the "metronome" forcing every camera to open its shutter at the exact same millisecond, and treat **LTC** as the "page numbers" for your editor to locate frames. Both are mandatory. Always distribute via a DA and strictly adhere to 75-ohm termination rules to avoid signal reflections. When both are locked, you achieve true **Frame-Accurate** multi-camera production.
