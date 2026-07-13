# Energy News Search — 能源新闻垂直搜索 Skill

> 受 [anysearch](https://github.com/anysearch-ai/anysearch-skill) 垂直搜索架构启发，专为能源领域打造的 AI Agent 搜索 Skill。

## 设计思想

```
用户搜 "比亚迪储能大单"
        │
        ▼
  识别 → storage 子域
        │
        ▼
  路由到储能专属源：OFweek储能网 / ES News / 集邦储能 / 北极星储能 ...
        │
        ▼
  返回相关新闻（零噪音）
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

**60+ 高质量新闻源**：国家能源局、中电联、中国能源新闻网、北极星系列、OFweek系列、集邦新能源、索比光伏、ES News、PV Magazine、Electrek、Reuters Energy、IEA、Utility Dive...

## 安装

```bash
# 克隆到 OpenCode skills 目录
git clone https://github.com/YOUR_USERNAME/energy-news-search.git ~/.config/opencode/skills/energy-news-search

# 也支持 Claude Code
# git clone ... ~/.claude/skills/energy-news-search
```

## 使用方法

Skill 会在用户查询能源新闻时自动激活。也可以手动触发：

```
"搜一下压缩空气储能的最新进展"
"光伏政策有什么变化？"
"比亚迪储能最近拿了哪些大单？"
"欧洲氢能市场近况"
```

Agent 会根据 `SKILL.md` 中的子域映射，自动路由到对应的专属新闻源，用 `site:` 过滤精准命中。

## 扩展

添加新源只需两步：

1. 在 `references/sources.md` 对应子域下加一行
2. 更新该子域的 `site:` 过滤串

```markdown
| 新源名称 | new-source.com | 行业垂直 | 中文 | 说明 |
```

## 文件结构

```
energy-news-search/
├── README.md           # 本文件
├── SKILL.md            # Skill 主文件（触发规则、使用方法）
├── references/
│   └── sources.md      # 源清单（等价于 anysearch 的 get_sub_domains）
└── .gitignore
```

## 许可

MIT
