# 识野 AI 会议纪要生成器

一个纯前端的 AI 工具：上传会议转录 txt，AI 一键生成标准格式的会议纪要 Excel。

🌐 **在线使用**：https://jeremy-hang.github.io/meeting-minutes-ai/
🏠 **主理人网站**：https://jeremyhang.online/

## ✨ 功能

- 📄 **转录导入**：拖入腾讯会议导出的 .txt 转录文件，或直接粘贴文本
- 🤖 **自动解析**：识别参会人、会议日期、发言人结构化对话
- 🎯 **两种生成方式**
  - **API 直连**：自带 DeepSeek · 通义千问 · Kimi · 智谱 GLM · 积算平台 5 家预设，也支持自定义 OpenAI 兼容接口；**3 种连接模式**：自动（推荐）/ 直连 / 默认代理（内置 Cloudflare Worker，Key 只经主理人服务，不经第三方）
  - **手动粘贴**：生成 prompt 发给任意 AI，粘回 JSON 即可
- 📊 **标准格式导出**：按 6 列标准会议纪要模板导出 Excel，加粗标题、灰底表头、合并单元格、日期格式齐全
- ✏️ **在线编辑**：预览页直接点击编辑所有字段，改完一键导出

## 🚀 使用

### 在线使用（推荐）
打开 https://jeremy-hang.github.io/meeting-minutes-ai/ 直接用。

### 本地使用
下载 3 个文件到同一文件夹，双击 `index.html`：
- `index.html` — 主程序
- `template_embed.js` — Excel 模板
- `README.md` — 本说明

## 🔑 API 配置

支持任意 OpenAI 兼容接口，点预设一键填充：

| 提供商 | API 地址 | 默认模型 | 浏览器直连 |
|---|---|---|---|
| **DeepSeek（推荐默认）** | `https://api.deepseek.com/v1` | `deepseek-chat` | ✅ |
| 通义千问 | `https://dashscope.aliyuncs.com/compatible-mode/v1` | `qwen-plus` | ✅ |
| Kimi | `https://api.moonshot.cn/v1` | `kimi-k2-0711-preview` | ✅ |
| 智谱 GLM | `https://open.bigmodel.cn/api/paas/v4` | `glm-4-flash` | ✅ |
| 积算平台 | `https://api.icompify.com/v1` | `deepseek-v4-flash` | ⚠️ 需走代理 |

API Key **不持久化**到 localStorage，每次页面加载都是空白（v0.9 起的设计，防他人共用浏览器看到上次 Key）。

> **积算平台**：服务器不支持浏览器 CORS。选择积算平台后「连接方式」请切 **默认代理** 或留 **自动**（被拦会自动走内置代理）。

## 🔗 连接方式说明

浏览器跨域限制：部分 API 服务器未配置 CORS，浏览器拦截请求报 `Failed to fetch`。

| 模式 | 说明 |
|---|---|
| **自动**（默认推荐） | 直连 → 被拦走**内置代理**（主理人的 Cloudflare Worker，100K 请求/天）→ 内置代理挂才走公共代理兜底 |
| **直连** | 最快，不经任何第三方（DeepSeek / 千问 / Kimi / 智谱可用） |
| **默认代理** | 强制走内置代理，对外完全隐藏代理细节 |

> 代理细节对外不可见，使用者无需关心地址、Worker 概念、自建流程。

## 📐 会议纪要规范

- **6 列布局**：A=No./标签 · B-D=值/待办 · E=标签（汇报人/纪要人/责任人）· F=值（完成时间）
- 标题：`XX周例会会议纪要0817`（标题内嵌日期）
- 表头：时间 / 地点 / 汇报人 · 参会人 · 请假人 · 纪要人（固定"识野AI纪要"）
- 正文结构：`一、议题名` + `1、要点`（中文数字 + 全角顿号）
- 待办表格：No. / 待办事项 / 责任人 / 完成时间（Excel 日期格式 `yyyy/m/d`）

## 🛠 技术

纯前端，无后端依赖：
- [ExcelJS](https://github.com/exceljs/exceljs) - Excel 生成
- FileSaver.js - 文件下载
- 原生 HTML / JS / CSS - 零构建工具
- Cloudflare Worker - 内置 CORS 代理（可选）

## 🔒 隐私

- 所有数据（转录、API Key、生成内容）都留在你浏览器
- API Key 仅本次会话内存，**不写入 localStorage**
- 仅"调用 AI API 时"把转录发给你选择的服务商
  - **直连 / 手动粘贴**：不经任何第三方
  - **自动 / 默认代理**：经主理人内置 Cloudflare Worker（只做转发，不存数据）
  - **兜底**（内置代理挂时）：经第三方公共代理（corsproxy.io 等）
- 无任何埋点、统计、上报

## 📄 License

MIT

---

by **[识野Insight](https://jeremyhang.online/)** · Crafted with care
