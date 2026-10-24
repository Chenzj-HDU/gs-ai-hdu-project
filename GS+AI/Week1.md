---
title: Week1
tags:
  - 毕业论文
  - 全基因组选择
  - 学习计划
created: 2026-10-24
week: 1
dates: 2026-10-26 ~ 2026-11-01
status: in-progress
---

# Week 1：Python 数据栈 + ML 复习（10/26–11/01）

> 本周只有一个目标：**在 EasyGeSe 的水稻数据上，用一条命令跑出 Ridge + 5 折 CV 的 Pearson r**。
> 对应主笔记：[[GS+AI Learning Diary]] 第 3 节（知识清单 W1 条目）、第 4 节（数据管线）、第 8 节（每日任务）。
> 铁律：周六结束必须有"能运行的东西 + 一张表"，不能只有"看完了"。

---

## 0. 本周产出（验收清单）

- [ ] GitHub 私有仓库 `gs-ai-thesis` 建好，含 `README.md` + `data_pipeline/` + `experiments/`
- [x] 环境 `gs` 装好：numpy / pandas / matplotlib / scikit-learn / lightgbm
- [ ] 一个 `metrics.py`，`pearson_r(y_true, y_pred)` 能跑通
- [ ] 一个 `cv.py`，手写 5 折 CV 循环，输出每折 r + 均值
- [ ] EasyGeSe rice 数据加载成功，打印基因型/表型矩阵形状
- [ ] **一张结果表**：Ridge vs LightGBM 的 5 折 r（M1 里程碑的原料）
- [ ] 1 页周记：学到什么 / 卡在哪 / 下周计划

---

## 1. 七天任务（约 2 小时/天）

### 周一：搭环境 + 建仓库
- [ ] 建私有仓库 `gs-ai-thesis`（GitHub 网页或 `gh repo create`），本地 clone
- [ ] 建目录：`data_pipeline/`、`experiments/`、`notebooks/`、`README.md`
- [ ] 建 conda 环境：
  ```bash
  conda create -n gs python=3.10 -y
  conda activate gs
  pip install numpy pandas matplotlib scikit-learn lightgbm
  ```
- [ ] README 写清楚：课题名、数据来源、目录说明、如何运行
- 溯源：[conda 入门](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html)

### 周二：numpy / pandas 复习
- [ ] 用 numpy 造 `100×1000` 的随机 0/1/2 矩阵，手写算每个位点的 **MAF**
  - 公式：`maf = min(alt_freq, 1 - alt_freq)`，其中 `alt_freq = mean(X)/2`
- [ ] 用 pandas 读一个表型 CSV，做：缺失值统计、每列描述统计、画直方图
- [ ] 存图到 `experiments/figs/`
- 溯源：[numpy 快速入门](https://numpy.org/doc/stable/user/quickstart.html) · [pandas 10 分钟](https://pandas.pydata.org/docs/user_guide/10min.html)

### 周三：sklearn 跑通线性回归
- [ ] `train_test_split` 划分 + `Ridge` 拟合 + 计算 Pearson r
- [ ] 写 `metrics.py`：
  ```python
  from scipy.stats import pearsonr
  def pearson_r(y_true, y_pred):
      return pearsonr(y_true, y_pred)[0]
  ```
- [ ] 用随机数据验证：预测值≈真实值时 r≈1，随机时 r≈0
- 溯源：[sklearn Ridge](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.Ridge.html)

### 周四：手写 5 折交叉验证
- [ ] 不用 sklearn 的 `cross_val_score`，手写循环：
  ```python
  import numpy as np
  def kfold_cv(X, y, model, k=5, seed=0):
      idx = np.random.RandomState(seed).permutation(len(y))
      folds = np.array_split(idx, k)
      rs = []
      for i in range(k):
          test = folds[i]
          train = np.concatenate([folds[j] for j in range(k) if j != i])
          model.fit(X[train], y[train])
          rs.append(pearson_r(y[test], model.predict(X[test])))
      return np.array(rs), np.mean(rs)
  ```
- [ ] 输出每折 r + 均值 + 标准差
- 溯源：[sklearn 交叉验证](https://scikit-learn.org/stable/modules/cross_validation.html)

### 周五：EasyGeSe 上手
- [ ] clone 仓库（国内建议开 VPN）：
  ```bash
  git clone https://github.com/stevenandrewyates/EasyGeSe
  ```
- [ ] 按仓库 README 下载数据（数据本体在 Zenodo，约 2.6 GB；建议断点续传/代理）
- [ ] 用 `LoadEasyGeSe.py`（Python，另有 R 版 `LoadEasyGeSe.R`）加载 **rice** 数据，打印：
  - 基因型矩阵形状（样本数 × SNP 数）
  - 表型向量形状 / 性状名
- [ ] 记录：这份数据与"华农 RiceVarMap v3"不是同一套——它是练手沙盒，华农数据 W5 才正式用
- 溯源：[EasyGeSe GitHub](https://github.com/stevenandrewyates/EasyGeSe) · [论文](https://pubmed.ncbi.nlm.nih.gov/41136918/)

### 周六：第一张结果表（本周核心）
- [ ] 在 EasyGeSe rice 上跑 **Ridge** 和 **LightGBM**，都用上面手写的 5 折 CV
- [ ] 结果写成表格（Markdown 或 CSV）：

  | 模型 | 性状 | 5 折 r（每折） | 均值 ± std |
  |---|---|---|---|
  | Ridge | ... | ... | ... |
  | LightGBM | ... | ... | ... |

- [ ] commit 代码 + 结果到仓库
- 溯源：[LightGBM 文档](https://lightgbm.readthedocs.io/)

### 周日：周记 + 汇报
- [ ] 写 1 页 `Week1.md` 周记：本周学到什么、卡在哪、下周（W2 PyTorch）计划
- [ ] （可选）发给导师，附那张结果表
- [ ] 用主笔记第 3 节 W1 的勾选项复核：哪些"上手"真的做完了

---

## 2. 常见坑

- **EasyGeSe 下载慢/断**：先 `git clone` 代码（小），数据从 Zenodo 用浏览器或 `wget -c` 断点续传；必要时挂 VPN。
- **标签形状对不上**：基因型行是样本、列是 SNP；表型要按**同一批样本 ID** 顺序对齐，别直接按行号拼。
- **r 算成 NaN**：预测值是常数或全 NaN 时 Pearson 无定义，先查模型是否没收敛/输入是否全 0。
- **CV 泄露**：本周 EasyGeSe 小数据可先随机 5 折；但心里要记住：**华农主数据必须按亚群分组划分（CV2）**，见主笔记 3.4。

---

## 3. 本周溯源清单（收藏）

- EasyGeSe：[GitHub](https://github.com/stevenandrewyates/EasyGeSe) · [BMC Genomics 2025](https://pubmed.ncbi.nlm.nih.gov/41136918/)
- 华农主数据（W5 用）：[RiceVarMap v3 下载](https://ricevarmap.ncpgr.cn/v3/download/) · [教程](https://ricevarmap.ncpgr.cn/v3/tutorial/)
- pandas-plink（W5 读 PLINK 用）：[GitHub](https://github.com/limix/pandas-plink)
- PLINK（W6 质控用）：[官网](https://www.cog-genomics.org/plink/)
