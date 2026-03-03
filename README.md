# MemoryOS · 带持久记忆的 AI 对话控制台

MemoryOS 是一个带「持久记忆」能力的轻量级 AI 对话控制台。  
你可以接入任意支持 OpenAI 接口风格的模型（DeepSeek / 通义千问 / Gemini / Claude / Kimi / OpenAI 等），在一个统一界面中聊天，同时系统会在后台持续提取并维护你的个人记忆——工作背景、个人风格、当前关注点和关键事实——并在每次对话前自动注入。

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

###2. 启动后端
> 需要 Python 3.10+，推荐 3.11。
com
cd backendpython -m venv .venvsource .venv/bin/activate  # Windows 使用 .venv\Scripts\activatepip install -r requirements.txt# 可根据需要在 backend/.env 覆盖配置# 例如：# DATABASE_URL=sqlite+pysqlite:///./memoryos.db# LLM_BASE_URL=https://api.deepseek.com# LLM_API_KEY=sk-xxxuvicorn app.main:app --reload --host 0.0.0.0 --port 1880
后台接口地址默认是：http://localhost:1880/api/v1/...
###3. 启动前端
> 需要 Node.js 18+。
cd memoryosnpm install# 配置前端环境变量（可选）# 在 memoryos/.env.local 中设置：# VITE_API_BASE_URL=http://localhost:1880npm run dev
浏览器访问：http://localhost:5173（Vite 默认端口），即可看到控制台界面。
🐳 Docker 部署（本机 & 服务器通用）
> 本仓库提供了 Dockerfile.backend、Dockerfile.frontend 和 docker-compose.yml，一键启动前后端。
在项目根目录执行：
docker compose builddocker compose up -d
默认映射端口：
前端：http://localhost:3000
后端：http://localhost:1880
在服务器（如阿里云 ECS）部署时：
把代码上传到服务器
安装 Docker / Docker Compose
执行同样的 docker compose build && docker compose up -d
确保安全组放通 3000 端口（或将 docker-compose.yml 中前端端口改为 80:80）
⚙️ 配置说明
后端配置（backend/app/config.py）
可通过环境变量或 .env 覆盖：
DATABASE_URL
默认：sqlite+pysqlite:///./memoryos.db
如需 PostgreSQL：postgresql+psycopg://user:pass@host:5432/memoryos
REDIS_URL（预留，将来可用于队列 / 缓存）
LLM_BASE_URL
默认：https://api.deepseek.com（可改为任意 OpenAI 兼容网关）
LLM_API_KEY / LLM_MODEL
后端用于执行记忆抽取的模型配置。
前端配置（memoryos/.env.*）
VITE_API_BASE_URL
开发环境：http://localhost:1880
Docker + Nginx 下：建议设为 /api，由 Nginx 反向代理转发到后端服务。
🧠 记忆模型概览
MemoryOS 的记忆分为两层：
用户画像（Profile）
workContext：工作背景、领域、公司 / 项目概况
personalStyle：沟通偏好、语言风格
currentFocus：最近一段时间的重点关注 / 项目
原子事实（Facts）
每条记录包含：content / category / confidence / createdAt / sourceSessionId
用于存储更细粒度的习惯、偏好、历史事件等
在对话结束后，后端会调用 LLM 对本轮对话做总结与抽取，更新上述结构；在新对话开始前，系统会将这些信息格式化后注入到 System Prompt 中，让模型对用户有「持续的记忆」。
