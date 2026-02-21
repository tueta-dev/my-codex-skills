# my-codex-skills

Codex で使うカスタムスキル集です。設計・レビュー・現状把握のワークフローを、再現性のある成果物として出力します。

## 前提
- Codex が利用できる環境で動作します。
- スキルの詳細仕様は各 `SKILL.md` に記載しています。

## 使い方（インストール）
使いたいスキルのフォルダを、Codex のスキルディレクトリにコピーしてください。

```
~/.codex/skills/
```

コピー後、必要に応じて Codex を再起動（またはスキル再読み込み）します。

## 収録スキル
以下は「何をするか」と「何が出力されるか」を最短で把握できる要約です。

### as-is-discovery
- 役割: 既存システムの現状把握（事実/推論を分離、証拠と信頼度を明記）
- 出力: `docs/as-is/as-is-YYYY-MM-DD.md`（Markdown レポート + JSON サマリ）
- 使いどころ: 現状理解や設計の前提整理が必要なとき

### design-orchestrator
- 役割: 複数デザイン系スキルの実行計画を作るコントロールプレーン
- 出力: `docs/design/<design_id>/design-plan.md`
- 使いどころ: どの設計スキルをどの順で回すか決めたいとき

### domain-design
- 役割: ドメイン（概念）設計を整理（用語、境界づけ、集約、不変条件など）
- 出力: `docs/design/<design_id>/domain-design.md`（Markdown + JSON サマリ）
- 使いどころ: 用語整理やビジネスルールの明文化が必要なとき

### db-design
- 役割: ドメイン要件から RDB スキーマを設計（2〜3案 + 推奨案）
- 出力: `docs/design/<design_id>/db-design.md`（Markdown + JSON サマリ）
- 使いどころ: スキーマ/制約/インデックスまで含めて設計したいとき

### design-review
- 役割: 複数の設計成果物の整合性・欠落・リスクをレビュー
- 出力: `docs/design/<design_id>/design-review.md`（Markdown + JSON サマリ）
- 使いどころ: 既存の設計ドキュメント群を横断チェックしたいとき

## セキュリティ / プライバシー
- 機密情報や個人情報は成果物に含めない運用を推奨します。

## Contributing
Issue / PR どちらでも歓迎です。新スキル追加時は、用途と成果物パスが README に反映されているか確認してください。

## License
このリポジトリには LICENSE が含まれていません。公開前に方針を決めて追加することをおすすめします。
