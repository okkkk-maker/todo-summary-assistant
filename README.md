[README.md](https://github.com/user-attachments/files/35285578/README.md)
# 待办自动汇总助手 (Chat To-Do Assistant)

![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square)
![Python](https://img.shields.io/badge/Python-3.10%20~%203.13-3776AB?style=flat-square)
![Version](https://img.shields.io/badge/Version-v1.5.3.3-14B8A6?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

一款运行在 Windows 本地的**多平台聊天消息只读采集与待办自动汇总桌面工具**。支持从 QQ (NT)、个人微信 (4.x)、飞书、企业微信、钉钉统一采集增量消息，生成带原始消息溯源的结构化待办素材与清单，并支持定时调度与企业微信推送。

## 核心特性

- **独立桌面应用形态**：基于 Chromium App Mode 运行（无浏览器地址栏与多余标签页），支持一键创建桌面与开始菜单快捷方式（`安装桌面App.bat`）。
- **6 步可视化新手向导**：内置首次启动配置向导，自动完成本机环境体检、通道装配、连通性测试、脱敏示例体验与定时调度配置。
- **开箱即用的便携运行时**：不强制要求目标机器预装 Python，双击 `获取Python.bat` 即可自动准备隔离的 `.venv` 运行环境（支持断网离线装配）。
- **多平台统一接入**：
  - **QQ (NT)**：本地只读副本解密（SQLCipher）与自动表结构适配。
  - **个人微信 (4.x)**：本地只读明文镜像与增量解析。
  - **飞书 (Feishu)**：官方 Open API 主动增量拉取。
  - **企业微信 / 钉钉**：支持收件箱模式导入，以及企业微信群机器人 Webhook 推送待办清单。
- **内置安全防护**：本机控制台默认仅监听 `127.0.0.1`，启用 `Host` 头防 DNS Rebinding 校验、同源写保护、敏感密钥脱敏存储、出站 Webhook SSRF 校验及 Zip Slip 防护。
- **一键彻底卸载**：内置 `彻底卸载.bat`（及界面一键清理），完整注销自启动、删除快捷方式并销毁本地解密镜像与缓存。

## 快速开始

### 1. 获取代码

```bash
git clone https://github.com/<你的用户名>/<你的仓库名>.git
cd <你的仓库名>
```

> 若通过 GitHub 页面下载 ZIP 压缩包，请解压到非系统受保护目录（例如 `D:\Tools\todo-assistant`，避免放在 `C:\Program Files` 下导致权限受限）。

### 2. 准备运行环境

- **电脑未安装 Python（或版本不兼容）**：双击根目录下的 **`获取Python.bat`**，程序将自动在项目目录内构建 `.venv` 便携运行时。
- **已有 Python 3.10 ~ 3.13**：可直接执行：

```bash
pip install -r requirements.txt
```

### 3. 启动桌面控制台

- **推荐方式**：首次双击 **`安装桌面App.bat`** 生成桌面图标，之后直接双击桌面上的「待办总结助手」即可启动。
- **命令行方式**：

```bash
python app.py
```

启动后将自动打开独立桌面窗口（后台服务默认运行在 `http://127.0.0.1:8000`），按向导完成配置即可使用。

## 配置说明

首次运行可在控制台界面直接填写配置，也可复制 `.env.example` 为 `.env` 后手动编辑：

```bash
copy .env.example .env
```

> **安全提示**：`.env`、`work/`、`output/` 与 `config/schema.json` 包含你的本地密钥与聊天记录素材，已被 `.gitignore` 忽略，**切勿提交到公开仓库**。

## 目录结构

```text
.
├─ app.py                  桌面 App 统一入口（拉起服务 + 独立窗口）
├─ server.py               本地监控台服务（127.0.0.1:8000，含向导与调度器）
├─ run.py                  多平台增量采集执行器
├─ index.html              桌面工作台前端界面
├─ automation-prompt.md    自动化待办提炼提示词模板
├─ tools/                  各通道采集、调度、健康检查与运维脚本
├─ config/                 平台启用状态、调度计划与向导状态配置
├─ docs/                   部署迁移与各平台接入指引文档
├─ vendor/                 离线便携运行时与预编译依赖（可选放 Releases）
├─ 安装桌面App.bat         一键创建桌面快捷方式
├─ 启动桌面App.bat         启动桌面应用窗口
├─ 获取Python.bat          一键准备便携 Python 环境
└─ 彻底卸载.bat            一键彻底清理自启、图标与本地缓存
```

## 安全与隐私原则

1. **本地只读优先**：聊天数据库仅在本地工作区复制临时副本进行只读解析，不修改任何原始客户端数据文件。
2. **数据留在本地**：原始聊天素材仅保存在本地 `work/` 目录，不向任何未授权第三方服务器上传。
3. **来源可溯、故障告警**：每条提炼待办均保留会话名、发送人与时间戳；通道异常时自动生成诊断单并冻结游标，避免漏消息。

更多安全说明与漏洞反馈方式请参阅 [SECURITY.md](SECURITY.md)。

## 免责声明

本项目仅供个人效率提升、学习交流与本地数据整理使用，使用者须严格遵守所在地区法律法规及对应聊天软件的服务条款，**仅限读取本人合法拥有的聊天数据**，禁止用于任何未经授权的数据采集或商业侵权行为。

## 参与贡献

欢迎通过 [Issues](../../issues) 反馈 Bug 或提出功能建议，也欢迎提交 Pull Request。提交前请先阅读 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 开源协议

本项目基于 [MIT License](LICENSE) 开源。
