# Parallel Decoding/Sampling 论文优先级排序

基于**影响力、创新性、工业界采用度**三个维度，将 26 篇论文分为三级。

---

## 一、必读（8 篇）— 奠基性 / 工业级标配 / 开辟新方向

| 序号 | 论文 | 时间 | 机构 | 理由 |
|:---:|------|:---:|------|------|
| 1 | **Blockwise Parallel Decoding** | 2018.11 | Berkeley & Google | 并行解码的雏形与奠基，首次提出块状并行解码框架，所有后续工作的思想源头 |
| 2 | **Speculative Sampling** | 2023.02 | DeepMind | 推测采样的开山之作，用小模型做草稿+大模型验证，保证输出分布不变（无损） |
| 3 | **Speculative Decoding** | 2023.05 | Google Research 等 | 独立提出推测解码，给出更系统的算法分析与理论保证，有开源实现 |
| 4 | **Medusa** | 2023.09 | Tianle Cai 等 | 多解码头路线的代表作，无需额外草稿模型，工业界广泛采用，代码成熟 |
| 5 | **LookaheadDecoding** | 2024.02 | UCSD & Google & Berkeley | 零训练成本的 n-gram 前瞻解码，工程落地门槛最低，有开源代码 |
| 6 | **TriForce** | 2024.04 | CMU & Meta AI | 分层推测解码，针对长序列生成优化，KV Cache 复用思路巧妙 |
| 7 | **MagicDec** | 2024.08 | CMU 等 | 长上下文场景下打破延迟-吞吐权衡，揭示推测解码在大 batch 下的新价值 |
| 8 | **DIVERSED** | 2026.04 | Purdue & Amazon 等 | 动态集成验证的松弛推测解码，代表 2026 最新前沿，有开源代码 |

### 核心阅读路径

```
Blockwise Parallel Decoding → Speculative Sampling → Medusa → LookaheadDecoding
     (思想源头)          (推测解码范式)      (多解码头)    (零训练方案)
                                                                        ↓
                                              TriForce → MagicDec → DIVERSED
                                              (长序列)   (长上下文)  (集成验证)
```

---

## 二、补充（13 篇）— 特定方向代表 / 有工业价值 / 技术有特色

| 序号 | 论文 | 时间 | 方向 | 机构 | 理由 |
|:---:|------|:---:|------|------|------|
| 9 | **OSD** | 2023.10 | 在线自适应 | UC Berkeley 等 | Online Speculative Decoding，在线调整草稿长度，自适应思路有启发性 |
| 10 | **Decoding Speculative Decoding** | 2024.02 | 理论分析 | UW-Madison | 对推测解码的系统性理论剖析，帮助理解本质，有代码 |
| 11 | **S3D** | 2024.05 | 自推测 | LG | 自推测解码，无需额外草稿模型，低内存 GPU 友好 |
| 12 | **Exploring and Improving Drafts** | 2024.06 | 块状并行改进 | KAIST & Google | 改进块状并行解码中的草稿质量，Blockwise 路线的完善 |
| 13 | **Multi-Token Joint Speculative** | 2024.07 | 多 token 联合 | UC 等 | 多 token 联合推测解码，提升每步接受率 |
| 14 | **Token Recycling** | 2024.08 | token 回收 | 哈工大 等 | 将被拒绝的 token 变废为宝，回收利用思路独特 |
| 15 | **PEARL** | 2024.08 | 自适应草稿 | USTC 等 | 自适应草稿长度的并行推测解码，有开源代码 |
| 16 | **Boosting Lossless Speculative** | 2024.08 | 蒸馏增强 | BIT | 特征采样+部分对齐蒸馏，提升无损推测解码效率 |
| 17 | **PARALLELSPEC** | 2024.10 | 并行草稿 | 腾讯 AI Lab 等 | 并行草稿生成器，提升草稿质量和生成速度 |
| 18 | **Fast Best-of-N** | 2024.10 | Best-of-N 加速 | CMU 等 | 用推测拒绝采样加速 Best-of-N 解码，对齐场景有实用价值 |
| 19 | **Mamba Drafters** | 2025.06 | SSM 草稿 | KAIST & Amazon 等 | 用 Mamba 做草稿模型，探索 SSM 架构在推测解码中的应用 |
| 20 | **STAND** | 2025.06 | 无模型推测 | KAIST & Amazon 等 | 无模型推测采样，测试时缩放加速，无需训练草稿模型 |
| 21 | **MineDraft** | 2026.03 | 批并行 | NUS & MIT | 批并行推测解码框架，面向批量推理场景，有开源代码 |

### 补充篇主题阅读路线

#### 路线 1：推测解码主线演进（5 篇）

适合人群：想系统理解推测解码从诞生到成熟的完整脉络。

```
阅读顺序：
  Blockwise Parallel Decoding → Speculative Sampling → Speculative Decoding → Medusa → DIVERSED

路线特点：
  从并行解码的思想源头出发
  → 推测采样确立"草稿+验证"范式
  → Google 的系统化理论分析
  → Medusa 开辟多解码头路线
  → DIVERSED 代表集成验证的最新前沿
  → 完整覆盖推测解码 8 年演进脉络
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **Blockwise Parallel Decoding (2018.11)** | 并行解码的雏形，理解"打破自回归串行依赖"的核心思想 |
| 2 | **Speculative Sampling (2023.02)** | DeepMind 开山之作，小模型草稿+大模型验证，无损保证输出分布 |
| 3 | **Speculative Decoding (2023.05)** | Google 独立工作，更系统的算法分析，理论接受率推导 |
| 4 | **Medusa (2023.09)** | 多解码头，无需额外模型，工业界落地最广的方案之一 |
| 5 | **DIVERSED (2026.04)** | 动态集成验证，松弛化接受策略，代表最新研究前沿 |

#### 路线 2：零训练 / 无额外模型方案（3 篇）

适合人群：不想训练额外草稿模型，关注即插即用的加速方案。

```
阅读顺序：
  LookaheadDecoding → S3D → STAND

路线特点：
  从 n-gram 前瞻的 LookaheadDecoding 开始
  → 自推测的 S3D（利用模型自身做草稿）
  → 无模型推测的 STAND（测试时缩放）
  → 共同主题：降低推测解码的落地门槛
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **LookaheadDecoding (2024.02)** | n-gram 匹配做前瞻，零训练成本，工程落地最简单 |
| 2 | **S3D (2024.05)** | 自推测解码，模型自身分层做草稿，低内存 GPU 友好 |
| 3 | **STAND (2025.06)** | 无模型推测采样，测试时缩放加速，完全无需额外训练 |

#### 路线 3：长序列 / 长上下文优化（3 篇）

适合人群：关注长文本生成、长上下文推理场景的加速方案。

```
阅读顺序：
  TriForce → MagicDec → Multi-Token Joint Speculative

路线特点：
  从分层推测的 TriForce 开始
  → 长上下文打破延迟-吞吐权衡的 MagicDec
  → 多 token 联合推测提升长序列接受率
  → 聚焦"长序列场景下推测解码的特殊价值"
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **TriForce (2024.04)** | CMU & Meta，分层推测解码，KV Cache 复用，长序列生成优化 |
| 2 | **MagicDec (2024.08)** | CMU，揭示推测解码在大 batch 长上下文下的新价值，打破延迟-吞吐权衡 |
| 3 | **Multi-Token Joint Speculative (2024.07)** | 多 token 联合推测，提升长序列场景的每步接受率 |

#### 路线 4：草稿质量与验证策略优化（4 篇）

适合人群：已有推测解码基础，想深入优化草稿生成和验证效率。

```
阅读顺序：
  OSD → PEARL → PARALLELSPEC → Boosting Lossless Speculative

路线特点：
  从在线自适应的 OSD 开始
  → 自适应草稿长度的 PEARL
  → 并行草稿生成器 PARALLELSPEC
  → 蒸馏增强的 Boosting Lossless
  → 聚焦"如何让草稿更准、验证更高效"
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **OSD (2023.10)** | Online Speculative Decoding，在线动态调整草稿长度 |
| 2 | **PEARL (2024.08)** | USTC，自适应草稿长度的并行推测解码，有开源代码 |
| 3 | **PARALLELSPEC (2024.10)** | 腾讯，并行草稿生成器，同时生成多个草稿提升质量 |
| 4 | **Boosting Lossless Speculative (2024.08)** | BIT，特征采样+部分对齐蒸馏，提升无损推测效率 |

#### 路线 5：新架构与新范式（3 篇）

适合人群：想了解推测解码与非 Transformer 架构、批量推理等新方向的结合。

```
阅读顺序：
  Mamba Drafters → STAND → MineDraft

路线特点：
  从 SSM 架构做草稿的 Mamba Drafters
  → 无模型推测的 STAND
  → 批并行框架 MineDraft
  → 共同主题：探索推测解码在新架构和新场景下的可能性
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **Mamba Drafters (2025.06)** | KAIST & Amazon，用 Mamba SSM 做草稿模型，利用 SSM 的并行性优势 |
| 2 | **STAND (2025.06)** | 无模型推测采样，测试时缩放，完全不依赖草稿模型 |
| 3 | **MineDraft (2026.03)** | NUS & MIT，批并行推测解码框架，面向批量推理场景优化 |

#### 时间有限推荐（3-5 篇）

从每个方向选最有代表性的：

| 推荐 | 论文 | 理由 |
|:---:|------|------|
| 1 | **Speculative Sampling** | 推测解码的起点，必须理解核心思想 |
| 2 | **Medusa** | 多解码头工业级方案，代码成熟可落地 |
| 3 | **LookaheadDecoding** | 零训练成本，工程落地最快 |
| 4 | **TriForce** | 长序列场景的代表，KV Cache 复用思路巧妙 |
| 5 | **DIVERSED** | 2026 最新前沿，了解推测解码的发展方向 |

#### 按兴趣深入组合

- **工程落地向**：路线 2（零训练方案）+ 路线 4（草稿优化）
- **理论研究向**：路线 1（主线演进）+ Decoding Speculative Decoding（理论分析）
- **长序列场景向**：路线 3（长上下文优化）+ Token Recycling（token 回收）
- **前沿探索向**：路线 5（新架构新范式）+ DIVERSED（集成验证）

---

## 三、非必要（5 篇）— 场景小众 / 增量改进 / 优先级最低

| 序号 | 论文 | 时间 | 理由 |
|:---:|------|:---:|------|
| 22 | Cascade Speculative | 2023.12 | 级联草稿模型思路，影响力有限，无开源代码 |
| 23 | Hidden Transfer | 2024.04 | 隐状态转移实现并行解码，思路有特色但无代码，验证不充分 |
| 24 | Instructive Decoding | 2024.05 | 更偏 NLP 应用（指令自精炼），非核心推理加速方向 |
| 25 | FocusLLM | 2024.08 | 用并行解码扩展上下文长度，场景特殊，与主流推测解码目标不同 |
| 26 | Hybrid Inference | 2024.09 | 混合推理（端云协同），场景小众，与纯推测解码路线偏离 |

---

## 总览

```
必读    ████ 8 篇  ── 奠基 + 标杆 + 前沿，构建知识体系
补充    █████████████ 13 篇 ── 方向代表 + 工业价值 + 技术特色
非必要  █████ 5 篇  ── 场景小众 + 增量改进 + 路线偏离
```

### 精简阅读建议

- **只读 3 篇**：Speculative Sampling → Medusa → LookaheadDecoding
- **只读 5 篇**：再加 TriForce → DIVERSED
- **跟上 2025-2026 前沿**：Mamba Drafters + STAND + MineDraft + DIVERSED
