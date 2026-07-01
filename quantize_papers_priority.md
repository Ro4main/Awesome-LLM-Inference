# Weight/Activation Quantize/Compress 论文优先级排序

基于**影响力、创新性、工业界采用度**三个维度，将 35 篇论文分为三级。

---

## 一、必读（8 篇）— 奠基性 / 工业级标配 / 开辟新方向

| 序号 | 论文 | 时间 | 机构 | 理由 |
|:---:|------|:---:|------|------|
| 1 | **LLM.int8()** | 2022.08 | Facebook AI Research 等 | 首次提出离群值分离机制，量化入门必读，bitsandbytes 库被广泛使用 |
| 2 | **SmoothQuant** | 2022.11 | MIT 等 | 激活量化的里程碑，"难度迁移"思想极其巧妙，W8A8 的标杆 |
| 3 | **GPTQ** | 2022.10 | IST Austria 等 | 4-bit PTQ 的标准范式，几乎所有推理框架都支持，理论基础扎实 |
| 4 | **AWQ** | 2023.06 | MIT 等 | 激活感知权重量化，精度高、速度快，当前工业界最主流的 W4 方案 |
| 5 | **ZeroQuant** | 2022.06 | Microsoft | 微软系列工作的起点，per-token/per-channel 粒度量化的系统性框架 |
| 6 | **QServe (W4A8KV4)** | 2024.05 | MIT & NVIDIA | 全栈量化系统设计的代表，算法+kernel+调度协同，工业级落地标杆 |
| 7 | **BitNet v2** | 2025.04 | Microsoft | 原生 1-bit 训练 + Hadamard 变换，极低比特的终极方向，2025 最新前沿 |
| 8 | **GuidedQuant** | 2025.05 | SNU & Samsung AI & Google | 端到端损失引导量化，突破了"逐层最小化误差"的局限，代表 PTQ 新范式 |

### 核心阅读路径

```
LLM.int8() → SmoothQuant → GPTQ → AWQ → QServe
  (理解离群点)  (激活量化)  (4-bit PTQ)  (更优 PTQ)  (全栈系统)
                                                          ↓
                                               BitNet v2 / GuidedQuant
                                               (原生低比特) (端到端引导)
```

---

## 二、补充（15 篇）— 特定方向代表 / 有工业价值 / 技术有特色

| 序号 | 论文 | 时间 | 方向 | 机构 | 理由 |
|:---:|------|:---:|------|------|------|
| 9 | **WINT8/4** | 2022.11 | 工业部署 | NVIDIA & Microsoft | MoE 模型量化部署的工业实践 |
| 10 | **ZeroQuant-V2** | 2023.03 | 误差补偿 | Microsoft | 低秩补偿（LoRC）技术有特色，ZeroQuant 系列的完善 |
| 11 | **ZeroQuant-FP** | 2023.07 | 浮点量化 | Microsoft | 首次提出 W4A8 浮点量化，FP 路线的重要节点 |
| 12 | **FP8-Quantization** | 2022.08 | FP8 基础 | Qualcomm AI Research | FP8 格式理论分析，理解 FP8 原理的必读 |
| 13 | **FP8-LM** | 2023.10 | FP8 训练 | Microsoft 等 | FP8 训练 LLM 的系统实践，MS-AMP 库 |
| 14 | **SparQ Attention** | 2023.12 | 注意力量化 | Graphcore | Query 侧量化+稀疏选择 Key，带宽优化方向代表 |
| 15 | **SpQR** | 2023.06 | 稀疏量化 | University of Washington 等 | 稀疏量化表示，接近无损压缩，技术思路独特 |
| 16 | **SqueezeLLM** | 2023.06 | 稠密+稀疏 | UC Berkeley | 稠密+稀疏混合量化 |
| 17 | **FP6-LLM** | 2024.01 | FP6 系统 | Microsoft 等 | 6-bit 浮点算法+系统协同设计，非标准位宽的代表 |
| 18 | **SpinQuant** | 2024.05 | 旋转量化 | Meta | 学习旋转矩阵来改善量化，思路新颖 |
| 19 | **VPTQ** | 2024.09 | 向量量化 | Microsoft | 向量 PTQ，极低比特的探索 |
| 20 | **BitNet a4.8** | 2024.11 | 1-bit 演进 | Microsoft | BitNet v2 的前置工作，理解 1-bit 激活量化的演进 |
| 21 | **ABQ-LLM** | 2024.08 | 任意位宽 | ByteDance | 任意比特量化加速，系统实现有参考价值 |
| 22 | **LLM-FP4** | 2023.10 | FP4 量化 | UST & Meta 等 | 4-bit 浮点量化，FP4 方向代表 |
| 23 | **1-bit LLMs** | 2024.08 | 1-bit 分析 | University of South Carolina | 对 1-bit LLM 是否需要 matmul 的理论分析，有启发性 |

---

## 三、非必要（12 篇）— 增量改进 / 场景小众 / 优先级最低

| 序号 | 论文 | 时间 | 理由 |
|:---:|------|:---:|------|
| 24 | KV Cache FP8 + WINT4 | 2023.09 | 博客文章，非正式论文，实践参考价值有限 |
| 25 | LLM-Shearing | 2023.10 | 实际是结构化剪枝，非量化技术，归类有误 |
| 26 | 2-bit LLM | 2023.11 | 场景过于极端，2-bit 实用性有限 |
| 27 | SmoothQuant+ | 2023.12 | SmoothQuant 的增量改进，ZTE 内部实践 |
| 28 | OdysseyLLM W4A8 | 2023.11 | Meituan 实践，增量性工作 |
| 29 | Agile-Quant | 2023.12 | 边缘设备场景，受众窄 |
| 30 | CBQ | 2023.12 | 跨块量化，学术探索，工业采用度低 |
| 31 | QLLM | 2023.10 | SenseTime 工作，影响力有限 |
| 32 | I-LLM | 2024.05 | 纯整数推理，场景特殊 |
| 33 | OutlierTune | 2024.06 | 通道级量化优化，增量改进 |
| 34 | GPTQT | 2024.06 | "量化两次"思路，增量性 |
| 35 | TEAL (Activation Sparsity) | 2024.08 | 激活稀疏化，严格说不是量化 |

---

## 总览

```
必读    ████ 8 篇  ── 奠基 + 标杆 + 前沿，构建知识体系
补充    ███████████████ 15 篇 ── 方向代表 + 工业价值 + 技术特色
非必要  ████████████ 12 篇  ── 增量改进 + 小众场景 + 归类存疑
```

### 精简阅读建议

- **只读 3 篇**：SmoothQuant → GPTQ → AWQ
- **只读 5 篇**：再加 LLM.int8() → QServe
- **跟上 2025 前沿**：BitNet v2 + GuidedQuant
