# 计划看板 · 目标拆解工作台

> 单文件 HTML 应用，月目标 → 周任务 → 每日待办三级拆解，带完成率仪表盘和逾期自动顺延。

![status](https://img.shields.io/badge/status-active-success)
![tech](https://img.shields.io/badge/stack-vanilla_HTML%2FCSS%2FJS-blue)
![deploy](https://img.shields.io/badge/deploy-GitHub_Pages-brightgreen)
![size](https://img.shields.io/badge/size-~90KB-lightgrey)

🔗 **线上预览**：<https://hangchi.github.io/plan-dashboard/>

## ✨ 特性

- **三级目标拆解**：月目标 → 周任务（按真实日历周，支持跨月延伸） → 每日待办
- **三档优先级**：P0（紧急）/ P1（重要）/ P2（普通），列表自动按优先级排序
- **逾期自动顺延**：未完成的待办每天自动滚到今天，并标注「顺延自 X月X日」
- **完成率仪表盘**：SVG 环形进度条 + 周完成趋势柱状图 + 优先级分布
- **月目标拖拽排序**：六点手柄拖动调整顺序，自动持久化
- **删除二次确认**：弹窗确认，避免误删
- **响应式布局**：≥1280px 三栏（左侧栏 + 主内容 + 右侧栏），≥1024px 双栏，移动端单栏 + 底部 Tab
- **零依赖**：纯 HTML/CSS/JS，单文件 ≈ 90KB，离线可用

## 📸 界面预览

```
┌──────────────────────────────────────────────────────────┐
│ Header：标题 + 月份 + 日期                                 │
├──────────┬───────────────────────────────┬──────────────┤
│ 左栏      │ 「今天要处理」折叠区           │ 右侧栏        │
│ 迷你日历  │ ─ 逾期顺延                    │ 今日进度环    │
│ 每日激励  │ ─ 今日待办（按优先级排序）      │ 连续天数      │
│ 本周任务  │                               │ 本月速览      │
│ 月末倒计  │ Tab：月目标 / 每日待办 / 进度看板 │ 最近完成      │
│          │                                │ 快捷操作      │
└──────────┴───────────────────────────────┴──────────────┘
```

## 🚀 快速开始

### 方式一：本地直接打开

```bash
# 直接用浏览器打开
open plan-dashboard.html      # macOS
xdg-open plan-dashboard.html  # Linux
start plan-dashboard.html     # Windows
```

无需任何构建步骤、无需服务器。

### 方式二：部署到 GitHub Pages

1. Fork 本仓库（或创建新仓库）
2. 将 `plan-dashboard.html` 重命名为 `index.html` 并推送到 `master` 分支
3. 仓库 Settings → Pages → Source 选择 `master` 分支根目录
4. 等待 1-2 分钟，通过 `https://<username>.github.io/<repo>/` 访问

### 方式三：本地预览服务器（可选）

```bash
# Python 3
python -m http.server 8000

# Node.js
npx serve .
```

访问 `http://localhost:8000` 即可。

## 🛠️ 技术栈

| 类别 | 选型 |
|------|------|
| 结构 | HTML5 |
| 样式 | 原生 CSS（CSS Variables + Grid + Flexbox） |
| 交互 | 原生 JavaScript（ES6+，无框架） |
| 图表 | 手写 SVG（仪表盘 / 柱状图 / 进度环） |
| 持久化 | localStorage（键前缀 `wb_planboard_`） |
| 字体 | 系统字体栈（`-apple-system, BlinkMacSystemFont, ...`） |

**为什么不用框架？**
单文件应用 + 数据全在本地，无需 React/Vue 的组件化与状态管理，原生 JS ≈ 2000 行已足够清晰。零依赖也意味着零供应链风险，离线秒开。

## 📁 项目结构

```
.
├── plan-dashboard.html    # 主应用（HTML + CSS + JS 全部内联）
├── README.md              # 本文件
├── _check.js              # 开发期本地校验脚本（可忽略）
└── check_tmp*.js          # 同上，迭代过程的临时版本（可忽略）
```

> 注：`*_check.js` 和 `check_tmp*.js` 是开发过程留下的代码片段，不参与运行，可直接删除。

## 💾 数据存储

所有数据保存在浏览器 `localStorage`，**不上传任何服务器**，**不联网同步**。

| Key | 内容 |
|-----|------|
| `wb_planboard_goals` | 月目标列表 |
| `wb_planboard_weekly` | 周任务列表 |
| `wb_planboard_todos` | 每日待办列表 |
| `wb_planboard_meta` | 元信息（暂未使用） |

### 备份建议

- **清除浏览器数据前**先导出（在「进度看板」页 / 设置里如果有导出按钮即可）
- 或手动复制：DevTools → Application → Local Storage → 复制 `wb_planboard_*` 三个 key 的 value
- 想换电脑同步数据：在浏览器 A 复制出来 → 在浏览器 B 粘贴进去

## 🗓️ 周计算逻辑

按真实周一到周日划分，跨月延伸：

- 本月第 1 周从「该月第一个周一」开始
- 1 号到第一个周一前的天数 → 归到上月最后一周
- 月底最后一周若是周一开头但不满 7 天 → 延伸到下月周日

例：
| 月份 | 第 1 周 | 最后一周 |
|------|---------|----------|
| 2026-08（8/1 是周六） | 8/3 ~ 8/9（8/1、8/2 归上月） | 8/31 ~ 9/6（跨月延伸） |

## 🎨 主题色

主色调为蓝色 `#4F6BED`，辅助色：

| 用途 | 颜色 |
|------|------|
| 主色（Primary） | `#4F6BED` |
| 紧急 P0 | `#FF5A5F` |
| 重要 P1 | `#FFB144` |
| 普通 P2 | `#4F6BED` |
| 成功 | `#00C896` |
| 警告 | `#FFB144` |
| 危险 | `#FF5A5F` |

通过修改 `:root` 内的 CSS 变量即可整体换色。

## 🌐 浏览器兼容性

| 浏览器 | 支持情况 |
|--------|---------|
| Chrome / Edge | ✅ 完全支持 |
| Safari 14+ | ✅ 完全支持 |
| Firefox 90+ | ✅ 完全支持 |
| 移动浏览器 | ✅ 响应式适配 |

需要：ES6+、CSS Grid、localStorage。所有目标浏览器均已满足。

## ⚠️ 已知限制

- **数据仅在本地浏览器**：清除浏览器缓存 / 换浏览器 / 换电脑 = 数据丢失（除非手动导出迁移）
- **不联网同步**：多设备共用需手动复制 localStorage
- **单文件体积**：所有 CSS/JS 内联，首次加载 ≈ 90KB
- **没有用户系统**：所有人共用同一份本地数据，没有账号/权限

如需云同步，可在「设置 → 数据管理」中扩展后端接入（当前未实现）。

## 📜 License

MIT