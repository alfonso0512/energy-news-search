---
name: energy-news-search
description: 能源领域新闻垂直搜索。按子域（储能/光伏/风电/氢能/政策/市场/电网/电动车）路由到专属高质量新闻源，支持批量并行搜索和全文提取。学习自 anysearch 垂直搜索架构。
version: 1.0.0
author: 阿星
---

## 设计思想

本 Skill 复刻 anysearch 的垂直搜索架构：

```
用户搜 "比亚迪储能大单"
        │
        ▼
  energy-news-search 识别 → storage 子域
        │
        ▼
  路由到储能专属源：OFweek储能网 / ES News / 集邦储能 / 北极星储能
        │
        ▼
  Exa site: 过滤 → 结构化结果
        │
        ▼
  可选 webfetch 提取全文
```

**核心差异**：anysearch 后端做路由，我们用 **Exa site: 过滤 + 预定义源表** 实现等价效果。

## 触发条件

当用户明确涉及以下场景时激活：
- "能源新闻"、"储能动态"、"光伏最新"、"风电消息"
- "新能源政策"、"电池行业"、"氢能进展"
- "电力市场"、"电动车产业链"
- 任何包含具体能源子领域的新闻查询

## 子域体系

| 子域 | 关键词 | 专属源（搜索时用 site: 过滤） |
|------|--------|------------------------------|
| `storage` | 储能/电池储能/BESS/液流/压缩空气 | OFweek储能网 `chuneng.ofweek.com`, 集邦储能 `energytrend.cn`, ES News `energy-storage.news`, 北极星储能 `chuneng.bjx.com.cn`, 北极星能源 `energy.bjx.com.cn`, 能源界 `nengyuanjie.net` |
| `solar` | 光伏/太阳能/组件/逆变器/硅料 | 索比光伏 `solarbe.com`, PV Magazine `pv-magazine.com`, 北极星光伏 `guangfu.bjx.com.cn`, 集邦新能源 `energytrend.cn`, 北极星能源 `energy.bjx.com.cn`, 能源界 `nengyuanjie.net` |
| `wind` | 风电/海上风电/风机/叶片 | 北极星风电 `fd.bjx.com.cn`, 能见 `nengapp.com`, Recharge `rechargenews.com`, 北极星能源 `energy.bjx.com.cn`, 能源界 `nengyuanjie.net` |
| `hydrogen` | 氢能/绿氢/电解槽/燃料电池 | 氢能网 `h2.bjx.com.cn`, 新能源技术与装备 `ner.jgvogel.cn`, Hydrogen Insight `hydrogeninsight.com`, 北极星能源 `energy.bjx.com.cn`, 能源界 `nengyuanjie.net` |
| `policy` | 政策/补贴/双碳/绿证/电力市场 | 国家能源局 `nea.gov.cn`, 北极星电力 `bjx.com.cn`, 中国电力新闻网 `cpnn.com.cn`, IEA `iea.org`, 中国能源新闻网 `cnenergynews.cn`, 中电联 `cecaweb.org.cn`, 环球网能源 `energy.huanqiu.com` |
| `market` | 企业/订单/财报/出海/融资/并购 | 集邦新能源 `energytrend.cn`, OFweek锂电 `libattery.ofweek.com`, 能见 `nengapp.com`, Reuters Energy `reuters.com`, 中国能源新闻网 `cnenergynews.cn`, 北极星能源 `energy.bjx.com.cn`, 能源界 `nengyuanjie.net`, 环球网能源 `energy.huanqiu.com` |
| `grid` | 电网/特高压/配电网/虚拟电厂 | 北极星输配电 `shupeidian.bjx.com.cn`, 中国电力新闻网 `cpnn.com.cn`, Utility Dive `utilitydive.com`, 中国能源新闻网 `cnenergynews.cn`, 北极星能源 `energy.bjx.com.cn`, 中电联 `cecaweb.org.cn` |
| `ev_battery` | 电动车/动力电池/固态电池/钠电 | OFweek锂电 `libattery.ofweek.com`, 电池中国 `cbea.com`, Electrek `electrek.co`, 集邦新能源 `energytrend.cn`, 北极星能源 `energy.bjx.com.cn`, 能源界 `nengyuanjie.net`, 中国能源新闻网 `cnenergynews.cn` |

完整源列表见 `references/sources.md`。

## 使用方法

### 搜索

```bash
# 单子域搜索 - 用 Exa site: 过滤 + 时间范围
web_search_exa "比亚迪储能 大单 订单 site:chuneng.ofweek.com OR site:energy-storage.news" --numResults 10

# 多子域并行（推荐：一次摸清全貌）
# 同时搜 storage + market + policy 三个子域
```

### 批量搜索（复刻 anysearch batch_search）

当用户问题涉及多个能源子域时，**并行发起多个 Exa 搜索**，每个带不同 site: 过滤：

```
并行查询1: storage 子域 → site:chuneng.ofweek.com OR site:energy-storage.news
并行查询2: policy 子域  → site:nea.gov.cn OR site:bjx.com.cn
并行查询3: market 子域  → site:energytrend.cn OR site:reuters.com
```

### 全文提取

搜索结果摘要不够时，用 `webfetch` 提取原文：

```
webfetch "https://chuneng.ofweek.com/news/2026-07/ART-xxx.html"
```

## 子域发现（复刻 get_sub_domains）

查询 `references/sources.md` 获取完整子域和源列表。这是一个静态"发现文档"，等价于 anysearch 的动态 `get_sub_domains` API。

## 搜索策略

### 中文优先 → 英文补充

1. **先搜中文源**：覆盖国内政策和产业动态
2. **再搜英文源**：覆盖全球市场和技术前沿
3. **结果合并**：按时间排序

### 时效性控制

- 新闻类：搜索近 7-30 天内容
- 政策类：搜索近 30-90 天内容
- 技术前沿：不受时间限制

### 源优先级

| 优先级 | 场景 |
|--------|------|
| 行业垂直媒体 | 产业新闻、企业动态、技术进展 |
| 官方机构 | 政策法规、统计数据 |
| 综合媒体 | 补充视角、全球视野 |
| 学术/研究 | 技术深度分析 |

## 输出格式

搜索结果以结构化方式呈现：

```
## [子域名] 搜索结果 (N条, Xms)

### 1. [标题]
- 来源: [域名]
- 日期: [YYYY-MM-DD]
- URL: [链接]
- 摘要: [关键信息]

### 2. ...
```

## 示例会话

**用户**: "储能领域最近有什么大新闻？"

**Agent 执行**:
1. 读 `references/sources.md` 确认 storage 子域源
2. 并行搜索中英文储能源
3. 汇总返回

**用户**: "光伏和风电的政策有什么变化？"

**Agent 执行**:
1. 识别涉及 solar + wind + policy 三个子域
2. 并行三个搜索
3. 合并结果

## 扩展指南

添加新子域只需三步：
1. 在 `references/sources.md` 添加子域条目和源列表
2. 在本文档子域体系表中添加对应行
3. 搜索时带上新子域的 site: 过滤即可
