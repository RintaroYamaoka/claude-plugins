# claude-plugins — Rintaro Yamaoka の plugin marketplace

Claude Code プラグインの**配布インデックス専用 repo**。各プラグインの実体はそれぞれの repo にあり、
ここは `.claude-plugin/marketplace.json` で指すだけ。**載せるのは生きている plugin だけ。**退役した物は下の表に残し、index からは外す。

## 今 配っている plugin

| plugin | 何をする | repo |
|---|---|---|
| **tdd-guards** | コードを書く作業の小さな規律。skill 1 本(`tdd`:書く前に読む / Red → Green → Refactor / commit・push の前の手順)+ PreToolUse hook 3 本(commit 前の test+lint・PR の CI がある repo は CI に任せる / 古い checkout からの push の拒否 / 強制 push・`reset --hard`・protected 直 push の拒否)。並走・lane・作業指示書の運用は持たない | https://github.com/RintaroYamaoka/tdd-guards |
| **roentgen** | repo のコードを知識グラフ(関数・型・表・入口・呼び出し・読み書き)にして、リファレンス風の HTML 19 ページ・意味の diff・query・MCP・notes を出す。北極星は「コードを読まずに読んだのと近い状態」。Rust の単一バイナリ(launcher が Releases から取得、無ければ cargo でビルド) | https://github.com/RintaroYamaoka/roentgen |

## 利用

```
/plugin marketplace add RintaroYamaoka/claude-plugins
/plugin install tdd-guards@rintaro-yamaoka
/plugin install roentgen@rintaro-yamaoka
```

## 退役した plugin(index からは外してある・repo は残っている)

| plugin | 期間 | 何だったか | 退役の理由 | 後継 |
|---|---|---|---|---|
| upstream-process | 2026-07-10 〜 08-07 | 上流を手順+カタログで支援 | kanban-flow へ移行 | kanban-flow → project-bootstrap |
| kanban-flow | 2026-07 〜 08-07 | 上流(計画・設計・カード起票・レビュー・監査)をカンバンと GitHub Projects で回す | project-bootstrap の commission サブシステムに吸収 | project-bootstrap |
| project-bootstrap | 2026-05 〜 10-16 | 実装以降(詳細設計〜統合)と発注〜検収を 22 本の hook と 11 本の skill で強制 | 運用の前提(複数 Claude の並走・lane・作業指示書)を使わなくなった。「モデルの進化で gate が不要になった」は transcript で検証して否定(repo の ADR 0027 に数字) | **tdd-guards**(実際に効いていた物だけを継いだ) |

退役した plugin を入れたままの環境は、index から消えたので update も再 install もできない。外すなら `/plugin uninstall <name>@rintaro-yamaoka`。

## 経緯

marketplace はもともと project-bootstrap repo 内に同居していた(plugin が 1 本の時代に、
「配布インデックスはどこに住むか」を誰も決めないまま無音で埋まった配置)。
2 本目(upstream-process)が出た 2026-07-10 に、この repo へ切り出した。
配布(上位層)が特定製品(下位層)の中にある依存方向違反の解消。
marketplace 名 `rintaro-yamaoka` は据え置き(インストール済み参照を壊さないため)。

2026-10-16:project-bootstrap を退役し tdd-guards を後継に。同時に、退役済みだった kanban-flow・upstream-process も含めて **index から外した**(配布層には生きている物だけを置く。退役の記録はこの README が持つ)。

## plugin を追加するとき

1. plugin 側の repo を作り、`.claude-plugin/plugin.json` を置く
2. この repo の `marketplace.json` の `plugins` 配列に entry を追加(source は git URL)
3. `metadata.version` を上げて commit & push
4. `/plugin marketplace update rintaro-yamaoka`

## plugin を退役させるとき

1. plugin 側の repo に退役の理由を書く(README 冒頭+ADR)。理由は現物で検証してから書く
2. この repo の `marketplace.json` から entry を**消す**。上の「退役した plugin」の表に 1 行足す
3. `metadata.version` を上げて commit & push → `/plugin marketplace update rintaro-yamaoka`
