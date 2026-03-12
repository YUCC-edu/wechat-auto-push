# 微信公众号排版引擎 & OpenClaw 自动化技能

本项目是一个专为微信公众号深度优化的 Markdown 转 HTML 排版引擎。结合了可视化的 React 实时预览前端，以及专供 AI Agent（如 OpenClaw）使用的 Python 自动化 Skill。

## ✨ 核心特性

* **极致的微信兼容性**：彻底解决微信后台编辑器常见的“多余空行”、“代码块无法横向滚动”、“表格错乱”等痛点。
* **现代化的排版美学**：
  * **H2 标题**：采用自适应主题色的圆角背景色块（`inline-block`），在微信中渲染极其稳定且美观。
  * **段落排版**：强制零边距（`margin: 0`），依靠 `line-height` 撑开间距，中文字符无缝拼接，保留作者的手动留白。
  * **原生表格**：支持真正的 HTML 表格渲染，带有主题色表头和细致边框，支持横向滑动防溢出。
  * **原生列表**：使用原生的 `disc` 和 `decimal` 样式，告别错位的自定义小圆点。
* **双语核心实现**：
  * **TypeScript 版**：用于前端 Web 编辑器的实时预览与交互。
  * **Python 版 (OpenClaw Skill)**：专为 AI Agent 设计的独立脚本，内置高颜值 Fallback 主题，确保自动化流水线 100% 稳定运行。

## 🚀 模块说明

### 1. Web 可视化编辑器 (React + Vite)
提供了一个所见即所得的 Markdown 编辑器，支持多主题切换，实时预览微信排版效果。
* **核心逻辑**：`/src/utils/WeChatHTMLConverter.ts`
* **运行方式**：
  ```bash
  npm install
  npm run dev
  ```

### 2. OpenClaw 自动化技能 (Python)
将排版引擎封装为标准的 OpenClaw Skill，供 AI Agent 在“自动化写稿 -> 排版 -> 上传公众号”的流水线中无缝调用。
* **文件位置**：`/src/skills/wechat_formatter_skill.py`
* **使用示例**：
  ```python
  from src.skills.wechat_formatter_skill import convert_markdown_to_wechat_html

  markdown_text = "## 自动生成的文章\n\n这是一段测试文本。"
  
  # 直接调用，自带默认高颜值主题，无需额外加载 YAML 配置文件
  html_content = convert_markdown_to_wechat_html(markdown_text)
  ```

## 🛠️ 微信排版“黑科技”细节

1. **HTML 终极压缩**：在输出 HTML 时，移除了所有标签间的换行符（`\n`）和多余空格（`><` 替换 `> <`）。这是因为微信的 UEditor 魔改版会把源码中的每一个回车符解析为空行（`<br>` 或 `<p>`），压缩为单行 HTML 是保持排版紧凑的唯一解。
2. **防溢出设计**：所有的 `<pre>`（代码块）和 `<table>`（表格）外层均包裹了 `<section style="max-width: 100%; overflow-x: auto; box-sizing: border-box;">`，确保在任何尺寸的手机屏幕上都能完美横向滑动，不会撑破页面。
3. **伪元素规避**：微信编辑器会过滤 CSS 伪元素（如 `::before`、`::after`），因此所有的样式设计（如 H2 的色块、列表的缩进）全部采用了最基础的内联样式（Inline Styles）和真实的 HTML 标签实现。
