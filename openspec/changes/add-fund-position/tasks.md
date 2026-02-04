## 1. Data Layer

- [x] 1.1 新增 `positions` 状态 (`useState`) 管理持仓金额数据
- [x] 1.2 实现从 localStorage 加载持仓数据 (`useEffect`)
- [x] 1.3 实现持仓数据保存到 localStorage 的逻辑
- [x] 1.4 在删除基金时同步清理对应的持仓数据

## 2. Position Setting UI

- [x] 2.1 新增持仓设置图标按钮（在基金卡片/行内）
- [x] 2.2 新增持仓金额输入弹窗组件 `PositionModal`
- [x] 2.3 实现金额输入验证（非负数、数字格式）
- [x] 2.4 添加相关 CSS 样式

## 3. Daily Profit Display

- [x] 3.1 实现当日收益计算函数 `calculateDailyProfit(position, changeRate)`
- [x] 3.2 在卡片视图中显示当日收益（金额格式：+¥123.45）
- [x] 3.3 在列表视图中新增收益列
- [x] 3.4 添加收益金额的样式（红涨绿跌）

## 4. Portfolio Summary

- [x] 4.1 新增汇总信息组件 `PortfolioSummary`
- [x] 4.2 计算并显示总资产
- [x] 4.3 计算并显示当日总收益（金额 + 百分比）
- [x] 4.4 添加汇总区域的 CSS 样式
- [x] 4.5 处理无持仓时的空状态显示

## 5. Data Import/Export

- [x] 5.1 在导出配置中包含 `positions` 数据
- [x] 5.2 在导入配置中处理 `positions` 数据合并

## 6. Verification

- [ ] 6.1 验证持仓设置功能正常工作
- [ ] 6.2 验证当日收益计算正确
- [ ] 6.3 验证汇总信息显示正确
- [ ] 6.4 验证数据导入导出包含持仓信息
- [ ] 6.5 验证删除基金时持仓数据正确清理
- [ ] 6.6 验证页面刷新后数据持久化
