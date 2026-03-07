<!-- PPT Markdown: 基于分解增强注意力与跨维度蒸馏的长期时间序列预测方法 -->
<!-- Slides: 27 | Generated from: thesis_liuxuewen.tex -->
<!-- Compatible with: Gamma, Kimi, Beautiful.ai, Marp -->

## Slide Index
1. Cover
2. Agenda
3. Background & Motivation
4. Challenge 1: Attention Dilution
5. Challenge 2: Time-Variable Coupling
6. Challenge 3: Black-box Fusion
7. Prior Work Landscape
8. Technical Roadmap
9. Work 1: AdapTFormer Overview
10. MV-FDE Module
11. SAM Module
12. AdapTFormer: The Synergy
13. Experiment Setup
14. AdapTFormer: Main Results
15. AdapTFormer: Ablation Study
16. AdapTFormer: Attention Visualization
17. MV-FDE Generalizability
18. Bridge: From AdapTFormer to GraphTD
19. Work 2: GraphTD Overview
20. TimeBridge Normalization
21. Dual-Path Feature Extraction
22. Cross-Dimensional Knowledge Distillation
23. Structured Spatiotemporal Fusion
24. GraphTD: Main Results
25. GraphTD: Ablation Study
26. GraphTD: Interpretability & Robustness
27. Conclusion & Future Work

---

# 基于分解增强注意力与跨维度蒸馏的长期时间序列预测方法

**刘学文** | 导师：翟俊海 教授 | 河北大学数学与信息科学学院

工程硕士（软件工程）· 答辩：2026年3月

> 从单维度优化到时空协同，实现长期多变量时间序列预测的系统性突破

---

# 目录 Agenda

1. 🔍 **研究背景**：为什么多变量时序预测很重要？
2. ⚠️ **三大挑战**：现有方法的根本局限
3. 🔧 **工作一 AdapTFormer**：分解增强注意力机制
4. 🚀 **工作二 GraphTD**：跨维度蒸馏与可解释融合
5. 📊 **实验验证**：SOTA性能 + 消融 + 可视化
6. 🎯 **总结与展望**

---

# 背景与意义：时序预测驱动关键决策

> 多变量长期时间序列预测（MTSF）是AI驱动高风险决策的核心引擎

- ⚡ **能源调度**：同时预测数十变量（负荷、温度、湿度）避免电网崩溃
- 🚗 **智慧交通**：路网流量预测，提前1周规避拥堵
- 💹 **金融风控**：股价、汇率多变量联合预测，控制投资风险
- 🌪️ **气象预警**：极端天气提前预报，每小时精度至关重要
- 💡 **本文聚焦**：预测步长 L ∈ {96, 192, 336, 720}，对应4小时～30天

---

# 挑战一：注意力稀释 Attention Dilution

> **核心问题**：高维变量场景下注意力权重被"稀释"，模型无法聚焦关键依赖

- 📊 **规模量化**：100个变量 → 10,000个变量对需要分配注意力权重
- 🌪️ **根本原因**：点积注意力直接作用于趋势+季节+噪声的**混合原始信号**
- 📉 **表现**：注意力分布熵 > 阈值，权重近乎均匀分布，关键依赖被淹没
- ❌ **现有尝试**：Informer 用稀疏注意力降复杂度，但稀疏模式由先验决定；Autoformer 直接**替换**注意力机制，丢弃了点积注意力的优势

---

# 挑战二：时间-变量协同建模困境

> **单路径架构的固有矛盾**：时间信息与变量信息被迫共享同一表示空间

| 方法类型 | 代表 | 优势 | 缺陷 |
|---------|------|------|------|
| 以时间为中心 | Autoformer, PatchTST | 强时序模式 | 忽略变量依赖 |
| 以变量为中心 | iTransformer | 强变量关联 | 细粒度时间动态丢失 |
| **本文方案** | **GraphTD** | **时空双维度协同** | — |

- 🔑 **问题本质**：单路径迫使模型在时间 vs. 变量之间"二选一"，长预测步长（L≥336）误差加剧

---

# 挑战三：黑盒不可解释性

> **高风险场景的痛点**：无法回答"是趋势还是季节成分主导了预测？"

- 🔬 现有方法用简单特征拼接（Concatenation）融合多路径信息
- ❗ 信息冲突无法溯源，预测驱动力不透明
- 🏭 **实际影响**：能源/金融领域专家拒绝部署"说不清原因"的模型
- ✅ **本文解决方案**：3×3交互矩阵 + 正交约束 → **成分级可解释融合**

---

# 先行工作全景：方法演进路径

[Figure: MTSF方法演进路径图 - RNN→CNN→Transformer→本文工作定位]

- **统计方法**：ARIMA/VAR → 线性假设，高维性能受限
- **深度学习**：LSTM/TCN → 序列化计算瓶颈，长程依赖不足
- **Transformer时代**：稀疏注意力 → 分解驱动 → 结构重组 → 混合方法
- 🎯 **本文定位**：
  - AdapTFormer：分解驱动方向的**重构-增强范式**
  - GraphTD：结构重组方向的**时空协同机制**

---

# 技术路线：从单维度优化到时空协同

```
问题1: 注意力稀释     → Work1: AdapTFormer  → MV-FDE + SAM
问题2: 时间-变量耦合   → Work2: GraphTD      → 双路径 + 跨维度蒸馏
问题3: 黑盒不可解释   → Work2: GraphTD      → 结构化时空融合
```

- ✅ **两工作关系**：递进而非并列
  - AdapTFormer：变量维度优化，奠定基础
  - GraphTD：在此基础上**引入时间维度**，实现真正协同
- 📐 **共同理念**：增强 > 替换；分解创造清晰工作环境

---

# 工作一 AdapTFormer：整体架构

> 设计理念："**重构 → 增强**"，而非替换注意力机制

[Figure: AdapTFormer三阶段架构图：MV-FDE → Transformer编码器(SAM) → 预测头]

| 阶段 | 模块 | 功能 |
|------|------|------|
| 重构 | **MV-FDE** | 分解输入信号，创造清晰工作环境 |
| 编码 | **SAM** | 自适应调制注意力权重，精准聚焦 |
| 预测 | 线性投影头 | 一次性输出L步预测值 |

- 💡 **核心区别**：保留并强化点积注意力，而非替换它

---

# MV-FDE：多视角特征分解与编码

> **目标**：在输入阶段分解混合信号，简化后续注意力的工作任务

**处理流程**（对每个变量序列）：

1. 📉 **移动平均分解** → 趋势成分 $X_{trend}$（低频）
2. 📈 **残差提取** → 残差成分 $X_{residual}$（高频，含季节性+噪声）
3. 🔢 **双路线性嵌入** → 各自独立映射到嵌入空间
4. ➕ **融合** → $h_v^0 = h_v^{trend} + h_v^{residual}$

- 🏗️ **设计区别**：MV-FDE在**输入层一次性完成**（vs. Autoformer在每层嵌入）
- 🔌 **即插即用**：可插入任何Transformer变体，平均降MSE **49.2%**

---

# SAM：自适应注意力机制

> **目标**：动态调制注意力权重，在Softmax之前放大关键变量、抑制冗余变量

**工作流程**：

1. 计算标准点积注意力分数：$\text{Scores} = QK^T / \sqrt{d_k}$
2. 并行计算自适应权重：$W_{adaptive} = \text{Softmax}(\text{Mean}(\text{FC}(Q)))$
3. Pre-Softmax调制：$\text{Scores}' = \text{Scores} \odot W_{adaptive}$
4. 输出：$A = \text{Softmax}(\text{Scores}')$，$O = AV$

- 🎯 **关键设计**：Pre-Softmax（而非Post-Softmax）调制——直接影响竞争概率
- 🔗 **协同效应**：MV-FDE简化了SAM的学习任务，SAM放大了MV-FDE的效果

---

# MV-FDE + SAM：深度协同机制

[Figure: 注意力热图对比：Vanilla Transformer（分散）vs. AdapTFormer（高度集中）]

> AdapTFormer = 清晰工作环境（MV-FDE）× 精准聚焦能力（SAM）

- 🧩 **1+1>2**：完整模型性能超过两个组件独立贡献的叠加
- 📐 **物理意义**：分解后的趋势/残差成分有明确含义，SAM能更快判断哪些变量对重要
- 📊 **熵分析定量验证**：注意力分布熵降低 **>15%**，权重从均匀分布收敛到尖峰分布

---

# 实验设置

- 📚 **9个公开数据集**：Weather(21变量), Traffic(862变量), Electricity(321变量), ETTh1/h2, ETTm1/m2, Exchange, Solar-Energy
- 🏆 **8个基线模型**：iTransformer, PatchTST, Crossformer, FEDformer, Autoformer, DLinear, TimesNet, Stationary
- ⚙️ **预测步长**：L ∈ {96, 192, 336, 720}
- 📏 **评估指标**：MSE（主）+ MAE（辅）
- 🖥️ **硬件**：2× NVIDIA RTX 4090 GPU

---

# AdapTFormer 主实验结果

[Figure: 主实验结果对比表（9数据集×4预测步长）]

**关键数据**：
- 🏆 **Electricity L=720**：MSE **0.203**，优于iTransformer(0.228) **11%**
- ⚡ **Weather**：相比Transformer基线 MSE降低 **5.0%**
- 🔋 **Electricity**：相比Transformer基线 MSE降低 **6.7%**
- 📈 **长步长优势**：随L从96→720，AdapTFormer相对优势持续增强
- ✅ 9个数据集上**绝大多数**场景最优或次优

---

# AdapTFormer 消融实验

| 模型变体 | Weather MSE | Electricity MSE |
|---------|------------|----------------|
| **(1) 完整模型** | **0.245** | **0.166** |
| (2) w/o SAM（仅分解）| 0.249 (+1.6%) | 0.170 (+2.4%) |
| (3) w/o MV-FDE（仅注意力）| 0.256 (+4.5%) | 0.177 (+6.6%) |
| (4) Vanilla Transformer | 0.258 | 0.178 |

- 🔑 **MV-FDE贡献 > SAM贡献**：分解重构是基础，注意力增强是放大器
- 🎯 **协同效应**：完整模型 = 0.245，预期叠加 = 0.247，**超额收益 18%**

---

# AdapTFormer 注意力可视化分析

[Figure: 左：注意力热图（Baseline=分散带状 vs. AdapTFormer=极细集中线）| 右：注意力熵下降曲线]

**定性**（热图）：
- AdapTFormer注意力权重极其锐利，高度集中于关键变量对
- Baseline呈现扩散的带状分布

**定量**（熵分析）：
- AdapTFormer曲线更陡峭 → 更低熵 → 更高集中度
- 注意力熵降低 **>15%** → 权重不确定性可量化降低

> 双重验证：SAM在MV-FDE提供的清晰表示基础上，成功实现注意力聚焦

---

# MV-FDE 泛化性验证：即插即用能力

[Figure: Transformer & Informer 添加MV-FDE前后MSE/MAE对比柱状图]

| 骨干模型 | 平均MSE提升 | 最大提升（数据集） |
|---------|-----------|---------------|
| Transformer + MV-FDE | **49.2%** | 65.9%（Weather）|
| Informer + MV-FDE | **43.1%** | 46.4%（Electricity）|

- 🔌 **结论**：MV-FDE是通用增强组件，**与具体注意力机制解耦**
- 💡 **分解等价值**：清晰的工作环境对所有注意力架构都有益

---

# 桥接：从 AdapTFormer 到 GraphTD

> AdapTFormer 已解决变量维度问题，但时间维度建模仍有约束

- ✅ **AdapTFormer方案**：变量Token化 + MV-FDE + SAM → 变量依赖建模成功
- ⚠️ **单路径局限**：时间信息被隐式编码进变量特征，细粒度时间动态模糊化
- 💡 **GraphTD的思路**：分离时间路径 vs. 变量路径，**各自专精**，再通过跨维度蒸馏融合

```
AdapTFormer：  输入  →  [单路径: 变量Token化]  →  预测
GraphTD：     输入  →  [时间路径]  →  跨维度蒸馏  →  结构化融合  →  预测
                   ↘  [变量路径]  ↗
```

---

# 工作二 GraphTD：整体架构

> 设计理念："**分离 → 对齐 → 融合**"，实现时间-变量双维度真正协同

[Figure: GraphTD整体架构图：TimeBridge → 双路径 → 跨维度蒸馏 → 结构化融合]

| 模块 | 功能 |
|------|------|
| **TimeBridge** | 构造强/弱归一化互补视图 |
| **时间教师路径** | 专精时间动态（小波+膨胀卷积+MLP-Mixer）|
| **变量学生路径** | 专精变量依赖（分解嵌入+Transformer）|
| **跨维度蒸馏** | "时间→变量"知识迁移 |
| **结构化融合** | 3×3成分级可解释交互 |

---

# TimeBridge：差异化归一化策略

> **核心洞察**：单一归一化无法同时满足时间建模和变量建模的需求

| 视图 | 归一化方式 | 目的 | 输入到 |
|------|-----------|------|-------|
| **强归一化** $X_{strong}$ | Instance Norm (IN) | 消除变量间统计差异，聚焦时间模式 | 时间教师路径 |
| **弱归一化** $X_{weak}$ | Layer Norm (LN) | 保留变量相对幅值，适配依赖建模 | 变量学生路径 |

- 📊 **消融验证**：TimeBridge vs. 单一LN/IN/ReViN → MSE降低 **3.2%**（ETTh1）
- 🎭 **互补视图**：两路径各取所需，避免"一归一化策略适配所有"的矛盾

---

# 双路径特征提取

**时间教师路径**（专注时间动态）：
1. 🌊 **多分辨率DWT（Daubechies-4小波）** → 低频趋势 + 高频细节分离
2. 🔧 **膨胀卷积（rate=2）** → 拼接特征，扩大感受野捕捉长程时间依赖
3. 🧬 **MLP-Mixer（3层，隐层256）** → 全局时间信息聚合，输出 $F_t \in \mathbb{R}^{B×T×D}$

**变量学生路径**（专注变量依赖）：
1. 📦 **分解嵌入（MovingAvg窗口=7）** → 趋势+季节独立嵌入（继承MV-FDE思想）
2. 🔗 **变量级自注意力（8头，2层Transformer）** → 跨变量动态依赖，输出 $F_v \in \mathbb{R}^{B×V×D}$

---

# 跨维度知识蒸馏：首次"时间→变量"迁移

> **突破**：传统KD是"大模型→小模型"，GraphTD实现"时间域→变量域"跨维度迁移

[Figure: 跨维度蒸馏示意图：时间教师（冻结）→ TimeBridge对齐 → 变量学生（学习）]

**三重蒸馏损失**：
- 🎯 **特征蒸馏** $\mathcal{L}_{feat}$：对齐时间特征与变量特征的表示空间
- 👁️ **注意力蒸馏** $\mathcal{L}_{attn}$：时间注意力模式指导变量注意力
- 📈 **预测蒸馏** $\mathcal{L}_{pred-kd}$：教师预测指导学生优化方向

$$\mathcal{L}_{distill} = \alpha\mathcal{L}_{feat} + \beta\mathcal{L}_{attn} + \gamma\mathcal{L}_{pred-kd}$$

- 💡 **目的**：增强（而非压缩），让变量路径具备**时间感知能力**

---

# 结构化时空融合：从黑盒到可解释

> **核心创新**：用 3×3 交互矩阵显式建模时间成分与变量成分的协同作用

**成分分解**：
- 时间成分：趋势（Trend）/ 季节（Seasonal）/ 残差（Residual）
- 变量成分：均衡（Equilibrium）/ 相关（Correlation）/ 滞后（Lag）

**3×3交互矩阵** = 9种协同模式，每格 $H_{i,j}$ 量化对应类型的交互强度

- 🔐 **正交约束** $\mathcal{L}_{orth}$：保证9个成分相互独立，避免信息重复
- 🔍 **可视化溯源**：ETTh1呈现**趋势主导**，Electricity呈现**季节性主导**

---

# GraphTD 主实验结果

[Figure: GraphTD vs. AdapTFormer vs. iTransformer 在5个数据集L=720场景的对比]

**关键数据**（L=720长步长场景）：
- 📉 **ETTh1**：GraphTD MSE **0.459** vs. AdapTFormer 0.468 → 降低 **1.9%**
- 📉 **Electricity**：GraphTD MSE **0.195** vs. AdapTFormer 0.203 → 降低 **3.9%**
- 🏆 5个数据集所有长步长场景（L=336/720）均取得最优
- 📈 **长步长优势加剧**：L增大→时间感知能力价值凸显

---

# GraphTD 消融实验

| 模块 | ETTh1 MSE | Electricity MSE |
|------|----------|----------------|
| 完整GraphTD | **0.459** | **0.195** |
| w/o 跨维度蒸馏 | 0.532 (+15.9%) | 0.228 |
| w/o 结构化融合 | 0.516 (+12.4%) | 0.216 |
| w/o TimeBridge | 0.474 (+3.3%) | 0.203 |
| 单路径（变量only）| 0.468 | 0.207 |

- 🔑 **跨维度蒸馏贡献最大**：提升 **15.9%**，是时间感知能力的核心来源
- 🎯 **结构化融合贡献**：提升 **12.0%**，可解释性与性能同步提升

---

# GraphTD 可解释性 & 鲁棒性

[Figure: 左：3×3融合矩阵可视化（ETTh1=趋势主导 vs. Electricity=季节性主导）| 右：鲁棒性噪声曲线]

**可解释性**：
- ETTh1融合矩阵：Trend-Equilibrium值最高 → **趋势主导预测**
- Electricity融合矩阵：Seasonal-Correlation值最高 → **季节性主导预测**

**鲁棒性**（噪声率 r∈{0.1→0.5}）：
- GraphTD在 r=0.5时 MSE仅 **0.637**
- vs. iTransformer: 0.857 (+34.7%)，Crossformer: 0.915 (+43.6%)
- 💪 **来源**：TimeBridge去噪 + 结构化分解锁定长期本质特征（趋势/均衡）

---

# 总结与贡献

| | AdapTFormer（第三章）| GraphTD（第四章）|
|--|---------------------|-----------------|
| **解决问题** | 注意力稀释 | 时空协同 + 可解释性 |
| **核心方法** | MV-FDE + SAM | 双路径 + 跨维度蒸馏 + 结构化融合 |
| **顶线性能** | Electricity MSE 0.203 (L=720) | ETTh1 MSE 0.459 (L=720) |
| **理论贡献** | 重构-增强范式 | 跨维度KD + 成分级可解释融合 |

**技术路线总结**：
- 单维度 → 多维度协同 ✅
- 黑盒预测 → 可解释建模 ✅
- 精度优先 → 性能-效率-鲁棒性平衡 ✅

> 🎓 感谢各位评委！欢迎提问与交流！
