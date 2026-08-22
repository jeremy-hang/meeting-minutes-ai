# 识野 AI 会议纪要生成器

一个纯前端的 AI 工具：上传会议转录 txt，AI 一键生成标准格式的会议纪要 Excel。

## ✨ 功能

- 📄 **转录导入**：拖入腾讯会议导出的 .txt 转录文件，或直接粘贴文本
- 🤖 **自动解析**：识别参会人、会议日期、发言人结构化对话
- 🎯 **两种生成方式**
  - **API 直连**：自带积算平台、DeepSeek、通义千问、Kimi、OpenAI、智谱 GLM 等 6 家预设，也支持自定义 OpenAI 兼容接口；三种连接模式：**自动回退**（推荐，直连被拦自动走代理）/ **直连** / **代理中转**
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

| 提供商 | API 地址 | 默认模型 |
|---|---|---|
| 积算平台 | `https://api.icompify.com/v1` | `deepseek-v4-flash` |
| DeepSeek | `https://api.deepseek.com/v1` | `deepseek-chat` |
| 通义千问 | `https://dashscope.aliyuncs.com/compatible-mode/v1` | `qwen-plus` |
| Kimi | `https://api.moonshot.cn/v1` | `kimi-k2-0711-preview` |
| OpenAI | `https://api.openai.com/v1` | `gpt-4o-mini` |
| 智谱 GLM | `https://open.bigmodel.cn/api/paas/v4` | `glm-4-flash` |

API Key 仅存储在浏览器 `localStorage`，不会发送到任何服务器。

### 连接方式说明

浏览器跨域限制：部分 API 服务器（如积算平台）未配置 CORS 头，浏览器会直接拦截请求，表现为 `Failed to fetch`。三种模式选择：

| 模式 | 适用场景 | 说明 |
|---|---|---|
| **自动回退**（推荐） | 大多数场景 | 先直连，被拦截自动切换公共 CORS 代理，体验最顺畅 |
| **直连** | API 服务器已配置 CORS | 最快，不经任何中转，部分服务器不支持 |
| **代理中转** | 直连确认不可用 | 公共代理（corsproxy.io / allorigins.win）绕过浏览器限制，Key 会经第三方 |

若"自动回退"和"代理中转"都失败，说明公共代理不稳定，请切换"手动粘贴"模式。

## 📐 会议纪要规范

- 标题：`XX周例会会议纪要0817`（标题内嵌日期）
- 表头：时间 · 参会人 · 请假人・纪要人（固定"识野AI纪要"）· 汇报人（留空）
- 正文结构：`一、议题名` + `1、要点`（中文数字+全角顿号）
- 待办表格：No. / 待办事项 / 责任人 / 完成时间（Excel 日期格式 `yyyy/m/d`）

## 🛠 技术

纯前端，无后端依赖：
- [ExcelJS](https://github.com/exceljs/exceljs) - Excel 生成
- FileSaver.js - 文件下载
- 原生 HTML / JS / CSS - 零构建工具

## 🔒 隐私

- 所有数据（转录、API Key、生成内容）都留在你浏览器
- 仅"调用 AI API 时"把转录发给你选择的服务商；自动回退/代理中转模式下请求会经第三方公共代理（corsproxy.io / allorigins.win / codetabs.com），介意请切"直连"或"手动粘贴"
- 无任何埋点、统计、上报

## 📄 License

MIT

---

by **识野Insight** · Crafted with care
