# Project Context

## Purpose

**基基宝 (Real-time Fund Valuation)** 是一个纯前端基金估值与重仓股实时追踪工具。

**核心目标：**
- 通过输入基金编号，实时获取并展示基金的单位净值、估值净值及实时涨跌幅
- 自动获取基金前 10 大重仓股票，并实时追踪重仓股的盘中涨跌情况
- 提供简洁优雅的用户界面，支持自选、分组、多种视图等个性化管理功能
- 无需后端服务器，可在 GitHub Pages 等静态环境直接部署

**项目地址：** https://hzm0321.github.io/real-time-fund/

## Tech Stack

**核心框架：**
- **Next.js** 14.2.5 (App Router)
- **React** 18.3.1
- **React DOM** 18.3.1

**UI 与动画：**
- **Framer Motion** 12.29.2 - 动画与交互效果
- **原生 CSS** - 玻璃拟态设计 (Glassmorphism)

**数据获取方案（JSONP）：**
- 基金估值：天天基金 (`fundgz.1234567.com.cn`)
- 重仓数据：东方财富 (`fundf10.eastmoney.com`)
- 股票行情：腾讯财经 (`qt.gtimg.cn`)
- 基金搜索：东方财富 (`fundsuggest.eastmoney.com`)

**部署方案：**
- GitHub Actions 自动化构建
- GitHub Pages 静态站点托管
- Vercel 备选部署

**分析工具：**
- Google Analytics (GA4)

## Project Conventions

### Code Style

**文件与命名：**
- 使用 `.jsx` 扩展名（非 TypeScript）
- 组件文件名使用 PascalCase：`Announcement.jsx`
- 页面文件使用小写：`page.jsx`、`layout.jsx`
- CSS 文件使用小写：`globals.css`

**组件风格：**
- 所有组件使用**函数式组件 + Hooks**
- 使用 `'use client'` 指令标记客户端组件
- 内联 SVG 图标组件（如 `PlusIcon`、`TrashIcon`）
- 组件内部辅助函数放在组件定义之前

**CSS 规范：**
- 使用 CSS 变量管理主题色彩（定义在 `:root`）
- 主要颜色变量：
  ```css
  --bg: #0f172a;      /* 背景色 */
  --card: #111827;    /* 卡片背景 */
  --text: #e5e7eb;    /* 文字颜色 */
  --muted: #9ca3af;   /* 次要文字 */
  --primary: #22d3ee; /* 主色调（青色） */
  --accent: #60a5fa;  /* 强调色（蓝色） */
  --success: #34d399; /* 成功（绿色） */
  --danger: #f87171;  /* 危险/下跌（红色） */
  --border: #1f2937;  /* 边框颜色 */
  ```
- 玻璃拟态效果使用 `.glass` 类
- 响应式断点：640px（移动端）、768px（平板）、1024px（桌面）

**命名约定：**
- 状态变量使用 `camelCase`：`searchTerm`、`isSearching`
- 事件处理函数使用 `handle` 前缀：`handleAddGroup`、`handleMouseDown`
- Ref 变量使用 `Ref` 后缀：`timerRef`、`tabsRef`

### Architecture Patterns

**应用架构：**
- 单页面应用（SPA）
- 所有核心逻辑集中在 `app/page.jsx`
- 可复用组件提取到 `app/components/` 目录
- 全局样式在 `app/globals.css`

**状态管理：**
- 使用 React `useState` 管理本地状态
- 使用 `useRef` 管理定时器和 DOM 引用
- 使用 `useEffect` 处理副作用和生命周期
- **无外部状态管理库**

**数据持久化：**
- 使用 `localStorage` 存储用户配置：
  - `funds` - 已添加的基金列表
  - `favorites` - 自选基金代码
  - `groups` - 自定义分组
  - `collapsedCodes` - 收起状态
  - `refreshMs` - 刷新频率
  - `viewMode` - 视图模式（card/list）

**数据获取模式：**
- 使用 JSONP 解决跨域问题
- 通过动态创建 `<script>` 标签加载数据
- 全局回调函数临时挂载在 `window` 对象

**UI 模式：**
- 模态框（Modal）组件用于设置、反馈、分组管理等
- Tab 切换用于分组筛选
- 拖拽排序使用 Framer Motion 的 `Reorder` 组件
- 动画过渡使用 `AnimatePresence` 管理进出动画

### Testing Strategy

**当前状态：**
- 项目暂无自动化测试
- 依赖手动测试验证功能

**建议方向：**
- 可考虑添加 Jest + React Testing Library
- 关键逻辑函数可添加单元测试
- E2E 测试可使用 Playwright 或 Cypress

### Git Workflow

**分支策略：**
- `main` 分支为生产分支
- 推送到 `main` 自动触发 GitHub Actions 部署
- 功能开发建议使用 `feature/*` 分支

**提交规范：**
- 中文提交信息
- 描述清晰的改动内容
- 提交信息格式参考：`功能: 添加xxx功能` 或 `修复: 解决xxx问题`

**CI/CD：**
- GitHub Actions 自动构建 Next.js 静态站点
- 构建产物输出到 `out/` 目录
- 自动部署到 GitHub Pages

## Domain Context

**基金相关术语：**
- **基金代码**：6 位数字，如 `110022`
- **单位净值 (dwjz)**：基金的实际净值，每日收盘后更新
- **估值净值 (gsz)**：基于重仓股实时行情估算的净值
- **估值涨跌幅 (gszzl)**：当日估值相对前一日净值的变化百分比
- **重仓股**：基金持有的前 10 大股票及其占比

**涨跌显示规则：**
- 中国市场习惯：**红涨绿跌**
- 上涨使用 `--danger`（红色）
- 下跌使用 `--success`（绿色）

**数据更新频率：**
- 默认 30 秒自动刷新
- 用户可自定义 5-300 秒
- 非交易时段数据可能无更新

## Important Constraints

**技术约束：**
- 纯前端应用，不可添加后端服务
- 必须支持 GitHub Pages 静态部署
- 跨域数据必须使用 JSONP 方案
- 不使用 TypeScript（保持 JSX）

**数据约束：**
- 数据来源为公开接口，可能存在延迟
- 估算数据与真实结算数据会有约 1% 误差
- 非股票型基金误差较大
- 数据仅供个人学习及参考，不作为投资建议

**性能约束：**
- 多只基金时需串行获取数据（避免接口限流）
- 本地存储有 5MB 限制
- 动画效果需考虑低端设备性能

**用户体验约束：**
- 保持简洁优雅的 UI 风格
- 移动端需完美适配
- 支持 iOS Safari 的输入框放大问题

## External Dependencies

**数据接口（无需认证）：**

| 接口 | 用途 | 域名 |
|------|------|------|
| 天天基金估值 | 获取基金实时估值 | `fundgz.1234567.com.cn` |
| 东方财富重仓 | 获取基金重仓股列表 | `fundf10.eastmoney.com` |
| 腾讯财经行情 | 获取股票实时涨跌 | `qt.gtimg.cn` |
| 东方财富搜索 | 基金名称/代码搜索 | `fundsuggest.eastmoney.com` |

**第三方服务：**

| 服务 | 用途 | 说明 |
|------|------|------|
| Web3Forms | 用户反馈表单提交 | 免费表单服务 |
| Google Analytics | 用户行为分析 | GA4 |
| GitHub Pages | 静态站点托管 | 主要部署平台 |
| Vercel | 备选部署平台 | 可选 |

**开源协议：**
- 项目采用 **AGPL-3.0** 开源协议
- 基于本项目的网络服务需向用户提供源代码
