### Part 1: Prompt Repetition

**Original prompt:**  
> "From first principles, explain the mathematics behind quantization. In addition, show a code example of quantization. List the math topics to study in order to develop intuition for quantization."

---

### Part 2: Essence in Concise, Colloquial Chinese

> 從第一性原理推導量化的數學本質：用有限個離散整數（如 256 個）去逼近無限連續的 32 位浮點數。核心問題是找到最優的線性映射（縮放因子 \( \Delta \) 和零點 \( Z \)），使量化前後的誤差最小。並附上程式碼範例，以及建立直覺所需的數學主題清單。

---

### Part 3: Bilingual Response

#### CHINESE SECTION

**摘要**

從第一性原理看，量化本質上是一個**有損壓縮（lossy compression）** 問題。神經網路的權重 \( R \) 原本是 32 位浮點數（連續、高精度），我們想用 \( n \) 位整數（離散、低精度）來表示它 [1]。量化的數學目標是：在給定的整數範圍（如 0 ~ 255）內，找到一個**線性映射（仿射變換）** \( f: R \mapsto Q \)，使得量化後的反推值 \( \tilde{R} \) 與原始值 \( R \) 之間的誤差最小化 [2]。這個線性映射由兩個參數決定：**縮放因子（Scale, \( \Delta \)）** 和**零點（Zero Point, \( Z \)）** [3]。透過極值對應原則（Min-Max），可以解析地求出這兩個參數，並保證均勻量化的誤差上界為 \( \Delta/2 \)。以下從最基礎的公式開始完整推導，並提供 NumPy 與 PyTorch 的實作範例。

---

**細節**

**1. 問題定義（第一性原理）**

假設我們有一組原始浮點數（權重或激活值），記為集合 \( \mathcal{R} \)。我們要將其壓縮到 \( n \) 位整數（即 \( 2^n \) 個離散級別），記為集合 \( \mathcal{Q} \)。

- 浮點範圍：\( R \in [\beta, \alpha] \)，其中 \( \beta = \min(\mathcal{R}) \)，\( \alpha = \max(\mathcal{R}) \)。
- 整數範圍：\( Q \in [q_{min}, q_{max}] \)。對於 \( n \) 位無符號整數，\( q_{min} = 0 \)，\( q_{max} = 2^n - 1 \)（例如 INT8 是 0 ~ 255）[1]。

量化目標：找到一個映射 \( f: \mathcal{R} \mapsto \mathcal{Q} \)，使得**平均量化誤差**最小。在**均勻量化（Uniform Quantization）** 中，我們限定 \( f \) 為**線性映射**（仿射變換），因為線性映射最簡單且計算最快。

**2. 推導縮放因子（Scale, \( \Delta \)）與零點（Zero Point, \( Z \)）**

我們定義線性映射的通用公式：

\[
Q = \text{round}\left( \frac{R}{\Delta} + Z \right)
\]

其反量化（Dequantization）公式為：

\[
\tilde{R} = \Delta \cdot (Q - Z)
\]

其中 \( \Delta \) 是縮放因子，\( Z \) 是零點。我們要解出這兩個參數。

- **極端對應原則**：為了最大化利用整數範圍，我們讓浮點的最小值 \( \beta \) 映射到整數的最小值 \( q_{min} \)，浮點的最大值 \( \alpha \) 映射到整數的最大值 \( q_{max} \) [3]。

由此建立方程組：
\[
\begin{cases}
q_{min} = \frac{\beta}{\Delta} + Z \\
q_{max} = \frac{\alpha}{\Delta} + Z
\end{cases}
\]

兩式相減消去 \( Z \)：
\[
q_{max} - q_{min} = \frac{\alpha - \beta}{\Delta}
\]

解得**縮放因子（Scale）**：

\[
\Delta = \frac{\alpha - \beta}{q_{max} - q_{min}}
\]

這個公式的物理意義：**縮放因子等於浮點數的跨度除以整數的跨度**，即每個整數步長代表多少浮點數值。

將 \( \Delta \) 代入第一個方程，解得**零點（Zero Point）**：

\[
Z = q_{min} - \frac{\beta}{\Delta}
\]

因為 \( Z \) 必須是整數，所以最後取四捨五入：\( Z = \text{round}(q_{min} - \frac{\beta}{\Delta}) \)。

**3. 量化誤差的數學上界**

在均勻量化中，只要原始值不超出 \( [\beta, \alpha] \) 範圍，**四捨五入**保證量化誤差 \( \epsilon = R - \tilde{R} \) 的絕對值不超過半個步長 [2]：

\[
|\epsilon| \leq \frac{\Delta}{2}
\]

這是「有損壓縮」的根本代價：原始值中低於 \( \Delta/2 \) 的細微差異將被永久丟失（即碰撞）。

**4. 對稱量化（Symmetric）與非對稱量化（Asymmetric）的數學區別**

- **對稱量化（Symmetric）**：強制 \( Z = 0 \)。此時 \( \Delta = \frac{\max(|\alpha|, |\beta|)}{q_{max}} \)。這適用於權重（分佈通常對稱於零）[1]。
- **非對稱量化（Asymmetric）**：如上推導，\( Z \neq 0 \)。這適用於激活值（如 ReLU 後的輸出，均為非負），能更充分地利用整數範圍 [2]。

**5. 程式碼範例（NumPy 與 PyTorch）**

```python
# === NumPy 範例（從頭實現非對稱量化） ===
import numpy as np

# 1. 模擬浮點權重
weights = np.array([0.2, -0.5, 0.8, -0.1, 0.6], dtype=np.float32)
print(f"原始浮點權重: {weights}")

# 2. 量化參數設定（目標：8 位無符號整數，範圍 0~255）
q_min, q_max = 0, 255
n_bits = 8

# 3. 計算 Min-Max 邊界
beta = np.min(weights)   # -0.5
alpha = np.max(weights)  # 0.8

# 4. 計算縮放因子 (Scale) 和零點 (Zero Point)
delta = (alpha - beta) / (q_max - q_min)  # (0.8 - (-0.5)) / 255 ≈ 0.005098
zero_point = int(round(q_min - beta / delta))  # round(0 - (-0.5)/0.005098) ≈ 98

print(f"縮放因子 (delta): {delta:.6f}")
print(f"零點 (zero_point): {zero_point}")

# 5. 量化（前向）
weights_q = np.round(weights / delta + zero_point).astype(np.int32)
# 確保數值不超出邊界
weights_q = np.clip(weights_q, q_min, q_max)
print(f"量化後整數值: {weights_q}")

# 6. 反量化（推論時還原）
weights_deq = (weights_q - zero_point) * delta
print(f"反量化還原值: {weights_deq}")

# 7. 計算量化誤差
error = np.abs(weights - weights_deq)
print(f"絕對誤差: {error}")
print(f"最大誤差: {np.max(error):.6f} (理論上界 delta/2 ≈ {delta/2:.6f})")

# === PyTorch 範例（使用框架內建函數） ===
import torch

# 模擬張量
weights_torch = torch.tensor([0.2, -0.5, 0.8, -0.1, 0.6], dtype=torch.float32)

# 使用 PyTorch 的量化函數（per-tensor 非對稱量化）
# 注意：torch.quantization 的 API 通常需要先定義觀察器，但這裡用低階函數展示原理
scale, zero_point = torch.quantization.choose_qparams(weights_torch, dtype=torch.quint8)
print(f"\nPyTorch 自動計算的 scale: {scale}, zero_point: {zero_point}")

# 手動量化（使用 torch.quantize_per_tensor）
quantized_tensor = torch.quantize_per_tensor(weights_torch, scale, zero_point, dtype=torch.quint8)
dequantized_tensor = quantized_tensor.dequantize()

print(f"PyTorch 量化後張量: {quantized_tensor}")
print(f"PyTorch 反量化還原: {dequantized_tensor}")
```

**6. 培養直覺所需掌握的數學主題**

| 數學主題       | 具體內容                             | 與量化的關聯                                                 |
| :------------- | :----------------------------------- | :----------------------------------------------------------- |
| **線性代數**   | 線性映射、向量空間、範數（L2, L∞）   | 理解量化本質上是一個從連續空間到離散空間的線性變換，並使用 L∞ 範數來衡量最大誤差 [1]。 |
| **數值分析**   | 近似理論、四捨五入誤差、誤差上界分析 | 理解 \( \Delta/2 \) 誤差界的來源，以及如何分析不同量化策略對數值精度的影響 [2]。 |
| **機率與統計** | 資料分佈、離群值、期望值             | 理解 Min-Max 量化對離群值敏感（會被極端值拉偏），以及百分位數量化（Percentile Quantization）如何透過統計方法緩解此問題 [3]。 |
| **最優化理論** | 目標函數、最小化誤差                 | 理解更先進的量化方法（如 KL 散度最小化）是如何透過優化來選擇最佳的縮放因子，而不僅是 Min-Max。 |

---

#### ENGLISH SECTION

**Abstract**

From first principles, quantization is fundamentally a **lossy compression** problem. Neural network weights \( R \) are originally 32-bit floating-point numbers (continuous, high-precision). We want to represent them with \( n \)-bit integers (discrete, low-precision) [1]. The mathematical goal is to find a **linear mapping (affine transformation)** \( f: R \mapsto Q \) that minimizes the error between the original value \( R \) and its dequantized approximation \( \tilde{R} \), given a fixed integer range (e.g., 0 to 255) [2]. This linear mapping is defined by two parameters: the **Scale factor (\( \Delta \))** and the **Zero Point (\( Z \))** [3]. Using the Min-Max principle, these parameters can be derived analytically, guaranteeing an error bound of \( \Delta/2 \) for uniform quantization. The complete derivation, along with NumPy and PyTorch code examples, is provided below.

---

**Details**

**1. Problem Definition (First Principles)**

Assume we have a set of original floating-point values (weights or activations), denoted as set \( \mathcal{R} \). We want to compress them into \( n \)-bit integers (i.e., \( 2^n \) discrete levels), denoted as set \( \mathcal{Q} \).

- Floating-point range: \( R \in [\beta, \alpha] \), where \( \beta = \min(\mathcal{R}) \), \( \alpha = \max(\mathcal{R}) \).
- Integer range: \( Q \in [q_{min}, q_{max}] \). For \( n \)-bit unsigned integers, \( q_{min} = 0 \), \( q_{max} = 2^n - 1 \) (e.g., INT8 is 0 to 255) [1].

The quantization goal is to find a mapping \( f: \mathcal{R} \mapsto \mathcal{Q} \) that minimizes the **average quantization error**. In **Uniform Quantization**, we restrict \( f \) to be a **linear mapping** (affine transformation) because it is the simplest and fastest to compute.

**2. Deriving the Scale Factor (\( \Delta \)) and Zero Point (\( Z \))**

We define the general formula for the linear mapping:

\[
Q = \text{round}\left( \frac{R}{\Delta} + Z \right)
\]

The corresponding dequantization formula is:

\[
\tilde{R} = \Delta \cdot (Q - Z)
\]

Where \( \Delta \) is the scale factor and \( Z \) is the zero point. We need to solve for these two parameters.

- **Extreme Correspondence Principle**: To maximize the utilization of the integer range, we map the floating-point minimum \( \beta \) to the integer minimum \( q_{min} \), and the floating-point maximum \( \alpha \) to the integer maximum \( q_{max} \) [3].

This yields a system of equations:
\[
\begin{cases}
q_{min} = \frac{\beta}{\Delta} + Z \\
q_{max} = \frac{\alpha}{\Delta} + Z
\end{cases}
\]

Subtracting the first equation from the second eliminates \( Z \):
\[
q_{max} - q_{min} = \frac{\alpha - \beta}{\Delta}
\]

Solving for the **Scale factor (\( \Delta \))**:

\[
\Delta = \frac{\alpha - \beta}{q_{max} - q_{min}}
\]

The physical meaning is: **The scale factor equals the floating-point span divided by the integer span**—i.e., how much floating-point value each integer step represents.

Substituting \( \Delta \) back into the first equation, we solve for the **Zero Point (\( Z \))**:

\[
Z = q_{min} - \frac{\beta}{\Delta}
\]

Since \( Z \) must be an integer, we take the rounding: \( Z = \text{round}(q_{min} - \frac{\beta}{\Delta}) \).

**3. Mathematical Upper Bound of Quantization Error**

In uniform quantization, as long as the original values do not exceed the \( [\beta, \alpha] \) range, the **rounding operation** guarantees that the absolute quantization error \( \epsilon = R - \tilde{R} \) is bounded by half the step size [2]:

\[
|\epsilon| \leq \frac{\Delta}{2}
\]

This is the fundamental cost of "lossy" compression: subtle differences smaller than \( \Delta/2 \) are permanently lost (collisions).

**4. Mathematical Distinction: Symmetric vs. Asymmetric Quantization**

- **Symmetric Quantization**: Forces \( Z = 0 \). Then \( \Delta = \frac{\max(|\alpha|, |\beta|)}{q_{max}} \). Suitable for weights (distributions typically centered around zero) [1].
- **Asymmetric Quantization**: As derived above, \( Z \neq 0 \). Suitable for activations (e.g., ReLU outputs, all non-negative), which allows more efficient use of the integer range [2].

**5. Code Example (NumPy and PyTorch)**

```python
# === NumPy Example (Asymmetric Quantization from Scratch) ===
import numpy as np

# 1. Simulate floating-point weights
weights = np.array([0.2, -0.5, 0.8, -0.1, 0.6], dtype=np.float32)
print(f"Original floating-point weights: {weights}")

# 2. Quantization parameters (target: 8-bit unsigned int, range 0~255)
q_min, q_max = 0, 255
n_bits = 8

# 3. Compute Min-Max boundaries
beta = np.min(weights)   # -0.5
alpha = np.max(weights)  # 0.8

# 4. Compute Scale and Zero Point
delta = (alpha - beta) / (q_max - q_min)  # (0.8 - (-0.5)) / 255 ≈ 0.005098
zero_point = int(round(q_min - beta / delta))  # round(0 - (-0.5)/0.005098) ≈ 98

print(f"Scale (delta): {delta:.6f}")
print(f"Zero Point: {zero_point}")

# 5. Quantize (forward)
weights_q = np.round(weights / delta + zero_point).astype(np.int32)
# Clip to ensure values stay within bounds
weights_q = np.clip(weights_q, q_min, q_max)
print(f"Quantized integer values: {weights_q}")

# 6. Dequantize (for inference)
weights_deq = (weights_q - zero_point) * delta
print(f"Dequantized values: {weights_deq}")

# 7. Calculate quantization error
error = np.abs(weights - weights_deq)
print(f"Absolute errors: {error}")
print(f"Max error: {np.max(error):.6f} (Theoretical bound delta/2 ≈ {delta/2:.6f})")

# === PyTorch Example (Using built-in functions) ===
import torch

# Simulate tensor
weights_torch = torch.tensor([0.2, -0.5, 0.8, -0.1, 0.6], dtype=torch.float32)

# PyTorch's quantization functions (low-level API to demonstrate principle)
scale, zero_point = torch.quantization.choose_qparams(weights_torch, dtype=torch.quint8)
print(f"\nPyTorch calculated scale: {scale}, zero_point: {zero_point}")

# Manual quantization using torch.quantize_per_tensor
quantized_tensor = torch.quantize_per_tensor(weights_torch, scale, zero_point, dtype=torch.quint8)
dequantized_tensor = quantized_tensor.dequantize()

print(f"PyTorch quantized tensor: {quantized_tensor}")
print(f"PyTorch dequantized tensor: {dequantized_tensor}")
```

**6. Math Topics to Study for Intuition**

| Math Topic                   | Specific Content                                            | Connection to Quantization                                   |
| :--------------------------- | :---------------------------------------------------------- | :----------------------------------------------------------- |
| **Linear Algebra**           | Linear mappings, vector spaces, norms (L2, L∞)              | Understand that quantization is a linear transformation from continuous to discrete space, and use the L∞ norm to measure maximum error [1]. |
| **Numerical Analysis**       | Approximation theory, rounding errors, error bound analysis | Understand the origin of the \( \Delta/2 \) error bound and how different quantization strategies affect numerical precision [2]. |
| **Probability & Statistics** | Data distributions, outliers, expectation                   | Understand why Min-Max quantization is sensitive to outliers (skewed by extreme values) and how percentile-based methods mitigate this [3]. |
| **Optimization Theory**      | Objective functions, error minimization                     | Understand advanced methods like KL-divergence minimization which *optimize* the scale factor rather than just using Min-Max. |

---

### Part 4: Acronyms Defined

| Acronym  | Full Name                   | Chinese Translation |
| :------- | :-------------------------- | :------------------ |
| **FP32** | 32-bit Floating Point       | 32 位浮點數         |
| **INT8** | 8-bit Integer               | 8 位整數            |
| **MSE**  | Mean Squared Error          | 均方誤差            |
| **KL**   | Kullback-Leibler Divergence | KL 散度（相對熵）   |

---

### Part 5: References

[1] NVIDIA Developer. (2024). Introduction to Quantization. Available at: https://developer.nvidia.com/blog/introduction-to-quantization/

[2] Hugging Face. (2025). Quantization in Transformers. Available at: https://huggingface.co/docs/transformers/v4.41.3/en/performance#quantization

[3] TensorFlow. (n.d.). Quantization: Optimizing Model Performance. Available at: https://www.tensorflow.org/model_optimization/guide/quantization
