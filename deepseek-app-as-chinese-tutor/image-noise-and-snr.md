這是一份關於影像雜訊 (Image Noise) 與信噪比 (SNR) 的完整中英雙語詳解，涵蓋定義、分類、數學關係及實務影響。

---

### 1. 定義與基本概念 | Definition & Basic Concepts


影像雜訊 是指影像中非來自於被拍攝物體真實輻射或反射光的非預期訊號干擾。它是數位影像中像素值 (Pixel Values) 的隨機或規律性變異，會降低影像的清晰度、細節和色彩準確度。雜訊是限制影像系統性能的主要物理瓶頸。

Image noise refers to the unwanted signal interference in an image that does not originate from the actual radiation or reflected light of the subject being captured. It is the random or periodic variation of pixel values in a digital image, which degrades sharpness, detail, and color accuracy. Noise is the primary physical bottleneck that limits the performance of any imaging system.

---

### 2. 影像雜訊的主要類型 | Major Types of Image Noise

雜訊可依其統計特性分為以下幾類：

· 高斯雜訊 (Gaussian Noise)：又稱常態分佈雜訊，通常來自於感光元件的熱擾動 (Thermal Noise) 或電路讀出過程。其強度在統計上呈現常態分佈，幾乎存在於所有影像中。<br>
· 泊松雜訊 (Poisson Noise / Shot Noise)：又稱散粒雜訊，源於光量子 (Photon) 的粒子性。光子抵達感光元件的數量在時間和空間上具有隨機波動，其變異數等於訊號強度本身（即雜訊隨訊號增強而增加，但比例下降）。<br>
· 脈衝雜訊 (Impulse Noise / Salt-and-Pepper Noise)：又稱椒鹽雜訊，表現為隨機出現的全白或全黑像素點，通常由傳輸過程中的位元錯誤 (Bit errors)、壞點 (Dead Pixels) 或感測器放電異常所引起。<br>
· 固定圖樣雜訊 (Fixed Pattern Noise, FPN)：由像素間靈敏度不均勻（Photo-Response Non-Uniformity, PRNU）或暗電流差異所導致，在長時間曝光或高增益設定下尤為明顯。<br>


Noise can be classified by its statistical behavior:

· Gaussian Noise: Also known as normal-distribution noise, typically arising from thermal agitation in the sensor and read-out circuitry. Its intensity follows a normal distribution and is present to some degree in almost every image. <br>
· Poisson Noise (Shot Noise): Stemming from the particle nature of light (photons). The arrival of photons at the sensor fluctuates randomly over time and space. The variance of this noise equals the signal intensity itself (meaning noise increases with signal, but at a slower relative rate). <br>
· Impulse Noise (Salt-and-Pepper Noise): Appears as randomly occurring pure white or pure black pixels. It is usually caused by bit errors during transmission, dead pixels, or sensor discharge anomalies. <br>
· Fixed Pattern Noise (FPN): Caused by non-uniform sensitivity between pixels (Photo-Response Non-Uniformity, PRNU) or variations in dark current. It becomes notably prominent during long exposures or high-gain settings. <br>

---

### 3. 信噪比 (SNR) 的數學定義 | Mathematical Definition of SNR

信噪比 (SNR) 是衡量影像品質最關鍵的定量指標。它定義為有效訊號的強度（平均值）與雜訊的強度（標準差）之比率。

在影像處理中，若我們將影像視為二維矩陣，令像素強度的平均值為 $\mu$，雜訊的標準差（即變異數的平方根）為 $\sigma$，則：

$$\text{SNR} = \frac{\mu}{\sigma}$$

在工程應用上，通常使用分貝 (dB) 來表示：

$$\text{SNR}_{\text{dB}} = 20 \times \log_{10}\left(\frac{\mu}{\sigma}\right)$$


Signal-to-Noise Ratio (SNR) is the most critical quantitative metric for evaluating image quality. It is defined as the ratio of the effective signal intensity (mean value) to the intensity of the noise (standard deviation).

In image processing, treating the image as a 2D matrix—where \mu is the mean pixel intensity and \sigma is the standard deviation of the noise (the square root of the variance)—the formula is:

$$\text{SNR} = \frac{\mu}{\sigma}$$

In engineering practice, it is often expressed in decibels (dB):

$$\text{SNR}_{\text{dB}} = 20 \times \log_{10}\left(\frac{\mu}{\sigma}\right)$$

(Note: In optics, if measuring the power of light, 10 \times \log_{10} is used. For pixel amplitude/gray levels, 20 \times \log_{10} is the standard convention.)

---

### 4. 雜訊與 SNR 的深層相關性 | The Deep Correlation between Noise and SNR

此兩者的關係不僅僅是數學上的「分母與分子」，更體現了物理極限：

1. 泊松統計極限 (訊號相依雜訊)：因為光子的散粒雜訊遵循泊松分佈（變異數 = $\mu$），因此理想的 SNR 實際上等於訊號的平方根：
   $$\text{SNR}_{\text{理想}} = \frac{\mu}{\sqrt{\mu}} = \sqrt{\mu}$$


   這代表訊號越強（曝光越充足），SNR 越高，且 SNR 與雜訊的平方根成反比。要讓 SNR 提高一倍，你必須將收集到的光子數量（訊號）增加四倍。
2. 雜訊加成性 (Additive Nature)：總雜訊功率（變異數）是各類獨立雜訊源（熱雜訊、讀出雜訊、量化雜訊）的總和：
   $$\sigma_{\text{總}}^2 = \sigma_{\text{讀出}}^2 + \sigma_{\text{熱}}^2 + \sigma_{\text{光子}}^2 + \dots$$


   因此，SNR 由最強的雜訊源主宰。要提升 SNR，必須針對最主要的雜訊源進行抑制（例如降溫以減少熱雜訊）。
3. 動態範圍壓縮：當雜訊過大時，微弱的訊號差異（低對比細節）會被淹沒在雜訊基底 (Noise Floor) 之中。此時 SNR 低下意味著有效位元深度 (Effective Bit Depth) 的喪失，即便相機設定為 14-bit RAW，若 SNR 僅有 40 dB，實際上僅能解析約 6-7 個有效位元。


The relationship is not just a mathematical "denominator versus numerator"; it reflects profound physical limits:

1. Poisson Statistics Limit (Signal-Dependent Noise): Since photon shot noise follows a Poisson distribution (variance = \mu), the ideal SNR is actually the square root of the signal:
   $$\text{SNR}_{\text{ideal}} = \frac{\mu}{\sqrt{\mu}} = \sqrt{\mu}$$


   This means the stronger the signal (the more sufficient the exposure), the higher the SNR. Furthermore, SNR is inversely related to the square root of the noise. To double the SNR, you must increase the number of collected photons (signal) by a factor of four.
2. Additive Nature of Noise: The total noise power (variance) is the sum of the variances of independent noise sources (thermal, read-out, quantization):
   $$\sigma_{\text{Total}}^2 = \sigma_{\text{Read}}^2 + \sigma_{\text{Thermal}}^2 + \sigma_{\text{Photon}}^2 + \dots$$


   Consequently, the SNR is dominated by the strongest noise source. To improve SNR, one must specifically suppress the dominant noise source (e.g., cooling the sensor to reduce thermal noise).
3. Dynamic Range Compression: When noise is excessive, subtle signal differences (low-contrast details) are buried beneath the noise floor. A low SNR implies a loss of effective bit depth. Even if the camera is set to 14-bit RAW, if the SNR is only 40 dB, the system can only effectively resolve about 6–7 usable bits of data.

---

### 5. 實務上的權衡與改善策略 | Practical Trade-offs and Improvement Strategies

| 中文 (繁體) 策略             | English Strategy                                                                                                                               | 對 SNR 的影響 (Effect on SNR)                                                     |
| ---------------------- |------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| 延長曝光時間                 | Increase Exposure Time                                                                                                                         | 線性增加訊號 ($\mu$ 提高)，雜訊僅增加平方根 ($\sqrt{\mu}$)，因此 SNR 顯著提升。但需注意動態模糊 (Motion Blur)。 |
| Extended Exposure | Linear increase in signal ($\mu$), noise increases only by square root ($\sqrt{\mu}$), thus SNR improves significantly. Beware of motion blur. |
| 加大光圈 / 補光              | Widen Aperture / Add Light                                                                                                                     | 增加光子通量，直接提高 SNR。這是物理上最「乾淨」的作法。                                                |
| 提高 ISO / 類比增益          | Increase ISO / Analog Gain                                                                                                                     | 放大訊號的同時也等比放大讀出雜訊，SNR 通常維持不變或在高 ISO 時惡化（因熱雜訊被同步放大）。                            |
| 多幀疊加 (Frame Averaging) | Multi-frame Stacking                                                                                                                           | 若雜訊為隨機且均值為零，疊加 N 張影像可讓 SNR 提升 $\sqrt{N}$ 倍。                                   |
| 降溫 (冷卻感光元件)            | Sensor Cooling                                                                                                                                 | 減少暗電流與熱雜訊，降低 $\sigma$，進而 提升低光環境下的 SNR。                                        |


| |                                                                                                                                                                 | 
| ---------------------- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| Wider Aperture/More Light | Increases photon flux, directly boosting SNR. This is physically the "cleanest" method.                                                                         |
| Higher ISO/Analog Gain | Amplifies signal but amplifies read-out noise proportionally. SNR usually remains the same or degrades at extremely high ISOs (due to amplified thermal noise). |
| Multi-frame Stacking | If noise is random with zero mean, averaging N frames improves the SNR by a factor of \sqrt{N}.                                                                 |
| Sensor Cooling | Reduces dark current and thermal noise, lowering $\sigma$, effectively improving SNR in low-light conditions.                                                   |

---

### 6. 結論 | Conclusion

總而言之，影像雜訊是決定 SNR 的核心變量。SNR 不僅作為評判影像乾淨程度的指標，更是物理感光極限（光子散粒雜訊）與電子工程妥協（熱雜訊、增益）的綜合表現。在數位攝影與電腦視覺中，所有的去噪演算法 (Denoising Algorithms) 本質上都是在不顯著破壞訊號 \mu 的前提下，試圖估計並減去雜訊 \sigma，從而「恢復」出更高的有效 SNR。

In summary, image noise is the core determinant of SNR. SNR is not merely an indicator of how "clean" an image looks; it is the comprehensive manifestation of the physical limits of light sensing (photon shot noise) and electronic engineering trade-offs (thermal noise, gain). In digital photography and computer vision, all denoising algorithms are, in essence, attempts to estimate and subtract the noise \sigma without significantly destroying the signal \mu, thereby "restoring" a higher effective SNR for the end-user.