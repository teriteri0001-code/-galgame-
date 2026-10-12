---
created: '2026-10-12T13:53:46.939916'
tags:
- 工具配置
- mimo-code
- 踩坑记录
---

# MiMo Code 安装配置与踩坑记录

> 记录时间：2026-10-12 | 由 Shiro 整理

## 基本信息

- **工具：** MiMo Code（mimo CLI），小米的 AI 编程助手命令行工具
- **版本：** @mimo-ai/cli 0.1.15（全局 npm 安装）
- **安装命令：** npm install -g @mimo-ai/cli（需要 Node.js 18+）
- **验证：** mimo --version
- **配置目录：** ~/.config/mimocode/mimocode.jsonc
- **凭据目录：** ~/.local/share/mimocode/auth.json

## ⚠️ 核心踩坑：授权时的 Key 类型二选一

登录授权时（mimo providers login → 选 MiMo），平台会让你**选择 API Key 的计费类型**：

| Key 类型 | 计费方式 | 适用 |
|---------|---------|------|
| Pay-as-you-go API | 按量付费，走账户**余额** | 轻度使用 |
| Token Plan | 订阅套餐，走 **token plan 额度** | 订阅用户 |

**坑点：** 手快选了 Pay-as-you-go → 调用 xiaomi/* 模型一直报 Insufficient account balance（查的是余额，而钱在 token plan 里）。必须重新登录选 Token Plan 类型。

**另一个坑：** mimo/mimo-auto 通道是免费期服务，已结束（报 MiMo free API service has ended）。要走 xiaomi/* 模型。

- Key 管理：<https://platform.xiaomimimo.com/#/console/api-keys>
- Token Plan 额度查询：<https://platform.xiaomimimo.com/#/console/plan-manage>
- 官方文档：<https://mimo.mi.com/docs/en-US/tokenplan/integration/mimo-code>

## 常用命令

- `mimo` — 启动 TUI（在项目目录下运行，支持 /init 生成 AGENTS.md）
- `mimo run 任务描述` — 无头模式派活（Shiro 当项目经理的用法）
- `mimo models` — 查看可用模型（xiaomi/mimo-v2.6-pro 等）
- `mimo providers login / list / whoami` — 凭据管理
- `mimo session list / delete` — 会话管理（共享数据库，TUI 里 /sessions 或 ctrl+x l 可切换）
- `mimo stats` — token 用量统计

## 注意事项

- mimo run 子命令**不支持 --trust** 参数（顶层命令才有），传了会报错退出码 1
- mimo run 派的活儿在源端 TUI 里看不到动静（独立无头进程），用 /sessions 切换会话可围观
- 会话列表按项目目录过滤，在哪个目录派的活就要在哪个目录打开 TUI 才看得到
- TUI 里 /thinking 可显示模型思考过程；tab 键切换 Build/Plan 模式

## 用途

Shiro 当项目经理的派活流水线：源酱提需求 → Shiro 拆解任务 → mimo run 派给组员 → Shiro 审代码验收 → 源酱用 /sessions 围观全程。