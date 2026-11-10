# position-monitor

币安 U 本位（USDⓈ-M / fapi）持仓监控，单文件前端 `index.html`，浏览器直连 API，密钥只存 localStorage。

## 按钮规则（点卡片上的「加仓 / 开仓」）

| 当前状态 | 行为 |
| --- | --- |
| 该方向无持仓 | 按点击方向市价开 100 USDT（按步长折算成币数量） |
| 回报率 > 100% | 按当前持仓的 **38%** 加仓 → 立即挂止损，止损价锁**当前浮盈的 50%**；止损失败则**立即市价平仓**（绝不留裸仓） |
| 回报率 < -100% | 按当前持仓数量**等量**加仓 |
| -100% ~ +100% | 不操作 |

止损细节：`STOP_MARKET + GTE_GTC`，双向持仓用 `positionSide=LONG/SHORT`（不带 reduceOnly），单向持仓用 `positionSide=BOTH + reduceOnly=true`；挂单前先撤同方向旧止损；触发价太近（`-2021/-4004`）自动推远但不越过入场价。

## 改动流程

改 `index.html` → 抽出 `<script>` 用 `node --check` 验语法 → 提交推送 → 等 GitHub Pages 构建后带 `?v=` 抓线上页面确认。
