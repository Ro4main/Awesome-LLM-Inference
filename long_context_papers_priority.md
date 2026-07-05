# Long Context Attention/KV Cache Optimization 论文优先级排序

基于**影响力、创新性、工业界采用度**三个维度，将 29 篇论文分为三级。

---

## 一、必读（8 篇）— 奠基性 / 工业级标配 / 开辟新方向

| 序号 | 论文 | 时间 | 机构 | 理由 |
|:---:|------|:---:|------|------|
| 1 | **RingAttention** | 2023.10 | UC Berkeley | 分布式长上下文的奠基性工作，设备环轮转通信+计算重叠，实现近无限上下文扩展，被广泛引用 |
| 2 | **KVQuant** | 2024.01 | UC Berkeley | KV量化的系统性框架，四大创新（逐通道/RoPE前/非均匀/离群值分离），3-bit量化下精度损失<0.1 |
| 3 | **Infini-attention** | 2024.04 | Google | 开辟"无限上下文"新方向，融合局部因果注意力+线性注意力压缩记忆，有界内存处理任意长度 |
| 4 | **Quest** | 2024.06 | MIT | 查询感知的KV稀疏选择，只加载关键页，自注意力加速7.03×，代表稀疏注意力的最新范式 |
| 5 | **Blockwise Attention** | 2023.05 | UC Berkeley | 块级并行Transformer的起点，将激活内存从O(n)降至O(block_size)，RingAttention的基础 |
| 6 | **LightningAttention-2** | 2023.07 | OpenNLPLab | 分块注意力+重计算策略，实现"免费"的无限序列长度，被MiniMax-01等工业化应用 |
| 7 | **StripedAttention** | 2023.11 | MIT | RingAttention的重要优化，针对因果掩码减少冗余计算，加速效果显著 |
| 8 | **YOCO** | 2024.05 | Microsoft | 架构级创新，Decoder-Decoder架构实现"只缓存一次"，从模型结构层面减少KV |

### 核心阅读路径

```
Blockwise Attention → RingAttention → StripedAttention
  (块级计算基础)    (分布式扩展)     (优化加速)
         ↓
    LightningAttention-2
    (分块+重计算路线)
         ↓
KVQuant → Quest
  (量化压缩)  (稀疏选择)
         ↓
    Infini-attention → YOCO
    (无限上下文)     (架构创新)
```

---

## 二、补充（14 篇）— 特定方向代表 / 有工业价值 / 技术有特色

| 序号 | 论文 | 时间 | 方向 | 机构 | 理由 |
|:---:|------|:---:|------|------|------|
| 9 | **Landmark Attention** | 2023.05 | 随机访问 | EPFL | landmark token索引机制，实现随机访问的无限上下文，思路独特 |
| 10 | **HyperAttention** | 2023.11 | 近似算法 | Yale & Google | 列采样+低秩近似实现近线性时间长上下文注意力，理论基础扎实 |
| 11 | **Prompt Cache** | 2023.11 | 缓存复用 | Yale | 模块化注意力复用，针对系统提示等重复前缀的优化，有工业价值 |
| 12 | **CLA** | 2024.05 | 跨层共享 | MIT-IBM | 跨层注意力，多层共享KV缓存，架构级优化思路 |
| 13 | **MInference 1.0** | 2024.06 | 动态稀疏 | Microsoft | 动态稀疏注意力加速预填充阶段，预填充优化的代表 |
| 14 | **SKVQ** | 2024.05 | 滑动窗口量化 | 上海AI实验室 | 滑动窗口KV量化，适配流式处理场景 |
| 15 | **PQCache** | 2024.07 | 乘积量化 | PKU | 乘积量化应用于KV缓存，压缩率更高 |
| 16 | **RelayAttention** | 2024.02 | 长提示优化 | 商汤 | 针对长系统提示的服务优化策略 |
| 17 | **InfiniGen** | 2024.06 | 动态KV管理 | SNU | 动态KV缓存管理，生成式推理优化 |
| 18 | **ShadowKV** | 2024.10 | 影子缓存 | CMU & ByteDance | 影子KV缓存机制，高吞吐长上下文推理 |
| 19 | **RetrievalAttention** | 2024.09 | 检索替代 | Microsoft | 用向量检索替代部分注意力计算，检索与注意力融合方向 |
| 20 | **HOMER** | 2024.04 | 分层合并 | KAIST | 分层上下文合并，提升长上下文理解能力 |
| 21 | **RAGCache** | 2024.04 | RAG缓存 | PKU & ByteDance | 检索增强生成的知识缓存，RAG方向代表 |
| 22 | **LOOK-M** | 2024.06 | 多模态长上下文 | OSU等 | 多模态长上下文KV优化，跨模态方向 |

### 补充篇主题阅读路线

#### 路线 1：分布式并行路线（3 篇）

适合人群：关注多卡/多节点分布式训练与推理，想深入理解序列并行的演进。

```
阅读顺序：
  Blockwise Attention → RingAttention → StripedAttention

路线特点：
  从块级并行的基础概念出发
  → RingAttention的设备环轮转机制
  → StripedAttention的因果掩码优化
  → 完整覆盖序列并行的核心技术脉络
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **Blockwise Attention (2023.05)** | 块级注意力和FFN计算，将激活内存从O(n)降至O(block_size)，RingAttention的理论基础 |
| 2 | **RingAttention (2023.10)** | UC Berkeley，序列分片到多设备，KV块环中轮转，通信与计算重叠，实现近无限上下文 |
| 3 | **StripedAttention (2023.11)** | MIT，针对因果Transformer优化RingAttention，减少冗余计算，进一步加速 |

#### 路线 2：KV量化深入（3 篇）

适合人群：关注内存优化，想深入理解KV缓存量化的各种技术路线。

```
阅读顺序：
  KVQuant → SKVQ → PQCache

路线特点：
  从最系统的KVQuant开始
  → 流式场景的SKVQ滑动窗口量化
  → 更激进的乘积量化PQCache
  → 覆盖量化精度和压缩率的权衡空间
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **KVQuant (2024.01)** | 四大创新：逐通道Key量化、RoPE前量化、非均匀量化、逐向量稠密-稀疏分解，3-bit量化精度损失<0.1 |
| 2 | **SKVQ (2024.05)** | 上海AI实验室，滑动窗口KV缓存量化，适用于流式处理和长上下文推理 |
| 3 | **PQCache (2024.07)** | PKU，乘积量化(Product Quantization)用于KV缓存，更高压缩率的探索 |

#### 路线 3：稀疏注意力路线（3 篇）

适合人群：关注计算效率优化，想了解如何通过稀疏化加速长上下文推理。

```
阅读顺序：
  HyperAttention → Quest → RetrievalAttention

路线特点：
  从近似注意力算法HyperAttention入手
  → 查询感知的动态稀疏选择Quest
  → 用向量检索替代注意力的RetrievalAttention
  → 从"近似计算"到"选择性加载"再到"检索替代"
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **HyperAttention (2023.11)** | Yale & Google，列采样+低秩近似实现近线性时间长上下文注意力，理论基础扎实 |
| 2 | **Quest (2024.06)** | MIT，查询感知的KV页选择，用min/max元数据快速估算重要性，只加载Top-K页 |
| 3 | **RetrievalAttention (2024.09)** | Microsoft，用向量检索选择相关KV，检索与注意力深度融合的新方向 |

#### 路线 4：架构与系统优化（4 篇）

适合人群：关注模型架构和系统层面的优化，想了解如何从结构上减少KV需求。

```
阅读顺序：
  YOCO → CLA → HOMER → LOOK-M

路线特点：
  从Decoder-Decoder架构的YOCO开始
  → 跨层共享KV的CLA
  → 分层上下文合并的HOMER
  → 多模态长上下文的LOOK-M
  → 覆盖架构级优化的多种思路
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **YOCO (2024.05)** | Microsoft，Decoder-Decoder架构，"只缓存一次"，从架构层面减少KV重复存储 |
| 2 | **CLA (2024.05)** | MIT-IBM，跨层注意力，多层共享Key和Value，减少每层独立KV缓存的冗余 |
| 3 | **HOMER (2024.04)** | KAIST，分层上下文合并，通过层次化压缩提升长上下文理解能力 |
| 4 | **LOOK-M (2024.06)** | OSU等，多模态长上下文KV优化，跨模态场景的KV管理策略 |

#### 路线 5：缓存管理与存储（3 篇）

适合人群：关注KV缓存的高效管理和存储优化，想了解系统层面的工程实践。

```
阅读顺序：
  Prompt Cache → RelayAttention → ShadowKV

路线特点：
  从重复前缀缓存的Prompt Cache开始
  → 长系统提示优化的RelayAttention
  → 影子缓存机制的ShadowKV
  → 覆盖KV缓存管理的不同场景
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **Prompt Cache (2023.11)** | Yale，模块化注意力复用，针对系统提示等重复前缀的缓存优化 |
| 2 | **RelayAttention (2024.02)** | 商汤，针对长系统提示的服务优化策略，减少重复计算 |
| 3 | **ShadowKV (2024.10)** | CMU & ByteDance，影子KV缓存机制，高吞吐长上下文推理 |

#### 时间有限推荐（3-5 篇）

从每个方向选最有代表性的：

| 推荐 | 论文 | 理由 |
|:---:|------|------|
| 1 | **Landmark Attention** | 随机访问无限上下文的独特思路 |
| 2 | **MInference 1.0** | 预填充阶段优化的代表 |
| 3 | **InfiniGen** | 动态KV管理的系统实践 |
| 4 | **RAGCache** | RAG与缓存结合的方向 |
| 5 | **Infini-attention** | 无限上下文的核心思路 |

#### 按兴趣深入组合

- **算法/理论向**：路线 2（KV量化）+ 路线 3（稀疏注意力）
- **系统/工程向**：路线 1（分布式并行）+ 路线 5（缓存管理）
- **架构创新向**：路线 4（架构优化）+ Infini-attention
- **多模态向**：LOOK-M + RAGCache

---

## 三、非必要（7 篇）— 增量改进 / 场景小众 / 优先级最低

| 序号 | 论文 | 时间 | 理由 |
|:---:|------|:---:|------|
| 23 | **LightningAttention-1** | 2023.07 | LightningAttention-2已大幅超越，v1参考价值有限 |
| 24 | **Streaming Attention** | 2023.11 | 单次流式算法，精度损失较大，工业采用度低 |
| 25 | **KCache** | 2024.04 | 论文质量一般，影响力有限 |
| 26 | **InstInfer** | 2024.09 | 存储内注意力卸载，依赖特殊硬件，场景小众 |
| 27 | **SentenceVAE** | 2024.08 | 基于VAE的句子级压缩，精度与效率权衡不佳 |
| 28 | **REFORM** | 2025.06 | 2025年新论文，尚需时间验证，暂列为非必要 |
| 29 | **MiniMax-01** | 2025.01 | 本质是LightningAttention的工业化应用，算法创新有限 |

---

## 总览

```
必读    ████ 8 篇  ── 奠基 + 标杆 + 前沿，构建知识体系
补充    ████████████████ 14 篇 ── 方向代表 + 工业价值 + 技术特色
非必要  ████████ 7 篇  ── 增量改进 + 小众场景 + 待验证
```

### 精简阅读建议

- **只读 3 篇**：RingAttention → KVQuant → Quest
- **只读 5 篇**：再加 Infini-attention → YOCO
- **跟上最新前沿**：关注 DeepSeek MLA / FlashMLA 方向（架构级KV压缩）