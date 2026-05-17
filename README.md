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

## 访问方式(GitHub Pages, 公开 + 无鉴权)

repo 是 public, 通过 GitHub Pages 静态托管, 直接 GET:

```
GET https://haihai1113.github.io/a-share-snapshot/snapshot/latest.json
GET https://haihai1113.github.io/a-share-snapshot/snapshot/dates.json
GET https://haihai1113.github.io/a-share-snapshot/snapshot/YYYY-MM-DD.json
```

不需要 Authorization header, 不需要 PAT, 任何客户端都能拉。

## 安全边界

- repo 是 **public**, 自选股 24 只代码 + 行情/指标可被任何人拉取 (老板已同意暴露范围)
- 仅同步 `data/a_share_snapshot/` 下的 JSON,**不含**持仓 / 订单 / 账本 / .env
- 持仓 / 真实交易记录由 trader-copilot 主仓单独管控, 跟本 repo **物理隔离**
- 同步脚本走老板本机 `gh auth` 凭证(只用来 push 本 repo), 不嵌任何密钥
