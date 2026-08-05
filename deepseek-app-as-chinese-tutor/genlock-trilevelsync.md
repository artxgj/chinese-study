**Genlock**（Generator Locking，同步鎖定）是一種在影視製作與廣播系統中，將多個視訊裝置（如攝影機、導播機、圖形產生器）的視訊幀率和掃描時序鎖定至單一共同參考訊號（House Sync）的技術。其主要目的是確保切換畫面時無撕裂（glitch-free switching）與流暢的視覺混合。

**Genlock** (Generator Locking) is a technique used in broadcast and video production to synchronize the timing of multiple video sources (cameras, switchers, and graphics generators) to a single common reference signal (House Sync). Its main purpose is to ensure glitch-free switching and seamless visual mixing during transitions.

---

**Bi-level Sync（雙電平同步）**

這是一種傳統的類比同步訊號，定義了 **兩個電壓準位**（黑階與同步階層）。它通常遵循 **SMPTE 170M** 標準，主要用於 **標準畫質（SD, 480i/576i）** 及部分較低幀率的 **HD（1080p/720p, 僅限較慢時脈）** 系統。

· **特點**：每個掃描線僅有一個負向脈衝（下降沿觸發）。其同步邊緣較寬，時基誤差（jitter）較大。

· **限制**：由於高畫質（HD）的像素時脈極高，雙電平訊號無法提供足夠的時序精確度來鎖定高速的數位數據流。

**Bi-level Sync**

This is a traditional analog synchronization signal that defines **two voltage levels** (Black level and Sync level). It typically adheres to the **SMPTE 170M** standard and is primarily used for **Standard Definition (SD, 480i/576i)** and some lower-frame-rate **HD (1080p/720p)** systems.

· **Characteristics**: One negative-going pulse per scan line (falling edge trigger). It has a wider sync edge, resulting in higher timing jitter.

· **Limitation**: Because HD has a much higher pixel clock, bi-level signals cannot provide the timing precision required to lock high-speed digital data streams.

---

**Tri-level Sync（三電平同步）**

這是專為 **高畫質（HD）** 及 **超高畫質（UHD**） 設計的現代同步訊號，定義了 **三個電壓準位**（正電壓、零電位、負電壓）。它符合 **SMPTE 274M / 296M** 標準。

· **核心機制**：它使用一個 **雙相脈衝（biphasic pulse）**——先是一段負向脈衝，緊接一段正向脈衝（或反之）。接收端偵測的是 **零交越點（zero-crossing point）**，而非脈衝邊緣。

· **優勢**：零交越點的偵測不受纜線長度或訊號衰減造成的振幅變化影響，因此能提供 **極高的時序精確度 與 極低的抖動（jitter）**。這使其能輕鬆應對 1080p/60、4K 乃至 8K 的高幀率訊號。

**Tri-level Sync**

This is a modern synchronization signal designed specifically for **High Definition (HD)** and **Ultra-High Definition (UHD)** video, defining **three voltage levels** (Positive, Zero, and Negative). It complies with the **SMPTE 274M / 296M** standards.

· **Core Mechanism**: It uses a **biphasic pulse**—a negative pulse immediately followed by a positive pulse (or vice versa). The receiving device detects the zero-crossing point rather than the pulse edge.

· **Advantage**: The zero-crossing detection is immune to amplitude variations caused by cable length or signal attenuation. This provides **extremely high timing precision** and **very low jitter**, easily supporting high frame rates like 1080p/60, 4K, and even 8K.

---

**關鍵差異總結 | Key Differences Summary**

| 特性 Feature          | Bi-level Sync (雙電平)   | Tri-level Sync (三電平)                |
| ------------------- | --------------------- | ----------------------------------- |
| 電壓準位 Voltage Levels | 2 個 (黑 / 同步)          | 3 個 (正 / 零 / 負)                     |
| 同步偵測 Sync Detection | 脈衝下降沿 (Falling edge)  | 零交越點 (Zero-crossing)                |
| 抗干擾性 Immunity       | 較差 (受振幅影響)            | 優秀 (不受振幅影響)                         |
| 適用解析度 Resolution    | SD (480i/576i) 及低速 HD | HD (720p/1080i/1080p) 及 UHD (4K/8K) |
| 典型標準 Standard       | SMPTE 170M            | SMPTE 274M / 296M                   |

**結論**：Genlock 是系統架構，而 Bi-level 和 Tri-level 是其實體訊號標準。在現代高解析度製作環境中，Tri-level sync 已全面取代 Bi-level，成為精確幀同步的首選方案。

**Conclusion**: Genlock is the system architecture, while Bi-level and Tri-level are the physical signal standards. In modern high-resolution production environments, **Tri-level sync** has completely replaced Bi-level as the go-to solution for precise frame synchronization.