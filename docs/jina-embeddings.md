# Jina Embeddings 支持

本文档说明如何在 CLIProxyAPI 中配置和使用 Jina Embeddings API。

## 功能支持

- ✅ Web UI 模型获取（自动尝试 `/v1/models` 端点）
- ✅ Web UI API Key 测试（支持 Embeddings 类型）
- ✅ 后端请求转发（`/v1/embeddings` 端点）

## 配置示例

在 `config.yaml` 中添加 Jina 配置：

```yaml
openai-compatibility:
  - name: "jina"
    base-url: "https://api.jina.ai/v1"
    api-key-entries:
      - api-key: "jina_你的API密钥"
    models:
      - name: "jina-embeddings-v2-base-en"
        alias: "jina-embeddings"
      - name: "jina-embeddings-v3"
        alias: "jina-embeddings-v3"
      - name: "jina-embeddings-v4"
        alias: "jina-embeddings-v4"
```

## 使用方式

### 通过代理调用

```bash
curl -X POST http://localhost:8317/v1/embeddings \
  -H "Authorization: Bearer 你的API密钥" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "jina-embeddings",
    "input": "Hello world"
  }'
```

**注意**：请使用模型别名（如 `jina-embeddings`）而不是原始模型名。

### 可用模型

| 别名 | 原始模型名 | 说明 |
|------|-----------|------|
| `jina-embeddings` | jina-embeddings-v2-base-en | 英文 Embedding (768维) |
| `jina-embeddings-v3` | jina-embeddings-v3 | 多语言 Embedding (1024维) |
| `jina-embeddings-v4` | jina-embeddings-v4 | 多模态 Embedding (1024维) |
| `jina-reranker` | jina-reranker-v3 | 重排序模型 |

### 通过 Web UI 测试

1. 打开 Management Center（`http://localhost:8317/management.html`）
2. 进入 **AI Providers** → **OpenAI-compatible providers**
3. 添加或编辑 Jina 配置
4. 在测试区域选择 **Embeddings** 类型
5. 点击 **Test All Keys** 验证 API Key

## API 端点

| 端点 | 方法 | 说明 |
|------|------|------|
| `/v1/embeddings` | POST | Embeddings 向量生成 |
| `/v1/models` | GET | 获取可用模型列表 |

## 注意事项

- Jina API 不支持 `/v1/chat/completions` 端点，仅支持 Embeddings 和 Reranker
- 使用时请确保传入正确的模型别名