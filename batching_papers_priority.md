# Continuous/In-flight Batching 论文优先级排序

基于**核心价值、技术原创性、与章节主题关联度**三个维度，将 11 篇论文分为三级。

---

## 一、必读（5 篇）— 范式奠基 / 工业标杆 / 思想突破

| 序号 | 论文 | 时间 | 机构 | 理由 |
|:---:|------|:---:|------|------|
| 1 | **Orca** | 2022.07 | Seoul National University 等 | 整个领域的奠基之作，提出 iteration-level scheduling 和 selective batching，确立了 continuous batching 的基本范式 |
| 2 | **TensorRT-LLM Batch Manager** | 2023.10 | NVIDIA | 工业级标杆实现，提出 in-flight batching 概念，是 NVIDIA 生产级推理栈的核心组件 |
| 3 | **DeepSpeed-FastGen** | 2023.11 | Microsoft | Dynamic SplitFuse 思路极具代表性，将 prefill 切片与 decode 混合调度，是 continuous batching 向精细化调度演进的关键节点 |
| 4 | **vAttention** | 2024.05 | Microsoft Research India | 思想突破：用 GPU MMU 硬件虚拟内存替代 PagedAttention 的软件查表，开辟 KV Cache 管理的第二条技术路线 |
| 5 | **vTensor** | 2024.07 | Shanghai Jiao Tong University 等 | vAttention 的升级版，Segment + Page 两层虚拟张量抽象，代表虚拟内存路线的最高水平 |

### 核心阅读路径

```
Orca → TensorRT-LLM → DeepSpeed-FastGen → vAttention → vTensor
(调度范式)  (工业落地)  (混合调度)        (虚拟内存)   (虚拟张量)
```

---

## 二、补充（4 篇）— 专项方向代表 / 跨章桥梁 / SLO 优化

| 序号 | 论文 | 时间 | 方向 | 机构 | 理由 |
|:---:|------|:---:|------|------|------|
| 6 | **BatchLLM** | 2024.12 | 离线大批次 | Microsoft | 最新工作，Global Prefix Sharing + Throughput-oriented Token Batching，挖掘前缀复用潜力 |
| 7 | **SJF Scheduling / Learning to Rank** | 2024.08 | 智能调度 | UCSD 等 | 用学习模型预测输出长度的相对排序，实现近似 SJF 调度，调度策略智能化方向代表 |
| 8 | **Automatic Inference Engine Tuning** | 2024.08 | SLO 调优 | Nanjing University 等 | 面向 SLO 约束的自动调参，关注如何在给定延迟目标下最大化吞吐 |
| 9 | **Splitwise** | 2023.11 | prefill/decode 分离 | Microsoft 等 | prefill 与 decode 物理分离的早期探索，是连接本章与「P-D Disaggregating」章节的桥梁 |

### 补充篇主题阅读路线

#### 路线 1：调度策略智能化（2 篇）

适合人群：关注推理服务调度策略，想了解从静态规则到学习式调度的演进。

```
阅读顺序：
  SJF Scheduling → Automatic Inference Engine Tuning

路线特点：
  从"预测输出长度做 SJF 排序"切入
  → 再到"SLO 约束下的自动引擎调参"
  → 共同主题：让调度从手工规则走向自动化/智能化
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **SJF Scheduling / Learning to Rank (2024.08)** | 不需要精确预测输出长度，只需相对排序；用 Kendall τ 衡量排序质量，理论分析扎实 |
| 2 | **Automatic Inference Engine Tuning (2024.08)** | SLO 优化视角，自动为不同延迟目标选择最优引擎配置，更偏系统工程调优 |

#### 路线 2：场景化与跨章衔接（2 篇）

适合人群：关注特定场景优化（离线大批次）或想理解 batching 与 disaggregating 的演进关系。

```
阅读顺序：
  Splitwise → BatchLLM

路线特点：
  从"prefill/decode 物理分离"的早期探索
  → 到"离线大批次 + 全局前缀共享"的最新实践
  → 前者是跨章桥梁，后者是场景化深耕
```

| 顺序 | 论文 | 核心看点 |
|:---:|------|---------|
| 1 | **Splitwise (2023.11)** | prefill 与 decode 分到不同 GPU 池，避免 phase 间资源竞争，启发了 DistServe/Mooncake 等后续工作 |
| 2 | **BatchLLM (2024.12)** | 离线大 batch 场景，Global Prefix Sharing 跨请求共享公共前缀，Throughput-oriented Token Batching 重新定义批次构成 |

#### 时间有限推荐（2 篇）

从补充篇中选最有代表性的：

| 推荐 | 论文 | 理由 |
|:---:|------|------|
| 1 | **BatchLLM** | 最新工作，离线场景痛点清晰，前缀共享思路实用 |
| 2 | **SJF Scheduling** | 调度智能化方向的代表，思想简洁有效 |

---

## 三、非必要（2 篇）— 场景过窄 / 归类存疑

| 序号 | 论文 | 时间 | 理由 |
|:---:|------|:---:|------|
| 10 | SpotServe | 2023.12 | 聚焦云抢占式实例（spot instance）下的推理服务，场景过于特定。核心问题是容错和迁移，而非 batching 本身 |
| 11 | LightSeq | 2023.10 | 主题是 Sequence Level Parallelism（序列级并行），属于并行计算范畴而非 batching 调度。大概率是误归类到本章，应去「Multi-GPUs/Multi-Nodes Parallelism」章节查找 |

---

## 总览

```
必读    █████ 5 篇  ── 范式奠基 + 工业标杆 + 思想突破，构建知识体系
补充    ████ 4 篇   ── 专项方向 + 跨章桥梁 + SLO 优化
非必要  ██ 2 篇     ── 场景过窄 + 归类存疑
```

### 精简阅读建议

- **只读 3 篇**：Orca → TensorRT-LLM → vAttention
- **只读 5 篇**：再加 DeepSpeed-FastGen → vTensor
- **跟上 2024 前沿**：BatchLLM + SJF Scheduling
