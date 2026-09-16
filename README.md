# Kiva 小额信贷数据分析项目

基于 Kiva 平台 42 万条贷款记录，从**国家、行业、时间**三个维度进行探索性数据分析（EDA），提炼业务洞察并输出可视化报告。

---

## 项目结构
kiva-analysis/
├── data/
│ └── kiva_loans.csv # 主数据
├── notebooks/
│ └── kiva_analysis.ipynb # 分析主代码
├── figures/ # 可视化图表
│ ├── 01_country_top10.png
│ ├── 02_sector_top10.png
│ ├── 03_quarterly_trend.png
│ ├── 04_country_quarter.png
│ └── 05_sector_quarter.png
├── report_summary.md # 报告摘要
└── README.md

text

---

## 数据来源

数据来自 Kaggle 公开数据集：[Kiva Crowdfunding Loans](https://www.kaggle.com/datasets/kiva/data-science-for-good-kiva-crowdfunding)

主表 `kiva_loans.csv` 包含 **423,081 条贷款记录、20 个字段**。

> 由于数据文件较大，本仓库未直接上传原数据文件。
> 下载后放到 `data/` 目录即可运行 notebook。

---

## 分析框架
单变量分析 → 双变量分析 → 时间序列分析
│ │ │
国家 / 行业 国家 × 季度 季度趋势
行业 × 季度

text

---

## 关键发现

| 维度 | 结论 |
|------|------|
| **国家** | Top 5 国家占 **47.41%**，菲律宾单国占 **21.58%** |
| **行业** | Top 5 行业占 **76.92%**，农业单行业占 **27.77%** |
| **时间** | 2014Q2 → 2017Q2 季度贷款量增长 **2.3 倍** |

**一句话结论**：Kiva 是一家"国家集中 + 行业集中 + 持续增长"的普惠金融平台，核心业务为农业 + 发展中国家小微创业。

---

## 技术栈

- **Python 3**
- **pandas**：数据清洗、聚合、透视
- **matplotlib / seaborn**：可视化
- **Jupyter Notebook**：分析载体

---

## 关键代码片段

### 1. 交叉分组 + 宽表构建

```python
pivot = (sub.groupby(['quarter', 'country'])
            .size()
            .unstack()
            .fillna(0))
2. 多线趋势图
python
plt.figure(figsize=(14, 6))
pivot.plot(ax=plt.gca(), marker='o')
plt.title("Top 5 国家季度贷款量")
plt.tight_layout()
plt.show()
3. 报告自动摘要
python
top5 = data['country'].value_counts().head(5)
share = top5.sum() / len(data)
print(f"Top 5 国家占据份额：{share:.2%}")
如何运行
bash
# 1. 克隆仓库
git clone <https://github.com/Sudashui0804/kiva-analysis>
cd kiva-analysis

# 2. 安装依赖
pip install pandas matplotlib seaborn jupyter

# 3. 下载数据到 data/ 目录
# 4. 打开 notebook
jupyter notebook notebooks/kiva_analysis.ipynb
我的收获
掌握从数据清洗 → 单变量 → 双变量 → 时间序列的完整 EDA 流程

熟练使用 groupby / unstack / crosstab 处理多维交叉数据

学会了时间序列分析中的首尾残缺季度处理

养成"每写一行复合操作就 print 中间结果"的调试习惯

联系方式
GitHub: @Sudashui0804

Email: suyuhang0804@163.com