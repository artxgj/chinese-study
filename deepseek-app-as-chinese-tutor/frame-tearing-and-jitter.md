
#### 1. 概覽 Overview

- Jitter（延遲抖動）和 Frame Tearing（畫面撕裂）都是影響視覺流暢度的顯示異常，但它們的成因與表現截然不同。Jitter 關乎**時間軸上的不均勻**，而 Tearing 關乎**空間軸上的不同步**。
- Jitter and Frame Tearing are both visual anomalies that ruin smoothness, but they stem from completely different root causes. Jitter relates to **uneven timing on the timeline**, while Tearing relates to **asynchronization in spatial display**.

---

#### 2. 深度解析 Jitter（延遲抖動 / 幀數不穩）

- **定義**： Jitter 指的是每一幀（Frame）顯示之間的時間間隔不一致。理想狀態下，60Hz 螢幕每 16.7ms 顯示一幀。若遊戲幀數從 60fps 瞬間掉到 30fps，或渲染時間忽長忽短（例如 10ms、25ms、12ms），就會產生 Jitter。這導致物體移動時出現明顯的「卡頓感」（Stutter）或「跳格」，儘管平均幀率可能很高。
- **Definition**: Jitter refers to the variation in frame presentation time. Ideally, a 60Hz monitor displays a frame every 16.7ms. If the frame rate drops from 60fps to 30fps instantly, or if rendering times fluctuate erratically (e.g., 10ms, 25ms, 12ms), Jitter occurs. This causes visible "stutter" or "hitching" during motion, even if the average FPS is high. 
- **主要成因 (Root Causes)**：
  - CPU/GPU 渲染負載驟變（如遊戲場景爆炸特效）、背景程式搶佔資源、驅動程式延遲（DPC Latency）、或顯示器 VRR（可變刷新率）範圍不足導致 LFC（低幀率補償）頻繁切換。
  - Sudden CPU/GPU workload spikes (e.g., explosion effects), background processes hogging resources, high DPC (Deferred Procedure Call) latency, or insufficient VRR range causing LFC (Low Framerate Compensation) to toggle frequently.
- **視覺表現 (Visual Symptoms)**：
  - 物體移動時「一頓一頓」，像是輕微煞車，但幀率顯示器上的數字卻沒大幅波動。鏡頭平移（Panning）時尤其明顯。
  - Objects move in a "juddery" or "hiccupping" manner, like a slight brake tap, even if the FPS counter looks stable. Especially noticeable during camera panning.
- **解決方案 (Solutions)**：
  - 鎖定幀率（如 RTSS 限幀）、關閉背景應用、開啟 NVIDIA Reflex / AMD Anti-Lag 降低渲染佇列延遲，或調整 VRR 範圍。
  - Cap the frame rate (e.g., using RTSS), close background apps, enable NVIDIA Reflex / AMD Anti-Lag to reduce render queue backpressure, or fine-tune the VRR range. 

---

#### 3. 深度解析 Frame Tearing（畫面撕裂）

- **定義**： 畫面撕裂發生在顯示器刷新畫面時，GPU 恰好向幀緩衝區（Frame Buffer）寫入新的一幀。由於顯示器從上到下逐行掃描（Scanout），若掃描途中緩衝區被覆蓋，螢幕上半部顯示舊幀，下半部顯示新幀，兩者交界處產生明顯的「水平斷裂線」（Tear Line）。
- **Definition**: Frame Tearing occurs when the monitor refreshes the screen at the exact moment the GPU writes a new frame into the Frame Buffer. Because the display scans out pixels line-by-line (top to bottom), if the buffer is overwritten mid-scan, the top half shows the old frame and the bottom half shows the new frame, resulting in a visible horizontal "tear line".
- **主要成因 (Root Causes)**：
  - GPU 渲染速率與顯示器刷新率完全不同步（Asynchronous）。通常是關閉了垂直同步（V-Sync Off），或幀率遠高於/低於螢幕刷新率（例如 200fps 顯示在 60Hz 螢幕上）。
  - The GPU render rate is completely out of sync with the monitor's refresh rate. This usually happens when V-Sync is turned off, or when the FPS is significantly higher/lower than the refresh rate (e.g., 200fps on a 60Hz monitor).
- **視覺表現 (Visual Symptoms)**：
  - 畫面中出現一道或多道水平「切痕」，尤其在快速左右移動視角（如 FPS 遊戲甩槍）時，切痕處的物體會錯位（斷層）。
  - One or multiple horizontal "cut lines" appear across the screen. When panning the camera quickly (e.g., flick-shotting in FPS games), objects at the tear line appear disjointed or sheared.
- **解決方案 (Solutions)**：
  - 開啟垂直同步（V-Sync）強制同步，但會增加輸入延遲。最佳解法是使用 VRR 技術（G-Sync / FreeSync），讓螢幕刷新率動態匹配 GPU 輸出幀率。
  - Turn on V-Sync to force synchronization, but this introduces input lag. The best solution is using VRR technology (G-Sync / FreeSync), which dynamically matches the monitor's refresh rate to the GPU's output.

- --

#### 4. 核心對比表 (Core Comparison Table)

| 維度 Dimension | 中文解釋 (Jitter)            | 英文 (Jitter)                            | 中文解釋 (Tearing)          | 英文 (Tearing)                      |
| ------------ | ------------------------ | -------------------------------------- | ----------------------- | --------------------------------- |
| **根本屬性**         | 時間流暢度問題                  | Temporal (Time) issue                  | 空間圖像問題                  | Spatial (Image) issue             |
| **直觀感受**         | 卡頓、抖動、不連貫                | Stutter, Hitching                      | 水平斷裂、畫面錯位               | Horizontal split, Dislocation     |
| **幀率狀態**         | 幀生成時間不穩定（幀率波動）           | Unstable Frame Time (FPS Fluctuation)  | 幀率與刷新率倍數不匹配             | Mismatch between FPS and Hz ratio |
| **判定方式**         | 查看 Frametime 曲線（是否筆直）    | Check Frametime graph (should be flat) | 查看畫面中是否有撕裂線             | Check for visible tear lines      |
| **終極剋星**         | Reflex / Anti-Lag + 幀數限制 | Reflex / Anti-Lag + Frame Capping      | G-Sync / FreeSync (VRR) | G-Sync / FreeSync (VRR)           |

---

#### 5. 進階知識：兩者的關聯性 (Advanced: The Correlation)

- 很多人誤以為關閉 V-Sync 只會造成 Tearing，但實際上，當 V-Sync 開啟且效能不足時，GPU 會強制重複顯示同一幀（即幀數腰斬，如 60fps 掉到 30fps），這時反而會**加劇 Jitter（嚴重卡頓）**。這就是為什麼現代遊戲推薦使用 **VRR + 遊戲內限幀（略低於螢幕最高刷新率）**，既能消除 Tearing，又能最小化 Jitter 和輸入延遲。
- Many assume turning off V-Sync only causes Tearing. However, when V-Sync is ON but performance is insufficient, the GPU forcibly repeats frames (halving FPS, e.g., 60fps to 30fps), which actually **exacerbates Jitter (severe stutter)**. This is why modern gaming recommends **VRR + In-game FPS cap (slightly below the max refresh rate)**, which eliminates Tearing while minimizing both Jitter and input lag. 

---

#### 6. 終極調試清單 (Ultimate Debugging Checklist)

遇到視覺異常時，請依序檢查：

1. **檢查 Frametime 曲線**（使用 MSI Afterburner）：曲線劇烈波動 → **Jitter** 問題。請降低畫質或鎖幀。
2. **檢查畫面撕裂**：快速轉動視角，看是否有水平切痕 → **Tearing 問題**。請開啟 G-Sync/FreeSync 或 V-Sync。
3. **混合情況**：若開啟 V-Sync 後依然「黏滯」且撕裂消失，但卡頓明顯，請關閉 V-Sync，改為開啟 **VRR**，並將幀數上限設為 刷新率 - 3（例如 144Hz 螢幕設為 141fps）。


您說得對，第六段「終極調試清單」只寫了中文，漏掉了英文翻譯。非常抱歉！現補上完整的中英雙語版本如下：

---

6. 終極調試清單 (Ultimate Debugging Checklist)

遇到視覺異常時，請依序檢查： <br>
When encountering visual anomalies, follow these steps in order:

1. **檢查 Frametime 曲線**（使用 MSI Afterburner）：曲線劇烈波動 → **Jitter** 問題。請降低畫質或鎖幀。 <br>
   **Check the Frametime graph** (using MSI Afterburner): If the graph fluctuates wildly → **Jitter** issue. Lower graphics settings or cap the frame rate.
2. **檢查畫面撕裂**：快速轉動視角，看是否有水平切痕 → **Tearing** 問題。請開啟 G-Sync/FreeSync 或 V-Sync。 <br>
   **Check for screen tearing**: Quickly pan the camera and look for horizontal cut lines → **Tearing** issue. Enable G-Sync/FreeSync or V-Sync.
3. **混合情況**：若開啟 V-Sync 後依然「黏滯」且撕裂消失，但卡頓明顯，請關閉 V-Sync，改為開啟 **VRR**，並將幀數上限設為 刷新率 - 3（例如 144Hz 螢幕設為 141fps）。 <br>
   **Mixed scenario**: If enabling V-Sync removes tearing but still feels "sluggish" with obvious stutter, turn off V-Sync, enable **VRR** instead, and set an FPS cap to (Refresh Rate - 3), e.g., 141 fps for a 144Hz monitor.



