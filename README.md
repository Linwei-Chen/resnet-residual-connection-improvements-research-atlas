# ResNet 残差连接改进研究地图

<!-- generated-complete-readme:v1 -->
> 从恒等捷径到缩放、路由、聚合与可逆连接的证据导航

**完整文字版综述：** 本 README 已直接收录领域全貌、综合报告、检索与证据审计，以及全部 55 项工作的逐篇解读。无需打开网页即可阅读全部研究内容；[在线地图](https://linwei-chen.github.io/resnet-residual-connection-improvements-research-atlas/)仅提供可选的可视化筛选与机制图。

[在线地图](https://linwei-chen.github.io/resnet-residual-connection-improvements-research-atlas/) · [结构化真源](https://github.com/Linwei-Chen/resnet-residual-connection-improvements-research-atlas/blob/main/atlas.json) · [原始综合报告](docs/REPORT.md) · [原始检索审计](docs/SEARCH_AUDIT.md)

## 阅读导航

1. [研究全貌](#研究全貌)
2. [路线与层级](#路线与层级)
3. [综合报告](#综合报告)
4. [全部研究工作](#全部研究工作)
5. [检索与证据审计](#检索与证据审计)
6. [复现与使用边界](#复现与使用边界)

<a id="研究全貌"></a>
## 研究全貌

**领域概览：** 残差连接的演进并非一条从简单到复杂的单线竞赛。最稳定的共识是保持跨块近恒等传播并控制残差分支幅度；随机路径和输入自适应路由利用可跳过性换取正则化或计算效率；跨层聚合与可逆设计分别改变信息复用方式和存储/可逆性约束。地图按连接机制分流，再按证据从机制分析到跨任务系统验证分层，读者可同时比较收益、代价和没有被证明的部分。

**研究领域：** ResNet 残差连接改进

**核心判断：** 多数可迁移的改进不是削弱恒等捷径，而是让残差分支在训练早期更小、更可控，或在需要时有选择地参与计算；越复杂的门控、聚合与可逆约束越依赖特定任务和实现条件。

**阅读建议：** 先读原始 ResNet 与 pre-activation，再沿缩放稳定化路线理解 Fixup、SkipInit 与 NFNet；需要正则化、动态计算、特征复用或内存节省时，再分别进入随机路径、自适应路由、聚合拓扑和可逆路线。

**范围与纳排边界：** 覆盖 2015 年前驱至 2026-08-13，可由正式论文、DOI、作者入口或官方代码核验、且直接改变 ResNet 捷径路径、残差分支与主路径合并、路径选择、跨块拓扑或为恒等传播服务的稳定化机制的工作。仅把 ResNet 当骨干的应用、只改卷积算子/宽度/深度而不改变连接行为的变体，以及 Transformer、U-Net、GNN 等专属 skip 技巧不进入主体。少量直接替代、机制诊断和负面证据作为桥接或背景保留；不声称穷尽任务特定长尾。

**覆盖说明：** 已完成查询收敛、55 个实体核验、40 个核心条目全文深读、30 条高风险主张审计、独立终稿抽查，以及桌面、375px 手机、200% 缩放和 file:// 真实浏览器验收，达到 L2。

**目标读者：** 研究人员与具备基础背景的专业读者

**来源材料：** 正式会议/期刊页、DOI、arXiv 正文、作者项目与官方代码仓库

**地图中心标签：** 残差连接

**内容语言：** zh-CN

| 维度 | 数值 |
|---|---|
| 纳入成果 | 55 项 |
| 年份范围 | 2015—2025 |
| 研究路线 | 7 条 |
| 分析层级 | 4 个 |
| 覆盖等级 | L2 |
| 资料截止 | 2026-08-13 |
| 第二轴 | 最高已核验证据覆盖层级 |

<a id="路线与层级"></a>
## 路线与层级

| 路线（ID） | 收录 | 判定问题 | 说明 |
|---|---:|---|---|
| 恒等保真与投影 / 恒等/投影（`identity-projection`） | 6 | 它是否直接改善 identity shortcut 在算子次序、维度匹配或下采样处的保真度？ | 调整激活与归一化次序、通道/尺度变化方式或下采样捷径，使跨块信号尽量保持无阻传播。 |
| 残差更新控制与稳定化 / 更新/稳定（`residual-scaling`） | 12 | 它是否主要通过控制残差更新的幅度、方向或初始状态来稳定深度扩展？ | 通过固定或可学习缩放、方向投影、初始化、归一化替代与信号传播约束，控制残差分支注入恒等状态的幅度或方向。 |
| 随机路径正则 / 随机路径（`stochastic-paths`） | 4 | 它是否把残差连接的路径或合并系数随机化，并把随机性作为主要正则来源？ | 训练时随机丢弃、选择或扰动残差路径与多分支合并，以形成隐式子网络分布并抑制共适应。 |
| 自适应门控与路由 / 自适应路由（`adaptive-routing`） | 7 | 它是否让具体样本或特征决定 residual/shortcut 的权重、执行深度或路径？ | 依据输入、空间位置、通道或残差响应，自适应决定传递、重标定、执行或跳过哪些更新。 |
| 跨层聚合拓扑 / 聚合拓扑（`aggregation-topology`） | 9 | 它是否主要重构跨块、跨层或多分支的连接拓扑与信息复用方式？ | 把单块二路相加扩展为多层级捷径、多项式路径、稠密连接、双路复用或层次聚合。 |
| 可逆与信息保持 / 可逆连接（`reversible-connections`） | 6 | 它是否以可逆性、信息不丢失或深度无关激活存储作为连接设计的首要目标？ | 用可逆耦合或 Lipschitz 约束重写残差映射，以重建激活、保存信息或支持可逆生成。 |
| 机制诊断与边界 / 诊断/边界（`mechanism-limits`） | 11 | 它是否主要提供对连接机制的验证、反驳、风险或适用边界，而不是提出新架构？ | 用消融、谱分析、损失面、梯度或安全实验检验残差连接为何有效、何时失效以及常见解释的边界。 |

| 层级 | 说明 |
|---|---|
| 机制与理论 / E0（`0`） | 最高直接证据来自理论、初始化分析、简化模型或诊断性实验，尚无目标架构的大规模任务验证。 |
| 小型分类基准 / E1（`1`） | 在 MNIST、CIFAR、SVHN、Tiny ImageNet 等小型或中型分类基准上有直接结果。 |
| ImageNet 规模 / E2（`2`） | 在 ImageNet 级大规模分类或同等级视觉训练上有直接结果，但跨任务证据有限。 |
| 跨任务或系统级 / E3（`3`） | 除大规模分类外，还验证了检测、分割、生成、内存系统、安全或多个部署场景中的可迁移性与代价。 |

<a id="综合报告"></a>
## 综合报告

### ResNet 残差连接改进：领域全貌与路线地图

> 最终版；检索截止：2026-08-13；主地图收录 55 个无重复实体、7 条路线、4 个证据覆盖层级。

#### 执行结论

残差连接的演进不是“把捷径做得越来越复杂”的单线竞赛。跨越十年的一手证据更支持四个结论：

1. **恒等传播仍是最可靠的设计锚点。** 原始 ResNet 解决的是深层普通网络的优化退化；pre-activation 随后用直接消融说明，跨单元路径越接近严格恒等，优化通常越容易。任何门控、投影、滤波或多级聚合都应先回答：它是否破坏了这条低阻力通道？
2. **控制残差更新比单纯增加深度更关键。** 分支缩放、零初始化系数、随深度缩放的初始化、方差递推与方向约束，都可看作控制更新幅度或方向。它们显著改善可训练性，但“无归一化可训练”不等于“普遍追平 BatchNorm 的准确率与泛化”。
3. **随机与自适应跳块的价值取决于预算口径。** Stochastic Depth 在等训练轮数 ImageNet 上略差；SkipNet 的软硬路由失配可严重坍塌；动态模型减少 FLOPs 也未必减少墙钟时间。训练时长、路由开销和硬件利用率必须同时报告。
4. **可逆连接把激活内存换成了约束、重计算与数值风险。** 代数上的可逆性不保证逆运算稳定，也不等于零存储。表达能力、求逆成本、下采样处理和浮点误差是不可省略的评测轴。

目前证据最突出的空白不是再提出一个孤立的新门控，而是缺少一套独立、统一的现代复现：同训练配方、匹配参数与 FLOPs、多个随机种子，同时覆盖 ImageNet、跨任务、墙钟延迟、峰值内存与失效压力测试。

#### 范围合同

##### 研究问题

从 2015 年前驱到 2026-08-13，哪些工作直接改变了卷积 ResNet 的捷径路径、残差分支与主路径合并、路径选择、跨块拓扑，或直接服务于恒等传播的初始化与归一化机制？这些改变解决什么问题，证据能外推到哪里，代价和失败方式是什么？

##### 纳入边界

主体只收录可由正式论文、DOI、作者入口或官方代码核验，且至少满足下列一项的工作：

- 改变 shortcut 的算子、投影、下采样或滤波；
- 改变 `x + F(x)` 的幅度、方向、初始化或归一化条件；
- 随机或按输入选择残差块；
- 改变跨块、跨阶段或多分支聚合拓扑；
- 以连接形式实现可逆、信息保持或激活重构；
- 直接诊断上述连接为何有效、何时失败，或给出不依赖显式捷径的反证。

仅把 ResNet 当作骨干的应用论文不进入主体；只改卷积算子、宽度、深度或训练配方而没有可分离连接机制的变体不进入主体；Transformer、U-Net、GNN、SNN 专属技巧不进入主体。ResNeXt、SE、Residual Attention、Res2Net 等主要改变残差变换内部或特征重标定的工作，只在边界说明中出现。DenseNet、FractalNet、DiracNet 与 Residual Distillation 作为替代、诊断或训练部署解耦证据保留，不与“ResNet 连接改进”混为同一强度。

##### “全面”的操作定义

本地图完成七类查询家族、逐路线前向与后向引文追踪、正式版本优先去重、核心全文定位、负面结果专项检索，以及两个独立补漏轮次。最后两轮分别筛选 27 和 29 个候选，新增 1 和 0 个实体，边际新增率为 3.7% 与 0%，未出现新路线。因此可称为**机制路线上的操作性饱和**，但不声称穷尽任务特定、低影响或仅以预印本流通的长尾微变体。

#### 地图坐标系

##### 七条主要路线

| 路线 | 数量 | 核心问题 | 代表机制 | 主要风险 |
|---|---:|---|---|---|
| 恒等保真与投影 | 6 | 如何让跨块与跨尺度信号少受损？ | pre-activation、平滑通道变化、ResNet-D、选择性低通 | 投影与强低通会破坏恒等或高频 |
| 残差更新控制与稳定化 | 12 | 深度增加时如何控制更新幅度、方向与方差？ | residual scaling、Fixup、SkipInit、ReZero、NF-ResNet、正交更新 | 多个配套强耦合；可训练不等于更准 |
| 随机路径 | 4 | 能否以训练期路径采样兼顾正则化与深度？ | stochastic depth、Swapout、Shake 系列 | 预算不匹配、浅网无益、训练推理分布差异 |
| 自适应门控与路由 | 7 | 能否按样本决定执行哪些块？ | Highway gate、SACT、BlockDrop、SkipNet、AIG | 路由成本、软硬失配、训练复杂 |
| 跨层聚合拓扑 | 9 | 能否超越逐块串联的单尺度加法？ | RoR、PolyNet、DPN、DLA、Sparse/Mix、密集捷径 | 参数与宏结构混杂，层级过多可退化 |
| 可逆与信息保持 | 6 | 能否重构激活或保证映射可逆？ | RevNet coupling、i-RevNet、i-ResNet、Momentum | 表达约束、重计算、逆不稳定、仍需缓存 |
| 机制诊断与边界 | 11 | 捷径究竟改善了什么，是否是唯一方案？ | 路径展开、梯度相关性、奇异性、迭代推断、条件数、无捷径替代 | 理论常基于线性或简化模型，不能过度外推 |

`primary_route` 按论文首先解决的问题唯一归类；一篇工作可通过辅助标签连接其他路线。例如 ReZero 形式上使用门，但核心问题是初始更新幅度，所以归入稳定化。这样可避免把“门控”误当成天然更高级的单调轴。

##### 四级证据覆盖层级

| 层级 | 操作定义 | 数量 | 解释边界 |
|---|---|---:|---|
| E0 | 理论、机制或诊断，不要求完整任务基准 | 3 | 不能直接转写为精度结论 |
| E1 | CIFAR、Tiny ImageNet 或其他小型分类基准 | 16 | 可比较机制，不足以声称 ImageNet 普适 |
| E2 | 已核验 CNN-ResNet 的 ImageNet 规模证据 | 24 | 不自动代表跨任务或系统收益 |
| E3 | 检测、分割、视频、生成、跨域或系统级指标 | 12 | 仍需检查是否由同一作者和混合配方给出 |

层级表示**最高已核验证据外推范围**，不表示论文质量。严谨的小型基准机制论文可以是 A 级证据，但仍只能放在 E1。

#### 演进时间线

- **2015–2016：恒等与路径视角奠基。** Highway Networks 提供可学习变换门和携带门；[ResNet](https://openaccess.thecvf.com/content_cvpr_2016/html/He_Deep_Residual_Learning_CVPR_2016_paper.html) 用恒等捷径解决优化退化；[Identity Mappings](https://arxiv.org/abs/1603.05027) 把 pre-activation 与严格恒等路径固化。Stochastic Depth、Swapout、Weighted Residuals 和“展开为浅路径集合”的解释同时出现。
- **2017–2018：结构多样化。** Inception-ResNet scaling、Shake-Shake、PyramidNet、PolyNet、DPN、DLA、RoR、LM-ResNet、Merge-and-Run 等探索缩放、多分支和跨层聚合；BlockDrop、SkipNet、ConvNet-AIG 把跳块变成输入相关决策；RevNet、i-RevNet 与 Hamiltonian 结构开启可逆路线。
- **2019–2021：稳定化、系统成本与反证成熟。** Fixup、SkipInit、ReZero、NF-ResNet/NFNet、RescaleNet 把重点转向深度缩放、零初始化和无归一化信号传播；ResNet-D 与抗混叠重新审视下采样捷径；i-ResNet、Momentum ResNet 和 Exploding Inverses 明确可逆性的表达、速度与数值代价。
- **2023–2025：连接机制精化。** Residual Alignment 研究训练中残差与捷径的对齐现象；Gauss–Newton 条件数分析给出受限理论解释；Adaptive Depth 更新动态子路径；2025 年 Orthogonal Residual Update 把更新方向也纳入控制，但 CNN 证据仍只到小型基准。

#### 路线一：恒等保真、投影与下采样

原始块写作 `y = x + F(x)`。其价值首先是为优化提供一条低阻力路径，而不是由原始论文单独证明“彻底解决梯度消失”。pre-activation 的关键变化是把归一化和激活移入残差分支，使加法后的跨块路径更接近恒等。原文对门控、投影和非恒等 shortcut 的消融构成这一结论最直接的证据。

通道或空间尺寸变化时，严格恒等不再可行。PyramidNet 通过渐进增加通道减轻阶段边界突变；ResNet-B/C/D 调整下采样位置和捷径池化。这里必须避免一个常见误读：ResNet-D 在 B+C 之上的可辨识边际约为 **0.29 个 top-1 点**，不能把 B+C+D 的累计约 0.95 点全部归给 D，而且原文没有 baseline+D-only 或多随机种子。

BlurPool 与 critical-path anti-aliasing 提出另一个观察：stride shortcut 是混叠进入主干的关键路径。适度低通可改善平移一致性、鲁棒性和准确率，但 BlurPool 没有 shortcut-only 消融；后续关键路径研究还给出强反例——全层大核低通可把准确率显著压低。结论不是“所有下采样前都应滤波”，而是**只在临界降采样路径上，以保留任务所需带宽为约束地滤波**。

#### 路线二：残差更新控制与稳定化

这一族可统一写成 `x_{l+1} = a_l x_l + b_l F_l(x_l)`，或进一步约束 `F_l` 更新方向。方法差异在于 `a_l、b_l` 是常数、可学习标量、深度函数，还是由方差与几何条件推导。

- Inception-ResNet 发现很宽的残差块需要把残差乘约 0.1 至 0.3 才能稳定；Weighted Residuals 学习路径权重。
- Fixup 通过随深度缩放初始化和特定零初始化，在没有归一化时训练到 10,000 层；但 ImageNet ResNet-50 的 Fixup-alone error 为 27.6，明显差于 BN 的 23.6。它证明了可优化性，不证明普遍更好的泛化。
- SkipInit 与 ReZero 都让分支初始接近零。SkipInit 的复核说明 Fixup 的若干规则并非必须同时满足；ReZero 在 ResNet-56 上反而从 6.27 恶化到 6.58，收益随深度和初始化实现变化。
- NF-ResNet 结合方差传播、Scaled Weight Standardization 与 SkipInit，在五随机种子 ImageNet 比较中略高于对应 BN，但未正则化的更深模型五次有两次崩溃。NFNet 与 RescaleNet 的大规模结果很强，却都包含多个配套，无法归因于单一连接系数。
- Stable ResNet 与大深度缩放理论更清楚地解释为什么残差分支必须随深度缩小。Orthogonal Residual Update 则控制更新方向，在 CIFAR/Tiny ImageNet 多数改善，但没有 CNN-ResNet ImageNet-1k 证据，且吞吐开销约 3% 至 13%。

实践上，优先把这族方法视为**稳定性合同**：明确方差递推、初始恒等偏置、深度缩放和归一化条件，再比较准确率。不要把“能训练更深”直接当作“同预算更优”。

#### 路线三：随机路径正则

Stochastic Depth 训练时随机删除整个残差块、推理时恢复期望路径，兼有正则化和缩短平均反向路径的效果。它在 CIFAR-1202 上报告 4.91% error 与约 25% 训练提速，但 ImageNet 同 90 epoch 的 error 23.38 略差于 baseline 23.06，延长训练后才改善。公平比较必须同时锁定训练轮数与总算力。

Swapout 在单元级随机选择输入、残差或两者；Shake-Shake 和 ShakeDrop 在多分支间加入随机仿射混合。它们在 CIFAR 上强，但与具体多分支架构、正则强度和训练长度耦合。Shake-Shake 的代表版本属于 workshop；ShakeDrop 的主实体应采用 IEEE Access 2019 扩展版。出版状态不能替代证据质量，也不应被误标。

#### 路线四：自适应门控与路由

Highway Networks 是门控携带路径的前驱。SACT 把空间位置的计算步数也变成动态变量；BlockDrop、SkipNet、ConvNet-AIG 和 ε-ResNet 则按输入选择块。Adaptive Depth 把阶段拆成必选与可跳子路径，用自蒸馏帮助不同深度共享预测能力。

这一族最需要系统指标。BlockDrop 在 ResNet-101 ImageNet 保持 76.4% top-1 时报告平均约 20% 加速，并计入策略开销；但它依赖预训练、强化学习与课程学习。SkipNet 报告约减少 30% 计算，可是 soft 训练后直接 hard 推理可从约 90.83 跌到 66.67，训练时间通常还多 30% 至 40%。因此评测至少应同时报告：策略本身延迟、批量大小、执行稀疏度、实际设备墙钟、训练总算力，以及软硬路径的一致性。

#### 路线五：跨层与多分支聚合

RoR 在块、阶段和网络级叠加捷径；PolyNet 把组合结构解释为多项式；DPN 联合残差复用与稠密探索；DLA 用层级与迭代聚合连接不同尺度；SparseNet、MixNet、LM-ResNet、Merge-and-Run 和 Dense Shortcuts 分别探索稀疏、混合、线性多步、多列合并和密集短路。

它们证明“逐块串联加法”不是唯一可行拓扑，但单因素证据普遍弱于 pre-activation：宏结构、宽度、参数量、卷积类型与测试策略经常一起改变。RoR 三级通常最好，四级或五级可能低于基线；PolyNet 需要插入与缩放等稳定化配套，过早加入随机路径会妨碍收敛。DPN 与 DLA 的跨任务结果强，但更适合作为聚合范式桥接，而不是“某一种 shortcut 单独带来全部增益”的证据。

#### 路线六：可逆与信息保持连接

RevNet 把特征拆成两组，以 additive coupling 让激活可由输出重构，且不要求内部 `F/G` 可逆。它把随深度增长的激活存储换成重计算；ImageNet 精度接近同规模 ResNet，但下采样与首尾层仍需保存，通用实现计算约为原来的 1.5 至 2 倍。

i-RevNet 追求更完整的架构可逆；i-ResNet 对普通 residual map 施加 Lipschitz 小于 1 的充分条件，并用不动点迭代求逆。这带来三个代价：约束越强越可能损害分类表达，逆运算可比前向慢 5 至 20 倍，log-determinant 近似还存在截断偏差。Momentum ResNet 用额外状态构造闭式逆，但 bit buffer 说明它不是零存储。

[Exploding Inverses](https://proceedings.mlr.press/v130/behrmann21a.html) 给出关键反证：无正则模型可出现 `7.2×10^4` 条件数，仿射版本可达 `8.6×10^14` 并产生无穷重构误差。可逆网络因此必须分别验收**代数可逆、浮点重构、梯度一致性、逆的条件数、峰值内存和总耗时**。同维可逆网络还存在拓扑近似限制；增加维度或加入非可逆输出层可缓解，却改变了端到端可逆的承诺。

#### 路线七：机制解释、替代与边界

“残差网络像许多浅路径的集合”是有用的展开视角：删除单块通常只造成渐进退化，梯度也集中在较短路径。但共享权重和非线性意味着这些路径并不独立，不能严格宣称为指数多个相互独立的集成成员。

Shattered Gradients、奇异性分析、迭代推断、shortcut 收敛理论、Residual Alignment 与 Gauss–Newton 条件数从梯度相关、损失几何、表示细化和曲率等角度提供解释。它们互补而非互相替代：部分定理依赖深线性、浅层、非重叠卷积或奇异向量假设；Gauss–Newton 的 ResNet-20 诊断只用了约 1000 个 CIFAR 样本。任何机制表述都应标出适用模型。

FractalNet、DiracNet 和无捷径替代说明显式加法 shortcut 不是训练深网的逻辑必要条件；Residual Distillation 更进一步，把训练时教师的捷径优势迁移给部署时的普通 CNN。但这些结果不证明普通网络从头训练可普遍替代 ResNet，也不否定捷径作为工程默认值的稳健性。

#### 关键负面证据与常见误读

| 常见表述 | 一手证据要求的修正 |
|---|---|
| “ResNet 原论文证明解决了梯度消失” | 原论文直接展示的是深层普通网络的优化退化；具体梯度机制由后续工作补充。 |
| “ResNet-D 提升约 0.95 点” | 0.95 是 B+C+D 累计变化；D 在 B+C 上的可辨识边际约 0.29 点。 |
| “低通越强，平移稳健性越高” | 全层大核滤波可严重降准；应限于临界降采样路径并验证带宽。 |
| “Fixup 已无条件替代 BN” | Fixup-alone 的 ImageNet 结果明显落后 BN；强增强与配套会改变结论。 |
| “零初始化门总能改善 ResNet” | ReZero 在 ResNet-56 上变差；收益依深度、块型与实现。 |
| “Stochastic Depth 同预算更准” | 同 90 epoch ImageNet 略差；延长训练后才改善。 |
| “动态跳块减少 FLOPs 就会加速” | 路由开销、批处理碎片和软硬失配可抵消收益。 |
| “更多捷径层级总是更好” | RoR 四级或五级可退化；PolyNet 也需要稳定化配套。 |
| “可逆网络无需激活内存且数值可靠” | 首尾、下采样或 bit buffer 仍占内存；逆可能爆炸，且重计算显著。 |
| “残差路径就是独立浅网集成” | 路径共享参数并由非线性耦合，只能作为启发式视角。 |

#### 如何选择

| 目标 | 优先核查 | 不应省略的验收 |
|---|---|---|
| 稳健训练标准 CNN | 先保护恒等路径，再比较 pre-activation、零初始化分支与适度深度缩放 | 同训练配方、多随机种子、等参数与等轮数 |
| 无 BN 或极小批量 | SkipInit、NF-ResNet、RescaleNet 路线 | 崩溃率、正则化敏感性、推理精度，不能只看能否收敛 |
| 改善下采样与平移稳健性 | ResNet-D 与临界路径低通 | shortcut-only 消融、频带敏感性、ImageNet-C/P 与标准精度 |
| 按样本节省计算 | BlockDrop、SkipNet、ConvNet-AIG、Adaptive Depth | 真实设备延迟、策略开销、训练总算力、批量与软硬一致性 |
| 降低训练激活内存 | RevNet、i-RevNet、Momentum ResNet | 峰值内存、重计算比、浮点重构误差、逆条件数、不可逆边界层 |
| 探索新连接机制 | 幅度、方向、路径选择和聚合应分别做因果消融 | 现代基线、匹配预算、负面压力测试和跨任务复现 |

#### 推荐阅读路径

1. **先建立问题定义：** ResNet → Identity Mappings → Stochastic Depth。三篇分别说明优化退化、恒等传播和训练期路径随机化。
2. **再理解稳定化：** Fixup → SkipInit → NF-ResNet → Stable ResNet。阅读时把可训练性、准确率和多配套归因分开。
3. **研究计算自适应：** BlockDrop → SkipNet → Adaptive Depth。重点看墙钟、训练成本与软硬路径差异。
4. **研究拓扑：** RoR → PolyNet → DPN/DLA。把连接形式与宏结构、宽度和测试增强拆开。
5. **研究内存与可逆性：** RevNet → i-ResNet → Exploding Inverses → Approximation Limits。顺序对应“能重构、如何保证、何时数值失败、表达边界”。
6. **校准机制叙事：** Unraveled Ensemble → Shattered Gradients → Residual Alignment → Gauss–Newton Conditioning，再用 FractalNet、DiracNet 与 Residual Distillation 检查“捷径是否必要”的边界。

网页中的每张卡片均包含一手入口、2 至 5 步机制链、表图或章节定位、证据向量、局限与纳入级别，可从宏观路线继续下钻到单篇工作。

#### 覆盖、复现与更新限制

- 55 个实体中，40 个为核心、12 个为桥接、3 个为背景；53 个有同行评议正式版本，2 个仅以预印本状态纳入并明确标识。
- 证据层级分布为 E0 3 个、E1 16 个、E2 24 个、E3 12 个。证据级别 A/B/C 是质量判断，E0–E3 是外推范围，两者不可混用。
- 检索优先使用会议、期刊、DOI、作者项目与官方代码。对受限页面采用同一正式论文的一手替代入口；没有用聚合博客或搜索摘要代替结论证据。
- 旧论文常缺多随机种子、置信区间、严格等算力与真实硬件指标；跨论文绝对精度不适合作直接排行榜。
- 最新工作可能在 2026-08-13 后出现正式版本或新的独立复现。更新时应先核对版本族，再按同一纳排边界和两轮低新增率标准重新验收。

详细查询、筛选、去重、版本冲突与失败入口见 [检索与证据审计](#检索与证据审计)；结构化原始结论见项目根目录的 `atlas.json` 与 `planning/claim_ledger.csv`。

<a id="全部研究工作"></a>
## 全部研究工作（55 项）

以下条目严格保持 `atlas.json` 的策展顺序，并在 README 内直接列出问题、机制、证据、局限、启示、核验边界与全部可用来源。

<a id="paper-resnet-2016"></a>
**1. 用于图像识别的深度残差学习｜Deep Residual Learning for Image Recognition（2016 · CVPR 2016）**

**作者：** Kaiming He、Xiangyu Zhang、Shaoqing Ren、Jian Sun

**书目：** 年份 2016；载体 CVPR 2016；状态 同行评议；来源类型 paper

**分类：** 主路线 恒等保真与投影；相关路线 恒等保真与投影；层级 跨任务或系统级；阅读层级 核心；证据等级 A；简称 ResNet；相关性排序 1

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P3 / Q=Q3

**标签：** 基线、恒等捷径、投影

**定位：** 尺寸相同时使用 y=x+F(x)，尺寸变化时才投影或补零；原始单元相加后仍有 ReLU。

**问题：** 更深 plain network 的训练误差反而上升；论文把它界定为优化退化，而不只是过拟合。

**机制：** 尺寸相同时使用 y=x+F(x)，尺寸变化时才投影或补零；原始单元相加后仍有 ReLU。

**步骤：**

1. 学习残差 F(x)
2. 形状相同时与恒等 x 相加
3. 形状变化时投影或补零
4. 相加后施加 ReLU

**证据：**

- plain-34 比 plain-18 更难优化，而 ResNet-34 同时降低训练与测试误差。
- 只在升维处投影与全投影 top-1 error 相差约 0.33 点。
- 1202 层 CIFAR 模型训练误差极低但测试更差。

**局限：**

- 全网仍有非恒等 transition 和 post-add ReLU。
- 投影比较未严格匹配参数/计算，不能断言投影一概有害。

**意义：**

- 定义 residual branch、identity shortcut 和 projection shortcut 三个后续研究对象。

**边界：** 采用 CVPR 正式版；arXiv:1512.03385 是 2015 先行稿。

**工作族：** resnet-v1-v2

**标识：** DOI 10.1109/CVPR.2016.90

**证据位置：**

- §3.2，式(1)–(2)
- Figure 4
- Tables 2–3、6

**资源：** [一手入口](<https://openaccess.thecvf.com/content_cvpr_2016/html/He_Deep_Residual_Learning_CVPR_2016_paper.html>)

---

<a id="paper-identity-mappings-2016"></a>
**2. 深度残差网络中的恒等映射｜Identity Mappings in Deep Residual Networks（2016 · ECCV 2016）**

**作者：** Kaiming He、Xiangyu Zhang、Shaoqing Ren、Jian Sun

**书目：** 年份 2016；载体 ECCV 2016；状态 同行评议；来源类型 paper

**分类：** 主路线 恒等保真与投影；相关路线 恒等保真与投影；层级 ImageNet 规模；阅读层级 核心；证据等级 A；简称 Pre-activation ResNet；相关性排序 2

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q3

**标签：** 预激活、无遮挡梯度

**定位：** 将 BN/ReLU 移到卷积前，相加后不再激活，使前向加性展开和反向直接项成立。

**问题：** post-add ReLU 与非恒等 shortcut 会阻断跨多块的直接传播。

**机制：** 将 BN/ReLU 移到卷积前，相加后不再激活，使前向加性展开和反向直接项成立。

**步骤：**

1. 移动 BN/ReLU 到权重层前
2. 相加后保持恒等
3. 展开跨块加性路径
4. 保留不经权重层的梯度项

**证据：**

- 门控、0.5 缩放、全 1×1 与 dropout shortcut 都劣于恒等基线。
- full pre-activation 在 CIFAR-110/164 消融中最好。
- ImageNet-152 只改善约 0.2 点，200 层约 1.1 点。

**局限：**

- 严格推导不覆盖 transition units。
- BN 位置与正则化同时变化，不能把全部收益归给捷径。

**意义：**

- 建立保持捷径简单、把变换留在残差支路的核心原则。

**边界：** 书目采用 ECCV 2016；公开正文为同族 arXiv，不重复计数。

**工作族：** resnet-v1-v2

**标识：** DOI 10.1007/978-3-319-46493-0\_38

**证据位置：**

- §2，式(3)–(5)
- Tables 1–3、5

**资源：** [一手入口](<https://arxiv.org/abs/1603.05027>) · [代码](<https://github.com/KaimingHe/resnet-1k-layers>)

---

<a id="paper-pyramidnet-2017"></a>
**3. 深度金字塔残差网络｜Deep Pyramidal Residual Networks（2017 · CVPR 2017）**

**作者：** Dongyoon Han、Jiwhan Kim、Junmo Kim

**书目：** 年份 2017；载体 CVPR 2017；状态 同行评议；来源类型 paper

**分类：** 主路线 恒等保真与投影；相关路线 恒等保真与投影；层级 ImageNet 规模；阅读层级 桥接；证据等级 B；简称 PyramidNet；相关性排序 3

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** 补零捷径、平滑升维

**定位：** 逐块平滑增加通道；旧通道沿 identity 保留，新通道由 residual branch 生成，捷径用补零匹配。

**问题：** 阶段边界突然加倍通道，使少数 transition blocks 承担剧烈变换。

**机制：** 逐块平滑增加通道；旧通道沿 identity 保留，新通道由 residual branch 生成，捷径用补零匹配。

**步骤：**

1. 逐块增加通道
2. 恒等保留旧通道
3. 新增维度补零
4. 残差支路生成新信息

**证据：**

- 逐块移除实验中 transition 敏感性峰值减弱。
- identity mapping + zero-pad 优于全投影组合。
- ImageNet-200 相比作者重训基线约改善 1.2 点。

**局限：**

- 宽度、补零与 block 顺序同时变，无法单因素归因。
- 新增通道不含前块恒等内容。

**意义：**

- 把阶段转换确立为恒等路线的断点。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.1109/CVPR.2017.668

**证据位置：**

- §2.1–2.2
- Figure 4
- Tables 2–3、5

**资源：** [一手入口](<https://openaccess.thecvf.com/content_cvpr_2017/html/Han_Deep_Pyramidal_Residual_CVPR_2017_paper.html>)

---

<a id="paper-resnet-d-2019"></a>
**4. 卷积网络图像分类技巧中的 ResNet-D｜Bag of Tricks for Image Classification with Convolutional Neural Networks（2019 · CVPR 2019）**

**作者：** Tong He、Zhi Zhang、Hang Zhang、Zhongyue Zhang、Junyuan Xie、Mu Li

**书目：** 年份 2019；载体 CVPR 2019；状态 同行评议；来源类型 paper

**分类：** 主路线 恒等保真与投影；相关路线 恒等保真与投影；层级 ImageNet 规模；阅读层级 核心；证据等级 A；简称 ResNet-D；相关性排序 4

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 下采样、投影捷径

**定位：** ResNet-D 将捷径改为 2×2 average-pool stride 2 后接 stride-1 的 1×1 projection。

**问题：** stride-2 的 1×1 shortcut 只采样每个 2×2 区域的一个位置。

**机制：** ResNet-D 将捷径改为 2×2 average-pool stride 2 后接 stride-1 的 1×1 projection。

**步骤：**

1. 捷径先平均池化
2. 再做 stride-1 投影
3. 与主支路对齐相加

**证据：**

- Figure 2 明确区分 B/C/D 干预位置。
- Table 5 为累积消融，D 在 B+C 上可识别边际约 +0.29 top-1，而非累计 +0.95。

**局限：**

- 没有 baseline+D-only 或多随机种子。
- 平均池化的高频代价未在本文评估。

**意义：**

- 使阶段转换捷径保真成为独立问题。

**边界：** 只把 ResNet-D shortcut 纳入连接贡献；B 改主支路，C 改 stem。

**标识：** DOI 10.1109/CVPR.2019.00065

**证据位置：**

- §4.2，Figure 2
- Table 5

**资源：** [一手入口](<https://openaccess.thecvf.com/content_CVPR_2019/html/He_Bag_of_Tricks_for_Image_Classification_with_Convolutional_Neural_Networks_CVPR_2019_paper.html>)

---

<a id="paper-blurpool-2019"></a>
**5. 让卷积网络重新获得平移稳定性｜Making Convolutional Networks Shift-Invariant Again（2019 · ICML 2019）**

**作者：** Richard Zhang

**书目：** 年份 2019；载体 ICML 2019；状态 同行评议；来源类型 paper

**分类：** 主路线 恒等保真与投影；相关路线 恒等保真与投影、机制诊断与边界；层级 跨任务或系统级；阅读层级 桥接；证据等级 B；简称 BlurPool；相关性排序 5

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P3 / Q=Q2

**标签：** 抗混叠、平移稳定

**定位：** 将 stride 运算拆成稠密运算、固定低通和抽样；官方 ResNet 也把捷径改成 BlurPool+stride-1 projection。

**问题：** 未充分低通的 stride 运算让小平移引起离散输出突变，transition shortcut 同样受影响。

**机制：** 将 stride 运算拆成稠密运算、固定低通和抽样；官方 ResNet 也把捷径改成 BlurPool+stride-1 projection。

**步骤：**

1. 拆分稠密运算与抽样
2. 抽样前低通
3. 处理主支路与 shortcut
4. 评估一致性与鲁棒性

**证据：**

- ResNet-50 的平移一致性与准确率随合适低通改善。
- ImageNet-C/P 指标改善。
- 官方代码核实 shortcut 被直接改写。

**局限：**

- 无 shortcut-only 消融。
- 更强低通会损失高频，后续论文给出反例。

**意义：**

- 把 ResNet-D 的经验推广为采样理论。

**边界：** 机制与指标来自 ICML 正文；shortcut 实现由作者代码交叉核验。

**证据位置：**

- §3，式(4)–(6)
- Figure 6
- Table 2
- 官方 resnet.py

**资源：** [一手入口](<https://proceedings.mlr.press/v97/zhang19a.html>) · [代码](<https://github.com/adobe/antialiased-cnns>)

---

<a id="paper-critical-path-aliasing-2021"></a>
**6. 混叠对深度卷积网络泛化的影响｜Impact of Aliasing on Generalization in Deep Convolutional Networks（2021 · ICCV 2021）**

**作者：** Cristina Vasconcelos、Hugo Larochelle、Vincent Dumoulin、Rob Romijnders、Nicolas Le Roux、Ross Goroshin

**书目：** 年份 2021；载体 ICCV 2021；状态 同行评议；来源类型 paper

**分类：** 主路线 恒等保真与投影；相关路线 恒等保真与投影、机制诊断与边界；层级 跨任务或系统级；阅读层级 核心；证据等级 A；简称 Critical-path anti-aliasing；相关性排序 6

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**标签：** 临界路径、独立反证

**定位：** 只在 subsampling 前可学习支持不足的 critical path 加低通；stride-2 shortcut 改成 stride-1 projection 加滤波。

**问题：** 全网 BlurPool 可能过早删除可学习高频，关键是临界路径而非滤波越强越好。

**机制：** 只在 subsampling 前可学习支持不足的 critical path 加低通；stride-2 shortcut 改成 stride-1 projection 加滤波。

**步骤：**

1. 定位临界路径
2. 判断可学习支持
3. 只在不足处低通
4. 单独审计 shortcut

**证据：**

- shortcut 滤波三次均值约提升 0.6–0.7 点，全部临界路径约提升 1 点。
- 全层 k=7 可降到约 68%，pre+post 甚至约 61.5%。

**局限：**

- 最佳核与放置依赖数据和骨干。
- 跨论文汇总表不算统一协议。

**意义：**

- 把抗混叠修订为位置敏感的 shortcut 设计原则。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.1109/ICCV48922.2021.01036

**证据位置：**

- §3–4.3，Figure 3
- Tables 1–2

**资源：** [一手入口](<https://openaccess.thecvf.com/content/ICCV2021/html/Vasconcelos_Impact_of_Aliasing_on_Generalization_in_Deep_Convolutional_Networks_ICCV_2021_paper.html>)

---

<a id="paper-inception-resnet-scaling-2017"></a>
**7. Inception-v4、Inception-ResNet 与残差连接对学习的影响｜Inception-v4, Inception-ResNet and the Impact of Residual Connections on Learning（2017 · AAAI 2017）**

**作者：** Christian Szegedy、Sergey Ioffe、Vincent Vanhoucke、Alexander A. Alemi

**书目：** 年份 2017；载体 AAAI 2017；状态 同行评议；来源类型 paper

**分类：** 主路线 残差更新控制与稳定化；相关路线 残差更新控制与稳定化；层级 ImageNet 规模；阅读层级 桥接；证据等级 B；简称 Inception-ResNet scaling；相关性排序 7

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** 固定缩放、宽残差

**定位：** 相加前将 F(x) 固定乘 0.1–0.3，保留 x 恒等传播并限制单步扰动。

**问题：** 极宽 Inception-ResNet 的残差通道超过约 1000 时会早期失稳。

**机制：** 相加前将 F(x) 固定乘 0.1–0.3，保留 x 恒等传播并限制单步扰动。

**步骤：**

1. 计算宽残差 F(x)
2. 乘固定小系数
3. 与恒等状态相加

**证据：**

- 缩放小节记录未缩放模型激活死亡，而降低学习率或增加 BN 不可靠。
- 残差版本收敛更快，但同规模终点差异很小。

**局限：**

- 没有 α 单因素表或多随机种子。
- 不是标准 ResNet，宏结构混杂。

**意义：**

- 提供早期大规模 residual scaling 经验。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.1609/aaai.v31i1.11231

**证据位置：**

- Scaling of the Residuals
- Table 1

**资源：** [一手入口](<https://ojs.aaai.org/index.php/aaai/article/view/11231>)

---

<a id="paper-weighted-residuals-2016"></a>
**8. 超深网络的加权残差｜Weighted Residuals for Very Deep Networks（2016 · ICSAI 2016）**

**作者：** Falong Shen、Rui Gan、Gang Zeng

**书目：** 年份 2016；载体 ICSAI 2016；状态 同行评议；来源类型 paper

**分类：** 主路线 残差更新控制与稳定化；相关路线 残差更新控制与稳定化；层级 小型分类基准；阅读层级 桥接；证据等级 B；简称 Weighted Residuals；相关性排序 8

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** 可学习标量、零初始化

**定位：** 使用 x\_{l+1}=x\_l+λ\_lF\_l(x\_l)，λ 零初始化、约束在 (-1,1)，并移除 post-add ReLU。

**问题：** 超深残差叠加与 post-add 非线性仍妨碍优化，希望学习每块更新步长。

**机制：** 使用 x\_{l+1}=x\_l+λ\_lF\_l(x\_l)，λ 零初始化、约束在 (-1,1)，并移除 post-add ReLU。

**步骤：**

1. 移除相加后 ReLU
2. 加入分支标量 λ
3. λ 零初始化
4. 投影 SGD 约束 λ

**证据：**

- 1192 层 CIFAR-10 WResNet 报告 94.9%，联合 dropout 95.3%。
- 正文展示收敛与学习权重。

**局限：**

- 只测 CIFAR-10，最佳结果混入 dropout/stochastic depth。
- 正式三作者与 arXiv 两作者不同。

**意义：**

- 是 SkipInit/ReZero 前的零标量先驱。

**边界：** 书目采用 ICSAI 正式三作者版；arXiv:1605.08831 是两作者初稿。

**标识：** DOI 10.1109/ICSAI.2016.7811085

**证据位置：**

- §3.1–3.3，式(2)
- Figure 6
- Table 2

**资源：** [一手入口](<https://doi.org/10.1109/ICSAI.2016.7811085>)

---

<a id="paper-how-to-start-2018"></a>
**9. 如何开始训练：初始化与架构的作用｜How to Start Training: The Effect of Initialization and Architecture（2018 · NeurIPS 2018）**

**作者：** Boris Hanin、David Rolnick

**书目：** 年份 2018；载体 NeurIPS 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 残差更新控制与稳定化；相关路线 残差更新控制与稳定化、机制诊断与边界；层级 机制与理论；阅读层级 桥接；证据等级 B；简称 Residual scaling theory；相关性排序 9

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** 初始化理论、激活尺度

**定位：** 分析激活长度均值/方差递推，并为 residual modules 设置随深度控制的尺度。

**问题：** 传统权重方差规则不自动保证极深残差网的初始函数尺度稳定。

**机制：** 分析激活长度均值/方差递推，并为 residual modules 设置随深度控制的尺度。

**步骤：**

1. 推导初始长度递推
2. 识别残差爆炸条件
3. 按深度缩放模块
4. 用启动训练实验核对

**证据：**

- 对全连接、卷积和残差架构给出严格初始条件。
- 常见初始化违反条件，正确缩放可启动更深训练。

**局限：**

- 聚焦训练起点，不证明终点泛化。
- 分析整个模块而非新 shortcut 拓扑。

**意义：**

- 为 Fixup 与 Stable ResNet 提供理论背景。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- 主要定理与 ResNet 推论
- 可训练性实验

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2018/hash/d81f9c1be2e08964bf9f24b15f0e4900-Abstract.html>)

---

<a id="paper-fixup-2019"></a>
**10. Fixup 初始化：无归一化的残差学习｜Fixup Initialization: Residual Learning Without Normalization（2019 · ICLR 2019）**

**作者：** Hongyi Zhang、Yann N. Dauphin、Tengyu Ma

**书目：** 年份 2019；载体 ICLR 2019；状态 同行评议；来源类型 paper

**分类：** 主路线 残差更新控制与稳定化；相关路线 残差更新控制与稳定化；层级 ImageNet 规模；阅读层级 核心；证据等级 A；简称 Fixup；相关性排序 10

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q3

**标签：** 无归一化、深度缩放、负面结果

**定位：** 最后层置零，其余权重乘 L^{-1/(2m-2)}，并加入分支 multiplier 与标量 bias。

**问题：** 无归一化 ResNet 的单步函数更新会随深度失控。

**机制：** 最后层置零，其余权重乘 L^{-1/(2m-2)}，并加入分支 multiplier 与标量 bias。

**步骤：**

1. 分支末层和分类层置零
2. 按深度缩放其余权重
3. 加入 multiplier
4. 层前加入 bias

**证据：**

- CIFAR 上最深至 10,000 层可启动训练。
- ImageNet ResNet-50 Fixup-alone error 27.6，明显差于 BN 23.6；加 mixup 为 24.0。
- ResNet-101 Fixup+mixup 仍差 BN+mixup 约 0.6。

**局限：**

- 可优化不等于追平 BN 泛化。
- SkipInit 后续证明部分规则非必要。

**意义：**

- 证明归一化不是优化的逻辑必要条件，但其正则化功能未被完全替代。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- §3
- Figure 3
- Tables 1–2

**资源：** [一手入口](<https://openreview.net/forum?id=H1gsz30cKX>)

---

<a id="paper-skipinit-2020"></a>
**11. 批归一化使深层残差块偏向恒等函数｜Batch Normalization Biases Residual Blocks Towards the Identity Function in Deep Networks（2020 · NeurIPS 2020）**

**作者：** Soham De、Samuel L. Smith

**书目：** 年份 2020；载体 NeurIPS 2020；状态 同行评议；来源类型 paper

**分类：** 主路线 残差更新控制与稳定化；相关路线 残差更新控制与稳定化、机制诊断与边界；层级 ImageNet 规模；阅读层级 核心；证据等级 A；简称 SkipInit；相关性排序 11

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P2 / Q=Q3

**标签：** BN 机制、零门

**定位：** 测量 BN 分支相对捷径的方差衰减；在无 BN 分支末端加入零初始化 α。

**问题：** 需要拆分 BN 在 ResNet 中的恒等偏置、稳定性和小批量正则化作用。

**机制：** 测量 BN 分支相对捷径的方差衰减；在无 BN 分支末端加入零初始化 α。

**步骤：**

1. 测量残差/捷径相对方差
2. 加入分支标量 α
3. α 零初始化
4. 比较稳定与泛化

**证据：**

- 深度 1000 时 SkipInit 保持可训练，α=1 和简单 1/√2 失败。
- 去 weight decay 后精度显著下降。
- 复核显示 Fixup 最后层置零或深度缩放任选其一即可稳定。

**局限：**

- 主证据以 CIFAR 和 ResNet-v2 为主。
- BN 在小批量下仍有泛化优势。

**意义：**

- 把恒等起点转为可测机制并独立修订 Fixup。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- 理论推导
- Table 1
- Figure 3
- Fixup 复核

**资源：** [一手入口](<https://proceedings.neurips.cc/paper/2020/hash/e6b738eca0e6792ba8a9cbcba6c1881d-Abstract.html>)

---

<a id="paper-rezero-2021"></a>
**12. ReZero：大深度网络的快速收敛｜ReZero is All You Need: Fast Convergence at Large Depth（2021 · UAI 2021）**

**作者：** Thomas Bachlechner、Bodhisattwa Prasad Majumder、Henry Mao、Gary Cottrell、Julian McAuley

**书目：** 年份 2021；载体 UAI 2021；状态 同行评议；来源类型 paper

**分类：** 主路线 残差更新控制与稳定化；相关路线 残差更新控制与稳定化；层级 小型分类基准；阅读层级 核心；证据等级 B；简称 ReZero；相关性排序 12

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q3

**标签：** 动态等距、零门、反例

**定位：** 每块用 x+αF(x)，α 可学习且初值为零。

**问题：** 深网初始 Jacobian 偏离动态等距，希望用最小连接修改恢复恒等起点。

**机制：** 每块用 x+αF(x)，α 可学习且初值为零。

**步骤：**

1. 加入单标量 α
2. α 初始化为 0
3. 先学习门再启用分支
4. 保留恒等捷径

**证据：**

- 补充 Table 2 报告多次运行与标准误。
- ResNet-56 error 从 6.27 变成 6.58，反而恶化；更深 pre-activation 模型多改善。
- 结果对 PyTorch 默认初始化版本敏感。

**局限：**

- CNN 证据主要是 CIFAR。
- 与 SkipInit 方程近同构，收益不单调。

**意义：**

- 强化零门谱系并揭示实现敏感性。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- 模型定义
- 补充 D.7、E，Table 2

**资源：** [一手入口](<https://proceedings.mlr.press/v161/bachlechner21a.html>)

---

<a id="paper-stable-resnet-2021"></a>
**13. 稳定残差网络｜Stable ResNet（2021 · AISTATS 2021）**

**作者：** Soufiane Hayou、Eugenio Clerico、Bobby He、George Deligiannidis、Arnaud Doucet、Judith Rousseau

**书目：** 年份 2021；载体 AISTATS 2021；状态 同行评议；来源类型 paper

**分类：** 主路线 残差更新控制与稳定化；相关路线 残差更新控制与稳定化；层级 小型分类基准；阅读层级 核心；证据等级 A；简称 Stable ResNet；相关性排序 13

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 1/√L、无限宽

**定位：** 令 y\_l=y\_{l-1}+λ\_lF\_l，要求 Σλ² 有界；均匀方案为 1/√L，也给递减序列。

**问题：** 未缩放的无限深 ResNet 相关核会退化，梯度上界随深度指数增长。

**机制：** 令 y\_l=y\_{l-1}+λ\_lF\_l，要求 Σλ² 有界；均匀方案为 1/√L，也给递减序列。

**步骤：**

1. 设置每层 λ
2. 约束 Σλ²
3. 构造均匀或递减缩放
4. 比较核、梯度与任务

**证据：**

- Proposition 1 的深度因子由 exp(cΣλ²) 控制。
- CIFAR/Tiny ImageNet 三次运行中稳定缩放通常优于 unscaled。

**局限：**

- 核心定理依赖无限宽初始化极限。
- 无 ImageNet-1k，有限宽并非充分必要。

**意义：**

- 把经验缩放提升为大深度理论条件。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- Proposition 1
- Tables 2–3

**资源：** [一手入口](<https://proceedings.mlr.press/v130/hayou21a.html>)

---

<a id="paper-nf-resnet-2021"></a>
**14. 用信号传播分析弥合无归一化 ResNet 的性能差距｜Characterizing Signal Propagation to Close the Performance Gap in Unnormalized ResNets（2021 · ICLR 2021）**

**作者：** Andrew Brock、Soham De、Samuel L. Smith

**书目：** 年份 2021；载体 ICLR 2021；状态 同行评议；来源类型 paper

**分类：** 主路线 残差更新控制与稳定化；相关路线 残差更新控制与稳定化、恒等保真与投影；层级 ImageNet 规模；阅读层级 核心；证据等级 A；简称 NF-ResNet；相关性排序 14

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q3

**标签：** Normalizer-Free、方差递推

**定位：** 采用 x+αf(x/β)：固定 α 控制增量，递推 β 预测尺度，并在 transition 重置；Scaled WS 为配套。

**问题：** Fixup/SkipInit 可启动训练，但方差漂移、激活均值和 transition reset 仍未解决。

**机制：** 采用 x+αf(x/β)：固定 α 控制增量，递推 β 预测尺度，并在 transition 重置；Scaled WS 为配套。

**步骤：**

1. 固定 α
2. 递推 β
3. 以 x/β 输入支路
4. 转换处重置
5. Scaled WS 控制均值

**证据：**

- ImageNet 5 seeds 中 NF-ResNet-50/101/200 略高于对应 BN。
- batch-size stress 中 batch 4 仍稳定。
- 未正则化 NF-ResNet-288 五次中两次崩溃。

**局限：**

- α/β、Scaled WS、SkipInit 与正则化强耦合。
- EfficientNet 和强增强存在失败边界。

**意义：**

- 将恒等起点扩展为训练全程的信号能量管理。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- 信号传播推导
- Tables 1–2

**资源：** [一手入口](<https://openreview.net/forum?id=IX3Nnir2omJ>)

---

<a id="paper-nfnet-2021"></a>
**15. 无归一化的大规模高性能图像识别｜High-Performance Large-Scale Image Recognition Without Normalization（2021 · ICML 2021）**

**作者：** Andrew Brock、Soham De、Samuel L. Smith、Karen Simonyan

**书目：** 年份 2021；载体 ICML 2021；状态 同行评议；来源类型 paper

**分类：** 主路线 残差更新控制与稳定化；相关路线 残差更新控制与稳定化；层级 跨任务或系统级；阅读层级 桥接；证据等级 B；简称 NFNet；相关性排序 15

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P3 / Q=Q1

**标签：** 系统级、AGC

**定位：** 继承 α/β 与 Scaled WS，再加入 AGC、新 block、SE 和系统级训练方案。

**问题：** 把无归一化信号传播扩展到大模型时仍有梯度异常与扩展效率问题。

**机制：** 继承 α/β 与 Scaled WS，再加入 AGC、新 block、SE 和系统级训练方案。

**步骤：**

1. 继承残差尺度
2. AGC 控制梯度
3. 联合架构与正则化扩展
4. 大规模 ImageNet 验证

**证据：**

- NFNet-F1 与 EfficientNet-B7 相近精度而训练快约 8.7 倍。
- 最大模型配合 SAM 达 86.5% top-1。

**局限：**

- 无连接单因素消融，性能高度依赖 AGC 和配方。
- 大模型与长训练成本高。

**意义：**

- 证明整套 normalization-free 系统可扩展，因果证据应回溯 NF-ResNet。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- 系统设计
- 主结果与效率表

**资源：** [一手入口](<https://proceedings.mlr.press/v139/brock21a.html>) · [代码](<https://github.com/deepmind/deepmind-research/tree/master/nfnets>)

---

<a id="paper-rescalenet-2020"></a>
**16. 归一化对深度网络训练是否不可或缺？｜Is normalization indispensable for training deep neural network?（2020 · NeurIPS 2020）**

**作者：** Jie Shao、Kai Hu、Changhu Wang、Xiangyang Xue、Bhiksha Raj

**书目：** 年份 2020；载体 NeurIPS 2020；状态 同行评议；来源类型 paper

**分类：** 主路线 残差更新控制与稳定化；相关路线 残差更新控制与稳定化；层级 跨任务或系统级；阅读层级 核心；证据等级 A；简称 RescaleNet；相关性排序 16

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P3 / Q=Q2

**标签：** 双路缩放、跨任务

**定位：** 使用 y\_i=α\_i x\_i+β\_iF\_i(x\_i) 配平两路方差，再加参数化、偏置与可学习倍率。

**问题：** 移除归一化后 y=x+F(x) 的方差逐层增长并伴随 dead ReLU。

**机制：** 使用 y\_i=α\_i x\_i+β\_iF\_i(x\_i) 配平两路方差，再加参数化、偏置与可学习倍率。

**步骤：**

1. 估计方差增长
2. 联合选择 α/β
3. 重参数化并补偿 dead ReLU
4. 多任务验证

**证据：**

- RescaleNet-50 在 ImageNet 报告比对应 BN/GN 低约 0.3 error。
- 覆盖 COCO、Kinetics 与 WMT。

**局限：**

- 最终模型含多项配套，不能只归因 α/β。
- 跨任务主要是作者实现，缺独立统一复现。

**意义：**

- 把缩放扩展到恒等与残差两路共同配平。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- RescaleNet 方程
- ImageNet/COCO/Kinetics/WMT 表

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2020/hash/9b8619251a19057cff70779273e95aa6-Abstract.html>)

---

<a id="paper-scaling-large-depth-2025"></a>
**17. 大深度极限下的 ResNet 缩放｜Scaling ResNets in the Large-depth Regime（2025 · JMLR 2025）**

**作者：** Pierre Marion、Adeline Fermanian、Gérard Biau、Jean-Philippe Vert

**书目：** 年份 2025；载体 JMLR 2025；状态 同行评议；来源类型 paper

**分类：** 主路线 残差更新控制与稳定化；相关路线 残差更新控制与稳定化、机制诊断与边界；层级 小型分类基准；阅读层级 核心；证据等级 A；简称 Large-depth scaling；相关性排序 17

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 1/√L、ODE、SDE

**定位：** 联合分析 α\_L 与权重沿深度的正则性：i.i.d. 初始化只有 1/√L 非平凡，ODE 极限需相关权重与 1/L。

**问题：** 1/√L 与 1/L 结论常忽略层间权重相关性，连续极限解释因而冲突。

**机制：** 联合分析 α\_L 与权重沿深度的正则性：i.i.d. 初始化只有 1/√L 非平凡，ODE 极限需相关权重与 1/L。

**步骤：**

1. 指定 α\_L
2. 区分独立与相关权重
3. 推导爆炸/恒等/非平凡极限
4. 扫描两参数区域

**证据：**

- 主定理给出 i.i.d. 下 1/√L 的非平凡随机极限。
- 1/L ODE 极限要求相关/平滑初始化。
- 实验显示缩放与权重正则性共同影响性能。

**局限：**

- 特定概率极限不是所有有限深度 BN-ResNet 调参规则。
- 没有现代 ImageNet 统一横评。

**意义：**

- 把残差缩放与 ODE/SDE 的适用条件绑定。

**边界：** 采用 JMLR 2025 正式卷期；提交年份不替代书目年份。

**证据位置：**

- 主要定理
- 连续极限章节
- 实验章节

**资源：** [一手入口](<https://www.jmlr.org/papers/v26/22-0664.html>)

---

<a id="paper-orthogonal-residual-update-2025"></a>
**18. 重访残差连接：稳定高效深网的正交更新｜Revisiting Residual Connections: Orthogonal Updates for Stable and Efficient Deep Networks（2025 · NeurIPS 2025）**

**作者：** Giyeong Oh、Woohyun Cho、Siyeol Kim、Suhwan Choi、Youngjae Yu

**书目：** 年份 2025；载体 NeurIPS 2025；状态 同行评议；来源类型 paper

**分类：** 主路线 残差更新控制与稳定化；相关路线 残差更新控制与稳定化；层级 小型分类基准；阅读层级 核心；证据等级 A；简称 Orthogonal Residual Update；相关性排序 18

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q3

**标签：** 正交投影、2025 前沿

**定位：** 计算 s=&lt;x,F&gt;/(||x||²+ε)，令 F⊥=F-sx，只更新 x←x+F⊥。

**问题：** 标准 F(x) 可能主要强化现有状态方向，浪费学习新方向的容量。

**机制：** 计算 s=&lt;x,F&gt;/(||x||²+ε)，令 F⊥=F-sx，只更新 x←x+F⊥。

**步骤：**

1. 计算 F(x)
2. 投影出平行分量
3. 丢弃平行分量
4. 只加入 F⊥

**证据：**

- 式(3)–(4) 和 Algorithm 1 给出实现。
- 5-run ResNetV2 在 CIFAR/Tiny ImageNet 多数改善，但 ResNetV2-50 CIFAR-10 略差。
- CNN 吞吐开销约 3%–13%。

**局限：**

- ImageNet-1k 只测 ViT，没有 CNN-ResNet ImageNet 证据。
- ResNetV2 收益较小且不全为正。

**意义：**

- 把残差控制从幅值扩展到输入相关更新方向。

**边界：** 全文核验纠正摘要易造成的“ResNet 也有 ImageNet”误读，故定为 E1。

**标识：** DOI 10.52202/085713-2409

**证据位置：**

- §3.1–3.3，式(3)–(4)
- Tables 1–2
- §4.1–4.2

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2025/hash/67c15da4a9340140c60783d9a175fd3f-Abstract-Conference.html>)

---

<a id="paper-stochastic-depth-2016"></a>
**19. 随机深度网络｜Deep Networks with Stochastic Depth（2016 · ECCV 2016）**

**作者：** Gao Huang、Yu Sun、Zhuang Liu、Daniel Sedra、Kilian Q. Weinberger

**书目：** 年份 2016；载体 ECCV 2016；状态 同行评议；来源类型 paper

**分类：** 主路线 随机路径正则；相关路线 随机路径正则；层级 ImageNet 规模；阅读层级 核心；证据等级 A；简称 Stochastic Depth；相关性排序 19

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 块丢弃、预算反例

**定位：** 训练为每块采样 Bernoulli b\_l，关闭时只走捷径；测试用生存率缩放。

**问题：** 极深 ResNet 训练成本高且易过拟合，希望利用块的可跳过性。

**机制：** 训练为每块采样 Bernoulli b\_l，关闭时只走捷径；测试用生存率缩放。

**步骤：**

1. 设置生存率
2. 训练采样是否执行
3. 关闭时走 identity
4. 测试用期望缩放

**证据：**

- CIFAR-1202 报告 4.91% error 和约 25% 训练提速。
- ImageNet 同 90 epoch 为 23.38，略差 baseline 23.06；延长后才改善。

**局限：**

- 训练/推理图不同且路径按批次随机。
- 不是无额外预算的普遍提升，浅网未必受益。

**意义：**

- 成为 drop-path 与动态块路由的共同参照。

**边界：** 书目采用 ECCV 2016；arXiv 为同一全文版本族。

**标识：** DOI 10.1007/978-3-319-46493-0\_39

**证据位置：**

- §3，式(2)–(4)
- Figure 2
- CIFAR/ImageNet 表

**资源：** [一手入口](<https://arxiv.org/abs/1603.09382>)

---

<a id="paper-swapout-2016"></a>
**20. Swapout：学习深层架构集合｜Swapout: Learning an Ensemble of Deep Architectures（2016 · NeurIPS 2016）**

**作者：** Saurabh Singh、Derek Hoiem、David Forsyth

**书目：** 年份 2016；载体 NeurIPS 2016；状态 同行评议；来源类型 paper

**分类：** 主路线 随机路径正则；相关路线 随机路径正则；层级 小型分类基准；阅读层级 核心；证据等级 B；简称 Swapout；相关性排序 20

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** 双门控、Monte Carlo

**定位：** 两个 Bernoulli 张量独立门控 X 和 F(X)，形成丢弃、仅捷径、仅变换或完整相加四种状态。

**问题：** 希望比 dropout 或块级随机深度更一般地采样局部架构。

**机制：** 两个 Bernoulli 张量独立门控 X 和 F(X)，形成丢弃、仅捷径、仅变换或完整相加四种状态。

**步骤：**

1. 采样 shortcut 门
2. 采样 residual 门
3. 形成四种状态
4. 缩放或 Monte Carlo 推理

**证据：**

- CIFAR 比较概率调度和推理方式。
- 最佳 Monte Carlo 约需 30 次采样，正式评审质疑成本。

**局限：**

- 无 ImageNet，BN 与确定性推理有交互。
- 逐单元随机难获得结构化硬件加速。

**意义：**

- 说明随机性可直接作用于两路合并节点。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- §2
- §3.1
- CIFAR 结果与评审

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2016/hash/c51ce410c124a10e0db5e4b97fc2af39-Abstract.html>)

---

<a id="paper-shake-shake-2017"></a>
**21. 三分支残差网络的 Shake-Shake 正则化｜Shake-Shake Regularization of 3-Branch Residual Networks（2017 · ICLR 2017 Workshop）**

**作者：** Xavier Gastaldi

**书目：** 年份 2017；载体 ICLR 2017 Workshop；状态 预印本；来源类型 paper

**分类：** 主路线 随机路径正则；相关路线 随机路径正则；层级 小型分类基准；阅读层级 桥接；证据等级 B；简称 Shake-Shake；相关性排序 21

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P1 / Q=Q1

**标签：** 随机混合、工作坊

**定位：** 前向 x+αF1+(1-α)F2，反向可独立采样 β，测试时各取 0.5。

**问题：** 多分支残差模型可能共适应，需要在合并处引入梯度扰动。

**机制：** 前向 x+αF1+(1-α)F2，反向可独立采样 β，测试时各取 0.5。

**步骤：**

1. 构造双残差分支
2. 前向随机混合
3. 反向独立混合
4. 测试取均值

**证据：**

- CIFAR 比较前后向相关性、采样粒度和批大小。
- 原始结果多依赖约 1800 epoch。

**局限：**

- 只适用于多分支。
- 工作坊/预印本状态且对训练长度敏感。

**意义：**

- 把随机性推进到多增量合并方式。

**边界：** 明确标为 ICLR Workshop/预印本，不冒充主会。

**证据位置：**

- 式(1)–(2)
- CIFAR 消融

**资源：** [一手入口](<https://openreview.net/forum?id=HkO-PCmYl>)

---

<a id="paper-shakedrop-2019"></a>
**22. 深度残差学习的 ShakeDrop 正则化｜ShakeDrop Regularization for Deep Residual Learning（2019 · IEEE Access 2019）**

**作者：** Yoshihiro Yamada、Masakazu Iwamura、Takuya Akiba、Koichi Kise

**书目：** 年份 2019；载体 IEEE Access 2019；状态 同行评议；来源类型 paper

**分类：** 主路线 随机路径正则；相关路线 随机路径正则；层级 小型分类基准；阅读层级 核心；证据等级 B；简称 ShakeDrop；相关性排序 22

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 单分支、稳定条件

**定位：** 结合 Bernoulli 生存变量、前向 α 与反向 β；测试用系数期望。

**问题：** Shake-Shake 依赖双分支，希望推广到普通单残差分支。

**机制：** 结合 Bernoulli 生存变量、前向 α 与反向 β；测试用系数期望。

**步骤：**

1. 采样块生存
2. 丢弃时随机 α
3. 反向独立 β
4. 测试取期望

**证据：**

- 正式版系统比较 α、β、生存率和多种 backbone。
- 论文给 stabilizer 条件并承认不合适参数会失稳。

**局限：**

- 参数随网络和 block 变化。
- 早期结果依赖延长训练。

**意义：**

- 完成双分支 shake 到单分支扰动的谱系。

**边界：** 主实体采用 2019 正式期刊版；2018 workshop 是前身。

**标识：** DOI 10.1109/ACCESS.2019.2960566

**证据位置：**

- IEEE Access §II–III
- Tables 4–9

**资源：** [一手入口](<https://doi.org/10.1109/ACCESS.2019.2960566>)

---

<a id="paper-highway-networks-2015"></a>
**23. 训练超深网络｜Training Very Deep Networks（2015 · NeurIPS 2015）**

**作者：** Rupesh Kumar Srivastava、Klaus Greff、Jürgen Schmidhuber

**书目：** 年份 2015；载体 NeurIPS 2015；状态 同行评议；来源类型 paper

**分类：** 主路线 自适应门控与路由；相关路线 自适应门控与路由；层级 小型分类基准；阅读层级 背景；证据等级 B；简称 Highway Networks；相关性排序 23

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** 前驱、carry gate

**定位：** y=H(x)T(x)+xC(x)，常用 C=1-T，并用负门偏置让初始状态偏向 carry。

**问题：** 普通深网难训练，希望让每层按输入选择变换或直通。

**机制：** y=H(x)T(x)+xC(x)，常用 C=1-T，并用负门偏置让初始状态偏向 carry。

**步骤：**

1. 计算 H(x)
2. 预测 transform gate
3. carry gate 保留输入
4. 负偏置初始化

**证据：**

- 正文给出门方程、活动可视化和 lesion。
- MNIST/CIFAR 显示可训练很深网络。

**局限：**

- 每层增加 sigmoid 门开销；carry 只有饱和时近似恒等。
- 它是 ResNet 前驱而非后续。

**意义：**

- 凸显 ResNet 去门并固定 identity 的关键简化。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- §2 方程
- gate bias 与 lesion

**资源：** [一手入口](<https://papers.nips.cc/paper_files/paper/2015/hash/215a71a12769b056c3c32e7299f1c5ed-Abstract.html>)

---

<a id="paper-sact-2017"></a>
**24. 残差网络的空间自适应计算时间｜Spatially Adaptive Computation Time for Residual Networks（2017 · CVPR 2017）**

**作者：** Michael Figurnov、Maxwell D. Collins、Yukun Zhu、Li Zhang、Jonathan Huang、Dmitry Vetrov、Ruslan Salakhutdinov

**书目：** 年份 2017；载体 CVPR 2017；状态 同行评议；来源类型 paper

**分类：** 主路线 自适应门控与路由；相关路线 自适应门控与路由；层级 跨任务或系统级；阅读层级 核心；证据等级 A；简称 SACT；相关性排序 24

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P3 / Q=Q2

**标签：** 空间路由、COCO

**定位：** 每个位置累积 halting probability，达到阈值后停止残差更新，并用 ponder cost 约束计算。

**问题：** 逐图动态深度太粗，密集预测需要按空间位置分配层数。

**机制：** 每个位置累积 halting probability，达到阈值后停止残差更新，并用 ponder cost 约束计算。

**步骤：**

1. 预测位置停止概率
2. 累积到阈值
3. 停止位置保留状态
4. ponder cost 约束

**证据：**

- ImageNet 分类与 COCO 检测都报告效率改善。
- 计算图与 CAT2000 注视位置相关。

**局限：**

- 真实硬件收益依赖稀疏执行实现。
- 区域级控制增加复杂度。

**意义：**

- 把可跳过性推进到空间粒度和跨任务。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.1109/CVPR.2017.194

**证据位置：**

- SACT 定义与算法
- ImageNet/COCO
- CAT2000

**资源：** [一手入口](<https://openaccess.thecvf.com/content_cvpr_2017/html/Figurnov_Spatially_Adaptive_Computation_CVPR_2017_paper.html>)

---

<a id="paper-blockdrop-2018"></a>
**25. BlockDrop：残差网络的动态推理路径｜BlockDrop: Dynamic Inference Paths in Residual Networks（2018 · CVPR 2018）**

**作者：** Zuxuan Wu、Tushar Nagarajan、Abhishek Kumar、Steven Rennie、Larry S. Davis、Kristen Grauman、Rogerio Feris

**书目：** 年份 2018；载体 CVPR 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 自适应门控与路由；相关路线 自适应门控与路由；层级 ImageNet 规模；阅读层级 核心；证据等级 A；简称 BlockDrop；相关性排序 25

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 动态推理、实际延迟

**定位：** 策略网络一次预测全部 blocks 的二值向量，用强化学习奖励、课程学习和联合微调。

**问题：** 固定深度对简单样本浪费计算。

**机制：** 策略网络一次预测全部 blocks 的二值向量，用强化学习奖励、课程学习和联合微调。

**步骤：**

1. 预测整条路径
2. 执行或跳过块
3. 优化准确率与块数
4. 课程学习后微调

**证据：**

- ResNet-101 ImageNet 保持 76.4% top-1 时平均约 20% 加速。
- Table 2 计入策略开销与实际速度。

**局限：**

- 依赖预训练、强化学习和课程学习。
- 块数/FLOPs 不等于墙钟延迟。

**意义：**

- 代表推理期全局输入相关路由。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.1109/CVPR.2018.00919

**证据位置：**

- §3
- Figure 3
- Table 2

**资源：** [一手入口](<https://openaccess.thecvf.com/content_cvpr_2018/html/Wu_BlockDrop_Dynamic_Inference_CVPR_2018_paper.html>)

---

<a id="paper-skipnet-2018"></a>
**26. SkipNet：卷积网络的动态路由学习｜SkipNet: Learning Dynamic Routing in Convolutional Networks（2018 · ECCV 2018）**

**作者：** Xin Wang、Fisher Yu、Zi-Yi Dou、Trevor Darrell、Joseph E. Gonzalez

**书目：** 年份 2018；载体 ECCV 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 自适应门控与路由；相关路线 自适应门控与路由；层级 ImageNet 规模；阅读层级 核心；证据等级 A；简称 SkipNet；相关性排序 26

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 软硬失配、强化学习

**定位：** G\_i(x\_i) 在 residual 与 identity 间选择；先监督预训练，再强化学习硬门。

**问题：** 希望按当前激活逐层调整深度并优化准确率—计算权衡。

**机制：** G\_i(x\_i) 在 residual 与 identity 间选择；先监督预训练，再强化学习硬门。

**步骤：**

1. 当前激活预测门
2. 执行或跳过块
3. 软门预训练
4. 强化硬路由

**证据：**

- ImageNet 报告约减少 30% 计算。
- soft 训练后直接 hard 推理可从约 90.83 跌到 66.67。
- 训练通常多约 30%–40% 时间。

**局限：**

- 顺序门有运行开销。
- 软硬失配是核心风险。

**意义：**

- 给出动态路由的关键部署反例。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.1007/978-3-030-01261-8\_25

**证据位置：**

- §3，式(1)
- Table 3
- 训练/计算分析

**资源：** [一手入口](<https://openaccess.thecvf.com/content_ECCV_2018/html/Xin_Wang_SkipNet_Learning_Dynamic_ECCV_2018_paper.html>)

---

<a id="paper-convnet-aig-2018"></a>
**27. 具有自适应推理图的卷积网络｜Convolutional Networks with Adaptive Inference Graphs（2018 · ECCV 2018）**

**作者：** Andreas Veit、Serge Belongie

**书目：** 年份 2018；载体 ECCV 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 自适应门控与路由；相关路线 自适应门控与路由；层级 ImageNet 规模；阅读层级 核心；证据等级 A；简称 ConvNet-AIG；相关性排序 27

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** Gumbel、模式坍塌

**定位：** 每层 gate 预测概率，用 Gumbel sampling 与 straight-through 联合训练，推理形成离散图。

**问题：** argmax 硬门不可微且易模式坍塌。

**机制：** 每层 gate 预测概率，用 Gumbel sampling 与 straight-through 联合训练，推理形成离散图。

**步骤：**

1. 预测执行概率
2. Gumbel 采样二值门
3. 直通估计梯度
4. 离散推理

**证据：**

- ImageNet ResNet-50/101 报告 gate overhead 和跳层统计。
- 朴素 argmax 会快速 mode collapse。

**局限：**

- 依赖温度与随机代理，仍有训练部署失配。
- 路径可能学到数据集特定捷径。

**意义：**

- 与 BlockDrop/SkipNet 构成同期平行路由分叉。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.1007/978-3-030-01246-5\_1

**证据位置：**

- Figure 1
- §3.2–3.3
- ImageNet 结果

**资源：** [一手入口](<https://openaccess.thecvf.com/content_ECCV_2018/papers/Andreas_Veit_Convolutional_Networks_with_ECCV_2018_paper.pdf>)

---

<a id="paper-epsilon-resnet-2018"></a>
**28. 在深度残差网络中学习严格恒等映射｜Learning Strict Identity Mappings in Deep Residual Networks（2018 · CVPR 2018）**

**作者：** Xin Yu、Zhiding Yu、Srikumar Ramalingam

**书目：** 年份 2018；载体 CVPR 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 自适应门控与路由；相关路线 自适应门控与路由、恒等保真与投影；层级 ImageNet 规模；阅读层级 核心；证据等级 A；简称 ε-ResNet；相关性排序 28

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 严格恒等、剪枝

**定位：** 用 ε 阈值让小残差响应精确为零，块严格退化为 identity，训练后删除。

**问题：** 一般稀疏门只近似为零，难以安全删除冗余残差块。

**机制：** 用 ε 阈值让小残差响应精确为零，块严格退化为 identity，训练后删除。

**步骤：**

1. 计算残差响应
2. ε 阈值压零
3. 块变严格恒等
4. 训练后删除

**证据：**

- CIFAR、SVHN 和 ImageNet 均评估层选择。
- 752 层 CIFAR-100 示例压缩约 3.2 倍且精度不降。
- 部分设置参数量最多减少约 80%。

**局限：**

- 阈值依赖数据和配方。
- 属于训练后静态剪枝，不是逐输入路由。

**意义：**

- 连接可学习门与可证明可删除块。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.1109/CVPR.2018.00466

**证据位置：**

- 模型定义
- Figure 1
- 多数据集结果

**资源：** [一手入口](<https://openaccess.thecvf.com/content_cvpr_2018/html/Yu_Learning_Strict_Identity_CVPR_2018_paper.html>)

---

<a id="paper-adaptive-depth-2024"></a>
**29. 具有可跳过子路径的自适应深度网络｜Adaptive Depth Networks with Skippable Sub-Paths（2024 · NeurIPS 2024）**

**作者：** Woochul Kang、Hyungseop Lee

**书目：** 年份 2024；载体 NeurIPS 2024；状态 同行评议；来源类型 paper

**分类：** 主路线 自适应门控与路由；相关路线 自适应门控与路由；层级 ImageNet 规模；阅读层级 核心；证据等级 B；简称 Adaptive Depth；相关性排序 29

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 可跳子路径、自蒸馏

**定位：** 每个 residual stage 分成 essential 和 refining sub-path，自蒸馏让后半段可省略，测试组合多深度子网。

**问题：** 早期自适应深度训练复杂，缺少哪些层可跳的原则。

**机制：** 每个 residual stage 分成 essential 和 refining sub-path，自蒸馏让后半段可省略，测试组合多深度子网。

**步骤：**

1. 拆阶段为必选/可跳
2. 自蒸馏区分角色
3. 共享一次训练
4. 组合部署深度

**证据：**

- 给出跳过子路径误差的形式化理由。
- CNN 与 Transformer 均验证单模型多档位。

**局限：**

- 是预算可选静态子网，不是逐样本门控。
- 跨架构结论需分别核对。

**意义：**

- 把路由目标转向可预测的部署档位。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.52202/079017-1046

**证据位置：**

- 方法定义
- 自蒸馏章节
- CNN/Transformer 结果

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2024/hash/3a2d96d2eb2902043c2db705ca03e9a2-Abstract-Conference.html>)

---

<a id="paper-ror-2018"></a>
**30. 残差网络之残差网络：多层级残差网络｜Residual Networks of Residual Networks: Multilevel Residual Networks（2018 · IEEE TCSVT 2018）**

**作者：** Ke Zhang、Miao Sun、Tony X. Han、Xingfang Yuan、Liru Guo、Tao Liu

**书目：** 年份 2018；载体 IEEE TCSVT 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 跨层聚合拓扑；相关路线 跨层聚合拓扑；层级 ImageNet 规模；阅读层级 核心；证据等级 B；简称 RoR；相关性排序 30

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** 多层级、非单调

**定位：** 在 block shortcut 上叠加 stage 和 root shortcut，转换处用投影。

**问题：** 单块捷径之外，跨阶段和整网最短路径仍较长。

**机制：** 在 block shortcut 上叠加 stage 和 root shortcut，转换处用投影。

**步骤：**

1. 保留块级残差
2. 增加阶段捷径
3. 增加 root 捷径
4. 转换处投影

**证据：**

- 层级数与 shortcut 类型有消融。
- 3 levels 通常最好，4/5 levels 可低于基线。

**局限：**

- 联合 stochastic depth 的结果不能全归因 RoR。
- 最佳层级和投影依数据。

**意义：**

- 最直接的 shortcut-of-shortcut 与非单调反例。

**边界：** 书目用 2018 正式卷期；2016 arXiv 与 2017 early access 同族。

**标识：** DOI 10.1109/TCSVT.2017.2654543

**证据位置：**

- §3
- 层级/shortcut 消融

**资源：** [一手入口](<https://arxiv.org/abs/1608.02908>)

---

<a id="paper-polynet-2017"></a>
**31. PolyNet：追求超深网络的结构多样性｜PolyNet: A Pursuit of Structural Diversity in Very Deep Networks（2017 · CVPR 2017）**

**作者：** Xingcheng Zhang、Zhizhong Li、Chen Change Loy、Dahua Lin

**书目：** 年份 2017；载体 CVPR 2017；状态 同行评议；来源类型 paper

**分类：** 主路线 跨层聚合拓扑；相关路线 跨层聚合拓扑、随机路径正则、残差更新控制与稳定化；层级 ImageNet 规模；阅读层级 核心；证据等级 B；简称 PolyNet；相关性排序 31

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** 多项式路径

**定位：** 将 I+F 扩成 I+F+F²、I+F+GF 或 I+F+G，并用 insertion init、缩放稳定极深模型。

**问题：** 重复 I+F 路径结构单调，希望增加高阶和并行组合。

**机制：** 将 I+F 扩成 I+F+F²、I+F+GF 或 I+F+G，并用 insertion init、缩放稳定极深模型。

**步骤：**

1. 把块视为 I+F
2. 加入二阶/并行项
3. 级联实现路径
4. 插入初始化与缩放

**证据：**

- Figure 4 给出算子展开，逐项替换通常改善。
- 随机初始化不稳，过早随机路径会妨碍收敛。

**局限：**

- 替换也增加计算，最终结果含多尺度多裁剪。
- Inception 宏结构与稳定配套混杂。

**意义：**

- 把连接从二路相加改成路径代数。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.1109/CVPR.2017.415

**证据位置：**

- Figure 4
- §3.2
- Figures 9–10

**资源：** [一手入口](<https://openaccess.thecvf.com/content_cvpr_2017/html/Zhang_PolyNet_A_Pursuit_CVPR_2017_paper.html>)

---

<a id="paper-dpn-2017"></a>
**32. 双路径网络｜Dual Path Networks（2017 · NeurIPS 2017）**

**作者：** Yunpeng Chen、Jianan Li、Huaxin Xiao、Xiaojie Jin、Shuicheng Yan、Jiashi Feng

**书目：** 年份 2017；载体 NeurIPS 2017；状态 同行评议；来源类型 paper

**分类：** 主路线 跨层聚合拓扑；相关路线 跨层聚合拓扑；层级 跨任务或系统级；阅读层级 核心；证据等级 B；简称 DPN；相关性排序 32

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P3 / Q=Q1

**标签：** 加法、拼接、双状态

**定位：** block 输出分成 residual 与 dense 部分；前者加到共享状态，后者拼接到探索状态。

**问题：** 纯加法偏重复用，纯拼接持续增长状态，希望兼顾复用与探索。

**机制：** block 输出分成 residual 与 dense 部分；前者加到共享状态，后者拼接到探索状态。

**步骤：**

1. 拆共享/探索状态
2. block 生成两类输出
3. 残差部分相加
4. 新特征部分拼接

**证据：**

- 正文给出 dual-path 公式与图。
- ImageNet 外还验证检测与分割。

**局限：**

- 宽度、增长率、分组卷积和连接同时变。
- dense 状态仍带来显存压力。

**意义：**

- 代表加法与拼接混合聚合。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- §3–4，Figure 2
- ImageNet/检测/分割表

**资源：** [一手入口](<https://proceedings.neurips.cc/paper/2017/hash/f7e0b956540676a129760a3eae309294-Abstract.html>)

---

<a id="paper-dla-2018"></a>
**33. 深层聚合｜Deep Layer Aggregation（2018 · CVPR 2018）**

**作者：** Fisher Yu、Dequan Wang、Evan Shelhamer、Trevor Darrell

**书目：** 年份 2018；载体 CVPR 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 跨层聚合拓扑；相关路线 跨层聚合拓扑；层级 跨任务或系统级；阅读层级 桥接；证据等级 B；简称 DLA；相关性排序 33

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P3 / Q=Q1

**标签：** 树形聚合、跨尺度

**定位：** Iterative DLA 逐步融合阶段，Hierarchical DLA 用树形节点聚合多深度与多尺度。

**问题：** 相邻 block skip 不足以系统融合浅层定位与深层语义。

**机制：** Iterative DLA 逐步融合阶段，Hierarchical DLA 用树形节点聚合多深度与多尺度。

**步骤：**

1. 迭代跨阶段聚合
2. 树形层次聚合
3. 融合多深度状态
4. 迁移到密集任务

**证据：**

- Figure 1 和 §3 定义 IDA/HDA。
- 分类、分割等多任务验证。

**局限：**

- 连接树、节点、宽深和任务头共同变。
- 属网络级拓扑而非局部 shortcut 小改。

**意义：**

- 将跨层连接提升为层次网络图设计。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.1109/CVPR.2018.00255

**证据位置：**

- Figure 1
- §3.1–3.2
- 多任务表

**资源：** [一手入口](<https://openaccess.thecvf.com/content_cvpr_2018/html/Yu_Deep_Layer_Aggregation_CVPR_2018_paper.html>)

---

<a id="paper-sparsenet-2018"></a>
**34. 稀疏聚合卷积网络｜Sparsely Aggregated Convolutional Networks（2018 · ECCV 2018）**

**作者：** Ligeng Zhu、Ruizhi Deng、Michael Maire、Zhiwei Deng、Greg Mori、Ping Tan

**书目：** 年份 2018；载体 ECCV 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 跨层聚合拓扑；相关路线 跨层聚合拓扑；层级 ImageNet 规模；阅读层级 桥接；证据等级 B；简称 SparseNet；相关性排序 34

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** 稀疏图

**定位：** 每层只连指数间隔的一组前层，入边从线性降为对数，可用加法或拼接。

**问题：** DenseNet 全连接图的边、激活和内存成本随深度增长。

**机制：** 每层只连指数间隔的一组前层，入边从线性降为对数，可用加法或拼接。

**步骤：**

1. 指数间隔选前驱
2. 加法或拼接聚合
3. 减少入边
4. 扩展超深网络

**证据：**

- 正文给出连接集合与复杂度。
- CIFAR/ImageNet 比较参数效率并含 1000+ 层。

**局限：**

- 日程手工固定，不自适应输入。
- 删除直连会增加部分最短路径。

**意义：**

- 区分固定稀疏拓扑与随机丢边。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.1007/978-3-030-01258-8\_12

**证据位置：**

- §3
- 复杂度
- CIFAR/ImageNet 表

**资源：** [一手入口](<https://openaccess.thecvf.com/content_ECCV_2018/html/Ligeng_Zhu_Sparsely_Aggregated_Convolutional_ECCV_2018_paper.html>)

---

<a id="paper-mixnet-2018"></a>
**35. 混合链接网络｜Mixed Link Networks（2018 · IJCAI 2018）**

**作者：** Wenhai Wang、Xiang Li、Tong Lu、Jian Yang

**书目：** 年份 2018；载体 IJCAI 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 跨层聚合拓扑；相关路线 跨层聚合拓扑；层级 ImageNet 规模；阅读层级 桥接；证据等级 B；简称 MixNet；相关性排序 35

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** 混合链接

**定位：** inner addition 更新固定宽度复用状态，outer concatenation 累积新特征。

**问题：** ResNet 加法与 DenseNet 拼接有不同复用偏好，需要混合连接代数。

**机制：** inner addition 更新固定宽度复用状态，outer concatenation 累积新特征。

**步骤：**

1. 维护加法状态
2. 维护拼接状态
3. block 同时更新
4. 传递混合状态

**证据：**

- §3 给公式与图。
- CIFAR、SVHN、ImageNet 报告参数效率。

**局限：**

- 统一性依赖作者状态分解。
- 连接数、增长率与宏结构共同变。

**意义：**

- 在 DPN 后整理混合连接代数。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.24963/ijcai.2018/391

**证据位置：**

- §3
- 多数据集表

**资源：** [一手入口](<https://www.ijcai.org/Proceedings/2018/391>)

---

<a id="paper-lm-resnet-2018"></a>
**36. 超越有限层网络：连接深层架构与数值微分方程｜Beyond Finite Layer Neural Networks: Bridging Deep Architectures and Numerical Differential Equations（2018 · ICML 2018）**

**作者：** Yiping Lu、Aoxiao Zhong、Quanzheng Li、Bin Dong

**书目：** 年份 2018；载体 ICML 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 跨层聚合拓扑；相关路线 跨层聚合拓扑、可逆与信息保持、机制诊断与边界；层级 ImageNet 规模；阅读层级 核心；证据等级 B；简称 LM-ResNet；相关性排序 36

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** 多步方法、动力系统

**定位：** LM-architecture 让新状态依赖当前和历史状态与残差，可插入 ResNet/ResNeXt。

**问题：** 一阶 Euler 式 ResNet 只用当前状态，多步法提示更高阶更新。

**机制：** LM-architecture 让新状态依赖当前和历史状态与残差，可插入 ResNet/ResNeXt。

**步骤：**

1. 解释为一阶离散
2. 保留历史状态
3. 线性多步组合
4. 加入残差变换

**证据：**

- CIFAR 与 ImageNet 上 LM-ResNet/ResNeXt 报告改善。
- 相近性能时可压缩超过 50% 层数。

**局限：**

- 无权重平滑性时 ResNet 不保证收敛到 ODE。
- 历史状态和系数改变整个块。

**意义：**

- 建立多步状态连接支线。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- LM 定义
- CIFAR/ImageNet 表
- 压缩实验

**资源：** [一手入口](<https://proceedings.mlr.press/v80/lu18d.html>)

---

<a id="paper-merge-and-run-2018"></a>
**37. 采用 Merge-and-Run 映射的深度卷积网络｜Deep Convolutional Neural Networks with Merge-and-Run Mappings（2018 · IJCAI 2018）**

**作者：** Liming Zhao、Mingjie Li、Depu Meng、Xi Li、Zhaoxiang Zhang、Yueting Zhuang、Zhuowen Tu、Jingdong Wang

**书目：** 年份 2018；载体 IJCAI 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 跨层聚合拓扑；相关路线 跨层聚合拓扑；层级 ImageNet 规模；阅读层级 核心；证据等级 B；简称 Merge-and-Run；相关性排序 37

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 幂等映射、版本冲突

**定位：** 对平行分支输入求平均，再把均值加到各分支输出；映射矩阵线性且幂等。

**问题：** 串行 residual paths 仍长，希望并行并交换状态。

**机制：** 对平行分支输入求平均，再把均值加到各分支输出；映射矩阵线性且幂等。

**步骤：**

1. 并行残差分支
2. 平均分支输入
3. 均值加回各支
4. 重复幂等映射

**证据：**

- Table 7 直接比较 identity 与 merge-and-run。
- ImageNet DMRNet-50 error 23.16，对 ResNet-98 23.38，参数近似。
- K=2 整体最好，更多分支不更优。

**局限：**

- 并行化同时缩短路径和增加宽度。
- 正式八作者版与早期五作者版不同。

**意义：**

- 代表非恒等但幂等的分支聚合算子。

**边界：** 采用 IJCAI 2018 八作者正式版；早期 arXiv 不重复。

**标识：** DOI 10.24963/ijcai.2018/440

**证据位置：**

- Figure 1
- Tables 6–7
- §5

**资源：** [一手入口](<https://www.ijcai.org/proceedings/2018/0440.pdf>)

---

<a id="paper-dense-shortcuts-2021"></a>
**38. ResNet 还是 DenseNet？向 ResNet 引入稠密捷径｜ResNet or DenseNet? Introducing Dense Shortcuts to ResNet（2021 · WACV 2021）**

**作者：** Chaoning Zhang、Philipp Benz、Dawit Mureja Argaw、Seokju Lee、Junsik Kim、Francois Rameau、Jean-Charles Bazin、In So Kweon

**书目：** 年份 2021；载体 WACV 2021；状态 同行评议；来源类型 paper

**分类：** 主路线 跨层聚合拓扑；相关路线 跨层聚合拓扑；层级 ImageNet 规模；阅读层级 桥接；证据等级 B；简称 Dense Shortcuts；相关性排序 38

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** 稠密捷径

**定位：** 多个早期特征经归一化加权后求和，介于单一 shortcut 与全拼接之间。

**问题：** 单一 identity 可能限制复用，dense concatenation 又耗显存。

**机制：** 多个早期特征经归一化加权后求和，介于单一 shortcut 与全拼接之间。

**步骤：**

1. 选择多前驱
2. 归一化贡献
3. 加权稠密捷径
4. 求和保持紧凑

**证据：**

- 多基准比较 DSNet、ResNet 与 DenseNet。
- 作者报告性能接近 DenseNet 而资源更低。

**局限：**

- 权重、归一化、连接数和宽度共同变。
- 缺现代配方独立复现。

**意义：**

- 提供 2021 年直接 dense shortcut 融合。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.1109/WACV48630.2021.00359

**证据位置：**

- 方法定义
- 多基准与资源表

**资源：** [一手入口](<https://openaccess.thecvf.com/content/WACV2021/html/Zhang_ResNet_or_DenseNet_Introducing_Dense_Shortcuts_to_ResNet_WACV_2021_paper.html>)

---

<a id="paper-revnet-2017"></a>
**39. 可逆残差网络：无需存储激活的反向传播｜The Reversible Residual Network: Backpropagation Without Storing Activations（2017 · NeurIPS 2017）**

**作者：** Aidan N. Gomez、Mengye Ren、Raquel Urtasun、Roger B. Grosse

**书目：** 年份 2017；载体 NeurIPS 2017；状态 同行评议；来源类型 paper

**分类：** 主路线 可逆与信息保持；相关路线 可逆与信息保持；层级 跨任务或系统级；阅读层级 核心；证据等级 A；简称 RevNet；相关性排序 39

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P3 / Q=Q3

**标签：** 加性耦合、内存

**定位：** 拆通道做两路 additive coupling，反向按相反顺序重构，只存不可逆边界。

**问题：** 普通 ResNet 激活内存随深度增长，而 x+F(x) 一般无闭式逆。

**机制：** 拆通道做两路 additive coupling，反向按相反顺序重构，只存不可逆边界。

**步骤：**

1. 拆成两路
2. y1=x1+F(x2)
3. y2=x2+G(y1)
4. 反向闭式重构

**证据：**

- 不要求 F/G 可逆。
- ImageNet ResNet-101 error 23.01，RevNet-104 23.10。
- 理论额外计算约 33%，通用实现约 1.5–2 倍。

**局限：**

- 下采样、首尾层不可逆。
- 同参数不等于同架构，浮点误差会积累。

**意义：**

- 把 residual connection 扩展为内存系统设计。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- §2 方程
- CIFAR Tables 3
- ImageNet Table 4

**资源：** [一手入口](<https://proceedings.neurips.cc/paper/2017/hash/f9be311e65d81a9ad8150a60844bb94c-Abstract.html>)

---

<a id="paper-i-revnet-2018"></a>
**40. i-RevNet：深度可逆网络｜i-RevNet: Deep Invertible Networks（2018 · ICLR 2018）**

**作者：** Jörn-Henrik Jacobsen、Arnold W. M. Smeulders、Edouard Oyallon

**书目：** 年份 2018；载体 ICLR 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 可逆与信息保持；相关路线 可逆与信息保持、恒等保真与投影；层级 ImageNet 规模；阅读层级 核心；证据等级 A；简称 i-RevNet；相关性排序 40

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 可逆下采样

**定位：** 保留 coupling，并用 space-to-depth 重排降低 H/W、增加通道，保持自由度。

**问题：** RevNet 的阶段下采样仍不可逆。

**机制：** 保留 coupling，并用 space-to-depth 重排降低 H/W、增加通道，保持自由度。

**步骤：**

1. 耦合块
2. 空间重排到通道
3. 降分辨率保维数
4. 分类头前反演

**证据：**

- 匹配 ResNet 精度的版本需约 181M 参数，对比 26M；匹配参数版差约 1.5–2 点。
- 反演误差约 10^-6，但奇异谱显示方向压缩。

**局限：**

- 维数保持不等于良好条件数。
- 分类头仍不可逆，墙钟增加约三分之一。

**意义：**

- 展示可逆下采样的精度—参数折中。

**边界：** 与 Behrmann 的 i-ResNet 按题名、作者和机制消歧。

**证据位置：**

- 下采样定义
- Table 1
- 反演/奇异谱

**资源：** [一手入口](<https://openreview.net/forum?id=HJsjkMb0Z>)

---

<a id="paper-hamiltonian-reversible-2018"></a>
**41. 任意深残差网络的可逆架构｜Reversible Architectures for Arbitrarily Deep Residual Neural Networks（2018 · AAAI 2018）**

**作者：** Bo Chang、Lili Meng、Eldad Haber、Lars Ruthotto、David Begert、Elliot Holtham

**书目：** 年份 2018；载体 AAAI 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 可逆与信息保持；相关路线 可逆与信息保持；层级 小型分类基准；阅读层级 核心；证据等级 B；简称 Hamiltonian reversible nets；相关性排序 41

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** Hamiltonian、数值稳定

**定位：** 从 Hamiltonian/二阶系统设计 Verlet、midpoint、leapfrog 等可逆层，并约束算子结构。

**问题：** 显式 Euler 不可逆也未必稳定，代数逆不是良好动力学的充分条件。

**机制：** 从 Hamiltonian/二阶系统设计 Verlet、midpoint、leapfrog 等可逆层，并约束算子结构。

**步骤：**

1. 选择可逆动力系统
2. 可逆数值离散
3. 约束谱/步长
4. 末态重建

**证据：**

- CIFAR 比较多种格式，若干 midpoint/leapfrog 明显较差。
- 1202 层比较使用不同 batch size。

**局限：**

- 无 ImageNet，设置不完全公平。
- 可逆格式表现差异大。

**意义：**

- 把可逆性与数值稳定分开。

**边界：** 已回到一手正文核验机制、结果与边界。

**标识：** DOI 10.1609/aaai.v32i1.11668

**证据位置：**

- 动力系统章节
- Table 1

**资源：** [一手入口](<https://ojs.aaai.org/index.php/AAAI/article/view/11668>)

---

<a id="paper-invertible-resnet-2019"></a>
**42. 可逆残差网络｜Invertible Residual Networks（2019 · ICML 2019）**

**作者：** Jens Behrmann、Will Grathwohl、Ricky T. Q. Chen、David Duvenaud、Jörn-Henrik Jacobsen

**书目：** 年份 2019；载体 ICML 2019；状态 同行评议；来源类型 paper

**分类：** 主路线 可逆与信息保持；相关路线 可逆与信息保持；层级 跨任务或系统级；阅读层级 核心；证据等级 A；简称 i-ResNet；相关性排序 42

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P3 / Q=Q2

**标签：** Lipschitz、固定点逆

**定位：** 约束 Lip(g)&lt;1，Banach 定理保证 I+g 可逆；固定点迭代求逆并近似 logdet。

**问题：** 希望保留单支路 x+g(x)，而非 coupling，同时保证可逆。

**机制：** 约束 Lip(g)&lt;1，Banach 定理保证 I+g 可逆；固定点迭代求逆并近似 logdet。

**步骤：**

1. 谱归一化约束 Lip(g)
2. 保留 x+g(x)
3. 固定点求逆
4. 近似 logdet

**证据：**

- 理论给充分条件和逆稳定界。
- 更强约束常损分类精度。
- 逆约为 forward 的 5–20 倍，生成结果也未全面最优。

**局限：**

- Lip&lt;1 保守且限制表达。
- 迭代逆昂贵，logdet 截断有偏。

**意义：**

- 使收缩、表达与反演成本显式化。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- 可逆定理
- Tables 2、4
- 反演时间

**资源：** [一手入口](<https://proceedings.mlr.press/v97/behrmann19a.html>)

---

<a id="paper-exploding-inverses-2021"></a>
**43. 理解并缓解可逆网络中的爆炸逆映射｜Understanding and Mitigating Exploding Inverses in Invertible Neural Networks（2021 · AISTATS 2021）**

**作者：** Jens Behrmann、Paul Vicol、Kuan-Chieh Wang、Roger Grosse、Jörn-Henrik Jacobsen

**书目：** 年份 2021；载体 AISTATS 2021；状态 同行评议；来源类型 paper

**分类：** 主路线 可逆与信息保持；相关路线 可逆与信息保持、机制诊断与边界；层级 跨任务或系统级；阅读层级 核心；证据等级 A；简称 Exploding Inverses；相关性排序 43

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**标签：** 条件数、数值反证

**定位：** 测量局部 Jacobian/逆条件数，构造扰动压力测试，并用 finite-difference/flow 正则约束。

**问题：** 数学上一一映射仍可能因巨大逆条件数在浮点数中失效。

**机制：** 测量局部 Jacobian/逆条件数，构造扰动压力测试，并用 finite-difference/flow 正则约束。

**步骤：**

1. 估计逆条件数
2. 扰动输出
3. 测重构与梯度误差
4. 加入稳定正则

**证据：**

- 无正则 additive 模型可有 4.3×10^-2 重构误差与 7.2×10^4 条件数。
- affine 版本可为 Inf 和 8.6×10^14。
- 正则后误差约 10^-3 且精度基本保持。

**局限：**

- 估计与正则增加计算。
- 不是所有可逆网的统一最坏界。

**意义：**

- 把代数可逆、数值条件与系统可靠性分开。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- 理论/压力测试
- Table 2
- memory-saving gradients

**资源：** [一手入口](<https://proceedings.mlr.press/v130/behrmann21a.html>)

---

<a id="paper-momentum-resnet-2021"></a>
**44. 动量残差神经网络｜Momentum Residual Neural Networks（2021 · ICML 2021）**

**作者：** Michael E. Sander、Pierre Ablin、Mathieu Blondel、Gabriel Peyré

**书目：** 年份 2021；载体 ICML 2021；状态 同行评议；来源类型 paper

**分类：** 主路线 可逆与信息保持；相关路线 可逆与信息保持、跨层聚合拓扑；层级 ImageNet 规模；阅读层级 核心；证据等级 A；简称 Momentum ResNet；相关性排序 44

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q3

**标签：** 动量、bit buffer

**定位：** 引入 v：v'=γv+(1-γ)f(x)，x'=x+v'；新状态可闭式恢复，bit buffer 保存有限精度位。

**问题：** coupling 改结构，i-ResNet 需收缩约束，希望对任意 block 做 drop-in 可逆化。

**机制：** 引入 v：v'=γv+(1-γ)f(x)，x'=x+v'；新状态可闭式恢复，bit buffer 保存有限精度位。

**步骤：**

1. 引入动量 v
2. 残差更新 v
3. v 更新 x
4. 闭式反演
5. bit buffer

**证据：**

- Table 1 比较闭式逆与约束。
- CIFAR 每模型 10 次，CIFAR-10 等效，CIFAR-100 低约 0.5 点。
- ImageNet 接近但仍有约 1 点内差距。

**局限：**

- bit buffer 不是零存储。
- 固定 γ、额外状态使同参数不等于同延迟。

**意义：**

- 融合二阶动力学与激活重构。

**边界：** ICML 正式版；相近 m-RevNet 只能核得预印本，未冒充 ICCV。

**证据位置：**

- 更新/逆方程
- Tables 1–2
- ImageNet Figure 6

**资源：** [一手入口](<https://proceedings.mlr.press/v139/sander21a.html>)

---

<a id="paper-ensemble-view-2016"></a>
**45. 残差网络表现得像相对浅层网络的集合｜Residual Networks Behave Like Ensembles of Relatively Shallow Networks（2016 · NeurIPS 2016）**

**作者：** Andreas Veit、Michael J. Wilber、Serge Belongie

**书目：** 年份 2016；载体 NeurIPS 2016；状态 同行评议；来源类型 paper

**分类：** 主路线 机制诊断与边界；相关路线 机制诊断与边界、随机路径正则；层级 ImageNet 规模；阅读层级 核心；证据等级 A；简称 Unraveled ensemble view；相关性排序 45

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 路径展开、解释边界

**定位：** 展开 identity/residual 路径，并用删除、重排和梯度长度分布测试 ensemble-like 行为。

**问题：** 需要解释极深网络的有效路径长度与路径依赖。

**机制：** 展开 identity/residual 路径，并用删除、重排和梯度长度分布测试 ensemble-like 行为。

**步骤：**

1. 展开多路径
2. 删除块
3. 重排块
4. 统计梯度路径长度

**证据：**

- ResNet-110 大部分梯度来自约 10–34 层路径。
- 删除块使性能渐进下降。
- 正式评审指出非线性下“指数独立集成”等价过强。

**局限：**

- 共享权重和非线性使路径不独立。
- lesion 不能证明唯一机制。

**意义：**

- 为随机路径提供背景并限制“指数集成”叙述。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- 路径展开
- lesion/reordering
- 梯度图
- 官方评审

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2016/hash/37bc2f75bf1bcfe8450a1a41c200364c-Abstract.html>)

---

<a id="paper-fractalnet-2017"></a>
**46. FractalNet：无残差的超深网络｜FractalNet: Ultra-Deep Neural Networks without Residuals（2017 · ICLR 2017）**

**作者：** Gustav Larsson、Michael Maire、Gregory Shakhnarovich

**书目：** 年份 2017；载体 ICLR 2017；状态 同行评议；来源类型 paper

**分类：** 主路线 机制诊断与边界；相关路线 机制诊断与边界、随机路径正则、跨层聚合拓扑；层级 小型分类基准；阅读层级 背景；证据等级 B；简称 FractalNet；相关性排序 46

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** 直接替代、反事实

**定位：** 递归构造多长度路径，join 取均值，训练用 local/global drop-path。

**问题：** 需要区分 residual addition 与更一般短路径脚手架。

**机制：** 递归构造多长度路径，join 取均值，训练用 local/global drop-path。

**步骤：**

1. 递归分形路径
2. join 均值
3. drop-path 训练
4. 测试完整图或单列

**证据：**

- CIFAR 表明无 ResNet identity addition 也能训练深网。
- 单列结果支持多路径脚手架解释。

**局限：**

- 仍含丰富短路径，不是 plain 串行网。
- 宽度、计算和显存增加。

**意义：**

- 反证残差表征是深度训练唯一必要条件。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- §2–3
- 完整图/单列实验

**资源：** [一手入口](<https://openreview.net/forum?id=S1VaB4cex>)

---

<a id="paper-shattered-gradients-2017"></a>
**47. 破碎梯度问题：如果 ResNet 是答案，问题是什么？｜The Shattered Gradients Problem: If resnets are the answer, then what is the question?（2017 · ICML 2017）**

**作者：** David Balduzzi、Marcus Frean、Lennox Leary、J. P. Lewis、Kurt Wan-Duo Ma、Brian McWilliams

**书目：** 年份 2017；载体 ICML 2017；状态 同行评议；来源类型 paper

**分类：** 主路线 机制诊断与边界；相关路线 机制诊断与边界；层级 小型分类基准；阅读层级 核心；证据等级 A；简称 Shattered Gradients；相关性排序 47

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 梯度相关性

**定位：** 分析相关性随深度衰减；plain 为指数，skip 架构更接近次线性，并给 looks-linear 初始化对照。

**问题：** 梯度范数正常时，不同输入的梯度相关性仍可能像白噪声般消失。

**机制：** 分析相关性随深度衰减；plain 为指数，skip 架构更接近次线性，并给 looks-linear 初始化对照。

**步骤：**

1. 定义破碎相关性
2. 推导深度衰减
3. 比较有无 skip
4. 卷积/全连接核对

**证据：**

- 理论与实验均显示 skip 更抗梯度破碎。
- 初步 LL 初始化可训练更深无 skip 网络。

**局限：**

- 偏初始化与理想随机网络。
- 不证明训练全程或泛化只由该机制解释。

**意义：**

- 把优势从梯度幅度扩展到方向结构。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- 主要理论
- 全连接/卷积实验

**资源：** [一手入口](<https://proceedings.mlr.press/v70/balduzzi17b.html>)

---

<a id="paper-skip-singularities-2018"></a>
**48. 跳跃连接消除奇异性｜Skip Connections Eliminate Singularities（2018 · ICLR 2018）**

**作者：** Emin Orhan、Xaq Pitkow

**书目：** 年份 2018；载体 ICLR 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 机制诊断与边界；相关路线 机制诊断与边界；层级 小型分类基准；阅读层级 核心；证据等级 A；简称 Singularity analysis；相关性排序 48

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 损失面、不可辨识性

**定位：** 分析重叠、消除和线性依赖奇异性；shortcut 打破对称并让初始化远离这些结构。

**问题：** 不可辨识性的退化流形会减慢学习。

**机制：** 分析重叠、消除和线性依赖奇异性；shortcut 打破对称并让初始化远离这些结构。

**步骤：**

1. 识别三类奇异性
2. 分析对称破坏
3. 测局部几何
4. 真实数据核对

**证据：**

- 简化模型与深网实验支持假设。
- 典型初始化被推离奇异结构。

**局限：**

- 不能泛化为消除所有优化障碍。
- 与梯度/信号解释并不互斥。

**意义：**

- 提供损失面几何视角。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- 奇异性分类
- 理论
- 真实数据实验

**资源：** [一手入口](<https://openreview.net/forum?id=HkwBEMWCZ>)

---

<a id="paper-iterative-inference-2018"></a>
**49. 残差连接促进迭代推断｜Residual Connections Encourage Iterative Inference（2018 · ICLR 2018）**

**作者：** Stanisław Jastrzębski、Devansh Arpit、Nicolas Ballas、Vikas Verma、Tong Che、Yoshua Bengio

**书目：** 年份 2018；载体 ICLR 2018；状态 同行评议；来源类型 paper

**分类：** 主路线 机制诊断与边界；相关路线 机制诊断与边界；层级 小型分类基准；阅读层级 核心；证据等级 B；简称 Iterative Inference；相关性排序 49

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** 迭代细化、共享反例

**定位：** 测量残差更新与损失负梯度的对齐，区分早层表征学习与后层迭代细化，并测试共享权重。

**问题：** 路径集成不是唯一解释，需要检验 blocks 是否逐步修正表征。

**机制：** 测量残差更新与损失负梯度的对齐，区分早层表征学习与后层迭代细化，并测试共享权重。

**步骤：**

1. 测残差方向
2. 与负梯度比较
3. 区分层级角色
4. 共享权重反事实

**证据：**

- CIFAR 中高层更像迭代细化。
- 朴素共享会表征膨胀并伤害泛化。

**局限：**

- 现象依数据和架构，不是普遍定理。
- 共享负结果不等于所有循环式架构失败。

**意义：**

- 给出行为解释及其直接反例。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- 理论形式化
- 中间表征
- 共享消融

**资源：** [一手入口](<https://openreview.net/forum?id=SJa9iHgAZ>)

---

<a id="paper-shortcut-importance-2019"></a>
**50. 理解残差网络中捷径连接的重要性｜Towards Understanding the Importance of Shortcut Connections in Residual Networks（2019 · NeurIPS 2019）**

**作者：** Tianyi Liu、Minshuo Chen、Mo Zhou、Simon S. Du、Enlu Zhou、Tuo Zhao

**书目：** 年份 2019；载体 NeurIPS 2019；状态 同行评议；来源类型 paper

**分类：** 主路线 机制诊断与边界；相关路线 机制诊断与边界；层级 机制与理论；阅读层级 核心；证据等级 A；简称 Shortcut convergence theory；相关性排序 50

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 收敛理论、外推边界

**定位：** 在二层非重叠卷积 ResNet 中分析伪局部最优，证明特定归一化与首层零初始化下梯度下降全局收敛。

**问题：** 希望给 shortcut 改变非凸优化的可证明解释。

**机制：** 在二层非重叠卷积 ResNet 中分析伪局部最优，证明特定归一化与首层零初始化下梯度下降全局收敛。

**步骤：**

1. 构造受限模型
2. 识别伪局部最优
3. 首层置零并归一化
4. 证明收敛

**证据：**

- 正式定理给多项式时间全局收敛。
- 数值实验支持受限模型。

**局限：**

- 只有两层且卷积不重叠。
- 依赖特定初始化/归一化，不能外推真实深网。

**意义：**

- 提供严格但窄范围的优化证据。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- 主要定理
- 数值实验

**资源：** [一手入口](<https://proceedings.neurips.cc/paper/2019/hash/7716d0fc31636914783865d34f6cdfd5-Abstract.html>)

---

<a id="paper-diracnet-2018"></a>
**51. DiracNet：无需跳跃连接训练超深网络｜DiracNets: Training Very Deep Neural Networks Without Skip-Connections（2018 · ICLR 2018 submission / arXiv）**

**作者：** Sergey Zagoruyko、Nikos Komodakis

**书目：** 年份 2018；载体 ICLR 2018 submission / arXiv；状态 预印本；来源类型 paper

**分类：** 主路线 机制诊断与边界；相关路线 机制诊断与边界、残差更新控制与稳定化；层级 ImageNet 规模；阅读层级 背景；证据等级 C；简称 DiracNet；相关性排序 51

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P1 / Q=Q2

**标签：** 无显式捷径、负面对照

**定位：** 把可学习倍率乘 Dirac identity 加剩余卷积折入卷积核，从近恒等开始但无单独 shortcut。

**问题：** 需要区分近恒等参数化与显式加法捷径。

**机制：** 把可学习倍率乘 Dirac identity 加剩余卷积折入卷积核，从近恒等开始但无单独 shortcut。

**步骤：**

1. 嵌入 Dirac identity
2. 学习倍率与剩余核
3. 堆叠无 shortcut 深网
4. 与 ResNet 比较

**证据：**

- ImageNet DiracNet-18/34 error 30.37/27.79，对应 ResNet 29.62/27.17。
- CIFAR 上 ResNet 参数效率也更好。

**局限：**

- 只核得预印本/投稿状态。
- 卷积参数化不复现加法路径组合。

**意义：**

- 反证显式 shortcut 是唯一方式，同时保留其略逊基线。

**边界：** 按预印本收录，不标成已确认 ICLR 正式论文。

**证据位置：**

- 参数化
- ImageNet 表
- CIFAR 效率

**资源：** [一手入口](<https://arxiv.org/abs/1706.00388>)

---

<a id="paper-invertible-approximation-limits-2020"></a>
**52. 神经 ODE 与可逆残差网络的逼近能力｜Approximation Capabilities of Neural ODEs and Invertible Residual Networks（2020 · ICML 2020）**

**作者：** Han Zhang、Xi Gao、Jacob Unterman、Tom Arodz

**书目：** 年份 2020；载体 ICML 2020；状态 同行评议；来源类型 paper

**分类：** 主路线 机制诊断与边界；相关路线 机制诊断与边界、可逆与信息保持；层级 机制与理论；阅读层级 核心；证据等级 A；简称 Invertible approximation limits；相关性排序 52

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** 拓扑限制、维度增广

**定位：** 用 homeomorphism 构造同维不可逼近目标，并证明维度增广或末端 linear cap 可解除限制。

**问题：** 信息保持是否必然不损表达能力？

**机制：** 用 homeomorphism 构造同维不可逼近目标，并证明维度增广或末端 linear cap 可解除限制。

**步骤：**

1. 识别拓扑限制
2. 构造不可逼近映射
3. 增加维度
4. 或加非可逆线性层

**证据：**

- 给出同维限制与 2p 维增广充分性定理。
- 实验演示增广/linear cap 解除失败。

**局限：**

- 存在性理论不直接预测有限数据精度。
- linear cap 使端到端不再可逆。

**意义：**

- 与 exploding inverse 构成表达和数值双边界。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- 主要定理
- 增广实验

**资源：** [一手入口](<https://proceedings.mlr.press/v119/zhang20h.html>)

---

<a id="paper-residual-alignment-2023"></a>
**53. 残差对齐：揭示残差网络机制｜Residual Alignment: Uncovering the Mechanisms of Residual Networks（2023 · NeurIPS 2023）**

**作者：** Jianing Li、Vardan Papyan

**书目：** 年份 2023；载体 NeurIPS 2023；状态 同行评议；来源类型 paper

**分类：** 主路线 机制诊断与边界；相关路线 机制诊断与边界；层级 小型分类基准；阅读层级 核心；证据等级 A；简称 Residual Alignment；相关性排序 53

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** Jacobian、训练后几何

**定位：** 线性化 blocks，做 residual Jacobian SVD，测量表示共线、奇异向量对齐、低秩与随深度缩放。

**问题：** 需要训练后几何描述解释 branches 如何协同。

**机制：** 线性化 blocks，做 residual Jacobian SVD，测量表示共线、奇异向量对齐、低秩与随深度缩放。

**步骤：**

1. 线性化 block
2. Jacobian SVD
3. 测跨层对齐
4. 移除 skip 反事实

**证据：**

- RA1–RA4 在全连接/卷积、多深度宽度和多数据集出现。
- 移除 skip 后现象停止，并在特定数学模型中证明。

**局限：**

- 作者定义的现象不是所有成功 ResNet 的充要条件。
- 缺少跨连接方法的独立统一复现。

**意义：**

- 提供 2023 年训练后几何视角。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- RA1–RA4
- Jacobian/表示实验
- no-skip 对照

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2023/hash/b3f48945f6fb402b4b5cdcf490e72847-Abstract-Conference.html>)

---

<a id="paper-gauss-newton-conditioning-2024"></a>
**54. 神经网络中 Gauss-Newton 条件性的理论刻画｜Theoretical characterisation of the Gauss-Newton conditioning in Neural Networks（2024 · NeurIPS 2024）**

**作者：** Jim Zhao、Sidak Pal Singh、Aurelien Lucchi

**书目：** 年份 2024；载体 NeurIPS 2024；状态 同行评议；来源类型 paper

**分类：** 主路线 机制诊断与边界；相关路线 机制诊断与边界、残差更新控制与稳定化；层级 小型分类基准；阅读层级 核心；证据等级 A；简称 Gauss-Newton conditioning；相关性排序 54

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**标签：** Gauss-Newton、谱平移

**定位：** 在线性网络中把 residual layer 写成 W+βI，分析谱平移和 GN 条件数界，并延伸到卷积 Toeplitz。

**问题：** shortcut 如何改变二阶曲率代理条件数仍缺少可计算理论。

**机制：** 在线性网络中把 residual layer 写成 W+βI，分析谱平移和 GN 条件数界，并延伸到卷积 Toeplitz。

**步骤：**

1. 矩阵化深网
2. 写成 W+βI
3. 分析谱平移
4. 推导 GN 条件数
5. 延伸卷积

**证据：**

- 给任意深线性网紧界和 residual/convolution 延伸。
- ResNet-20 只在约 1000 个 CIFAR 样本上诊断。

**局限：**

- 改善界依赖奇异向量假设。
- 线性/小样本不能外推大规模准确率。

**意义：**

- 增加曲率条件数视角并明确证据边界。

**边界：** NeurIPS 正式全文；题名从初筛 spectrum 纠正为 conditioning。

**证据位置：**

- residual 定理
- 卷积延伸
- CIFAR 子集图

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2024/file/d067d16e3e5fe8fa8a3e62909907659a-Paper-Conference.pdf>)

---

<a id="paper-residual-distillation-2020"></a>
**55. 残差蒸馏：迈向无捷径的可部署深网｜Residual Distillation: Towards Portable Deep Neural Networks without Shortcuts（2020 · NeurIPS 2020）**

**作者：** Guilin Li、Junlei Zhang、Yunhe Wang、Chuanjian Liu、Matthias Tan、Yunfeng Lin、Wei Zhang、Jiashi Feng、Tong Zhang

**书目：** 年份 2020；载体 NeurIPS 2020；状态 同行评议；来源类型 paper

**分类：** 主路线 机制诊断与边界；相关路线 机制诊断与边界、自适应门控与路由；层级 跨任务或系统级；阅读层级 桥接；证据等级 B；简称 Residual Distillation；相关性排序 55

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P3 / Q=Q1

**标签：** 训练部署解耦、系统指标

**定位：** 联合训练 ResNet 教师与 plain CNN，跨模型传递特征和混合梯度，部署只留学生。

**问题：** shortcut 延长推理期特征生命周期，能否只在训练时借用其优化优势？

**机制：** 联合训练 ResNet 教师与 plain CNN，跨模型传递特征和混合梯度，部署只留学生。

**步骤：**

1. 构建教师/学生
2. 传递中间特征
3. 混合梯度
4. 部署 plain CNN

**证据：**

- 作者报告 ImageNet/CIFAR 达 ResNet 基线。
- 约 1.4× 加速、1.25× 内存降低，并迁移 MIT67/Caltech101。
- 称 ResNet-50 shortcut 占约 40% 推理特征内存。

**局限：**

- 不证明 plain net 从头训练可替代 ResNet。
- 系统数字来自作者，缺独立统一复现。

**意义：**

- 拆开训练需要连接与部署需要连接。

**边界：** 已回到一手正文核验机制、结果与边界。

**证据位置：**

- 方法框架
- ImageNet/CIFAR
- 移动效率表
- 迁移实验

**资源：** [一手入口](<https://proceedings.neurips.cc/paper/2020/hash/657b96f0592803e25a4f07166fff289a-Abstract.html>) · [代码](<https://github.com/leoozy/JointRD_Neurips2020>)

---

<a id="检索与证据审计"></a>
## 检索与证据审计

<details>
<summary><strong>展开完整检索、纳排、去重、证据分级与覆盖限制</strong></summary>

### ResNet 残差连接改进：检索与证据审计

> 检索截止：2026-08-13；范围版本：1.0；最终语料：55 项；本文件记录可重复的查询、筛选、版本去重、反证与覆盖收敛过程。

#### 1. 范围与来源

研究问题是：从 2015 年前驱到截止日，卷积 ResNet 中直接改变捷径、残差合并、路径选择、跨块拓扑或恒等传播稳定条件的机制有哪些，它们的证据强度、代价和失败边界是什么？

来源优先级如下：

1. 会议或期刊正式页面与全文：CVF Open Access、NeurIPS Proceedings、PMLR、ICLR Proceedings、OpenReview 正式接收记录、AAAI、IJCAI、JMLR、Springer、IEEE DOI；
2. 与正式成果可消歧的作者预印本、arXiv、作者项目页与官方代码；
3. 二手索引只用于发现线索，不承担结论证据。

检索语言以英文为主，中文用于叙事与同义词回查。年份从 Highway Networks 的 2015 年正式前驱覆盖至 2026-08-13。数据库界面经常不提供稳定、可复现的总命中数，因此查询账本把 `hits` 记为 `null`，同时保留实际逐项筛选数；不以搜索引擎估算命中数伪装精确召回率。

#### 2. 查询矩阵与执行轮次

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

#### 3. 纳入、排除与最终裁决

##### 纳入规则

一项工作必须有可核验的一手入口，并至少直接改变以下一项：

- shortcut 映射、投影、下采样或滤波；
- 恒等路与残差路的合并系数、初始化、归一化条件或更新方向；
- 残差块是否执行以及随机或输入相关选择；
- 跨块、跨阶段、多分支的聚合拓扑；
- 通过耦合、状态增广或 Lipschitz 条件实现的可逆/信息保持连接；
- 对上述连接给出可定位的机制、负面结果或直接替代反证。

##### 排除规则

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

#### 4. 去重与版本族审计

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

#### 5. 全文、主张与证据向量审计

40 个核心条目逐项记录：2 至 5 步机制链、正文表图或章节位置、结果、局限、一手入口、出版状态与证据向量。桥接和背景项至少核验摘要、关键正文与正式元数据。证据向量四维为：

- `V`：来源与版本可核验程度；
- `D`：实验或理论设计对因果归因的支持程度；
- `P`：同行评议与出版成熟度；
- `Q`：结果透明度、重复运行、反例和复现信息。

主张账本包含 30 条高风险陈述，每条绑定论文 ID、原文位置、`V/D/P/Q` 和审核状态。重点不是给论文排总分，而是阻止四类偷换：累计改进冒充单因素改进，小型基准冒充 ImageNet 证据，作者系统结果冒充独立复现，代数性质冒充数值或系统性质。

#### 6. 反证与负面结果专项审计

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

#### 7. 覆盖饱和判断

操作性饱和通过五道闸门判断：

1. 七个独立查询家族均有可审计记录；
2. 七条路线均包含奠基或代表作、后续发展、反证或明确缺口；
3. 核心条目完成全文级机制、表图位置、结果与局限核验；
4. 版本冲突、撤稿、工作坊与预印本状态已单独审核；
5. 两个独立补漏轮次的边际新增率均低于 5%，分别为 3.7% 与 0%，且没有出现第八条路线。

第一轮唯一新增是 NeurIPS 2025 Orthogonal Residual Update。全文显示其 CNN-ResNet 仅在 CIFAR/Tiny ImageNet 验证，ImageNet-1k 只用于 ViT，因此严格放在 E1。第二轮返回的都是已知实体或 Transformer、U-Net、视觉语言、生成模型等语义伪候选。

这支持“主要机制路线已收敛”，不支持“所有论文已经穷尽”。任务特定微变体、低影响 venue、非英文数据库和截止日后的正式版本仍是更新风险。

#### 8. 访问、复现与审计限制

- 部分出版页面只提供元数据或跳转；遇到这种情况，使用同一版本族的官方 PDF、作者预印本或正式 proceedings 页面交叉核对，没有用博客摘要替代证据。
- 搜索接口不稳定展示总命中量，所以只报告实际人工筛选数与决策，不声称数据库层面的精确召回率。
- 本次复现审计针对原论文的重复运行、代码入口、预算口径、消融和失败条件；没有重新训练 55 个模型，也没有把“存在官方代码”等同于可在当前硬件完整复现。
- 没有找到覆盖 PreAct、ResNet-D、残差缩放、SkipInit/ReZero、随机深度/Shake、聚合和可逆路线的现代独立统一复现。多数旧论文还缺置信区间、等算力、真实设备延迟或峰值内存。
- 生成式模型、Transformer 与任务特定残差技巧只用于检查边界，不应由其负面结果反推 CNN-ResNet 分类结论。

#### 9. 审计产物索引

- `planning/research_contract.yaml`：冻结的范围合同；
- `planning/search_ledger.jsonl`：12 轮精确查询记录；
- `planning/screening.csv`：84 个候选的最终纳排；
- `planning/claim_ledger.csv`：30 条高风险主张的证据审计；
- `planning/agents/`：三份独立证据包；
- `atlas.json`：55 个最终实体的数据真源；
- `data/`：编译生成的 JSON、CSV、BibTeX 和离线网页数据。

</details>

<a id="复现与使用边界"></a>
## 复现与使用边界

- `atlas.json` 是人工维护的结构化研究真源；`data/`、网页与本 README 是确定性派生阅读层。
- 页面可离线打开；论文、代码、数据集与官方图表等一手外部入口需要联网。
- 机制步骤与网页机制图是依据一手文字证据形成的解释性整理，不替代原论文图表或独立复核。
- 出版状态、阅读优先级、证据等级与展示层级是不同维度，不能互相替代。
- 本综述有明确截止日期和纳入边界，不声称穷尽互联网中的全部长尾资料。

生成与验证工具：[`build-research-atlas`](https://github.com/Linwei-Chen/build-research-atlas)。
