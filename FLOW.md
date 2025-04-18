# roo code ブーメランモードフロー

このドキュメントは、roo code のモード切り替えによる開発タスクの自動化ワークフローを定義します。

## ワークフロー概要

roo code の開発タスク自動化は、複数の AI モード間の連携により実現されます。各モードは特定の役割を持ち、単一の GitHub イシュー上でコメントを通じて進捗を管理します。

### 使用する AI モード

- **AI-P**: 進捗管理エージェント（Progress Manager）
- **AI-A**: 分析・技術選定・環境準備エージェント（Analyzer）
- **AI-T**: テスト実装エージェント（Test Implementer）
- **AI-D**: 機能実装エージェント（Development Implementer）
- **AI-Q**: 品質チェック兼コードレビューエージェント（Quality Checker）

## 詳細ワークフロー

### 0. 準備フェーズ

1. ユーザーが仕様書の作成を Claude desktop で行う
   1. 主にブランジング結果の理解が現時点(2025/04/08)で一番優秀だったため
2. 開発プロジェクトの初期作成はユーザーで行う
   1. AI が実施するほど複雑な処理ではない
   2. ユーザーが実施した方が言語やライブラリなどのバージョンアップに対応しやすい
   3. AI による不備が起きた場合の責任をユーザーが払いきれない将来の可能性がある
3. ユーザーが単一のイシューを作成する（このイシューのみを使用）

### 1. タスク受付フェーズ

1. ユーザーが **AI-P**（進捗管理エージェント）にイシューの URL を正確に伝える
2. **AI-P** が指定されたイシュー URL の内容を読み取り、要求・仕様を把握する
3. **AI-P** はイシューに最初のコメントを追加する
   - コメント内容：「**[AI-P] タスク受付完了**」を明記し、把握した要求・仕様のサマリーを記載
   - タスクの基本情報と処理プロセスの初期化内容を記載

### 2. 分析・計画および環境準備フェーズ

1. **AI-P** が **AI-A**（分析・技術選定・環境準備エージェント）にモード切り替えを指示
2. **AI-A** がイシュー URL にアクセスし、タスク指示内容を詳細に分析する
3. **AI-A** が最適な技術スタックを選定する
   - **重要**: 言語・パッケージ・フレームワークの選定は必ず公式ドキュメントかつ最新のものを参照する
   - 一般人のブログや公式以外のウェブページの参照は禁止
   - 参照した公式ドキュメントの URL を明記すること
   - **特殊ケース**: 不明点がある場合は、イシューにその不明点と必要な情報の根拠を明記し、ユーザーに確認を求める
   - ユーザーからの回答があった場合は、それを考慣して分析・選定を続ける
4. **AI-A** が適切なプロジェクト構造を設計・作成する
5. **AI-A** がイシューに新しいコメントを追加する
   - コメント内容：「**[AI-A] 分析・技術選定・環境準備完了**」を明記
   - 分析結果、選定した技術スタック（バージョン情報含む）、作成したプロジェクト構造の詳細を記載
   - 参照した公式資料の URL リストを必ず記載
6. **AI-A** から **AI-P** へモード切り替え

### 3. イシュー更新フェーズ

1. **AI-P** がイシュー URL にアクセスし、**AI-A**のコメントを確認する
2. **AI-P** がイシューにコメントを追加する
   - コメント内容：「**[AI-P] 環境準備確認完了**」を明記
   - イシューのラベルを「環境準備完了」に更新

### 4. テスト実装フェーズ

1. **AI-P** が **AI-T**（テスト実装）へモード切り替えを指示
2. **AI-T** がイシュー URL にアクセスし、これまでのコメント内容とプロジェクト構造を確認する
3. **AI-T** がテストコードを実装する
   - テスト実装の際も公式ドキュメントのみを参照（一般ブログ等は禁止）
4. **AI-T** がイシューにコメントを追加する
   - コメント内容：「**[AI-T] テスト実装完了**」を明記
   - 実装したテストコードの詳細、テスト戦略、カバレッジ計画を記載
   - 実装時に参照した公式資料の URL リストを必ず記載
5. **AI-T** から **AI-P** へモード切り替え

### 5. 機能実装フェーズ

1. **AI-P** がイシュー URL にアクセスし、**AI-T**のコメントを確認する
2. **AI-P** がイシューにコメントを追加する
   - コメント内容：「**[AI-P] テスト実装確認完了**」を明記
   - イシューのラベルを「テスト実装完了」に更新
3. **AI-P** が **AI-D**（機能実装）へモード切り替えを指示
4. **AI-D** がイシュー URL にアクセスし、これまでのコメント内容、テストコード、プロジェクト構造を確認する
5. **AI-D** が機能コードを実装する
   - 機能実装の際も公式ドキュメントのみを参照（一般ブログ等は禁止）
6. **AI-D** がイシューにコメントを追加する
   - コメント内容：「**[AI-D] 機能実装完了**」を明記
   - 実装した機能コードの詳細、アーキテクチャ、主要機能の説明を記載
   - 実装時に参照した公式資料の URL リストを必ず記載
7. **AI-D** から **AI-P** へモード切り替え

### 6. 品質保証・レビューフェーズ

1. **AI-P** がイシュー URL にアクセスし、**AI-D**のコメントを確認する
2. **AI-P** がイシューにコメントを追加する
   - コメント内容：「**[AI-P] 機能実装確認完了**」を明記
   - イシューのラベルを「機能実装完了」に更新
   - 改善要求回数カウンターを 0 と記載
3. **AI-P** が **AI-Q**（品質チェック兼コードレビュー）へモード切り替えを指示
4. **AI-Q** がイシュー URL にアクセスし、これまでのコメント内容、テストコード、機能コードを検証する
   - 品質検証の際も公式ドキュメントのみを参照（一般ブログ等は禁止）

#### 6-A. 改善が必要な場合

1. **AI-Q** がイシューにコメントを追加する
   - コメント内容：「**[AI-Q] 改善要求**」を明記
   - 改善が必要な箇所、理由、改善方法の提案を具体的に記載
   - イシューのラベルを「改善要求」に更新
   - 参照した公式資料の URL リストを必ず記載
2. **AI-Q** から **AI-P** へモード切り替え
3. **AI-P** がイシュー URL にアクセスし、**AI-Q**のコメントを確認する
4. **AI-P** がイシューにコメントを追加する
   - コメント内容：「**[AI-P] 改善要求確認**」を明記
   - 改善要求回数カウンターをインクリメントした値を記載
   - 改善要求回数が 3 回未満かどうかの判定結果を記載
5. **AI-P** が改善担当エージェント（**AI-T**または**AI-D**）へモード切り替えを指示
6. 担当エージェントがイシュー URL にアクセスし、これまでのコメント内容を確認する
7. 担当エージェントが改善を実施する
8. 担当エージェントがイシューにコメントを追加する
   - コメント内容：「**[AI-T/AI-D] 改善実装完了**」を明記
   - 改善した内容の詳細、改善前後の比較、改善の効果を記載
   - 改善時に参照した公式資料の URL リストを必ず記載
9. 担当エージェントから **AI-P** へモード切り替え
10. **AI-P** がイシュー URL にアクセスし、担当エージェントのコメントを確認する
11. **AI-P** がイシューにコメントを追加する
    - コメント内容：「**[AI-P] 改善実装確認完了**」を明記
    - イシューのラベルを「改善実装完了」に更新
12. **AI-P** が **AI-Q** へモード切り替えを指示

#### 6-B. 品質基準を満たした場合（または改善要求回数が 3 回に達した場合）

1. **AI-Q** がイシューにコメントを追加する
   - コメント内容：「**[AI-Q] 品質承認**」を明記
   - 検証結果、コードの品質評価、強みと弱みの分析を記載
   - イシューのラベルを「品質チェック・レビュー完了」に更新
   - (改善要求回数が 3 回に達した場合は、その旨と残存する課題を明記)
2. **AI-Q** が Pull Request を作成する
3. **AI-Q** がイシューに Pull Request の URL をコメントする
4. **AI-Q** から **AI-P** へモード切り替え

### 7. 完了フェーズ

1. **AI-P** がイシュー URL にアクセスし、**AI-Q**のコメントと Pull Request の URL を確認する
2. **AI-P** が Pull Request をマージする
3. **AI-P** がイシューにコメントを追加する
   - コメント内容：「**[AI-P] タスク完了**」を明記
   - マージした Pull Request の URL、完了した機能の概要、今後の課題を記載
4. **AI-P** がイシューのステータスを「クローズ」に更新する

## 特殊ケースのフロー管理

### 1. 各モードの不明点確認フロー

各 AI モード（AI-A、AI-T、AI-D、AI-Q）において、不明点や確認が必要な事項がある場合は、以下のフローで対応します。

1. 各 AI モードが不明点を発見した場合
2. イシューに「[モード名] 情報確認依頼」のコメントを投稿
   - 不明点や必要な情報、確認理由を具体的に記載
   - 現時点での考察や代替案を記載
   - 調査した公式資料の URL を明記
3. ユーザーがイシューに回答コメントを投稿
4. 各 AI モードがユーザーからの回答を受け取り、タスクを続行
5. 各 AI モードのレポートコメント内に、ユーザーから回答を受けた不明点と対応を記載

## イシューコメントフォーマット

各 AI モードがイシューに追加するコメントのフォーマットを以下のように定めます。すべてのコメントはマークダウン形式で記述し、一貫性のあるスタイルと構造を維持します。

### AI-P: タスク受付時

```markdown
## [AI-P] タスク受付完了

- 難易度: 【低/中/高】

### 要求・仕様の要約

- 【要求 1】
- 【要求 2】
- 【要求 3】
  ...

### 初期プロセス情報

- 処理モード: 【初期モード】
- 次の担当: AI-A
- アクション: 分析・技術選定・環境準備の実行
```

### AI-P: 各フェーズ確認時

```markdown
## [AI-P] 【フェーズ名】確認完了

### 検証結果

- ステータス: 【OK/NG】
- 検証日時: YYYY-MM-DD HH:MM

### アクション

- 次の担当: 【次の AI モード】
- タスク: 【次のタスク内容】

### 更新情報

- ラベル更新: 【新ラベル名】
- 改善要求カウント: 【数値】（該当する場合のみ）
```

### AI-A: 情報確認依頼時

```markdown
## [AI-A] 情報確認依頼

### 確認が必要な事項

- 不明点: 【不明点の詳細】
- 必要な情報: 【必要な情報の内容】

### 確認理由

- 【確認が必要な理由】
- 【現状で判断できない理由】

### 現状の考察

- 【現時点での判断や考察】
- 【代替案の候補や検討中の選択肢】

### 情報源

- 【調査した公式資料や参照元】: 【URL】（公式）
```

### AI-A: 分析・技術選定・環境準備時

```markdown
## [AI-A] 分析・技術選定・環境準備完了

### 要件分析

#### 機能要件

- 【機能要件 1】
- 【機能要件 2】
  ...

#### 非機能要件

- 【非機能要件 1】
- 【非機能要件 2】
  ...

#### 制約条件

- 【制約 1】
- 【制約 2】
  ...

### 技術スタック選定

#### 言語・フレームワーク

- 言語: 【言語名】 【バージョン】
  - 選定理由: 【理由】
  - 公式ドキュメント: 【URL】
- フレームワーク: 【フレームワーク名】 【バージョン】
  - 選定理由: 【理由】
  - 公式ドキュメント: 【URL】

#### ライブラリ・パッケージ

- 【ライブラリ名】 【バージョン】
  - 用途: 【用途】
  - 選定理由: 【理由】
  - 公式ドキュメント: 【URL】
    ...

#### アーキテクチャパターン

- パターン: 【パターン名】
- 選定理由: 【理由】

### プロジェクト構造
```

プロジェクト/
├── src/
│ ├── 【ディレクトリ 1】/
│ ├── 【ディレクトリ 2】/
│ └── ...
├── test/
│ ├── 【テストディレクトリ 1】/
│ └── ...
├── 【設定ファイル 1】
└── 【設定ファイル 2】

```

### 環境構築手順
1. 【手順1】
2. 【手順2】
...

### 参照資料
- 【資料名1】: 【URL】（公式）
- 【資料名2】: 【URL】（公式）
...
```

### AI-T: 情報確認依頼時

```markdown
## [AI-T] 情報確認依頼

### 確認が必要な事項

- 不明点: 【不明点の詳細】
- 必要な情報: 【必要な情報の内容】

### 確認理由

- 【確認が必要な理由】
- 【現状で判断できない理由】

### 現状の考察

- 【テスト戦略や実装に関する現状の考え】
- 【代替案の候補や検討中の選択肢】

### 情報源

- 【調査した公式資料や参照元】: 【URL】（公式）
```

### AI-T: テスト実装時

````markdown
## [AI-T] テスト実装完了

### テスト戦略

- テスト種別: 【単体/統合/E2E/etc】
- テスト方針: 【方針】
- カバレッジ目標: 【目標値】%
  - 検証内容: 【内容】
  - ファイルパス: 【パス】
- 【テスト名 2】
  ...

#### 【テストカテゴリ 2】

...

### テスト実行方法

```bash
# テストコマンド
【コマンド】
```
````

```

### 実装上の注意点

- 【注意点 1】
- 【注意点 2】
  ...

### 参照資料

- 【資料名 1】: 【URL】（公式）
- 【資料名 2】: 【URL】（公式）
  ...
```

### AI-D: 機能実装時

````markdown
## [AI-D] 機能実装完了

### 実装概要

- 実装機能: 【機能名】
- 対応要件: 【要件 IDs】

### アーキテクチャ実装詳細

#### コアモジュール

- 【モジュール名 1】
  - 役割: 【役割】
  - 実装: 【実装内容】
  - ファイルパス: 【パス】
- 【モジュール名 2】
  ...

#### データモデル

- 【モデル名 1】
  - 項目: 【項目リスト】
  - 関連: 【関連情報】
- 【モデル名 2】
  ...

### 実装のポイント

- 【ポイント 1】
- 【ポイント 2】
  ...

### 制約・制限事項

- 【制約 1】
- 【制約 2】
  ...

### 動作確認方法

```bash
# 実行コマンド
【コマンド】
```
````

```

### 参照資料

- 【資料名 1】: 【URL】（公式）
- 【資料名 2】: 【URL】（公式）
  ...
```

### AI-Q: 品質チェック・コードレビュー時（改善要求）

```markdown
## [AI-Q] 改善要求

### レビュー概要

- レビュー日時: YYYY-MM-DD HH:MM
- 総合評価: 【評価】/5

### 改善要求事項

#### 1. 【改善項目 1】

- 対象ファイル: 【ファイルパス】
- 問題点: 【問題点】
- 改善提案: 【提案内容】
- 重要度: 【高/中/低】
- 担当: 【AI-T/AI-D】

#### 2. 【改善項目 2】

...

### テスト評価

- カバレッジ: 【値】%
- 品質評価: 【評価】
- 指摘事項: 【指摘内容】

### コード品質評価

- 可読性: 【評価】/5
- 保守性: 【評価】/5
- パフォーマンス: 【評価】/5
- セキュリティ: 【評価】/5

### 参照資料

- 【資料名 1】: 【URL】（公式）
- 【資料名 2】: 【URL】（公式）
  ...
```

### AI-Q: 品質チェック・コードレビュー時（品質承認）

```markdown
## [AI-Q] 品質承認

### レビュー概要

- レビュー日時: YYYY-MM-DD HH:MM
- 総合評価: 【評価】/5

### 品質評価

#### テスト品質

- カバレッジ: 【値】%
- テスト品質評価: 【評価】/5
- 強み: 【強み】
- 弱み: 【弱み】（該当する場合）

#### コード品質

- 可読性: 【評価】/5
- 保守性: 【評価】/5
- パフォーマンス: 【評価】/5
- セキュリティ: 【評価】/5

### 優れている点

- 【点 1】
- 【点 2】
  ...

### 今後の改善余地

- 【改善点 1】
- 【改善点 2】
  ...

### Pull Request 情報

- PR URL: 【URL】
- ブランチ: 【ブランチ名】

### 参照資料

- 【資料名 1】: 【URL】（公式）
- 【資料名 2】: 【URL】（公式）
  ...
```

### AI-T/AI-D: 改善実装時

````markdown
## [AI-T/AI-D] 改善実装完了

### 対応した改善要求

- 改善要求 ID: 【改善要求項目番号】
- 対象ファイル: 【ファイルパス】

### 改善内容

- 変更前: 【変更前の状態】
- 変更後: 【変更後の状態】
- 改善効果: 【効果】

### 対応内容詳細

```diff
【差分内容】
```
````

### テスト結果（該当する場合）

- テスト実行結果: 【成功/失敗】
- カバレッジ変化: 【変化】

### 参照資料

- 【資料名 1】: 【URL】（公式）
- 【資料名 2】: 【URL】（公式）
  ...

````

### AI-P: タスク完了時

```markdown
## [AI-P] タスク完了

### 完了情報
- 完了日時: YYYY-MM-DD HH:MM
- 総所要時間: 【時間】h

### 成果物
- Pull Request: 【URL】
- マージ先ブランチ: 【ブランチ名】

### 実装機能サマリー
- 【機能1】: 【概要】
- 【機能2】: 【概要】
...

### 技術スタック最終確認
- 言語: 【言語】 【バージョン】
- フレームワーク: 【フレームワーク】 【バージョン】
- 主要ライブラリ: 【ライブラリリスト】

### 今後の課題・展望
- 【課題1】
- 【課題2】
...

### プロジェクト統計
- コード行数: 【行数】
- テスト数: 【テスト数】
- カバレッジ: 【値】%
````

## コミットメッセージの形式

各タスク完了時に、以下の形式でコミットメッセージを作成してください：

```
feat|fix|docs|style|refactor|test|chore: [AI-P] タスク完了の概要

### 変更内容の詳細

#### ファイル単位の変更概要
- [ファイルパス1]: [変更内容の概要]
- [ファイルパス2]: [変更内容の概要]
...

### 変更の趣旨
[このタスクでの変更の目的や意図を簡潔に説明]

### 技術的な詳細
[必要に応じて技術的な詳細を記載]
```

### コミット種類の説明

- feat: 新機能の追加
- fix: バグ修正
- docs: ドキュメントの変更
- style: コードの意味に影響しない変更（空白、フォーマットなど）
- refactor: リファクタリング
- test: テストの追加・修正
- chore: ビルドプロセスや補助ツールの変更
