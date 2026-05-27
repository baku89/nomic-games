# nomic-games

[nomic-bot](https://github.com/baku89/nomic-bot) が管理する、進行中・終了済みノミックゲームの記録リポジトリ。

[ノミック (Nomic)](https://ja.wikipedia.org/wiki/%E3%83%8E%E3%83%9F%E3%83%83%E3%82%AF) は Peter Suber が考案した「プレイヤーがルールを自己改変できるゲーム」。
このリポジトリの各 Markdown ファイルが 1 つのゲームインスタンスを表す。

## ディレクトリ構造

- `<game-name>.md` — 進行中ゲーム (1 ファイル = 1 ゲーム)
- `archive/<game-name>.md` — 終了/中断済みゲーム
- README.md / LICENSE — このリポ自体のメタ情報

## 1ファイルの構成

各ゲームファイルは YAML frontmatter と Markdown 本文からなる。

```yaml
---
status: active | completed | paused
started_at: "2026-05-27T10:00:00Z"
current_turn: "<Discord User ID>"
active_proposal: <提案オブジェクト or null>
pending_end: <終了確認オブジェクト or null>
---
```

Discord のチャンネル ID は Bot 側のローカルキャッシュ (`<bot>/.cache/channels.json`) に保持され、このリポには出力されない。

```markdown
# <game-name>

## 参加者
- @username (123456789012345678)
- @another-user (987654321098765432)

## ルール
- 101. すべてのプレイヤーは現行のすべてのルールに従わなければならない。本初期ルールセットは、ゲーム開始時に有効となる。
- 102. ルール改変とは、ルールを制定・廃止・修正することのいずれかをいう。
- 103. プレイヤーはゲーム開始時にメンションされた順 (参加者リスト順) に手番を行う。最後のプレイヤーの次は先頭のプレイヤーに戻る。
- ...

## 勝者 (終了済みゲームのみ)
- @winner の勝利 — <理由>
- 終了日時: 2026-05-28T12:34:56Z
- 最終ルール数: 14 条
```

参加者リストの並び順がそのまま手番順 (Rule 103)、最後尾の次は先頭に戻る。

## コミット履歴の読み方

ルール改変・参加・終了などのイベントごとに 1 コミットが積まれ、**コミットメッセージが提案文・採択結果・投票内訳を含む**。

```bash
git log --oneline           # 全ゲームの変更履歴
git log -p <name>.md        # 特定ゲームの全進化
git blame <name>.md         # ルールごとの導入提案者・改変履歴
```

Bot が動いていなくても、git log だけで「ノミックが何を考えてどう変化したか」が追える設計。

## ライセンス

[MIT](./LICENSE)
