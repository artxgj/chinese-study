## Timecode、SMPTE Timecode 與 Linear Timecode 完整中英雙語詳解

## Complete Bilingual Explanation of Timecode, SMPTE Timecode, and Linear Timecode

---

### 1. 核心概念總覽 (Core Concept Overview)

###### 中文

- Timecode（時間碼）：一個廣義的總稱，指任何用於標記影音素材時間位置的數值標籤（通常格式為 時:分:秒:幀）。
- SMPTE Timecode（SMPTE 時間碼）：由美國電影與電視工程師協會制定的國際標準（SMPTE 12M），定義時間碼的格式、幀率運算及丟幀規則，是專業影視工業的共通語言。
- Linear Timecode（LTC／線性時間碼）：SMPTE 時間碼的其中一種實體信號傳輸形式（載體），以數位音頻信號的型態，透過平衡式類比音頻線或 AES/EBU 數位音頻線進行傳輸。

##### English

- Timecode：A broad umbrella term for any numeric label that marks the temporal position of audio/video material (typically formatted as HH:MM:SS:FF).
- SMPTE Timecode：An international standard (SMPTE 12M) defined by the Society of Motion Picture and Television Engineers, specifying the format, frame‑rate arithmetic, and drop‑frame rules – it is the common language of the professional media industry.
- Linear Timecode (LTC)：One of the physical transport mechanisms (carriers) for SMPTE timecode, encoded as an audio‑frequency digital signal and transmitted via balanced analog audio cables or AES/EBU digital audio lines.

---

### 2. 詳細定義與特性 (Detailed Definitions & Characteristics)

#### A. Timecode（時間碼）— 通用術語 (Generic Term)

##### 中文
時間碼是一個泛稱，本質是為每一格畫面或音頻樣本賦予一個獨特的時間位址。它不限定於特定幀率，也不限定於影視產業（如 MIDI Timecode 用於音樂）。核心角色是作為編輯與同步的「量尺」。

##### English
Timecode is a generic term; its essence is to assign a unique temporal address to each frame or audio sample. It is not restricted to any specific frame rate or to the film/TV industry (e.g., MIDI Timecode is used in music). Its core role is to serve as a “ruler” for editing and synchronisation.

---

#### B. SMPTE Timecode（SMPTE 時間碼）— 業界標準 (Industry Standard)

##### 中文
這是 SMPTE 組織制定的標準化時間碼（規範書 SMPTE 12M）。它定義了嚴格的二進制碼格式，並針對 29.97fps 與 59.94fps 等降幀率制定了 Drop‑Frame（丟幀） 與 Non‑Drop‑Frame（非丟幀） 機制，以確保時間碼能與真實的時鐘（秒）對齊。可視為行業的「文法與字典」。

##### English
This is the standardised timecode defined by SMPTE (specification SMPTE 12M). It specifies a strict binary bit structure and introduces Drop‑Frame and Non‑Drop‑Frame mechanisms for fractional frame rates like 29.97fps and 59.94fps, ensuring that the timecode aligns with real‑world clock time. It can be thought of as the “grammar and dictionary” of the industry.

---

#### C. Linear Timecode (LTC) — 實體傳輸形式 (Physical Carrier)

##### 中文
LTC 是 SMPTE 時間碼的「載波信號」。它將二進制的時間碼數據轉換為雙相制標記（Biphase Mark）的方波音頻信號（頻率約 1.2kHz 至 2.4kHz）。LTC 必須在設備播放（Playback）狀態下才能讀取；若磁帶或硬碟暫停（Still），信號停止，LTC 無法輸出。它是時間碼的「聲波」載體。

##### English
LTC is the “carrier signal” for SMPTE timecode. It encodes the binary data into a Biphase Mark square‑wave audio signal (frequency roughly 1.2 kHz to 2.4 kHz). Crucially, LTC requires the tape or transport to be moving at play speed to be decoded; during still/pause the signal ceases and LTC becomes unreadable. It is the “sound wave” that carries the timecode.

---

### 3. 三者關係對照表 (Comparison Matrix)

| 項目 (Item) | Timecode (時間碼)   | SMPTE Timecode (SMPTE 時間碼)  | Linear Timecode (LTC)           |
| --------- | ---------------- | --------------------------- | ------------------------------- |
| 中文定位      | 最高層級的抽象概念        | 具體的標準規範（內容）                 | 具體的傳輸介質（載體）                     |
| 英文定位      | Abstract Concept | Content / Standard (What)   | Physical Carrier / Medium (How) |
| 主要功能      | 標示媒體檔案的絕對位置      | 定義幀率、丟幀規則與格式                | 將 SMPTE 碼轉為可傳輸的音頻信號             |
| 讀取條件      | 靜態數據（如檔案屬性）      | 需靠 LTC 或 VITC 傳輸才能存在        | 必須在播放/運行中才能產生信號                 |
| 傳輸媒介      | 無（僅為數值）          | 無（僅為協議）                     | 類比音頻線、數位音頻線 (AES/EBU)           |
| 對應形式      | 例如：Frame 12345   | 例如：01:02:03:04 (Drop‑Frame) | 例如：時碼音軌上的方波聲音                   |

<br>

| | | |                                                     |
| - | - | - |-----------------------------------------------------|
| English Positioning | Highest‑level abstraction | Concrete standard (the what) | Concrete transmission medium (the how)              |
| Primary Function | Marks absolute position in media | Defines frame rate, drop‑frame rules, and bit structure | Converts SMPTE code into transmittable audio signal |
| Reading Condition | Static data (e.g., file metadata) | Exists only when carried by LTC or VITC | Must be in playback/motion to generate signal       |
| Transmission Medium | None (just numeric value) | None (just protocol) | Analog audio cables, digital audio (AES/EBU)        |
| Example Form | e.g., Frame 12345 | e.g., 01:02:03:04 (Drop‑Frame) | e.g., square‑wave sound on a timecode track         |
---

### 4. 進階補充：VITC 與 LTC 的對比 (Advanced: VITC vs. LTC)

##### 中文
既然 LTC 無法在暫停時讀取，SMPTE 同時制定了 Vertical Interval Timecode (VITC)。VITC 將時間碼寫入影像信號的垂直遮沒間期（VBI），因此即使畫面暫停，讀取頭依然能讀取該格畫面的時間碼。這是 LTC 最大的局限所在。

- LTC：適合高速傳輸與外部設備同步（如錄音機與攝影機對時），但無法在靜幀工作。
- VITC：適合後期剪輯逐格校對，但易受影像信號干擾。

##### English
Since LTC cannot be read during still/pause, SMPTE also defined Vertical Interval Timecode (VITC). VITC embeds the timecode into the vertical blanking interval (VBI) of the video signal, so even when the picture is frozen, the reader can still extract the timecode of that exact frame – this is the key limitation that LTC suffers from.

- LTC：Ideal for high‑speed transmission and synchronising external devices (e.g., sync between audio recorders and cameras), but unusable during still frames.
- VITC：Preferred for frame‑accurate editing in post‑production, though more susceptible to interference from the video signal.

---

### 5. 實際應用情境 (Real‑world Application)

##### 中文

- 拍攝現場：使用 SMPTE 時間碼 統整多機位攝影機與錄音機的時鐘。
- 動作：錄音機將 SMPTE 規則寫入信號，透過 LTC 格式經由音頻線發送給攝影機。
- 後期製作：剪輯師導入素材時，軟體讀取檔案中內嵌的 SMPTE 元數據（非 LTC）進行同步。若使用老式錄影帶，剪輯師暫停畫面時需依賴 VITC 來判讀當前位置，而非 LTC。

##### English

- On set：SMPTE timecode is used to synchronise the clocks of multiple cameras and audio recorders.
- Action：The audio recorder encodes the SMPTE rules into a signal and sends it to the cameras via LTC over analogue audio cables.
- Post‑production：When editors import the media, the NLE software reads the embedded SMPTE metadata (not LTC) for auto‑syncing. For legacy tape workflows, editors rely on VITC (not LTC) to read the timecode when the tape is paused.

---

### 6. 總結 (Summary)

##### 中文
Timecode 是「語言的概念」，SMPTE 是「正式的文法與字典」，而 LTC 則是「念出聲音的聲波」。
沒有 LTC，SMPTE 碼無法在設備間動態傳遞；沒有 SMPTE 標準，LTC 僅是無意義的雜音；而 Timecode 則是這一切的總稱。

##### English
Timecode is the conceptual “language” – a generic label for position.

SMPTE timecode is the formal “grammar and dictionary” – the standard that defines frame rates, drop‑frame rules, and bit structure.

Linear Timecode (LTC) is the “sound wave” that speaks that language – the physical audio signal that carries the SMPTE data.

Without LTC, SMPTE codes cannot be dynamically transmitted between devices; without the SMPTE standard, LTC is just meaningless noise; and Timecode is the overarching term that encompasses all of these.
