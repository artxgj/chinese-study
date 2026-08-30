## 1. Prompt Repetition & Chinese Essence

**Original Prompt (English):**  
> Someone on X posted a tweet with this question "Why KV cache stores K and V vectors but never Q?" What is Q? Describe and explain LLM's KV cache. What are the contents of the K, V, and Q vectors. How often is the KV cache refreshed? What kind of storage is used for KV cache?

**Chinese Essence (简明口语化):**  
> X上有人问：“为什么KV缓存只存K和V向量，从来不存Q？” 你想知道Q到底是什么，以及LLM的KV缓存完整机制：K、V、Q向量里装了什么？缓存多久刷新一次？用什么存储介质？

---

## 2. Bilingual Response

### 中文部分 (Chinese Section)

**摘要**  
Q（Query，查询向量）是**当前词用来“寻找”历史信息的临时向量**——它只在生成当前词时计算一次，用完即弃，因此从不缓存。KV缓存存储的是历史所有词元的K（Key，键）和V（Value，值）向量，目的是避免自回归生成中每步重复计算整个历史序列（将O(N²)的复杂度降为O(N)的逐词追加）。K、V、Q都是高维浮点数向量（如FP16），承载经过多层Transformer变换后的**潜在语义特征**：K充当“索引/地址”，V充当“内容/取值”，Q充当“查询条件”。缓存每生成一个新词元就追加一次，每次全新请求开始时清空重置，主要存储在**GPU的高带宽显存（HBM）** 中，极端长上下文可分层溢出到CPU内存或SSD [1][2][3]。

**详细说明**

**1. 什么是Q（Query）？——瞬时的“查询者”**  
Q是“Query”的缩写，中文叫**查询向量**。在注意力机制中，Q代表当前处理位置“想要寻找什么信息”。  
- **在Prefill（预填充）阶段**：所有输入词元同时生成各自的Q，用于计算它们彼此之间的注意力分数。  
- **在Decode（自回归解码）阶段**：只有最新生成的**那一个词元**会生成Q（记作 $q_t$）。这个 $q_t$ 会去和所有历史K做点积，计算“当前词”与“每个历史词”的相关性。  
- **关键特性**：Q是**完全瞬时的**——它只服务于当前这一个时间步，用完后就再也用不到了。每一步都会产生一个全新的Q，和历史上的Q没有任何关系。因此，缓存Q是纯粹的显存浪费 [1][2]。

**2. 为什么缓存K和V，却从来不存Q？——核心机制**  

- **历史K为什么需要缓存**：在生成当前词时，$q_t$ 需要和**每一个历史词**的K做点积（$q_t \cdot K_{1:t-1}$）。如果没有缓存，每步都要把历史所有词重新输入模型再算一遍K，代价极高。  
- **历史V为什么需要缓存**：算完注意力分数后，需要用这些分数去**加权求和**历史所有V（$A \cdot V_{1:t-1}$）来得到当前输出。没有缓存同样要重算。  
- **Q为什么不缓存**：历史Q在算完历史注意力后就已经完成了使命。在生成新词时，模型用的是**当前的新Q**，而不是历史的旧Q。历史的旧Q永远不会再被任何未来的步骤使用。  
- **类比**：就像查字典时，你每次查一个词（Q）都会去翻目录（K）找到页码，然后翻到内容页（V）读取解释。目录（K）和内容页（V）要一直放在桌上反复翻阅，但你写完笔记后那张“查询纸条”（Q）就可以扔掉了 [2]。

**3. K、V、Q向量里到底装了什么？**  
- **不是原始文字**，而是**高维潜在语义特征（Latent Representations）**——即该词在经过多层Transformer神经网络变换后，在数学空间中的投影坐标。  
- **数据类型**：通常是**浮点数**（FP16、BF16或FP8），维度为 $d_k$（例如Llama 3 70B中，每头维度为128）。  
- **三者的角色分工**：  
  - **K（Key，键）**：代表该词“在什么位置、具备哪些可匹配的特征”。用于回答“这个历史词和我的查询有多相关？”  
  - **V（Value，值）**：代表该词“携带的实际语义内容”。一旦通过K匹配上，就从V中提取具体信息用于当前输出。  
  - **Q（Query，查询）**：代表当前词“想找什么类型的特征”。它是计算出的动态需求，不存储 [1][2]。

**4. KV缓存多久刷新一次？**  
- **追加频率**：生成**每一个新词元（Token）** 时，缓存就会**追加**该新词对应的K和V向量（即在缓存序列尾部增加一组新向量）。  
- **重置频率**：每当开始处理**一个全新的独立用户请求（新的Prompt/新会话）** 时，缓存会被**完全清空（Reset）**。  
- **注意**：同一个请求内的多轮对话（如把Chat History拼接在Prompt里），K和V是在同一个缓存序列上**连续追加**，不会中途重置。缓存从不修改旧值，只有“追加”和“清空”两种操作 [2][3]。

**5. 用什么存储介质？**  
- **第一优先级（绝大多数情况）**：**GPU的HBM（高带宽显存）**，例如NVIDIA H100的80GB HBM3。这是因为Decode每一步都需以极高速度**读取整个缓存**（内存带宽是瓶颈），HBM高达3.35 TB/s的带宽是唯一能喂饱GPU计算核心的选择 [2][3]。  
- **缓存溢出策略（超长上下文，如100万Token）**：当HBM装不下时，推理引擎（如vLLM、Hugging Face TGI）会启用**分层存储（Tiered Storage）**：热数据（最近使用的）留在HBM，冷数据换出到 **CPU DRAM（系统内存）**，极端情况下甚至换出到 **NVMe SSD**。但换出到SSD会使延迟从微秒级骤增至毫秒级 [3]。  
- **PagedAttention（vLLM核心优化）**：把KV缓存切成固定大小的“页”存储在HBM中，像操作系统管理虚拟内存一样管理，减少碎片，提高显存利用率——但存储介质依然是HBM [2]。

---

### English Section

**Abstract**  
Q (Query) is a **temporary vector used by the current token to “look up” historical information** – it is computed once, used immediately, and discarded, so it is never cached. The KV cache stores the Key (K) and Value (V) vectors of all historical tokens to avoid recomputing the entire history at every autoregressive step (reducing overall complexity from $O(N^2)$ to $O(N)$ per-step append). K, V, and Q are all high-dimensional floating-point vectors (e.g., FP16) carrying **latent semantic features** after multi-layer Transformer transformation: K acts as an “index/address,” V as “content/value,” and Q as the “query condition.” The cache appends new entries per generated token, resets at the start of each new request, and is primarily stored in **GPU HBM (High-Bandwidth Memory)** , with tiered offloading to CPU DRAM or SSD for extreme long contexts [1][2][3].

**Detailed Explanation**

**1. What is Q (Query)? – The Ephemeral “Searcher”**  
Q stands for "Query" vector. In the attention mechanism, Q represents “what information the current position is looking for.”  
- **During Prefill phase**: All input tokens generate their Q simultaneously to compute attention scores between each other.  
- **During Decode phase (autoregressive)**: Only the **most recently generated token** computes a Q (denoted $q_t$). This $q_t$ performs dot-product with all historical Ks to compute the relevance of “current token” vs. “each historical token.”  
- **Key characteristic**: Q is **completely ephemeral** – it serves only the current time step and is never needed again. Each step produces a brand-new Q unrelated to historical Qs. Caching Q would be pure memory waste [1][2].

**2. Why Cache K and V but Never Q? – Core Mechanism**  
- **Why historical K must be cached**: When generating the current token, $q_t$ must perform dot-product with **every historical token’s K** ($q_t \cdot K_{1:t-1}$). Without caching, each step would require re-processing the entire history through the model to recompute all Ks – extremely expensive.  
- **Why historical V must be cached**: After computing attention scores, the model uses them to perform a **weighted sum** over all historical Vs ($A \cdot V_{1:t-1}$) to produce the current output. Without caching, this also requires recomputation.  
- **Why Q is not cached**: Historical Qs completed their mission when they were used to compute past attention. When generating a new token, the model uses the **current new Q**, not the historical ones. Historical Qs are never reused in any future step.  
- **Analogy**: Like looking up a dictionary. Each time you look up a word (Q), you check the index (K) to find the page number, then flip to the content page (V) to read the definition. The index (K) and content pages (V) stay on the desk for repeated use, but your scratch note with the lookup word (Q) is thrown away after you write down the answer [2].

**3. What’s Actually Inside the K, V, and Q Vectors?**  
- **Not raw words**, but **high-dimensional latent semantic features** – i.e., mathematical projections of the token after transformation through multiple Transformer layers.  
- **Data type**: Typically **floating-point numbers** (FP16, BF16, or FP8), with dimension $d_k$ (e.g., per-head dimension 128 in Llama 3 70B).  
- **Role differentiation**:  
  - **K (Key)**: Represents “what features this token has for matching.” Answers: “How relevant is this historical token to my query?”  
  - **V (Value)**: Represents “the actual semantic content carried by this token.” Once matched via K, V is used to extract specific information for the current output.  
  - **Q (Query)**: Represents “what kind of features the current token is looking for.” It is a computed dynamic need and is never stored [1][2].

**4. How Often is the KV Cache Refreshed?**  
- **Append frequency**: For **every newly generated token**, the cache **appends** the new token’s K and V vectors (adding a new set of vectors to the end of the cache sequence).  
- **Reset frequency**: At the start of **each brand-new independent user request (new Prompt / new session)** , the cache is **completely cleared (Reset)**.  
- **Note**: For multi-turn conversations within the same request (where Chat History is appended to the prompt), K and V are **continuously appended** on the same cache sequence – never reset mid-request. The cache never modifies old values; it only supports “append” and “clear” [2][3].

**5. What Kind of Storage is Used?**  
- **Primary (most cases)**: **GPU HBM (High-Bandwidth Memory)** , e.g., NVIDIA H100’s 80GB HBM3. This is because every Decode step requires **extremely fast reading of the entire cache** (memory bandwidth is the bottleneck), and HBM’s ~3.35 TB/s bandwidth is the only option that can feed the compute cores [2][3].  
- **Overflow strategies (extreme long contexts, e.g., 1M tokens)**: When HBM is insufficient, inference engines (vLLM, Hugging Face TGI) enable **Tiered Storage**: hot data (recently used) stays in HBM, cold data is offloaded to **CPU DRAM (system RAM)** , and in extreme cases, to **NVMe SSDs**. However, offloading to SSDs increases latency from microseconds to milliseconds [3].  
- **PagedAttention (vLLM core optimization)**: The KV cache is stored in **fixed-size pages** within HBM, managed like virtual memory in an OS – reducing fragmentation and improving utilization. The underlying storage medium remains HBM [2].

---

## 3. References

[1] Vaswani, A., et al. (2017). Attention Is All You Need. *NeurIPS*. URL: https://proceedings.neurips.cc/paper/7181-attention-is-all-you-need.pdf  
[2] Kwon, W., et al. (2023). Efficient Memory Management for Large Language Model Serving with PagedAttention (vLLM). *SOSP*. URL: https://arxiv.org/abs/2309.06180  
[3] Hugging Face Transformers – KV Cache Generation. URL: https://huggingface.co/docs/transformers/en/kv_cache