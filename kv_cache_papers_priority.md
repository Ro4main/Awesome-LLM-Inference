# KV Cache Scheduling/Quantize/Dropping 论文优先级排序

基于**影响力、创新性、工业界采用度**三个维度，将本章节 55 篇论文（含 NexusQuant 早期开源项目）按 Scheduling / Quantize / Dropping 三大方向分为三级。

> 三大方向作用于显存公式的不同因子，**正交可组合**：Scheduling 优化"分配效率与并发"，Quantize 压缩"单条 KV 体积"，Dropping 削减"KV 条目数量"。

---

## 一、必读（9 篇）— 奠基性 / 工业级标配 / 方向代表

### Scheduling 方向（3 篇）

| 序号 | 论文 | 时间 | 机构 | 理由 |
|:---:|------|:---:|------|------|
| 1 | **PagedAttention / vLLM** | 2023.09 | UC Berkeley 等 | KV Cache 分页管理的**奠基之作**，借鉴 OS 虚拟内存消除碎片，vLLM 已成现代推理栈事实标准，吞吐提升 2–4× |
| 2 | **SARATHI (Chunked Prefills)** | 2023.08 | Microsoft 等 | Chunked-prefill + stall-free 调度的起源，解决 prefill 阻塞 decode 的核心痛点，当前 vLLM/SGLang 标准做法 |
| 3 | **RadixAttention / SGLang** | 2023.12 | Stanford 等 | 前缀缓存（prefix caching）的代表，Radix 树管理跨请求 KV 复用，多轮对话/RAG 场景工业标配 |

### Quantize 方向（3 篇）

| 序号 | 论文 | 时间 | 机构 | 理由 |
|:---:|------|:---:|------|------|
| 4 | **GEAR** | 2024.03 | Georgia Tech | 量化 + 低秩 + 稀疏三组合的**代表性工作**，展示突破 2-bit 极限的补偿范式，近无损高压缩比，有开源代码 |
| 5 | **TensorRT-LLM KV Cache FP8** | 2023.10 | NVIDIA | 工业级 FP8 KV 量化的标准实现，NVIDIA 官方方案，了解生产部署必须读 |
| 6 | **KVQuant** | 2024.01 | UC Berkeley | KV 量化的**集大成之作**，四项创新（Per-Channel K + Pre-RoPE + 非均匀 + Dense-Sparse），3-bit 困惑度损失 <0.1，单卡跑 1M 上下文（位于 README 长上下文章节 L394） |

### Dropping 方向（3 篇）

| 序号 | 论文 | 时间 | 机构 | 理由 |
|:---:|------|:---:|------|------|
| 7 | **H2O** | 2023.06 | Rice University 等 | 动态驱逐的**奠基之作**，"heavy hitter"概念来源，子模最大化理论保证，20% 预算近无损 |
| 8 | **SnapKV** | 2024.04 | UIUC | Prefill 阶段压缩的代表，"观察窗口投票"思路简洁有效，LLM 生成前即知关注点，有开源代码 |
| 9 | **Adaptive KV Cache Compress** | 2023.10 | Illinois & Microsoft | "模型告诉你丢弃什么"的自适应压缩早期探索，代表学习式驱逐方向 |

### 核心阅读路径

```
Scheduling:  PagedAttention → SARATHI → RadixAttention
              (分页基石)      (chunk调度)  (前缀复用)

Quantize:    GEAR → TensorRT-LLM FP8 → KVQuant
             (补偿范式) (工业FP8)      (量化集大成)

Dropping:    H2O → SnapKV → Adaptive KV Cache Compress
             (heavy hitter) (观察窗口)  (自适应丢弃)
                          ↓
              三者正交可组合：调度为基座 + 量化做压缩 + 驱逐按场景叠加
```

> 注：**StreamingLLM**（attention sink 发现者，位于 README 框架章节 L175）虽不在本章节，但为 Dropping 方向的奠基工作，强烈建议交叉阅读。**KVQuant** 已提升为必读（位于 README 长上下文章节 L394），因其为 Quantize 方向的标杆之作。

---

## 二、补充（16 篇）— 特定方向代表 / 有工业价值 / 技术有特色

### Scheduling 补充（6 篇）

| 序号 | 论文 | 时间 | 方向 | 机构 | 理由 |
|:---:|------|:---:|------|------|------|
| 10 | **vAttention** | 2024.05 | 连续虚拟内存 | Microsoft Research India | 与 PagedAttention 对照的另一路线，保留连续虚拟内存 + 按需物理页，复用 FlashAttention 内核 |
| 11 | **GQA** | 2023.05 | 架构级 KV 缩减 | Google | 多头注意力分组共享 KV，架构层面减少 KV head 数，现代模型（LLaMA-2/3）标配 |
| 12 | **MQA** | 2019.11 | 架构级 KV 缩减 | Google | GQA 的前身，单 query-head 共享 KV，极致压缩但精度损失较大 |
| 13 | **Hydragen (Shared Prefixes)** | 2024.02 | 前缀共享 | Stanford 等 | 系统级前缀共享的另一实现路径，与 RadixAttention 互补 |
| 14 | **CacheBlend** | 2024.05 | 缓存融合 | University of Chicago | 跨请求缓存知识融合，代表 RAG/知识库场景的 KV 复用方向，LMCache 开源 |
| 15 | **DistKV-LLM (Infinite-LLM)** | 2024.01 | 分布式 KV | Alibaba | DistAttention + 分布式 KVCache，跨 GPU 显存池化，代表分布式调度方向 |

### Quantize 补充（6 篇）

| 序号 | 论文 | 时间 | 方向 | 机构 | 理由 |
|:---:|------|:---:|------|------|------|
| 16 | **QAQ** | 2024.03 | 自适应量化 | Nanjing University | 质量自适应量化，按 token 重要性分配比特，代表自适应量化方向，有开源代码 |
| 17 | **KVCache-1Bit** | 2024.05 | 极限压缩 | Rice University | 1-bit 耦合量化的极端尝试，探索 KV 量化的下限 |
| 18 | **ZipCache** | 2024.05 | 显著 token 识别 | Zhejiang University 等 | 显著 token 识别 + 量化，另一角度的重要性感知量化 |
| 19 | **AlignedKV** | 2024.09 | 内存访问优化 | Tsinghua University | 精度对齐量化，关注内存访问效率，工程参考价值高，有开源代码 |
| 20 | **MiniCache** | 2024.05 | 深度维度压缩 | ZIP Lab | 深度维度压缩 KV，跨层共享思路独特 |
| 21 | **NexusQuant** | 2026.04 | E8 格点量化 | Independent (jagmarques) | E8 根格向量量化 + 注意力感知 token 驱逐，10-33× 压缩，training-free 一行接入。**早期开源研究项目**（非同行评审论文，仅 Mistral-7B/Phi-3-mini 验证），代表 2026 格点量化前沿探索，有 PyPI 包 `nexusquant-kv` |

### Dropping 补充（5 篇）

| 序号 | 论文 | 时间 | 方向 | 机构 | 理由 |
|:---:|------|:---:|------|------|------|
| 22 | **Keyformer** | 2024.03 | key token 选择 | UBC 等 | d-matrix 硬件公司出品，代表硬件-算法协同的 token 选择思路，有开源代码 |
| 23 | **SqueezeAttention** | 2024.04 | 层间预算分配 | Lanzhou University 等 | 层间最优预算分配，与 PyramidKV（未收录）属同一子方向 |
| 24 | **AdaKV** | 2024.10 | 自适应预算 | USTC | 自适应预算分配驱逐，head-wise 分配，有开源实现 |
| 25 | **ThinK** | 2024.07 | query 驱动剪枝 | Salesforce AI Research | query 驱动的 key cache 剪枝，思路新颖 |
| 26 | **LayerKV** | 2024.10 | 层级管理 | Ant Group | 层级 KV Cache 管理，按层卸载与压缩，工程视角 |

### 补充篇主题阅读路线

#### 路线 1：Scheduling 演进脉络（4 篇）

适合人群：想深入理解 KV Cache 内存管理与请求调度的系统设计演进。

```
阅读顺序：
  PagedAttention（必读）→ vAttention → Hydragen → DistKV-LLM

路线特点：
  从分页管理的奠基作出发
  → 连续虚拟内存的对照路线
  → 前缀共享的系统实现
  → 分布式跨 GPU 显存池化
  → 完整覆盖单机到分布式的调度演进
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **PagedAttention（必读）** | OS 分页思想，非连续 KV block 存储，消除碎片，vLLM 基石 |
| 2 | **vAttention (2024.05)** | 保留连续虚拟内存 + OS 按需物理页，无需改注意力内核，复用 FlashAttention |
| 3 | **Hydragen (2024.02)** | 共享前缀的 KV 复用，多请求共享 prompt 的系统级优化 |
| 4 | **DistKV-LLM (2024.01)** | DistAttention + 分布式 KVCache，跨 GPU 显存池化，突破单机显存上限 |

#### 路线 2：Quantize 极限压缩（4 篇）

适合人群：关注 KV Cache 低比特压缩，想了解从 FP8 到 sub-2-bit 的演进。

```
阅读顺序：
  TensorRT-LLM FP8（必读）→ QAQ → GEAR（必读）→ KVCache-1Bit

路线特点：
  从工业级 FP8 标准实现入手
  → 自适应比特分配的 QAQ
  → 低秩+稀疏补偿的 GEAR
  → 1-bit 耦合量化的极限探索
  → 完整覆盖"工业可用 → 极限压缩"的谱系
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **TensorRT-LLM FP8（必读）** | NVIDIA 官方 FP8 KV 量化，生产部署的标准方案 |
| 2 | **QAQ (2024.03)** | 质量自适应量化，按 token 重要性分配比特，平衡精度与压缩 |
| 3 | **GEAR（必读）** | 量化 + 低秩矩阵逼近误差 + 稀疏矩阵修补 outlier，近无损高压缩 |
| 4 | **KVCache-1Bit (2024.05)** | 1-bit per channel 耦合量化，探索 KV 量化的理论下限 |

#### 路线 3：Dropping 策略演进（4 篇）

适合人群：关注注意力稀疏性与 KV 驱逐策略，想了解从盲删到结构化选择的演进。

```
阅读顺序：
  H2O（必读）→ Adaptive KV Cache Compress（必读）→ SnapKV（必读）→ AdaKV

路线特点：
  从 heavy hitter 动态驱逐出发
  → 模型自判丢弃的自适应探索
  → Prefill 观察窗口投票
  → head-wise 自适应预算分配
  → 完整覆盖"动态 → 自适应 → 静态优化"的驱逐演进
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **H2O（必读）** | 累计注意力得分驱逐，heavy hitter + 最近 token，子模最大化理论保证 |
| 2 | **Adaptive KV Cache Compress（必读）** | 模型自判丢弃内容，自适应压缩的早期探索 |
| 3 | **SnapKV（必读）** | Prefill 阶段观察窗口投票，生成前即知关注点，简洁高效 |
| 4 | **AdaKV (2024.10)** | head-wise 自适应预算分配，不同 head 分配不同 KV 预算 |

#### 路线 4：架构级 KV 缩减（2 篇）

适合人群：关注从模型架构层面减少 KV Cache 体积，而非后处理压缩。

```
阅读顺序：
  MQA → GQA

路线特点：
  从单 head 共享 KV 的极致压缩
  → 分组共享的平衡方案
  → 理解现代模型（LLaMA-2/3、Mistral）为何标配 GQA
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **MQA (2019.11)** | 单 query-head 共享 KV，极致压缩但精度损失，GQA 的前身 |
| 2 | **GQA (2023.05)** | 分组共享 KV，在压缩与精度间取得平衡，现代模型标配 |

#### 路线 5：系统级缓存与复用（3 篇）

适合人群：关注 RAG、多轮对话等场景下的 KV Cache 跨请求复用。

```
阅读顺序：
  RadixAttention（必读）→ CacheBlend → MemServe

路线特点：
  从 Radix 树前缀缓存出发
  → 缓存知识融合
  → 弹性内存池的上下文缓存
  → 聚焦"复用而非压缩"的系统级视角
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **RadixAttention（必读）** | Radix 树管理跨请求 KV 复用，SGLang 的核心 |
| 2 | **CacheBlend (2024.05)** | 跨请求缓存知识融合，RAG 场景的 KV 复用 |
| 3 | **MemServe (2024.06)** | 弹性内存池 + 上下文缓存， disaggregated serving 视角 |

#### 时间有限推荐（3–5 篇）

从每个方向选最有代表性的：

| 推荐 | 论文 | 理由 |
|:---:|------|------|
| 1 | **PagedAttention** | Scheduling 基石，不读无法理解现代推理栈 |
| 2 | **H2O** | Dropping 奠基，heavy hitter 概念的来源 |
| 3 | **GEAR** | Quantize 补偿范式代表，近无损高压缩 |
| 4 | **SARATHI** | Chunked-prefill 是当前工业标准做法 |
| 5 | **SnapKV** | Prefill 压缩代表，思路简洁有效 |

#### 按兴趣深入组合

- **系统/工程向**：路线 1（Scheduling 演进）+ 路线 5（缓存复用）
- **算法/压缩向**：路线 2（Quantize 极限）+ 路线 3（Dropping 演进）
- **架构设计向**：路线 4（架构级缩减）+ 路线 1（Scheduling）

---

## 三、非必要（29 篇）— 增量改进 / 场景小众 / 优先级最低

### Scheduling 非必要（6 篇）

| 序号 | 论文 | 时间 | 理由 |
|:---:|------|:---:|------|
| 27 | LTP | 2022.06 | 早期 token 剪枝，思想已被后续工作覆盖 |
| 28 | KV Cache FP8 + WINT4 | 2023.09 | 博客文章，非正式论文，实践参考价值有限 |
| 29 | CacheGen | 2023.10 | 侧重上下文加载速度，与 KV 管理主方向偏离 |
| 30 | KV-Cache Optimizations + Speculative | 2023.12 | 增量结合工作，独立价值有限 |
| 31 | FASTDECODE | 2024.03 | 异构调度，niche 场景 |
| 32 | KV Cache Prefetch | 2025.04 | 预取优化，增量改进 |

### Quantize 非必要（4 篇）

| 序号 | 论文 | 时间 | 理由 |
|:---:|------|:---:|------|
| 33 | MiKV | 2024.02 | 混合精度思路与 QAQ 重叠度高 |
| 34 | CompressKV | 2024.06 | 压缩 KV head，与 GQA 类似但影响精度 |
| 35 | Zero-Delay QKV Compression | 2024.08 | 零延迟压缩，增量改进 |
| 36 | Inference-Time Hyper-Scaling | 2025.06 | NVIDIA 报告，偏宣传性质 |

### Dropping 非必要（11 篇）

| 序号 | 论文 | 时间 | 理由 |
|:---:|------|:---:|------|
| 37 | Scissorhands | 2023.05 | 重要性持久性假设，已被更精细方法超越 |
| 38 | QK-Sparse/Dropping Attention | 2023.06 | 稀疏 FlashAttention，与主方向偏离 |
| 39 | KV Cache Compress with LoRA | 2023.12 | LoRA 压缩 KV，niche |
| 40 | LESS | 2024.02 | 合成循环 KV 压缩，增量工作 |
| 41 | DMC | 2024.03 | 动态内存压缩，需改造模型 |
| 42 | ALISA | 2024.03 | 稀疏感知缓存，增量改进 |
| 43 | Palu | 2024.07 | 低秩投影压缩，与 GEAR 重叠 |
| 44 | ClusterKV | 2024.12 | 语义空间压缩，niche |
| 45 | DynamicKV | 2024.12 | 任务自适应压缩，增量性 |
| 46 | KVzip | 2025.05 | Query-agnostic 压缩，场景特殊 |
| 47 | KV Cache Recomputation | 2024.11 | I/O 感知重计算，增量改进 |

### 其他/跨场景非必要（8 篇）

| 序号 | 论文 | 时间 | 理由 |
|:---:|------|:---:|------|
| 48 | ChunkAttention | 2024.02 | 前缀感知 KV + 两阶段分区，与 RadixAttention 重叠 |
| 49 | KV-Runahead | 2024.05 | 并行 KV 生成，niche |
| 50 | MLKV | 2024.07 | 多层 KV head，需改架构 |
| 51 | MemServe | 2024.06 | 弹性内存池（已列入路线 5，时间紧可跳过） |
| 52 | Prompt Caching | 2024.02 | 嵌入相似度前缀缓存，与 RadixAttention 重叠 |
| 53 | DynamicLLaVA | 2025.02 | 多模态场景，非纯 LLM |
| 54 | CacheCraft | 2025.02 | RAG chunk-cache 管理，场景特殊 |
| 55 | AVP | 2026.03 | 跨模型 KV 迁移协议，前沿但小众 |

---

## 总览

```
必读    █████████ 9 篇   ── 三大方向各 3 篇奠基/标杆，构建知识体系
补充    ██████████████████ 17 篇 ── 方向代表 + 工业价值 + 技术特色（含 NexusQuant 早期开源项目）
非必要  █████████████████████████████ 29 篇 ── 增量改进 + 小众场景 + 归类存疑
```

### 精简阅读建议

- **只读 3 篇**：PagedAttention → H2O → GEAR（三大方向各取奠基作）
- **只读 5 篇**：再加 SARATHI → SnapKV（补全 Scheduling 调度与 Dropping 进阶）
- **只读 9 篇**：补全 TensorRT-LLM FP8、KVQuant、RadixAttention、Adaptive KV Cache Compress（覆盖全部必读）
- **跟上 2026 前沿**：AVP（跨模型 KV 迁移协议）

### 三大方向组合部署建议

工业部署的最优解是**三层栈**，而非单点突破：

```
基座层：Scheduling（PagedAttention + SARATHI）  ── 精度无损，吃满并发红利
压缩层：Quantize（GEAR / FP8）                   ── 2-bit 近无损，压缩单条体积
场景层：Dropping（H2O / SnapKV）                 ── 长上下文按需叠加，极端压缩
```
