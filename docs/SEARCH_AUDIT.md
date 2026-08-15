# ResNet 残差连接改进：检索与证据审计

> 检索截止：2026-08-13；范围版本：1.0；最终语料：55 项；本文件记录可重复的查询、筛选、版本去重、反证与覆盖收敛过程。

## 1. 范围与来源

研究问题是：从 2015 年前驱到截止日，卷积 ResNet 中直接改变捷径、残差合并、路径选择、跨块拓扑或恒等传播稳定条件的机制有哪些，它们的证据强度、代价和失败边界是什么？

来源优先级如下：

1. 会议或期刊正式页面与全文：CVF Open Access、NeurIPS Proceedings、PMLR、ICLR Proceedings、OpenReview 正式接收记录、AAAI、IJCAI、JMLR、Springer、IEEE DOI；
2. 与正式成果可消歧的作者预印本、arXiv、作者项目页与官方代码；
3. 二手索引只用于发现线索，不承担结论证据。

检索语言以英文为主，中文用于叙事与同义词回查。年份从 Highway Networks 的 2015 年正式前驱覆盖至 2026-08-13。数据库界面经常不提供稳定、可复现的总命中数，因此查询账本把 `hits` 记为 `null`，同时保留实际逐项筛选数；不以搜索引擎估算命中数伪装精确召回率。

## 2. 查询矩阵与执行轮次

发现检索按七个机制家族展开：恒等/投影、缩放/初始化/无归一化、随机路径、自适应路由、聚合拓扑、可逆连接、机制诊断/反证。随后进行近年前沿、前后向引文追踪和两个独立收敛轮次。

| 轮次 | 来源族 | 查询主题摘要 | 筛选 | 当轮纳入 | 边际新增率 |
|---|---|---|---:|---:|---:|
| 1 发现 | CVF、arXiv、DOI | `identity mapping shortcut projection downsampling` | 18 | 5 | 27.8% |
| 2 发现 | NeurIPS、PMLR、OpenReview | `initialization normalization signal propagation` | 22 | 7 | 31.8% |
| 3 发现 | Springer、NeurIPS、OpenReview、IEEE | `stochastic depth Swapout Shake-Shake ShakeDrop` | 14 | 4 | 28.6% |
| 4 发现 | CVF、PMLR、OpenReview | `adaptive computation routing skip block gate` | 19 | 6 | 31.6% |
| 5 发现 | CVF、NeurIPS、IEEE、arXiv | `multilevel residual dense dual path aggregation` | 21 | 7 | 33.3% |
| 6 发现 | NeurIPS、PMLR、OpenReview | `reversible invertible residual memory` | 13 | 4 | 30.8% |
| 7 发现与反证 | NeurIPS、PMLR、OpenReview | `ensemble shattered gradients singularities limitation` | 20 | 6 | 30.0% |
| 8 前沿 | CVF、NeurIPS、PMLR、OpenReview | `2023 2024 2025 residual connection ResNet CNN` | 26 | 2 | 7.7% |
| 9 引文补漏 | PMLR、NeurIPS、arXiv、引文链 | `Fixup SkipInit normalization-free scaling` | 24 | 4 | 16.7% |
| 10 前沿补漏 | CVF、NeurIPS、PMLR、ICLR | `2024 2025 2026 shortcut adaptive depth conditioning` | 31 | 2 | 6.5% |
| 11 独立收敛 | PMLR、NeurIPS、CVF、OpenReview | 七路线逐路组合查询 | 27 | 1 | 3.7% |
| 12 独立收敛 | NeurIPS、CVF、PMLR、OpenReview | `recent negative reproducibility 2024 2025 2026` | 29 | 0 | 0% |

精确 JSONL 查询记录、来源、日期、轮次与备注位于 `planning/search_ledger.jsonl`。同一候选可能在多个轮次出现，表中筛选数不能直接相加为独立文献数。

## 3. 纳入、排除与最终裁决

### 纳入规则

一项工作必须有可核验的一手入口，并至少直接改变以下一项：

- shortcut 映射、投影、下采样或滤波；
- 恒等路与残差路的合并系数、初始化、归一化条件或更新方向；
- 残差块是否执行以及随机或输入相关选择；
- 跨块、跨阶段、多分支的聚合拓扑；
- 通过耦合、状态增广或 Lipschitz 条件实现的可逆/信息保持连接；
- 对上述连接给出可定位的机制、负面结果或直接替代反证。

### 排除规则

- 仅以 ResNet 为骨干的应用；
- 只改变 `F(x)` 内部卷积、宽度、基数、感受野或注意力，`x + F(x)` 合并不变；
- 只改变训练配方、数据增强或网络规模；
- 仅属于 Transformer、U-Net、GNN、SNN、RNN 或生成式 residual stream；
- 已被同路线更完整条目覆盖、无法形成不可替代机制证据；
- 撤稿、匿名投稿、正式状态无法确认，或一手证据不足。

最终筛选表共 84 个候选：55 个纳入主地图，29 个排除。其中范围外 20 个、覆盖冗余 6 个、非目标成果类型 3 个。典型边界如下：

| 候选 | 最终处理 | 理由 |
|---|---|---|
| Wide ResNet、ResNeXt | 排除主体 | 改宽度或残差变换内部基数，连接规则不变 |
| SE、CBAM、Residual Attention | 排除主体 | 调制特征或残差分支，不改块级恒等捷径和合并原则 |
| Res2Net | 排除主体 | 多尺度层级位于 `F(x)` 内部 |
| DenseNet | 边界说明 | 是拼接式替代谱系，不是 ResNet 内部改进 |
| FractalNet、DiracNet | 背景/桥接 | 提供无显式捷径的直接反证，机制不可替代 |
| ReSet | 覆盖冗余 | 动态路由已由 BlockDrop、SkipNet、AIG、Adaptive Depth 覆盖 |
| Residual Flows | 排除主体 | 主要解决生成密度估计与 log-determinant |
| Lambda-Skip、CVPR 2026 生成式反证 | 排除主体 | 正式正文以序列或 ViT/生成模型为主，无 CNN-ResNet 主线证据 |
| IDInit、部分 Jacobian 初始化 | 覆盖冗余 | 通用初始化贡献，连接侧问题已由 Fixup/SkipInit/Stable ResNet 覆盖 |

完整逐项裁决、规范 ID、一手 URL 和备注位于 `planning/screening.csv`。

## 4. 去重与版本族审计

去重依次使用 DOI、会议/期刊稳定 ID、arXiv ID、规范题名和作者组合；同一成果优先正式且元数据最完整的版本。重要冲突及裁决如下：

| 版本族或冲突 | 最终裁决 |
|---|---|
| 原始 ResNet | 时间线可写 2015 预印本，规范实体使用 CVPR 2016 |
| Highway Networks / Training Very Deep Networks | 同一概念谱系，以 NeurIPS 2015 正式题名建单一前驱实体 |
| Weighted Residuals | arXiv 为两作者，正式 ICSAI 记录为三作者；采用正式三作者版本 |
| Inception-ResNet | 早期预印本/展示不重复计数，采用 AAAI 2017 正式版本 |
| RoR | 2016 预印本、2017 在线、2018 卷期属于同一论文；采用期刊实体 |
| RiR / RoR | 名称相似但机制不同；RiR 仅工作坊且最终不占主图，RoR 进入聚合路线 |
| Merge-and-Run | 早期 arXiv 五作者与 IJCAI 正式八作者同族；采用 2018 正式版本 |
| LM-ResNet | 2017 arXiv、2018 workshop 与 ICML 2018 同族；采用 PMLR 正式版本 |
| Shake-Shake | 保留 ICLR workshop 状态，不误标主会 |
| ShakeDrop | 采用 IEEE Access 2019 正式扩展版；早期 workshop 为前身 |
| iResNet 名称碰撞 | Behrmann 的可逆 i-ResNet 与 Duta 的改进型 iResNet 是不同成果；主地图仅保留前者并明确机制 |
| Scaling ResNets | 2022 预印本与 JMLR 2025 为同族；采用 JMLR 正式版本 |
| m-RevNet / Momentum ResNet | 不同作者的相近机制；前者只核得预印本，主图采用 ICML 2021 Momentum ResNet |

地图中 53 项为同行评议版本，2 项保持预印本状态并明确标识。出版状态只说明审核与版本成熟度，不替代对实验设计和负面结果的评价。

## 5. 全文、主张与证据向量审计

40 个核心条目逐项记录：2 至 5 步机制链、正文表图或章节位置、结果、局限、一手入口、出版状态与证据向量。桥接和背景项至少核验摘要、关键正文与正式元数据。证据向量四维为：

- `V`：来源与版本可核验程度；
- `D`：实验或理论设计对因果归因的支持程度；
- `P`：同行评议与出版成熟度；
- `Q`：结果透明度、重复运行、反例和复现信息。

主张账本包含 30 条高风险陈述，每条绑定论文 ID、原文位置、`V/D/P/Q` 和审核状态。重点不是给论文排总分，而是阻止四类偷换：累计改进冒充单因素改进，小型基准冒充 ImageNet 证据，作者系统结果冒充独立复现，代数性质冒充数值或系统性质。

## 6. 反证与负面结果专项审计

每条路线都补查 `failure`、`limitation`、`ablation`、`negative result`、`stability`、`reproducibility`、`same budget`、`wall-clock` 等词及相邻引文。最终叙事保留下列可定位反例：

- ResNet-D 的 D-only 边际不能从累积表中误读为约 0.95 点；可识别边际约 0.29 点；
- 强或全网抗混叠会显著降低准确率；
- Fixup-alone 在 ImageNet 没有追平 BN，SkipInit 又反证部分 Fixup 规则必须成套存在的说法；
- ReZero 在 ResNet-56 上变差；
- Stochastic Depth 在同 90 epoch ImageNet 略差于基线；
- SkipNet 的 soft 到 hard 路由失配可造成严重坍塌；
- RoR 四级或五级可能退化，PolyNet 的随机路径过早会妨碍收敛；
- NF-ResNet 的部分深模型运行会崩溃；
- Orthogonal Residual Update 在一个 CNN 设置略差且有吞吐开销，CNN 证据未到 ImageNet-1k；
- RevNet 需要重计算，i-ResNet 求逆昂贵，代数可逆网络可出现 exploding inverse；
- 浅路径集成、ODE、条件数等解释都有明确模型假设，不能当作唯一普适机制。

这些反例已同时进入 `atlas.json` 卡片局限、综合报告和 `planning/claim_ledger.csv`，避免负面证据只存在于内部笔记。

## 7. 覆盖饱和判断

操作性饱和通过五道闸门判断：

1. 七个独立查询家族均有可审计记录；
2. 七条路线均包含奠基或代表作、后续发展、反证或明确缺口；
3. 核心条目完成全文级机制、表图位置、结果与局限核验；
4. 版本冲突、撤稿、工作坊与预印本状态已单独审核；
5. 两个独立补漏轮次的边际新增率均低于 5%，分别为 3.7% 与 0%，且没有出现第八条路线。

第一轮唯一新增是 NeurIPS 2025 Orthogonal Residual Update。全文显示其 CNN-ResNet 仅在 CIFAR/Tiny ImageNet 验证，ImageNet-1k 只用于 ViT，因此严格放在 E1。第二轮返回的都是已知实体或 Transformer、U-Net、视觉语言、生成模型等语义伪候选。

这支持“主要机制路线已收敛”，不支持“所有论文已经穷尽”。任务特定微变体、低影响 venue、非英文数据库和截止日后的正式版本仍是更新风险。

## 8. 访问、复现与审计限制

- 部分出版页面只提供元数据或跳转；遇到这种情况，使用同一版本族的官方 PDF、作者预印本或正式 proceedings 页面交叉核对，没有用博客摘要替代证据。
- 搜索接口不稳定展示总命中量，所以只报告实际人工筛选数与决策，不声称数据库层面的精确召回率。
- 本次复现审计针对原论文的重复运行、代码入口、预算口径、消融和失败条件；没有重新训练 55 个模型，也没有把“存在官方代码”等同于可在当前硬件完整复现。
- 没有找到覆盖 PreAct、ResNet-D、残差缩放、SkipInit/ReZero、随机深度/Shake、聚合和可逆路线的现代独立统一复现。多数旧论文还缺置信区间、等算力、真实设备延迟或峰值内存。
- 生成式模型、Transformer 与任务特定残差技巧只用于检查边界，不应由其负面结果反推 CNN-ResNet 分类结论。

## 9. 审计产物索引

- `planning/research_contract.yaml`：冻结的范围合同；
- `planning/search_ledger.jsonl`：12 轮精确查询记录；
- `planning/screening.csv`：84 个候选的最终纳排；
- `planning/claim_ledger.csv`：30 条高风险主张的证据审计；
- `planning/agents/`：三份独立证据包；
- `atlas.json`：55 个最终实体的数据真源；
- `data/`：编译生成的 JSON、CSV、BibTeX 和离线网页数据。
