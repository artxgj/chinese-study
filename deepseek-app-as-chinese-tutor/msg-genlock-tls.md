## Master Sync Generator (MSG) 详解：與 Genlock 及 Tri-Level Sync 的關係

## Master Sync Generator (MSG) Explained: Relationship with Genlock and Tri-Level Sync

---

### 一、Master Sync Generator (MSG) 主同步產生器

#### 中文說明

**主同步產生器（Master Sync Generator，簡稱 MSG）** 是廣播電視與專業影音系統中的「心跳」與「時間心臟」。它是一個超高精度的時鐘源，負責產生並分配標準的參考同步信號給系統中的所有設備。

在一個典型的廣播攝影棚或後期製作設施中，所有設備——包括攝影機、視訊切換台、LED 處理器、媒體伺服器——都依賴 MSG 提供的同步信號來協調運作。MSG 可以鎖定到外部參考源（如 GPS 信號、另一個 MSG，或從其他攝影棚恢復的主同步時鐘），透過數位鎖相迴路（DPLL）清除相位雜訊，然後合成所需的時鐘頻率並將其嵌入標準視訊信號中，再透過 BNC 電纜分配給所有設備。

MSG 的關鍵功能包括：

&bull; 同步到不同的輸入頻率（GPS 的 1Hz 信號到各種視訊幀率）

&bull; 無縫參考源切換

&bull; 保持模式（Holdover Mode）——參考源失效時仍能維持穩定輸出

&bull; 可編程 DPLL 迴路頻寬以過濾抖動

&bull; 產生黑場（Black Burst）複合視訊信號與 HD 三電平同步（Tri-Level Sync）輸出

&bull; 可調整各輸出的延遲或提前量，以補償配線延遲差異

以 **Miranda MSG-5300HD** 為例，它是一個模組化多格式主同步產生器，基本單元提供 2 個黑場輸出和 4 個三電平同步輸出（最多可擴充至 8 個）。每個輸出都可以獨立設定時序，解析度高達 6.7ns。

---

#### English Explanation

A **Master Sync Generator (MSG)** is the "heartbeat" and "timekeeper" of a broadcast television or professional AV system. It is an ultra-precise clock source responsible for generating and distributing standard reference synchronization signals to all equipment in the facility.

In a typical broadcast studio or post-production facility, all devices—including cameras, video switchers, LED processors, and media servers—rely on the MSG's sync signals to coordinate their operation. The MSG can lock to external reference sources (such as GPS signals, another MSG, or recovered master sync clocks from other studios), clean the selected reference from phase noise using a Digital Phase Locked Loop (DPLL), synthesize the required clock frequencies, embed them into standard video synchronization signals, and distribute them to all equipment via BNC cables.

Key features of an MSG include:

&bull; Ability to synchronize to different input frequencies (from GPS 1Hz signals to various video frame rates)

&bull; Seamless reference switching

&bull; Holdover mode—maintains stable output even when the reference fails

&bull; Programmable DPLL loop bandwidth to filter jitter

&bull; Simultaneous generation of integer and non-integer frame rate sync signals

&bull; Generation of black burst composite video signals and HD tri-level sync outputs

&bull; Adjustable delay/advance of each sync output to compensate for cable delay differences

Take the **Miranda MSG-5300HD** as an example: it is a modular multi-format master sync generator. The base unit provides 2 black burst outputs and 4 tri-level sync outputs (expandable up to 8). Each output can be independently timed with a resolution as fine as 6.7ns.

---

### 二、Genlock（同步鎖相／ Generator Locking）

#### 中文說明

**Genlock（Generator Locking，同步鎖相）** 是一種讓多個視訊來源同步於單一參考源或參考信號的技術。其核心目標是確保多個輸入之間具有一致的時序，從而實現無縫的視訊切換。

Genlock 的工作原理是：透過一個參考信號（Reference Signal），使所有設備「鎖定」到統一的時間基準，從而保證影像幀與掃描信號的一致性，防止畫面抖動、延遲或相位漂移。

在廣播系統中，Genlock 參考信號通常以 **黑場信號（Black Burst）** 的形式傳送——這是一個不攜帶圖像資訊的複合視訊信號，只包含垂直同步、水平同步和色同步（Color Burst）分量。

當所有設備都同步到同一個 MSG 時，這些設備就被稱為「**Genlocked**」（同步鎖相）。在這種狀態下，視訊切換台可以在不同信號源之間切換而不會出現畫面撕裂、滾動條或不穩定的切換延遲。

---

#### English Explanation

**Genlock (Generator Locking)** is a technique for synchronizing multiple video sources based on a single reference source or reference signal. Its core goal is to ensure consistent timing across multiple inputs, enabling seamless video switching.

The working principle of genlock is: through a reference signal, all devices are "locked" to a unified time base, ensuring consistency between video frames and scan signals, preventing picture抖动, delay, or phase drift.

In broadcast systems, the genlock reference signal is typically delivered as a **Black Burst** signal—a composite video signal that carries no picture information, containing only vertical sync, horizontal sync, and color burst components.

When all devices are synchronized to the same MSG, they are said to be "**Genlocked**". In this state, a video switcher can cut between different sources without frame tearing, roll bars, or unstable switching delays.

---

### 三、Tri-Level Sync（三電平同步）

#### 中文說明

**三電平同步（Tri-Level Sync）** 是一種模擬視訊同步脈衝，主要用於鎖定高畫質（HD）視訊信號。它由 SMPTE（電影與電視工程師協會）在 SMPTE 240 模擬 HDTV 標準中引入。

傳統的 **雙電平同步（Bi-Level Sync）** ——也就是黑場信號所使用的格式——有兩個電平：同步脈衝頂端（Sync Tip）和消隱電平（Blanking Level）。這種方式會將不希望有的 DC 分量引入視訊信號中。

三電平同步則解決了這個問題：

&bull; 它與雙電平同步具有相同的消隱電平（位於 0V/接地）

&bull; 但同步脈衝同時包含**負向**和**正向**兩個元素

&bull; 信號從 0V 開始 → 下降到 -300mV → 上升到 +300mV → 回到 0V

&bull; 這使得信號保持**對稱平衡**，DC 分量為零

&bull; 時序擷取點位於信號**跨越消隱電平的零點**，不再受振幅變化的影響

這使得三電平同步成為一個**更穩健（robust）** 的信號，抖動更小，非常適合高資料速率和嚴格抖動要求的 HD 系統。

在 HD 系統中，三電平同步已取代黑場成為首選的 Genlock 參考信號。SD 系統使用黑場（雙電平），HD 系統使用三電平同步。

---

#### English Explanation

**Tri-Level Sync** is an analog video synchronization pulse primarily used for locking high-definition (HD) video signals. It was introduced by SMPTE (Society of Motion Picture and Television Engineers) in the SMPTE 240 analog HDTV standard.

Traditional **Bi-Level Sync**—the format used by black burst signals—has two levels: sync tip and blanking level. This approach introduces unwanted DC components into the video signal.

Tri-Level Sync solves this problem:

&bull; It has the same blanking level as bi-level sync (at ground/0V)

&bull; But the sync pulse contains **both a negative and a positive** element

&bull; The signal goes from 0V → drops to -300mV → rises to +300mV → returns to 0V

&bull; This keeps the signal **symmetrically balanced**, resulting in zero DC content

&bull; The timing pickoff point is at the **zero-crossing point** where the signal crosses blanking, no longer subject to amplitude variation

This makes Tri-Level Sync a **much more robust signal** with less jitter, ideal for high data rate and tight jitter requirement HD systems.

In HD systems, Tri-Level Sync has replaced black burst as the preferred genlock reference signal. SD systems use black burst (bi-level), while HD systems use tri-level sync.

---

### 四、三者關係總覽 │ The Relationship Overview

| 概念 | Concept | 說明 | Explanation |
| :--- | :--- | :--- | :--- |
| **MSG（主同步產生器）** | MSG | 系統中的「時間心臟」，產生並分配參考信號 | The "timekeeper" of the system—generates and distributes reference signals |
| **Genlock（同步鎖相）** | Genlock | 一種「機制／技術」——讓所有設備鎖定到 MSG 的參考信號 | A "mechanism/technique"—locks all devices to the MSG's reference signal |
| **Tri-Level Sync（三電平同步）** | Tri-Level Sync | 一種「信號格式」——MSG 用來傳送 HD 參考信號的具體波形 | A "signal format"—the specific waveform used by MSG to deliver HD reference signals |

**簡單來說：**

**MSG 是「產生者」**（產生參考信號）→ **Tri-Level Sync 是「語言」**（HD 系統使用的信號格式）→ **Genlock 是「動作」**（設備鎖定到該信號的過程）

**In simple terms:**

**MSG is the "producer"** (generates reference signals) → **Tri-Level Sync is the "language"** (the signal format used in HD systems) → **Genlock is the "action"** (the process of devices locking to that signal)

---

### 五、信號波形草圖 │ Signal Waveform Sketches

#### 草圖 1：Bi-Level Sync（雙電平同步／黑場 Black Burst）

```
電壓
  ↑
  │                                    ┌──────────────┐
  │                                    │              │
  │    ┌──────────────────────────────┐│   視訊區間    │
  │    │         視訊區間             ││   (Active    │
 0V ├────┼──────────────────────────────┼│   Video)     │
  │    │                              ││              │
  │    │                              │└──────────────┘
  │    │                              │
  │    │         ┌────┐               │
  │    │         │    │               │
-300mV├────┼─────────┼────┼───────────────┼──────────────────→ 時間
  │    │         │    │               │
  │    │         └────┘               │
  │    │     同步脈衝頂端              │
  │    │     (Sync Tip)               │
  │    │                              │
  └────┴──────────────────────────────┴──────────────────
       
        Bi-Level Sync = 兩個電平：0V（消隱）和 -300mV（同步脈衝頂端）
        Bi-Level Sync = Two levels: 0V (blanking) and -300mV (sync tip)
```

---

#### 草圖 2：Tri-Level Sync（三電平同步）

```
電壓
  ↑
+300mV ─────┼──────────────┐
  │          │              │  ┌──────┐
  │          │              │  │      │
  │          │              │  │      │
 0V ────────┼──────────────┼──┼──────┼───────────────→ 時間
  │          │              │  │      │
  │          │              │  │      │
  │          │              │  └──────┘
-300mV ─────┼──────────────┘
  │          │
  │          │
  │    ┌─────┘
  │    │
  └────┴─────────────────────────────────────────────

        Tri-Level Sync = 三個電平：0V → -300mV → +300mV → 0V
        零點（Zero-Crossing Point）為時序擷取點
        Tri-Level Sync = Three levels: 0V → -300mV → +300mV → 0V
        Zero-crossing point is the timing pickoff point
```

---

#### 草圖 3：Bi-Level vs Tri-Level 比較

```
Bi-Level Sync (雙電平)：
────────────────────────────────────────────────────
電壓
  │     ┌──────────────┐
  │     │              │
 0V ────┼──────────────┼─────────────────────────
  │     │              │
  │     │    ┌────┐    │
-300mV──┼────┼────┼────┼─────────────────────────
  │     │    └────┘    │
  │     └──────────────┘
  └──────────────────────────────────────────────→ 時間
        ↑ 有 DC 分量問題 (DC offset issue)


Tri-Level Sync (三電平)：
────────────────────────────────────────────────────
電壓
+300mV ─┼──────┐
  │     │      │  ┌──┐
  │     │      │  │  │
 0V ────┼──────┼──┼──┼─────────────────────────
  │     │      │  │  │               ← 零點為時序參考
  │     │      │  └──┘                 (Zero-crossing 
-300mV ─┼──────┘                         = timing ref)
  │     │
  └─────┴──────────────────────────────────────→ 時間
        ↑ 對稱平衡，DC 分量為零 (Symmetric, zero DC)
```

---

#### 草圖 4：廣播系統中的 MSG、Genlock 與 Tri-Level Sync

```
                      ┌─────────────────────────────────────┐
                      │                                     │
                      │     Master Sync Generator (MSG)     │
                      │       主同步產生器                   │
                      │                                     │
                      │  ┌──────────┐  ┌───────────────┐   │
                      │  │ Black    │  │  Tri-Level    │   │
                      │  │ Burst    │  │  Sync Output  │   │
                      │  │ 輸出     │  │  輸出 (HD)    │   │
                      │  │ (SD)     │  │               │   │
                      │  └────┬─────┘  └───────┬───────┘   │
                      │       │                │           │
                      └───────┼────────────────┼───────────┘
                              │                │
                    (SD 設備)  │        (HD 設備) │
                              │                │
                              ▼                ▼
                     ┌─────────────────────────────────────┐
                     │                                     │
                     │     各設備透過 Genlock 機制          │
                     │     鎖定到 MSG 的參考信號           │
                     │   All devices Genlocked to MSG      │
                     │                                     │
                     │  ┌─────┐  ┌─────┐  ┌──────────┐    │
                     │  │攝影機│  │切換台│  │LED處理器 │    │
                     │  │Cam. │  │Swit.│  │LED Proc. │    │
                     │  └─────┘  └─────┘  └──────────┘    │
                     │                                     │
                     └─────────────────────────────────────┘
                                    │
                                    ▼
                          ┌─────────────────┐
                          │  同步播出 / 切換  │
                          │  無畫面撕裂      │
                          │  Seamless Playout│
                          └─────────────────┘
```

---

### 六、總結 │ Summary

| 中文                                           | English                                                                                                                                     |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **MSG** 是廣播系統的「時間心臟」，產生所有設備所需的參考同步信號。            | **MSG** is the "timekeeper" of a broadcast system, generating reference sync signals for all devices.                                           |
| **Genlock** 是讓所有設備鎖定到 MSG 參考信號的機制與技術。            | **Genlock** is the mechanism and technique that locks all devices to the MSG's reference signal.                                                |
| **Tri-Level Sync** 是 HD 系統使用的同步信號格式，比傳統黑場更精確、更穩健。 | **Tri-Level Sync** is the sync signal format used in HD systems—more precise and robust than traditional black burst.                           |
| 三者共同構成專業影音系統的**同步基礎架構**，確保多設備間的無縫協作。             | Together, they form the **synchronization infrastructure** of professional AV systems, ensuring seamless collaboration across multiple devices. |

---

### 七、參考標準 │ Reference Standards

&bull; **SMPTE 274M** – 1920×1080 影像信號格式

&bull; **SMPTE 296M** – 1280×720 影像信號格式

&bull; **SMPTE 240** – 模擬 HDTV 標準（引入 Tri-Level Sync）

&bull; **ITU-R BT.1358** – 三電平同步信號規範
