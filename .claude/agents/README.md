# .claude/agents/ — サブエージェント

各 `*.md` が 1 つのサブエージェント定義です。Claude が必要に応じて起動したり、
明示的に呼び出したりできます。コミットされるのでクラウド/Slack でも有効です。

## フォーマット
```markdown
---
name: agent-name                 # 必須・一意
description: いつ使うか（Claude が起動判断に使う）  # 必須
tools: Read, Grep, Glob          # 任意。省略すると全ツール継承
model: sonnet                    # 任意（sonnet / opus / haiku など）
---
ここにそのエージェントのシステムプロンプト（役割・手順・出力形式）。
```

## 追加方法
1. `my-agent.md` を作成
2. frontmatter（特に `description` を具体的に）＋本文を記述
3. コミット → Claude が自動選択、または明示的に委任できる
