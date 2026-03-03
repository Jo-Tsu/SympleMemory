# MemoryOS · 带持久记忆的 AI 对话控制台

MemoryOS 是一个带「持久记忆」能力的轻量级 AI 对话控制台。  
你可以接入任意支持 OpenAI 接口风格的模型（DeepSeek / 通义千问 / Gemini / Claude / Kimi / OpenAI 等），在一个统一界面中聊天，同时系统会在后台持续提取并维护你的个人记忆——工作背景、个人风格、当前关注点和关键事实——并在每次对话前自动注入。

> ✅ 本项目设计灵感来自 DeerFlow 的记忆系统，并做了简化与前后端一体化实现，适合作为个人项目或小团队自托管使用。

---

## ✨ 特性 Highlights

- **模型接入（Model Hub）**
  - 支持配置多个外部模型：DeepSeek / Qwen / Claude / Kimi / Gemini / OpenAI / 其他兼容代理
  - 为每个模型设置：`displayName`、`platform`、`modelId`、`baseUrl`、`apiKey`、是否默认模型
  - 前端可视化管理：创建 / 编辑 / 删除模型，测试连通性

- **对话控制台（Chat Console）**
  - 类 ChatGPT 的对话界面，支持多会话管理
  - 会话本地持久化（localStorage），标题自动根据首条消息生成
  - 可随时切换当前使用模型

- **记忆系统（Memory Engine）**
  - 后端维护结构化记忆：
    - 画像：工作背景（workContext）、个人风格（personalStyle）、当前关注点（currentFocus）
    - 原子事实列表：`content` / `category` / `confidence` / `createdAt`
  - 对话结束后异步提取记忆并更新数据库
  - 每次对话前自动注入「画像 + 相关事实」到 LLM 提示词中

- **记忆中心（Memory Center）**
  - 可视化编辑三块画像信息（工作背景 / 个人风格 / 当前关注点）
  - 支持逐条删除原子事实
  - 画像修改后自动调用后端 API 持久化

- **一键 Docker 启动**
  - 前端：Vite + React + Tailwind，打包后由 Nginx 提供
  - 后端：FastAPI + Uvicorn
  - 存储：默认使用 SQLite 文件（无需额外数据库服务）

---

## 🏗️ 技术栈

- **前端**
  - React 18 + TypeScript
  - Vite
  - Tailwind CSS
  - Framer Motion（动效）

- **后端**
  - FastAPI
  - SQLAlchemy + SQLite（默认，可换 PostgreSQL）
  - Pydantic / pydantic-settings

---

## 🚀 快速开始（本地开发）

### 1. 克隆项目

git clone https://github.com/your-name/memoryos.git
cd memoryos

