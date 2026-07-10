# claude-plugins — Rintaro Yamaoka の plugin marketplace

Claude Code プラグインの**配布インデックス専用 repo**。各プラグインの実体はそれぞれの repo にあり、
ここは `.claude-plugin/marketplace.json` で指すだけ。

| plugin | 管轄 | repo |
|---|---|---|
| project-bootstrap | 実装以降(詳細設計〜統合)を hook で強制 | https://github.com/RintaroYamaoka/project-bootstrap |
| upstream-process | 上流(要件定義・基本設計・現行仕様復元)を手順+カタログで支援 | https://github.com/RintaroYamaoka/upstream-process |

## 利用

```
/plugin marketplace add RintaroYamaoka/claude-plugins
/plugin install project-bootstrap@rintaro-yamaoka
/plugin install upstream-process@rintaro-yamaoka
```

## 経緯

marketplace はもともと project-bootstrap repo 内に同居していた(plugin が 1 本の時代に、
「配布インデックスはどこに住むか」を誰も決めないまま無音で埋まった配置)。
2 本目(upstream-process)が出た 2026-07-10 に、この repo へ切り出した。
配布(上位層)が特定製品(下位層)の中にある依存方向違反の解消。
marketplace 名 `rintaro-yamaoka` は据え置き(インストール済み参照を壊さないため)。

## plugin を追加するとき

1. plugin 側の repo を作り、`.claude-plugin/plugin.json` を置く
2. この repo の `marketplace.json` の `plugins` 配列に entry を追加(source は git URL)
3. `metadata.version` を上げて commit & push
4. `/plugin marketplace update rintaro-yamaoka`
