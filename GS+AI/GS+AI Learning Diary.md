---
title: GS+AI Learning Diary
aliases:
  - 全基因组选择+AI 四个月时间做毕业设计
tags:
  - 毕业论文
  - 全基因组选择
  - AI
  - 学习计划
created: 2026-10-24
status: in-progress
---

# 毕业论文撰写指南：四个月从零到算法创新（全基因组选择 + AI）

> 适用对象：计算机/AI 专业，PyTorch 无完整项目经验，基因组选择方向零基础
> 时间线：2026-10-26 起 16 周（约 4 个月），建议每周 15 小时
> 主数据：华中农业大学 RiceVarMap v3（水稻，基因型 + 表型）
> 说明：本指南覆盖"学习路线 → 实验 → 论文写作"的完整流程。

---

## 如何使用本指南

- **第 0–1 周**：读第 0、1 节，然后直接按第 8 节的每日任务开工，不要等"规划完再开始"
- **每周日**：对照第 1 节表格勾掉本周任务，更新第 3 节知识清单
- **每个里程碑（W4 / W8 / W12 / W16）**：用第 6 节的汇报模板和导师沟通
- **进度落后**：直接按第 7 节的"砍单顺序"删任务，不要自己乱删
- **最后一个月**：回到第 5 节，按实验矩阵逐项跑

---

## 0. 先说结论：三条关键取舍

1. **"打基础"= 以主数据为练手对象打基础**。第 5 周就开始碰华农数据，不要等"学完再做"。
2. **最后一个月只做"实验 + 写作"，不再学新东西**。所以基线复现、数据管线、方法雏形必须在第 12 周末前完成。
3. **创新点选"无标签基因型自监督预训练 + 小样本微调"**。原因：华农数据里 4,726 份有基因型、只有 529 份有表型——"数据稀缺"正是自监督预训练的主场，故事天然成立，而且模型小、算力低、适合一个人一个月做完。

---

## 1. 十六周总览

| 阶段                | 周次                | 主题                 | 核心任务                                                                 | 里程碑（验收标准）                     |
| ----------------- | ----------------- | ------------------ | -------------------------------------------------------------------- | ----------------------------- |
| **第 1 月：基础闭环**    | W1 (10/26–11/01)  | Python 数据栈 + ML 复习 | numpy/pandas、sklearn、CV、指标；EasyGeSe 上跑通 Ridge + 5 折 CV               | 能一条命令输出 Pearson r             |
|                   | W2 (11/02–11/08)  | PyTorch 入门         | Tensor/autograd/nn.Module/DataLoader/训练循环；MLP 跑通 EasyGeSe            | MLP 结果与 Ridge 同量级             |
|                   | W3 (11/09–11/15)  | 深度学习组件             | 1D-CNN、attention、归一化、dropout、早停、过拟合；读 DNNGP                          | 写出 1D-CNN 并训练成功               |
|                   | W4 (11/16–11/22)  | 第一次对比实验            | 复现 DNNGP demo；Ridge/GBLUP/LightGBM/CNN 对比表；写 1 页总结                   | **M1：一份可给导师看的对比表**（可直接用于开题）   |
| **第 2 月：数据 + GS** | W5 (11/23–11/29)  | GS 概念 + 数据下载       | SNP 编码、GBLUP、CV1/CV2、MAF/LD/群体结构；下载 RiceVarMap v3 PLINK + phenos.csv | 数据下载完成，能打印矩阵形状                |
|                   | W6 (11/30–12/06)  | HZAU 数据基线          | 质控（MAF>0.05、缺失<0.1、LD 剪枝）；GBLUP + LightGBM 在 11 性状上跑 CV1/CV2         | 基线结果表 v1                      |
|                   | W7 (12/07–12/13)  | 论文精读               | DeepGS / DNNGP / GEFormer 三篇，固定四栏笔记                                  | 3 份笔记 + 能讲清"改进点类型"            |
|                   | W8 (12/14–12/20)  | 管线工程化              | PLINK→numpy(.npz)、表型对齐、固定 split、随机种子、结果自动汇总                          | **M2：端到端可复现 pipeline + 基线数字** |
| **第 3 月：方法与雏形**   | W9 (12/21–12/27)  | 自监督学习基础            | AutoEncoder / MAE / BERT 式 masked modeling；在无标签基因型上跑 toy 预训练         | pretrain.py 跑通，loss 下降        |
|                   | W10 (12/28–01/03) | 预训练→微调打通           | 4,726 无标签预训练 + 529 有表型微调；少样本 10/25/50/100%                           | 初步结果表（有收益/无收益都要记录）            |
|                   | W11 (01/04–01/10) | 实验框架               | config 管理、多 seed、日志、消融模板、结果汇总 CSV                                    | 一键跑完整组实验                      |
|                   | W12 (01/11–01/17) | 中期总结               | 确定创新点与最终实验矩阵；写中期报告/论文方法初稿                                            | **M3：方法雏形 + 中期材料**            |
| **第 4 月：创新冲刺**    | W13 (01/18–01/24) | 主实验                | 主方法 vs 4 类基线，CV1/CV2，10 seeds，11 性状                                  | 主结果表 v1                       |
|                   | W14 (01/25–01/31) | 消融 + 泛化            | 掩码比例/token 粒度/预训练数据量；跨亚群；EasyGeSe 其他物种                               | 消融表 + 泛化表                     |
|                   | W15 (02/01–02/07) | 可解释性 + 图表          | 梯度×输入 / SHAP / attention 可视化；论文图表统一风格                                | 全部图表定稿                        |
|                   | W16 (02/08–02/14) | 写作 + 答辩            | 论文成稿、PPT、代码整理、复现说明                                                   | **M4：论文初稿 + 可运行代码仓库**         |

> 注：W15 前后可能赶上春节，把"写作、整理、画图"这类低强度任务排在这段，关键实验放在 W13–W14。

---

## 2. 每周时间分配模板（15 小时）

| 模块 | 时长 | 内容 |
|---|---|---|
| 学新知识 | 3 h | 视频/教程/书，只学当周要用的 |
| 读论文 | 2 h | 每周 1 篇，只读图 + 方法 + 结果表 |
| 写代码 | 8 h | 当周任务，必须 commit |
| 记录与汇报 | 2 h | 实验日志 + 每两周一次导师汇报 |

**铁律：每周结束时必须有"能运行的东西"或"能看的表"，不能只有"看完了"。**

---

## 3. 知识清单（教学版：每条 = 概念 + 上手项目 + 溯源）

> 用法：每条三个层次——**概念**（先懂一句话）、**上手**（照着做一个能跑的东西）、**溯源**（要引用/查细节时点进去）。勾选表示"上手部分做完了"，不是"看完了"。
> 两套数据别混：**EasyGeSe 内置 rice** 是 W1–W4 的练手沙盒（小、干净、加载一行搞定）；**华农 RiceVarMap v3** 才是主数据（W5 起正式用）。练手用前者，结论用后者。

### 3.0 工具速查表（先收藏，用到哪个装哪个）

| 工具 / 项目 | 是什么 | 语言 | 安装 | 溯源 |
|---|---|---|---|---|
| **EasyGeSe** | 基因组预测基准资源：10 物种（含水稻）预处理好数据 + 加载器，W1–W4 练手 | R / Python | `git clone https://github.com/stevenandrewyates/EasyGeSe`（数据在 Zenodo） | [GitHub](https://github.com/stevenandrewyates/EasyGeSe) · [论文 BMC Genomics 2025](https://pubmed.ncbi.nlm.nih.gov/41136918/) |
| **DNNGP** | 1D-CNN 基因组预测（3 层卷积），W3–W4 精读/复现对象 | Python / TF 2.6 | `git clone https://github.com/AIBreeding/DNNGP` | [GitHub](https://github.com/AIBreeding/DNNGP) · [Mol Plant 2023](https://doi.org/10.1016/j.molp.2022.11.004) |
| **DeepGS** | CNN 基因组选择 R 包（8-32-1 结构），W7 精读 | R / MXNet | `docker pull malab/deepgs_cpu` | [GitHub](https://github.com/cma2015/DeepGS) |
| **GEFormer** | 基因型×环境 Transformer（gMLP + 线性注意力），W7 精读 | Python / PyTorch | `git clone https://github.com/Deep-Breeding/GEFormer` | [GitHub](https://github.com/Deep-Breeding/GEFormer) · [Mol Plant 2025](https://doi.org/10.1016/j.molp.2025.01.020) |
| **pandas-plink** | 读 PLINK 二进制（.bed/.bim/.fam）进 Python | Python | `pip install pandas-plink` | [GitHub](https://github.com/limix/pandas-plink) |
| **PLINK** | 基因型质控、MAF 过滤、LD 剪枝 | C/C++ 二进制 | 官网下载二进制 | [官网](https://www.cog-genomics.org/plink/) |
| **BGLR** | 贝叶斯回归 / GBLUP / BayesA·B·C 基线 | R | `install.packages("BGLR")` | [GitHub](https://github.com/gdlc/BGLR-R) |
| **RiceVarMap v3** | 华农水稻主数据（4,726 基因型 / 529 表型 / 11 性状） | 数据 | 网页下载 PLINK + phenos.csv | [下载页](https://ricevarmap.ncpgr.cn/v3/download/) · [教程](https://ricevarmap.ncpgr.cn/v3/tutorial/) |
| **MAE** | 掩码自编码器（你的创新点直接参考） | PyTorch | `git clone https://github.com/facebookresearch/mae` | [GitHub](https://github.com/facebookresearch/mae) · [论文 2022](https://arxiv.org/abs/2111.06377) |

> 注：raw.githubusercontent.com / GitHub 克隆在国内可能需要 VPN（你已有）。Zenodo 大数据下载建议挂代理或断点续传。

### 3.1 编程 / 工具

- [ ] **numpy / pandas / matplotlib（W1）**
  - 概念：numpy 是多维数组——基因型矩阵就是 n 份样本 × m 个 SNP 的 0/1/2 二维数组；pandas 是表格（读表型 CSV）；matplotlib 画分布和散点。
  - 上手：用 pandas 读华农 `phenos.csv`，统计缺失值、画直方图；用 numpy 造一个 `100×1000` 随机基因型矩阵，手写算一遍 MAF（见 3.4）。
  - 溯源：[numpy 文档](https://numpy.org/doc/) · [pandas 文档](https://pandas.pydata.org/docs/) · [matplotlib 教程](https://matplotlib.org/stable/tutorials/)

- [ ] **Git / GitHub（W1）**
  - 概念：每次 `commit` 是一个可回退的快照，`branch` 是平行开发分支；所有实验代码必须进仓库才能复现、才能给导师看。
  - 上手：建私有仓库 `gs-ai-thesis`，push 第一份 README + 空 `data_pipeline/` 目录；养成"每天 commit"习惯。
  - 溯源：[Pro Git 书](https://git-scm.com/book/zh/v2) · [GitHub 文档](https://docs.github.com/)

- [ ] **PyTorch（W2）**
  - 概念：`Tensor`（可上 GPU 的多维数组）、`autograd`（自动求导→反向传播）、`nn.Module`（网络层基类）、`Dataset/DataLoader`（批量喂数据）、训练循环（forward → loss → backward → optimizer.step）、`state_dict` 存模型。
  - 上手：跑通一个 3 层 MLP 预测 EasyGeSe rice 表型；用 `.to('cuda')` 上 GPU，打印显存占用。
  - 溯源：[PyTorch 官方 60 分钟](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html) · [中文翻译版](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html)

- [ ] **GPU 与实验管理（W2 / W11）**
  - 概念：`.to(device)` 把张量/模型搬到 GPU；混合精度（AMP）省显存提速度（可选）。实验管理 = 配置进 YAML、固定随机种子、日志到文件、结果自动汇总 CSV，保证"一键复现"。
  - 上手：写一个 `config.yaml` + `train.py`，换参数只改配置文件；固定 `torch.manual_seed(0)`。
  - 溯源：[PyTorch AMP 文档](https://pytorch.org/docs/stable/amp.html) · [hydra 配置库](https://hydra.cc/)（可选）

### 3.2 机器学习

- [ ] **训练/验证/测试划分 + 交叉验证（W1）**
  - 概念：训练集学参数、验证集调超参、测试集最后只碰一次；K 折 CV 把数据切 K 份轮流当验证，取平均更稳。**关键**：华农数据必须按品种（家系/亚群）分组划分，不能随机按样本切——否则亲属混进测试集，分数虚高（这就是 CV2 的意义，见 3.4）。
  - 上手：sklearn `train_test_split` + `KFold` 在随机数据跑通；再实现按亚群分组的 `GroupKFold`。
  - 溯源：[sklearn 交叉验证文档](https://scikit-learn.org/stable/modules/cross_validation.html)

- [ ] **过拟合与正则化（W1–W3）**
  - 概念：过拟合=训练集很好、测试集差；L1/L2 给损失加参数惩罚项，dropout 随机丢神经元，早停=验证 loss 不再下降就停。GS 里样本少、SNP 多，极容易过拟合，这三样是救命稻草。
  - 上手：在 MLP 上做 A/B——加/不加 dropout、加/不加 L2、早停 vs 跑满，对比验证曲线。
  - 溯源：[3Blue1Brown 神经网络](https://www.3blue1brown.com/topics/neural-networks) · [Andrew Ng 机器学习](https://www.coursera.org/learn/machine-learning)

- [ ] **常用模型：Ridge / LightGBM / GBLUP（W1 / W5）**
  - 概念：Ridge 是带 L2 的线性回归（GBLUP 本质是"用亲缘矩阵当特征的 Ridge"）；LightGBM 是梯度提升树，对表格型小样本极稳，往往是深度模型的强基线。**你的深度模型必须先打赢这两个才有意义。**
  - 上手：在 EasyGeSe 上跑 Ridge + LightGBM，得到第一张对比表——这就是 W4 里程碑 M1 的原料。
  - 溯源：[sklearn Ridge](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.Ridge.html) · [LightGBM 文档](https://lightgbm.readthedocs.io/)

- [ ] **评价指标：Pearson r / MSE / Spearman（W1）**
  - 概念：r 是预测值与真实值的线性相关（GS 论文最常用，∈[−1,1]，越大越好）；MSE 是均方误差；Spearman 是秩相关（对极端值更稳）。GS 领域统一报 r。
  - 上手：自己写 `corr(y_true, y_pred)`，和 `scipy.stats.pearsonr` 对照一致；写进一个 `metrics.py` 复用。
  - 溯源：[scipy.stats](https://docs.scipy.org/doc/scipy/reference/stats.html)

### 3.3 深度学习

- [ ] **MLP / 激活函数 / 损失 / 优化器（W2）**
  - 概念：MLP=若干全连接层堆叠；ReLU 引入非线性；回归用 MSE loss，分类用交叉熵；Adam 是默认优化器，SGD 更稳但慢。
  - 上手：PyTorch 手写 3 层 MLP，在 EasyGeSe rice 训练，确认结果与 Ridge 同量级（W2 验收标准）。
  - 溯源：[PyTorch 快速入门](https://pytorch.org/tutorials/beginner/basics/intro.html)

- [ ] **1D-CNN（W3）**
  - 概念：CNN 用卷积核在序列上滑动提取局部特征，权重共享；基因型是 1D 序列，1D-CNN 天然适合。**DNNGP 就是 1D-CNN**：3 层卷积 + BatchNorm + dropout + 全连接，输入先做 PCA 降维。
  - 上手：打开 DNNGP 仓库，读 `DNNGP_OPN.py` 的网络结构，用它的 demo（wheat599 / maize1404）跑一遍；再自己写一个 1D-CNN 在 EasyGeSe 上训练。
  - 溯源：[DNNGP GitHub](https://github.com/AIBreeding/DNNGP) · [Mol Plant 2023](https://doi.org/10.1016/j.molp.2022.11.004)

- [ ] **Attention / Transformer（W3）**
  - 概念：attention 让序列每个位置能"加权地看"其他位置；Transformer = 多层 self-attention + 前馈。基因型按窗口/LD block 切成 token 序列就能喂 Transformer。**GEFormer** 是"基因型 + 环境"联合输入的 Transformer（gMLP + 线性注意力）。
  - 上手：读《The Illustrated Transformer》图解（最易上手）；再看 GEFormer 仓库怎么组织 geno/pheno/env 三个输入。
  - 溯源：[图解 Transformer](https://jalammar.github.io/illustrated-transformer/) · [GEFormer GitHub](https://github.com/Deep-Breeding/GEFormer) · [Attention Is All You Need](https://arxiv.org/abs/1706.03762)

- [ ] **归一化与残差连接（W3）**
  - 概念：BatchNorm 对一批样本归一化（稳定训练），LayerNorm 对单样本的特征归一化（Transformer 常用）；残差连接=输出加输入，缓解深层网络梯度消失。
  - 上手：在 1D-CNN 里做"加/不加 BN"的对比，观察训练是否更稳。
  - 溯源：[BatchNorm 论文](https://arxiv.org/abs/1502.03167) · [ResNet 论文](https://arxiv.org/abs/1512.03385)

- [ ] **自监督：AutoEncoder / MAE / 掩码建模（W9，创新点核心）**
  - 概念：自监督=不靠人工标签，用数据自身造监督信号；AutoEncoder 是"压缩再重建"；MAE/BERT 式=随机遮住一部分输入、让模型预测被遮住的部分。**你的创新点**：遮住基因型片段→模型重建→学到遗传表示→下游小样本微调预测表型（正好利用 4,726 份无表型基因型）。
  - 上手：先跑 MAE 官方 toy（MNIST 或 CIFAR）理解 mask→encoder→decoder→重建 loss；再把输入换成"基因型窗口序列"做一个最小可跑版本。
  - 溯源：[MAE GitHub](https://github.com/facebookresearch/mae) · [MAE 论文](https://arxiv.org/abs/2111.06377) · [BERT 论文](https://arxiv.org/abs/1810.04805)

### 3.4 领域知识（华农数据 + GS，只学够用的）

- [ ] **基因型矩阵编码 0/1/2（W5）**
  - 概念：每个 SNP（单核苷酸多态性）有两个等位基因（如 A/G）；某样本在该位点有三种状态——0（两个都是参考等位）、1（杂合）、2（两个都是替代等位），缺失记 NaN。华农 rice4k 约 4,726 份 × 上百万 SNP。
  - 上手：用 pandas-plink 读华农 PLINK 数据（.bed/.bim/.fam），打印矩阵形状、每个 SNP 的缺失率和 MAF，存成 `.npz`。
  - 溯源：[pandas-plink GitHub](https://github.com/limix/pandas-plink) · [PLINK 格式说明](https://www.cog-genomics.org/plink/)

- [ ] **质控：MAF / 缺失率 / LD（W5–W6）**
  - 概念：MAF（次等位基因频率）太低的 SNP 信息量小、易测错；缺失率高的位点/样本要剔除；LD（连锁不平衡）=相邻 SNP 高度相关，需剪枝去冗余，否则输入维度过大且重复。
  - 上手：PLINK 一条命令完成 `--maf 0.05 --geno 0.1 --indep-pairwise 50 5 0.2`，把百万 SNP 降到 5–10 万。
  - 溯源：[PLINK 官方文档](https://www.cog-genomics.org/plink/) · [华农下载页](https://ricevarmap.ncpgr.cn/v3/download/)

- [ ] **表型与 BLUP（W5）**
  - 概念：表型=观测到的性状值（株高、粒重等连续值）；多环境重复测量的表型常用 BLUP（最佳线性无偏预测）估计遗传值、剥离环境噪声。华农 529 份核心种质 × 11 个性状。
  - 上手：读华农 `phenos.csv`，画 11 个性状直方图，挑 5 个高遗传力性状（粒长、粒宽、粒重、株高、抽穗期）。
  - 溯源：[华农下载页](https://ricevarmap.ncpgr.cn/v3/download/) · [BGLR 文档](https://github.com/gdlc/BGLR-R)

- [ ] **GBLUP（W5）**
  - 概念：用亲缘关系矩阵 G（由基因型算个体间遗传相似度）做线性混合模型预测表型；直觉="和你基因像的个体，表型也像"。它是 GS 的经典基线，你的深度模型必须和它比、且说清何时更优。
  - 上手：用 BGLR（R）或 `sommer` 包在华农数据跑 GBLUP，得到 11 个性状的 r 基线表。
  - 溯源：[BGLR GitHub](https://github.com/gdlc/BGLR-R) · [VanRaden 2008 原论文](https://doi.org/10.3168/jds.2007-0980)

- [ ] **CV1 vs CV2（W5）**
  - 概念：CV1=随机把个体分训练/测试；CV2=按家系/亚群分组（测试集个体与训练集不同亲缘），更接近真实育种场景，通常 r 更低但更可信。
  - 上手：两种划分各实现一次，对比 GBLUP 的 r，亲身体会"CV2 为什么更难、为什么更真实"。
  - 溯源：[Crossa et al. 2017 GS 综述](https://doi.org/10.1016/j.tplants.2017.08.012)

- [ ] **群体结构 / LD / 编码影响（W5–W6）**
  - 概念：群体结构=样本来自不同亚群（籼/粳稻等），会引入虚假相关性；LD 让相邻 SNP 携带重复信息；这些都影响模型泛化，也解释了 CV2 更难的根源。
  - 上手：对华农 4,726 份做 PCA（PLINK `--pca`），画前两个主成分，看是否按亚群分开（品种信息 CSV 里有亚群标签）。
  - 溯源：[RiceVarMap 品种信息](https://ricevarmap.ncpgr.cn/v3/download/) · [PLINK PCA 文档](https://www.cog-genomics.org/plink/)

---

## 4. 数据管线（华农 RiceVarMap v3）具体步骤

**下载**（W5 完成）：
- 基因型：<https://ricevarmap.ncpgr.cn/v3/download/> → PLINK 格式 `rice4k_geno_no_del.bed/.bim/.fam`（或 VCF）
- 表型：同页 `phenos.csv`（约 529 份核心种质 × 11 个性状：抽穗期、株高、穗数、有效穗、单产、粒重、颖壳长、粒长、粒宽、粒厚）
- 品种信息：`ricevarmap_cultivars_information.csv`（4,726 份，含亚群/地理位置）

**预处理**（W6 完成，脚本保存到 `data_pipeline/`）：
1. 用 `pandas-plink` 或 PLINK 命令行读取，得到样本 × SNP 矩阵
2. 质控：MAF > 0.05、缺失率 < 0.1、LD 剪枝（`--indep-pairwise 50 5 0.2`）→ 目标 5–10 万 SNP
3. 表型与基因型按品种 ID 对齐（同一套 C/W 编号，无需跨库匹配）
4. 保存为 `.npz`（X 矩阵 + y 矩阵 + 样本 ID），后续所有实验直接读取
5. 固定 5 折 × 10 seeds 的划分文件并保存，保证所有模型用同一套划分

**输入表示（为创新做准备）**：
- 方案 a：逐 SNP 向量（0/1/2），MLP/CNN 直接吃
- 方案 b：按 1 kb 窗口 / LD block 聚合 → token 序列，供 Transformer 使用
- 方案 c：one-hot（3 通道）

---

## 5. 第 4 个月创新方案

### 主案：无标签基因型自监督预训练 + 小样本微调

**问题动机**：作物 GS 的痛点是"有基因型的材料多、有表型的少"——华农数据正好是 4,726 份基因型 vs 529 份表型。我们验证：在小样本条件下，用无标签基因型预训练能否显著提升预测精度。

**设定**：
- 预训练语料：4,726 份基因型（无表型），掩码 SNP/block 建模
- 下游任务：529 份 × 11 性状回归（少样本 10% / 25% / 50% / 100%）
- 模型：轻量 1D-CNN 或小 Transformer（参数量 0.1M–5M），单卡/CPU 可训
- 基线：Ridge、GBLUP（BGLR）、LightGBM、同架构随机初始化

**实验矩阵**（W13–W15 完成）：

| 编号 | 实验 | 目的 |
|---|---|---|
| E1 | 预训练 vs 随机初始化（4 种标注比例） | 核心结论 |
| E2 | 掩码比例 15% / 30% / 50% | 超参规律 |
| E3 | token 粒度：单 SNP / 1 kb block / LD block | 表示学习规律 |
| E4 | 预训练数据量 1K/2K/4K 份 | scaling 行为 |
| E5 | CV1 vs CV2 | 泛化性/防泄漏 |
| E6 | 11 性状逐个性状 + 平均 | 稳健性 |
| E7 | EasyGeSe 换 2 个物种 | 跨物种泛化（加分） |

**成功标准**（写进论文的"结论"）：
- 少样本（10%–25%）下，预训练相对随机初始化 r 提升 ≥ 3% → 主结论成立
- 全量数据下不明显下降 → 说明预训练不损害性能
- 给出"什么 token 粒度/掩码比例最好"的经验规律

**论文故事线**：小样本是 GS 落地的核心瓶颈 → 自监督预训练把无表型基因型变成"免费"的训练信号 → 实验证明有效 → 给出可复用的预训练配置建议。

### Fallback 1（W10 发现预训练无收益）
转向**多任务学习 + 可解释性**：11 个性状联合训练，研究性状相关性对多任务增益的影响；配 SHAP/梯度归因，分析哪些 SNP 区域被模型使用。

### Fallback 2（W12 发现进度落后）
降级为**系统评测型论文**：统一评测 8–10 种模型（线性/树/CNN/Transformer）在华农数据上的表现，分析"什么条件下深度模型优于线性模型"，加上可解释性分析。工作量可控，结论依然成立。

---

## 6. 里程碑与导师沟通节点

| 时间 | 里程碑 | 给导师看什么 |
|---|---|---|
| W4（11/22） | M1 基础闭环 | 一张 4 模型对比表 + 1 页总结（可用于开题） |
| W8（12/20） | M2 数据管线 + 基线 | 华农数据上的基线结果表 + 代码仓库 |
| W12（01/17） | M3 方法雏形 | 中期报告 + 预训练初步结果 |
| W16（02/14） | M4 论文 | 初稿 + 代码 + 答辩 PPT |

**汇报模板**：进展（表格/图）→ 遇到的问题 → 下周计划 → 需要老师帮忙的地方。

---

## 7. 风险预案（进度落后时按顺序砍）

1. **砍数据规模**：不用 4,726 全量预训练，用 1,000–2,000 份即可（E4 实验本来也要做）
2. **砍性状数量**：11 个性状只做 5 个（选遗传力高的：粒长、粒宽、粒重、株高、抽穗期）
3. **砍跨物种实验**：EasyGeSe 泛化（E7）可删
4. **砍可解释性**：只留一张 attention/Saliency 图，不做完整 SHAP
5. **最后底线**：Fallback 2 的系统评测型论文，2 周可完成

**绝不能砍**：基线复现（没有对比就没有结论）、多 seed 重复（否则结果不可信）、数据划分防泄漏。

---

## 8. 第一周每日任务（10/26–11/01）

| 天 | 任务（约 2 小时/天） |
|---|---|
| 周一 | 建 GitHub 私有仓库 `gs-ai-thesis`；装 Python 环境（numpy/pandas/scikit-learn/lightgbm/matplotlib）；写 README |
| 周二 | pandas/numpy 快速复习：读一个 CSV、做缺失值统计、画直方图 |
| 周三 | sklearn：用随机生成数据跑通 train_test_split + 线性回归 + 计算 Pearson r |
| 周四 | 学 K 折交叉验证概念，手写一个 5 折 CV 循环，输出每折 r 和均值 |
| 周五 | 打开 EasyGeSe GitHub README，按说明安装，加载 rice 数据，打印形状 |
| 周六 | 在 EasyGeSe 上跑 Ridge + LightGBM，得到第一张结果表，commit 代码 |
| 周日 | 写 1 页周记：学到什么、卡在哪、下周计划；发给导师（可选） |

---

## 9. 不要做的事

- ❌ 不要先读生物/农学教材或遗传学课本
- ❌ 不要一开始下载 3K/19K 的几十 GB 原始数据（4 个月内不需要）
- ❌ 不要追求 SOTA 或复现所有论文
- ❌ 不要在没有基线的情况下直接训练自己的模型
- ❌ 不要只用一个随机种子/一种数据划分
- ❌ 不要到第 4 个月才第一次跑主数据（那时已经来不及调 bug）
- ❌ 不要把时间花在调参刷 0.1% 的精度上，先把实验框架和故事线搭好
