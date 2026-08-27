## FPGA (現場可程式化邏輯閘陣列) 完整詳解

## Comprehensive Guide to FPGA (Field-Programmable Gate Array)

---

### 1. 什麼是 FPGA？ | What is an FPGA?

#### 中文
FPGA（Field-Programmable Gate Array）是一種可重複編程的積體電路（IC），設計人員可在製造完成後根據應用需求進行「現場」配置。不同於固定功能的專用晶片（ASIC），FPGA 透過硬體描述語言（HDL）來定義其內部邏輯行為，實現「軟體定義硬體」的彈性。

#### English
An FPGA is a reprogrammable integrated circuit that enables designers to configure its internal logic "in the field" after manufacturing. Unlike fixed-function Application-Specific Integrated Circuits (ASICs), FPGAs use Hardware Description Languages (HDLs) to define their logic behavior, offering the flexibility of "software-defined hardware."

---

### 2. FPGA 核心架構 | Core Architecture

FPGA 內部主要由三大基本單元構成，透過可編程的互連網路串聯起來：

#### 中文

&bull; **可配置邏輯塊 (CLB)**：FPGA 的基本邏單元，內部包含查找表（LUT）、正反器（Flip-Flop）和多工器（MUX）。LUT 本質上是一個靜態記憶體（SRAM），用來實現任意組合邏輯函數（例如 N-bit 輸入對應 1-bit 輸出）。

&bull; **輸入/輸出塊 (IOB)**：負責晶片引腳與內部邏輯之間的訊號介面，支援多種電氣標準（如 LVTTL、LVCMOS、SSTL）與高速傳輸協議。

&bull; **可編程互連線路**：由金屬導線和開關矩陣組成，負責將 CLB、IOB 與其他硬核（Hard IP）連接起來，決定了訊號的路徑與延遲。

&bull; **嵌入式硬核**：現代 FPGA 還整合了 DSP 單元（數位訊號處理，支援乘法累加）、BRAM（塊式靜態記憶體）、PLL（鎖相迴路，用於時脈管理）甚至 ARM 處理器核心（如 Zynq 系列）。

#### English

&bull; **Configurable Logic Blocks (CLBs)**：The foundational logic unit, containing Look-Up Tables (LUTs), Flip-Flops, and Multiplexers. LUTs are essentially SRAM-based tables that implement any combinational logic function (e.g., N-bit input to 1-bit output).

&bull; **I/O Blocks (IOBs)**：Manage signal interfacing between the chip's pins and internal logic, supporting various electrical standards (e.g., LVTTL, LVCMOS, SSTL) and high-speed protocols.

&bull; **Programmable Interconnects**：Composed of metal traces and switch matrices, these connect CLBs, IOBs, and hard IPs, determining signal routing and propagation delays.

&bull; **Embedded Hard IPs**：Modern FPGAs integrate DSP slices (for multiply-accumulate), BRAM (block RAM for data storage), PLLs (for clock management), and even ARM processor cores (e.g., Xilinx Zynq series).

---

### 3. 運作原理：如何「程式化」硬體？ | Working Principle: How to "Program" Hardware?

#### 中文
FPGA 的「程式」並非指令序列（如 CPU），而是**位元流（Bitstream）**。

1\. 設計者使用 Verilog / VHDL 撰寫 RTL（暫存器傳輸級）代碼。

2\. 透過 EDA 工具（如 Vivado、Quartus）**進行合成（Synthesis）**，將代碼轉為邏輯閘電路。

3\. **進行佈局與繞線（Place & Route）**，決定邏輯單元的位置與連線路徑。

4\. 產生位元流檔案，在上電時載入 FPGA 內部的 **配置記憶體（SRAM-based）**。這些 SRAM 單元控制著開關的導通與斷路，從而「定義」硬體功能。<br>
   _註：部分 FPGA（如 Flash-based 或 Anti-fuse）為非揮發性，斷電後仍保留配置。_

#### English
FPGA "programming" is not an instruction sequence (like a CPU) but a **Bitstream**.

1\. Designers write RTL (Register-Transfer Level) code using Verilog / VHDL.

2\. EDA tools (e.g., Vivado, Quartus) perform **Synthesis**, converting code into gate-level circuits.

3\. **Place & Route** determines the physical positions of logic units and routing paths.

4\. The generated bitstream is loaded into the FPGA's internal **Configuration Memory (typically SRAM-based)** upon power-up. These SRAM cells control the on/off states of pass transistors, thereby "defining" the hardware function. <br>
   _Note: Some FPGAs (Flash-based or Anti-fuse) are non-volatile, retaining configurations after power-off._

---

### 4. FPGA 對比 ASIC 與 CPU | FPGA vs. ASIC vs. CPU

| 比較維度 (Aspect)     | FPGA        | ASIC (專用晶片)    | CPU / GPU (處理器) |
| ----------------- | ----------- | -------------- | --------------- |
| **靈活性 (Flexibility)** | 極高 (可重覆修改)  | 低 (固化後無法變更)    | 高 (軟體可變)        |
| **開發成本 (NRE Cost)**   | 低 (無光罩費用)   | 極高 (需光罩，百萬美元起) | 中高              |
| **單位成本 (Unit Cost)**  | 高 (適合少量多樣)  | 極低 (適合大量生產)    | 中               |
| **功耗 (Power)**        | 較高 (靜態功耗大)  | 最佳 (優化後極低)     | 高               |
| **延遲 (Latency)**      | 極低 (硬體平行處理) | 極低             | 高 (依賴軟體排程)      |
| **改版週期 (Revison)**    | 數小時至數天      | 數月至數年          | 數週 (軟體更新)       |

**中文總結**：FPGA 是介於純軟體（CPU）與純硬體（ASIC）之間的硬體加速器。它保留了硬體的並行性與低延遲，同時具備可重構的靈活性，特別適合於標準尚未確立或需頻繁升級的領域。

**English Summary**：FPGA sits between pure software (CPU) and pure hardware (ASIC) as a Hardware Accelerator. It retains the parallelism and low latency of hardware while offering reconfigurable flexibility, making it ideal for applications with evolving standards or frequent upgrades.

---

### 5. FPGA 設計流程 (數位設計前端到後端) | Design Flow (Front-end to Back-end)

#### 中文
完整的 FPGA 開發流程通常包含以下步驟：

1\. **規格制定 (Specification)**：定義功能、時脈頻率、I/O 協議與面積限制。

2\. **RTL 編碼 (RTL Coding)**：使用 Verilog/SystemVerilog/VHDL 撰寫模組。

3\. **功能模擬 (Functional Simulation / RTL Sim)**：驗證邏輯行為是否符合預期，無需考慮延遲。

4\. **合成 (Synthesis)**：將 RTL 對映至特定 FPGA 廠商的邏輯單元（LUT/FF）。

5\. **佈局與繞線 (Place & Route)**：將邏輯單元擺放至晶片實際位置並連接。此階段決定最終的時序收斂。

6\. **時序模擬與分析 (Timing Simulation & STA)**：執行靜態時序分析（STA），檢查建立時間（Setup）與保持時間（Hold）是否違規。

7\. **產生位元流 (Bitstream Generation)**：生成用於下載的 .bit 或 .sof 檔案。

8\. **板級驗證 (Board-Level Verification)**：將位元流燒錄至 FPGA 進行實際測試。

#### English
A complete FPGA development flow includes:

1\. **Specification**：Define functions, clock frequency, I/O protocols, and area constraints.

2\. **RTL Coding**：Write modules using Verilog/SystemVerilog/VHDL.

3\. **Functional Simulation (RTL Sim)**：Verify logic behavior without timing delays.

4\. **Synthesis**：Map RTL to the specific FPGA vendor's primitives (LUTs/FFs).

5\. **Place & Route**：Place logic cells in physical locations and route interconnects. This stage determines final timing closure.

6\. **Timing Simulation & STA**：Perform Static Timing Analysis to check Setup/Hold time violations.

7\. **Bitstream Generation**：Generate the .bit or .sof file for download.

8\. **Board-Level Verification**：Program the FPGA and test on actual hardware.

---

### 6. 主要優勢與挑戰 | Key Advantages & Challenges

#### 中文
#### ✅ 優勢 (Advantages)：

&bull; **高度並行性**：硬體電路可同時運作，適合大規模數據流處理（如影像、雷達）。

&bull; **低延遲**：無需作業系統中斷，延遲可控制在奈秒級（ns）。

&bull; **可重構性**：同一片晶片在不同時間可執行不同功能（動態部分重構）。

&bull; **I/O 靈活**：可模擬各種通訊協議（如 SPI、I2C、PCIe、DDR）。

#### ❌ 挑戰 (Challenges)：

&bull; **功耗較高**：SRAM 配置與大量互連開關導致靜態洩漏電流較大。

&bull; **資源利用率**：大型設計可能面臨邏輯單元不足或繞線擁塞。

&bull; **學習曲線陡峭**：需具備數位邏輯設計、時序分析與硬體除錯能力。

&bull; **單位成本高**：在百萬級以上出貨量時，成本遠高於 ASIC。

#### English
#### ✅ Advantages：

&bull; **Massive Parallelism**：Hardware circuits operate concurrently, ideal for large data streams (e.g., video, radar).

&bull; **Ultra-Low Latency**：No OS interrupts; latency is controlled at the nanosecond (ns) level.

&bull; **Reconfigurability**：The same chip can perform different functions over time (Dynamic Partial Reconfiguration).

&bull; **I/O Flexibility**：Can emulate various communication protocols (SPI, I2C, PCIe, DDR).

#### ❌ Challenges：

&bull; **Higher Power Consumption**：SRAM configuration and numerous interconnect switches cause significant static leakage.

&bull; **Resource Utilization**：Large designs may face logic cell shortages or routing congestion.

&bull; **Steep Learning Curve**：Requires knowledge of digital logic design, timing analysis, and hardware debugging.

&bull; **High Unit Cost**：For shipments above millions, costs far exceed those of ASICs.

---

### 7. 主要應用領域 | Major Application Fields

#### 中文

&bull; **通訊與 5G/6G**：基地台波束成形（Beamforming）、通道編碼/解碼（LDPC、Turbo）、前傳（Fronthaul）數據壓縮。

&bull; **航太與國防**：雷達信號處理、電子戰（EW）、衛星在軌重構（抗輻射 FPGA）。

&bull; **汽車電子**：ADAS（先進駕駛輔助系統）、感測器融合（相機/雷達/光達）、車載娛樂。

&bull; **資料中心與 AI 加速**：微軟 Azure、亞馬遜 AWS 使用 FPGA 進行即時推論（Inference）、搜尋排序與資料壓縮（SmartNIC）。

&bull; **醫療影像**：CT/MRI 重建、超音波都普勒處理。

&bull; **工業自動化**：馬達控制、可編程邏輯控制器（PLC）、即時乙太網路（EtherCAT）。

#### English

&bull; **Telecom & 5G/6G**：Baseband beamforming, channel coding/decoding (LDPC, Turbo), Fronthaul data compression.

&bull; **Aerospace & Defense**：Radar signal processing, Electronic Warfare (EW), Satellite on-orbit reconfiguration (Radiation-tolerant FPGAs).

&bull; **Automotive**：ADAS (Advanced Driver Assistance Systems), sensor fusion (Camera/Radar/LiDAR), In-Vehicle Infotainment.

&bull; **Data Centers & AI Acceleration**：Microsoft Azure and AWS employ FPGAs for real-time inference, search ranking, and data compression (SmartNICs).

&bull; **Medical Imaging**：CT/MRI reconstruction, Ultrasound Doppler processing.

&bull; **Industrial Automation**：Motor control, Programmable Logic Controllers (PLCs), Real-time Ethernet (EtherCAT).

---

### 8. 未來趨勢 | Future Trends

#### 中文

1\. **AI 專用引擎**：AMD（Xilinx）Versal 與 Intel Agilex 系列內建 AI 張量核心，大幅提升深度學習運算效率。

2\. **異質整合（Heterogeneous Integration）**：將 FPGA 與 HBM（高頻寬記憶體）、光學 I/O 封裝在同一基板，突破記憶體牆。

3\. **開源工具鏈崛起**：Yosys + NextPNR 等開源工具逐漸成熟，降低中小企業進入門檻。

4\. **Chiplet 化**：透過 UCIe 標準將不同製程的小晶片組合，實現超大規模 FPGA 系統。

5\. **車規與功能安全**：針對 ISO 26262 ASIL-D 等級的強化設計，進一步滲透自動駕駛核心運算。

#### English

1\. **AI-Specific Engines**：AMD (Xilinx) Versal and Intel Agilex series integrate AI tensor cores, drastically improving deep learning compute efficiency.

2\. **Heterogeneous Integration**：Packaging FPGAs with HBM (High-Bandwidth Memory) and optical I/O on the same substrate to break the memory wall.

3\. **Rise of Open-Source Tools**：Open-source stacks like Yosys + NextPNR are maturing, lowering entry barriers for SMEs.

4\. **Chiplet Architecture**：Combining chiplets from different process nodes via the UCIe standard to build ultra-large-scale FPGA systems.

5\. **Automotive & Functional Safety**：Enhanced designs targeting ISO 26262 ASIL-D, further penetrating autonomous driving core compute domains.

---

### 總結 | Conclusion
FPGA 是現代電子系統中不可或缺的「萬能硬體」。它既沒有 ASIC 的高昂流片風險，又具備 CPU 無法比擬的實時性與並行處理能力。隨著 AI 與邊緣運算爆發，FPGA 正從傳統的「膠合邏輯」角色，轉變為異構運算平台的核心主導者。掌握 FPGA 設計，等同於掌握「定義硬體」的能力，是當前與未來電機資訊領域的關鍵技能。

### Summary
FPGAs are indispensable "universal hardware" in modern electronic systems. They avoid the high tape-out risks of ASICs while delivering real-time performance and parallelism unmatched by CPUs. With the explosion of AI and edge computing, FPGAs are evolving from traditional "glue logic" into the central pillars of heterogeneous computing platforms. Mastering FPGA design equips engineers with the power to "define hardware"—a critical skill in today's and tomorrow's EECS fields.