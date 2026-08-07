## 图像传感器读出全面解析

## Comprehensive Guide to Image Sensor Readout

---

### 1. 引言：什麼是影像感測器讀出？<br><br>&nbsp;&nbsp;&nbsp;&nbsp;Introduction: What is Image Sensor Readout?

#### 中文
影像感測器讀出（Image Sensor Readout）是指將感測器畫素陣列中所累積的光生電荷（類比信號）依序提取、放大，並轉換為數位影像資料的完整過程。此過程是相機、手機、監控及自駕車等成像系統的「心臟」，直接主宰了最終影像的**幀率（Frame Rate）**、**雜訊表現（Noise Performance）**、**動態範圍（Dynamic Range）** 與**功耗（Power Consumption）**。

#### English
Image sensor readout is the complete process of sequentially extracting, amplifying, and converting the photogenerated charges (analog signals) accumulated in the pixel array into digital image data. This process serves as the "heart" of all imaging systems—including cameras, smartphones, surveillance, and autonomous vehicles—directly dictating the final image's **frame rate, noise performance, dynamic range, and power consumption**.

---

### 2. 核心讀出流程（由類比到數位）<br><br>&nbsp;&nbsp;&nbsp;&nbsp;Core Readout Workflow (Analog to Digital) ###

#### 中文
典型的讀出管線可分為以下六個關鍵步驟：

1. **重設（Reset）**：讀出前先清除畫素內浮動擴散節點（FD）的殘留電荷，以定義一個乾淨的基準位準。<br>
2. **積分 / 曝光（Integration / Exposure）**：入射光子被光電二極體吸收，轉換為電子-電洞對，電荷存儲於光電二極體的勢阱中。<br>
3. **電荷轉移（Charge Transfer）**：透過啟動傳輸閘（Transfer Gate, TX），將光電二極體中的信號電荷完全轉移至浮動擴散節點（FD），將電荷轉換為電壓（轉換增益由FD電容決定）。<br>
4. **放大與採樣（Amplification & Sampling）**：FD節點的電壓經由畫素內源極隨耦器（Source Follower, SF）進行緩衝與放大，隨後行級（Column）類比電路對信號進行採樣。此時通常會執行**相關雙重取樣（CDS）**，分別採樣「重設位準」與「信號位準」以消除低頻雜訊。<br>
5. **類比數位轉換（ADC）**：行平行或晶片內建之類比數位轉換器將類比電壓轉換為數位碼（通常為10~14-bit RAW資料）。<br>
6. **數位輸出與處理（Digital Output & ISP）**：數位資料經由MIPI、LVDS等高速介面輸出，並送入影像信號處理器（ISP）進行黑位校正、白平衡、去馬賽克等後處理。<br>

#### English
A typical readout pipeline consists of the following six critical steps:

1. **Reset**: Before readout, residual charges in the Floating Diffusion (FD) node are cleared to define a clean reference level. <br>
2. **Integration / Exposure**: Incident photons are absorbed by the photodiode, generating electron-hole pairs. The charges are stored in the potential well of the photodiode.<br>
3. **Charge Transfer**: The Transfer Gate (TX) is activated to completely move the signal charges from the photodiode to the FD node, converting charge into voltage (conversion gain is determined by the FD capacitance).<br>
4. **Amplification & Sampling**: The voltage at the FD node is buffered and amplified by the in-pixel Source Follower (SF). Column-level analog circuits then sample the signal. At this stage, **Correlated Double Sampling (CDS)** is typically performed—sampling both the "reset level" and the "signal level" to eliminate low-frequency noise.<br>
5. **Analog-to-Digital Conversion (ADC)**: Column-parallel or on-chip ADCs convert the analog voltage into digital codes (typically 10~14-bit RAW data).<br>
6. **Digital Output & ISP**: The digital data is transmitted via high-speed interfaces (e.g., MIPI, LVDS) and fed into the Image Signal Processor (ISP) for further processing, including black level correction, white balance, and demosaicing.<br>

---

### 3. 讀出架構分類：滾動式 vs. 全域式快門<br><br>&nbsp;&nbsp;&nbsp;&nbsp;Readout Architectures: Rolling Shutter vs. Global Shutter

#### 中文
依據畫素曝光與讀出的時序差異，主流架構分為兩種：

| 特性   | 滾動式快門 (Rolling Shutter)                  | 全域式快門 (Global Shutter)                           |
| ---- | ---------------------------------------- | ------------------------------------------------ |
| **運作原理** | 畫素陣列逐行（Row-by-row）依序曝光與讀出，每行之間存在微小的時間延遲。 | 所有畫素同時開始與結束曝光，隨後將電荷存入畫素內記憶體（儲存節點），再統一進行讀出。       |
| **優點**   | 畫素結構簡單，填充因子（Fill Factor）高，成本低廉，暗電流低。     | 無果凍效應（Jello Effect），可完美捕捉高速移動物體，適合閃光燈同步。         |
| **缺點**   | 拍攝高速物體或震動時會產生扭曲變形（果凍效應），且不適合搭配閃光燈。       | 畫素內需增加儲存電容與開關，填充因子下降，雜訊較高（尤其kTC雜訊），且製造成本與功耗顯著提升。 |
| **應用場景** | 消費級手機、一般監控、多數數位相機。                       | 工業檢測、自動駕駛（車載攝影機）、AR/VR眼球追蹤、高速攝影。                 |

#### English
Based on the timing differences between pixel exposure and readout, there are two mainstream architectures:

| Feature       | Rolling Shutter                                                                                                                   | Global Shutter                                                                                                                                                           |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Principle**     | Pixels are exposed and read out row by row sequentially, with a slight time delay between each row.                               | All pixels start and end exposure simultaneously. The charges are then stored in in-pixel memory (storage nodes) and read out uniformly.                                 |
| **Advantages**    | Simple pixel structure, high Fill Factor (FF), low cost, and low dark current.                                                    | No Jello Effect, perfectly captures fast-moving objects, and supports flash synchronization.                                                                             |
| **Disadvantages** | Causes motion distortion (Jello Effect) when capturing high-speed objects or under vibration; not suitable for flash photography. | Requires additional storage capacitors and switches inside the pixel, reducing FF; higher noise (especially kTC noise); significantly higher cost and power consumption. |
| **Applications**  | Consumer smartphones, general surveillance, most DSLR/mirrorless cameras.                                                         | Industrial inspection, autonomous driving (automotive cameras), AR/VR eye tracking, high-speed photography.                                                              |

---

### 4. 關鍵進階讀出技術<br><br>&nbsp;&nbsp;&nbsp;&nbsp;Key Advanced Readout Techniques

#### 中文
為了提升影像品質，現代感測器整合了以下讀出技術：

(1) 相關雙重取樣（CDS – Correlated Double Sampling）

· **原理**：對同一個畫素的「重設電壓」與「信號電壓」進行兩次採樣並相減。<br>
· **效果**：有效消除低頻雜訊（如1/f雜訊、熱雜訊）及源極隨耦器的臨界電壓變異，大幅降低固定圖樣雜訊（FPN）。

(2) 高動態範圍讀出（HDR Readout）

· **多曝光合成（Multi-Exposure）**：依次讀出長、短不同曝光時間的畫素資料，於數位域合成為高動態範圍影像。<br>
· **雙轉換增益（DCG – Dual Conversion Gain）**：透過切換FD電容，於同一幀內讀出低增益（高容量）與高增益（低容量）信號，無需延遲即可擴展動態範圍（常見於Sony STARVIS系列）。

(3) 像素合併（Pixel Binning）

· **原理**：將相鄰的2x2或4x4畫素的電荷在讀出前進行類比合併（或數位相加）。<br>
· **效果**：提升信噪比（SNR）達3~6dB，同時有效降低解析度以換取更快的讀出幀率（多用於低光預覽或4K錄影）。

(4) 子採樣 / 跳行讀出（Sub-sampling / Skipping）

· **原理**：僅讀出陣列中特定的行或列，跳過其他畫素。<br>
· **效果**：大幅提升幀率，但易產生摩爾紋（Aliasing），現多被「像素合併」取代。

#### English
To enhance image quality, modern sensors integrate the following readout techniques:

(1) Correlated Double Sampling (CDS)

· **Principle**: Performs two samples on the same pixel—the "reset voltage" and the "signal voltage"—and subtracts them.<br>
· **Effect**: Effectively eliminates low-frequency noise (e.g., 1/f noise, thermal noise) and threshold variations of the Source Follower, significantly reducing Fixed Pattern Noise (FPN).

(2) HDR Readout (High Dynamic Range)

· **Multi-Exposure Synthesis**: Sequentially reads out long and short exposure pixel data, combining them in the digital domain to produce an HDR image.<br>
· **Dual Conversion Gain (DCG)**: Switches the FD capacitance to read out both low-gain (high capacity) and high-gain (low capacity) signals within the same frame, extending dynamic range without temporal lag (commonly found in Sony STARVIS series).

(3) Pixel Binning

· **Principle**: Combines the charges of adjacent pixels (e.g., 2x2 or 4x4) either in the analog domain (before ADC) or digitally (after ADC).<br>
· **Effect**: Boosts the Signal-to-Noise Ratio (SNR) by 3~6dB while effectively reducing resolution to achieve faster readout frame rates (widely used in low-light previews or 4K video recording).

(4) Sub-sampling / Skipping

· **Principle**: Reads out only specific rows and columns from the array, skipping the rest.<br>
· **Effect**: Dramatically increases frame rate but is prone to aliasing (Moiré patterns). It is now largely replaced by pixel binning.

---

### 5. 讀出速度的瓶頸與多通道架構<br><br>&nbsp;&nbsp;&nbsp;&nbsp;Readout Speed Bottlenecks and Multi-Channel Architectures

#### 中文
隨著解析度邁向8K（33MP）甚至1億畫素以上，讀出速度面臨嚴峻挑戰。傳統單通道（Single-channel）架構已無法應付，現今高階感測器採用：

· **多通道行平行ADC（Column-Parallel ADC）**：每一行（或每兩行）配備專屬ADC，所有行同時進行數位轉換，將讀出時間縮短為單一畫素延遲乘以行數，而非全部畫素總數。<br>
· **多斜率ADC（Multi-Slope ADC）**：動態調整轉換斜率，在暗部使用細步階（高解析度）、亮部使用粗步階（高速），兼顧速度與精度。<br>
· **3D堆疊（3D-Stacking）**：將畫素陣列（上層晶片）與邏輯/記憶體/ADC（下層晶片）透過TSV或Cu-Cu混鍵合分離。此架構允許超寬匯流排（例如Sony IMX系列達數千個ADC平行運作），將讀出幀率推升至120fps~480fps以上。

#### English
As resolutions climb toward 8K (33MP) and even 100MP+, readout speed faces severe challenges. Traditional single-channel architectures are no longer sufficient. Today's high-end sensors employ:

· **Column-Parallel ADC**: Equips each column (or every two columns) with its own dedicated ADC. All columns perform digital conversion simultaneously, reducing readout time to the single-pixel delay multiplied by the number of rows, <br>
· **Multi-Slope ADC**: Dynamically adjusts the conversion slope—using fine steps (high resolution) for dark areas and coarse steps (high speed) for bright areas—balancing both speed and precision.<br>
· **3D-Stacking**: Separates the pixel array (top die) from the logic/memory/ADC (bottom die) via TSVs or Cu-Cu hybrid bonding. This architecture enables ultra-wide bus widths (e.g., thousands of ADCs operating in parallel in Sony IMX series), pushing readout frame rates beyond 120fps to 480fps+.

---

### 6. 讀出雜訊的來源與抑制<br><br>&nbsp;&nbsp;&nbsp;&nbsp;Sources of Readout Noise and Suppression

#### 中文
讀出階段是影像雜訊的主要貢獻來源，包含：

| 雜訊類型                     | 成因                           | 抑制方法                                     |
| ------------------------ | ---------------------------- | ---------------------------------------- |
| **kTC雜訊（重設雜訊）**              | 重設FD節點時，熱擾動導致電荷隨機增減。         | 採用CDS（相關雙重取樣）完美消除。                       |
| **1/f 閃爍雜訊**                 | 半導體表面缺陷造成源極隨耦器電流波動。          | 採用較大通道尺寸的電晶體或雙重取樣技術。                     |
| **行固定圖樣雜訊（Column FPN）**      | 各行ADC或放大器偏移量不均。              | 晶片出廠前進行行校正（Column Calibration）儲存於OTP記憶體。 |
| **量化雜訊（Quantization Noise）** | ADC位元數不足（如10-bit vs 12-bit）。 | 提高ADC解析度或應用抖動（Dithering）技術。              |

#### English
The readout stage is a primary contributor to image noise, including:

| Noise Type              | Cause                                                                                | Suppression Method                                                      |
| ----------------------- | ------------------------------------------------------------------------------------ | ----------------------------------------------------------------------- |
| **kTC Noise (Reset Noise)**| Thermal fluctuations cause random charge variations when resetting the FD node.      | Eliminated perfectly by employing CDS.                                  |
| **1/f Flicker Noise**       | Current fluctuations in the Source Follower caused by semiconductor surface defects. | Use of larger channel-size transistors or double-sampling techniques.   |
| **Column FPN**              | Non-uniform offsets in ADC or amplifiers across different columns.                   | Factory column calibration with correction values stored in OTP memory. |
| **Quantization Noise**      | Insufficient ADC bit depth (e.g., 10-bit vs. 12-bit).                                | Increase ADC resolution or apply dithering techniques.                  |

---

### 7. 功耗管理與讀出策略<br><br>&nbsp;&nbsp;&nbsp;&nbsp;Power Management and Readout Strategies

#### 中文
在行動裝置與物聯網感測器中，讀出功耗至關重要。常見省電策略包含：

· **智慧幀率調節（Adaptive Frame Rate）**：靜態場景自動降低讀出幀率（如從30fps降至5fps）。<br>
· **區域讀出（Region-of-Interest, ROI）**：僅讀出畫面中特定區塊（如人臉偵測區域），大幅減少讀出資料量。<br>
· **動態電壓頻率調節（DVFS）**：依據讀出行數動態調整ADC與介面的時脈頻率與電壓。

#### English
In mobile devices and IoT sensors, readout power consumption is critical. Common power-saving strategies include:

· **Adaptive Frame Rate**: Automatically reduces the readout frame rate (e.g., from 30fps to 5fps) in static scenes.<br>
· **Region-of-Interest (ROI) Readout**: Reads out only specific blocks of the frame (e.g., face detection areas), drastically reducing the amount of readout data.<br>
· **Dynamic Voltage and Frequency Scaling (DVFS)**: Dynamically adjusts the clock frequency and voltage of the ADC and interface based on the number of rows being read.

---

### 8. 總結與未來趨勢<br><br>&nbsp;&nbsp;&nbsp;&nbsp;Conclusion and Future Trends

#### 中文
影像感測器讀出技術正朝「高速化」、「高動態」、「低雜訊」與「低功耗」四個極致方向演進。隨著堆疊製程（Stacked CMOS）、事件驅動型感測器（Event-based Vision Sensor, EVS）與智慧畫素內運算（In-pixel Computing）的崛起，未來的讀出將不再只是被動的資料傳輸，而是融合「感測－記憶－運算」一體化的智慧讀出，為邊緣AI視覺開啟全新紀元。

#### English
Image sensor readout technology is evolving toward four ultimate directions: **Higher Speed**, **Higher Dynamic Range**, **Lower Noise**, and **Lower Power**. With the rise of stacked CMOS processes, Event-based Vision Sensors (EVS), and in-pixel computing, future readout will no longer be a passive data transfer but an intelligent readout integrating "Sensing-Memory-Computing" into one unit, opening a new era for edge-AI computer vision.
