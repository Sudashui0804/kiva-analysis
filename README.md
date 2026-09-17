# Kiva 小额信贷数据分析项目

基于 Kiva 平台 **66.7 万条**贷款记录，从**国家、行业、时间、性别**四个维度进行探索性数据分析（EDA），提炼业务洞察并输出可视化报告。

---

## 项目结构

```text
kiva-analysis/
├── data/
│   └── kiva_loans.csv              # 主数据（未上传，需自行下载）
├── notebooks/
│   └── kiva_analysis.ipynb         # 分析主代码
├── figures/                        # 可视化图表
│   ├── 01_country-sector_top10.png
│   ├── 02_different-sector_amount.png
│   ├── 03_repayment_interval.png
│   ├── 04_quarterly_loans.png
│   ├── 05_top5_countries_quarterly_loans.png
│   └── 06_top5_sectors_quarterly_loans.png
├── report_summary.md               # 报告摘要
└── README.md
数据来源
数据来自 Kaggle 公开数据集：Kiva Crowdfunding Loans

主表 kiva_loans.csv 包含 671,205 条贷款记录、20 个字段，清洗后保留 666,984 条（保留率 99.4%）。

由于数据文件较大，本仓库未直接上传原数据文件。
下载后放到 data/ 目录即可运行 notebook。

分析框架
text
单变量分析  →   双变量分析   →   时间序列分析
   │              │                 │
国家 / 行业    国家 × 行业       季度趋势
性别维度       性别 × 行业       性别 × 时间
               性别 × 国家       国家 × 季度
                                行业 × 季度
关键发现
维度	结论
国家	Top 5 国家占 50.44%，菲律宾单国占 24.04%
行业	Top 5 行业占 77.91%，农业单行业占 26.87%
时间	2014Q2 → 2017Q2 季度贷款量从 41,249 笔增至 57,398 笔，增长 1.4 倍
性别	单人借款中女性占 76.0%，男性占 24.0%，平台以女性创业者为主
波动	季度贷款量波动系数 10.4%，整体呈平稳上升趋势
一句话结论：Kiva 是一家"国家集中 + 行业集中 + 女性主导 + 持续增长"的普惠金融平台，核心业务为农业 + 发展中国家女性小微创业。

技术栈
Python 3

pandas：数据清洗、聚合、透视

matplotlib / seaborn：可视化

Jupyter Notebook：分析载体

关键代码片段
1. 交叉分组 + 宽表构建
python
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
top5_share = data['country'].value_counts().head(5).sum() / len(data)
print(f"Top 5 国家占据份额：{top5_share:.2%}")
如何运行
bash
# 1. 克隆仓库
git clone https://github.com/Sudashui0804/kiva-analysis
cd kiva-analysis

# 2. 安装依赖
pip install pandas matplotlib seaborn jupyter

# 3. 下载数据到 data/ 目录
# 4. 打开 notebook
jupyter notebook notebooks/kiva_analysis.ipynb
我的收获
掌握从数据清洗 → 单变量 → 双变量 → 时间序列的完整 EDA 流程

熟练使用 groupby / unstack / crosstab 处理多维交叉数据

学会时间序列分析中的首尾残缺季度处理（q.iloc[1:-1]）

建立多维度交叉分析思路（国家×行业、性别×时间等）

养成"每写一行复合操作就 print 中间结果"的调试习惯

联系方式
GitHub: @Sudashui0804

Email: suyuhang0804@163.com