# 识野 AI 会议纪要生成器

上传腾讯会议转录 txt，AI 一键生成符合标准格式的会议纪要 Excel。

## 🚀 使用方式

### 在线使用（无需安装）
直接访问 GitHub Pages 部署地址（见仓库 About）。

### 本地使用
下载 `index.html` 和 `template_embed.js` 放在同一文件夹，双击 `index.html` 即可。

## 📋 核心功能

- **转录导入**：拖入腾讯会议 GMT 导出的 .txt 转录文件，或直接粘贴文本
- **自动解析**：自动识别参会人、会议日期、清洗口语化转录
- **两种 AI 调用方式**：
  - **API 直连**：支持 DeepSeek、通义千问、Kimi、OpenAI、智谱 GLM 等
  - **手动粘贴**：生成 prompt 发给任意 AI，粘回 JSON
- **标准格式导出**：按公司会议纪要模板导出 Excel，含加粗标题、灰底表头、合并单元格、日期格式
- **在线编辑**：预览页直接点击编辑所有字段

## 🔑 API 配置

支持任意 OpenAI 兼容接口，点预设一键填充：

| 服务商 | API 地址 | 推荐模型 |
|---|---|---|
| DeepSeek | `https://api.deepseek.com/v1` | `deepseek-chat` |
| 通义千问 | `https://dashscope.aliyuncs.com/compatible-mode/v1` | `qwen-plus` |
| Kimi | `https://api.moonshot.cn/v1` | `kimi-k2-0711-preview` |
| OpenAI | `https://api.openai.com/v1` | `gpt-4o-mini` |
| 智谱 GLM | `https://open.bigmodel.cn/api/paas/v4` | `glm-4-flash` |

API Key 只存储在浏览器 localStorage，不会发送到任何服务器。

## 🛠 技术栈

纯前端，无后端依赖：
- [ExcelJS](https://github.com/exceljs/exceljs) - Excel 生成
- FileSaver.js - 文件下载
- 原生 HTML/JS/CSS - 无需构建工具

## 📝 会议模板格式

按浪潮信息渠道部周例会标准格式生成：
- 标题：渠道部周例会会议纪要 MMDD
- 内容结构：`一、议题名` + `1、要点`（中文数字+全角顿号）
- 待办表格：No. / 待办事项 / 责任人 / 完成时间（日期格式）

## 📄 License

MIT

---

by **识野Insight** · 让 AI 干脏活
