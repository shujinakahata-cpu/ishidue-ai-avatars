# CLAUDE.md

ISHIDUE 役員 AI アバターのリポジトリ。画像そのものと、同じ画風で作り続けるための
仕様・プロンプトを置いている。コードはプロンプト生成スクリプト 1 本だけ。

## 大前提

**画風は羊毛フェルト人形風（needle-felted wool doll）に統一する。**

`ceo.jpg` と `secretary.jpg` の 2 枚だけが正しい画風で、これがシリーズの基準。
残る 7 名は旧・実写風なので、最終的に全て差し替える。

仕様文（`docs/style-guide.md`）と実物 2 枚が食い違ったときは、**実物を優先**して
仕様文を直す。仕様は実物から逆算したものであって、その逆ではない。

## 作業の入り口

| やること | 見る場所 |
|---|---|
| 画風の仕様を知る | `docs/style-guide.md` |
| アバターを 1 体作る | `prompts/out/<role>.txt` を画像生成ツールに貼る |
| 手順とチェックリスト | `prompts/README.md` |
| 役職・状態の一覧 | `avatars.json` |

## プロンプトの構造

プロンプトは合成物であり、`prompts/out/` は**生成物**。ここを直接編集しない。

```
prompts/_base.md          共通スタイルブロック（全員に効く画風・背景・小物・禁止事項）
prompts/subjects/<role>.md 被写体記述（その人物の髪型・表情・服装だけ）
        ↓ scripts/build_prompts.py
prompts/out/<role>.txt    コピペ用プロンプト
```

編集したら必ず再生成する。

```bash
python3 scripts/build_prompts.py           # 再生成
python3 scripts/build_prompts.py --check   # ソースとずれていないか確認（ずれていれば exit 1）
```

`_base.md` の `## STYLE` / `## NEGATIVE`、`subjects/*.md` の `## SUBJECT` という見出しは
スクリプトが正規表現で拾っているので**変更しない**。中身の文章は自由に変えてよい。

依存は Python 3 標準ライブラリのみ。追加パッケージを入れない。

## 命名規則

- 画像は `<role>.jpg`（`role` は小文字の役職略称。`ceo` `cfo` `chro` `clo` `cmo` `coo` `cso` `cto` `secretary`）
- `role` は `avatars.json` / `prompts/subjects/` / `prompts/out/` で一貫した ID として使う

## アバターを追加するとき

1. `avatars.json` の `avatars` に 1 件追加（`role` が ID になる）
2. `prompts/subjects/<role>.md` を作り `## SUBJECT` セクションを書く
3. `python3 scripts/build_prompts.py`

## アバターを差し替えたとき

`avatars.json` の該当エントリの `style` を `felt`、`status` を `current` に更新し、
`width` / `height` を実測値に合わせる。README のステータス表も同時に直す。

## 触るときの注意

- **画像ファイルを勝手に改名・移動しない。** 9 枚すべて拡張子が `.jpg` で中身は PNG (RGBA)
  という不一致があるが、外部から `raw.githubusercontent.com` 経由で参照されている
  可能性があるため意図的に残している。改名は参照元の確認が済んでから。
- **既存の実写画像には写り込みがある。** `chro` 右下に別画像のサムネイル、`cmo` 左下に
  「ステム設定」の文字、`cso` 左下に黒い点。参照元として使うときも、この部分は無視する。
- **実在の人物に似せない。** 役職ごとの人物像に留める。
- CSO / CLO の日本語役職名は未確定（暫定値が入っている）。確定情報が出たら
  `avatars.json` と README の両方を直す。

## ドキュメントの言語

README・`docs/`・`prompts/README.md`・`avatars.json` のコメントは日本語。
画像生成に渡すプロンプト本文（`_base.md` の STYLE / NEGATIVE、`subjects/*.md` の SUBJECT）は
英語で書く。
