# floria-card-models

Floria 工作区预览卡片「模型」——在 Floria 预览面板里查看凭据池的供应商与模型，并勾选启用哪些模型。

## 功能

- **供应商列表**：品牌图标 + 已启用模型数 + base URL，右上角刷新 / 新增供应商，支持搜索
- **供应商详情**：列出该供应商端点返回的全部模型（`/gateway/models/catalog`），点动作位 `+` / `✓` 勾选启用；「保存启用项」写回凭据池
- **模型搜索**、手动补充模型（目录里没有的也能加）
- **新增供应商**：填写 base URL / key / 模型，写前自动备份当前 `credentials.json`

## 依赖

结构、样式、图标来自 Floria 内置的 `@floria/web` 包（`/webui/floria-web.css`、`/webui/floria-web.iife.js`），需要随附 mgr 组件族的 Floria 版本；除此之外单文件、零构建、无外部依赖。

## 数据接口

卡片运行在 Floria 预览宿主内时，通过同源网关接口取数：

| 接口 | 用途 |
|---|---|
| `GET /gateway/file?label=&path=` | 读取 `.claude/credentials.json`（`ETag` = 毫秒 mtime，写回冲突基线） |
| `POST /gateway/file/write` | 写回凭据池（带 `baseMtime` 冲突校验） |
| `GET /gateway/models/catalog?provider=` | 供应商的实时模型目录（只读） |
| `POST /gateway/file/create` + `POST /gateway/file/write` | 新增供应商前备份原文件 |
| `?token=<网关 token>` | 非浏览器授权场景下的鉴权参数 |

页面 URL 上的 `token` 查询参数会被自动拼到后续请求上。

## 使用

把 `index.html` 放到工作区的 `<工作区>/.claude/preview/models/index.html`，并在同级 `preview.json` 里注册：

```json
{
  "id": "models",
  "title": "模型",
  "icon": "chip",
  "host": "view",
  "path": "models/index.html"
}
```

也可以直接用浏览器打开 `index.html` 查看静态界面——但此时没有网关，数据接口不可用。
