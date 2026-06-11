# Codex × Claude Code 連携（両方シームレスに）

このリポジトリは、Claude Code から OpenAI Codex を呼び出す方法を **2系統** 同梱しています。
どちらもプロジェクトにコミット済みなので、リポジトリを clone して Claude Code を開けば
（フォルダを trust すれば）そのまま使えます。用途で使い分けてください。

| 方式 | 主な用途 | 設定ファイル |
|------|----------|--------------|
| **A. 公式 Codex Plugin** | レビュー駆動の定型ワークフロー（通常/敵対レビュー・タスク委任） | `.claude/settings.json` |
| **B. Codex MCP Server** | 汎用ツールとして Codex を軽く呼ぶ | `.mcp.json` |

両方を同時に入れても衝突しません。Plugin のスラッシュコマンドと、MCP のツールは別レイヤーです。

---

## 前提（共通）

- **Codex CLI がローカルにインストール済み**で `PATH` から `codex` が叩けること
- **ChatGPT サブスク（無料枠可）または OpenAI API キー**で Codex に認証済みであること
  - 未認証なら Claude Code 内で `!codex login`、または `codex login` を実行
- **Node.js 18.18 以降**（公式 Plugin が要求）

> MCP サーバーはログインシェルと異なる環境で起動されることがあり、`PATH` が通らず
> `codex` が見つからないことがあります。その場合は `.mcp.json` の `command` を
> `codex` のフルパス（例: `/usr/local/bin/codex`）に書き換えてください。

---

## A. 公式 Codex Plugin（`openai/codex-plugin-cc`）

`.claude/settings.json` で marketplace 登録と plugin 有効化を**事前設定済み**です。
初回はフォルダを trust するとセットアップを促されます。手動で行う場合：

```text
/plugin marketplace add openai/codex-plugin-cc
/plugin install codex@openai-codex
/reload-plugins
/codex:setup
```

### 提供されるスラッシュコマンド

| コマンド | 用途 |
|----------|------|
| `/codex:review` | 通常の read-only Codex レビュー |
| `/codex:adversarial-review` | ステアリング可能な「敵対的レビュー」モード |
| `/codex:rescue` | タスクをサブエージェントとして Codex に委任 |
| `/codex:status` | バックグラウンドジョブの進捗確認 |
| `/codex:result` | 完了ジョブの出力表示 |
| `/codex:cancel` | 実行中ジョブの停止 |

> Plugin は MCP ではなく、ローカルの **Codex CLI / Codex app server** を直接叩く作りです。
> そのため Codex 側の認証・設定をそのまま流用できます。

---

## B. Codex MCP Server

`.mcp.json` に project スコープで登録済みです。`codex mcp-server` が Codex 自身を
stdio 経由の MCP サーバーとして起動し、そのツールを Claude Code から利用できます。

```json
{
  "mcpServers": {
    "codex": {
      "type": "stdio",
      "command": "codex",
      "args": ["mcp-server"]
    }
  }
}
```

初回は Claude Code がプロジェクトの MCP サーバーを信頼するか確認します。承認後、
`/mcp` で `codex` サーバーの状態とツール一覧を確認できます。

> 注: `codex mcp-server` は「Codex を MCP サーバーにする」サブコマンドです。
> 似た `codex mcp`（add/enable/disable 等）は逆向きで、Codex が**他の** MCP サーバーを
> 消費する側の管理用なので混同しないでください。

---

## どちらを使うべきか

- **クロスプロバイダのレビューループを定着させたい** → **A. 公式 Plugin**
  （特に `/codex:adversarial-review` は MCP 生ツールだと自前のプロンプト設計が必要な部分）
- **軽さ・構成の透明性・汎用呼び出し重視** → **B. MCP Server**
  （プロセス1個、Codex 純正サブコマンドなのでラッパー陳腐化リスクもない）

迷ったら両方入れておき、レビューは A、雑に呼ぶときは B、という併用が一番ラクです。
