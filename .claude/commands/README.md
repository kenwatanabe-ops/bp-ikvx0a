# .claude/commands/ — カスタム slash コマンド

このディレクトリの各 `*.md` が `/<ファイル名>` の slash コマンドになります。
（例: `proofread.md` → `/proofread`）。コミットされるのでクラウド/Slack でも使えます。

## フォーマット
```markdown
---
description: /help に出る短い説明
argument-hint: [引数の見せ方]      # 任意
allowed-tools: Read, Edit, Bash    # 任意。省略時は通常の権限に従う
model: claude-sonnet-4-6           # 任意
---
ここにプロンプト本文。以下が使えます:
- $ARGUMENTS … 引数全体
- $1, $2 …    … 位置引数
- @path/to/file … ファイル内容を差し込み
- !`command`    … シェル実行結果を差し込み（allowed-tools 必要）
```

## 追加方法
1. このディレクトリに `my-command.md` を作る
2. 上記フォーマットで中身を書く
3. コミット → `/my-command` で使える
