# AgentOrchestrator 部署指南

## 环境要求

| 组件 | 最低版本 |
|------|---------|
| Docker | 24.0 |
| Docker Compose | 2.20 |
| Python (本地开发) | 3.10 |

## 方式一：Docker Compose 部署 (推荐)

### 1. 配置环境变量

```bash
cp .env.example .env
```

编辑 `.env` 文件：

```env
OPENAI_API_KEY=sk-your-key-here
OPENAI_BASE_URL=https://api.openai.com/v1
LLM_MODEL=gpt-4o
```

### 2. 启动所有服务

```bash
docker compose up -d
```

### 3. 查看日志

```bash
docker compose logs -f agent-core
docker compose logs -f dashboard
```

### 4. 验证部署

```bash
curl http://localhost:8000/health
```

### 5. 停止服务

```bash
docker compose down
```

---

## 方式二：本地开发部署

### Python Agent Core

```bash
cd agent-core
pip install -r requirements.txt

export OPENAI_API_KEY=sk-your-key
python main.py
```

### Streamlit 仪表盘 (可选)

```bash
cd dashboard
pip install -r requirements.txt
streamlit run app.py
```

---

## 国内镜像加速

### Docker

编辑 `/etc/docker/daemon.json`：

```json
{
  "registry-mirrors": [
    "https://docker.1ms.run",
    "https://docker.xuanyuan.me"
  ]
}
```

### pip

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```
