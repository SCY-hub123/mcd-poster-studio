# MCP_INTEGRATION.md — 麦当劳 MCP 接入说明

## 一、使用的 MCP Server

| 项目 | 值 |
|------|-----|
| Server 名称 | mcd-mcp（麦当劳中国官方 MCP Server） |
| 地址 | `https://mcp.mcd.cn` |
| 协议 | Model Context Protocol，Streamable HTTP，版本 2025-03-26 |
| 鉴权 | Header `Authorization: Bearer <MCP_TOKEN>`，Token 于 [open.mcd.cn](https://open.mcd.cn/mcp) 免费激活 |

## 二、实际使用的 Tool 清单

| Tool | 在本 Skill 中的作用 | 必要性 |
|------|-------------------|--------|
| `campaign-calendar` | 拉取当月营销活动日历（进行中/未来），是海报选题的**主数据源** | ✅ 核心 |
| `available-coupons` | 获取麦麦省可领优惠券列表，提取价格锚点（如 9.9 元两件套） | ✅ 核心 |
| `now-time-info` | 获取服务器时间，计算活动倒计时（「仅剩 X 天」） | ✅ 核心 |
| `list-nutrition-foods` | 获取餐品热量/营养数据，用于健康向海报的卖点背书 | ⭕ 增强 |
| `query-my-account` | 查询积分账户，支撑「APP 积分专享」类活动的玩法说明 | ⭕ 增强 |
| `auto-bind-coupons` | 引导用户领券后的路径闭环（评论区置顶「已帮你领好券」） | ⭕ 增强 |

## 三、调用流程

```
initialize (protocolVersion 2025-03-26)
  → notifications/initialized
    → tools/call: now-time-info        （确定当前时间锚点）
    → tools/call: campaign-calendar    （活动池）
    → tools/call: available-coupons    （价格锚点池）
    → [可选] list-nutrition-foods / query-my-account
      → 卖点提炼 → 文案包 + AI绘图Prompt + 排版说明 + 发布建议
```

关键实现细节（`scripts/mcd_client.py`）：

1. 响应格式兼容：Server 可能返回 SSE（`data:` 行）或纯 JSON，客户端统一做了 `data:` 行提取；
2. `notifications/initialized` 的响应为空 body（HTTP 202），客户端对通知类请求不做 JSON 解析；
3. 所有工具结果为 `content[].text` 聚合文本，交给 LLM 提炼卖点。

## 四、业务价值

1. **对创作者**：把「刷活动 → 想卖点 → 写文案 → 找图 → 排版」的 1-2 小时流程压缩到 1 分钟，且不漏活动节点；
2. **对麦当劳**：让活动日历和优惠券获得更多 UGC 二次传播位，券的领取路径（auto-bind-coupons）被自然带进评论区，形成「内容 → 领券 → 核销」闭环；
3. **差异化**：市面同类 Skill 都在解决「怎么吃到」（点餐/比价/营养），本 Skill 解决「怎么传播」，服务内容生态而非单次交易。
