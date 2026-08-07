這是一份關於 像素合併 (Pixel Binning) 的完整中英雙語技術詳解。內容涵蓋定義、運作原理、硬體 vs. 軟體實現、優缺點、進階架構（如 Tetra-cell / Nona-cell）以及實際應用場景。

---

### 1. 定義與核心概念 (Definition & Core Concept)

像素合併是一種數位影像處理技術，將感光元件（CMOS / CCD）上相鄰的數個像素單元結合成一個較大的「超級像素」（Super Pixel）來運作。最常見的模式是合併 2 \times 2 的陣列（共 4 個像素），或更高階的 3 \times 3 陣列（共 9 個像素）。其核心目的是犧牲解析度來換取更高的感光靈敏度與更低的影像雜訊。

Pixel Binning is a digital image processing technique that combines several adjacent pixel units on an image sensor (CMOS / CCD) into a single, larger "super pixel." The most common configurations are 2 \times 2 arrays (combining 4 pixels) or higher-end 3 \times 3 arrays (combining 9 pixels). The core objective is to sacrifice resolution in exchange for higher light sensitivity and lower image noise.

---

### 2. 運作原理與實現方式 (Working Principles & Implementation)

像素合併的實現可分為兩大類，其效果有本質上的差異：

- 類比合併（Analog Binning / 電荷域合併）：在讀出電路之前，直接將相鄰像素的電荷（電子）在浮動擴散節點（Floating Diffusion）中進行物理性相加。這種方式能從源頭降低讀出噪聲（Read Noise），信噪比（SNR）提升最為顯著。
- 數位合併（Digital Binning / 數位域合併）：先將每個像素的類比訊號獨立轉換為數位訊號（ADC），然後在影像處理器（ISP）中進行軟體層級的相加平均。這種方式雖然能降低隨機噪聲，但無法抑制讀出噪聲，效果略遜於類比合併。

The implementation of Pixel Binning falls into two categories, which differ substantially in effectiveness:

- Analog Binning (Charge-domain)：Physical addition of charges (electrons) from adjacent pixels at the Floating Diffusion node before the readout circuit. This method fundamentally reduces read noise, resulting in the most significant Signal-to-Noise Ratio (SNR) improvement.
- Digital Binning (Digital-domain)：Each pixel's analog signal is independently converted to a digital signal (ADC) first, then averaged via software in the Image Signal Processor (ISP). While this reduces random noise, it cannot suppress read noise, making it less effective than analog binning.

---

### 3. 主要優勢 (Key Advantages)

- 卓越的低光表現：合併後像素面積變大（例如 2 \times 2 相當於感光面積變為 4 倍），捕獲光子數量劇增，使夜間拍攝畫面更明亮。
- 顯著降低雜訊：隨機雜訊（光子散粒雜訊）在訊號平均過程中互相抵銷，畫面純淨度大幅提高。
- 增加動態範圍：合併後的「大像素」滿阱容量（Full Well Capacity）相對提升，能同時保留高光與陰影細節。


- Superior Low-light Performance：The combined pixel area increases significantly (e.g., 2 \times 2 yields 4x the light-gathering area), capturing far more photons, resulting in brighter night-time images.
- Significant Noise Reduction：Random noise (photon shot noise) is canceled out during the signal averaging process, drastically improving image cleanliness.
- Enhanced Dynamic Range：The resulting "large pixel" has a relatively higher Full Well Capacity, allowing for better preservation of both highlight and shadow details.

---

### 4. 取捨與固有缺點 (Trade-offs & Inherent Drawbacks)

- 解析度驟降：這是最大的代價。將 5000 萬像素合併為 2 \times 2 僅能輸出 1250 萬像素的照片，大幅限制了大型輸出或裁切重構的彈性。
- 失去細節紋理：合併過程中的平均演算法會抹除微小的紋理（如皮膚毛孔、草葉脈絡），導致影像呈現「油畫感」。
- 色彩偽影風險：若合併邏輯未正確匹配拜爾濾色鏡（Bayer Filter）的 RGB 排列，可能導致顏色錯誤或摩爾紋。


- Drastic Resolution Loss：This is the biggest cost. Merging a 50MP sensor via 2 \times 2 binning only outputs a 12.5MP image, severely limiting large-format prints or cropping flexibility.
- Loss of Fine Texture：The averaging algorithm smoothens out fine textures (e.g., skin pores, leaf veins), often resulting in a "watercolor painting" effect.
- Color Artifact Risk：If the binning logic doesn't perfectly match the Bayer Filter's RGB arrangement, it may cause color inaccuracies or moiré patterns.

---

### 5. 進階架構與現代演變 (Advanced Architectures & Modern Evolution)

現代智慧型手機感光元件（如 Samsung ISOCELL 系列）採用可重排式合併：

- Tetra-cell / Quad Bayer (2x2)：允許在「高解析度模式」與「低光四合一模式」間切換。
- Nona-cell (3x3，如 108MP / 200MP 感光元件)：將 9 個像素合併為 1 個，輸出 12MP 或 24MP。此外，現代 ISP 導入 Re-mosaic 演算法，能在合併輸出後，透過 AI 深度學習重建遺失的高頻細節，試圖在亮度和細節間取得完美平衡。同時，Dual Conversion Gain (DCG) 技術常與之協同，在高光下使用低轉換增益避免過曝，在暗部使用高轉換增益提升訊號。


Modern smartphone sensors (e.g., Samsung ISOCELL series) utilize Remosaic-reconfigurable binning：

- Tetra-cell / Quad Bayer (2x2)：Allows switching between "High-Resolution mode" and "Low-light 4-in-1 mode."
- Nona-cell (3x3, e.g., 108MP / 200MP sensors)：Combines 9 pixels into 1, outputting 12MP or 24MP. Furthermore, modern ISPs introduce Re-mosaic algorithms that leverage AI deep learning to reconstruct lost high-frequency details after binning, aiming for an optimal balance between brightness and detail. Additionally, Dual Conversion Gain (DCG) often works in tandem—using low conversion gain in highlights to avoid clipping and high conversion gain in shadows to boost signals.

---

### 6. 輸出策略：何時使用？ (Output Strategy: When to use it?)

- 建議開啟（合併模式）：極低光源（夜間）、室內昏暗環境、錄製 4K 或 FHD 影片（此時解析度超過所需，利用合併可獲得極為純淨的動態畫面）。
- 建議關閉（全像素模式）：光線充足的戶外、拍攝風景或建築（需要細節裁切）、商業攝影（需要後製精修紋理）。


- Enable (Binning Mode)：Extremely low light (night time), dim indoor environments, or when recording 4K/FHD videos (where resolution exceeds the requirement, binning yields exceptionally clean motion footage).
- Disable (Full Resolution Mode)：Well-lit outdoor scenes, landscape or architectural photography (requiring cropping flexibility), or commercial work needing fine texture retention for post-processing.

---

### 7. 結論與未來趨勢 (Conclusion & Future Trends)

像素合併並非萬能藥，而是一種權衡策略。在物理定律（光圈大小受限）無法突破的手機領域，它是解決低光拍攝痛點的關鍵技術。未來趨勢朝向 「智慧型可變合併」 發展——不再只是固定的 2x2 或 3x3，而是依據場景物體動態調整合併權重（如 Sony 的「Intelligent Binning」），並結合 計算攝影（Computational Photography），透過多幀融合進一步彌補單次合併的細節損失。

Pixel binning is not a panacea but a strategic trade-off. In the smartphone domain, where physical laws (aperture size limitations) cannot be easily overcome, it remains a critical solution to low-light pain points. The future trend points toward "Intelligent Variable Binning"—moving away from fixed 2x2 or 3x3 grids toward dynamically adjusting binning weights based on scene motion (e.g., Sony's "Intelligent Binning"), combined with Computational Photography and multi-frame fusion to further compensate for the single-shot detail loss.