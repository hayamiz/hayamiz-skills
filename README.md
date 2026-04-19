# hayamiz-agentkit

[早水](https://github.com/hayamiz) がコーディングエージェント (主に
[Claude Code](https://claude.com/claude-code)) 向けに使っているスキル・プラグイン集です。

配布経路は 2 系統:

- **プラグイン** (`plugins/`) — Claude Code plugin marketplace 経由。`/ticket-fix`
  のようにプラグイン名をプレフィックスに付けたコマンドとして使えます。
- **スキル** (`skills/`) — [APM](https://github.com/apm-pkg/apm) 経由。
  `.claude/skills/` に直接展開されて素の名前で動きます。

## 構成

```
skills/     APM 配布のスキル (1 ディレクトリ 1 スキル、各 SKILL.md を持つ)
  commit-all/      worktree のすべての差分をセマンティックにまとめて commit
  commit-session/  現セッションで変更したファイルだけを commit
plugins/    Claude Code plugin marketplace から配る (1 ディレクトリ 1 プラグイン)
  gardener/  リポジトリ健全性の監査 — docs sync / best-practices チェック等
  ticket/    ファイルベースのチケット運用 (init / create / check / triage / fix)
.claude-plugin/
  marketplace.json   plugins/ を Claude Code plugin marketplace として公開する定義
```

各プラグイン・スキルの詳細は、それぞれの `SKILL.md` / `plugin.json` を参照してください。

## インストール

### プラグイン (Claude Code)

Claude Code の plugin marketplace コマンドで追加します。

```text
# marketplace を登録 (初回のみ)
/plugin marketplace add hayamiz/hayamiz-agentkit

# プラグインを入れる
/plugin install ticket@hayamiz-agentkit
/plugin install gardener@hayamiz-agentkit
```

インストール後は `/ticket-init`, `/ticket-create`, `/ticket-check`,
`/ticket-triage`, `/ticket-fix`, `/gardener` といったコマンドが使えます。

### スキル (APM)

[`apm`](https://github.com/apm-pkg/apm) CLI が入っている前提です。

```sh
apm install hayamiz/hayamiz-agentkit/skills/commit-all
apm install hayamiz/hayamiz-agentkit/skills/commit-session
```

ユーザーグローバル (`~/.apm/`) に入れるときは `-g` を付けます:

```sh
apm install -g hayamiz/hayamiz-agentkit/skills/commit-session
```

このリポジトリ自身の `apm.yml` に書いてある依存 (`skill-creator` + 上の 2 スキル)
をまとめて入れ直すには:

```sh
apm install
```

## 開発時のメモ

- `.claude/skills/` は `apm install` が生成するため gitignore 済みです。ソースを編集するときは
  `skills/<name>/` または `plugins/<name>/skills/<name>/` を触ってください。
- プラグインを追加したら `.claude-plugin/marketplace.json` の `plugins` エントリにも
  足してください。スキルを追加したら `apm.yml` の `dependencies.apm` に足して
  `apm install` でロックファイルを更新します。
- 詳しい規約は [`CLAUDE.md`](CLAUDE.md)、[`skills/CLAUDE.md`](skills/CLAUDE.md)、
  [`plugins/CLAUDE.md`](plugins/CLAUDE.md) を参照してください。
