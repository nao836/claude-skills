# claude-skills

Claude Code 用の自作スキル集（`~/.claude/skills`）。

| スキル | 内容 |
|---|---|
| [commit-msg](commit-msg/SKILL.md) | ステージ済みの差分からコミットメッセージを1つ提案する（コミットはしない） |
| [show-me](show-me/SKILL.md) | 直前の説明をツリー・表・図などのコンパクトな形で見せ直す |
| [uiux-review](uiux-review/SKILL.md) | 画面のコードを使いやすさ・アクセシビリティの観点でレビューする |

## 使い方

```bash
git clone https://github.com/nao836/claude-skills.git ~/.claude/skills
```

Claude Code から `/commit-msg` のように呼び出すか、説明文に合う依頼をすると自動で使われる。
