# Change: 新增基金持仓设置与当日收益显示

## Why

用户需要追踪基金投资的实际收益情况。目前应用只展示基金的估值涨跌幅，但无法直观地看到持有基金的实际盈亏金额。通过为每只基金设置持仓金额，用户可以清晰地了解当日的收益情况和总资产变化。

## What Changes

- 新增基金持仓金额设置功能，用户可为每只基金设置持有的金额
- 新增当日收益计算与显示（基于持仓金额和涨跌幅）
- 新增汇总信息显示区域，展示总资产和当日总收益
- 扩展 localStorage 数据结构以存储持仓信息

## Impact

- Affected specs: `fund-position`（新建）
- Affected code:
  - `app/page.jsx` - 新增持仓设置 UI、收益显示、汇总信息组件
  - `app/globals.css` - 新增相关样式
- Affected storage:
  - `localStorage.positions` - 新增持仓金额存储（格式：`{ [fundCode]: amount }`）
