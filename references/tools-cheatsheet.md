# 麦当劳 MCP 工具速查（35 个）

> Server：`https://mcp.mcd.cn` ｜ 鉴权：`Authorization: Bearer <Token>` ｜ 实测于 2026-10-09

## 🕐 基础
| 工具 | 说明 |
|------|------|
| `now-time-info` | 服务器当前时间（日期/星期/时区），用于倒计时计算 |

## 📅 营销活动（海报工坊主数据源）
| 工具 | 说明 |
|------|------|
| `campaign-calendar` | 当月营销活动日历：进行中/往期/未来，含活动图文 |

## 🎫 优惠券
| 工具 | 说明 |
|------|------|
| `available-coupons` | 麦麦省可领券列表（名称/图片/标签） |
| `auto-bind-coupons` | 一键领取全部可领优惠券 |
| `query-my-coupons` | 卡包已领券（分页） |
| `query-store-coupons` | 指定门店+订单类型可用券 |

## 🍔 点餐（到店/外送）
| 工具 | 说明 |
|------|------|
| `query-meals` | 餐品列表（orderType：1=到店自提，2=外送） |
| `query-meal-detail` | 餐品详情（套餐组成/默认选项） |
| `calculate-price` | 算价（含券优惠），下单前必调 |
| `create-order` | 创建订单（返回支付链接） |
| `query-order` / `order-list` / `cancel-order` | 订单详情/历史/取消 |
| `query-survey-coupon` | 按订单号查满意度答卷 |

## 📍 门店与地址
| 工具 | 说明 |
|------|------|
| `query-nearby-stores` | 到店场景可点餐门店（自提/车道） |
| `delivery-query-stores` | 外送场景按地址查可配门店 |
| `delivery-query-addresses` / `delivery-create-address` | 配送地址查/增 |

## 🎁 积分体系（含抽奖/商城）
| 工具 | 说明 |
|------|------|
| `query-my-account` | 积分账户：可用/本月将过期/累计 |
| `mall-points-products` | 积分可兑换餐品券列表 |
| `mall-product-detail` / `mall-create-order` | 兑换商品详情/下单（shopId=2） |
| `mall-order-list` / `mall-order-detail` | 商城订单查/详情 |
| `query-lottery-info` / `draw-lottery` / `query-my-prizes` | 积分抽奖：活动信息/抽奖/奖品 |

## 🎉 派对与体验
| 工具 | 说明 |
|------|------|
| `query-party-city` | 派对活动城市列表 |
| `query-party-store` / `query-party-store-date` / `query-party-store-session` | 门店→日期→场次逐级查询 |
| `party-order-create` | 创建派对/体验营/品鉴会/读书会订单 |

## 🥗 营养与团餐
| 工具 | 说明 |
|------|------|
| `list-nutrition-foods` | 餐品营养：能量/蛋白/脂肪/碳水/钠/钙 |
| `query-promotions` | 企业团餐促销规则（满减/满折） |
| `query-meal-assistance` | 团餐算价辅助（beType=6） |

## ⚠️ 实测注意
1. 响应可能是 SSE（`data:` 行）或纯 JSON，客户端需兼容；
2. `notifications/initialized` 返回空 body，勿做 JSON 解析；
3. 官方文档仅列 13 个工具，实际 35 个——新工具以 `tools/list` 实测为准。
