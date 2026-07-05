# OC手环助手

小米手环 Vela OS 平台的 AI 对话客户端，基于 OpenCode Server API。

## 功能

- **AI 对话** - 与 AI 进行实时对话
- **中文输入** - 支持拼音输入法
- **会话管理** - 创建、切换、浏览历史会话
- **字体调节** - 小/中/大三种字体大小
- **使用统计** - 查看 Token 使用量、模型排名

## 安装

1. 下载 Release 中的 `.rpk` 文件
2. 打开小米健身 App → 我的 → 关于小米手环 → 连续点击版本号 10 次开启调试模式
3. 在调试模式中安装 RPK 文件

## 开发

```bash
# 安装依赖
npm install

# 开发构建
npm run start

# 生产构建
npm run build
```

## 配置

1. 打开应用进入设置页面
2. 输入 OpenCode Server 的 IP 地址
3. 点击连接测试确认

## 技术栈

- 小米 Vela 快应用框架
- NEORUAA 输入法组件
- OpenCode Server API

## 系统限制

- 息屏时震动马达关闭（系统省电机制）
- 不支持 `<text lines="n">`、`white-space: nowrap`、`text-overflow: ellipsis`
- 不支持 `overflow-y: scroll`（无法滚动长文本）
- 推荐开启"抬腕唤醒"功能以获得最佳体验

## 许可证

MIT License
