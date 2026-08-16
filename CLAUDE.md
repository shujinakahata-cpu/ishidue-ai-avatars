# ishidue-ai-avatars — 経営チームエージェントのアバター画像（Claude 入口）

株式会社いしづえのAI経営チームエージェント（ceo/coo/cto/cmo/cfo/clo/cso/chro/secretary）のアバター画像を置くリポジトリ。
会社の基準・地図は親リポジトリ `ishidue-main-Repository`（`docs/00_インデックス.md` が会社の地図）。

## このリポジトリでのルール

- **変更はブランチを切ってPR経由で入れる**（`main` へ直接pushしない）。マージは人間が判断する。
- セッション中断・終了時は必ず作業ブランチへコミット＆プッシュしてから離れる。
- **🚫 PRの定期チェックイン（send_later等の時限自己確認）を自分で仕込まない。同時セッションは目安5個まで。1タスク1セッション。**（正本＝親 `ishidue-main-Repository` の CLAUDE.md と `.claude/rules/lessons.md`。見張りはwebhookのイベント駆動のみ・時限の再確認は予約しない）
