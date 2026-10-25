# 部署记录

> 本文只记录配置项名称与状态，**不包含任何密钥、Cookie、Webhook 的实际值**。

## 仓库信息

- 仓库：`silzhan/GLaDOS-Auto-Checkin`（Fork 自 `MZRidaz/GLaDOS-Auto-Checkin`）
- 默认分支：`main`
- 工作流：`.github/workflows/glados.yml`

## 配置记录（2026-10-25）

| 项目 | 位置 | 状态 |
|------|------|------|
| `COOKIES` | Settings → Secrets and variables → Actions | 已配置 |
| `FEISHU_WEBHOOK` | Settings → Secrets and variables → Actions | 已配置（未启用加签，无需 `FEISHU_SECRET`） |
| Actions 工作流 | Actions 页面 | Fork 限制已解除，状态 `disabled_fork` → `active` |
| 线上验证 | Actions → GLaDOS Auto Checkin | 手动触发运行成功：已签到、飞书推送成功（1/1） |

## 触发方式

| 触发 | 时间 | 说明 |
|------|------|------|
| 定时签到 | 每天 UTC 04:17（北京时间 12:17，非整点避开高峰） | 执行 `checkin.py` 并推送 |
| 定时保活 | 每月 1 日 UTC 00:00 | 空提交，防止公开仓库 60 天无活动导致定时任务被停 |
| 手动 | Actions 页面 Run workflow / `gh workflow run glados.yml` | 立即执行一次 |

cron 使用 UTC 时间；定时任务只读取默认分支 `main` 的代码。

## 注意事项

- Cookie 会过期，失效后需重新登录 GLaDOS 获取并更新 `COOKIES` Secret。
- 飞书机器人若改为「签名校验」，需补充 `FEISHU_SECRET` Secret。
- 公开仓库的 Actions 日志公开可见（邮箱已脱敏）；如需隐藏运行记录，可将仓库转为私有。
- Fork 仓库的工作流需在 Actions 页面手动启用一次；此后如长期无运行，GitHub 可能再次禁用定时任务。
