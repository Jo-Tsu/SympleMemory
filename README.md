🧠 MemoryOS · 带持久记忆的 AI 对话控制台

MemoryOS 是一个具备「持久记忆」能力的轻量级 AI 对话控制台。

你可以接入任意支持 OpenAI API 风格的模型（DeepSeek / 通义千问 / Gemini / Claude / Kimi / OpenAI 等），在统一界面中进行多会话对话。系统会在后台自动提取、维护并更新你的个人长期记忆，并在每次对话前自动注入相关上下文。

⸻

✨ Features

🧩 Model Hub（模型中心）
	•	支持配置多个外部模型：
	•	DeepSeek
	•	Qwen
	•	Claude
	•	Gemini
	•	Kimi
	•	OpenAI
	•	其他兼容 OpenAI API 的代理
	•	每个模型可配置：
	•	displayName
	•	platform
	•	modelId
	•	baseUrl
	•	apiKey
	•	是否设为默认模型
	•	前端可视化管理：
	•	创建 / 编辑 / 删除模型
	•	API 连通性测试

⸻

💬 Chat Console（对话控制台）
	•	类 ChatGPT 的多会话界面
	•	支持多会话管理
	•	会话本地持久化（localStorage）
	•	会话标题根据首条消息自动生成
	•	可随时切换当前使用模型

⸻

🧠 Memory Engine（记忆引擎）

后端维护结构化记忆系统：

用户画像（Profile）
	•	workContext — 工作背景
	•	personalStyle — 沟通风格
	•	currentFocus — 当前关注点

原子事实（Facts）
每条事实包含：
	•	content
	•	category
	•	confidence
	•	createdAt
	•	sourceSessionId

工作机制
	1.	对话结束后：
	•	异步调用 LLM 总结并抽取记忆
	•	更新数据库
	2.	新对话开始前：
	•	自动注入「画像 + 相关事实」
	•	构建增强后的 System Prompt

⸻

🗂 Memory Center（记忆中心）
	•	可视化编辑：
	•	工作背景
	•	个人风格
	•	当前关注点
	•	支持逐条删除原子事实
	•	修改后自动调用后端 API 持久化

⸻

🐳 Docker 一键启动
	•	前端：Vite + React + Tailwind → Nginx 托管
	•	后端：FastAPI + Uvicorn
	•	存储：默认 SQLite（无需额外数据库）

⸻

🏗️ Tech Stack

Frontend
	•	React 18
	•	TypeScript
	•	Vite
	•	Tailwind CSS
	•	Framer Motion

Backend
	•	FastAPI
	•	SQLAlchemy
	•	SQLite（默认）/ PostgreSQL（可选）
	•	Pydantic
	•	pydantic-settings

⸻

🚀 Quick Start（本地开发）

1️⃣ 克隆项目

git clone https://github.com/your-name/memoryos.git
cd memoryos


⸻

2️⃣ 启动后端

需要 Python 3.10+（推荐 3.11）

cd backend

python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate

pip install -r requirements.txt

可选：修改后端配置

在 backend/.env 中设置：

DATABASE_URL=sqlite+pysqlite:///./memoryos.db
LLM_BASE_URL=https://api.deepseek.com
LLM_API_KEY=sk-xxx
LLM_MODEL=deepseek-chat

启动后端服务

uvicorn app.main:app --reload --host 0.0.0.0 --port 1880

默认接口地址：

http://localhost:1880/api/v1/...


⸻

3️⃣ 启动前端

需要 Node.js 18+

cd ../

npm install

可选：配置前端 API 地址

在根目录 .env.local 中设置：

VITE_API_BASE_URL=http://localhost:1880

启动前端

npm run dev

访问：

http://localhost:5173


⸻

🐳 Docker 部署（本地 / 服务器通用）

仓库已提供：
	•	Dockerfile.backend
	•	Dockerfile.frontend
	•	docker-compose.yml

一键启动

docker compose build
docker compose up -d

默认端口映射：

服务	地址
前端	http://localhost:3000
后端	http://localhost:1880


⸻

🚀 服务器部署（如阿里云 ECS）
	1.	上传代码到服务器
	2.	安装 Docker & Docker Compose
	3.	执行：

docker compose build
docker compose up -d

	4.	确保安全组开放：

	•	3000（或改为 80:80）

⸻

⚙️ Configuration

后端配置

文件位置：

backend/app/config.py

支持通过环境变量或 .env 覆盖：

变量	默认值	说明
DATABASE_URL	sqlite+pysqlite:///./memoryos.db	数据库连接
REDIS_URL	-	预留缓存/队列
LLM_BASE_URL	https://api.deepseek.com	LLM 网关
LLM_API_KEY	-	API Key
LLM_MODEL	-	用于记忆抽取

PostgreSQL 示例

DATABASE_URL=postgresql+psycopg://user:pass@host:5432/memoryos


⸻

前端配置

.env.* 文件：

VITE_API_BASE_URL=http://localhost:1880

Docker + Nginx 环境推荐

VITE_API_BASE_URL=/api

由 Nginx 反向代理转发到后端容器。

⸻

🧠 Memory Architecture

MemoryOS 的记忆系统分为两层：

1️⃣ 用户画像（Profile）
	•	workContext
	•	personalStyle
	•	currentFocus

2️⃣ 原子事实（Facts）

字段：
	•	content
	•	category
	•	confidence
	•	createdAt
	•	sourceSessionId

⸻

🔄 记忆流程

对话结束
    ↓
LLM 总结 & 抽取记忆
    ↓
更新数据库
    ↓
新对话开始
    ↓
注入画像 + 相关事实
    ↓
构建增强 System Prompt

通过这种机制，模型获得「持续性的用户理解」。

⸻

📌 Roadmap（可选扩展）
	•	向量数据库支持（记忆语义检索）
	•	Redis 异步任务队列
	•	多用户支持
	•	记忆版本回溯
	•	记忆权重衰减机制

⸻

📄 License

MIT License

⸻

如果你愿意，我可以再帮你：
	•	写一个更偏“产品感”的 README（适合开源展示）
	•	或写一个偏“架构设计说明”的版本（适合技术分享）
	•	或做一个更简洁的开源首页版本（更利于 GitHub Star 转化）

你这个项目结构其实已经很成熟了，可以往「可开源产品级」再推一步。
