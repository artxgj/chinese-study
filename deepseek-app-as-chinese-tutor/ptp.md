## 精確時間協議 (Precision Time Protocol – IEEE 1588) 完整詳解

---

### 1. 概述 (Overview)

PTP 是由 IEEE 1588 標準定義的網絡同步協議，旨在透過數據網路，將分散式系統中的時鐘同步至亞微秒（μs）甚至納秒（ns）等級的精度。它不依賴 GPS 等外部信號，僅靠網路封包交換與精確時間戳記，即可為工業控制、電信、電力、數據中心等應用提供統一的時間基準。

PTP is a network synchronization protocol defined by the IEEE 1588 standard. It aims to synchronize clocks in distributed systems over a data network to sub‑microsecond or even nanosecond accuracy. Without relying on external signals like GPS, it uses packet exchanges and precise timestamping to provide a unified time reference for applications such as industrial control, telecommunications, power grids, and data centers.

---

### 2. 同步原理與封包交換 (Synchronization Principle & Packet Exchange)

PTP 採用主從（Master‑Slave）階層架構。主時鐘（Grandmaster）作為時間源，從時鐘（Slave）透過四次封包交換來測量並補償兩項誤差：

&middot; 時鐘偏移（Offset）：主從兩端時鐘的絕對時間差。

&middot; 網路延遲（Delay）：封包在鏈路中單向傳輸的時間（假設對稱）。

四個關鍵封包及時間戳記如下（假設延遲對稱）：

| 步驟 | 動作                     | 記錄時間戳              |
| -- | ---------------------- | ------------------ |
| 1  | 主端發送 Sync              | 主端記錄 t₁，從端收到時記錄 t₂ |
| 2  | 主端發送 Follow_Up（攜帶 t₁）  | 從端取得 t₁            |
| 3  | 從端發送 Delay_Req         | 從端記錄 t₃，主端收到時記錄 t₄ |
| 4  | 主端回覆 Delay_Resp（攜帶 t₄） | 從端取得 t₄            |

從端利用四個時間戳計算：

&middot; 平均延遲： $$\text{Offset} = \frac{(t_2 - t_1) - (t_4 - t_3)}{2}$$

&middot; 時鐘偏移： $$\text{Offset} = \frac{(t_2 - t_1) - (t_4 - t_3)}{2}$$
  之後從端將本地時鐘調整為「主時鐘時間 + Offset」，即可達到同步。


PTP uses a Master‑Slave hierarchy. The master (Grandmaster) is the time source, and slaves measure and compensate for two errors via four packet exchanges:

&middot; Clock offset – the absolute time difference between master and slave.

&middot; Network delay – the one‑way transmission time (assuming symmetric links).

The four key messages and timestamps are:

| Step | Action                                 | Timestamp recorded                               |
| ---- | -------------------------------------- | ------------------------------------------------ |
| 1    | Master sends Sync                      | Master records t₁; Slave records t₂ upon receipt |
| 2    | Master sends Follow_Up (carries t₁)    | Slave obtains t₁                                 |
| 3    | Slave sends Delay_Req                  | Slave records t₃; Master records t₄ upon receipt |
| 4    | Master replies Delay_Resp (carries t₄) | Slave obtains t₄                                 |

The slave computes:

&middot; Mean delay: $$\text{Delay} = \frac{(t_2 - t_1) + (t_4 - t_3)}{2}$$

&middot; Clock offset:  $$\text{Offset} = \frac{(t_2 - t_1) - (t_4 - t_3)}{2}$$

The slave then adjusts its local clock to “master time + Offset”, achieving synchronization.

---

### 3. 一步模式與兩步模式 (One‑Step vs Two‑Step Mode)

&middot; 一步模式：Sync 封包本身即攜帶精確傳送時間 t₁，無需 Follow_Up。此模式要求硬體在發送封包時即時嵌入時間戳，硬體要求較高，但頻寬效率更佳。

&middot; 兩步模式：Sync 僅觸發測量，稍後由 Follow_Up 攜帶 t₁。此模式對硬體要求較低，廣泛用於一般設備。


&middot; One‑step mode: The Sync message carries the precise departure time t₁ itself, so no Follow_Up is needed. This requires hardware that can embed timestamps on the fly, offering better bandwidth efficiency but stricter hardware requirements.

&middot; Two‑step mode: The Sync only triggers the measurement; t₁ is delivered later via a Follow_Up message. This is easier on hardware and is widely adopted in general equipment.

---

### 4. 時鐘角色 (Clock Roles)

IEEE 1588 定義三種主要設備角色：

&middot; OC（Ordinary Clock，普通時鐘）：只有一個 PTP 連接埠，可作為主時鐘或從時鐘（終端設備）。

&middot; BC（Boundary Clock，邊界時鐘）：具多個連接埠，在一個網段內作為從時鐘，在另一個網段作為主時鐘，用以隔離延遲變動，提升擴展性。

&middot; TC（Transparent Clock，透明時鐘）：轉發 PTP 封包，並計算封包在自身內部的停留時間（Residence Time），將其加入校正欄位，以補償交換機造成的延遲，不參與主從競爭。


IEEE 1588 defines three main device roles:

&middot; OC (Ordinary Clock): Has a single PTP port, acts as master or slave (end device).

&middot; BC (Boundary Clock): Has multiple ports; acts as slave in one segment and master in another, isolating delay variations and improving scalability.

&middot; TC (Transparent Clock): Forwards PTP packets and computes the residence time inside itself, adding it to the correction field to compensate for switch‑induced delays; it does not participate in master‑slave election.

---

### 5. 最佳主時鐘演算法 (Best Master Clock Algorithm – BMC)


BMC 是用於自動選出網域內最優質主時鐘的分散式演算法。每個時鐘會宣告自身的屬性（如時鐘品質、優先級、精確度等）。所有時鐘互相交換這些資訊，經由比較後，具最高品質的時鐘成為 Grandmaster，其餘則自動跟隨。此機制確保網路在時鐘故障或新增設備時能自動重新收斂。

The BMC algorithm is a distributed algorithm that automatically selects the best master clock within a domain. Each clock advertises its attributes (e.g., clock quality, priority, accuracy). All clocks exchange this information and, after comparison, the highest‑quality clock becomes the Grandmaster, while others automatically follow. This ensures automatic re‑convergence upon clock failure or new device insertion.

---

### 6. 硬體時間戳記與精度 (Hardware Timestamping and Accuracy)

PTP 的精確度高度依賴於時間戳記的產生位置。

&middot; 軟體時間戳記（應用層）誤差可達數毫秒，僅適合一般用途。

&middot; 硬體時間戳記（在實體層或 MAC 層）可在封包進出網路介面時精確記錄，誤差通常在 1 ns ~ 100 ns 之間，是達到高精度的關鍵。實作上常使用具備 PTP 硬體支援的專用 NIC（網路卡）或 FPGA。

PTP accuracy heavily depends on where timestamps are generated.

&middot; Software timestamping (at application layer) can have errors of several milliseconds, suitable only for general purposes.

&middot; Hardware timestamping (at PHY or MAC layer) records timestamps precisely when packets enter or leave the network interface, with errors typically in the range of 1 ns ~ 100 ns, which is the key to high precision. Dedicated NICs or FPGAs with PTP hardware support are often used.

---

### 7. PTP 設定檔 (PTP Profiles)

為了適應不同產業需求，IEEE 1588 定義了多種設定檔（Profile），規定特定的參數與行為：

&middot; Default Profile（預設） – 一般用途，支援二層（L2）或三層（UDP）傳輸。

&middot; Telecom Profile（電信設定檔，G.8265.1 / G.8275.1） – 優化用於電信網路，支援頻率與相位同步。

&middot; Power Profile（電力設定檔，IEEE C37.238） – 針對電力系統的保護與控制應用，要求極高可靠性和冗餘。

&middot; Enterprise Profile – 簡化版，適用於一般企業數據中心。


To meet different industry needs, IEEE 1588 defines several profiles that specify parameters and behaviors:

&middot; Default Profile – general purpose, supports Layer‑2 or UDP transport.

&middot; Telecom Profile (G.8265.1 / G.8275.1) – optimised for telecom networks, supporting frequency and phase synchronization.

&middot; Power Profile (IEEE C37.238) – designed for power system protection and control, requiring high reliability and redundancy.

&middot; Enterprise Profile – simplified version for general enterprise data centers.

---

### 8. 與 NTP 的關鍵差異 (Key Differences from NTP)

| 項目   | PTP (IEEE 1588) | NTP (RFC 5905)    |
| ---- | --------------- | ----------------- |
| 精度   | 亞微秒～納秒          | 毫秒～微秒（通常 1‑10 ms） |
| 硬體需求 | 需硬體時間戳記以達高精度    | 軟體時間戳記即可          |
| 時鐘階層 | 主從階層，BMC 自動選擇   | 樹狀階層，多層參考         |
| 校正頻率 | 每秒可達數次至數十次      | 通常每數分鐘一次          |
| 應用場景 | 工業、電信、電力、金融高頻交易 | 網路伺服器、一般日誌時間      |


| Feature         | PTP (IEEE 1588)                                   | NTP (RFC 5905)                         |
| --------------- | ------------------------------------------------- | -------------------------------------- |
| Accuracy        | Sub‑µs to ns                                      | ms to µs (typically 1‑10 ms)           |
| Hardware        | Requires hardware timestamping for high precision | Software timestamping sufficient       |
| Clock hierarchy | Master‑slave with BMC auto‑selection              | Hierarchical tree with multiple strata |
| Correction rate | Up to tens of times per second                    | Typically every few minutes            |
| Use cases       | Industrial, telecom, power, HFT                   | Network servers, general logging       |

---

### 9. 應用領域 (Applications)

&middot; 工業自動化：機器人協作、運動控制需精確同步。

&middot; 電信 5G 前傳/回傳：基站間時間對齊，支援 TDD 及協作通信。

&middot; 電力系統：故障錄波、相量測量（PMU）同步。

&middot; 數據中心：分散式資料庫、金融交易排序。

&middot; 時間敏感網路（TSN）：與 TSN 協同，實現確定性通信。


&middot; Industrial automation – precise coordination for robotics and motion control.

&middot; 5G fronthaul/backhaul – time alignment among base stations for TDD and coordinated communication.

&middot; Power systems – fault recording and phasor measurement (PMU) synchronization.

&middot; Data centers – distributed databases and financial transaction ordering.

&middot; Time‑Sensitive Networking (TSN) – in conjunction with TSN to enable deterministic communication.

---

### 10. 總結 (Summary)

IEEE 1588 PTP 是目前業界最高精度的網路時間同步協議，其核心在於硬體時間戳記、對稱延遲假設下的封包交換演算法，以及 BMC 的自動主時鐘選擇。透過適當的設定檔與設備角色，可靈活部署於從簡單企業網路到複雜電信、電力系統的各種場景。

IEEE 1588 PTP is the most precise network time synchronization protocol available today. Its core lies in hardware timestamping, packet‑exchange algorithms based on symmetric delay assumptions, and automatic master selection via BMC. With appropriate profiles and clock roles, it can be flexibly deployed across scenarios ranging from simple enterprise networks to complex telecom and power systems.

