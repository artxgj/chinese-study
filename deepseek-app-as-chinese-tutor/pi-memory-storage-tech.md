
---

### 1. NOR & NAND (Flash Memory)

**English (Detailed)**

NOR and NAND are two types of non‑volatile flash memory.

· NOR allows byte‑level random access, so it can execute code directly in place (XIP). It has faster read speed but lower density and higher cost.

· NAND is block‑accessible (serial), offering much higher storage density and lower cost per bit, but requires a controller and error correction. It is used in SSDs, USB drives, and memory cards.

**中文（台灣）**

NOR 快閃記憶體 / NAND 快閃記憶體（簡稱 NOR / NAND）。

NOR 可隨機讀取，適合儲存韌體；NAND 為序列存取，容量大且便宜，用於儲存裝置。

**中文（大陸）**

NOR 闪存 / NAND 闪存。

NOR 支持随机读取，可执行代码；NAND 密度高、成本低，用于固态硬盘等。

---

### 2. SRAM (Static Random‑Access Memory)

**English (Detailed)**

SRAM is volatile memory that retains data as long as power is supplied, without requiring periodic refresh. It uses flip‑flop circuits to store each bit, which makes it very fast (low latency) but also large per cell and expensive. It is commonly used as CPU cache (L1/L2/L3) and in high‑speed registers.

**中文（台灣）**

靜態隨機存取記憶體（SRAM）。

不需刷新，速度極快，但單位面積大、成本高，常用於處理器快取記憶體。

**中文（大陸）**
静态随机存取存储器（SRAM）。

无需刷新，速度快，但集成度低、价格高，用于CPU缓存。

---

### 3. DRAM (Dynamic Random‑Access Memory)

**English (Detailed**

DRAM is volatile memory that stores each bit in a capacitor. Because capacitors leak charge, DRAM must be periodically refreshed (thousands of times per second) to keep data. It is much denser and cheaper than SRAM, making it the primary system memory (main RAM) in computers and phones.

**中文（台灣）**

動態隨機存取記憶體（DRAM）。

以電容儲存資料，需持續刷新，密度高且便宜，為電腦與手機的主記憶體。

**中文（大陸）**

动态随机存取存储器（DRAM）。

靠电容存储，需周期性刷新，容量大、成本低，用作系统主存。

---

## 4. SDRAM (Synchronous DRAM)

**English (Detailed)**

SDRAM is a type of DRAM that synchronizes its operations with the system bus clock. This allows it to accept new commands on every clock cycle and perform burst transfers, significantly improving performance compared to older asynchronous DRAM. All modern DRAM is based on SDRAM.

**中文（台灣）**

同步動態隨機存取記憶體（SDRAM）。

與系統時脈同步運作，可進行爆發傳輸，為現代記憶體的基礎。

**中文（大陸）**

同步动态随机存取存储器（SDRAM）。

与系统时钟同步，支持突发传输，是现代内存的基础。

---

## 5. DDR (Double Data Rate SDRAM)

**English (Detailed)**

DDR is an evolution of SDRAM that transfers data on both the rising and falling edges of the clock signal, effectively doubling the data rate without increasing the clock frequency. Generations include DDR1, DDR2, DDR3, DDR4, and DDR5, each offering higher speeds and lower voltage.

**中文（台灣）**

雙倍資料速率記憶體（DDR 記憶體）。

在時脈的上升與下降邊緣皆傳輸資料，速度倍增。常見世代為 DDR1~DDR5。

**中文（大陸）**

双倍数据率内存（DDR 内存）。

在时钟上升沿和下降沿都传输数据，速率翻倍。代际有 DDR1 至 DDR5。

---

6. LPDDR (Low Power DDR)

**English (Detailed)**

LPDDR is a variant of DDR specifically designed for mobile and battery‑powered devices. It achieves lower power consumption through reduced operating voltage, partial array refresh, and other power‑saving features. It is widely used in smartphones, tablets, and ultra‑portable laptops (e.g., LPDDR4X, LPDDR5).

**中文（台灣）**

低功耗雙倍資料速率記憶體（LPDDR 記憶體），也常稱為「行動記憶體」。

針對手機、平板等省電需求設計，電壓更低且具節能機制。

**中文（大陸）**

低功耗双倍数据率内存（LPDDR 内存），也常稱「移动内存」。

為移動設備優化，功耗低，用於智能手機、輕薄筆電等。

---

## 7. ECC (Error‑Correcting Code) Memory

**English (Detailed)**

ECC memory includes extra data bits (usually 8 extra bits per 64‑bit word) that allow the system to detect and correct single‑bit errors and detect multi‑bit errors. This prevents data corruption from cosmic rays or electrical noise. It is essential for servers, workstations, and mission‑critical systems where stability is paramount.

**中文（台灣）**

錯誤糾正碼記憶體（ECC 記憶體）。

內含額外校驗位，可修正單一位元錯誤並偵測多位元錯誤，提升系統穩定性，常用於伺服器與工作站。

**中文（大陸）**

纠错码内存（ECC 内存）。

包含额外校验位，能纠正单比特错误并检测多比特错误，用于服务器等关键场合。

---

## 8. HBM (High Bandwidth Memory)

**English (Detailed)**

HBM is a high‑performance 3D‑stacked DRAM architecture that places memory dies vertically and connects them through tiny silicon vias (TSVs). It uses a very wide bus (e.g., 1024 bits per stack) to achieve extremely high bandwidth while consuming less power per bit than GDDR. It is found in AI accelerators, high‑end GPUs, and supercomputers.

**中文（台灣）**

高頻寬記憶體（HBM 記憶體）。

採用 3D 堆疊與寬匯流排（如 1024 位元），頻寬極高，功耗效率佳，用於 AI 晶片、高階顯示卡及超級電腦。

**中文（大陸）**

高带宽内存（HBM 内存）。

3D 堆叠 DRAM，使用宽总线，带宽极高，用于 AI 加速器、高端显卡和超算。

---

## 快速名稱對照表（Summary Table）

| English Term | 台灣 (Taiwan)        | 大陸 (Mainland China) |
| ------------ | ------------------ | ------------------- |
| NOR / NAND   | NOR / NAND 快閃記憶體   | NOR / NAND 闪存       |
| SRAM         | 靜態隨機存取記憶體          | 静态随机存取存储器           |
| DRAM         | 動態隨機存取記憶體          | 动态随机存取存储器           |
| SDRAM        | 同步動態隨機存取記憶體        | 同步动态随机存取存储器         |
| DDR          | 雙倍資料速率記憶體 (DDR記憶體) | 双倍数据率内存 (DDR内存)     |
| LPDDR        | 低功耗DDR / 行動記憶體     | 低功耗DDR / 移动内存       |
| ECC          | 錯誤糾正碼記憶體 (ECC記憶體)  | 纠错码内存 (ECC内存)       |
| HBM          | 高頻寬記憶體 (HBM記憶體)    | 高带宽内存 (HBM内存)       |

---
