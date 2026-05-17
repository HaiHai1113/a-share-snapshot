# a-share-snapshot

A 股自选股每日快照(只读),给老板自定义 GPT 用。

## 数据来源

- 数据生成: `trader-copilot/src/copilot/daily_radar` (每天 9:31 触发)
- 同步上传: `trader-copilot/scripts/sync_snapshot_to_github.sh` (每天 9:35 触发)
- 字段细节: 见 `openapi.yaml`

## 目录结构

```
.
├── openapi.yaml          ← 给 ChatGPT 自定义 GPT 的 Actions 配置粘贴用
├── README.md             ← 本文件
└── snapshot/
    ├── latest.json       ← 最新一份快照(每个交易日 9:35 覆盖)
    ├── YYYY-MM-DD.json   ← 当日存档(按交易日积累)
    └── dates.json        ← 可查询日期索引
```

## 访问方式

仅限带 GitHub fine-grained PAT 的 GET 请求。无 token 一律 404。

```
GET https://raw.githubusercontent.com/HaiHai1113/a-share-snapshot/main/snapshot/latest.json
Authorization: Bearer <PAT>
```

## 安全边界

- repo 是 private,不索引,无搜索曝光
- 仅同步 `data/a_share_snapshot/` 下的 JSON,不含持仓/订单/账本/.env
- 同步脚本走老板本机 `gh auth` 凭证,不嵌任何密钥
- GPT Actions 用一个**专用** fine-grained PAT(只读这一个 repo 的 Contents),
  跟老板本人的 GitHub 主 token 完全隔离
