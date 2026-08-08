Here is a comprehensive bilingual (English + Traditional Chinese) deep dive into **Camera Calibration**, with a specific focus on **Motion Capture (Mocap) Camera Systems**.

---

### Comprehensive Guide to Camera Calibration (Including Mocap Systems)

### 相機校準完整指南（含動作捕捉系統）

---

#### 1. Introduction | 引言

##### English:
Camera calibration is the process of estimating the parameters of a camera model to transform 2D image pixels into accurate 3D world coordinates. It corrects optical distortions and defines the geometric relationship between the camera's sensor and the physical world. In the context of **Mocap cameras** (typically high-speed infrared units), calibration is not a one-time static process but a rigorous, dynamic routine performed daily to ensure sub-millimeter tracking accuracy.

##### 中文：
相機校準是估算相機模型參數的過程，目的是將 2D 影像像素準確轉換為 3D 世界座標。它能修正光學畸變，並定義相機感光元件與物理世界之間的幾何關係。在**動作捕捉（Mocap）相機**（通常為高速紅外線單元）的應用中，校準並非一次性靜態流程，而是每日執行、極為嚴謹的動態常規操作，以確保達到次毫米級的追蹤精度。

---

#### 2. The Core Mathematical Model | 核心數學模型 (The Pinhole Model)

##### English:
The standard geometric model is the **Pinhole Camera Model**. The projection of a 3D world point $P(X, Y, Z)$ onto a 2D image point $p(u, v)$ is defined as:

$$s \begin{bmatrix} u \\ v \\ 1 \end{bmatrix} = A \begin{bmatrix} R | t \end{bmatrix} \begin{bmatrix} X \\ Y \\ Z \\ 1 \end{bmatrix}$$

Where:

- $s$ is an arbitrary scale factor.
- $A$ is the **Intrinsic Matrix** (camera internal properties).
- $[R|t]$ is the **Extrinsic Matrix** (rotation and translation from world to camera).

**The Intrinsic Matrix** $A$:

$$A = \begin{bmatrix} f_x & \gamma & c_x \\ 0 & f_y & c_y \\ 0 & 0 & 1 \end{bmatrix}$$

- $f_x, f_y$: Focal lengths in pixel units (scaled by sensor pixel size).
- $c_x, c_y$: Principal point (usually the image center).
- $\gamma$: Skew factor (usually near zero in modern sensors).

##### 中文：
標準的幾何模型是針孔相機模型。將 $3D$ 空間點 $P(X, Y, Z)$ 投影至 2D 影像點 $p(u, v)$ 的公式為：

$$s \begin{bmatrix} u \\ v \\ 1 \end{bmatrix} = A \begin{bmatrix} R | t \end{bmatrix} \begin{bmatrix} X \\ Y \\ Z \\ 1 \end{bmatrix}$$

其中：

- $s$ 是任意尺度係數。
- $A$ 是內參矩陣（相機內部屬性）。
- $[R|t]$ 是外參矩陣（從世界座標到相機座標的旋轉與平移）。

**內參矩陣** $A$:

$$A = \begin{bmatrix} f_x & \gamma & c_x \\ 0 & f_y & c_y \\ 0 & 0 & 1 \end{bmatrix}$$

- $f_x, f_y$：以像素為單位的焦距（由感光元件像素尺寸換算）。
- $c_x, c_y$：主點（通常為影像中心）。
- $\gamma$：傾斜係數（現代感光元件中通常接近零）。

---

#### 3. Optical Distortions | 光學畸變

##### English:
Real lenses introduce distortions. The most common model compensates for **Radial** and Tangential distortions.

- **Radial Distortion**: Caused by the lens shape. Corrected by $k_1$, $k_2$, $k_3$ coefficients. It makes straight lines bend inward (Pincushion) or outward (Barrel).
- **Tangential Distortion**: Caused by the lens not being perfectly parallel to the sensor. Corrected by $p_1$, $p_2$ coefficients.

**Mathematical correction formula (Brown-Conrady model)**:

$$x_{corrected} = x(1 + k_1 r^2 + k_2 r^4 + k_3 r^6) + [2p_1 xy + p_2 (r^2 + 2x^2)]$$


$$y_{corrected} = y(1 + k_1 r^2 + k_2 r^4 + k_3 r^6) + [p_1 (r^2 + 2y^2) + 2p_2 xy]$$

##### 中文：
真實鏡頭會引入畸變。最常見的模型會補償**徑向畸變**與**切向畸變**。

- **徑向畸變**：由鏡頭形狀造成，透過係數 $k_1$, $k_2$, $k_3$ 修正。使直線向內彎曲（枕形）或向外彎曲（桶形）。
- **切向畸變**：由鏡頭與感光元件不完全平行造成，透過係數 $p_1$, $p_2$ 修正。

**數學修正公式（Brown-Conrady 模型）**：

$$x_{校正} = x(1 + k_1 r^2 + k_2 r^4 + k_3 r^6) + [2p_1 xy + p_2 (r^2 + 2x^2)]$$

$$y_{校正} = y(1 + k_1 r^2 + k_2 r^4 + k_3 r^6) + [p_1 (r^2 + 2y^2) + 2p_2 xy]$$

---

#### 4. Calibration Targets: Standard CV vs. Mocap | 校準標靶：標準電腦視覺 vs. 動作捕捉

| **Feature**   | **Standard Computer Vision**                                             | **Mocap Systems (e.g., Vicon, OptiTrack)**                                                |
| --------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| **Target**    | **Checkerboard** or **ChArUco** board (planar).                              | **L-Frame** (static) & **Wand** (dynamic).                                                    |
| **Material**  | Printed paper on a flat board.                                       | Precision-machined carbon fiber with retro-reflective markers.                        |
| **Principle** | Detects chessboard corners; solves via Zhang’s method (closed-form). | Detects active/passive marker centroids; uses bundle adjustment with known distances. |
| **Objective** | Intrinsics + Extrinsics for a fixed stereo pair.                     | **Dynamic Calibration** to track moving rigid bodies in a large volume.                   |

| **<span style="white-space: nowrap;">特點</span>** | **標準電腦視覺**                 | **動作捕捉系統（如 Vicon, OptiTrack）**            |
|--------------------------------------------------| ---------------------- | ------------------------------------- |
| **標靶**                                           | **棋盤格** 或 **ChArUco** 板（平面）。   | **L 型框架**（靜態）與 **T 型/Wand 校準棒**（動態）。          |
| **材質**                                           | 印刷紙張貼於平面硬板上。           | 精密加工碳纖維材質，貼有反光標記點。                    |
| **原理**                                           | 偵測棋盤格角點；透過張正友法（封閉解）求解。 | 偵測主動式（LED）或被動式（反光）標記質心；利用已知距離進行光束法平差。 |
| **目標**                                           | 獲取固定立體視覺對的內參與外參。       | **動態校準**，以在大型空間中追蹤移動物體（剛體）。               |

---

#### 5. Mocap-Specific Calibration Process | 動作捕捉專用校準流程

##### English:
Mocap calibration is strictly divided into two distinct phases:

Phase 1: Static Calibration (Floor / L-Frame)

- The L-frame is placed at the origin of the capture volume.
- It defines the **World Origin (0,0,0)** and the orientation of the X, Y, and Z axes (usually Z-up).
- Cameras capture the L-frame to establish the global reference frame and compute coarse extrinsic parameters.

Phase 2: Dynamic Calibration (Wanding / Residual Reduction)

- An operator walks through the entire capture volume waving a **Wand** (a rigid rod of precisely known length, e.g., 500mm).
- The system tracks the two markers on the wand. Because the distance between them is known, the software triangulates the 3D positions across all camera views.
- The algorithm iteratively optimizes **intrinsic parameters** (focal length, distortion), **extrinsic parameters** (camera positions/rotations), and **synchronization offsets** to minimize the **Re-projection Error** (measured in pixels, targeting < 0.1 pixels).

##### 中文：
動作捕捉校準嚴格分為兩個不同階段：

第一階段：靜態校準（地面 / L 型框架）

- 將 L 型框架放置於捕捉空間的原點。
- 它定義了**世界原點 (0,0,0)** 以及 X、Y、Z 軸的方向（通常 Z 軸向上）。
- 相機拍攝 L 型框架以建立全球參考座標系，並計算粗略的外參。

第二階段：動態校準（揮棒校準 / 殘差優化）

- 操作人員在整個捕捉空間內走動並揮舞 **T 型/Wand 校準棒**（已知精確長度的剛性桿，如 500mm）。
- 系統追蹤校準棒上的兩個標記點。由於兩點間的距離已知，軟體能透過所有相機視角三角化出 3D 位置。
- 演算法會迭代優化**內參**（焦距、畸變）、**外參**（相機位置/旋轉）以及同步時間偏移，以最小**化重投影誤差**（單位為像素，目標通常低於 0.1 像素）。

---

#### 6. The Critical Role of Synchronization (Time Calibration) | 同步的關鍵角色（時間校準）

##### English:
In multicamera Mocap systems, **temporal calibration** is as vital as spatial calibration. Cameras are hardware-synchronized via a dedicated sync cable or network packets. If camera A captures an image 1ms later than camera B, a marker moving at 5 m/s will have a 5mm positional error. During dynamic wanding, the system locks the shutter timing to ensure all cameras sample the exact moment of the flash, making time a fourth-dimensional calibration parameter.

##### 中文：
在多相機的動作捕捉系統中，**時間校準**與空間校準同等重要。相機之間透過專用同步線或網路封包進行硬體同步。若 A 相機比 B 相機晚 1ms 拍攝，一個以 5 m/s 移動的標記點將產生 5mm 的位置誤差。在動態揮棒校準過程中，系統會鎖定快門時序，確保所有相機取樣到完全一致的瞬間閃光，使時間成為第四維度的校準參數。

---

#### 7. Reprojection Error & Quality Metrics | 重投影誤差與品質指標

##### English:
The final metric is Reprojection Error (RPE). It is the pixel distance between the observed 2D marker in the image and the projected 3D marker back onto the image using the solved parameters.

$$\text{RPE} = \sqrt{(u_{observed} - u_{projected})^2 + (v_{observed} - v_{projected})^2}$$

- **Acceptable Standard**: < 0.5 pixels for general CV.
- **Mocap Standard**: < 0.10 pixels (often 0.05) to achieve millimeter accuracy in a 10m x 10m volume. The Mocap software displays a "Wand Residual" graph; a high residual indicates lens condensation, loose stands, or poor lighting.

##### 中文：
最終指標為**重投影誤差（RPE）**。它是影像中觀測到的 2D 標記位置，與利用求解參數反投影回影像的 3D 標記位置之間的像素距離。

$$\text{RPE} = \sqrt{(u_{觀測} - u_{投影})^2 + (v_{觀測} - v_{投影})^2}$$

- **一般可接受標準**：< 0.5 像素（通用電腦視覺）。
- **動作捕捉標準**：< 0.10 像素（通常要求 0.05）以在 10m x 10m 空間中達到毫米級精度。Mocap 軟體會顯示「校準棒殘差」曲線；殘差過高代表鏡頭起霧、支架鬆動或環境光源干擾。

---

#### 8. Thermal Calibration (Ambient Stability) | 熱校準（環境穩定性）

##### English:
High-end Mocap cameras (e.g., Vicon Vantage) have internal temperature sensors. As the camera warms up, the sensors expand, causing the focal length and principal point to drift. Most systems require a 30-60 minute **warm-up time** before performing the optical calibration. Some advanced systems utilize auto-calibration algorithms that continuously track static markers to compensate for thermal expansion in real-time.

##### 中文：
高階動作捕捉相機（如 Vicon Vantage）內建溫度感測器。當相機升溫時，感光元件與鏡筒會熱脹冷縮，導致焦距與主點偏移。大多數系統要求在進行光學校準前，需經過 **30 至 60 分鐘**的**暖機時間**。部分先進系統採用自動校準演算法，持續追蹤靜態標記點，即時補償熱膨脹效應。

---

#### 9. Conclusion | 結論

##### English:
Camera calibration bridges the gap between raw pixels and physical reality. While standard computer vision treats calibration as a one-off factory process using checkerboards, **Mocap calibration** is an interactive, high-frequency routine involving dynamic wands, static L-frames, hardware sync, and thermal management. Mastering both the static geometry (intrinsics/extrinsics) and dynamic correction (wand residuals, sync offsets) is the foundation for producing physically accurate, jitter-free tracking data in any 3D reconstruction application.

##### 中文：
相機校準是連接原始像素與物理現實的橋樑。標準電腦視覺將校準視為使用棋盤格的一次性出廠流程，但**動作捕捉校準則**是一種互動式、高頻率的日常例行程序，涉及動態校準棒、靜態 L 型框架、硬體同步與熱管理。同時精通靜態幾何（內/外參）與動態修正（校準棒殘差、同步偏移），是在任何 3D 重建應用中產生物理精確、無抖動追蹤數據的根基。