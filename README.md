# 识野 AI 会议纪要生成器

一个纯前端的 AI 工具：上传会议转录 txt，AI 一键生成标准格式的会议纪要 Excel。

## ✨ 功能

- 📄 **转录导入**：拖入腾讯会议导出的 .txt 转录文件，或直接粘贴文本
- 🤖 **自动解析**：识别参会人、会议日期、发言人结构化对话
- 🎯 **两种生成方式**
  - **API 直连**：自带 DeepSeek、通义千问、Kimi、智谱 GLM、积算平台 5 家预设，也支持自定义 OpenAI 兼容接口；四种连接模式：**自动**（推荐，直连被拦自动走公共代理）/ **直连** / **公共代理** / **自有代理**（最稳，Key 只经你的服务）
  - **手动粘贴**：生成 prompt 发给任意 AI，粘回 JSON 即可
- 📊 **标准格式导出**：按标准会议纪要模板导出 Excel，加粗标题、灰底表头、合并单元格、日期格式齐全
- ✏️ **在线编辑**：预览页直接点击编辑所有字段，改完一键导出

## 🚀 使用

### 在线使用（推荐）
访问 GitHub Pages 部署地址（见仓库 About）。

### 本地使用
下载 3 个文件到同一文件夹，双击 `index.html`：
- `index.html` - 主程序
- `template_embed.js` - Excel 模板
- `README.md` - 本说明

## 🔑 API 配置

支持任意 OpenAI 兼容接口，点预设一键填充：

| 提供商 | API 地址 | 默认模型 | 浏览器直连 |
|---|---|---|---|
| **DeepSeek（推荐默认）** | `https://api.deepseek.com/v1` | `deepseek-chat` | ✅ |
| 通义千问 | `https://dashscope.aliyuncs.com/compatible-mode/v1` | `qwen-plus` | ✅ |
| Kimi | `https://api.moonshot.cn/v1` | `kimi-k2-0711-preview` | ✅ |
| 智谱 GLM | `https://open.bigmodel.cn/api/paas/v4` | `glm-4-flash` | ✅ |
| 积算平台 | `https://api.icompify.com/v1` | `deepseek-v4-flash` | ⚠️ 需走代理 |

API Key 仅存储在浏览器 `localStorage`，不会发送到任何服务器。

> **积算平台说明**：服务器 `api.icompify.com` 不支持浏览器直接调用（2026-08-23 OPTIONS 预检实测 404、无 CORS 头）。选择积算平台后请把「连接方式」切为 **公共代理** 或 **自有代理**。若想 Key 完全不暴露给第三方，可切「手动粘贴」tab 把 prompt 发给任意 AI。

## 🔗 连接方式说明

浏览器跨域限制：部分 API 服务器（如积算平台）未配置 CORS 头，浏览器会直接拦截请求，表现为 `Failed to fetch`。四种模式：

| 模式 | 适用场景 | 说明 |
|---|---|---|
| **自动**（默认推荐） | 大多数 | 先直连，被拦依次尝试 5 个公共代理。DeepSeek / 千问 / Kimi / 智谱都能直连成功 |
| **直连** | 4 家支持商 | 最快，不经任何第三方 |
| **公共代理** | 积算平台等 | Key 会经第三方公共服务（corsproxy.io 等） |
| **自有代理** 🏆 | 最稳 | 自己架 Cloudflare Worker（免费，100K 请求/天），Key 只经你的服务 |

### 自有代理部署步骤（5 分钟）

1. 页面「自有代理」tab 点**复制 Worker 代码**
2. 打开 https://workers.cloudflare.com/ ，登录（免费注册）
3. **Create application → Create Worker**，起个名比如 `my-cors-proxy`，点 **Deploy**
4. **Edit code**，把默认代码**全部替换**为刚才复制的代码，点 **Deploy**
5. 回到 Worker 页面，复制你的 URL（如 `https://my-cors-proxy.your-subdomain.workers.dev/`），粘到页面「自有代理」，**末尾加 `?target=`** 即可

完成后自有代理地址形如 `https://my-cors-proxy.your-subdomain.workers.dev/?target=`，系统会自动拼上 API 目标 URL。这样 Key 只经你自己的 Cloudflare Worker，彻底安全。

### 内置你的代理（团队分发）

想让别人拿到 `index.html` 就自带你的代理（不用填地址），打开 `index.html` 找到这行：

```js
const DEFAULT_CUSTOM_PROXY = ""; // 例："https://my-cors-proxy.your-subdomain.workers.dev/?target="
```

把引号里填上你的 Worker 地址，保存后再部署/分发。此后任何人打开这个文件，点「自有代理」都会自动带出这个地址，不用再配置。单人或试验也可先用「存为默认」按钮（仅记住在本浏览器）。

> ⚠️ 若把这个内置代理版本公开发到 GitHub Pages，等于所有人共用你的 Worker、Key 都经你服务器——只适合团队内部分发，不适合公网公开。

若"自动"模式下所有路线都失败（公共代理不稳定），系统自动切换「手动粘贴」tab 并复制好 prompt——发给任意 AI 粘回 JSON 即可，永远不会卡死。

## 📐 会议纪要规范

- 标题：`XX周例会会议纪要0817`（标题内嵌日期）
- 表头：时间 · 参会人 · 请假人・纪要人（固定"识野AI纪要"）· 汇报人(留空)
- 正文结构：`一、议题名` + `1、要点`（中文数字+全角顿号）
- 待办表格：No. / 待办事项 / 责任人 / 完成时间（Excel 日期格式 `yyyy/m/d`）

## 🛠 技术

纯前端，无后端依赖：
- [ExcelJS](https://github.com/exceljs/exceljs) - Excel 生成
- FileSaver.js - 文件下载
- 原生 HTML / JS / CSS - 零构建工具

## 🔒 隐私

- 所有数据（转录、API Key、生成内容）都留在你浏览器
- 仅"调用 AI API 时"把转录发给你选择的服务商
  - **自动 / 公共代理**：Key 会经第三方公共代理（corsproxy.io / allorigins.win / codetabs.com 等）
  - **自有代理**：Key 只经你自己的 Cloudflare Worker，最安全
  - **直连 / 手动粘贴**：Key 不经任何第三方
- 无任何埋点、统计、上报

## 📄 License

MIT

---

by **识野Insight** · Crafted with care
