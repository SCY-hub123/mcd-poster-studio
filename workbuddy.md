# workbuddy.md — WorkBuddy 开发对话上下文

> 本项目全程在腾讯 WorkBuddy 智能体中完成开发，符合麦当劳程序员创意开发大赛 WorkBuddy 专项奖励的提交要求。
> 记录形式：按开发时间线整理的关键会话节点（含用户原始指令与我方的执行动作）。
> 如需原始流水，可从 WorkBuddy 会话菜单「导出对话（Markdown）」追加至本文件末尾。

---

## 📌 项目信息

| 项 | 内容 |
|---|---|
| 项目名称 | 麦门海报工坊（mcd-poster-studio） |
| 开发平台 | 腾讯 WorkBuddy |
| 仓库 | https://github.com/SCY-hub123/mcd-poster-studio |
| MCP Server | mcd-mcp @ `https://mcp.mcd.cn`（Streamable HTTP） |
| 主用 MCP 工具 | `campaign-calendar`、`available-coupons`、`now-time-info`、`list-nutrition-foods` |

> 📌 按大赛规则（同一账号仅 Star 数最高的项目进榜），本仓库为作者的**同源技术展示项目**；作者主推的参赛项目为「麦麦治愈食堂」：https://github.com/SCY-hub123/mcd-comfort-canteen

---

## 🗓 开发会话记录

### 【10-09】赛事调研与 MCP 接入

**User**：（附麦当劳 Skill 开发比赛流程图）我想要参加这个比赛，我需要准备什么。

**Assistant**：抓取官方仓库 `M-China/mcd-developer-innovation-challenge` 的真实规则，整理参赛准备清单（赛期 10-09~10-25、Star 数排名、前 100 名进榜、必需文件 5 项）；实测 40+ 已参赛项目，识别出「省钱薅券 / 热量营养 / 点餐流程」三大红海方向，并据此建议走**内容生成型**差异化路线。

**User**：（提供麦当劳 MCP Token）

**Assistant**：直连验证 Token 有效，`tools/list` 返回 **35 个工具**（远超官方文档 13 个）；真实调用 5 个核心工具验证连通，并拿到 10 月真实活动素材（9.9 元早餐两件套、GD 联名棒球帽）。

---

### 【10-10】立项与工作流实测

**User**：配置好了，可以验证一下，选主题「麦门海报工坊」。

**Assistant**：搭建本仓库，产出：

- `scripts/mcd_client.py`：MCP 客户端封装（initialize → notifications/initialized → tools/call），处理两个实测坑——响应可能是 SSE（`data:` 行）需兼容；`notifications/initialized` 返回空 body，不可做 JSON 解析。
- `SKILL.md`：定义一句话触发的工作流与「海报物料包」标准输出格式（文案 + AI 绘图 Prompt + 排版说明 + 发布建议），内置麦门梗语料与合规慎用原则。
- `references/tools-cheatsheet.md`：35 个工具按场景分类速查。

**User**：给最近的麦当劳活动做个海报。

**Assistant**：按工作流 A 跑通，用 `campaign-calendar` 抓到 10-08~10-21 的 9.9 元早餐两件套活动，输出完整物料包（主标题「早八人的命，9.9 就能续」、双风格文案、复古美式插画 Prompt 及实图、排版说明、发布时段建议 + 倒计时钩子）。

**User**：差异化补充：给 skill 加一个「GD 联名」专属模板（潮酷黑金风海报）。

**Assistant**：从活动日历识别出联名是**三弹连发**（棒球帽 10-08 → 徽章 10-15 → 丝巾 10-22），产出黑金风联名物料包：主推「+39.9 元集齐三件套 + 三弹时间轴」，并标记 10:40 积分抢先购 vs 10:45 开售的 5 分钟手速局情报。

**User**：把「活动日历前瞻排期」写进 SKILL.md，并整理 10 月剩余追热点日历。

**Assistant**：新增「工作流 B：未来 7 日预热排期」——扫描活动日历的未来 2 周数据，按 S/A/B 三级优先级输出「追热点日历表」，对 S 级事件给出 T-7 / T-1 / T-0 / T+1 倒计时攻略；并把联名多弹合并为「收集线叙事」，形成 4 阶段内容连载（预告型 / 开售型 / 晒单型 / 收尾型）。同步产出 12 个事件的 10 月追热点日历表。

---

### 【10-14】定位调整

**User**：（在「合并 / 二选一主推 / 分两个号」中）选 2。

**Assistant**：本仓库转为技术展示定位，与治愈食堂完成 README 双向互链，将 Star 传播资源集中到主推项目；补齐 `.gitignore` 并扫描确认无 Token 泄露。

### 【10-16】上线与报名

**User**：（提供 GitHub Token）

**Assistant**：代办建仓、上传、发报名 Issue 全流程。仓库 PUBLIC 且必需文件齐全，报名 Issue #779 提交并审核通过。

---

## 🧩 关键技术与产品产出

| 类别 | 产出 |
|------|------|
| 客户端 | `scripts/mcd_client.py`（Streamable HTTP JSON-RPC 封装，SSE 兼容） |
| 工作流 | 工作流 A 单张海报 / 工作流 B 未来 7 日预热排期（S/A/B 三级 + 倒计时攻略） |
| 情报能力 | 自动标记精确开售时间、5 分钟积分先手、多弹连贯性三类关键情报 |
| 数据产出 | `references/追热点日历-2026年10月.md`（12 个事件分级 + 连载线叙事） |
| 视觉 | 复古美式风海报 + 黑金风联名海报各一张，及对应物料包示例 |

## 🔒 Token 安全

仓库内所有配置文件仅使用 `YOUR_MCP_TOKEN` 占位符，`.gitignore` 已屏蔽敏感文件，提交前经扫描确认无真实 Token。

## ✅ 参赛核验清单

- [x] 项目使用腾讯 WorkBuddy 智能体开发
- [x] 真实使用麦当劳 MCP 能力（`campaign-calendar` 等）
- [x] 含 README.md / CONTEST_DECLARATION.md / MCP_INTEGRATION.md / 源代码 / workbuddy.md
- [x] 仓库公开可访问
- [x] 报名 Issue 审核通过（#779）
