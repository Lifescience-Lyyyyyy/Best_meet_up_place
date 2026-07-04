# Best_meet_up_place
```markdown
# 🚄 网友面基·天命之城计算器 (Meetup City Optimizer)

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![Vue.js](https://img.shields.io/badge/Vue-3.0-4FC08D.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

每次群友说要线下聚会，大家都在为“去哪个城市”争论不休？
本项目是一个基于**真实交通拓扑数据**与**多目标优化算法**的选址决策工具。它通过调用地图 API 自动获取群友所在地到全国各大枢纽城市的真实交通耗时，并通过算法权衡“效率”、“公平”与“城市可玩性”，一键计算出最适合大家面基的“天命之城”。

## ✨ 核心特性

* 🗺️ **真实交通拓扑**：接入百度地图 Web 服务 API，自动计算跨城公共交通（高铁/飞机）端到端最短耗时，并自带驾车路线（Driving API）兜底机制，杜绝断头路。
* ⚖️ **多目标优化算法**：不再单纯追求“总距离最短”，而是引入方差计算，兼顾偏远地区群友的出行公平性。
* 📊 **高级可视化报告**：内置 Python 脚本，自动生成帕累托前沿散点图与 Top 3 候选城市能力雷达图。
* 🎮 **沉浸式交互看板**：提供一个纯前端、免部署的 HTML/Vue 小程序，群友可直接上传 CSV 数据，自行拖动滑块体验“动态洗牌”的乐趣。

## 🧮 算法原理解析

本项目的核心是一个多维度打分模型，最终得分越低，代表该城市的综合性价比越高。核心公式如下：

$$Score(c) = \alpha \cdot \text{Norm}(Cost_{\text{total}}) + \beta \cdot \text{Norm}(Cost_{\text{var}}) - \gamma \cdot W(c)$$

* **$\alpha$ (效率权重)**：衡量所有人出行时间的总和。权重越高，越倾向于总耗时最短的城市。
* **$\beta$ (公平权重)**：衡量各成员出行时间的方差。权重越高，越倾向于大家路程相近的城市，不让任何人吃亏。
* **$\gamma$ (繁华权重)**：衡量候选城市的级别 $W(c)$（一线城市/新一线枢纽等）。权重越高，越倾向于吃喝玩乐体验好的大城市。

## 🚀 快速开始

### 1. 准备工作

确保你的电脑已安装 Python 3.8+，并安装必要的依赖库：

```bash
pip install requests numpy pandas matplotlib

```

获取百度地图 API 密钥（AK）：

1. 前往 [百度地图开放平台](https://lbsyun.baidu.com/) 注册账号。
2. 创建一个**服务端**应用，获取对应的 `AK`。
3. **重要**：在应用设置的 IP 白名单中填入 `0.0.0.0/0`（允许本地任何网络调用）。

### 2. 数据抓取与矩阵生成

打开 `fetch_matrix.py` (你的 Python 爬虫文件)，将你的 `AK` 填入配置区，并根据你的群友分布修改 `user_cities_cn` 列表。

运行脚本：

```bash
python fetch_matrix.py

```

*运行结束后，项目目录下会生成一个 `travel_time_matrix_en.csv` 文件。*

### 3. 可视化与决策

**方案 A：生成专业分析图表 (用于汇报/发群装逼)**
运行绘图脚本，它将自动读取刚才生成的 CSV 文件，并输出一张极具专业感的分析图 `meetup_final_decision.png`。

```bash
python visualize.py

```

**方案 B：群友互动小程序 (用于民主投票)**

1. 在浏览器中双击打开 `meetup.html`。
2. 点击页面上的“选择文件”，上传 `travel_time_matrix_en.csv`。
3. 拖动页面上的 $\alpha, \beta, \gamma$ 滑块，实时查看动态排名！可以将这个 HTML 文件直接发给群友玩耍。

## 📁 文件结构建议

```text
.
├── README.md                           # 项目说明文档
├── fetch_matrix.py                     # 百度地图 API 数据抓取与打分脚本
├── visualize.py                        # 静态高级图表生成脚本 (雷达图/散点图)
├── meetup.html                         # Vue3 + Tailwind 交互式排名小程序
├── travel_time_matrix_en.csv           # (自动生成) 交通耗时矩阵缓存数据
└── meetup_final_decision.png           # (自动生成) 数据可视化结果图

```

## 📝 许可证

MIT License. 自由使用，欢迎 Fork 并邀请你的网友们一起测试！

```

```
