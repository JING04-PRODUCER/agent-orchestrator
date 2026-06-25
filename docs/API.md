# AgentOrchestrator API 文档

## Agent Core (Python FastAPI) — port 8000

### 健康检查

```http
GET /health
```

响应:
```json
{
  "status": "ok",
  "app": "AgentOrchestrator",
  "version": "0.1.0",
  "model": "gpt-4o",
  "tools_count": 3
}
```

---

### 工具

#### 列出所有工具

```http
GET /api/tools?category=database
```

响应:
```json
{
  "tools": [
    {
      "name": "execute_sql",
      "description": "执行只读SQL查询...",
      "category": "database",
      "parameters": [
        {"name": "query", "type": "string", "description": "...", "required": true}
      ]
    }
  ]
}
```

#### 获取工具详情

```http
GET /api/tools/{tool_name}
```

---

### Agent

#### 创建 Agent

```http
POST /api/agents
Content-Type: application/json

{
  "name": "data-analyst",
  "description": "数据分析专家",
  "system_prompt": "你是一个精通SQL和数据分析的AI助手...",
  "tools": ["read_file", "execute_sql", "list_tables"],
  "max_iterations": 8,
  "temperature": 0.3
}
```

#### 列出所有 Agent

```http
GET /api/agents
```

#### 获取 Agent 详情 (含历史)

```http
GET /api/agents/{agent_name}
```

#### 执行 Agent 任务

```http
POST /api/agents/{agent_name}/run
Content-Type: application/json

{
  "agent_name": "data-analyst",
  "task": "分析 sales 表的月度趋势并给出优化建议"
}
```

#### 删除 Agent

```http
DELETE /api/agents/{agent_name}
```

---

### 工作流

#### 执行多 Agent 工作流

```http
POST /api/workflows
Content-Type: application/json

{
  "agents": ["analyzer", "validator", "reporter"],
  "task": "分析项目代码质量并生成报告",
  "mode": "sequential"
}
```

---

## Streamlit Dashboard — port 8501

可视化仪表盘，查看 Agent 状态、任务执行情况。通过 Docker Compose 或手动 `streamlit run dashboard/app.py` 启动。

---

## 错误响应格式

```json
{
  "detail": "Agent 'xxx' not found"
}
```

HTTP 状态码:
- `200` 成功
- `201` 创建成功
- `400` 请求参数错误
- `404` 资源不存在
- `500` 服务内部错误
