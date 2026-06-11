# .claude/skills/ — プロジェクト skill

各 skill は `skills/<skill-name>/SKILL.md` という形で 1 ディレクトリ＝1 skill。
`description` の内容にタスクが合致したとき Claude が自動的に読み込みます。
コミットされるのでクラウド/Slack でも有効です。

## フォーマット
```markdown
---
name: skill-name                 # 必須・一意（ディレクトリ名と揃える）
description: 何をする skill か＋いつ使うか  # 必須。発火条件になるので具体的に
allowed-tools: Read, Edit        # 任意
---
ここに手順・知識・ルールなど。
```

## 補助ファイル
`SKILL.md` と同じディレクトリにスクリプトや参考資料を置き、本文から参照できます。

## 追加方法
1. `skills/my-skill/SKILL.md` を作成
2. frontmatter ＋ 本文を記述
3. コミット → 条件に合致すると自動で読み込まれる
