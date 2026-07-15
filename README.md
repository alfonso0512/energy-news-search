# Energy News Search — 能源新闻垂直搜索 Skill

> 8 个子域 × 60+ 新闻源。多后端自动适配，零配置开箱即用。

## 设计思想

不绑定任何特定搜索后端。你有 Exa 用 Exa，有 anysearch 用 anysearch，都没有？DuckDuckGo + curl 照样搜。

```
用户 "储能最新新闻"
        │
        ▼
  子域路由 → storage（8 个专属源）
        │
        ▼
  后端自适应：Exa → anysearch → Brave → websearch → DuckDuckGo
        │
        ▼
  site: 过滤 → 精准命中能源新闻源
```

## 8 个子域

| 子域 | 覆盖 | 源数 |
|------|------|:--:|
| `storage` | 储能 / BESS / 液流 / 压缩空气 | 10 |
| `solar` | 光伏 / 组件 / 逆变器 / 硅料 | 9 |
| `wind` | 风电 / 海上风电 / 风机 | 7 |
| `hydrogen` | 氢能 / 绿氢 / 电解槽 / 燃料电池 | 7 |
| `policy` | 政策 / 补贴 / 双碳 / 绿证 / 电力市场 | 11 |
| `market` | 企业 / 订单 / 财报 / 出海 / 融资 | 11 |
| `grid` | 电网 / 特高压 / 配电网 / 虚拟电厂 | 8 |
| `ev_battery` | 电动车 / 动力电池 / 固态电池 / 钠电 | 10 |

## 兼容性

| 后端 | 条件 | 体验 |
|------|------|:--:|
| Exa | 默认安装 | ⭐⭐⭐ |
| Brave Search | 有 MCP / API Key | ⭐⭐ |
| 通用搜索 | 任意 web_search | ⭐⭐ |
| curl 直搜 | 仅 bash 可用 | ⭐ |

> **不需要安装任何额外 Skill。**有 Exa 体验最好，没有也能跑。

## 安装

```bash
git clone https://github.com/alfonso0512/energy-news-search.git ~/.config/opencode/skills/energy-news-search
```

## 使用示例

```
"储能领域有什么大新闻？"       → storage 子域搜索
"光伏和风电政策有什么变化？"    → solar + wind + policy 并行
"比亚迪最近拿了哪些储能大单？"  → market + storage 并行
```

## 📱 关注公众号

扫码关注，获取更多 **AI 使用技巧、科研经验与能源资讯**：

![公众号二维码](assets/qrcode.jpg)

---

## 📚 IMA 知识库

扫码加入能源领域 IMA 知识库，获取：

- 📜 **政策法规与标准** — 最新能源政策、行业标准、双碳法规
- 🏭 **行业应用与案例** — 储能/光伏/风电/氢能落地案例
- 📰 **最新资讯** — 每日行业动态与市场分析

![IMA 知识库](assets/ima-knowledge-base.jpg)

---

## 许可

MIT
