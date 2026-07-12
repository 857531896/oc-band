# OC手环助手

小米手环 Vela OS 平台的 AI 对话客户端，基于 OpenCode Server API。

## 功能

- **AI 对话** - 与 AI 进行实时对话
- **中文输入** - 支持拼音输入法
- **会话管理** - 创建、切换、浏览历史会话
- **字体调节** - 小/中/大三种字体大小
- **使用统计** - 查看 Token 使用量、模型排名

## 安装步骤

### 1. 安装工具

推荐使用 **AstroBox** 安装 RPK 文件，支持 Windows、macOS、Linux、Android、iOS。

**下载地址：** https://astrobox.online/zh-tw/downloads

### 2. 连接手环

1. 打开手环 → 设置 → 连接新手机
2. 打开 AstroBox → 登录小米账号
3. 在 AstroBox 中添加设备并配对

### 3. 安装 RPK

1. 下载本仓库 Release 中的 `tlen.rpk` 文件
2. 在 AstroBox 中选择「安装应用」
3. 选择下载的 `.rpk` 文件
4. 等待安装完成，手环上即可看到「OC手环助手」

**其他安装方式：**

- **小米运动健康（开发者版本）**：我的 → 关于 → 连续点击版本号 10 次 → 调试 → 第三方应用 → 安装 RPK
- **表盘自定义工具**：工具 → 蓝牙安装 → 选择 RPK 文件

## 服务器配置

### 1. 启动 OpenCode Server

在电脑上启动 OpenCode Server，确保手环和电脑在同一局域网内：

```bash
# 启动 OpenCode Server（默认端口 4096）
opencode serve --port 4096
```

### 2. 手环连接服务器

1. 打开手环上的「OC手环助手」
2. 进入设置页面（主界面向右滑动）
3. 在「服务器地址」输入电脑的 IP 地址（如 `192.168.1.100`）
4. 点击「连接测试」确认连接成功
5. 确认后返回主界面

### 3. 开始对话

- 主界面显示当前会话
- 点击左下角输入框打开键盘
- 输入问题后点击「发送」
- AI 回复会显示在消息卡片中

## 手势操作

| 操作 | 效果 |
|------|------|
| 左滑 | 查看使用统计 |
| 右滑 | 从统计页返回主页 |
| 点击 ‹ › | 切换会话 |

## 使用统计

左滑进入统计页面，可以看到：

- **今日总用量** - 当天 Token 使用量
- **24 小时趋势** - 每 2 小时的使用量柱状图
- **模型 Top 5** - 各模型使用量排名

## 系统限制

- 息屏时震动马达关闭（系统省电机制）
- 不支持 `<text lines="n">`、`white-space: nowrap`、`text-overflow: ellipsis`
- 不支持 `overflow-y: scroll`（使用 Vela scroll 组件代替）
- 推荐开启「抬腕唤醒」功能以获得最佳体验

## 开发

```bash
# 安装依赖
npm install

# 开发构建
npm run start

# 生产构建
npm run build
```

## 技术栈

- 小米 Vela 快应用框架
- NEORUAA 输入法组件
- OpenCode Server API

## 许可证

MIT License
