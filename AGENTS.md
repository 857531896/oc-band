# Tlen Vela 快应用 - 项目文档

## 项目概述
- 项目名称：com.opencode.bandcontrol
- 版本：1.2.0
- 目标平台：小米手环 Vela OS
- 主要功能：OpenCode AI 伴聊应用，支持键盘输入、会话切换、字体调节、统计查看

## 功能模块

### 1. 主页面 (index.ux)
- 会话列表显示（卡片式布局，绿色边框）
- 会话切换导航（左/右按钮，长标题截断）
- 消息显示（固定高度 250px，白色文字，顶部对齐）
- 键盘输入（NEORUAA 输入法组件）
- 思考状态检测（`isThinking` 基于 `info.time.completed`）
- 权限请求提示
- 操作栏（发送/退出/统计按钮）

### 2. 设置页面 (settings.ux)
- IP 地址输入（键盘输入，确认按钮关闭键盘）
- 字体大小选择（小/中/大，存储到 storage）
- 连接测试（GET 请求测试）

### 3. 统计页面 (stats.ux)
- Token 使用统计（基于 `/session` 接口）
- 24 小时使用量图表（2 小时一档）
- 模型使用 Top5 排名

### 4. 输入法组件 (InputMethod.ux)
- 支持中文拼音输入
- 无英文/日文候选词
- 基于 NEORUAA 开源组件修改

## 代码修改记录

### 已完成的修改
| 修改内容 | 文件 | 说明 |
|---------|------|------|
| 输入法组件 | `components/InputMethod/InputMethod.ux` | 基于 NEORUAA/Vela_input_method，移除英文候选词 |
| 会话卡片 UI | `pages/index/index.ux` | 绿色边框卡片，白色文字，固定高度消息区 |
| 会话切换 | `pages/index/index.ux` | 左/右导航按钮，`shortTitle` 截断（5/4 字） |
| 思考状态 | `pages/index/index.ux` | `isThinking` 检测，轮询（3s），完成时显示提示 |
| 字体大小 | `pages/index/index.ux` | 内联样式 + 计算属性（`fs`/`fsMsg`/`fsBtn`） |
| 统计页面 | `pages/stats/stats.ux` | `loadSettings` 使用回调，GET 请求测试 |
| 震动通知 | `pages/index/index.ux` | `vibrator.vibrate({ mode: 'short' })` |
| 保持屏幕常亮 | `pages/index/index.ux` | `brightness.setKeepScreenOn({ keepScreenOn: true })` 5秒 |
| 移除 `lines` 属性 | 所有 `.ux` 文件 | Vela 不支持 `<text lines="n">` |
| 移除 `@key-down` | `components/InputMethod/InputMethod.ux` | 避免中文输入时插入英文字符 |

### 字体大小规范
| 场景 | 小 | 中 | 大 |
|------|----|----|----|
| 会话标题 (`fs`) | 28 | 36 | 48 |
| 消息文字 (`fsMsg`) | 30 | 33 | 45 |
| 按钮文字 (`fsBtn`) | 26 | 32 | 40 |

## Vela OS 系统限制

### 不支持的 CSS/HTML 特性
| 特性 | 状态 | 影响 |
|------|------|------|
| `<text lines="n">` | ❌ 不支持 | 无法限制文字行数，会导致崩溃 |
| `white-space: nowrap` | ❌ 不支持 | 无法强制单行显示 |
| `text-overflow: ellipsis` | ❌ 不支持 | 无法显示省略号 |
| `overflow-y: scroll` | ❌ 不支持 | div 无法滚动 |
| `border` 简写 | ❌ 不支持 | 必须分开写 `border-width` 等 |
| `list` 静态内容 | ❌ 不支持 | `list` 必须使用 `for` 绑定数据 |

### 不支持的 JS 特性
| 特性 | 状态 | 影响 |
|------|------|------|
| `require('@system.*')` | ❌ 不支持 | 必须使用 `import ... from '@system.*'` |
| `@key-down` 事件 | ❌ 不支持 | 输入法会插入英文字符 |

### 硬件/系统限制
| 限制 | 说明 | 解决方案 |
|------|------|---------|
| 息屏时震动马达关闭 | 省电机制，无法绕过 | 仅亮屏时有效 |
| 无法唤醒息屏屏幕 | SDK 无此 API | 用户开启"抬腕唤醒" |
| 抬腕唤醒是系统级 | 快应用无法控制 | 需用户手动开启 |

### 可用的系统模块
| 模块 | 能力 | 限制 |
|------|------|------|
| `system.vibrator` | 震动（短震/长震/自定义） | 息屏时无效 |
| `system.brightness` | 亮度/保持屏幕常亮 | 不能唤醒息屏 |
| `system.sensor` | 加速度/气压/指南针 | 不相关 |
| `system.event` | 电池/充电状态 | 不相关 |
| `system.interconnect` | 模块间通信 | 无唤醒能力 |
| `system.storage` | 本地存储 | 正常 |
| `system.fetch` | 网络请求 | 正常 |

## 接口说明

### OpenCode Server API
- **会话列表**: `GET /session` (端口 4096)
  - 返回 `tokens` 和 `time` 字段
  - `tokens.input` / `tokens.output` - token 使用量
  - `time.created` / `time.updated` - 时间戳
  - `info.time.completed` - undefined 表示仍在处理

- **最新消息**: `GET /session/:id/message?limit=1`
  - 返回消息内容和 `info.time.completed`

## 构建说明
- 输出路径：`/storage/emulated/0/tlen/11_Projects/手环Tlen/dist/tlen.rpk`
- 构建命令：`node /usr/local/bin/aiot build`
- 安装方式：小米健身 App 调试模式安装 RPK
