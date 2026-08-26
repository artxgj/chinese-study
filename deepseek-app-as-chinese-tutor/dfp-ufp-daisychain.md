### Part 1: Prompt Repetition

**Original prompt:**  
> Provide a comprehensive technical explanation of USB's DFP and UFP functions, their history, and how they affected daisy-chaining. Draw diagrams to illustrate, use Monospaced-fonts, to align boundaries of boxes, make sure text-labels fit inside the diagram's ascii boxes.

---

### Part 2: Essence in Concise, Colloquial Chinese

> 請詳細解釋 USB 的 DFP 和 UFP 功能的技術原理、它們的歷史演進，以及它們如何影響菊花鏈（daisy-chaining）的實現。請繪製等寬字體的 ASCII 圖表來說明，確保文字標籤能完整放入圖表框線內。

---

### Part 3: Bilingual Response

#### CHINESE SECTION

**摘要**

DFP（Downstream Facing Port，下行埠）和 UFP（Upstream Facing Port，上行埠）是 USB Type-C 規格中引入的術語，用於取代傳統的「主機」（Host）和「裝置」（Device）概念 [1]。DFP 是數據傳輸中的主機端（如電腦），而 UFP 是裝置端（如隨身碟）[2]。它們隨著 USB Type-C 規格 1.0 版於 **2014 年 8 月** 發布而誕生，目的是為了支持 Type-C 連接器的可正反插、更高功率傳輸（USB PD）以及數據與電源角色的動態獨立切換 [3][4]。

其核心工作原理是透過 **CC（Configuration Channel）引腳**上的電阻進行識別：DFP 的 CC 引腳有上拉電阻（Rp），UFP 有下拉電阻（Rd，固定值 5.1kΩ）[5]。當兩者連接時，DFP 檢測到 Rd 下拉電阻，識別到 UFP 裝置，隨後打開 VBUS 電源供電 [6]。**菊花鏈（Daisy Chaining）** 則依賴於 **DRP（Dual Role Port，雙角色埠）** 的動態角色切換能力 [7]。在菊花鏈中，中間設備（如顯示器）對上游扮演 UFP（裝置），對下游扮演 DFP（主機），從而實現多設備串聯 [8]。

---

**細節**

**1. 定義：什麼是 DFP、UFP 和 DRP？**

在 USB Type-C 的語境下，數據角色（Data Role）被重新定義 [1]：

| 術語    | 全稱                   | 角色                       | 典型設備                     |
| :------ | :--------------------- | :------------------------- | :--------------------------- |
| **DFP** | Downstream Facing Port | 主機（Host）／電源提供者   | 電腦、充電器、集線器的下行埠 |
| **UFP** | Upstream Facing Port   | 裝置（Device）／電源消耗者 | 隨身碟、鍵盤、滑鼠           |
| **DRP** | Dual Role Port         | 可動態切換為 DFP 或 UFP    | 筆記型電腦、新款手機         |

DFP 是數據流向下游的埠，負責發起數據傳輸，並在默認情況下提供 VBUS 電源 [2]。UFP 是數據流向上游的埠，接收來自 DFP 的數據，並從 VBUS 取電 [3]。DRP 則可以在連接時根據對端設備動態切換角色 [4]。

**2. 歷史：何時納入標準？為何增加？**

DFP 和 UFP 這兩個術語是伴隨著 **USB Type-C 規格**一同出現的。USB Type-C 規格 1.0 版於 **2014 年 8 月 11 日** 正式發布 [5]。它們被引入是為了支持 Type-C 帶來的新特性 [6]：

- **角色動態切換**：在 USB Type-C 之前，主機和裝置的角色是固定的。Type-C 允許數據角色（Data Role）和電源角色（Power Role）**獨立運作與協商**，一個裝置可以在不同連接情境中扮演不同角色 [7]。
- **支援 USB PD**：USB Power Delivery 協議允許動態調整電壓和電流，DFP/UFP/DRP 的定義為這種靈活的電源管理提供了基礎 [8]。
- **適應新連接器**：Type-C 連接器的對稱性（正反插）和多功能性（支援替代模式）需要更靈活的角色定義。

**3. 運作原理：硬體檢測與軟體協商**

DFP 和 UFP 的識別與角色分配通過硬體和軟體協同完成。

**3.1 硬體檢測（CC 引腳電阻機制）**

USB Type-C 連接器有兩個 CC 引腳（CC1 和 CC2）[1]。DFP 在 CC 引腳上提供**上拉電阻（Rp）**，而 UFP 提供**下拉電阻（Rd，固定值 5.1kΩ）**[2]。

- **連接檢測**：未連接時，DFP 的 VBUS 無輸出。當 DFP 和 UFP 通過線纜連接時，CC 引腳相連。DFP 檢測到 CC 引腳上的電壓被 UFP 的 Rd 下拉電阻拉低，從而識別到 UFP 設備連接 [3]。
- **方向檢測**：通過檢測哪個 CC 引腳（CC1 或 CC2）被拉低，系統判斷線纜的插入方向，並據此切換高速數據線（TX/RX）[4]。
- **電源供應**：一旦檢測到有效的 UFP 連接，DFP 打開 VBUS 開關，輸出電源給 UFP [5]。

**3.2 軟體協商（USB PD 協議）**

對於更複雜的場景（如角色互換、電壓協商），則需要 USB PD 協議。通過 CC 引腳上的雙向通訊，DFP 和 UFP 可以交換能力資訊，並動態改變數據角色（**DR_Swap**，Data Role Swap）或電源角色（**PR_Swap**，Power Role Swap）[6]。

**4. 菊花鏈（Daisy Chaining）與 DFP/UFP 的動態角色**

菊花鏈是指將多個設備（如顯示器）以串聯方式連接的拓撲結構 [7]。在 USB Type-C 環境中，實現菊花鏈的關鍵在於：

- 設備的埠必須是 **DRP（雙角色埠）**。
- 通常需要支援 **DisplayPort 替代模式（Alt Mode）** 或 **Thunderbolt** [8]。
- 中間設備必須能夠**同時扮演 UFP 和 DFP**：對上游是裝置（UFP），對下游是主機（DFP）。

**角色動態分配流程**：

1. **上游埠**：連接到主機（如筆記型電腦）的埠，扮演 **UFP** 角色，從主機接收視頻數據和電力。
2. **下游埠**：用於連接下一台設備（如第二台顯示器）的埠，扮演 **DFP** 角色，將視頻數據轉發出去，並為下游設備供電。

**示例**：筆記型電腦（DRP，作為 DFP）→ 顯示器 1（上游埠為 UFP，下游埠為 DFP）→ 顯示器 2（上游埠為 UFP）。顯示器 1 在這裡充當「中繼」或「集線器」的角色。

---

**圖示**

**圖 1：DFP 與 UFP 的 CC 引腳電阻檢測機制**

```
  +---------------------------+          +-----------------------------+
  |        DFP (Host)         |          |        UFP (Device)          |
  |                           |          |                             |
  |  +--------+  +--------+  |          |  +--------+  +--------+     |
  |  |  CC1   |  |  CC2   |  |          |  |  CC1   |  |  CC2   |     |
  |  |  (Rp)  |  |  (Rp)  |  |          |  |  (Rd)  |  |  (Rd)  |     |
  |  +----+---+  +----+---+  |          |  +----+---+  +----+---+     |
  |       |           |      |          |       |           |         |
  |       +-----------+      |          |       +-----------+         |
  |               |          |          |               |             |
  |           VBUS (Off)     |          |           VBUS (Sink)       |
  |               |          |          |               |             |
  +---------------+----------+          +---------------+-------------+
                  |                                  |
                  +--------- Type-C Cable -----------+
                               |
                        CC pin connected,
                        DFP detects Rd,
                        enables VBUS
```

**圖 2：菊花鏈中的 DFP/UFP 角色動態分配**

```
  +-------------+          +---------------------------+          +---------------------+
  |    Laptop   |          |       Monitor 1           |          |      Monitor 2      |
  |   (DRP)     |          |    (DRP with DP Alt)      |          |   (DRP with DP Alt) |
  |             |          |                           |          |                     |
  |  +-------+  |          |  +---------+ +---------+  |          |  +---------+        |
  |  |Port A |  |          |  |Upstream | |Downstream|  |          |  |Upstream |        |
  |  |(as    |  |          |  |(UFP)    | |(DFP)    |  |          |  |(UFP)    |        |
  |  | DFP)  |--|----------|->|         | |         |--|--------->|  |         |        |
  |  +-------+  |          |  +---------+ +---------+  |          |  +---------+        |
  |             |          |                           |          |                     |
  +-------------+          +---------------------------+          +---------------------+
        |                             |                                    |
        |  1. Laptop acts as DFP      |  2. Monitor 1 upstream = UFP      |  4. Monitor 2 upstream = UFP
        |     (Host)                  |     (Device to laptop)             |     (Device to Monitor 1)
        |                             |  3. Monitor 1 downstream = DFP     |
        |                             |     (Host to Monitor 2)            |
        |                             |                                    |
        +---------- Data/Video/Power Flow --------------------------------+
```

---

#### ENGLISH SECTION

**Abstract**

DFP (Downstream Facing Port) and UFP (Upstream Facing Port) are terms introduced with the USB Type-C specification to replace the traditional "Host" and "Device" concepts [1]. A DFP is the host side in data transfer (e.g., a computer), while a UFP is the device side (e.g., a USB flash drive) [2]. They were introduced alongside the USB Type-C specification, Release 1.0, published in **August 2014**, to accommodate new features like reversible plug orientation, higher power delivery (USB PD), and dynamic independent switching of data and power roles [3][4].

Their operation is based on resistor detection on the **CC (Configuration Channel) pins**: a DFP has a pull-up resistor (Rp) on its CC pins, while a UFP has a pull-down resistor (Rd, fixed at 5.1kΩ) [5]. When connected, the DFP detects the Rd pull-down resistor, recognizes the UFP device, and then enables VBUS power [6]. **Daisy chaining** relies on devices with **DRP (Dual Role Port)** capability for dynamic role switching [7]. In a daisy chain, an intermediate device (like a monitor) acts as a UFP (device) to the upstream host and as a DFP (host) to the downstream device [8].

---

**Details**

**1. Definition: What are DFP, UFP, and DRP?**

In the context of USB Type-C, the Data Role is redefined [1]:

| Term    | Full Name              | Role                   | Typical Devices                           |
| :------ | :--------------------- | :--------------------- | :---------------------------------------- |
| **DFP** | Downstream Facing Port | Host / Power Source    | Computers, chargers, hub downstream ports |
| **UFP** | Upstream Facing Port   | Device / Power Sink    | USB flash drives, keyboards, mice         |
| **DRP** | Dual Role Port         | Dynamically DFP or UFP | Laptops, modern smartphones               |

A DFP is the port through which data flows downstream, initiating data transactions and sourcing VBUS power by default [2]. A UFP is the port through which data flows upstream, receiving data from the DFP and sinking power from VBUS [3]. A DRP can dynamically switch its role based on the connected device [4].

**2. History: When and Why Were They Added?**

The terms DFP and UFP were introduced with the **USB Type-C specification** [5]. USB Type-C specification, Release 1.0, was published on **August 11, 2014** [5]. They were added to support new features brought by Type-C [6]:

- **Dynamic Role Switching**: Before Type-C, host and device roles were fixed. Type-C allows Data Role and Power Role to be **negotiated independently**, enabling a device to act differently depending on the connection context [7].
- **Support for USB PD**: USB Power Delivery allows dynamic voltage and current adjustment. The DFP/UFP/DRP definitions provide the foundation for this flexible power management [8].
- **Adaptation to the New Connector**: The symmetry (reversible plug) and multi-functionality (support for Alternate Modes) of the Type-C connector required more flexible role definitions.

**3. How Do They Work? Hardware Detection and Software Negotiation**

The identification and role assignment of DFP and UFP are achieved through a combination of hardware and software.

**3.1 Hardware Detection (CC Pin Resistor Mechanism)**

The USB Type-C connector has two CC pins (CC1 and CC2) [1]. A DFP provides a **pull-up resistor (Rp)** on its CC pins, while a UFP provides a **pull-down resistor (Rd, fixed at 5.1kΩ)** [2].

- **Connection Detection**: When disconnected, the DFP's VBUS has no output. When a DFP and UFP are connected via a cable, the CC pins are linked. The DFP detects that the voltage on its CC pin is pulled low by the UFP's Rd resistor, indicating a UFP device is attached [3].
- **Orientation Detection**: By detecting which CC pin (CC1 or CC2) is pulled low, the system determines the cable's orientation and switches the high-speed data lines (TX/RX) accordingly [4].
- **Power Delivery**: Once a valid UFP connection is detected, the DFP turns on the VBUS switch to provide power to the UFP [5].

**3.2 Software Negotiation (USB PD Protocol)**

For more complex scenarios like role swaps or voltage negotiation, the USB PD protocol is used. Through bidirectional communication over the CC pins, the DFP and UFP can exchange capability information and dynamically change their Data Role (**DR_Swap**, Data Role Swap) or Power Role (**PR_Swap**, Power Role Swap) [6].

**4. Daisy Chaining and Dynamic DFP/UFP Roles**

Daisy chaining is a topology where multiple devices (e.g., monitors) are connected in series [7]. In a USB Type-C environment, the key requirements are:

- Devices' ports must be **DRP (Dual Role Ports)**.
- Typically need to support **DisplayPort Alternate Mode (Alt Mode)** or **Thunderbolt** [8].
- Intermediate devices must be able to **act as both UFP and DFP simultaneously**: UFP to the upstream, DFP to the downstream.

**Dynamic Role Assignment Flow**:

1. **Upstream Port**: The port connected to the host (e.g., a laptop) acts as a **UFP**, receiving video data and power from the host.
2. **Downstream Port**: The port used to connect to the next device (e.g., a second monitor) acts as a **DFP**, forwarding the video data and providing power to the downstream device.

**Example**: Laptop (DRP, acting as DFP) → Monitor 1 (upstream port as UFP, downstream port as DFP) → Monitor 2 (upstream port as UFP). Monitor 1 acts as a "repeater" or "hub".

---

**Diagrams**

**Figure 1: DFP and UFP CC Pin Resistor Detection Mechanism**

```
  +---------------------------+          +-----------------------------+
  |        DFP (Host)         |          |        UFP (Device)          |
  |                           |          |                             |
  |  +--------+  +--------+  |          |  +--------+  +--------+     |
  |  |  CC1   |  |  CC2   |  |          |  |  CC1   |  |  CC2   |     |
  |  |  (Rp)  |  |  (Rp)  |  |          |  |  (Rd)  |  |  (Rd)  |     |
  |  +----+---+  +----+---+  |          |  +----+---+  +----+---+     |
  |       |           |      |          |       |           |         |
  |       +-----------+      |          |       +-----------+         |
  |               |          |          |               |             |
  |           VBUS (Off)     |          |           VBUS (Sink)       |
  |               |          |          |               |             |
  +---------------+----------+          +---------------+-------------+
                  |                                  |
                  +--------- Type-C Cable -----------+
                               |
                        CC pin connected,
                        DFP detects Rd,
                        enables VBUS
```

**Figure 2: Dynamic DFP/UFP Role Assignment in Daisy Chaining**

```
  +-------------+          +---------------------------+          +---------------------+
  |    Laptop   |          |       Monitor 1           |          |      Monitor 2      |
  |   (DRP)     |          |    (DRP with DP Alt)      |          |   (DRP with DP Alt) |
  |             |          |                           |          |                     |
  |  +-------+  |          |  +---------+ +---------+  |          |  +---------+        |
  |  |Port A |  |          |  |Upstream | |Downstream|  |          |  |Upstream |        |
  |  |(as    |  |          |  |(UFP)    | |(DFP)    |  |          |  |(UFP)    |        |
  |  | DFP)  |--|----------|->|         | |         |--|--------->|  |         |        |
  |  +-------+  |          |  +---------+ +---------+  |          |  +---------+        |
  |             |          |                           |          |                     |
  +-------------+          +---------------------------+          +---------------------+
        |                             |                                    |
        |  1. Laptop acts as DFP      |  2. Monitor 1 upstream = UFP      |  4. Monitor 2 upstream = UFP
        |     (Host)                  |     (Device to laptop)             |     (Device to Monitor 1)
        |                             |  3. Monitor 1 downstream = DFP     |
        |                             |     (Host to Monitor 2)            |
        |                             |                                    |
        +---------- Data/Video/Power Flow --------------------------------+
```

---

### Part 4: References

[1] Infineon Technologies. (2024). USB Type-C and USB-PD FAQs. *Infineon Community*. Available at: https://community.infineon.com/t5/Knowledge-Base-Articles/USB-Type-C-and-USB-PD-FAQs/ta-p/679219

[2] Microchip Technology. (2025). USB Type-C® Nomenclature and Features. *Microchip Developer Help*. Available at: https://developerhelp.microchip.com/xwiki/bin/view/applications/usb/typec-features/

[3] Texas Instruments. (n.d.). JAJSJU4 Data Sheet – TPS25850-Q1. *ti.com*. Available at: https://www.ti.com/document-viewer/ja-jp/TPS25850-Q1/datasheet/

[4] 中国电子技术论坛. (2018). TYPE-C接口的工作原理图文详解. *chinaaet.com*. Available at: http://m.chinaaet.com/article/3000093562

[5] Granite River Labs. (2022). USB Type-C Power Delivery 的角色交换功能. *graniteriverlabs.com.cn*. Available at: https://www.graniteriverlabs.com.cn/technical-blog/application-notes-usbc-role-swap/

[6] USB-IF. (2014). USB Type-C Cable and Connector Specification, Release 1.0. *usb.org*.

[7] Texas Instruments. (n.d.). SLVSDB5 Data Sheet – TPS65988. *ti.com*.

[8] KTC. (2026). Can You Daisy-Chain USB-C Monitors Without Thunderbolt Support? *us.ktcplay.com*. Available at: https://us.ktcplay.com